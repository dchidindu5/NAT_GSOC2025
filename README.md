## NAT_GSOC2025

**Organization:** **NetBSD**
**Project:** **_Enhancing Support for NAT64 Protocol Translation in NetBSD_**

### Community Bonding Period(May 8th - June 2nd)
**Intro to NetBSD**
NetBSD is a portable Unix operating system in common use. It is a free and opensource OS that runs on a broad variety of platforms from modern desktop systems, handheld devices to high end servers. it was written in C and belongs to the family of BSDs.

### NAT64(Network) and NPF(NetBSD Packet Filters)
In a nutshell, NAT64 is a transition mechanism from IPv6 to IPv4. it enables IPv6-only clients to communicate with IPv4-only servers. The aim of NAT64 is to translate an IPv6 address packets into IPv4 by modifying Network address info in the IP header of packets as it interacts to ipv4 servers and viceversa.

[NPF ](https://www.netbsd.org/~rmind/pub/npf_presentation_netbsd_6.pdf)known as NetBSD packet filter is a layer 3 packet filter that provides support for **stateful packet inspection**, **NAT, IPv6 and TCP/IP traffic filtering**. Although it's capability extends beyond that.
Originally inspired by the Berkeley Packet Filter ([BPF](https://man.netbsd.org/bpfjit.4)), NPF uses its own n-code. It consists of CISC-like instructions for the common patterns to reduce the processing overhead.


---

### Resources

- [Building the NetBSD system](https://www.netbsd.org/docs/guide/en/chap-build.html)
- [Setting up NetBSD Packet Filter (NPF)](https://pub.nethence.com/bsd/npf)
- [Introduction to NPF – the packet filter of NetBSD](https://www.netbsd.org/~rmind/pub/npf_manual_netbsd_6.pdf)
- [NPF in NetBSD 6](https://www.netbsd.org/~rmind/pub/npf_manual_netbsd_6.pdf)
- [Mode of Operation](https://rmind.github.io/npf/intro.html#mode-of-operation)
