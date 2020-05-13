# Netplan examples

```yaml
network:
    ethernets:
        eth0:
            dhcp4: true
            addresses: []
            nameservers: {}
    version: 2
    vlans:
        eth0.10:
            id: 10
            link: eth0
            addresses:
                - 172.16.10.99/24
        eth0.5:
            id: 5
            link: eth0
            addresses:
                - 192.168.5.237/24


network:
    ethernets:
        eth0:
            dhcp4: no
            addresses: [192.168.5.237/24]
            gateway4: 192.168.5.1
            nameservers:
               addresses:
                   - 8.8.8.8
                   - 8.8.4.4
    version: 2
    vlans:
        eth0.10:
            id: 10
            link: eth0
            addresses:
                - 172.16.10.99/24


network:
    ethernets:
        eth0:
            dhcp4: true
            addresses: []
            nameservers: {}
    version: 2
    vlans:
        eth0.10:
            id: 10
            link: eth0
            addresses:
                - 172.16.10.99/24


network:
    ethernets:
        eth0:
            dhcp4: no
            addresses: [192.168.5.237/24]
            gateway4: 192.168.5.1
            nameservers:
               addresses:
                   - 8.8.8.8
                   - 8.8.4.4
    version: 2
    vlans:
        eth0.10:
            id: 10
            link: eth0
            addresses:
                - 172.16.10.99/24


network:
    ethernets:
        eth0:
            dhcp4: no
            addresses: [172.16.32.99/24]
            gateway4: 172.16.32.1
            nameservers:
               addresses:
                   - 8.8.8.8
                   - 8.8.4.4
    version: 2
```