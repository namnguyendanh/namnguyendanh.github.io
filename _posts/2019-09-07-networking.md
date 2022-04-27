---
title:  "Networking: Network Basics"
categories: 
    - Technology
---

Bài này là bản tóm tắt kiến thức mình học được từ cuốn *Networking All-in-One For Dummies (7th Edition)*. Cuốn sách này sinh ra để tổng hợp toàn bộ kiến thức nên biết về Networking trong gần 1000 trang sách.

Bắt đầu thôi !

## Defining a Network
Two or more computers connected by a cable or by a wireless radio connection so that the can exchange information. Networks are about sharing 3 things: files, resources, and programs. There are 2 kinds of computers are on a network: servers and clients

- **Dedicated server**: a server perform no other task than netwrok services.
- **peer-to-peer network**: a computer can function as both a client and a server.

- **Network interface**: an electronic circuit. Ex: network interface card (NIC). Port, interface, adapter - three words that mean the same thing.
- **Network switch**: connect all the computers to each other.
- **Network router**: is used to connect two networks. Typically, a router is used to connect your network to the Internet.


- **LAN**: Local Area Network. "Local" doesn't mean small, a LAN can contain hundreds or even thousands of computers.
- **WAN**: Wide Area Network. WANs are typically used to connect two or more LANs that are relatively far apart.
- **MANs**: Metropolitan Area Network. This kind of network is smaller than a typical WAN but larger than a LAN. Typically, a MAN connects two or more LANs within the same city that are far enough apart.

## Network Topology
- **Node**: a node is a device that's connected to the netowrk.
- **Packet**: a packet is a message that's sent over the network from on node to another node. The packet includes the address of the node that sent the packet, the address of the node the packet is being sent to, and data.

### Bus topology
- Nodes are strung together in a line. 
- In a bus topology, every node on the network can see every packet that's sent on the cable. Each node looks at each packet to determine whether the packet is intended for it.
- If the cable in a bus network breaks, the entire network is effectively disabled.

### Star topology
- Each network node is connected to a central device called a hub or a switch. Star topologies are commonly used with LANs.

#### Hub vs Switch
- A hub doesn't know anything about the computers that are connected to each of its ports. So when computer connected to the hub sends a packet to a computer that's connected to another port, the hub sends a duplicat copy of the packet to all its ports. In contrast, a switch knows which computer is connected to each of its ports. As a result, when a switch receives a packet intended for a particular computer, it sends the packet only to the port that the recipient is connected to. Strictly speaking, only networks that use switches have a true star topology.

#### Expanding stars
For larger netowrks, it's common to create more complicated topologies that combine stars and buses. 

- The bus that connects the switches is sometimes called a *backbone*. 
- *Daisy-chaining*: a switch is connected to another switch as if it were one of the nodes on the star.

### Ring topology
- Packets are sent around the circle from computer to computer. Each computer looks at each packet to decide whether the packet was intended for it.

### Mesh topology
- Has multiple connections between each of the nodes on the network. The advantage of a mesh topology is that if one cable breaks, the network can use an alternative route to deliver its packets.
- Mesh networks are common for metropolitan or wide are networks. 

## Network Infrastructure
The infrastructure of a network consists of physical elements:
- Cables:
- Patch panels:
- Network switches
- At least one router.

**Protocol**: provides a precise sequence of steps that each element of a network must follow to enable communications. Protocols also define the precise format of all data that is exchanged in a network. Ex: The Internet Protocol (IP) defines the format of IP addresses: 4 eight-bit numbers called octets whose decimal values range from 0 to 255.

**Standard**: a detailed definition of a protocol that has been established by a standards organization and that vendors follow when they create products. Because of standards, you can instead purchase equipment from different vendors with the assurance that they'll work together.

Network standards are organized into a framework called the Open Systems Interconnection (OSI) Reference Model.

### Understanding Cable Infrastructure
- **Twisted-pair cable**: The maximum length of a single run of Cat-5e cable is 100m. Cat-5e cable is able to carry network data at speeds of up to 1 Gbps.

- **Repeater**: circumvent the maximum length limitation of twisted-pair network cables.
- **Hub**: a repeater with more than two ports. Hubs are almost never used anymore. Consider switches.
- **Switches**: allow you to connect more than one device, and packets received on one port are relayed to other ports. The switch is able to examine the actual contents of the data that it receives. A switch looks at the destination address and repeats the incoming packet only for the port that can deliver the packet to the intended destination.
- **MAC address**: Every network interface must have a unique identifier call a MAC address (or physical address). MAC addresses are important because the provide the means for a network to keep track of the devices that make up the network. 

- **Packet**: a small unit of data. It includes the MAC address of both the sender and the destination. The payload of an Ethernet packet is almost always a packet created by another higher-level protocol such as IP.

