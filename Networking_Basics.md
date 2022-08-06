# Networking Basics

TODO: Create notes out of this https://kodekloud.com/wp-content/uploads/2021/10/Networking-Basics.pdf

CheatSheet : https://access.redhat.com/sites/default/files/attachments/rh_ip_command_cheatsheet_1214_jcs_print.pdf

## Switch

1. To connect two systems/cloud we need switch which creates network between them
2. It needs interface to connect ex: eth01
3. Check interface using `ip link`
4. Switch helps to connect systems **within the network**


## Router
1. To connect systems outside of network, we need routers
2. It gets two different IPs on each side of network to communicate

## Gateways
1. How to locate before communicating outside of our Networking
2. Gateway is used to find the route the packet to the router.
3. Gateway is basically a door in network to go out
4. Displays current routing information and table `route`


### How to expose your network to the external world.

We are using the PDF mentioned in the above link for understanding and
kodekloud labs for example

1. let' say we have app01 and app03.
2. We have one jump host on which currently we are on.
3. Considering above situation, we would be easily able to connect to the app01
  app03 as they are on same n/w as jumphost(238 subnet). Every communication b/w
  app01 and app03 will go through jumphost as it is configured gateway.
  ```
  ip addr of app01 : eth0 172.16.238.11/24
  ip addr of app03:  eth0 172.16.238.13/24
  ip addr of jumphost: eth0 172.16.238.10/24
  ```
4. First basics requirement for system/network to get connected is , it should have
  **physical or virtual** interface connect to it. Like in PDF, 192.168.1.10 has eth0
5. To connect two different systems in different n/w **Router** is needed
6. let us move app01 and app03 to different networks by attaching interface to them
  using `ip addr add {interfaceIP}/{subnetMask} dev {interfaceName}`.
  This Add address interfaceIP with netmask 24 to device interfaceName
7. For our situation this is how it stands
  ```
  app01: sudo ip addr add 172.16.238.15/24 dev eth0
  app03: sudo ip addr add 172.16.239.15/24 dev eth0
  ```
8. IP addr shows the current config on two apps .

    *ip addr of app01*
    ```
    inet 172.16.238.11/24 brd 172.16.238.255 scope global eth0
    valid_lft forever preferred_lft forever
    inet 172.16.238.15/24 scope global secondary eth0
    valid_lft forever preferred_lft forever
    ```
    *ip addr of app03*
    ```
    inet 172.16.238.13/24 brd 172.16.238.255 scope global eth0
    valid_lft forever preferred_lft forever
    inet 172.16.239.15/24 scope global eth0
    valid_lft forever preferred_lft forever
    ```
9. Now since app01 and app03 are in different subnet we won't be able to PING them
  from each other. We have Jumphost (172.16.238.10) which is also out of app03 n/w
  so we won't be able to do SSH to app03 now. How to access the app03?
10. In order to ensure access to app03, we would need to setup gateway
  and routing information
11. Now if we expose 239 network interface on our JUMPHOST, our jumphost will be able
  to connect to both the networks via router.
12. Let us add interface for 239 n/w in JUMPHOST `sudo ip addr add 172.16.239.10/24 dev eth0`
    ip addr table of jumphost
    ```
    inet 172.16.238.10/24 brd 172.16.238.255 scope global eth0
    valid_lft forever preferred_lft forever
    inet 172.16.239.10/24 scope global eth0
    valid_lft forever preferred_lft forever
    ```
13. Now we will be able to ssh to app03
14. Next question, how to communicate b/w app01 and app03 as they are on different n/w
15. For this purpose, we have to tell our jumphost to act as router and add routing entry
  in route table of JUMPHOST for respective n/w
16. To connect to the external app03 we would add entry for app01 n/w via JUMPHOST
  and same for app01. Current route table stands like below

  app01 route
  ```
  Kernel IP routing table
  Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
  172.16.238.0    0.0.0.0         255.255.255.0   U     0      0        0 eth0
  172.17.0.0      0.0.0.0         255.255.0.0     U     0      0        0 eth1
  ```
  app03 route
  ```
  Kernel IP routing table
  Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
  172.16.239.0    0.0.0.0         255.255.255.0   U     0      0        0 eth0
  172.17.0.0      0.0.0.0         255.255.0.0     U     0      0        0 eth1
  ```
