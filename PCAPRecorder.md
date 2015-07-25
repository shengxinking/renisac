# Introduction #

In the previous section, we briefly described what entails a full packet capture system. In this section, we will discuss how basic command line tools can be utilized to implement a custom full packet capture system (FPC). TCPDump is the most popular for long term packet capture; however, we also present some alternative command line tools. In addition, we will describe how they can be utilized to rotate packet dumps. Finally, we will present some open source fully comprehensive solutions for indexing pcaps.

# Design Considerations #

1. **Know thy network architecture**

This may seem like a no brainer but is still an important step to determine placement of the FPC system. One could start by identifying the entry and exit points of the network, that is, where users are truly leaving and entering the network. One example of this could be a proxy server or boundary router. In addition, an FPC system could be placed near a VPN where teleworkers and remote users connect into the environment.  Ideally, it would be nice to see all traffic throughout the network. Therefore, an FPC system could be positioned where traffic from multiple sources becomes aggregated. Having an FPC system positioned near where multiple subnets converge reduces the need to deploy an FPC system to every subnet.

2. **To SPAN, or to TAP, that is the question...**

Once placement has been established, traffic must be sent to the FPC system. In a previous chapter, we discussed the advantages and disadvantages to SPAN and TAP. Ultimately, in deciding whether to use a SPAN or TAP, the deciding factor should come down to cost and the types of traffic being analyzed. Please refer to [SPAN vs TAP](https://code.google.com/p/renisac/wiki/SwitchConfiguration#SPAN_vs_TAP) to determine which solution best fits your needs. Refer to [Switch Configuration](https://code.google.com/p/renisac/wiki/SwitchConfiguration) for SPAN configuration guides for Cisco, HP, and Juniper switches.

3. **Know thine enemy**

Understanding organizational threats is an absolute must. If a threat exists, some thought should be taken to consider what application data should be extracted from packet information. Terabytes of pcaps are useless unless actionable intelligence can be extracted and acted on. This consideration is also part of the AppFlow layer of the [previously presented](https://code.google.com/p/renisac/wiki/Netflow_Cookbookv1#Prologue) traffic logging model. Although, these functions occur at different layers, the AppFlow layer can operate in tandem to the Packet Capture layer. Commercial AppFlow (based on the IETF IPFIX protocol [RFC 5101](http://tools.ietf.org/html/rfc5101) and [RFC 5102](http://tools.ietf.org/html/rfc5102)) products aside, packet dissectors written in languages such as C, Perl, and Python can be used to extract application data during packet capture.

4. **Determine traffic volume and data rentention**

We addressed the issue of traffic volume in the [Prologue](https://code.google.com/p/renisac/wiki/Netflow_Cookbookv1#Prologue) of this guide. Therefore, we can assume the following:

The amount of storage for packet capture on a fully saturated 1Gbps link can exceed
  * 1 day = 6TB
  * 1 week = 42TB
  * 1 month = 180TB

Although it is unlikely a 1Gbps link would be fully saturated for a whole day, week, or month, these figures provide a good starting point for deciding how much storage allocation is enough. Assuming traffic volume will regularly fluctuate, a good starting place should be 1TB per day. Additionally, if SPAN is being utilized to mirror traffic, one should also plan for the extra traffic volume. Since packets are being replicated, spanning may require additional links and storage capacity. Especially if one is considering utilizing Remote SPAN. In terms of storage capacity, one should also decide on data retention or the useful life of the data. This will determine how much storage capacity will be necessary.

5. **I have all the time in the world...**

The truth of the matter is you likely do not. The needle in the haystack method for searching is typically the least efficient in regards to resources and time. In the [prologue](https://code.google.com/p/renisac/wiki/Netflow_Cookbookv1#Prologue), we presented a tiered model for an efficient traffic logging system. Search times can be substantially reduced by indexing pcaps using Cache, NetFlow and application information (AppFlow). Adopting such an approach can substantially reduce lookup times. Once information has been gathered, and a search window has been narrowed, one can then query relevant data and ignore the rest. However, without a comprehensive system, searching through Terabytes of pcaps could take hours, or even more likely days. We can record all the traffic in the world, but, unless we gather actionable intelligence, the data is essentially useless. Likewise, if a security analyst is unable to quickly lookup the state of the network, effective and timely incident response is impossible.

# Custom Full Packet Capture #

## TCPDump ##

Next we will illustrate a TCPDump example and some useful scripts for post processing and indexing of pcaps. Some of the features of TCPDump are as follows:

### Features ###
  * Extremely powerful tool for packet capture
  * Filter syntax is easy to learn
  * Can later play back recorded pcaps and apply filters
  * Uses libpcap format
  * Can save whole packets or only the headers
  * Can apply filters during capture to avoid recording unwanted traffic
  * Can perform post processing upon rotating pcaps

Example usage:
```
$ sudo tcpdump -s0 -nn $EXCLUDED -G60 -w "%Y-%m-%d-%H%M.pcap" -i $IFACE -z "pcap_parsing_script.sh"```

  * -s0 switch tells tcpdump to capture the entire packet (this switch option forces the use of the default value of 65535 bytes)
  * -nn switch prevents the resolution of hostnames and port names. Without this option tcpdump issues a DNS lookup for resolution of the aforementioned items. Very useful when try to monitor transparently. Also, good practice for masking monitoring activity from an attacker.
  * -G rotates pcap files specified by the -w switch. The number indicates how many seconds until tcpdump should rotate to a new pcap file.
  * (Optional) **expression** An expression can be included as part of the command. In this instance, the variable $EXCLUDED is executed as an expression. This variable is passed by a shell script which contains the path to a plain text file with pre-written tcpdump filters. This is explained in the next section.
  * -w switch tells tcpdump what file captured packets should be written to. The parameters in the "" indicate the comformity to a strftime naming convention which is Year-Month-Day-HourMinute.
  * -i specifies the interface to listen on. In this instance, the variable $IFACE is passed from another script which will be explained in the next section.
  * -z switch indicates a post rotate command which will be executed when tcpdump finishes writing to a file. In the example above, a shell script is called which is explained in the next section.

### Post Processing/Indexing ###

This is one of the more complicated aspects of the system. There are many scripts publicly available online which perform this function. The following article provides a great write up on this topic using tcpdump: [Custom Full Packet Capture](http://www.sans.org/reading_room/whitepapers/logging/custom-full-packet-capture-system_34177). In particular, Appendix A-E provide example shell scripts which perform both indexing of IP and Application information. They can be modified to suit your needs. For this section, we describe the scripts presented in this paper. The goal of this section is to supply readers with a starting point to develop their own FPC system using tcpdump. We feel the scripts presented in the above paper serve this purpose very well. They were also tested in our lab, which worked quite nicely. An important note, the presented scripts only dissect IP and TCP packets. More logic must be added to the perl script to dissect other types of packets such as UDP and ICMP.

#### Prerequisites ####

**Step 1:** Set AppArmor for TCPDump from enforcement to complain mode

> ```
$ sudo grep tcpdump /sys/kernel/security/apparmor/profiles```

If the output is:
> ```
/usr/sbin/tcpdump (enforce)```

Then do the following:
> ```
$ sudo apt-get install apparmor-utils
$ sudo aa-complain /usr/sbin/tcpdump```

**Step 2:** Configure CPAN

> ```
$ sudo cpan```

Accept the defaults then answer the following the questions:

  * Choose answer "sudo" for the question "What approach do you want?"

  * Would you like me to automatically choose some CPAN mirror sites for you? Choose "yes"

Once complete, commit the configuration and exit

> ```
cpan[1]> o conf commit
cpan[2]> quit```

**Step 3:** Install the required perl modules to read tcpdump/libpcap logs and to assemble/disassemble Ethernet packets

> ```
$ sudo perl -MCPAN -e 'install Net::TcpDumpLog'```
> ```
$ perl -MCPAN -e 'install NetPacket::Ethernet'```

#### Discussion on Scripts ####

We highly recommend examining the source code to understand how these scripts work. In this section, we describe at a basic level the sequence of events and how they are executed. The tcpdump command exhibited above is passed the variable $IFACE and $EXCLUDED from the shell script in Appendix A. The $IFACE variable represents the interface to sniff which could be one or multiple interfaces. The $EXCLUDED variable contains the path to a text file with pre-written tcpdump filters. This allows for a cleaner tcpdump command. The -z switch of the tcpdump command then executes a post rotate command upon finishing writing to a file.

This switch is useful for performing compression or post processing operations. In this instance, tcpdump is executing the shell script in Appendix C. First this script passes the name of the rotated pcap file to a perl script and executes the script which is illustrated in Appendix D. This script is a packet dissector written using perl which dissects both IP and TCP packets. Similar to the format of a NetFlow record, the perl script prints in plain text to a file the following information: timestamp, source IP, source port, destination IP, and destination port. The timestamp information is utilized as an index for these recorded items.

This is advantageous since text files are much smaller than a raw pcap file. This allows one to more quickly lookup information than a typical tcpdump filter. After this operation is complete, the shell script in Appendix C then extracts HTTP and SMTP application data from the payloads contained within the pcap using ngrep. Finally, the shell script runs Snort which is passed the rotated pcap file as an argument.

# Other Tools #

## dumpcap ##

### Features ###
  * Command line utility which comes with wireshark
  * Utilized by both wireshark and tshark for capturing packets
  * Less overhead than both wireshark and tshark
  * Better solution for long term pcap capturing than wireshark or tshark
  * Supports lipcap format and pcap-ng (Uses pcap-ng by default)

Usage example:
```
$ sudo dumpcap -i 1 -b duration:3600 files:25 -f "ip host 10.0.1.1 or ip host 10.0.1.12" -w <desired directory path>/filename.pcap```

  * -i is the interface number to listen on
  * -b says to run in "multiple files" mode. duration: means to rotate pcaps every 3600 seconds (1 hour). files: means once the maximum number of files have been saved delete the oldest file and start a new one. These parameters in conjunction indicate we will be keeping up to 24 hours worth of pcaps.
  * -f specifies a capture filter. In this instance, filter for ip hosts 10.0.1.1 and 10.0.1.12
  * -w specifies the output file to save recorded packets

The article [Long Term Traffic Capture With Wireshark](http://packetlife.net/blog/2011/mar/9/long-term-traffic-capture-wireshark/) explains dumpcap usage in more detail. Also, the article [PCAP Files Are Great Aren't They??](http://blog.spiderlabs.com/2012/12/pcap-files-are-great-arnt-they.html) provides excellent tips for analyzing pcaps with tshark.


# Open Source Full Packet Capture Systems #

## Open FPC ##

http://www.openfpc.org/

<a href='http://www.youtube.com/watch?feature=player_embedded&v=YcPUgs-fDPs' target='_blank'><img src='http://img.youtube.com/vi/YcPUgs-fDPs/0.jpg' width='425' height=344 /></a>

[OpenFPC](http://www.openfpc.org/) is a set of tools that combine to provide a lightweight full-packet network traffic recorder & buffering system. It's design goal is to allow non-expert users to deploy a distributed network traffic recorder on commercial-off-the-shelve (COTS) hardware while integrating into existing alert and log management tools [OFPC](http://www.openfpc.org/). It was originally designed as an add-on to other network communication and security technologies (open source or commercial). OFPC has the capability to show complete network traffic associated with a particular network security event [OFPC](http://www.openfpc.org/).


Features

  * Support for IPv4 and IPv6 (after some modifications).
  * Comes with a central manager web user-interface.
  * Supports remote session request via the web interface.
  * Supports session capture (captures the complete session associated with a particular network security event)
  * User control interface
  * Command-line interface
  * Log formats supported: Snort Fast alert, Snort Syslog, Exim4, and Sourcefire 3D Defense Center, Sourcefire 3D Sensor.


For an example use of OFPC please visit: [Introducing OpenFPC – An Open Full Packet Capture Framework for Network Traffic Recording](http://leonward.wordpress.com/2010/05/17/introducing-openfpc-an-open-full-packet-capture-framework-for-network-traffic-recording/)


## Moloch ##

https://github.com/aol/moloch

<a href='http://www.youtube.com/watch?feature=player_embedded&v=BWxrXJz_Ay0' target='_blank'><img src='http://img.youtube.com/vi/BWxrXJz_Ay0/0.jpg' width='425' height=344 /></a>

[Moloch](https://github.com/aol/moloch#what-is-moloch) is an open source, large scale IPv4 packet capturing (PCAP), indexing and database system. A simple web interface is provided for PCAP browsing, searching, and exporting. APIs are exposed that allow PCAP data and JSON-formatted session data to be downloaded directly. Simple security is implemented by using HTTPS and HTTP digest password support or by using apache in front. Moloch is not meant to replace IDS engines but instead work along side them to store and index all the network traffic in standard PCAP format, providing fast access. Moloch is built to be deployed across many systems and can scale to handle multiple gigabits/sec of traffic.

Features

  * Comes with a central manager web user-interface.
  * Only supports IPv4
  * Can automatically generate topologies based IPs and can also organize according to country of origin
  * Wireshark like filters
  * Supports session capture (captures the complete session associated with a particular network security event)