
# Final Report – Enhancing Support for NAT64 Protocol Translation in NetBSD

[Source Code Branch:](https://github.com/dchidindu5/src/tree/gsoctest)

[Commits:](https://github.com/NetBSD/src/commit/d6dcfcfe40b7fa9f0acb566e23fd52b1482cfa24)

## Overview

A typical rule looks like:
`map wm0 algo "nat64" 64:ff9b:2a4:: -> 192.0.2.33`
`map wm0 algo nat64 plen 96 64:ff9b::8c52:7903 <- 140.82.121.3

This tells NPF to translate outgoing IPv6 packets using the prefix 64:ff9b:2a4::/96, rewriting them to use the IPv4 address 192.0.2.33. when the packet returns and hits the npf, it changes source from GitHub's IPv4 to GitHub's IPv6 address and then it rewrites the header.

**When an IPv6 packet from a client hits the NPF machine:**

- The NAT64 translation routine (npf_nat64_rwrheader()) rewrites the IPv6 header to an IPv4 header.
     The source becomes the host’s IPv4 address or any pool of IPv4 addresses.
     The destination becomes the IPv4 address of github extracted from IPv4 embedded IPv6 address defined in the rule configuration. (e.g. 140.82.121.3 from 64:ff9b::8c52:7903).

- TCP/UDP/ICMP checksums are recalculated using in4_cksum() or in6_cksum().

- The packet is then routed out to the IPv4 network.

**When a reply packet comes back from the IPv4 server:**

- NPF performs the reverse translation, embedding the IPv4 address inside the NAT64 prefix to form a valid IPv6 address using the npf_embed

- The IPv6 packet is then delivered back to the IPv6 client.

## Project Accomplishments

- Core Translation Path: Implemented IPv6 -> IPv4 header and reverse rewriting (npf_nat64_rwrheader() routines).

- Address Mapping: Added functions for embedding and extracting IPv4 addresses within IPv6 prefixes.

- Checksum Recalculation: Integrated checksum updates for IPv4/IPv6 and transport layers.

- Rule Parsing: Extended npf.conf syntax and parser to accept NAT64 configuration parameters.

- Userland & Kernel Integration: Updated kernel headers, userland utilities, and rule constructors.

- Testing: Verified translation with ping, curl, and dig, observing packets using tcpdump and Wireshark.

## Summary

This project successfully integrates NAT64 along with a separate DNS64 configuration into NPF, enabling IPv6-only clients to reach IPv4-only servers through seamless translation.
Although there's a need for additional changes and implementation.

This is indeed the End of my GSOC Program, it was indeed a exciting moment working with system developers and certainly I'd be an active contributor to the NetBSD codebase.
