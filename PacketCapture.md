# Introduction #

A full packet capture (FPC) system (or packet sniffer) is a tool which intercepts and records full packet data. The phrase "full packet data" refers to the raw packets traversing the network. The sniffer may also decode the packet and print it in a readable form. In previous sections, we described the usefulness of gathering flow data on packets traversing the network. However, flow data does not provide the fine granularity of a packet capture (pcap). An additional advantage of recording pcaps are that full packet data can be replayed back into the network, perhaps to an IDS. In addition, pcaps can be archived and stored for later analysis providing a historical view of the network.

# Keep Out of Trouble #

FPC systems are an extremely useful administrators in troubleshooting, managing, and securing network resources. However, one should be aware of privacy concerns when setting up a FPC system. Legal trouble could result without proper care and consideration. So how can one protect themselves?

  * Ensure you publish your intentions and methods whether it be through employee contracts or banner logins. Make your intentions known.
  * Ensure a publicly available Privacy Policy is available on your website.
  * Privacy Policy must include how the data is collected, used and managed. It should also indicate how long the data is retained.
  * Take precautions to ensure the security of the pcaps. Treat the data as if it were the original.
  * If moving pcaps offsite, consider Trans Border Data Flow issues if storing data in another country.
  * Permanently delete the pcaps if the data has expired its useful life.
  * Security review of the data might be a breach of privacy. Review the relevant laws in your jurisdiction.
  * In situations where the idea of a full packet capture system is legally murky, gathering flow data is an excellent alternative. There are not as many privacy concerns with flow data since the packet payload is not recorded as part of the flow. Thus, sensitive information is not exposed.

Article [[1](http://blog.blackfoundry.com/2011/04/)] provides many useful tips and best practices.

# References #

  1. http://blog.blackfoundry.com/2011/04/
  1. http://en.wikipedia.org/wiki/Computer_surveillance