# Introduction #

In section [6.1.1](https://code.google.com/p/renisac/wiki/NFSen_Introduction#The_NFSen_Advantage) we illustrated how the powerful feature set of NFSen can be used for advanced network monitoring. This section expands on the concept of traffic profiling and classification using NFSen profiles, channels, and plugins.

# Making Use of Profiles/Channels #

In the previous section, we demonstrated how NFSen could be utilized to separate traffic flows into distinct classifications using profiles and channels. Channels are extremely powerful and can be utilized in many different ways. For instance, perhaps your goal is to create a channel for a node or collection of nodes. The NFSen filter applied to the channel could then consider the network address or individual IP's of each node. The channel filter could also filter flows by port/protocol. As an example, a channel filter could be written to filter the network address of a web server cluster and traffic flowing to destination ports 80 and 443 (http and https).

Another example is to take the same web server cluster and write a filter which takes into consideration the network address of the cluster, and destination ports 22, 80, and 443. After generation, the RRD graphs in NFSen illustrate that 90% of traffic is targeted at SSH and only 10% of traffic is web. This could indicate a brute force attack on SSH. These are only just a couple of examples that demonstrate the power of profiles/channels in NFSen. The possibilities are virtually limitless. However, manually navigating through the web UI to accomplish this task is a long and arduous task, especially on a large scale. In the next section, we discuss how the NFSen commandline can accomplish this same task in a much shorter time period.

## Leveraging NFSen CLI for Profile/Channel/Filter Generation ##

This section is open for future work. This section is meant to be a guide on setting up groups, profiles, channels, and filters using the NFSen CLI.

## Scripting NFSen Group/Profile/Channel/Filter Generation ##

Artem Nosulchik wrote an [excellent article](http://www.linuxscrew.com/2012/03/15/nfsen-traffic-classification-breakdown/) on using NFSen to classify traffic by port/protocol using NetFlow data. The script presented in this section was authored by Artem Nosulchik. The script takes the process of generating profiles, groups, channels, and filters (which could take many hours), and completes the task in only a matter of seconds. We made a minor modification to the base script to include a new file (sources.list) which contains a list of NetFlow sources (as defined by the %sources variable in nfsen.conf). With this enhancement, one could create channels and filters for one or many sources of NetFlow without having to modify the main script. **The modified code can be found in [Appendix A](https://code.google.com/p/renisac/wiki/Traffic_Breakdown#Appendix_A).**

**Please Note:** We experienced a limitation on the length of NFSen filters. This [problem](https://code.google.com/p/renisac/wiki/Traffic_Breakdown#NFSen_filters_in_the_web_UI_end_in_mid_statement_after_a_certain) is discussed in more detail in the Known Issues section of this page. Please let us know in the comments if your results differ.

**Step 1:** Save the base script **add.nfsen.channels.sh** to any directory. Just make sure to note the path

**Step 2:** Create the .list files in the syntax specified in [Appendix A](https://code.google.com/p/renisac/wiki/Traffic_Breakdown#Appendix_A)

**Step 3:** Execute the **add.nfsen.channels.sh** shell script using the following syntax

Commandline Syntax:

> ```
$ sudo ./add.nfsen.channels.sh <profile-name> "<ip-address>/<CIDR> <ip-address>/<CIDR> [...]" <group-name>```

> Example 1:

> ```
$ sudo ./add.nfsen.channels.sh WebSpider1 "129.79.49.81/32" WebSpiders```

> Example 2:

> ```
$ sudo ./add.nfsen.channels.sh AllWebSpiders "129.79.49.81/32 129.79.49.82/32 129.79.49.83/32" WebSpiders```

> Example 3:

> ```
$ sudo ./add.nfsen.channels.sh ServerCluster "10.10.10.0/24" DMZ```

> Example 4:

> ```
$ sudo ./add.nfsen.channels.sh AllTraffic "0.0.0.0/0" AllTraffic```

**Step 4:** Verify the creation of the profile/group

By commandline:

> ```
$ sudo nfsen -A```

The output will be in the format of **<group name>/<profile name>** (if the profile belongs to a group) or **<profile name>**

Example Output:

> ```

HTML_Traffic
live
NetFlow_Collectors/iucollect
NetFlow_Sources/iuexport
Test/Test
WebSpiders/allwebspiders
WebSpiders/webspider1
WebSpiders/webspider2
WebSpiders/webspider3
```

One could also verify profile and group assignment by checking the NFSen web UI. Click the drop down menu in the _Profile:_ tab on the top right corner. Anything highlighted in **bold** indicates a group. Clicking on the group name in the list will reveal all member profiles. Likewise, anything not in bold is a profile. This is illustrated in the graphic below.

![http://www.ren-isac.net/img/googlecode/7.1-Profile-Group.jpg](http://www.ren-isac.net/img/googlecode/7.1-Profile-Group.jpg)

Select the newly created profile and navigate to the details tab. You will notice individual _in_ and _out_ channels have been generated for each port/protocol in **protocols.list**. Using our provided .list files, the output should resemble the following graphic.

![http://www.ren-isac.net/img/googlecode/7.1-New-Profile.jpg](http://www.ren-isac.net/img/googlecode/7.1-New-Profile.jpg)

**Please Note:** We experienced a limitation with the length of NFSen channel filters. If a lot of time has passed and the RRD graphs are still not being drawn, then please refer to the [problem](https://code.google.com/p/renisac/wiki/Traffic_Breakdown#NFSen_filters_in_the_web_UI_end_in_mid_statement_after_a_certain) discussed in the Known Issues section of this page.

**Step 5:** Verify the channel filters

Once the desired profile is selected, navigate to the "Stats" tab. Here you will find the details about the profile configuration as well as the configuration for each channel. Output should resemble the following graphic.

![http://www.ren-isac.net/img/googlecode/7.1-Profile-Stats.jpg](http://www.ren-isac.net/img/googlecode/7.1-Profile-Stats.jpg)

The channel filters are stored to disk as text files. You can open and view the filters by navigating to path of $PROFILESTATDIR (variable in /etc/nfsen.conf which indicates the path NFSen is using to store the profile configuration, RRD graphs, and filters).

> ```
$ cat /usr/local/nfsen/profiles-stat/<group name>/<profile name>/<channel name>-filter.txt```

Example:

> ```
$ cat /usr/local/nfsen/profiles-stat/WebSpiders/allwebspiders/dns-out-filter.txt```

Expected Output:

> ```
(src net 129.79.49.83/32 or src net 129.79.49.82/32 or src net 129.79.49.83/32) and ((dst port 53 and proto udp) or (dst port 53 and proto tcp) or (dst port 953 and proto tcp) or (dst port 953 and proto udp) or (dst port 5353 and proto udp))```

# Port Tracker Plugin #

This section is open for future work. This section should provide an overview of the port tracker plugin.

## Install Port Tracker ##

This section should include the installation steps for installing the Port Tracker plugin. Also, this section should contain verification steps. Pictures should be included where necessary.

<br><br>

<h1>Appendix A</h1>

<h3>sources.list</h3>

Syntax:<br>
<br>
<blockquote><pre><code>Source1|Source2|Source3|[...]</code></pre></blockquote>

Example sources.list:<br>
<br>
<blockquote><pre><code>NF_Collect1|NF_Collect2</code></pre></blockquote>

<h3>protocols.list</h3>

Syntax:<br>
<blockquote><pre><code>&lt;port number&gt;/&lt;protocol&gt; &lt;port range start-range end&gt;/&lt;protocol&gt; [...]|&lt;traffic pattern name&gt;</code></pre></blockquote>

Example protocols.list:<br>
<br>
<blockquote><pre><code><br>
80/tcp 80/udp 443/tcp 443/udp|http<br>
20-21/tcp 20-21/udp 989-990/tcp 989-990/udp|ftp<br>
5190-5193/tcp 1863/tcp 531/tcp 531/udp 5050/tcp 5190/tcp 5222/tcp 5223/tcp 5269/tcp 5298/tcp 5298/udp 6660-6664/tcp 6665-6669/tcp 6679/tcp 6697/tcp 1247/udp 2940-3000/tcp|im<br>
5060-5061/tcp 5060-5061/udp|sip<br>
22/tcp 22/udp 911/tcp|ssh<br>
23/tcp 23/udp 107/tcp 992/tcp 992/udp|telnet<br>
110/tcp 995/tcp|pop3<br>
143/tcp 993/tcp|imap<br>
25/tcp 465/tcp|smtp<br>
53/udp 53/tcp 953/tcp 953/udp 5353/udp|dns<br>
161-162/tcp 161-162/udp|snmp<br>
1494/tcp 2512/tcp 2513/tcp 1604/udp 8082/tcp 27000/tcp 2598/tcp 9001-9002/tcp 9005/tcp 2897/tcp|citrix<br>
1194/tcp 1194/udp 5000/tcp 5000/udp 12972/tcp 32976/tcp|vpn<br>
1645-1646/tcp 1645-1646/udp 1812-1823/tcp 1812-1813/udp 2083/tcp|radius</code></pre></blockquote>

<h3>colors.list</h3>

<blockquote><pre><code><br>
#FF0000<br>
#FCFF00<br>
#00FF2A<br>
#FFA800<br>
#613702<br>
#133D7C<br>
#00EAFF<br>
#9600FF<br>
#FF00C6<br>
#B0F10F<br>
#746FAE<br>
#993366<br>
#B8860B<br>
#CCFFFF<br>
#660066<br>
#FF6666<br>
#0066CC<br>
#CCCCFF<br>
#000066<br>
#0000FF<br>
#A7A3A3<br>
#C4B782</code></pre></blockquote>

<h3>add.nfsen.channels.sh</h3>

<i>Change the "nfsen" variable to the path of the nfsen binary if its not in the default location.</i>

<blockquote><pre><code><br>
#!/bin/bash<br>
nfsen="/usr/local/nfsen/bin/nfsen"<br>
remote=$1<br>
ips=$2<br>
group=$3<br>
sources=$(cat ./sources.list)<br>
if [[ $1 == "" || $2 == "" || $3 == "" ]];then<br>
exit<br>
fi<br>
$nfsen -a $group/$remote shadow=1<br>
dstfilter=""<br>
for dstip in $2;do<br>
if ((${#dstfilter}&gt;0));then<br>
dstfilter=$dstfilter" or dst net "$dstip<br>
else<br>
dstfilter="(dst net "$dstip<br>
fi<br>
done<br>
dstfilter=$dstfilter")"<br>
srcfilter=""<br>
for srcip in $2;do<br>
if ((${#srcfilter}&gt;0));then<br>
srcfilter=$srcfilter" or src net "$srcip<br>
else<br>
srcfilter="(src net "$dstip<br>
fi<br>
done<br>
srcfilter=$srcfilter")"<br>
colorcounter=0<br>
cat ./protocols.list | while read line;do<br>
echo<br>
echo<br>
echo $line<br>
let colorcounter++<br>
currentcolor=$(head -n $colorcounter colors.list | tail -1)<br>
dstportfilter=""<br>
srcportfilter=""<br>
otherdstportfilter=""<br>
othersrcportfilter=""<br>
serviceport=$(echo $line | awk -F '|' '{print$1}')<br>
servicename=$(echo $line | awk -F '|' '{print$2}')<br>
for line1 in $serviceport;do<br>
if [[ -n $(echo $line1 | grep '-') ]];then<br>
#range<br>
port1=$(echo $line1 | awk -F '/' '{print$1}' | awk -F '-' '{print$1}')<br>
port2=$(echo $line1 | awk -F '/' '{print$1}' | awk -F '-' '{print$2}')<br>
proto=$(echo $line1 | awk -F '/' '{print$2}')<br>
let "port1n=port1-1"<br>
let "port2n=port2+1"<br>
if ((${#srcportfilter}&gt;0));then<br>
srcportfilter=$srcportfilter" or (src port &gt; $port1n and src port &lt; $port2n and proto $proto)"<br>
else<br>
srcportfilter="(src port &gt; $port1n and src port &lt; $port2n and proto $proto)"<br>
fi<br>
if ((${#dstportfilter}&gt;0));then<br>
dstportfilter=$dstportfilter" or (dst port &gt; $port1n and dst port &lt; $port2n and proto $proto)"<br>
else<br>
dstportfilter="(dst port &gt; $port1n and dst port &lt; $port2n and proto $proto)"<br>
fi<br>
#for other<br>
if ((${#othersrcportfilter}&gt;0));then<br>
othersrcportfilter=$othersrcportfilter" and not (src port &gt; $port1n and src port &lt; $port2n and proto $proto)"<br>
else<br>
othersrcportfilter="not (src port &gt; $port1n and src port &lt; $port2n and proto $proto)"<br>
fi<br>
if ((${#otherdstportfilter}&gt;0));then<br>
otherdstportfilter=$otherdstportfilter" and not (dst port &gt; $port1n and dst port &lt; $port2n and proto $proto)"<br>
else<br>
otherdstportfilter="not (dst port &gt; $port1n and dst port &lt; $port2n and proto $proto)"<br>
fi<br>
else<br>
#norange<br>
port=$(echo $line1 | awk -F '/' '{print$1}')<br>
proto=$(echo $line1 | awk -F '/' '{print$2}')<br>
if ((${#srcportfilter}&gt;0));then<br>
srcportfilter=$srcportfilter" or (src port $port and proto $proto)"<br>
else<br>
srcportfilter="(src port $port and proto $proto)"<br>
fi<br>
if ((${#dstportfilter}&gt;0));then<br>
dstportfilter=$dstportfilter" or (dst port $port and proto $proto)"<br>
else<br>
dstportfilter="(dst port $port and proto $proto)"<br>
fi<br>
#for other<br>
if ((${#othersrcportfilter}&gt;0));then<br>
othersrcportfilter=$othersrcportfilter" and not (src port $port and proto $proto)"<br>
else<br>
othersrcportfilter="not (src port $port and proto $proto)"<br>
fi<br>
if ((${#otherdstportfilter}&gt;0));then<br>
otherdstportfilter=$otherdstportfilter" and not (dst port $port and proto $proto)"<br>
else<br>
otherdstportfilter="not (dst port $port and proto $proto)"<br>
fi<br>
fi<br>
done<br>
$nfsen --add-channel $group/$remote/$servicename-in  sourcelist="$sources" filter="$dstfilter and ($srcportfilter)" colour="$currentcolor" sign=+ order=$colorcounter<br>
$nfsen --add-channel $group/$remote/$servicename-out sourcelist="$sources" filter="$srcfilter and ($dstportfilter)" colour="$currentcolor" sign=- order=$colorcounter<br>
otherdstportfilterall=$otherdstportfilterall" and ("$otherdstportfilter")"<br>
othersrcportfilterall=$othersrcportfilterall" and ("$othersrcportfilter")"<br>
echo $otherdstportfilterall &gt; /tmp/otherdstportfilterall<br>
echo $othersrcportfilterall &gt; /tmp/othersrcportfilterall<br>
echo $colorcounter &gt; /tmp/colorcounter<br>
done<br>
if [[ -s /tmp/otherdstportfilterall &amp;&amp; -s /tmp/othersrcportfilterall &amp;&amp; -s /tmp/colorcounter ]];then<br>
otherdstportfilterall=$(cat /tmp/otherdstportfilterall)<br>
othersrcportfilterall=$(cat /tmp/othersrcportfilterall)<br>
colorcounter=$(cat /tmp/colorcounter)<br>
let colorcounter++<br>
$nfsen --add-channel $group/$remote/other-in  sourcelist="$sources" filter="$dstfilter $othersrcportfilterall" colour="#000000" sign=+ order=$colorcounter<br>
$nfsen --add-channel $group/$remote/other-out sourcelist="$sources" filter="$srcfilter $otherdstportfilterall" colour="#000000" sign=- order=$colorcounter<br>
fi<br>
rm -f /tmp/otherdstportfilterall<br>
rm -f /tmp/othersrcportfilterall<br>
rm -f /tmp/colorcounter<br>
$nfsen --commit-profile $group/$remote</code></pre></blockquote>

<br><br>

<h1>Known Issues</h1>

This section is for any known issues or problems encountered. Feel free to comment about any errors/bugs/problems not covered in this section. If possible, indicate the following: version of NFSen, the problem that was experienced, conditions which caused the problem to happen, known cause of the problem, solutions found, and any additional information which helps describe the problem. Even if you encountered a problem and have not found a solution, please let us know. We will post any discovered issues in this section. Your comments and issues will help make this the most comprehensive NetFlow manual out there.<br>
<br>
<h2>NFSen filters in the web UI end in mid statement after a certain length</h2>

<b>Status:</b> Unresolved<br>
<br>
<b>NFSen Version:</b> 1.3.6p1<br>
<br>
<b>NFSen Plugin Name:</b> Not applicable<br>
<br>
<b>NFSen Plugin Version:</b> Not Applicable<br>
<br>
<h4>Condition:</h4>
After a certain number of characters, the NFSen web UI ends a filter in mid statement. This results in no RRD graphs being drawn for the profile.<br>
<br>
<h4>Cause:</h4>
The <i>other-in</i> and <i>other-out</i> channel filters, generated by the <b>add.nfsen.channels.sh</b> script, include a series of <b>not</b> statements to filter any port/protocol combination that hasn't already had a channel/filter generated for it. A long <b>protocols.list</b> file will cause the script to generate a long and complex NFSen filter for these channels as a result.<br>
<br>
<h4>Additional Information:</h4>
Checking the contents of the stored filters:<br>
<br>
<blockquote><pre><code>/usr/local/nfsen/profiles-stat/&lt;group name&gt;/&lt;profile-name&gt;/other-in-filter.txt</code></pre>
<pre><code>/usr/local/nfsen/profiles-stat/&lt;group name&gt;/&lt;profile-name&gt;/other-out-filter.txt</code></pre></blockquote>

reveals the correct and complete NFSen filter.<br>
<br>
Copy and pasting the complete filter into the channel configuration filterbox through the NFSen web UI produces the same result (filter ends in mid statement). Tried using tcpdump to examine the HTTP Post message when committing the change. The results show the complete filter being passed as a parameter.<br>
<br>
An additional step taken to diagnose the problem was through adjusting the length value of ArgDecode in /usr/local/nfsen/libexec/Nfcomm.pm line 953 to a higher value. However, this only applies to the length of arguments in the NFSen CLI. As previously explained, the complete channel filters are being generated and stored to disc. This rules out the possibility of the filter string being cut off in the CLI due to length. With this information we have concluded this is a potential front end issue with NFSen. Another cause could also be the php.ini configuration.<br>
<br>
<h4>Solution:</h4>
No solution has yet been discovered. As discussed, there seems to be a character limitation for channel filters in NFSen. A short term solution is to create s shorter <b>protocols.list</b> file when using the <b>add.nfsen.channels.sh</b> script. Another possibility is to delete the <i>other-in</i> and <i>other-out</i> channels from the profile.