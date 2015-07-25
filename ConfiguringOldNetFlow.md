## Configuring NetFlow and NetFlow Data Export ##

In this documentation we assume that you are familiar with configuring cisco routers thus you know the what the different modes mean. Type the following commands on the router commandline interface.

**Step 1:**

Enable privileged EXEC mode.

> ```
Router> enable```


**Step 2:**

Enter global configuration mode

> ```
Router# configure terminal```

**Step 3:**

Enter the IP address of the workstation you want the NetFlow data to be sent/exported to, and the UDP port on which the workstation is listening on.

> ```
ip flow-export destination {ip-address | hostname} udp port```

Example:

> ```
Router(config)# ip flow-export destination 179.49.10.3 2055```

**Step 4:**

If you have more than one export destinations, then repeat step 3 for all your export destinations. **You can configure a maximum of 2 export destinations for NetFlow.**

**Step 5:**

To enable NetFlow version9 packet format.

> ```
ip flow-export version 9```

Example:

> ```
Router(config)# ip flow-export version 9```

**Step 6:**

Specify the interface you want to enable NetFlow on.

> ```
interface interface-type interface-number```

Example:

> ```
Router(config)# interface ethernet 0/0```

**Step 7:**

Enable NetFlow on the interface. Ingress captures traffics that is being captured the interface. Egress captures traffic that is being transmitted by interface.

> ```
ip flow {ingress | egress}```

Example:

> ```
Router(config-if)# ip flow ingress```

**Step 8:**

Exit configuration mode and return to global mode.

> ```
exit```

Example:

> ```
Router(config-if)# exit```

**Step 9:**

Repeat steps 6 to step 9 to enable NetFlow on other interfaces. Otherwise proceed to step 10.

**Step 10:**

Exits the the current configuration mode and returns to privileged EXEC mode.

> ```
end```

Example:

> ```
Router(config-if)# end```

## Verifying That NetFlow Working ##

Please enter the following commands on your router commandline interface to make sure that NetFlow is working properly:

**Step 1:**

Use this command to display the NetFlow configuration of an interface.

> ```
show ip flow interface```

Example:

> ```
Router# show ip flow interface```

**Step 2:**

Use this command to verify that NetFlow is operational and display a summary of NetFlow statistics.

> ```
show ip cache flow```

Example:

> ```
Router# show ip cache flow```


## To Verify That NetFlow is Exporting ##

To verify that NetFlow data is being exported the right way please perform the following command:

> ```
show ip flow export```


Example:

> ```
Router# show ip flow export```

The above command will display statistics for NetFlow data export.


All the above commands were courtesy of Cisco[.md](.md). For a more detailed explanation please refer to the  [References](https://code.google.com/p/renisac/wiki/ConfiguringOldNetFlow#References) section.

<br>
<h4><- <a href='https://code.google.com/p/renisac/wiki/DeploymentStrategies?ts=1370391879&updated=DeploymentStrategies'>Return to Deployment Strategies</a></h4>




<h1>References</h1>

<ol><li><a href='http://www.cisco.com/en/US/docs/ios-xml/ios/netflow/configuration/12-4t/get-start-cfg-nflow.pdf'>http://www.cisco.com/en/US/docs/ios-xml/ios/netflow/configuration/12-4t/get-start-cfg-nflow.pdf</a>