# Iptables

- first match rule: chain traversing stop at first match

## Tables
![image info](./images/iptables-diagram.png)

See the tables:
- filter: `iptables -n filter -L -v`
- nat: `iptables -n nat -L -v`
- mangle: `iptables -n mangle -L -v`
- raw: `iptables -n raw -L -v`

## Flow
![image info](./images/Iptables_diagram.png)

- NAT can alter the routing decision for an incoming packet (FORWARD chain or INPUT chain?)

## The NAT Table

### DNAT, SNAT and MASQUERADE
Network Address Translation (NAT) occurs when one of the IP addresses in an IP packet header is changed:
- SNAT: Source IP address is changed
  - Requires you to give it an IP address to apply to all the outgoing packets.
  - Applied after the routinng decision has made
  - If SNAT is in place a DNAT is performed on reply packets (https://superuser.com/questions/1210742/does-iptables-perform-a-automatically-snat-for-response-packet-if-it-does-when)
- DNAT: Destination IP address is changed
  - Applied before the routing decision has made

The MASQUERADE target lets you give it an interface, and whatever address is on that interface is the address that is applied to all the outgoing packets.
In addition, with SNAT, the kernel's connection tracking keeps track of all the connections when the interface is taken down and brought back up; the same is not true for the MASQUERADE target.

## Docker and Iptables
- Quick bu useful guide to the relationship between docker and iptables: https://medium.com/@havloujian.joachim/advanced-docker-networking-outgoing-ip-921fc3090b09\
- Unused MASQUERADE rule on iptable nat's POSTROUTING chain: https://github.com/moby/moby/issues/12632