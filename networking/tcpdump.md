# TCP dump

## TCP dump filter on src/dest host/port
`tcpdump -n -B 4096 -i ens5 '(src host 172.19.1.187 and dst host 172.19.1.195 and dst port 444) or (src host 172.19.1.195 and src port 444 and dst host 172.19.1.187)'`

## TCP dump end capture report meaning
```
8593 packets captured
12433 packets received by filter
3832 packets dropped by kernel
```
packets captured: packets tracked by tcpdump
packets received by filter: packets received before applying the filter
packets dropped by kernel: dropped because the tcpdump buffer was full (can be increased with the `-B` option)

## TCP dump and firewall (Iptables)
Incoming traffic:
`Wire -> NIC -> tcpdump -> netfilter/iptables`
this means that tcpdump can see incoming packets that can eventually be dropped by firewall

Outgoing traffic:
`iptables -> tcpdump -> NIC -> Wire`
this means that tcpdump cannot see packets dropped by the firewall

For more details:
https://superuser.com/questions/925286/does-tcpdump-bypass-iptables
