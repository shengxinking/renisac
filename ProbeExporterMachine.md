# Install FlowTraq Free Flow Exporter #


**Step 1:** Obtain the Flow Traq Free Flow Exporter .bin file:

> ```
$ wget http://www.flowtraq.com/downloads/flowexport/flowexport_linux_i386.bin ```

If the above command returns a 404 page not found, flow exporter can be obtained from the Flow Traq website.
http://www.flowtraq.com/corporate/product/flow-exporter

You must provide a name and email address. The download link will be sent to the specified email address.

**Step 2:**

Once downloaded, use chmod to add the owner execute privilege to the downloaded .bin file.

> ```
$ sudo chmod 755 flowexport_linux_i386.bin
$ sudo mv flowexport_linux_i386.bin /usr/local/bin/flowexport```

**Step 3:** Setup a configuration file for flow exporter.

Open vim or any text editor:

> ```
 $ sudo vi /etc/flowexport.conf ```

Add the following line and save the configuration file:
> ```
destination0 nf9 <ip-address to send netflow to> <desired destination port>```

The first parameter (destination0) defines a destination to send netflow records to. The next parameter (nf9) instructs flow exporter to send netflow records in the v9 format **(Netflow v9 must be specified for IPv6 support)**. Finally, the IP address and destination port parameter tells flow exporter who to send the netflow data and on what destination UDP port.

Note: The free flow exporter allows one to specify up to 16 destinations (Netflow collectors). _For [Scenario B](https://code.google.com/p/renisac/wiki/DeploymentStrategies?ts=1370391879&updated=DeploymentStrategies#Scenario_B:), make sure to specify the destination ip address to the loopback 127.0.0.1 address._

Additional, configuration options are available such as:

  * Using a pcap file instead of an interface
  * Minimum age time for flows to be exported
  * Maximum age time for anything to be exported
  * Thread count

**For more information on additional configuration options, please refer to the switches listed using the following command:**

> ```
$ flowexport -h```

**Step 4:** Run flow exporter using the configuration file verifying a successful install.

The following command can be used to start flow exporter. The additional options specify which local interface to put into promiscuous mode to listen for packet data. The last option specifies a configuration file to be used to export the packet data as netflow records to a collector.

> ```
$ sudo flowexport -i <interface to listen on> -c /etc/flowexport.conf```

**Wireshark or tcpdump can be used to verify whether flow records are actually being exported.** Create a wireshark filter udp.dstport == <port specified in flowexport.conf> or:

> ```
 # Replace <port_num> with destination UDP port defined in flowexport.conf
$ sudo tcpdump -vvvntttt -i <exit interface for NetFlow records> 'udp dst port <port_num>'```

The result will indicate "ICMP Unreachable" until the flow collector has been setup and the listening port has been opened.

**Step 5:** Add flow exporter to startup (optional)

Open /etc/rc.local in a text editor

> ```
$ sudo vi /etc/rc.local```

Add the following line anywhere before the exit condition (if one exists):

> ```
/usr/local/bin/flowexport -i <interface to listen on> -c /etc/flowexport.conf```



<br>
<h4><- <a href='https://code.google.com/p/renisac/wiki/DeploymentStrategies?ts=1370391879&updated=DeploymentStrategies'>Return to Deployment Strategies</a></h4>