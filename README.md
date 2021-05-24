# pyblackhat


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
Allows access to lower layer protocols such as IP and ICMP and is often used to test new protocol implementation
The type in python is SOCK_RAW


---


# Network packets and packet sniffers

When we communicate on a network, data sent is processed by various layers before being send alternatively when receiving data is decosnstructed and sent to the relevant application.
Example when we connect to www.somesite.com , <www.somesite.com> sends packet to our machine which is then deconstructed by our machine and sent to the relevant application in this case the browser.
Our machine is only allowed to view packets specifically addressed to it , this is known as non-promiscous mode to listen on all packets we have to enable promiscous mode on our machine.
Applications that listens on all packets and analyse them are commonly known as packet sniffers their are various sniffers available the most common one being wireshark in which creating our own can be seen as pretty useless but doing so shows us the inner workings of such sniffers and equips us with a tool that can never be uninstalled ie in case we need to sniff some packets and the host we are on dosent have any packet sniffers and dosent allow downloads but has python installed we can use our knowledge to do so.

