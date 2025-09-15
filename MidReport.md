# Status Report

## Written by Dennis Onyeka

## **Introduction & Description**

The goal of the NAT64 project is to implement IPv6-to-IPv4 translation inside NPF (NetBSD Packet Filter).
NAT64 enables IPv6-only clients to communicate with IPv4-only servers by embedding/extracting IPv4 addresses in IPv6 addresses as per RFC 6052 and RFC 6145.
We are using a 1:1 mapping for now, to implement NAT64 translation.
Whereby an IPv6 host will use its IPv4 address to communicate with an IPv4 only server. As an example of IPv4, we will use github.com (140.82.121.3) that supports only IPv4.
In order to enable NAT64 on NPF we will have a rule like this:

```
map wm0 algo "nat64" 64:ff9b:2a4:: -> 192.0.2.33
```

This means we want to use the host IPv4 address associated to `wm0` interface 192.0.2.33 to access the public internet in order to communicate with GitHub's IPv4 server.
During this process, IPv6 header will be rewritten to IPv4. Part of the IP structure requires source and destination address so our new IPv4 source address will be the Host IPv4 address (which is likely to change during further improvement) and the IPv4 destination address will be gotten from GitHub’s IPv4 embedded IPv6 address i.e. the IPv4 address embedded into IPv6 address (64:ff9b::8c52:7903) gotten from the DNS resolver.
Note that NPF is our router so we will have to enable and configure a DNS caching resolver like [unbound](https://man.NetBSD.org/unbound.8) on the NetBSD machine. The job of the DNS64 is to synthesize AAAA responses from IPv4-only A records using the well-known prefix (64:ff9b::/96), then embed the Github’s IPv4 address (140.82.121.3) into it which will be (64:ff9b::8c52:7903). The hexadecimal values represent Github’s IPv4 address.
When the packet is returning from GitHub, it uses the IPv6 interface, so the IPv4 address part will be embedded back into its required position based on the prefix length passed by the user on the configuration. Then it will be sent to the IPv6 interface of the host machine.
So far, I’ve been focusing on the core translation path, making sure headers are rewritten correctly and transport checksums are updated. This also requires changes in the userland and kernel.

### Current Status

- Address Embedding and Extraction: Implemented functions that are responsible to extract IPv4 from IPv6 and embed IPv4 back into IPv6. Supports prefix lengths 32–96, with 96 as default.
- IP Header Rewrite: Added a functionality that enables headers rewriting by stripping off the old IP header and creating a new header along with structures.
- Rule Parsing: Added keyword support in `npf.conf`, so users can specify NAT64 prefix length.
- Checksum Handling: Integrated checksum recalculation for IPv4, IPv6, and transport layers (TCP/UDP/ICMP).

### More functionalities to be added (In the coming weeks)

- State Integration: Hooked into NPF’s connection tracking so NAT64 works with stateful packet filtering.
- Fine tune the checksum handling

### Development Environment & Test Environment

- Running NetBSD in a VM (on Windows host) for test implementation
- Editing and building code on WSL (Windows Subsystem for Linux).

### Testing

- Testing traffic with `ping`, `nc` (`netcat`), `curl`, `dig`.
- Using `tcpdump`/`tshark`/Wireshark on NetBSD to inspect packets before/after translation.

### Debugging

- Building kernel and userland via `build.sh` with kernel debug logs enabled
- Tracing packet flow via `ktrace`
- Incremental testing/unit: Test functions one by one.

### Experience, Observation, Impressions

Experience: Working deep inside NetBSD’s kernel networking stack has been challenging but rewarding. It gave me hands-on experience with `mbuf`s, packet parsing, and kernel-level checksums.

### Observation

- Testing NAT64 also requires real network setups, not just unit tests, since correctness depends on seeing the actual wire-format packets.
- Prefix length handling is subtle, user input must be validated and handled gracefully (defaulting to 96).
- The separation between userland rule parser and kernel execution path is strict, everything is serialized into structures properly.

### Impressions about NetBSD

- The codebase is clean and modular, but sometimes well-documented to an extent.
- NetBSD’s networking stack is solid and flexible, NPF feels light and extensible.
- The build system is stable.
