# NAT_GSOC2025

**Organization:** **NetBSD**

**Project:** **_Enhancing Support for NAT64 Protocol Translation in NetBSD_**

## Community Bonding Period(May 8th - June 2nd)

### Intro to NetBSD

NetBSD systems are quite old but they have a lot of functionality in modern technology. NetBSD is a portable Unix operating system in common use. It is a free and opensource OS that runs on a broad variety of platforms from modern desktop systems, handheld devices to high end servers. it was written in C and belongs to the family of BSDs.

### Building Toolchain on NetBSD

Let's assume Netbsd source have been cloned from GitHub

- Make multiple directories where the build files will reside `mkdir -p /usr/obj /usr/tools`
followed by this `chown $(whoami) /usr/obj /usr/tools`
- On the terminal enter" `cd /usr/src` This is likely the directory where netbsd source is located.
- Enter ` ./build.sh -U -j2 -m amd64 -M /usr/obj -T /usr/tools tools `

### NAT64(Network) and NPF(NetBSD Packet Filters)

 > Note that NPF already supports IPv4 NAT (NAPT) and static IPv6 prefix translation (NPTv6), but lacks IPv6-to-IPv4 translation.
 > Currently, NPF doesn't know how to "translate" IPv6 packets into IPv4 ones.

In a nutshell, NAT64 is a transition mechanism from IPv6 to IPv4. it enables IPv6-only clients to communicate with IPv4-only servers. The aim of NAT64 is to translate an IPv6 address packets into IPv4 packets by modifying Network address info in the IP header of packets as it interacts to ipv4 servers and viceversa.

[NPF](https://www.netbsd.org/~rmind/pub/npf_presentation_netbsd_6.pdf) known as NetBSD packet filter is a layer 3 packet filter that provides support for **stateful packet inspection**, **NAT, IPv6 and TCP/IP traffic filtering**. Although it's capability extends beyond that.
Originally inspired by the Berkeley Packet Filter ([BPF](https://man.netbsd.org/bpfjit.4)), NPF uses its own n-code. It consists of CISC-like instructions for the common patterns to reduce the processing overhead.

### IPv4 and IPv6

IPv4 is a **32bit numeric address** written as four sets of numbers seperated by dots. It's made up of four sets of 8 bits binary called **Octet**. Each set has a range from 0 -255. Also known to have two main parts, the network address and a host address. e.g `10.22.34.0`

IPv6 is a **128bit hexdecimal address** written as eight(8) sets of numbers seperated by colons(:). It's made up of 8 sets of 16 bits called **Hextets**.e.g `2001:0db8:0000:0000:aa11....`

## Coding Period(June 2nd - Sept.)

Coming soon!!

---

### Resources

- [Building the NetBSD system](https://www.netbsd.org/docs/guide/en/chap-build.html)
- [Setting up NetBSD Packet Filter (NPF)](https://pub.nethence.com/bsd/npf)
- [Introduction to NPF – the packet filter of NetBSD](https://www.netbsd.org/~rmind/pub/npf_manual_netbsd_6.pdf)
- [NPF in NetBSD 6](https://www.netbsd.org/~rmind/pub/npf_manual_netbsd_6.pdf)
- [Mode of Operation](https://rmind.github.io/npf/intro.html#mode-of-operation)
