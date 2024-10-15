## Implement eBGP for IPv4

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


## Implement SPAN Technologies (Switch Port Analyzer)
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