- **Collision**: When two or more interfaces are shared on a single cable, there is always the possibility that two or more intefaces will try to send information at the same time. The result of a collision between two packets is that both packets will be destroyed in the process and will need to be sent again. That is why we need to use switches instead of hubs.

**Boardcast packets**: 
- are intended to be received by every device on the network. To send a broadcast packet, the sending interface sets the destination MAC address to FF-FF-FF-FF-FF-FF. 
- One of the most common users of broadcast packets is Dynamic Host Configuration Protocol (DHCP), which allows computers that join a network to be assigned an IP address. When a network interface is first connected to a network, it sends out a broadcast message requesting the address of the network's DHCP server. Every device on the network sees this packet. But only the DHCP service will respond.

**Wireless Networks**:
- Rather than a switch or a hub, wireless devices connect to a wireless access point (WAP)
- Collisions are possible on a WAP. Unfortunately, there is no equivalent to a wireless switch that reduces the collision problem. WAPs are essentially hubs in that every device that connects to the acces point is competing for the same bandwidth. Whenever the WAP sends a packet, all devices connected to the access point must inspect the packet to determine the MAC address destination. And if two devices try to send packets at the same time, a collision will occur. This is one of the inherent reasons that wireless networking is slower than wired networking.

### Switch
A switch needs to know what MAC addresses are reachable via each of its ports. So it need to learn. -> MAC address table or *forwarding database*.

Ex:

|Port|MAC Address|
|---|---|
|3|21-76-3D-7A-F6-1E|

- a switch port might actually connect to more than one device. For example, suppose port 5 connect to another switch.

Ex:

|Port|MAC Address|
|---|---|
|3|21-76-3D-7A-F6-1E|
|5|76-2F-F9-C8-B6-08|
|5|D6-4E-69-86-E9-F7|
|5|06-C1-15-A2-BA-60|

#### Three basic functions of a switch
**Learning**: The process of building the MAC address table

**Forwarding**: Look up the table then send the packet

- Switches have memory buffers assocated with each port. This allows the switch to hold onto the packet for a bit if necessary before forwarding it.
- When the destination device receives the packet, the device has no idea that the packet passed through the switch (no tracing information is added to the packet by the switch).

**Flooding**: When it can't find MAC address of destination, it simply forwards the packet on all available ports other than the one the packet arrived on (like a hub).

Switches don't completely eliminate collisions. For example, suppost a switch has received a packet intended for a computer, and that computer attempts to send a packet at the same momnet that the switch attempts to forward the received packet to the computer. In that case, the two packets collide, and both the switch and the computer must wait and try again a bit later.

#### Bridging
**Bridge**: a device that is very similar to a switch, but it typically has fewer ports. The primary purpose of a bridge is to provide a link between two networks. One function that a bridge can perform can come in handy: A bridge can be used to connect two different types of networks. Ex: Cat-5e port and fiber-optic port.

#### SFP ports and uplinks
SFP ports: connect a variety of different types of high-speed networks.

**uplink**: The interconnection between two switches.

#### Broadcast domain
**Broadcast domain**: The scope of the devices that broadcast packets are intended for.

Allowing broadcast packets to travel throughout a large network is not a good idea (consume bandwidth). 

The most common type of broadcast packet is and Address Resolution Protocol (ARP) request. ARP is the protocol used to determine the MAC address of a given IP address. The sender broadcasts an ARP request, which is essentially the question "Does anyone know the MAC address of this particular IP address? If so, please let me know."

Reducing the amount of broadcast traffic on a network is a key way to improve the network's overall performance. One of the best ways to do that is to segment the network in a way that splits up the broadcast domains.

#### Managed and unmanaged switches
Managed switches support VLANs.

### Understanding Routers
- Routers work with IP addresses
- Can facilitate communication between IP networks with different subnets
- Enable a private netowrk to communicate with the Internet
- Routers split up broadcast domains

### Network address translation (NAT)
When a computer on the private side of the network sends a packet through the router to the Internet, the router substitutes its own public IP address as the sender address, and keeps strack of the face that it sent a packet on behalf of a computer on the private side. 

When the recipient on the Internet receives the packet, it sees that the sender was the router. It then sends a response back to the router, which then substitutes the original sender's private IP address for the destination address and forwards the packet to the correct computer on the private network.

### Virtual private netowrk
VPN is a secure connection between two private networks over a public network. All the data that flows over the VPN is encrypted, so anyone who steals packets from the VPN will find them unintelligible, only the parties on either end of the VPN are able to decrpt the packets.

VPN connections are often called *tunnels*, because the privde an isolated pathway from one point to another through the Internet. 

wo common uses for VPNs;
- To provide remote workers with secure access to your company network.
- To establish a tunnel directly between routers on two networks that are separated geographically
