# Introduction #

# J-Flow #

## Juniper ##

J-Flow is Juniper's proprietary protocol that works with Juniper devices to monitor flows within the network. With  J-Flow, network devices such as routers, firewalls, and switches collect flow data and export that information to flow collectors [[1](https://code.google.com/p/renisac/wiki/SettingUpNetFlowDataExport?ts=1370808727&updated=SettingUpNetFlowDataExport#Juniper_Documentation)].  J-Flow has three version J-Flow v5, v8 and v9. For the purpose of our study we will only document enabling v9 because it supports IPv6.


## Configuring J-Flow and J-Flow Data Export ##

**Step 1:**

Configure J-Flow v9 template. (As of now, only IPv4 template is supported.)

> ```

services {
flow-monitoring {
version9 {
template <template name> {
ipv4-template;
}
}
}
} ```

**Step 2:**

To configure J-Flow to export to a specific collector machine. J-Flow supports up to eight flow collectors simultaneously.

> ```

sampling {
family inet {
output {
flow-server <Flow collector IP address> {
port <Flow collector port number>;
version9 {
template {
<J-Flow v9 Template>;
}
}
}
}
}
}
```

**Step 3:**

Configure inline J-Flow  so that sampling and the J-Flow service thread are implemented in the forwarding engine.

> ```

sampling {
family inet {
output {
inline-jflow {
source-address <Local IP address>;
}
}
}
} ```

**Step 4:**

Configure sampling rate and sampling run length.

> ```

sampling {
input {
rate <Sampling Rate>;
run-length <Sampling Run Length>;
}
}
}```


**Step 5:**

And finally, configure the sampling filter on an interface (or interfaces) in the direction where J-Flow service is required.

> ```

interfaces {
<Interface name> {
unit 0 {
family inet {
sampling {
< input | output >;
}
address <IP address>;
}
}
}
}```

<br>
<h4><- <a href='https://code.google.com/p/renisac/wiki/DeploymentStrategies?ts=1370391879&updated=DeploymentStrategies'>Return to Deployment Strategies</a></h4>

<h1>References</h1>

<h2>Juniper Documentation</h2>

<ol><li><a href='http://www.juniper.net/us/en/local/pdf/app-notes/3500204-en.pdf'>http://www.juniper.net/us/en/local/pdf/app-notes/3500204-en.pdf</a>
</li><li><a href='http://www.mail-archive.com/intermapper-talk@list.dartware.com/msg04721.html'>http://www.mail-archive.com/intermapper-talk@list.dartware.com/msg04721.html</a>