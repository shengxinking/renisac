# Install NFDump #

NFDump tools collect and process NetFlow from the command line. The filtering syntax for NFDump syntax is similar to tcpdump. NFDump allows one to display netflow data and create top N statistics of flows according to IP address, ports, etc. In this guide, an Ubuntu Server operating system is being used. **Not all of the steps in this guide are necessary if installing [NFSen](https://code.google.com/p/renisac/wiki/NFSen_Install) with NFDump. We make note of where the configuration steps end for a [NFSen install](https://code.google.com/p/renisac/wiki/NFSen_Install). This guide also now includes optional instructions for sFlow.**

**Step 1:** Download the source code for NFDump

Obtain the NFDump source from: http://sourceforge.net/projects/nfdump/files/latest/download

> Or:

> ```
$ wget http://sourceforge.net/projects/nfdump/files/stable/nfdump-1.6.10/nfdump-1.6.10.tar.gz/download ```

> ```
$ mv download nfdump-1.6.10.tar.gz```

**Step 2:**
Uncompress the nfdump-1.6.10.tar.gz file:

> ```
$ sudo tar -zxvf nfdump-1.6.10.tar.gz```

> You can change the location of the uncompressed files to a location of your liking or leave it in the Downloads directory.

**Step 3:**
Install prerequisite packages:

> ```
$ sudo apt-get install librrd-dev librrd4 flex gcc make byacc```

**Step 4:**
Now get into the nfdump-1.6.10 directory, this is where your config file is

> ```
$ cd nfdump-1.6.10```

Run configure with the specified options below. This command runs the script which checks for required dependencies before compile. **sFlow users must add the corresponding option below.**

> ```
$ sudo ./configure -–with-rrdpath -–enable-nfprofile```
#### **Options:** ####

  * --prefix=/usr/local/nfdump defines a directory path to install nfdump. The default is /usr/local **(optional)**
  * --with-rrdpath links to the directory where rrd.h resides. Set --with-rrdpath=<location of rrdtool> if configure cannot locate librrd
  * --with-ftpath=<path to flow tool source> defines the flow-tool source library for nfdump **(optional)**
  * --enable-nfprofile is mandatory if installing NFSen
  * --enable-nftrack is mandatory if installing the Port Tracker plugin for NFSen **(optional)**
  * --enable-sflow to install sfcapd. The sFlow capture daemon **(optional)**


**Step 5:** After installing all the required packages run:

> ```
$ sudo make```

**Step 6:** After make runs successfully run:

> ```
$ sudo make install```

**Step 7:** Run NFDump from the command line to verify a successful install:

> ```
$ nfdump```

#### If installing NFSen, the remaining steps are optional. The NetFlow Indexer is also optional. ####

**Step 8:** Startup nfcapd to start capturing NetFlow

> ```
$ sudo nfcapd -w -p 2055 -4 -D -T all -l /var/log/netflowdata/```

**sFlow users can run nfcapd alongside sfcapd if necessary.**

> ```
$ sudo sfcapd -w -p 2055 -4 -D -T all -l /var/log/sflowdata/```

The -w switch says to rotate flow dump files according to a time interval (by default this is every 5 minutes). Combining -w with -t can specify different rotation time intervals. The -p switch opens up a listening port on port 2055 for the nfcapd process. -4 tells nfcapd to listen for IPv4 traffic only. -D tells nfcapd to fork to the background and -T says to consider all NetFlow extensions including NetFlow v5, v7 and v9. Finally -l tells nfcapd the destination directory to store NetFlow data.

**Step 9:** Cron job to expire netflow data

> ```
$ sudo crontab -e```

Add the following line:

> ```
0 9 * * 1,3,5 nfexpire -e <path to NetFlow data> -t 2d```

_This crontab job will delete NetFlow dumps at 9am every Monday, Wednesday, and Friday that are older than 2 days._

To install NFsen please visit [NFsen](https://code.google.com/p/renisac/wiki/NFSen_Install)

# Install Justin Azoff's NetFlow Indexer #

Requires Python version > 2.5


**Step 1:** Install prerequisites
> ```
$ sudo apt-get install python-pip python-xapian xapian-tools python-ipy python-virtualenv git```

**Step 2:** Download source from lynx as a zip or clone from git
> ```
$ sudo git clone https://github.com/JustinAzoff/netflow-indexer.git
$ sudo tar -czvf netflowindexer-0.1.9.tar.gz netflow-indexer```

**Step 3:** Run pip installer
> ```
$ sudo pip install -s -E /usr/local/python_env/ netflowindexer-0.1.9.tar.gz```

**Step 4:** Export python binary path to path variable
> ```
$ PATH=/usr/local/python_env/bin/:$PATH
$ export PATH```

**Step 5:** Add the following lines to the end of .bashrc
> ```
$ vi /home/<username>/.bashrc```

> ```
PATH=/usr/local/python_env/bin:$PATH
export PATH```

In the terminal, source the newly configured .bashrc

> ```
$ source /home/<username>/.bashrc```

Open visudo and add the same path information

> ```
$ sudo visudo```

**Add the same path to the second line 'Defaults secure\_path'**

Should now look like:

> ```
Defaults       secure_path="/usr/local/python_env/bin:/usr/local/bin:/usr/local/sbin:/usr/bin:/usr/sbin:/bin:sbin"```

**Step 6:** Move daily\_compact.sh and xap\_compact.sh to the netflowindex directory. These scripts are used to compact the index database
> ```
$ sudo cp /home/<username>/netflow_indexer/examples/daily_compact.sh /var/log/netflowindex/daily_compact && sudo cp /home/<username>/netflow_indexer/examples/xap_compact.sh /var/log/netflowindex/xap_compact```

**Step 7:** Setup the ini file for netflow-index-update
Create file and write the netflow-indexer configuration file

Example:
> ```
 vi /var/log/netflowindex/nfdump.ini```

Then add the following lines:

> ```
[nfi]
indexer = nfdump_full
dbpath = /var/log/netflowindex
flowpath = /var/log/netflowdata
fileglob = %(flowpath)s/nfcapd.%(year)s%(month)s%(day)s*
allfileglob = %(flowpath)s/nfcapd.*
```

**Optional line in the config file**

> ```
pathregex = /profiles/:profile/:source/nfcapd```

**Step 8:** Perform a full index on the NetFlow data for the first time
> ```
$ sudo netflow-index-update /var/log/netflowindex/nfdump.ini --full-index```

**Step 9:** Setup Crontab job to index 5 minutes after every hour

> ```
$ sudo crontab -e```

**Pick a text editor**

Add the following lines:

> ```
MAILTO=root
PATH=/usr/local/python_env/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usrl/bin:/sbin:/bin
40 0 * * * cd /var/log/netflowindex && ./daily_compact > /dev/null
*/5 * * * * sleep 30;netflow-index-update <path to .ini file>
55 0 * * * netflow-index-cleanup <path to .ini file> -d
0 9 * * 1.3.5 nfexpire -e <path to NetFlow data> -t 2d && sleep 30;netflow-index-cleanup <path to ini file>```

_The last line says to expire netflow dumps older than two days every Monday, Wednesday, and Friday at 9am. Next, wait 30 seconds and clean the index database of all the removed flow dumps. This is just an example and can be adjusted to suit your needs._

## How to Use the Netflow-Indexer Tools ##

Clean the index database of old deleted NetFlow dumps. The -d says delete absent NetFlow dump files from the index database.

> ```
$ sudo netflow-index-cleanup /var/log/netflowindex/nfdump.ini -d```

Use the indexer to search for a specific IP. This is useful to find out which NetFlow dump a flow occurs in.

> ```
$ netflow-index-search-all /var/log/netflowindex/nfdump.ini 192.168.63.2```

Use the indexer to search for a specific IP and port 53. Also, send a filter to NFDump to search for all these flows.

> ```
$ netflow-index-search-all /var/log/netflowindex/nfdump.ini 192.168.63.2 -d -f 'port 53' | more```

Use the indexer to search for a specific IP and print the time of occurence and filename of the respective NetFlow dump.

> ```
$ netflow-index-search-all /var/log/neflowindex/nfdump.ini 192.168.63.2 -c time,filename```

Using the previous command you can narrow your search by issuing a query to the index database for a particular day. After you isolate a specific day to search, use the indexer to search all flow dumps for a  single day. The -d switch will use the flow tool specified in the .ini file (NFDump) to query all flow dumps for that particular day. The -f switch sends a filter to the flow tool, in this case, port 80. This method can greatly decrease your search time.

> ```
$ netflow-index-search /var/log/netflowindex/nfdump.ini/ /var/log/netflowindex/<yearmonthday>.db/ <ip to search> -d -f 'port 80'```

<br>
<h4><- <a href='https://code.google.com/p/renisac/wiki/DeploymentStrategies?ts=1370391879&updated=DeploymentStrategies'>Return to Deployment Strategies</a></h4>