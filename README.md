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

## setsockopt and getsockopts
use to manipulate options for the socket refered to by the
file descriptor.

To know more refer to the manual [page](https://manpages.debian.org/buster/manpages-dev/setsockopt.2.en.html)
