## P1 Implement eBGP for IPv4

```bash
#roter1
router bgp 100
network 192.168.1.0
network 192.168.2.0
neighbor 192.168.2.2 remote-as 200
neighbor 192.168.3.2 remote-as 200

#router2
router bgp 200
network 192.168.3.0
network 192.168.2.0
neighbor 192.168.2.1 remote-as 100
neighbor 192.168.1.2 remote-as 100
```


## P2 Implement SPAN Technologies (Switch Port Analyzer)
```bash
#router0
int loopback 0
ip add 10.10.20.1 255.255.255.0
no shut
exit
int fa0/0
ip add 192.168.0.2 255.255.255.0
no shut
exit

#router1
conf t
int loopback 0
ip add 10.10.10.1 255.255.255.0
no shut
exit
int fa0/0
ip add 192.168.0.1 255.255.255.0
no shut 
exit

#router2
en
conf t
int loopback 0
ip add 10.10.30.1 255.255.255.0
no shut
exit
int Serial0/0/0
ip add 172.168.1.1 255.255.255.252
no shut
exit
int fa0/0
ip add 192.168.0.3 255.255.255.0
no shut
exit

#router3
conf t
int loopback 0
ip add 10.10.40.1 255.255.255.0
no shut
exit
int Serial0/0/0
ip add 172.168.1.2 255.255.255.252
no shut
exit

#ospf router0
conf t
router ospf 1
router-id 10.2.2.2
network 10.10.20.0 0.0.0.255 area 0
network 192.168.0.0 0.0.0.255 area 0
exit
exit
wr

#ospf routuer1
conf t
router ospf 1
router-id 10.1.1.1
network 10.10.10.0 0.0.0.255 area 0
network 192.168.0.0 0.0.0.255 area 0
exit
exit
wr

#ospf router2
conf t
router ospf 1
router-id 10.3.3.3
network 10.10.30.0 0.0.0.255 area 0
network 172.168.0.0 0.0.0.3 area 0
network 192.168.0.0 0.0.0.255 area 0
exit
exit
wr

#ospf router3
conf t
router ospf 1
router-id 10.4.4.4
network 10.10.40.0 0.0.0.255 area 0
network 172.168.0.0 0.0.0.3 area 0
exit
exit
wr

#check will all router
sh ip route

```


## P4 Implement SPAN Technologies (Switch Port Analyzer)
```bash
#pc0
ip 10.1.1.100 255.0.0.0

#server0
ip 10.1.1.200 255.0.0.0

#switch0
en
conf t
monitor session 1 source int fa0/1
monitor session 1 destination int fa0/3

#switch
sh monitor
```

## P6 Implement Standard IPv4 ACLs

```bash
Configure R0
Router#conf t 
Router(config)#host R0 
R0(config)#int fa0/0 
R0(config-if)#ip add 192.168.3.1 255.255.255.0 
R0(config-if)#no shut 
R0(config-if)#exit 
R0(config)#int fa0/1 
R0(config-if)#ip add 192.168.1.1 255.255.255.0 
R0(config-if)#no shut 
R0(config-if)#exit 

Configure R1 
Router>en 
Router#conf t 
Router(config)#host R1 
R1(config)#int fa0/0 
R1(config-if)#ip add 192.168.3.2 255.255.255.0 
R1(config-if)#no shut 
R1(config-if)#exit 
R1(config)#int fa0/1 
R1(config-if)#ip add 192.168.2.1 255.255.255.0 
R1(config-if)#no shut 
R1(config-if)#exit 

Configure static route on R0 
R0(config)#ip route 192.168.2.0 255.255.255.0 192.168.3.2 
R0(config)#exit 

Configure static route on R1 
R1(config)#ip route 192.168.1.0 255.255.255.0 192.168.3.1 
R1(config)#exit

PC1 open cmd ping 192.168.1.100

PC2 open cmd ping 192.168.1.100 (this wont happen coz i blocked in later command)

Configure access list on R0 
R0>en 
R0#conf t 
Enter configuration commands, one per line. End with CNTL/Z. 
R0(config)#access-list 1 deny 192.168.2.101 0.0.0.0 
R0(config)#access-list 1 permit any 
R0(config)#int fa0/1 
R0(config-if)#ip access-group 1 out 
R0(config-if)# 
R0#
```

