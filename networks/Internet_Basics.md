
- [What is a protocol ?](#what-is-a-protocol--)
  * [TCP](#tcp)
  * [UDP](#udp)
  * [HTTP](#http)
- [Packets](#packets)
- [Addressing](#addressing)
  * [IP Address](#ip-address)
  * [Port](#port)
- [End Systems](#end-systems)
  * [The Network Edge](#the-network-edge)

## What is a protocol ?
A set of rules governing the exchange or transmission of data between devices.

### TCP
The Transmission Control Protocol (TCP) is one such protocol. It was
created to allow end systems to communicate effectively. The distinguishing
feature of TCP is that it ensures that data reaches the intended destination and
is not corrupted along the way.

### UDP
The User Datagram Protocol (UDP) is also one such key protocol. However, it
does not ensure that data reaches the destination and that it remains non-corrupt.

### HTTP
HyperText Transfer Protocol (HTTP) is a web protocol that defines the
format of messages to be exchanged between web clients, e.g., web browsers
and web servers and what action is to be taken in response to the message.
The World Wide Web uses this as its underlying protocol.

## Packets
Computers send messages to each other that are made up of ones and zeros
(bits). 

However, instead of sending messages of possibly trillions of bits all in one go,
they’re broken down into smaller units called packets to make transmission
more manageable. 

## Addressing
So, applications communicate with each other by sending messages based on
protocols. However, packets have to be addressed to a certain application on a
certain end system. 

An address identifies a sending entity and a receiving entity.

### IP Address
Every device that is connected to the Internet has an address called an ‘IP
Address.
1. IP addresses are 32 bit numbers.
1. The human readable way for looking at these numbers is the dotted
   decimal notation, whereby the number is considered one octet of bits (8
   bits) at a time. Those octets are read out in decimals, then separated by
   dots. Example : `1.2.3.4`
   
   Check yours out by running the command `curl ifconfig.me -s`

### Port
Any host connected to the Internet could be running many network
applications. In order to distinguish these applications, all bound to the same
IP address, from one another, another form of addressing, known as port
numbers, is used. Each endpoint in a communication session is identified
with a unique IP address and port combination. This combination is also
known as a *socket*.

1. IP addresses identify end systems but ports identify an application on the
   end system.

1. Every application has a 16-bit port number. So the port number could
   range from 0 to 2 = 65535.

1. Ports ranges :
    1. 0-1023 : Reserved for specific applications and are called *well-defined ports*. 80 for HTTP traffic.
    1. 1024 − 49152 : Registered ports, SQL : 1433
    1. 49152–65535 : Dynamic Ports, use it however you like.
   
![](https://slideplayer.com/slide/6224109/20/images/45/Sockets+and+Ports+%28cont%E2%80%99d.%29.jpg)

## End Systems 
End systems are devices that are connected to the Internet. The Internet's end systems include : 
 1. desktop computers (e.g., desktop PCs, Macs, and UNIX-based workstations) 
 1. servers (e.g., Web and e-mail servers) 
 1. mobile computers (e.g., portable computers, PDAs, and phones with wireless Internet connections). 
 1. Household appliances, Web TVs and set-top boxes, digital cameras, home appliances, factory floor equipment, and environmental sensors, are being attached to the Internet as end systems.

### The Network Edge
The network edge is simply the collection of end-systems that we use every
day: smartphones, laptops, tablets, etc. However, note that **devices that relay
messages (such as routers) are not part of the edge of the Internet.**
