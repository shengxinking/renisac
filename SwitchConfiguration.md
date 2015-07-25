# Introduction #

This section discusses the difference between a SPAN and network TAP. We also cover the basic configuration of SPAN (mirroring) on HP, Cisco, and Juniper equipment. As suggested, this section offers the basic configuration commands to configure mirroring. This section is not meant to be a detailed tutorial but instead to point one in the right direction for configuring a basic SPAN. We have not tested these commands in our lab (except for the HP ProCurve). It may be necessary to review the [References](https://code.google.com/p/renisac/wiki/SwitchConfiguration?ts=1370808727&updated=SwitchConfiguration#References) section. Documentation for advanced SPAN (mirroring) configuration can also be found in the  [References](https://code.google.com/p/renisac/wiki/SwitchConfiguration?ts=1370808727&updated=SwitchConfiguration#References) section.

# SPAN vs TAP #

The term SPAN stands for Switch Port for Analysis but is also referred to by some as port mirroring. SPAN describes the ability to copy traffic traversing a switch from any or all ports to an unused port. A Test Access Point (TAP) is a passive splitting mechanism installed between a Device of Interest (DOI) and its directly connected network. TAPs are useful for networks with moderate to high bandwidth utilization. TAPs also see everything including physical layer errors and VLAN tags. However, one of the major tradeoffs are cost. With SPAN one can simply just enable the feature in switches deployed throughout the network. SPAN is useful for when network bandwidth utilization is low to moderate or in situations where packet loss can be tolerated. For a detailed discussion on SPAN and TAP, refer to the SPAN vs TAP Articles in the [References](https://code.google.com/p/renisac/wiki/SwitchConfiguration?ts=1370808727&updated=SwitchConfiguration#References) section.

### Features of SPAN/TAP ###

<table border='2'>
<tr>
<td><b>SPAN</b></td>
<td><b>TAP</b></td>
</tr>
<tr>
<td>• RX & TX copied into one signal</td>
<td>• RX & TX signal delivered on separate ports</td>
</tr>
<tr>
<td>• Hardware and media errors are dropped</td>
<td>• Captures everything on the wire, including MAC and media errors</td>
</tr>
<tr>
<td>• If utilizations exceeds the SPAN link compacity, packets are dropped</td>
<td>• Guarantees complete capture even when the network is 100 percent saturated</td>
</tr>
</table>
Source [[1](https://code.google.com/p/renisac/wiki/SwitchConfiguration?ts=1371409774&updated=SwitchConfiguration#SPAN_vs_TAP_Articles)]

Another article [[2](https://code.google.com/p/renisac/wiki/SwitchConfiguration?ts=1371409774&updated=SwitchConfiguration#SPAN_vs_TAP_Articles)]  contrasts the advantages of TAPs over SPAN ports. The summarized findings are below.

<table border='2'>
<tr><td><b>Advantages of TAP over SPAN</b></td></tr>
<tr><td>• TAPs do not alter the time relationships of frames – spacing<br>
and response times especially important with VoIP and Triple<br>
Play analysis including FDX analysis.</td></tr>
<tr><td>• TAPs do not introduce any additional jitter or distortion which is important in VoIP / Video analysis.</td></tr>
<tr><td>• VLAN tags are not normally passed through the SPAN port<br>
so this can lead to false issues detected and difficulty in<br>
finding VLAN issues.</td></tr>
<tr><td>• TAPs do not groom traffic nor filter out physical layer<br>
errored packets</td></tr>
<tr><td>• Short or large frames are not filtered</td></tr>
<tr><td>• Bad CRC frames are not filtered</td></tr>
<tr><td>• TAPs do not drop packets regardless of the bandwidth</td></tr>
<tr><td>• TAPs are not addressable network devices and therefore<br>
cannot be hacked</td></tr>
<tr><td>• TAPs have no setups or command line issues so getting all<br>
the traffic is assured and saves users time.</td></tr>
<tr><td>• TAPs are completely passive and do not cause any distortion<br>
even on FDX and full bandwidth networks. They are also<br>
fault tolerant.</td></tr>
<tr><td>• TAPs do not care if the traffic is IPv4 or IPv6, it passes<br>
all traffic through.</td></tr></table>
Source [[2](https://code.google.com/p/renisac/wiki/SwitchConfiguration?ts=1371409774&updated=SwitchConfiguration#SPAN_vs_TAP_Articles)]

<table border='2'>
<tr><td><b>Disadvantages of SPAN</b></td></tr>
<tr><td>• Spanning or mirroring changes the timing<br>
of the frame interaction.</td></tr>
<tr><td>• The spanning algorithm is not designed to be<br>
the primary focus of the device like switching or routing, so<br>
if replicating a frame becomes an issue, the hardware will<br>
temporally drop the SPAN process.</td></tr>
<tr><td>• If the speed of the SPAN port becomes over loaded,<br>
frames are dropped.</td></tr>
<tr><td>• Proper spanning requires that a network engineer<br>
configure the switches properly. This takes away from the<br>
more important tasks that network engineers have. Many times<br>
configurations can become a political issue (constantly creating<br>
contention between the IT team, the security team and the<br>
compliance team).</td></tr>
<tr><td>• SPAN port drops all packets that are corrupt or those<br>
that are below the minimum size, so all frames are not passed.<br>
When these events occur no notification is sent to the user, so<br>
there is no guarantee that one will get all the traffic required<br>
for proper analysis.</td></tr></table>
Source [[2](https://code.google.com/p/renisac/wiki/SwitchConfiguration?ts=1371409774&updated=SwitchConfiguration#SPAN_vs_TAP_Articles)]

## Additional Note About Using SPAN ports ##

When using a SPAN port, it may be necessary to deploy the dedicated NetFlow probe with a dual homed interface.SPAN ports are not typically used  for regular communication unless explicitly configured.**This ability is usually disallowed in order to prevent the backflow of traffic into the network.**

It is important to understand the capabilities and limitations of your switch. Cisco offers a solution to this problem by appending the 'learning' keyword at the end of the following configuration command:

> ```
 Switch(config)#monitor session 1 destination interface GigabitEthernet 0/1 ingress vlan 30 learning ```

The interface connected to the SPAN port on the NetFlow probe should also be set to promiscuous mode so it may passively listen for the incoming mirrored traffic. Therefore, it may be necessary to setup a second interface for NetFlow data export and Internet connectivity.


# HP Procurve #

## Local Mirroring ##

**Step 1:** Configure a mirror (the port destination for mirrored traffic to be sent to):

> ```
 HP-ProCurve(config)# mirror 1 port eth b24 ```

**Step 2:** Configure the ports to be monitored:

> ```
 HP-ProCurve(config)# interface Ethernet b1-b23```
> ```
 HP-ProCurve(eth-B1-B23)# monitor all both mirror 1```

## Remote Mirroring ##

Obtained from HP Documentation  [[2](https://code.google.com/p/renisac/wiki/SwitchConfiguration?ts=1370808727&updated=SwitchConfiguration#HP_Documentation)]

**Step 1:** At the end switch configure a mirror endpoint

> ```
HP-ProCurve(config)# mirror endpoint ip <src-ip-add> <src-udp-port> <dst-ip-add> port <port#>```

> Example:

> ```
HP-ProCurve(config)# mirror endpoint ip 10.1.10.1 1000 10.1.10.2 port 3```

**Step 2:** Activate the mirror at the source switch

> ```
HP-ProCurve(config)# mirror <1-4> [name <name>] remote ip <src-ip-add> <srcudp-port> <dst-ip-add>```

> Example:

> ```
HP-ProCurve(config)# mirror 1 remote ip 10.1.10.1 1000 10.1.10.2```

<br>
<h4><- <a href='https://code.google.com/p/renisac/wiki/DeploymentStrategies?ts=1370391879&updated=DeploymentStrategies'>Return to Deployment Strategies</a></h4>

<h1>Cisco IOS</h1>

Detailed explanation of configuration commands can be found in Cisco Documentation reference <a href='https://code.google.com/p/renisac/wiki/SwitchConfiguration?ts=1370808727&updated=SwitchConfiguration#Cisco_Documentation'>[1</a>] and<br>
<a href='https://code.google.com/p/renisac/wiki/SwitchConfiguration?ts=1370808727&updated=SwitchConfiguration#Cisco_Documentation'>[2</a>]<br>
<h2>Local Mirroring</h2>

<b>Step 1:</b> Clear SPAN configuration for a session (if it exists):<br>
<br>
<blockquote><pre><code>Switch(config)# no monitor session 1</code></pre></blockquote>

<b>Step 2:</b> Define a port to be monitored:<br>
<br>
<blockquote><pre><code>Switch(config)# monitor session 1 source interface fastethernet0/1 both</code></pre></blockquote>

<b>Step 3:</b> Define a destination SPAN port:<br>
<br>
<blockquote><pre><code>Switch(config)# monitor session 1 destination interface fastethernet0/10 encapsulation dot1q </code></pre></blockquote>

<b>Step 4:</b> Go back to Privileged Exec mode and save your config:<br>
<br>
<blockquote><pre><code>Switch(config)# end</code></pre>
<pre><code>Switch# copy running-config startup-config</code></pre></blockquote>

<h2>Remote Mirroring</h2>

Obtained from Cisco Documentation  <a href='https://code.google.com/p/renisac/wiki/SwitchConfiguration?ts=1370808727&updated=SwitchConfiguration#Cisco_Documentation'>[2</a>]<br>
<br>
<h3>Source Switch</h3>

<b>Step 1:</b> Clear SPAN configuration for a session (if it exists):<br>
<br>
<blockquote><pre><code>Switch(config)# no monitor session 1</code></pre></blockquote>

<b>Step 2:</b> Define a port to be monitored<br>
<br>
<blockquote><pre><code>Switch(config)# monitor session 1 source interface fastEthernet0/10 tx</code></pre>
<pre><code>Switch(config)# monitor session 1 source interface fastEthernet0/2 rx</code></pre>
<pre><code>Switch(config)# monitor session 1 source interface fastEthernet0/3 rx</code></pre>
<pre><code>Switch(config)# monitor session 1 source interface port-channel 102 rx</code></pre></blockquote>

<b>Step 3:</b> Configure destination RSPAN VLAN and reflector port<br>
<br>
<blockquote><pre><code>Switch(config)# monitor session 1 destination remote vlan 901 reflector-port fastEthernet0/1</code></pre></blockquote>

<b>Step 4:</b> Go back to Privileged Exec mode and save your config:<br>
<br>
<blockquote><pre><code>Switch(config)# end</code></pre>
<pre><code>Switch# copy running-config startup-config</code></pre></blockquote>

<h3>Destination Switch</h3>

<b>Step 1:</b> Configure VLAN 901 as the remote source VLAN for monitor session 1<br>
<br>
<blockquote><pre><code>Switch(config)# monitor session 1 source remote vlan 901</code></pre></blockquote>

<b>Step 2:</b> Configure a destination SPAN port<br>
<br>
<blockquote><pre><code> Switch(config)# monitor session 1 destination interface fastEthernet0/5</code></pre></blockquote>

<b>Step 3:</b> Go back to Privileged Exec mode and save your config:<br>
<br>
<blockquote><pre><code>Switch(config)# end</code></pre>
<pre><code>Switch# copy running-config startup-config</code></pre></blockquote>

<br>
<h4><- <a href='https://code.google.com/p/renisac/wiki/DeploymentStrategies?ts=1370391879&updated=DeploymentStrategies'>Return to Deployment Strategies</a></h4>

<h1>Junos OS</h1>

<h2>Local Mirroring</h2>

<b>Step 1:</b> Include <b>port-mirroring</b> tag at the <b>edit forwarding-options</b> hierarchy level:<br>
<br>
Obtained from Juniper Documentation  <a href='https://code.google.com/p/renisac/wiki/SwitchConfiguration?ts=1370808727&updated=SwitchConfiguration#Juniper_Documentation'>[1</a>]<br>
<br>
<pre><code><br>
[edit forwarding-options]<br>
<br>
port-mirroring {<br>
family (ccc | inet | inet6 | vpls) {<br>
output {<br>
interface interface-name {<br>
next-hop address;<br>
}<br>
no-filter-check;<br>
}<br>
input {<br>
maximum-packet-length bytes;<br>
rate number;<br>
run-length number;<br>
}<br>
}<br>
}</code></pre>

<h2>Remote Mirroring</h2>

Refer to Juniper Documentation <a href='https://code.google.com/p/renisac/wiki/SwitchConfiguration?ts=1370808727&updated=SwitchConfiguration#Juniper_Documentation'>[2</a>] for remote mirroring configuration.<br>
<br>
<br>
<h4><- <a href='https://code.google.com/p/renisac/wiki/DeploymentStrategies?ts=1370391879&updated=DeploymentStrategies'>Return to Deployment Strategies</a></h4>

<h1>References</h1>
<h3>SPAN vs TAP Articles</h3>
<ol><li><a href='http://www.networkinstruments.com/includes/popups/taps/tap-vs-span.php'>http://www.networkinstruments.com/includes/popups/taps/tap-vs-span.php</a>
</li><li><a href='http://www.gigamon.com/stuff/contentmgr/files/1/4f5cd4f7078839df7a1e6f2a8e09e9db/download/span_port_or_tap_web.pdf'>http://www.gigamon.com/stuff/contentmgr/files/1/4f5cd4f7078839df7a1e6f2a8e09e9db/download/span_port_or_tap_web.pdf</a></li></ol>

<h3>Cisco Documentation</h3>
<ol><li><a href='http://www.cisco.com/en/US/products/hw/switches/ps708/products_tech_note09186a008015c612.shtml'>http://www.cisco.com/en/US/products/hw/switches/ps708/products_tech_note09186a008015c612.shtml</a>
</li><li><a href='http://www.cisco.com/en/US/docs/switches/lan/catalyst3550/software/release/12.1_19_ea1/configuration/guide/swspan.html'>http://www.cisco.com/en/US/docs/switches/lan/catalyst3550/software/release/12.1_19_ea1/configuration/guide/swspan.html</a>
</li><li><a href='http://www.cisco.com/en/US/products/hw/switches/ps708/products_tech_note09186a008015c612.shtml'>http://www.cisco.com/en/US/products/hw/switches/ps708/products_tech_note09186a008015c612.shtml</a></li></ol>

<h3>HP Documentation</h3>
<ol><li><a href='http://bizsupport2.austin.hp.com/bc/docs/support/SupportManual/c02640590/c02640590.pdf'>http://bizsupport2.austin.hp.com/bc/docs/support/SupportManual/c02640590/c02640590.pdf</a>
</li><li><a href='http://www.terena.org/activities/campus-bp/pdf/gn3-na3-t4-cbpd111.pdf'>http://www.terena.org/activities/campus-bp/pdf/gn3-na3-t4-cbpd111.pdf</a>
</li><li><a href='http://www.hiddenone.net/hp-procurve/local-port-mirroring/'>http://www.hiddenone.net/hp-procurve/local-port-mirroring/</a>
</li><li><a href='http://www.sysadmintutorials.com/tutorials/hp/hp-procurve-advanced-cli-commands-reference/'>http://www.sysadmintutorials.com/tutorials/hp/hp-procurve-advanced-cli-commands-reference/</a></li></ol>

<h3>Juniper Documentation</h3>
<ol><li><a href='http://www.juniper.net/techpubs/en_US/junos10.1/topics/usage-guidelines/policy-configuring-port-mirroring.html'>http://www.juniper.net/techpubs/en_US/junos10.1/topics/usage-guidelines/policy-configuring-port-mirroring.html</a>
</li><li><a href='https://kb.juniper.net/InfoCenter/index?page=content&cmid=no&id=KB10878&cat=EX3200_1&actp=LIST'>https://kb.juniper.net/InfoCenter/index?page=content&amp;cmid=no&amp;id=KB10878&amp;cat=EX3200_1&amp;actp=LIST</a>