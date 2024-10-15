#Implement eBGP for IPv4

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


