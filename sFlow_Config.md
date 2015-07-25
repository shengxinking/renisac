# Introduction #

sFlow is a technology similar to NetFlow and J-Flow and is used for monitoring network traffic in data networks containing switches and routers. It originated in HP's labs but is now maintained by an organization called sFlow.org.

# Cisco #

Cisco has added sFlow capabilities to the Nexus 3000 series switches, anything before the Nexus 3000 does not support sFlow. This section provides commands on how to configure sFlow on a Cisco switch.  [References [3](https://code.google.com/p/renisac/wiki/sFlow_Config#Cisco#)].

## Configuring sFlow and sFlow Data Export ##


**Step 1:**

You must enable the sFlow before you can configure sFlow on the switch. Enter global configuration mode.

> ```
switch# configure terminal```


**Step 2:**

Enable the sFlow feature.

> ```
switch(config)# feature sflow```


**Step 3:**

To save the changes. The following command saves persistently through reboots and restarts by copying the running configuration to the startup configuration.

> ```
switch(config)# copy running-config startup-config```


**Step 4:**

Now to Configure the Sampling Rate. Please repeat step 1 to enter into global configuration mode, then start the following command.

> ```
switch(config)# sflow sampling-rate 50000```

Note, a sampling-rate of 0 disables sampling.

**Step 5:**

To configure the Counter Poll Interval.

> ```
sflow counter-poll-interval poll-interval```

Example:

> ```
switch(config)# sflow counter-poll-interval 100```

**Step 6:**

To configure the Maximum Datagram Size;

> ```
sflow max-datagram-size datagram-size```

Example:
> ```
sflow max-datagram-size 2000```

**Step 7:**

To save the changes. The following command saves persistently through reboots and restarts by copying the running configuration to the startup configuration.

> ```
switch(config)# copy running-config startup-config```


**Step 8:**

Now to configure the sFlow Analyzer Address. Please repeat step 1 to enter into global configuration mode, then start the following command.

> ```
sflow collector-ip IP-address vrf-instance```

Example:

> ```
switch(config)# sflow collector-ip 192.0.2.5 vrf management```

**Step 9:**

Configuring the sFlow Analyzer Port

> ```
sflow collector-port collector-port```

Example:

> ```
switch(config)# sflow collector-port 7000```

**Step 10:**

To configure the sFlow Agent Address

> ```
sflow agent-ip ip-address```

Example:

> ```
switch(config)# sflow agent-ip 192.0.2.3```


**Step 11:**

Configuring the sFlow Sampling Data Source

> ```
sflow data-source interface [ethernet slot/port[-port] |port-channel channel-number]```

Example:

> ```
switch(config)# sflow data-source interface ethernet 1/5-12```

**Step 12:**

To save the changes. The following command saves persistently through reboots and restarts by copying the running configuration to the startup configuration.

> ```
switch(config)# copy running-config startup-config```

Example:
> ```
switch(config)# sflow data-source interface ethernet 1/5-12
switch(config)# copy running-config startup-config
[########################################] 100%
switch(config)#```


# HP #

## Configuring sFlow and sFlow Data Export ##

This section provides command syntax for configuring sFlow on a ProCurve switch.

**Step 1:**

Configure destination collectors. Three destinations (collectors) can be configured on each switch.

> ```
HP-ProCurve(config) # sFlow <1-3> destination <IP-addr> <udp-port-for-sFlow>```

Example:

> ```
HP-ProCurve(config)# sFlow 1 destination 10.3.108.36```

**Step 2:**

To view destination information

> ```
HP-ProCurve(config)# show sFlow <1-3> destination```

Example:

> ```
HP-ProCurve(config)# show sFlow 1 destination```

**Step 3:**

Activating sampling on a set of switch ports.

> ```
HP-ProCurve(config)# sFlow <1-3> sampling <ports-list> N```

Example:

> ```
HP-ProCurve(config)# sFlow 1 sampling all 500```


**Step 4:**

Activating polling on a set of switch ports.

> ```
HP-ProCurve(config)# sFlow <1-3> sampling <ports-list> P```

Where P is the interval in seconds between two polls of counters. P can vary between 0 (polling disabled) and
16777215.


**Step 5:**

To view sampling and polling statistics.

> ```
HP-ProCurve(config)# show sFlow 1 sampling```


All commands and information presented in this section are all courtesy of HP [[3](https://code.google.com/p/renisac/wiki/SettingUpNetFlowDataExport?ts=1370808727&updated=SettingUpNetFlowDataExport#Hp_Documentation)] . We have also included links in the References section to other HP resources that we feel will be helpful in configuring your HP device.


# Juniper #

## Configuring sFlow and sFlow Data Export ##

To configure sFlow technology:

**Step 1:**

Configure the IP address of the collector:

> ```
[edit protocols sflow]
user@switch# set collector <ip-address>```

Example:

> ```
[edit protocols sflow]
user@switch# set collector 10.204.32.46```


Note: You can configure a maximum of 4 collectors.

**Step 2:**

Configure the UDP port of the collector. The default UDP port assigned is 6343.

> ```
[edit protocols sflow]
user@switch# set collector udp-port <port-number>```

Example:

> ```
[edit protocols sflow]
user@switch# set collector udp-port 5600```

**Step 3:**

Enable sFlow technology on a specific interface:

> ```
[edit protocols sflow]
user@switch# set interfaces interface-name```

Example:

> ```
[edit protocols sflow]
user@switch# set interfaces ge-0/0/0.0```

Note: You cannot enable sFlow technology on a Layer 3 VLAN-tagged interface. Neither can you enable sFlow technology on a LAG interface. sFlow technology can be enabled on the member interfaces of the LAG.

**Step 4:**

Specify how often the sFlow agent polls the interface:

> ```
[edit protocols sflow]
user@switch# set polling-interval seconds```

Example:
> ```
[edit protocols sflow]
user@switch# set polling-interval 20```

Note: The polling interval can be specified as a global parameter also. Specify 0 if you do not want to poll the interface.

**Step 5:**

Specify the rate at which packets must be sampled:

> ```
[edit protocols sflow]
user@switch# set sample-rate number```

Example:
> ```
[edit protocols sflow]
user@switch# set sample-rate 1000```

<br>
<h4><- <a href='https://code.google.com/p/renisac/wiki/DeploymentStrategies?ts=1370391879&updated=DeploymentStrategies'>Return to Deployment Strategies</a></h4>

<h1>References</h1>

<h2>Cisco Documentation</h2>

<ol><li><a href='http://www.cisco.com/en/US/docs/switches/datacenter/nexus3000/sw/system_mgmt/503_U4_1/b_3k_System_Mgmt_Config_503_u4_1_chapter_010010.html'>http://www.cisco.com/en/US/docs/switches/datacenter/nexus3000/sw/system_mgmt/503_U4_1/b_3k_System_Mgmt_Config_503_u4_1_chapter_010010.html</a>
</li><li><a href='http://blog.sflow.com/2012/08/cisco-adds-sflow-support.html'>http://blog.sflow.com/2012/08/cisco-adds-sflow-support.html</a>
</li><li><a href='http://blog.sflow.com/2012/08/configuring-cisco-switches.html'>http://blog.sflow.com/2012/08/configuring-cisco-switches.html</a>
</li><li><a href='http://www.cisco.com/en/US/docs/switches/datacenter/nexus3000/sw/system_mgmt/503_U4_1/b_3k_System_Mgmt_Config_503_u4_1_chapter_010010.html'>http://www.cisco.com/en/US/docs/switches/datacenter/nexus3000/sw/system_mgmt/503_U4_1/b_3k_System_Mgmt_Config_503_u4_1_chapter_010010.html</a></li></ol>

<h2>HP Documentation</h2>

<ol><li><a href='http://www.draware.dk/fileadmin/SolarWinds/Guide/AN-S6_sFlow-PCM-traffic-mon-final-081108.pdf'>http://www.draware.dk/fileadmin/SolarWinds/Guide/AN-S6_sFlow-PCM-traffic-mon-final-081108.pdf</a>
</li><li><a href='http://www.plixer.com/blog/general/how-do-i-enable-sflow-on-my-hp-procurve-2800-series-switch/'>http://www.plixer.com/blog/general/how-do-i-enable-sflow-on-my-hp-procurve-2800-series-switch/</a></li></ol>

<h2>Juniper Documentation</h2>

<ol><li><a href='https://www.juniper.net/techpubs/en_US/junos9.3/topics/example/sflow-configuring-ex-series.html'>https://www.juniper.net/techpubs/en_US/junos9.3/topics/example/sflow-configuring-ex-series.html</a>
</li><li><a href='http://www.juniper.net/techpubs/en_US/junos9.4/topics/task/configuration/sflow-ex-series-cli.html'>http://www.juniper.net/techpubs/en_US/junos9.4/topics/task/configuration/sflow-ex-series-cli.html</a>