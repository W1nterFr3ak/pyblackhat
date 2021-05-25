# pyblackhat
This is my attempt to decompile the Blackhat Python in the most beginner friendly way as i possibly can.
Dont take this as a level wise decompilation, i will talk about the concepts as i meet them as in the book below is how this is gonna happen. The official sessions will begin on **JUNE** stay tuned __hahahahahahaha__ (evil laugh)


# Sockets
## Types of Sockets
    1. Stream
    2. Datagram
    3. Raw


### Stream
The stream socket defines a reliable conection service, data is sent without error or duplication but we have to know that there is no length boundaries between data cause its treated as a stream of bytes and so there must be an agreemant between the comunicating processes. Usually done by sending the length before the data. 
The type in python is SOCK_STREAM


### Datagram
The datagram socket defines a conectionless service and the datagrams are sent as indipendent packets and the size is limited to the size able to be sent in a single transaction the maximum being 65535
The type in python is SOCK_DGRAM

### Raw
Allows access to lower layer protocols such as IP and ICMP and is often used to test new protocol implementation.
Which means a raw socket receives un-extracted packets. There is no need to provide the port and IP address to a raw socket, unlike in the case of stream and datagram sockets.
The type in python is SOCK_RAW


---


# Network packets and packet sniffers

When we communicate on a network, data sent is processed by various layers before being send alternatively when receiving data is decosnstructed and sent to the relevant application.
Example when we connect to www.somesite.com. <br>
www.somesite.com sends packet to our machine which is then deconstructed by our machine and sent to the relevant application in this case the browser.
Our machine is only allowed to view packets specifically addressed to it , this is known as non-promiscous mode to listen on all packets we have to enable promiscous mode on our machine.
Applications that listens on all packets and analyse them are commonly known as packet sniffers their are various sniffers available the most common one being wireshark in which creating our own can be seen as pretty useless but doing so shows us the inner workings of such sniffers and equips us with a tool that can never be uninstalled.

#### Steps for building our packet sniffer
1. Open a raw socket
2. Set up promiscous mode
3. Receive network packets
4. Extract IP headers


---


# Host discovery Sniffer 

Now we will create a sniffer with a purpose of discovering hosts on a network on both a windows host and Linux host.

#### Opening a raw socket

To open a socket we have to know three things. This will apply to most of our script its not amust we cram all of them we can always refer back to documentations or this list
1. Socket family
    - AF_LOCAL: Used for local communication
    - AF_UNIX: Unix domain sockets
    - AF_INET: IP version 4 
    - AF_INET6: IP version 6
    - AF_IPX: Novell IPX
    - AF_NETLINK: Kernel user-interface device
    - AF_X25: Reserved for X.25 project
    - AF_AX25: Amateur Radio AX.25
    - AF_APPLETALK: Appletalk DDP
    - AF_PACKET: Low-level packet interface
    - AF_ALG: Interface to kernel crypto AP
2. Socket type 
    - SOCK_STREAM: Stream (connection) socket
    - SOCK_DGRAM: Datagram (connection-less) socket
    - SOCK_RAW: RAW socket
    - SOCK_RDM: Reliably delivered message
    - SOCK_SEQPACKET: Sequential packet socket
    - SOCK_PACKET: Linux-specific method of getting packets at the development level 
3. Protocol
    - TCP: 6
    - ICMP: 1
    - UDP: 17
    - RDP: 27 




## Sniffer Core code walkthrough

```python
import socket

import os 

HOST  = "192.168.56.1"

```

we import our modules that will assist us on our sniffing journey
Then we specify our host ip.

```python
if os.name == 'nt':
        socket_protocol =socket.IPPROTO_IP
    else:
        socket_protocol = socket.IPPROTO_ICMP   

```
We perform a os check because windows allows us to listen on all incoming packets but linux requires us to specify that we are sniffing ICMP packets


```python
sniffer = socket.socket(socket.AF_INET, socket.SOCK_RAW, socket_protocol)
sniffer.bind((HOST, 0))

# tell socket to include IP headers
sniffer.setsockopt(socket.IPPROTO_IP, socket.IP_HDRINCL, 1)
```
We create our socket object which is similar to a file descriptor, come to think of it, it is a file descriptor.
We bind our host to our socket and the set an option to include ip headers in our packet.

```python
   if os.name == 'nt':
        # set promiscous mode
        sniffer.ioctl(socket.SIO_RCVALL, socket.RCVALL_ON)
 sniffer.recvfrom(65565)
```
We send an [IOCTL](https://man7.org/linux/man-pages/man2/ioctl.2.html)(input/output control)  to the network card driver to enable promiscous(literaly lol) mode.<br>
After successfully opening a raw socket, its time to receive network packets, for which you need to use the recvfrom api. We can also use the recv api. But recvfrom provides additional information.


Aha we now we can assign the recvfrom api to a variable to store our network packet and the next thing is to deconstruct it and retrieve required information.