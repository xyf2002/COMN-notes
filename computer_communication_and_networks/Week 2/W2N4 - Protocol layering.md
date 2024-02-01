The internet is incredibly complicated, with many types of end systems, packet switches, applications, protocols, and link-level media. In order to make it easy to build, modify, maintain, and understand, it is divided into a layered stack of protocols:

| Layer | Works with | Purpose | Examples |
| :--: | :--: | ---- | ---- |
| Application | Messages | Where applications and their protocols exist | HTTP, FTP, DNS |
| Transport | Segments | Transports application layer messages between application endpoints | TCP/UDP |
| Network | Datagrams | Transport transport layer segments from one host to another | IP |
| Link | Frames | Transport network layer datagrams between nodes | Ethernet, WiFi |
| Physical | Bits | Transport individual bits within link layer frames from one network level between network elements. This is dependent on the physical link medium |  |
![[w2n4encapsulation.png]]
As data descends and ascends the network stack each layer adds or removes a header from the message. This also shows the difference between switches and routers, switches operate at the link layer while routers operate at the network layer.