17. Let us add new entries for jumphost

  app01
  `sudo ip route add 172.16.239.0/24 via 172.16.238.10`
  ```
  Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
  172.16.238.0    0.0.0.0         255.255.255.0   U     0      0        0 eth0
  172.16.239.0    jump_host.devop 255.255.255.0   UG    0      0        0 eth0
  172.17.0.0      0.0.0.0         255.255.0.0     U     0      0        0 eth1
  ```

  app03
  `sudo ip route add 172.16.238.0/24 via 172.16.239.10`
  ```
  Kernel IP routing table
  Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
  172.16.238.0    172.16.239.10   255.255.255.0   UG    0      0        0 eth0
  172.16.239.0    0.0.0.0         255.255.255.0   U     0      0        0 eth0
  172.17.0.0      0.0.0.0         255.255.0.0     U     0      0        0 eth1
  ```
18. Now to access 238 n/w from app03, our gateway is jumphost exposing eth0 interface
  ```
  [thor@app01 ~]$ sudo ping app03
  PING app03 (172.16.239.15) 56(84) bytes of data.
  64 bytes from app03 (172.16.239.15): icmp_seq=1 ttl=63 time=0.125 ms
  64 bytes from app03 (172.16.239.15): icmp_seq=2 ttl=63 time=0.113 ms
  64 bytes from app03 (172.16.239.15): icmp_seq=3 ttl=63 time=0.101 ms
  ```


# DNS

1. To resolve names against IPs. We use DNS to access/ping any particular n/w with
  domain name instead of using it's IP. Like staging.avipulse.vmware.com instead
  of 10.102.96.140 everytime

2. We can add entry in `/etc/hosts` to specify the "domainName domainIP" pair.
  Entry in file is the source of truth for host.

3. If there are N machines on n/w then all should have same /etc/hosts entries in
  order to discover each other using domain name. But this is not scalable as
  we one of the IP of m/c got changed, we have to update all of N files

4. So we should have centralised server 'DNS server', to handle this task per n/w.
  We can specify the information for look up to DNS Server in `/etc/resolv.conf`
  ```
  nameserver DNSServerIP
  ```
5. Now everytime m/c comes across domain Name which it can't resolve it would
  redirect that task to DNSServer configured.

6. Default behaviour for nameresolution is : **/etc/hosts > DNS Server**. m/c
  will give preference to /etc/hosts entry over DNS entry everytime by default.
  We can change this using `/etc/nsswitch.conf`
  ```
  hosts:          files dns
  ```
  By default it shows, for hosts it will use FILES first and then DNS

7. If we want to access any domains like facebook.com which our DNS is not aware
  about, we can forward all such requests to Google Server (8.8.8.8). Add forwad
  all entry in DNS Server config
  ```
  Foward All to 8.8.8.8
  ```

### How to resolve short domain name

1. Let's say in our internal DNS we have entry for HostName 192.168.1.10
  ```
  192.168.1.10 dummy.vmware.com
  ```
2. Now if we try to do ping on this it works. But we have to type complete domain
  name each time we have to access. If we are inside our internal n/w it does not
  makes sense to access each of hosts with common suffix. We should be able to do
  something like this
  `ping dummy`
3. For this we would need [search] block in our hosts to add common suffix
  ```
  nameserver DNSServerIP
  search vmware.com eng.vmware.com
  ```
  This ensures that if we do `ping dummy` it will search for `dummy.vmware.com`
  or `dummy.eng.vmware.com` whichever matches

### Nslookup

1. NSLook UP can be used to discover the domain names on networks similar to Ping
2. But NsLookUp doesn't check `/etc/hosts` entry , it will checks DNSServer entries
  so for local testing ,it is not advised to use this

### Dig
1. Dig can also be used to get extra information regarding dns lookup. `dig domainname`
