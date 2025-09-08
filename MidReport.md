# Status Report

## Written by Dennis Onyeka

## **Introduction & Description**

The goal of the NAT64 project is to implement IPv6-to-IPv4 translation inside NetBSD’s NPF (NetBSD Packet Filter).
NAT64 enables IPv6-only clients to communicate with IPv4-only servers by embedding/extracting IPv4 addresses in IPv6 addresses as per RFC 6052, RFC 6145).

We are using a 1:1 mapping for now, to implement nat64 translation.
Whereby an ipv6 host will use its ipv4 address to communicate with an ipv4 only server for e.g(Github).
On the NPF, we will have a rule like this `map wm0 algo NAT64 ipv6-> ipv4`
Invariably this means we want to use ipv4 to access the public internet in order to communicate with github’s ipv4 server.
During this process, ipv6 header will be rewritten to ipv4. Part of the ip structure requires source and destination address so our new ipv4 source address will be the Host ipv4 address (which is likely to change during further improvement) and the ipv4 destination address will be gotten from github’s ipv4 embedded ipv6 address i.e the ipv4 address in it’s ipv6 address gotten from the queried server.
When the packet is returning from github, it uses the ipv6 interface, so the ipv4 address part will be embedded back into its required position based on the prefix length passed by the user on the configuration. Then it will be sent to the ipv6 interface of the host machine.

So far, I’ve been focusing on the core translation path, making sure headers are rewritten correctly and transport checksums are updated. This also requires changes in the Userland and Kernel.

### Current Status

Address Embedding & Extraction: Implemented functions that are responsible to extract IPv4 from IPv6 and embed IPv4 back into IPv6. Supports prefix lengths 32–96, with 96 as default.
IP Header Rewrite: Added a functionality that enables rewriting of headers striping off the old ip header and creating a new header along with structures.

Rule Parsing: Added keyword support in npf.conf, so users can specify NAT64 prefix length.

Checksum Handling: Integrated checksum recalculation for IPv4, IPv6, and transport layers (TCP/UDP/ICMP).

### More functionalities to be added (In the coming weeks)

- State Integration: Hooked into NPF’s connection tracking so NAT64 works with stateful packet filtering.

- Fine tune the checksum handling

### Development Environment & Test Environment

- Running NetBSD in a VM (on Windows host) for test implementation

- Editing and building code on WSL (windows system linux).

### Testing

- Testing traffic with ping, nc (netcat), curl, dig.

- Using tcpdump/tshark/Wireshark on NetBSD to inspect packets before/after translation.

### Debugging

- Building kernel and userland with build shell script with Kernel debug logs

- Tracing packet flow with ktrace

- Incremental testing/unit: Test functions one by one.

### Experience, Observation, Impressions

Experience: Working deep inside NetBSD’s kernel networking stack has been challenging but rewarding. It gave me hands-on experience with mbufs, packet parsing, and kernel-level checksums.

### Observation

- Testing NAT64 requires real network setups, not just unit tests, since correctness depends on seeing the actual wire-format packets.

- Prefix length handling is subtle, user input must be validated and handled gracefully (defaulting to 96).

- The separation between userland rule parser and kernel execution path is strict, everything is serialized into structures properly.

### Impressions about NetBSD

- The codebase is clean and modular, but sometimes well-documented to an extent.

- NetBSD’s networking stack is solid and flexible, NPF feels lighter and more extensible.

- The build system is stable.
