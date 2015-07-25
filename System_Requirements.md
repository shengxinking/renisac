# Testbed System Requirements #

## NetFlow Collector ##
**Minimum System Requirements**

Ubuntu Server (CLI) Installation

  1. Intel 2 Duo, 2.93GHzx2
  1. 4 Gig of system memory (RAM)
  1. 500 GB of disk space(dependent on how big your network is - general rule of thumb is 1MB for every 2Gb of network traffic )
  1. Graphics card and monitor capable of 640x480
  1. CD/DVD drive



**Recommended System Requirements**

Ubuntu Server (CLI) Installation

  1. Intel Xeon processor
  1. at-least 16GB ram (RAM)
  1. at-least 5TB of free after OS install disk space(dependent on how big your network is - general rule of thumb is 1MB for every 2Gb of network traffic )
  1. Graphics card and monitor capable of 640x480
  1. CD/DVD drive
  1. RAID + LVM knowledge


Please consider the size of your network and make a decision accordingly.

## NetFlow Probe/Exporter ##
**Minimum System Requirements**

Ubuntu Workstation Installation

  1. Intel 2 Duo, 2.93GHzx2
  1. 4 Gig of system memory (RAM)
  1. 500 GB of disk space
  1. Graphics card and monitor capable of 640x480
  1. 2 x 10/100/1000 Ethernet LAN cards
  1. CD/DVD drive


**Recommended System Requirements**

Ubuntu Workstation Installation

  1. Intel Qud core processor
  1. 6 Gig of system memory (RAM)
  1. +1TB of disk space
  1. Graphics card and monitor capable of 640x480
  1. 2 x 10/100/1000 Ethernet LAN cards
  1. CD/DVD drive



# Webspider/Traffic Generator #

_This section is for those that want to setup NetFlow in a controlled environment before deploying it to a live network_.


**Minimum Systems Requirements**

Webspider VM Installation

  1. intel Core Duo T2600 (2.16 GHz)
  1. 2GB of system memory (RAM)
  1. 100GB of disk space
  1. Graphics card and monitor capable of 640x480
  1. CD/DVD drive


# Switch System Requirements #

Switch Requirements

Your switch must have a the capability to mirror traffic. Cisco calls this the SPAN port while HP uses the term Mirrored port. For our testbed we used the HP ProCurve 5406zl Intelligent Edge Switch.