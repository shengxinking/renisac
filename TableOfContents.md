# Navigation Panel #

[NetFlow Cookbookv1](https://code.google.com/p/renisac/wiki/Netflow_Cookbookv1)<br><br>
↳ <a href='https://code.google.com/p/renisac/wiki/DeploymentStrategies'>Quick Start NetFlow Manual</a>

<ul><li><b>Part 1:</b> Getting Started with NetFlow<br>
<ul><li><b>Chapter 1 Introduction:</b>
<ul><li><a href='https://code.google.com/p/renisac/wiki/NetFlow'>1.1 What is NetFlow?</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/NetFlow#Introduction'>1.1.1 Introduction</a>
</li><li><a href='https://code.google.com/p/renisac/wiki/NetFlow#NetFlow_v5_and_NetFlow_v9_Differences'>1.1.2 NetFlow v5 and NetFlow v9 Differences</a>
</li><li><a href='https://code.google.com/p/renisac/wiki/NetFlow#NetFlow_Collector'>1.1.3 NetFlow Collector</a>
</li><li><a href='https://code.google.com/p/renisac/wiki/NetFlow#NetFlow_Probe/Exporter'>1.1.4 NetFlow Probe/Exporter</a>
</li><li><a href='https://code.google.com/p/renisac/wiki/NetFlow#NetFlow_Analysis'>1.1.5 NetFlow Analysis</a>
</li></ul></li><li><a href='https://code.google.com/p/renisac/wiki/DeploymentStrategies'>1.2 Deployment Strategies</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/DeploymentStrategies#Introduction'>1.2.1 Introduction</a>
</li><li><a href='https://code.google.com/p/renisac/wiki/DeploymentStrategies#Testbed'>1.2.2 Testbed</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/DeploymentStrategies#Scenario_A:'>1.2.2.1 Scenario A</a>
</li></ul></li><li><a href='https://code.google.com/p/renisac/wiki/DeploymentStrategies#Other_Deployment_Strategies'>1.2.3 Other Deployment Strategies</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/DeploymentStrategies#Scenario_B:'>1.2.3.1 Scenario B</a>
</li><li><a href='https://code.google.com/p/renisac/wiki/DeploymentStrategies#Scenario_C:'>1.2.3.2 Scenario C</a>
</li></ul></li></ul></li><li><a href='https://code.google.com/p/renisac/wiki/System_Requirements'>1.3 Testbed System Requirements</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/System_Requirements#NetFlow_Collector'>1.3.1 NetFlow Collector</a>
</li><li><a href='https://code.google.com/p/renisac/wiki/System_Requirements#NetFlow_Probe/Exporter'>1.3.2 NetFlow Probe/Exporter</a>
</li><li><a href='https://code.google.com/p/renisac/wiki/System_Requirements#WebSpider/Traffic_Generator'>1.3.3 Webspider/Traffic Generator</a>
</li></ul></li></ul></li><li><b>Chapter 2 System Configuration:</b>
<ul><li><a href='https://code.google.com/p/renisac/wiki/Collector_Machine'>2.1 Collector Machine</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/Collector_Machine#Install_NFDump'>2.1.1 NFDump</a>
</li><li><a href="https://code.google.com/p/renisac/wiki/Collector_Machine#Install_Justin_Azoff's_NetFlow_Indexer">2.1.2 Justin Azoff's NetFlow Indexer</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/Collector_Machine#How_to_Use_the_NetFlow-Indexer_Tools'>2.1.2.1 How to Use the Netflow-Indexer Tools</a>
</li></ul></li></ul></li><li><a href='https://code.google.com/p/renisac/wiki/ProbeExporterMachine'>2.2 Probe/Exporter Machine</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/ProbeExporterMachine#Install_FlowTraq_Free_Flow_Exporter'>2.2.1 Install FlowTraq Free Flow Exporter</a>
</li></ul></li><li><a href='https://code.google.com/p/renisac/wiki/Traffic_Generator'>2.3 Traffic Generator</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/Traffic_Generator#Install_Virtual_Box'>2.3.1 Install Virtual Box</a>
</li><li><a href='https://code.google.com/p/renisac/wiki/Traffic_Generator#Install_OpenWebSpider_v0.7'>2.3.2 Install OpenWebSpider v0.7</a>
</li></ul></li><li><a href='https://code.google.com/p/renisac/wiki/SwitchConfiguration'>2.4 Switch Configuration</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/SwitchConfiguration#SPAN_vs_TAP'>2.4.1 SPAN vs TAP</a>
</li><li><a href='https://code.google.com/p/renisac/wiki/SwitchConfiguration#HP_Procurve'>2.4.2 HP Procurve</a>
</li><li><a href='https://code.google.com/p/renisac/wiki/SwitchConfiguration#Cisco_IOS'>2.4.3 Cisco IOS</a>
</li><li><a href='https://code.google.com/p/renisac/wiki/SwitchConfiguration#Junos_OS'>2.4.4 Junos OS</a>
</li></ul></li></ul></li><li><b>Chapter 3 Configure Flow Data Export:</b>
<ul><li><a href='https://code.google.com/p/renisac/wiki/FlowDataExport#Introduction'>3.1 Introduction</a>
</li><li><a href='https://code.google.com/p/renisac/wiki/FlowDataExport#NetFlow_,_sFlow,_and_J-Flow_oh_my!'>3.2 NetFlow, sFlow, and J-Flow oh my!</a>
</li><li><a href='https://code.google.com/p/renisac/wiki/NetFlow_Config'>3.3 NetFlow</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/NetFlow_Config#Cisco'>3.3.1 Cisco</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/ConfiguringOldNetFlow#ConfiguringOldNetFlow'>3.3.1.1 Old Cisco IOS NetFlow Configuration</a>
</li></ul></li><li><a href='https://code.google.com/p/renisac/wiki/NetFlow_Config#HP'>3.3.2 HP</a>
</li><li><a href='https://code.google.com/p/renisac/wiki/NetFlow_Config#Juniper'>3.3.3 Juniper</a>
</li></ul></li><li><a href='https://code.google.com/p/renisac/wiki/sFlow_Config'>3.4 sFlow</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/sFlow_Config#Cisco'>3.4.1 Cisco</a>
</li><li><a href='https://code.google.com/p/renisac/wiki/sFlow_Config#HP'>3.4.2 HP</a>
</li><li><a href='https://code.google.com/p/renisac/wiki/sFlow_Config#Juniper'>3.4.3 Juniper</a>
</li></ul></li><li><a href='https://code.google.com/p/renisac/wiki/JFlow_Config'>3.5 J-Flow</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/JFlow_Config#Juniper'>3.5.1 Juniper</a>
</li></ul></li></ul></li></ul></li><li><b>Part 2:</b> AppFlow Layer (WIP)<br>
<ul><li><b>Chapter 4 Application Monitoring with AppFlow:</b>
<ul><li><a href='https://code.google.com/p/renisac/wiki/FlowDataExport#Introduction'>4.1 Introduction</a>
</li><li><a href='https://code.google.com/p/renisac/wiki/JFlow_Config'>4.5 J-Flow</a>
</li></ul></li></ul></li><li><b>Part 3:</b> Packet Capture Layer<br>
<ul><li><b>Chapter 5: Full Packet Capture:</b>
<ul><li><a href='https://code.google.com/p/renisac/wiki/PacketCapture'>5.1 What is Full Packet Capture?</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/PacketCapture#Introduction'>5.1.1 Introduction</a>
</li><li><a href='https://code.google.com/p/renisac/wiki/PacketCapture#Keep_Out_of_Trouble'>5.1.2 Keep Out of Trouble</a>
</li></ul></li><li><a href='https://code.google.com/p/renisac/wiki/PCAPRecorder'>5.2 Setup a Custom PCAP Traffic Recorder</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/PCAPRecorder#Introduction'>5.2.1 Introduction</a>
</li><li><a href='https://code.google.com/p/renisac/wiki/PCAPRecorder#Design_Considerations'>5.2.2 Design Considerations</a>
</li><li><a href='https://code.google.com/p/renisac/wiki/PCAPRecorder#Custom_Full_Packet_Capture'>5.2.3 Custom Full Packet Capture</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/PCAPRecorder#TCPDump'>5.2.3.1 TCPDump</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/PCAPRecorder#Post_Processing/Indexing'>5.2.3.1.1 Post Processing/Indexing</a>
</li></ul></li></ul></li><li><a href='https://code.google.com/p/renisac/wiki/PCAPRecorder#Other_Tools'>5.2.4 Other Tools</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/PCAPRecorder#dumpcap'>5.2.4.1 dumpcap</a>
</li></ul></li><li><a href='https://code.google.com/p/renisac/wiki/PCAPRecorder#Open_Source_Full_Packet_Capture_Systems'>5.2.5 Open Source Full Packet Capture Systems</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/PCAPRecorder#Open_FPC'>5.2.5.1 Open FPC</a>
</li><li><a href='https://code.google.com/p/renisac/wiki/PCAPRecorder#Moloch'>5.2.5.2 Muloch</a>
</li></ul></li></ul></li></ul></li></ul></li><li><b>Part 4:</b> NFSen for Fun and Profit<br>
<ul><li><b>Chapter 6: Getting Stated with NFSen</b>
<ul><li><a href='https://code.google.com/p/renisac/wiki/NFSen_Introduction'>6.1 NFSen Basics</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/NFSen_Introduction#The_NFSen_Advantage'>6.1.1 The NFSen Advantage</a>
</li></ul></li><li><a href='https://code.google.com/p/renisac/wiki/NFSen_Install'>6.2 Get NFSen Up and Running (Installation Guide)</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/NFSen_Install#Introduction'>6.2.1 Introduction</a>
</li><li><a href='https://code.google.com/p/renisac/wiki/NFSen_Install#Install_NFSen'>6.2.2 Install NFSen</a>
</li></ul></li></ul></li><li><b>Chapter 7: Gather Actionable Intelligence with NFSen</b>
<ul><li><a href='https://code.google.com/p/renisac/wiki/Traffic_Classification'>7.1 Traffic Classification: Profiles, Channels, and Plugins</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/Traffic_Classification?ts=1374516064&updated=Traffic_Classification#Making_Use_of_Profiles/Channels'>7.1.1 Making Use of Profiles/Channels</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/Traffic_Classification?ts=1374515482&updated=Traffic_Classification#Leveraging_NFSen_CLI_for_Profile/Channel/Filter_Generation'>7.1.1.1 Leveraging NFSen CLI for Profile/Channel/Filter Generation (Requested)</a>
</li><li><a href='https://code.google.com/p/renisac/wiki/Traffic_Classification?ts=1374515787&updated=Traffic_Classification#Scripting_NFSen_Group/Profile/Channel/Filter_Generation'>7.1.1.2 Scripting NFSen Group/Profile/Channel/Filter Generation</a>
</li></ul></li><li><a href='https://code.google.com/p/renisac/wiki/Traffic_Classification?ts=1374515787&updated=Traffic_Classification#Port_Tracker_Plugin'>7.1.2 Port Tracker Plugin (Requested)</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/Traffic_Classification?ts=1374516708&updated=Traffic_Classification#Install_Port_Tracker'>7.1.2.1 Install Port Tracker (Requested)</a>
</li></ul></li></ul></li><li><a href='https://code.google.com/p/renisac/wiki/Botnet_Install'>7.2 Botnet Detection Using NetFlow and NFSen</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/Botnet_Install'>7.2.1 Installation Guide</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/Botnet_Install#Install_Events-Plugin'>7.2.1.1 Install Events Plugin</a>
</li><li><a href='https://code.google.com/p/renisac/wiki/Botnet_Install#Please_Read_First_Before_Proceeding'>7.2.1.2 Please Read First Before Proceeding</a>
</li><li><a href='https://code.google.com/p/renisac/wiki/Botnet_Install#Install_Botnet-Plugin'>7.2.1.3 Install Botnet Plugin</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/Botnet_Install#Importing_Datasets'>7.2.1.3.1 Importing Datasets</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/Botnet_Install#The_Emerging_Threats_Example'>7.2.1.3.1.1 The Emerging Threats Example</a>
</li><li><a href='https://code.google.com/p/renisac/wiki/Botnet_Install#Integrating_Data_from_CIF'>7.2.1.3.1.2 Integrating Data from CIF (Requested)</a>
</li></ul></li><li><a href='https://code.google.com/p/renisac/wiki/Botnet_Install#Testing_the_Botnet_Plugin'>7.2.1.3.2 Testing the Botnet Plugin</a>
</li></ul></li><li><a href='https://code.google.com/p/renisac/wiki/Botnet_Install#Advanced_Events_Plugin_Configuration'>7.2.1.4 Advanced Events Plugin Configuration</a>
</li></ul></li></ul></li></ul></li></ul></li><li><b>Part 5:</b> Cache Layer (Requested)<br>
<ul><li><b>Chapter 8: The Cache Layer</b>
</li><li><b>Chapter 9: Introduction to Cacti</b>
<ul><li><b>9.1 What is Cacti?</b>
</li></ul></li><li><b>Chapter 10: Investigating Cache Using Cacti</b>
<ul><li><a href='https://code.google.com/p/renisac/wiki/Cacti_Install'>10.1 Install Cacti</a>
<ul><li><b>10.1.1 Integrate NFSen into Cacti</b></li></ul></li></ul></li></ul></li></ul>







<a href='https://code.google.com/p/renisac/wiki/NotetoContributors'>Note to Contributors</a>