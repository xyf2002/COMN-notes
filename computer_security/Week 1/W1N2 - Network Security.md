# Stack of layers
Network communication models use a stack of layers.
Higher layers use services of lower layers, with the physical channel on the bottom layer.
A channel is open between 2 devices on every layer, though every layer but the bottom is an abstraction.

# Encapsulation
As a packet is sent and travels down the layers each layer adds its own header and footer.
![[w1n2packetencapsulation.png]]
Each level layer only needs to use information in its own header/footer

# Media Access Control (MAC) Addresses
Most network interfaces come with a predefined MAC address. (Some let you change it)

TODO - copy later

# Switch
A switch performs routing in a local area network
- Operates at the link layer
- Multiple interfaces, each connected to a computer or another switch/router
- A switch will forward each incoming frame to its target frame, a hub forwards it to all devices connected
- On connection a computer will broadcast its MAC address to inform the switch what happens

# Internet Protocol (IP)
## Addressing
In order to deliver data IP must be aware of where to deliver data to

## Routing
IP might be required to communicate across networks and communicate with networks not directly connected to the current network

## Fragmentation and reassembly
It is possible that a packet may be carried across a network with a shorter max packet length, so IP must be able to split and combine packets

## IP addresses
An address is subdivided into a <span style="color:green">network,</span> <span style="color:orange">subnet,</span> and <span style="color:red">host</span>
<span style="color:green">128.148.</span><span style="color:orange">32.</span><span style="color:red">110</span>

Broadcast addresses such as 128.148.32.<span style="color:red">255</span> send a packet to all hosts on the network

Private networks are not routed outside of a LAN
- 10.0.0.0/8
- 172.16.0.0/12
- 192.168.0.0/16

## IP header
Includes:
- Source address
- Destination address
- Packet length (up to 64KB)
- Time to live (up to 255)
	- Number of hops before the packet dies
- IP protocol version
- Fragmentation information

TODO get rest

## IP Routing
TODO

## Exploring internet routes
- Internet Control Message Protocol
	- Used for network testing and debugging
	- Simple messages in a single IP packets
	- Is a network layer protocol
- ICMP tools
	- Ping: sends a series of echo request messages and provides stats on roundtrip times and packetloss
	- Traceroute: sends a series of echo requests with increasing TTL
		- This shows what route the packets are taking

# Network attacks
A Denial of Service (DOS) blocks packets from reaching their destination.
Wiretapping/sniffing 

![[w1n2networkingattacks.png]]

# Wireshark
TODO