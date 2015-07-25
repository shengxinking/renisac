# Introduction #

This section assumes that [NFDump](https://code.google.com/p/renisac/wiki/Collector_Machine#Install_NFDump) has already been compiled (using the --enable-nfprofile option) and installed. This section also assumes a source of NetFlow (or sFlow) has been configured (see [Probe/Exporter Machine](https://code.google.com/p/renisac/wiki/ProbeExporterMachine) or [Configure Flow Data Export](https://code.google.com/p/renisac/wiki/FlowDataExport)).

# Install NFSen #

**Step 1:** Install prerequisite packages

> ```
$ sudo apt-get install apache2 libapache2-mod-php5 php5-common libmailtools-perl librrds-perl fping libsocket6-perl```

**Step 2:** Create directory for web root and NFSen binaries

> ```
$ sudo mkdir /var/www/nfsen && sudo mkdir /usr/local/nfsen```

**Step 3:** Download NFSen

> ```
$ cd /tmp
$ wget http://downloads.sourceforge.net/project/nfsen/stable/nfsen-1.3.6p1/nfsen-1.3.6p1.tar.gz```
> ```
$ sudo tar -zxvf nfsen-1.3.6p1.tar.gz```

**Step 4:** Create a new user and group for NFSen. Add to apache user group

> ```
$ sudo useradd -m nfsen```
  * -m switch creates home directory for user nfsen

> ```
$ sudo passwd nfsen```
  * Set a new password for nfsen. For testing purposes we assign the password iu1234

> ```
$ sudo groupadd nfsen```
  * If group already exists proceed to the next command

> ```
$ sudo usermod -G nfsen nfsen```
  * Adds the nfsen user to the nfsen group

Create an admin group for nfsen and add it to the apache user group

> ```
$ sudo groupadd nfsenadmin```
> ```
$ sudo usermod -a -G nfsenadmin nfsen```
> ```
$ sudo usermod -a -G nfsenadmin www-data```
  * (might be user wwwrun on other distros)

**Step 5:** Copy the nfsen configuration file to a new file

> ```
$ cd nfsen-1.3.6p1
$ sudo cp etc/nfsen-dist.conf etc/nfsen.conf```

**Step 6:** Edit the configuration file

> ```
$ sudo vi etc/nfsen.conf```

Change the following options or ensure they are already configured as shown.

> ```
$BASEDIR = "/usr/local/nfsen";
$HTMLDIR = "/var/www/nfsen/";
$PREFIX = "/usr/local/bin" # This is the path to the nfdump binary. Should be changed if nfdump was installed elsewhere.
$USER = "www-data";
$WWWUSER = "www-data";
$WWWGROUP = "www-data";
$syslog_facility = 'local3';```

**Add NetFlow or sFlow sources under %sources tag**

NetFlow and sFlow sources are differentiated by the **'type'** parameter. Replace the type with _'sflow'_ if specifying an sFlow source **(only supported if NFDump was compiled with --enable-sflow switch)**. The **'IP'** parameter is also optional.

> ```
%sources = (
'NF_Collect1' => { 'port' => '2055', 'IP' => '129.79.49.86', 'col' => '#0000ff', 'type' => 'netflow' }
);```

#### Other optional parameters: ####

For our installation, we modified these parameters as a matter of preference. They could be left to their defaults and still result in a successful install.

> ```
$EXTENSIONS = 'all'; # Enabled extensions
$SUBDIRLAYOUT = 0; # Indicates a flat directory hierarchy
$CONFDIR = "/etc"; # Where nfsen.conf file is stored
$PROFILEDATADIR = "/var/log/netflowdata"; # Where NetFlow data is stored```

#### Explanation: ####

This section explains these parameters in more detail. By default NFSen installs everything in one location {BASEDIR}. For our installation, we modified the nfsen.conf parameters illustrated above. This was done just as a matter of preference. They could be left to their defaults and still result in a successful install.

**$EXTENSIONS** - Identifies the list of extensions to be stored in the NetFlow dumps. Each version of NetFlow supports different extensions as well. Check the man pages for _nfcapd_ for a list of available options. If no preference is desired, 'all' enables all possible extensions.

**$SUBDIRLAYOUT** - This parameter indicates the directory structure for storing NetFlow dumps. The comment box explains the available options. If importing existing NetFlow data, make sure to use the '0' value. This tells nfsen to use a flat directory structure (the default behavior of nfcapd). Rebuilding an existing directory structure to a new directory structure is explained later.

**$CONFDIR** - Where nfsen.conf file is stored. NFSen is designed to install everything in a single location. By default nfsen.conf is stored in {BASEDIR}/etc. This can be left to its default if desired. Just make note of the path when changing configuration parameters for NFSen.

**$PROFILEDATADIR** - Where all the NetFlow data for each NFSen profile is stored.

**Step 7:** Install NFSen
> ```
$ sudo ./install.pl etc/nfsen.conf```

**Hit enter when prompted to install the appropriate perl modules**

**Step 8:** Export nfsen binary path to path variable
> ```
$ PATH=/usr/local/nfsen/bin/:$PATH
$ export PATH```

**Step 9:** Add the following lines to the end of .bashrc
> ```
$ vi /home/<username>/.bashrc```

> ```
PATH=/usr/local/nfsen/bin:$PATH
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
Defaults       secure_path="/usr/local/nfsen/bin:/usr/local/bin:/usr/local/sbin:/usr/bin:/usr/sbin:/bin:sbin"```

**Step 10:** Assign permissions for apache user

> ```
$ sudo chown -R www-data:www-data /var/www/nfsen
$ sudo chown -R www-data:www-data /usr/local/nfsen```
> ```
$ sudo ln -s /usr/local/nfsen/bin/nfsen /etc/init.d/nfsen # Symbolic link to run the nfsen daemon at startup
$ sudo update-rc.d nfsen defaults```

**Step 11:** Configure rsyslog to send nfsen log messages to a file (Optional)

**Although, this step is listed as optional, it is helpful for debugging issues with NFSen and verifying the successful install of plugins.**

Open /etc/rsyslog.d/50-default.conf

> ```
$sudo vi /etc/rsyslog.d/50-default.conf```

Add the following line to the standard log files section:

> ```
 local3.*			/var/log/nfsen/nfsen.log```

Restart rsyslogd for changes to take effect

> ```
$sudo service rsyslog restart```

**Step 12:** Start NFSen

**Make sure to apply the _[Error: Frontend - Backend version mismatch](https://code.google.com/p/renisac/wiki/NFSen_Install#Error:_Frontend_-_Backend_version_mismatch)_ patch under Known Issues. May be applied before or after starting the NFSen service.**

> ```
$ sudo service nfsen start```

# Known Issues #

This section is for any known issues or problems encountered. Feel free to comment about any errors/bugs/problems not covered in this section. If possible, indicate the following: version of NFSen, the problem that was experienced, conditions which caused the problem to happen, known cause of the problem, solutions found, and any additional information which helps describe the problem. Even if you encountered a problem and have not found a solution, please let us know. We will post any discovered issues in this section. Your comments and issues will help make this the most comprehensive NetFlow manual out there.

## Error: Frontend - Backend version mismatch ##

**Status:** Resolved

**Version:** 1.3.6p1

#### Condition: ####

Connecting to the NFSen web portal displays the above error message on the main page.

#### Cause: ####

Tried to connect to NFSen without a valid browser session.

#### Additional Information: ####

None

#### Solution: ####

Apply the following patch:

**Step 1:** Open /var/www/nfsen/nfsen.php with a text editor

> ```
$ sudo vi /var/www/nfsen/nfsen.php```

**Step 2:** Apply the following patch

Look for these lines starting at line 44:

> ```
 // Session check
if ( !array_key_exists('backend_version', $_SESSION ) || $_SESSION['backend_version'] !=  $expected_version ) {
session_destroy();
session_start();
$_SESSION['version'] = $expected_version;
print "<h1>Frontend - Backend version missmatch!

Unknown end tag for &lt;/h1&gt;

\n";
}```

Replace with these lines:

> ```
 // Session check
if ( !array_key_exists('backend_version', $_SESSION ) || $_SESSION['backend_version'] !=  $expected_version ) {
if ( array_key_exists('backend_version', $_SESSION ) &&
$_SESSION['backend_version'] != $expected_version ) {
session_destroy();
session_start();
$_SESSION['version'] = $expected_version;
print "<h1>Frontend - Backend version missmatch!

Unknown end tag for &lt;/h1&gt;

\n";
}
}```

**Step 3:** Save and exit