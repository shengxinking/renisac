# Introduction #

NetFlow is a network protocol developed by Cisco Systems for collecting IP traffic information. It was developed to help network administrators gain a better understanding of what their network traffic looked like [[1](https://code.google.com/p/renisac/wiki/NetFlow#References)]. NetFlow does not store the actual data for any IP session but simply keeps track of IP sessions. While this can be viewed as a drawback by other security experts, it is this behavior that makes NetFlow the best solution in situations where storage requirements, cpu overhead and bandwidth are a huge concern [[1](https://code.google.com/p/renisac/wiki/NetFlow#References)]. There are three components to the collection process, the collector, probe/exporter and analyzer. NetFlow collects IP traffic statistics and then exports those statistics as NetFlow records to a NetFlow collector - where the traffic analysis will take place.

NetFlow supports several protocol versions but for the purposes of this guide we will focus on only two of those, namely NetFlow v5 and NetFlow v9. NetFlow v5 is the most popular protocol because it supports the old and common IPv4, however its biggest drawback is that it doesn't support IPv6. NetFlow v9 on the other hand supports both IPv4 and IPv6 and it is slowly replacing NetFlow v5.

# NetFlow v5 and NetFlow v9 Differences #

Though NetFlow v9 is fast becoming the industry standard, NetFlow v5 is still the most common. For all NetFlow versions, the NetFlow export datagram consists of a header and a sequence of flow records. The header contains information such as sequence number, record count, and system uptime. The flow record contains flow information, for example IP addresses, ports, and routing information. NetFlow version 9 export format is the newest NetFlow export format. As a result, it supports more features including the IP version 6 [[2](https://code.google.com/p/renisac/wiki/NetFlow#References)].

The main differences between the two versions are that NetFlow v5 is a "fixed format" data structure while NetFlow v9 is a "variable format" data structure - it is template based [[2](https://code.google.com/p/renisac/wiki/NetFlow#References)]. This simply means that NetFlow v5 cannot be modified or enhanced to include any new fields. Any attempt to modify it the fields will result in flows that are corrupt and cannot be interpreted by the collector. On the other hand, NetFlow v9 allows modifications or enhancements because it is template based. This means future enhancements can be done to NetFlow without making changes to the basic flow-record format. Thus, even after modifications, the record can still be analyzed by the collector.

NetFlow v5 and v9 Packets Diagrams

Below is a Data Flow Datagram for NetFlow v5 and a link to a Data Flow Datagram for v9

![http://www.ren-isac.net/img/googlecode/1.1.2-NetFlowv5.png](http://www.ren-isac.net/img/googlecode/1.1.2-NetFlowv5.png)

Image courtesy of  [[5](https://code.google.com/p/renisac/wiki/NetFlow#References)]

For a Data Flow Datagram of NetFlow v9, please visit [NetFlow v9 ](http://netflow.caligare.com/netflow_v9.htm#Netflow:)

# NetFlow Collector #

As the name suggests, a NetFlow Collector is simply a device that gathers network statistics/data using Cisco's NetFlow network protocol. Depending on how your network is designed and also the size of your network, you can have one or more NetFlow collectors. A NetFlow Collector can be a device such as a switch, router, server, or even a workstation. NetFlow Collectors store data according to guidelines configured by an administrator, so that the data can be accessed for analysis [[3](https://code.google.com/p/renisac/wiki/NetFlow#References)]. Depending on the software used, the collection process can be initiated by a couple of commands. This guide utilizes NFDump as the flow tool. NFDump is a suite of tools used to gather and process NetFlow data. NFDump comes with six daemons and has optional support for sFlow. These daemons are briefly described below courtesy of the project page on  sourceforge.net [[4](https://code.google.com/p/renisac/wiki/NetFlow#References)]. NetFlow collection is performed by the **_nfcapd_** deamon process.

nfcapd example:

> nfcapd -b 129.79.49.86 -p 2055 -4 -D -T all -l /var/log/netflowdata/

The above command is basically listening on ip 129.79.49.86 on port 2055 (UDP) and saving the net flow data to /var/log/netflowdata/

  1. -b bind host. Specifies the hostname/IPv4/IPv6 address to bind for listening.
  1. -p port
  1. -4 forces nfcapd to listen on IPv4 addresses only
  1. -D fork to background
  1. -T specifies the list of extensions, to be stored in the netflow file, in case all.
  1. -l  specifies the base directory to store the output files

For a detailed explanation on Nfdump and it's tools, commands included: [Nfdump tools](http://nfdump.sourceforge.net/#Nfdump_tools:)

  * **nfcapd** - netflow capture daemon.
> > Reads the netflow data from the network and stores the data into files. Automatically rotate files every n minutes. ( typically ever 5 min ) nfcapd reads netflow v5, v7 and v9 flows transparently. You need one nfcapd process for each netflow stream.

  * nfdump - netflow dump.
> > Reads the netflow data from the files stored by nfcapd. It's syntax is similar to tcpdump. If you like tcpdump you will like nfdump. Displays netflow data and can create lots of top N statistics of flows IP addresses, ports etc ordered by whatever order you like.

  * nfprofile - netflow profiler.
> > Reads the netflow data from the files stored by nfcapd. Filters the netflow data according to the specified filter sets ( profiles ) and stores the filtered data into files for later use.

  * nfreplay - netflow replay
> > Reads the netflow data from the files stored by nfcapd and sends it over the network to another host.

  * nfclean.pl - cleanup old data
> > Sample script to cleanup old data. You may run this script every hour or so.

  * ft2nfdump - Read and convert flow-tools data.
> > Reads flow-tools data from files or from stdin in a chain of flow-tools commands and converts the data into nfdump format to be processed by nfdump.


# NetFlow Probe/Exporter #

A NetFlow Probe/Exporter can be any piece of hardware or software that monitors traffic flow on a network and exports a copy to a NetFlow collector device. An exporter works in the same way as a network passive monitor or a network sniffer, by placing its listening interface in promiscuous mode. A NetFlow Exporter exports netflow  from devices located at points of convergence, much like an IDS sensor  [[1](https://code.google.com/p/renisac/wiki/NetFlow#References)].

The NetFlow Probe/Exporter will export netflow data to the NetFlow Collector in the form of NetFlow records using the User Datagram Protocol (UDP). For the export process to work properly, both destination IP address (NetFlow Collector) and the destination UDP port must be properly configured on the NetFlow Exporter. Otherwise, the export process will fail. NetFlow records are typically exported on the standard UDP port 2055. However, the port value can be altered to any desired non standard port. For the purpose of this guide, we used Free FlowTraq Flow Exporter as the NetFlow probe software. The device employing FlowTraq will act as our NetFlow Probe/Exporter. The Free FlowTraq Flow Exporter is available at: [FlowTraq](http://www.flowtraq.com/corporate/product/flow-exporter:). We chose this particular tool as our NetFlow Probe/Exporter because it supports both NetFlow v5 and v9 (NetFlow v5 supports IPv4 traffic only and NetFlow v9 supports both IPv4 and IPv6 traffic).

By default, the exporter only exports NetFlow records after a flow has been expired or is now inactive. You can configure your exporter to export after a particular time interval (eg. 10 minutes). NetFlow records are "timed out/expired" from an exporting device based on the state of the flow. Flows are classified as "active" or "inactive". A flow becomes inactive when an IP connection is terminated. The inactive timeout setting determines how long (in seconds) a flow is held in cache after it becomes inactive and exported to the collector  [[1](https://code.google.com/p/renisac/wiki/NetFlow#References)].

Depending on the needs of your network and the software used, you can have more than one NetFlow Probe/Exporter on your network. With the free FlowTraq exporter you can have up to 16 destinations (collectors). This has the added advantage of having backups or redundant exporters in-case one goes down.

# NetFlow Analysis #

NetFlow analysis is the process of inspecting, transforming, and modeling data with the goal of deriving useful information from NetFlow records. The useful information can then be used for decision making purposes. A network administrator can use this information to determine when a specific network transaction was made, in particular, a transaction resembling suspicious behavior such as hogging bandwidth. The administrator can then act up on it accordingly. NetFlow also provides the metering base for a key set of applications including network traffic accounting, usage-based network billing, user and application monitoring, profiling, network planning and analysis, outbound marketing, and data warehousing for both service provider and enterprise customers [[6](https://code.google.com/p/renisac/wiki/NetFlow#References)].

# References #

  1. http://www.first.org/global/practices/Netflow.pdf
  1. http://netflow.caligare.com/index.htm
  1. http://www.solarwinds.com/it-management-glossary/what-is-a-netflow-collector.aspx
  1. http://nfdump.sourceforge.net/
  1. http://netflow.caligare.com/netflow_v5.htm
  1. http://www.caligare.com/netflow/netflow.php
  1. http://en.wikipedia.org/wiki/NetFlow#Export_of_NetFlow_records