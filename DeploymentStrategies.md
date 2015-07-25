# Introduction #
This section describes the testbed used for constructing this guide as well as other possible NetFlow deployment strategies. In the following paragraphs, we identify three main potential NetFlow deployment scenarios. Other configurations are also possible as well. For instance, the NetFlow Collector could act as a relay which forwards NetFlow data to another collector. A NetFlow Collector could also represent a cluster of Collectors receiving NetFlow data. Routers also have the capability to cache NetFlow data. Alternatively, NetFlow data can be generated from raw packet dumps collected by a dedicated traffic recorder. This is useful for extracting session information from packets in situations which require historical traffic analysis. In this case, the traffic recorder could employ NetFlow export software or send packet dumps to a dedicated NetFlow aware device. The packet dumps can then be replayed into NetFlow export software or a NetFlow aware device could then generate NetFlow data to be sent to a Collector. The setup and configuration of a traffic capture system is addressed in another module [Part 3: Chapter 5](https://code.google.com/p/renisac/wiki/PacketCapture).

Ultimately, the deployment and implementation of NetFlow can vary based upon available resources as well as the needs of the network. We identify three main potential types of NetFlow deployments, which are  illustrated in Scenario A, B, and C below. **Please note! The topology in Scenario A illustrates the NetFlow deployment environment utilized in constructing this manual.** Although, our environment was implemented and designed according to Scenario A, this manual is also applicable to all other scenarios as well. With each scenario there exists different configuration steps and design considerations. **An additional goal of this section is to highlight the differences between each approach and why one might be considered over the other. Also, this section will point to the relevant configuration sections for each scenario.**

# Testbed #
## Scenario A: ##
### Probe/Exporter and Collector on remote Machines ###

![http://www.ren-isac.net/img/googlecode/1.2.2.1-Physical.jpg](http://www.ren-isac.net/img/googlecode/1.2.2.1-Physical.jpg)

In Scenario A, the NetFlow data export and collection is distributed between the Ubuntu Workstation and Server respectively. This NetFlow deployment utilizes a dedicated NetFlow probe as illustrated in Scenario A. The probe is a dedicated box which passively listens on an interface connected to a SPAN port. The switch is configured to replay traffic out the SPAN port for any switch port assigned with a mirror port role. The dedicated NetFlow probe passively listens for the incoming traffic by placing its interface in promiscuous mode. The probe gathers IP traffic statistics which later exports the data as NetFlow records. In order to simulate user web traffic, we deploy a series of webspiders on multiple VM's. As the webspiders crawl and generate traffic, the traffic is then mirrored and sent to the NetFlow probe through the switch SPAN port.

#### Advantages: ####
  * A dedicated NetFlow probe can be deployed more easily in different areas of the network than a router or switch.
  * Fully compatible with NetFlow v5 and v9.
  * Can more easily upgrade standalone probes than a NetFlow aware router or switch.
  * Has the ability to do more than just export NetFlow, e.g. deep level packet inspection, traffic capture etc.

#### Disadvantages: ####
  * Higher cost of ownership. This approach requires the purchase of dedicated probes or computers capable of processing substantial amount of network traffic. In order to have broad coverage of the network, the number of probes should also be factored into the cost. The number of additional links can also be costly. Finally, the maintenance costs of dedicated probes should also be considered. The same is also true for dedicated NetFlow Collectors.
  * Dedicated probes do not have the same view of the network as a router. When packets are processed, the router records information such as ingoing/outgoing interface. Additionally, a dedicated probe does not have access to the same routing information as a router would. Fields such as AS number or IP mask may be omitted from the NetFlow data.

### Configuration Guide ###
#### Collector Machine ####

##### Standalone NFDump Install: #####

  1. [NFDump](https://code.google.com/p/renisac/wiki/Collector_Machine#Install_NFDump) - **Update 06/16/2013** - Now includes instructions for sFlow aka sfcapd.
  1. [Justin Azoff's NetFlow Indexer](https://code.google.com/p/renisac/wiki/Collector_Machine#Install_Justin_Azoff's_NetFlow_Indexer) (NetFlow Only)

##### NFDump with NFSen Install: #####

  1. [NFDump](https://code.google.com/p/renisac/wiki/Collector_Machine#Install_NFDump) - **Update 06/16/2013** - Now includes instructions for sFlow aka sfcapd.
  1. [Justin Azoff's NetFlow Indexer](https://code.google.com/p/renisac/wiki/Collector_Machine#Install_Justin_Azoff's_NetFlow_Indexer) (**Optional**) (NetFlow Only)
  1. [NFSen](https://code.google.com/p/renisac/wiki/NFSen_Install#Install_NFSen)
    1. [Configuring Profiles, Channels, and Plugins](https://code.google.com/p/renisac/wiki/Traffic_Classification) (Optional)
    1. [Port Tracker Plugin](https://code.google.com/p/renisac/wiki/Traffic_Classification?ts=1374515787&updated=Traffic_Classification#Port_Tracker_Plugin) (Optional)
    1. [Install Events Plugin](https://code.google.com/p/renisac/wiki/Botnet_Install#Install_Events-Plugin) (Optional)
    1. [Install Botnet Plugin](https://code.google.com/p/renisac/wiki/Botnet_Install#Install_Botnet-Plugin) (Optional)
      * [The Emerging Threats Example](https://code.google.com/p/renisac/wiki/Botnet_Install#The_Emerging_Threats_Example) (Optional)
      * [Testing the Botnet Plugin](https://code.google.com/p/renisac/wiki/Botnet_Install#Testing_the_Botnet_Plugin) (Optional)

#### Probe/Exporter Machine ####
  1. [FlowTraq Free Flow Exporter](https://code.google.com/p/renisac/wiki/ProbeExporterMachine?ts=1370442342&updated=ProbeExporterMachine#Install_FlowTraq_Free_Flow_Exporter) (NetFlow Only)

#### Switch SPAN Configuration ####
Pick the configuration guide corresponding to your vendor. Also take a look at [SPAN vs TAP](https://code.google.com/p/renisac/wiki/SwitchConfiguration#SPAN_vs_TAP) to determine whether a SPAN is a suitable solution for your needs.

  * [HP ProCurve](https://code.google.com/p/renisac/wiki/SwitchConfiguration#HP_ProCurve)
  * [Cisco IOS](https://code.google.com/p/renisac/wiki/SwitchConfiguration#Cisco_IOS)
  * [Junos OS](https://code.google.com/p/renisac/wiki/SwitchConfiguration#Junos_OS)

#### Traffic Generator (Optional) ####
  1. [Virtual Box](https://code.google.com/p/renisac/wiki/Traffic_Generator#Install_Virtual_Box)
  1. [OpenWebSpider v0.7](https://code.google.com/p/renisac/wiki/Traffic_Generator#Install_OpenWebSpider_v0.7)

# Other Deployment Strategies #
## Scenario B: ##
### Collector/Probe/Exporter on the same Machine ###

![http://www.ren-isac.net/img/googlecode/1.2.3.1-Single_Physical.jpg](http://www.ren-isac.net/img/googlecode/1.2.3.1-Single_Physical.jpg)

In Scenario B, the NetFlow data export and collection capability is local to a single machine, in this instance, the Ubuntu Workstation. The Ubuntu Server could be utilized in this fashion as well. Just like in Scenario A, the dedicated NetFlow probe receives the mirrored traffic of the traffic generator through its interface connected to the switch SPAN port. The probe gathers statistics on the incoming mirrored frames. The probe then exports these statistics as NetFlow data. In this scenario, the !Netflow data is exported to the machines loopback address, therefore, exporting the data to itself. The collection software listens on the local interface for incoming NetFlow records and stores them to the local disk. This scenario has many of the same advantages and disadvantages as Scenario A.

#### Advantages: ####
  * A dedicated NetFlow probe can be deployed more easily in different areas of the network than a router or switch.
  * Fully compatible with NetFlow v5 and v9.
  * Can more easily upgrade standalone probes than a NetFlow aware router or switch.
  * Has the ability to do more than just export NetFlow, e.g. deep level packet inspection, traffic capture etc.

#### Disadvantages: ####
  * Just like in Scenario A, there still exists the higher cost of ownership. This approach requires the purchase of dedicated probes or computers capable of processing substantial amount of network traffic. Additionally, since the probe is also acting as a collector, this approach requires more storage capacity and computational resources. Like before, in order to have a broad coverage area of the network, the number of probes should also be factored into the cost. However, the number of additional links would be lower than that of Scenario A. Finally, the maintenance costs of dedicated probes should be factored into the total cost.
  * Like before, dedicated probes do not have the same view of the network as a router. When packets are processed, the router records information such as ingoing/outgoing interface. Additionally, a dedicated probe does not have access to the same routing information as a router would. Fields such as AS number or IP mask may be omitted from the NetFlow data.

### Configuration Guide ###
#### Collector/Probe/Exporter Machine ####

##### Standalone NFDump Install: #####

  1. [NFDump](https://code.google.com/p/renisac/wiki/Collector_Machine#Install_NFDump) - **Update 06/16/2013** - Now includes instructions for sFlow aka sfcapd.
  1. [Justin Azoff's NetFlow Indexer](https://code.google.com/p/renisac/wiki/Collector_Machine#Install_Justin_Azoff's_NetFlow_Indexer) (NetFlow Only)
  1. [FlowTraq Free Flow Exporter](https://code.google.com/p/renisac/wiki/ProbeExporterMachine?ts=1370442342&updated=ProbeExporterMachine#Install_FlowTraq_Free_Flow_Exporter) (NetFlow Only)

##### NFDump with NFSen Install: #####

  1. [FlowTraq Free Flow Exporter](https://code.google.com/p/renisac/wiki/ProbeExporterMachine?ts=1370442342&updated=ProbeExporterMachine#Install_FlowTraq_Free_Flow_Exporter) (NetFlow Only)
  1. [NFDump](https://code.google.com/p/renisac/wiki/Collector_Machine#Install_NFDump) - **Update 06/16/2013** - Now includes instructions for sFlow aka sfcapd.
  1. [Justin Azoff's NetFlow Indexer](https://code.google.com/p/renisac/wiki/Collector_Machine#Install_Justin_Azoff's_NetFlow_Indexer) (**Optional**) (NetFlow Only)
  1. [NFSen](https://code.google.com/p/renisac/wiki/NFSen_Install#Install_NFSen)
    1. [Configuring Profiles, Channels, and Plugins](https://code.google.com/p/renisac/wiki/Traffic_Classification) (Optional)
    1. [Port Tracker Plugin](https://code.google.com/p/renisac/wiki/Traffic_Classification?ts=1374515787&updated=Traffic_Classification#Port_Tracker_Plugin) (Optional)
    1. [Install Events Plugin](https://code.google.com/p/renisac/wiki/Botnet_Install#Install_Events-Plugin) (Optional)
    1. [Install Botnet Plugin](https://code.google.com/p/renisac/wiki/Botnet_Install#Install_Botnet-Plugin) (Optional)
      * [The Emerging Threats Example](https://code.google.com/p/renisac/wiki/Botnet_Install#The_Emerging_Threats_Example) (Optional)
      * [Testing the Botnet Plugin](https://code.google.com/p/renisac/wiki/Botnet_Install#Testing_the_Botnet_Plugin) (Optional)


#### Switch SPAN Configuration ####
Pick the configuration guide corresponding to your vendor. Also take a look at [SPAN vs TAP](https://code.google.com/p/renisac/wiki/SwitchConfiguration#SPAN_vs_TAP) to determine whether a SPAN is a suitable solution for your needs.

  * [HP ProCurve](https://code.google.com/p/renisac/wiki/SwitchConfiguration#HP_ProCurve)
  * [Cisco IOS](https://code.google.com/p/renisac/wiki/SwitchConfiguration#Cisco_IOS)
  * [Junos OS](https://code.google.com/p/renisac/wiki/SwitchConfiguration#Junos_OS)

#### Traffic Generator (Optional) ####
  1. [Virtual Box](https://code.google.com/p/renisac/wiki/Traffic_Generator#Install_Virtual_Box)
  1. [OpenWebSpider v0.7](https://code.google.com/p/renisac/wiki/Traffic_Generator#Install_OpenWebSpider_v0.7)

## Scenario C: ##
### Exporting Network Device/Remote Collecting Machine ###

![http://www.ren-isac.net/img/googlecode/1.2.3.2-SwitchExporter_Physical.jpg](http://www.ren-isac.net/img/googlecode/1.2.3.2-SwitchExporter_Physical.jpg)

In Scenario C, the need for a dedicated probe is negated through utilizing a NetFlow aware router or switch. This demonstrates the ability of modern networking hardware to act as a NetFlow Probe/Exporter. As the network device processes in transit datagrams, it gathers IP statistics and builds a report based on its observations. These statistics are then exported as NetFlow data to a dedicated NetFlow Collector.

#### Advantages: ####
  * Easier to setup NetFlow export on a router. Doesn't require knowledge of Linux.
  * Fully compatible with NetFlow v5 and v9. Supported by most routers.
  * Can utilize existing infrastructure if the hardware has already been purchased and deployed.
  * Routers have a different view of the network compared to a NetFlow probe on a dedicated Linux machine. This allows for additional flow information such as inbound/outbound interface and AS number.

#### Disadvantages: ####
  * There is still a higher cost of ownership if new NetFlow aware routing equipment must be purchased. However, the number of additional links would be lower than that of Scenario A due to utilizing existing infrastructure. This approach would still warrant the purchase of a dedicated NetFlow Collector. However, the number of additional links would be lower than the deployment strategy illustrated in Scenario A. Finally, the maintenance costs should also be factored in the total cost.
  * Routers exporting NetFlow cannot be redeployed as easily to different areas of the network. Dedicated probes can be placed where needed.
  * Requires a network engineer to configure the NetFlow data export.

### Configuration Guide ###
#### Collector Machine ####

##### Standalone NFDump Install: #####

  1. [NFDump](https://code.google.com/p/renisac/wiki/Collector_Machine#Install_NFDump) - **Update 06/16/2013** - Now includes instructions for sFlow aka sfcapd.
  1. [Justin Azoff's NetFlow Indexer](https://code.google.com/p/renisac/wiki/Collector_Machine#Install_Justin_Azoff's_NetFlow_Indexer) (NetFlow Only)

##### NFDump with NFSen Install: #####

  1. [NFDump](https://code.google.com/p/renisac/wiki/Collector_Machine#Install_NFDump) - **Update 06/16/2013** - Now includes instructions for sFlow aka sfcapd.
  1. [Justin Azoff's NetFlow Indexer](https://code.google.com/p/renisac/wiki/Collector_Machine#Install_Justin_Azoff's_NetFlow_Indexer) (**Optional**) (NetFlow Only)
  1. [NFSen](https://code.google.com/p/renisac/wiki/NFSen_Install#Install_NFSen)
    1. [Configuring Profiles, Channels, and Plugins](https://code.google.com/p/renisac/wiki/Traffic_Classification) (Optional)
    1. [Port Tracker Plugin](https://code.google.com/p/renisac/wiki/Traffic_Classification?ts=1374515787&updated=Traffic_Classification#Port_Tracker_Plugin) (Optional)
    1. [Install Events Plugin](https://code.google.com/p/renisac/wiki/Botnet_Install#Install_Events-Plugin) (Optional)
    1. [Install Botnet Plugin](https://code.google.com/p/renisac/wiki/Botnet_Install#Install_Botnet-Plugin) (Optional)
      * [The Emerging Threats Example](https://code.google.com/p/renisac/wiki/Botnet_Install#The_Emerging_Threats_Example) (Optional)
      * [Testing the Botnet Plugin](https://code.google.com/p/renisac/wiki/Botnet_Install#Testing_the_Botnet_Plugin) (Optional)


#### Switch/Router Flow Export Configuration ####
Pick the configuration guide corresponding to your vendor and desired protocol.

  * NetFlow
    * [Cisco](https://code.google.com/p/renisac/wiki/NetFlow_Config#Cisco)
    * [HP](https://code.google.com/p/renisac/wiki/NetFlow_Config#HP)
    * [Juniper](https://code.google.com/p/renisac/wiki/NetFlow_Config#Juniper)

  * sFlow
    * [Cisco](https://code.google.com/p/renisac/wiki/sFlow_Config#Cisco)
    * [HP](https://code.google.com/p/renisac/wiki/sFlow_Config#HP)
    * [Juniper](https://code.google.com/p/renisac/wiki/sFlow_Config#Juniper)

  * J-Flow
    * [Juniper](https://code.google.com/p/renisac/wiki/JFlow_Config#Juniper)

#### Traffic Generator (Optional) ####
  1. [Virtual Box](https://code.google.com/p/renisac/wiki/Traffic_Generator#Install_Virtual_Box)
  1. [OpenWebSpider v0.7](https://code.google.com/p/renisac/wiki/Traffic_Generator#Install_OpenWebSpider_v0.7)

<br>
<h3>Changelog:</h3>
<blockquote>- Update 07/23/2013 - Added the configuration guides for NFSen to each scenario A,B, and C.</blockquote>

<blockquote>- Update 06/16/2013 - NFDump configuration guide now includes instructions for sFlow aka sfcapd.