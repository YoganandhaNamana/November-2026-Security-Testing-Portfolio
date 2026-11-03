# Project 01: Network Reconnaissance and VAPT

## Project Objective
Understand networking and perform an authorised
network security assessment.

## Target Information
- Lab Name:
- Target IP:
- Scope:
- Testing Date:

## Learning Topics
- OSI and TCP/IP
- IP Addressing
- Subnetting
- TCP and UDP
- DNS and ARP
- Ports and Services

OSI & TCP/IP Model — Packet Architecture
<img width="1672" height="941" alt="OSI and TCP_IP Packet Architecture Infographic" src="https://github.com/user-attachments/assets/26067cf9-3fd3-43ad-ad84-3b2aa636d64e" />

The infographic above covers the complete topic, including packet architecture, encapsulation, protocol headers, TCP flags, the TCP three-way handshake, socket communication, and the practical Network Traffic & Protocol Analyzer project.
1. Core concepts explained
Packet encapsulation is the process of adding information to data as it moves down the network layers before transmission. For example, when your browser sends an HTTP request:
1. Application layer: Creates the application data, such as an HTTP request.
2. Transport layer: TCP adds source and destination port numbers, sequence numbers, acknowledgement numbers, and flags. UDP uses a simpler header with port numbers.
3. Internet layer: IP adds source and destination IP addresses.
4. Network Access layer: Ethernet adds source and destination MAC addresses and other frame information.
5. Physical layer: Transmits the data as electrical, radio, or optical signals.
At the receiving computer, the reverse process is called decapsulation. Each layer processes and removes its corresponding header until the original application data is delivered.
2. TCP flags and three-way handshake
TCP flags are control indicators in the TCP header. Common flags include SYN, ACK, FIN, RST, PSH and URG.

A TCP connection normally begins with a three-way handshake:
TCP three-way handshake
1. SYN — Client to Server
   
   The client requests a connection and sends its initial sequence number.
2. SYN-ACK — Server to Client
   
   The server acknowledges the client's request and sends its own sequence number.
3. ACK — Client to Server
   
   The client acknowledges the server. The TCP connection is established.

3. Socket communication
A socket is an endpoint used by an application for network communication. It is commonly identified by an IP address, port number and transport protocol.

For example:
- Client: 192.168.1.10:52344
- Server: 93.184.216.34:80
- Protocol: TCP
The browser uses its client socket to communicate with the web server's listening socket.
4. Hands-on project: Network Traffic & Protocol Analyzer
The objective is to capture network traffic using Wireshark or tshark, identify individual packets, and understand how a connection is established and how application data travels.

You will examine:
Activity	What you investigate
Live capture	Capture traffic from a selected network interface
Packet filtering	Isolate TCP, HTTP, HTTPS and DNS traffic
TCP handshake	Identify SYN, SYN-ACK and ACK packets
Header analysis	Inspect Ethernet, IP and TCP/UDP headers
Stream analysis	Follow a TCP stream and inspect the exchange
Documentation	Record packet numbers, addresses, ports, flags and observations

Some useful Wireshark display filters:
tcp
http
https
dns
tcp.flags.syn == 1
tcp.flags.ack == 1
tcp.port == 80
tcp.port == 443


Note: HTTPS traffic is encrypted, so you can inspect its connection metadata and TLS handshake, but you generally cannot read the HTTP contents without authorized decryption
