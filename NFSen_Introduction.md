# Introduction #

In this section, we cover the basic concepts and terminology required to work with NFSen. A more detailed primer is available at the  [NFSen Project](http://nfsen.sourceforge.net/) page.

NfSen is a graphical web-based front end for the nfdump netflow tools. NfSen gives you a pictorial view of nfdump data, thus making the analysis process so much easier. NfSen lets you specify as many data sources as your export software supports, basically with NfSen, you can display your netflow data from many sources. You can view your data in the form of Flows, Packets and Bytes allowing to get different information like how many flows you have, packets per second and the bit rate which is powerful information to have. You can also easily navigate through the netflow data, process the netflow data within the specified time span by simply moving the graph pointer to the specific time you want to start your analysis. NfSen allows you to create history as well as continuous profiles - the profiles are extremely powerful because you can specify what kind of traffic to capture just like a filter. If you are not satisfied or rather, if you want some functionality that is not supported by NfSen by default, NfSen allows you to write your own plug-ins to process and display the specific netflow data you want [[1](https://code.google.com/p/renisac/wiki/NFSen_Introduction?ts=1373641217&updated=NFSen_Introduction#References)].

## Alerts ##

What is an Alert in NfSen? An Alert in NfSen is basically a notification sent out when specific conditions are met. Alerts allow you to execute specific actions based on specific conditions. An alert is basically a filter applied to the 'live' profile, if the conditions specified in the filter are met this then  triggers an Alert. The Alert applies the alert actions on the profile/flow that triggered the alert...these action vary from doing nothing, to emailing someone i.e the Systems Admin or calling a plugin to automatically deal with the flow in question.


![http://www.ren-isac.net/img/googlecode/6.1-alerts.png](http://www.ren-isac.net/img/googlecode/6.1-alerts.png)<br>
<b>Figure 6.1 Alert</b>


<h2>Profiles</h2>

A profile refers to a view on the netflow data with specific input filters applied. A profile is defined by its name, type and one or more profile filters, which are any valid filters accepted by nfdump <a href='https://code.google.com/p/renisac/wiki/NFSen_Introduction?ts=1373641217&updated=NFSen_Introduction#References'>[2</a>].  NfSen provides two profiles; History and Continuous. A history profile can be generated form past netflow data and it starts and ends back in the past as result it remains static. It does not grow or expire. On the other hand,  a continuous profile is generated from incoming data. It may start in the past and is continually updated as new netflow data becomes available. Unlike the history profile, it grows dynamically and expires according to what is set. Old data expires after a given amount of time or when a certain profile size is reached <a href='https://code.google.com/p/renisac/wiki/NFSen_Introduction?ts=1373641217&updated=NFSen_Introduction#References'>[2</a>]. Both the History and Continuous profiles can have subprofiles within them - Shadow profile, a shadow profiles is a profile in which no netflow data is collected to disk, saving disk space. A shadow profile simply tapes into  data of profile 'live' and uses that profile's filters to view the data. For detailed information please visit the NfSen project page.<br>
<br>
Below is a diagram showing profiles in NfSen:<br>
<br>
<br>
<img src='http://www.ren-isac.net/img/googlecode/6.1-Details_1.png' />
<b>Figure 6.2 Profiles</b>

<h2>Groups</h2>

NfSen allows you to group your profiles into different groups for easy identification and monitoring. For instance you can group all your profiles that are generating netflow from one particular host into a group.<br>
The way NfSen represents groups on the web interface is by showing the groups in Bold text; the example below shows three groups, NetFlow_Collectors, NetFlow_Sources and Webspiders. The webspiders group contains all our web spiders.<br>
<br>
<img src='http://www.ren-isac.net/img/googlecode/6.1-webspider_group.png' /><br>
<b>Figure 6.3 WebSpiders Group</b>

<h2>Filter</h2>

You can filter your netflow data according to your needs with the help of the process form provided as a GUI on web front. The filter syntax is the same as that of nfdump, which is similar to the well known pcap library used by tcpdump. Here is an example of a filter that filters for Http traffic :<br>
<br>
Filter Syntax:<br>
<br>
<blockquote><pre><code>dst net 129.79.49.81/32 and src port 80 and proto tcp or src port 80 and proto udp or src port 443 and proto tcp or src port 443 and proto udp</code></pre></blockquote>


<h2>Plugin</h2>

You can extend the use/functionality of NfSen through the use of plugins. There are basically two types of plugins; backend and frontend plugins. The backend plugins are loaded into the nfsend background process, while nfsend is started. A backend plugin may provide several functions for adding different functionalities. These include periodic data processing, alerting conditions and alerting actions. The frontend plugins may displays visually any results of the backend processing. Backend plugins are Perl Modules, where as frontend plugins are PHP files. Both plugins may exchange relevant data over the standard nfsend.comm socket <a href='https://code.google.com/p/renisac/wiki/NFSen_Introduction?ts=1373641217&updated=NFSen_Introduction#References'>[3</a>].<br>
<br>
<img src='http://www.ren-isac.net/img/googlecode/6.1-plugin.png' /><br>
<b>Figure 6.4 Plugin</b>


<h1>The NfSen Advantage</h1>

Why use NfSen? There are other tools that one could use out there, why use NfSen? Well, here is why!<br>
<br>
<b>NfSen allows you to:</b>

<ul><li>Display your NetFlow data:<br>
You can display your data by Flows, Packets and Bytes using RRD (Round Robin Database) within a time interval. The RRD allows you to create time graphs, that display the trend over time of one or more flows. A time graph displays on the x axis the time and on the y axis the values of the variables; in this case flows.</li></ul>

<ul><li>Easily navigate through the netflow data:<br>
Since NfSen is a graphical web based front end for the nfdump, it makes simply takes nfdump data and presents in a format that is easily readable via the web interface. NetFlow data can be broken down and grouped according to whatever the user needs.</li></ul>


<ul><li>Process the netflow data within the specified time span:<br>
</li></ul><blockquote>Through the inbuilt RRD tools, NfSen creates time graphs of flows and one can easily navigate to time in history of the flow they want to scrutinize by simple move of the mouse.</blockquote>


<ul><li>Create history as well as continuous profiles:<br>
The inbuilt RRDatabase allows you to save NetFlow data as it is being collected for later investigation. As already noted, History profiles start and end back in the past and don't change - these profiles can be referenced to if an anomaly is detected in the future.</li></ul>


<ul><li>Set alerts, based on various conditions:<br>
The image below shows an alert created for a flow for an ISP; a 30min average value is set and if the flow in<br>
terms of bit/s falls below 90% this could indicate a routing problem. So if this happens an alert is triggered, alerting the Systems Admin or who ever is in charge of getting the problem resolved.</li></ul>

<img src='http://www.ren-isac.net/img/googlecode/6.1-ISP-alert.png' /><br>
<b>Figure 6.5 Alert</b>


<ul><li>Write your own plugins to process netflow data on a regular interval:<br>
NfSen functionalities can easily be expanded or you can simply employ a plugin to make NfSen do whatever you want it to do, all you have to do is either write your own plugin or find a plugin online that does what you to do. Its easy to find plugins online because NfSen has a quite a big community of users. here we show a plugin that we use to keep.....</li></ul>


<img src='http://www.ren-isac.net/img/googlecode/6.1-NfSen_Plugins.png' /><br>
<b>Figure 6.6 Events plugin</b>






<h2>References</h2>

<ol><li><a href='http://www.networkuptime.com/tools/netflow/nfsen.html'>http://www.networkuptime.com/tools/netflow/nfsen.html</a>
</li><li><a href='http://nfsen.sourceforge.net/#mozTocId623518'>http://nfsen.sourceforge.net/#mozTocId623518</a>
</li><li><a href='http://nfsen.sourceforge.net/PluginGuide/plugin-guide.html'>http://nfsen.sourceforge.net/PluginGuide/plugin-guide.html</a>