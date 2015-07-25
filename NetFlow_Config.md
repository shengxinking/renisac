# Introduction #

# Cisco #

Cisco is moving away from NetFlow to Flexible NetFlow. Flexible NetFlow is an upgraded NetFlow, to be more specific; it is an extension of NetFlow v9. In this section we document how to configure the new Flexible NetFlow. For documentation on how to configure the old version of NetFlow please visit the [Configuring Old NetFlow](https://code.google.com/p/renisac/wiki/ConfiguringOldNetFlow#ConfiguringOldNetFlow) section.

Before you enable NetFlow on your router device, you must first:

  * Configure the router for IP routing
  * Ensure that one of the following is enabled on your router as well as the interfaces that you want to configure NetFlow on: Cisco Express Forwarding (CEF), distributed CEF, or fast switching.
  * Understand the resources required on your router because NetFlow consumes additional memory and CPU resources [[1](https://code.google.com/p/renisac/wiki/SettingUpNetFlowDataExport?ts=1370808727&updated=SettingUpNetFlowDataExport#Cisco_Documentation)].

As a reminder, we emphasize that NetFlow consumes additional memory and CPU resources. If you are worried about available resources, you can preset your cache size to a specific size, thus, only keeping a specific number of entries. Please refer to your router's manual for more information.

## Configuring Flexible NetFlow and Flexible NetFlow Data Export ##

In this documentation we assume that you are familiar with configuring cisco routers thus you know what the different modes mean. To setup the Exporter, please type the following commands on the router commandline interface.

**Step 1:**

Enable privileged EXEC mode.

> ```
Router> enable```


**Step 2:**

Enter global configuration mode

> ```
Router# configure terminal```

**Step 3:**

Create a flow exporter and enter Flexible NetFlow flow exporter configuration mode.

> ```
flow exporter exporter-name```

Example:

> ```
Router(config)# flow exporter EXPORTER-1```

**Step 4:**

(Optional) Creates a description for the flow exporter.
> ```
description description```

Example:
> ```
Router(config-flow-exporter)# description Exports to datacenter```

**Step 5:**

Specify the hostname or IP address of the system to which the exporter sends data.

> ```
destination {hostname | ip-address} [vrf vrf-name]```

Example:

> ```
Router(config-flow-exporter)# destination 172.16.10.2```

**Step 6:**

Configure UDP as the transport protocol and specify the UDP port on which the destination system is listening for exported Flexible NetFlow traffic.

> ```
transport udp udp-port```

Example:

> ```
Router(config-flow-exporter)# transport udp 65```

**Step 7:**

Exit Flexible NetFlow flow exporter configuration mode and return to global configuration mode.

> ```
exit```

Example:

> ```
Router(config-flow-exporter)# exit```

**Step 8:**

Enter Flexible NetFlow flow monitor configuration mode for the flow monitor that you created previously.

> ```
exporter exporter-name```

Example:

> ```
Router(config-flow-monitor)# exporter EXPORTER-1```

**Step 9:**

Exit Flexible NetFlow flow monitor configuration mode and return to privileged EXEC mode.

> ```
end```

Example:

> ```
Router(config-flow-monitor)# end```



## Verifying That Flexible NetFlow is Working ##

Please enter the following commands on your router commandline interface to make sure that NetFlow is working properly:

**Step 1:**


Display the current status of the specified flow exporter.

> ```
show flow exporter exporter-name```

Example:

> ```
Router# show flow exporter FLOW_EXPORTER-1```

**Step 2:**

Display the configuration of the specified flow exporter.

> ```
show running-config flow exporter exporter-name```

Example:

> ```
Router# show running-config flow exporter FLOW_EXPORTER-1```


## Configuring a Flow Monitor for IPv4/IPv6 Traffic Using the Flexible NetFlow ##

To configure a flow monitor for IPV4 traffic using the Flexible NetFlow:

**Step 1:**
Enable privileged EXEC mode.

> ```
Router> enable```


**Step 2:**

Enter global configuration mode

> ```
Router# configure terminal```

**Step 3:**

Create a flow monitor and enter Flexible NetFlow flow monitor configuration mode.

> ```
flow monitor monitor-name```

Example:

> ```
Router(config)# flow monitor FLOW-MONITOR-1```

**Step 4:**

(Optional) Creates a description for the flow monitor.

> ```
description description```

Example:
> ```
 Router(config-flow-monitor)# description Used for monitoring IPv4 traffic```

**Step 5:**

Specify the record for the flow monitor.

> ```
record netflow {ipv4 | ipv6} original-input```

Example:

> ```
Router(config-flow-monitor)# record netflow ipv4 original-input```

**Step 6:**

Exit Flexible NetFlow flow monitor configuration mode and return to privileged EXEC mode

> ```
end```

Example:

> ```
Router(config-flow-monitor)# end```

**Step 7:**

Optional) Display the status and statistics for a Flexible NetFlow flow monitor.

> ```
show flow monitor [[name] monitor-name [cache [format {csv | record | table}]][statistics]]```

Example:

> ```
Router# show flow monitor FLOW-MONITOR-2 cache```

**Step 8:**

(Optional) Display the configuration of the specified flow monitor.

> ```
show running-config flow monitor monitor-name```

Example:

> ```
Router# show flow monitor FLOW_MONITOR-1```



## Assigning an IPv4 Flow Monitor to an Interface ##

To activate an IPv4 flow monitor, use the following steps. You can not activate an IPv4 flow monitor without assigning it to an interface first. The following steps will assign and activate the interface for flow monitoring.


**Step 1:**
Enable privileged EXEC mode.

> ```
Router> enable```


**Step 2:**

Enter global configuration mode

> ```
Router# configure terminal```

**Step 3:**

Specify an interface and then enter interface configuration mode

> ```
interface type number```

Example:

> ```
Router(config)# interface ethernet 0/0```

**Step 4:**

Activate the flow monitor that you created previously by assigning it to the interface to analyze traffic.

> ```
ip flow monitor monitor-name input```

Example:
> ```
Router(config-if)# ip flow monitor FLOW-MONITOR-1 input```

**Step 5:**

Exit interface configuration mode and return to privileged EXEC mode.

> ```
end```

Example:

> ```
Router(config-if)# end```

**Step 6:**

Display the status of Flexible NetFlow (enabled or disabled) on the specified interface.

> ```
show flow interface type number```

Example:

> ```
Router# show flow interface ethernet 0/0```

**Step 7:**

Display the status, statistics, and flow data in the cache for the specified flow monitor.

> ```
show flow monitor name monitor-name cache format record```

Example:

> ```
Router# show flow monitor name FLOW_MONITOR-1 cache format record```



## Assigning an IPv6 Flow Monitor to an Interface ##

To activate an IPv6 flow monitor, use the following steps.

**Step 1:**
Enable privileged EXEC mode.

> ```
Router> enable```


**Step 2:**

Enter global configuration mode

> ```
Router# configure terminal```

**Step 3:**

Specify an interface and then enter interface configuration mode

> ```
interface type number```

Example:

> ```
Router(config)# interface ethernet 0/0```

**Step 4:**

Activate the flow monitor that you created previously by assigning it to the interface to analyze traffic.

> ```
ipv6 flow monitor monitor-name input```

Example:
> ```
Router(config-if)# ipv6 flow monitor FLOW-MONITOR-2 input```

**Step 5:**

Exit interface configuration mode and return to privileged EXEC mode.

> ```
end```

Example:

> ```
Router(config-if)# end```

**Step 6:**

Display the status of Flexible NetFlow (enabled or disabled) on the specified interface.

> ```
show flow interface type number```

Example:

> ```
Router# show flow interface ethernet 0/0```

**Step 7:**

Display the status, statistics, and flow data in the cache for the specified flow monitor.

> ```
show flow monitor name monitor-name cache format record```

Example:

> ```
Router# show flow monitor name FLOW_MONITOR-1 cache format record```



All the commands and most of the information above on Cisco products is courtesy of Cisco. We have included links to Cisco documentation in our references section.

<br>
<h4><- <a href='https://code.google.com/p/renisac/wiki/DeploymentStrategies?ts=1370391879&updated=DeploymentStrategies'>Return to Deployment Strategies</a></h4>

<h1>HP</h1>

In this section we show how to configure an Hp device (Hp 9300 series) for NetFlow v5. To configure NetFlow:<br>
<br>
<ul><li>Enable individual interfaces for Flow Switching. Flows are collected and exported only for the interfaces on which you enable Flow Switching.</li></ul>

<ul><li>Enable NetFlow globally.</li></ul>

<ul><li>Specify collector information. The collector is the device to which you are exporting the NetFlow data. You can use up to 10 collectors.</li></ul>

<b>Step 1:</b>

Enable Flow Switching on an interface.<br>
<br>
<blockquote><pre><code>HP9300(config)# interface ethernet 1/1</code></pre>
<pre><code>HP9300(config-if-1/1)# ip route-cache flow</code></pre>
<pre><code>HP9300(config-if-1/1)# exit</code></pre></blockquote>

<b>Step 2:</b>

Enable NetFlow at the global CONFIG level of the CLI.<br>
<br>
<blockquote><pre><code>HP9300(config)# ip flow-export enable</code></pre></blockquote>

<b>Step 3:</b>

Specify a collector, at the global CONFIG level of the CLI.<br>
<br>
<blockquote><pre><code>[no] ip flow-export destination &lt;ip-addr&gt; &lt;udp-portnum&gt; [&lt;collector-id&gt;]</code></pre></blockquote>

Example:<br>
<blockquote><pre><code>HP9300(config)# ip flow-export destination 10.10.10.1 8080 1</code></pre></blockquote>

<b>Step 4:</b>

Reload the software to place the change into effect.<br>
<br>
<blockquote><pre><code>HP9300(config)# exit </code></pre></blockquote>

<blockquote><pre><code>HP9300# reload</code></pre></blockquote>

<b>Step 5:</b>

Specify the source interface.<br>
<br>
<blockquote><pre><code>HP9300(config)# ip flow-export source ethernet 1/1</code></pre></blockquote>

This command configures port 1/1 to be the source interface for NetFlow packets. Since the command does not specify a collector ID, NetFlow exports the flows to collector 1.<br>
<br>
To specify the collector ID:<br>
<br>
<blockquote><pre><code></code></pre></blockquote>

<blockquote><pre><code>HP9300(config)# ip flow-export source ethernet 1/1 2</code></pre></blockquote>

This command uses port 1/1 as the source for flows exported to collector 2<br>
<br>
<b>Step 6:</b>

To display NetFlow information.<br>
<br>
<pre><code>HP9300(config)# show ip flow export</code></pre>


All the commands and most of the information above on Hp products is courtesy of Hp. We have included links to Hp documentation in our Hp references section.<br>
<br>
<br>
<h4><- <a href='https://code.google.com/p/renisac/wiki/DeploymentStrategies?ts=1370391879&updated=DeploymentStrategies'>Return to Deployment Strategies</a></h4>

<h1>Juniper</h1>

Juniper supports netflow/cflowd record exports to an analyzer/collector by the routing engine sampling packet headers and aggregating them into flows. Packet sampling is done by defining a firewall filter to accept and sample all traffic, applying that rule to the interface and then configuring the sampling forwarding option.<br>
<br>
<b>Step 1:</b>

Replace 192.168.1.1/24 with the IP address you wish to "show up" as being the<br>
source of the flows. Replace 192.168.1.100 with the IP address of the collector where NetFlow analysis will be done.<br>
<br>
<blockquote><pre><code>interfaces {<br>
ge-0/1/0 {<br>
unit 0 {<br>
family inet {<br>
filter {<br>
input all;<br>
output all;<br>
}<br>
address 192.168.1.1/24;<br>
}<br>
}<br>
}<br>
}</code></pre></blockquote>

<blockquote><pre><code>firewall {<br>
filter all {<br>
term all {<br>
then {<br>
sample;<br>
accept;<br>
}<br>
}<br>
}<br>
}</code></pre></blockquote>

<blockquote><pre><code>forwarding-options {<br>
sampling {<br>
input {<br>
family inet {<br>
rate 100;<br>
}<br>
}<br>
output {<br>
cflowd 192.168.1.100 {<br>
port 9996;<br>
version 5;<br>
}<br>
}<br>
}<br>
}</code></pre></blockquote>

All the commands and most of the information above on Juniper products is courtesy of Juniper and an online Blogger by the name "Juniperdude". We have included links to Juniper and Juniperdude's documentation in our Juniper references section.<br>
<br>
<br>
<h4><- <a href='https://code.google.com/p/renisac/wiki/DeploymentStrategies?ts=1370391879&updated=DeploymentStrategies'>Return to Deployment Strategies</a></h4>


<h1>References</h1>

<h2>Cisco Documentation</h2>

<ol><li><a href='http://www.cisco.com/en/US/docs/ios-xml/ios/netflow/configuration/12-4t/get-start-cfg-nflow.pdf'>http://www.cisco.com/en/US/docs/ios-xml/ios/netflow/configuration/12-4t/get-start-cfg-nflow.pdf</a>
</li><li><a href='http://www.cisco.com/en/US/docs/ios/fnetflow/configuration/guide/get_start_cfg_fnflow.html#wp1056535'>http://www.cisco.com/en/US/docs/ios/fnetflow/configuration/guide/get_start_cfg_fnflow.html#wp1056535</a></li></ol>

<h2>HP Documentation</h2>

<ol><li><a href='http://h20000.www2.hp.com/bc/docs/support/SupportManual/c02565639/c02565639.pdf'>http://h20000.www2.hp.com/bc/docs/support/SupportManual/c02565639/c02565639.pdf</a></li></ol>

<h2>Juniper Documentation</h2>

<ol><li><a href='http://netflow.caligare.com/configuration_juniper.htm'>http://netflow.caligare.com/configuration_juniper.htm</a>
</li><li><a href='http://www.mail-archive.com/intermapper-talk@list.dartware.com/msg04721.html'>http://www.mail-archive.com/intermapper-talk@list.dartware.com/msg04721.html</a>