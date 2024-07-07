---
title: Host discovery
date: 2024-07-07 21:18:00 +0200
categories: [learning, hacknotes]
tags: [cybersecurity, basics]
---

There are few helpful scanners: nmap, arp-scan and masscan, first of which is a industry standard

Nmap scan usually goes through the following steps shown below, although many are optional and depend on the command-line arguments provided
1. Enumerate targets
2. Discover live hosts
3. Reverse-DNS lookup
4. Scan ports
5. Detect versions
6. Detect OS
7. Traceroute
8. Scripts
9. Write output

## ARP scan
Nmap uses ARP requests when scanning inside the network. Otherwise it switches to ICMP echo or TCP 3-way handshake

- `-PR` - proceed with an ARP scan

We can also use `sudo arp-scan -I eth0 -l` to send ARP queries for all valid IP addresses on the eth0 interface. It can be installed with `apt install arp-scan`

## ICMP scan
ICMP has [many types](https://www.iana.org/assignments/icmp-parameters/icmp-parameters.xhtml). ICMP Ping uses type 8 and 0 (echo and echo reply).

- `-PE` - use echo requests to discover live hosts
- `-PP` - use timestamp requests
- `-PM` - use address mask queries

## Other
`-sn` options disables port scan, which is useful when detecting if host is up.

We can use TCP SYN ping via `-PS` option followed by the port number, range, list, or a combination of them. For instance, `-PS80`, `-PS5-35`, `-PS22,9001`. <br>
There is also TCP ACK option available as `-PA`. Nmap should be running as a privileged user. If you try it as an unprivileged user, Nmap will attempt a 3-way handshake. By default, port 80 is used.<br>
Finally, there is a UDP ping, which we can use with `-PU`. If we send a UDP packet to a closed UDP port, we expect to get an ICMP port unreachable packet; this indicates that the target system is up and available.

Compared to Nmap, Masscan is quite aggressive with the rate of packets it generates. It can be installed with `apt install masscan`.

On the final note, Nmap’s default behaviour is to use reverse-DNS online hosts. However, if you don’t want to send such DNS queries, you can use -n to skip this step.

By default, Nmap will look up online hosts; however, you can use the option `-R` to query the DNS server even for offline hosts. If you want to use a specific DNS server, you can add the `--dns-servers DNS_SERVER` option.