## P6 Implement Extended IPv4 ACLs

```bash
1. Configure R0
en
conf t
host R0
int fa0/0
ip add 192.168.3.1 255.255.255.0
no shut
exit
int fa0/1
ip add 192.168.1.1 255.255.255.0
no shut
exit
int eth0/0/0
ip add 192.168.4.1 255.255.255.0
no shut exit

2. Configure R1
en
conf t
host R1
int fa0/0
ip add 192.168.3.2 255.255.255.0
no shut
exit
int fa0/1
ip add 192.168.2.1 255.255.255.0
no shut exit

3. Configure static route on R0
ip route 192.168.2.0 255.255.255.0 192.168.3.2
exit

4.Configure static route on R1
ip route 192.168.1.0 255.255.255.0 192.168.3.1
ip route 192.168.4.0 255.255.255.0 192.168.3.1 
exit

5. Configure access list on R0
conf t
access-list 1 deny host 192.168.2.100
access-list 1 deny host 192.168.2.101
access-list 1 permit any 
int fa0/1
ip access-group 1 out

6. Configure access list on R1
en
conf t
access-list 100 deny ip 192.168.2.100 0.0.0.0 192.168.1.0 0.0.0.255
access-list 100 permit ip any
int fa0/1
ip access-group 100 in
```

## P7 Implement NAT

```bash
R0:
ip fe0/0 - 10.10.10.1
se2/0 - 192.162.10.1

R1:
ip fe0/0 - 20.20.20.1
se2/0 - 192.162.10.2

pc0:
ip - 10.10.10.2
subnet - 255.0.0.0
DG: 10.10.10.1

server0:
ip - 10.10.10.3
subnet - 255.0.0.0
DG - 10.10.10.1

pc1:
ip - 20.20.20.2
DG - 20.20.20.1

R0;
ip nat inside source static 10.10.10.2 50.50.50.2
ip nat inside source static 10.10.10.2 50.50.50.3
interface fa0/0
ip nat inside
exit
interface fa1/0
ip nat inside
exit
interface serial2/0
ip nat outside
exit 


R1:
ip nat inside source static 20.20.20.2 60.60.60.2
interface fa0/0
ip nat inside
exit
interface serial2/0
ip nat outside
exit

R0:
ip route 60.0.0.0 255.0.0.0 192.162.10.2
exit

R1:
ip route 50.0.0.0 255.0.0.0 192.162.10.1
exit

R0:
show ip route

R1:
show ip route

PC0:
ping 60.60.60.2
ping 20.20.20.2

PC1:
ping 50.50.50.2
ping 10.10.10.2
```

## P8 Implement a GRE Tunnel 

```bash
Configure Router Ro
en
conf t
int gig0/0
ip address 1.1.1.1 255.0.0.0
no shut
int lo 0
ip add 10.1.1.1 255.255.255.0
no shut
exit

Configure Router R1
en
conf t
int gig0/0
ip add 1.1.1.2 255.0.0.0
no shut
int gig0/1
ip add 2.2.2.1 255.0.0.0
no shut
int lo 0
ip add 20.1.1.1 255.255.255.0
no shut
exit
hostname ISP

Configure Router R2
en
conf t
int gig0/1
ip add 2.2.2.2 255.0.0.0
no shut
int lo 0
ip add 30.1.1.1 255.255.255.0
no shut
exit

Configure GRE on R0
interface tunnel 1
tunnel source gig0/0
tunnel destination 2.2.2.2
ip add 192.168.13.1 255.255.255.0
no shut
end

Configure GRE on R2
interface tunnel 1
tunnel source gig0/1
tunnel destination 1.1.1.1
ip add 192.168.13.3 255.255.255.0
no shut
end

Configure Static route on R0:
conf t
ip route 2.0.0.0 255.0.0.0 1.1.1.2

 
Configure Static route on R2:
ip route 1.0.0.0 255.0.0.0 2.2.2.1

Check connectivity from R0:
ping 192.168.13.3

Check connectivity from R2:
ping 192.168.13.1
```
