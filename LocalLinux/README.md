
run latest cloudflare warp on local linux

```
curl -O https://raw.githubusercontent.com/ExtremeDot/GoldenOne_MENU/main/extraMenu.sh && chmod +x extraMenu.sh

mv extraMenu.sh /bin/eMenu && chmod +x /bin/eMenu
```

```
eMenu
```

![image](https://user-images.githubusercontent.com/120102306/230684212-7440218a-3fe2-49a7-8fe8-5070c97b6a55.png)



```
11) CloudFlare WARP+
```

```
 2) Install Cloudflare Warp+
```

```
3) Register and Enable New Account
```

```
4) Set New Licence Key
```

### TEST PORT

```
curl -4 myip.wtf/json --socks5 socks5://localhost:
```

### ROUTING

```
nano /ExtremeDOT/dhcp_route.sh
```

```
#!/bin/bash

# Update This Values ###################
VPNNETNAME=CloudflareWARP
BRIDGE_NAME=bridge1
VPN_TABLE=900
# FINISH ##############################

###################################################
sleep 15
echo "1" > /proc/sys/net/ipv4/ip_forward
sysctl -w net.ipv4.ip_forward=1
sysctl -p
/usr/bin/systemctl start iptables
sleep 3

###################################################
## DONT TOUCH BELOW ###############################
DEF_TABLE=$VPN_TABLE
# Automated  VPN IP
VPNIP=$(ip -4 addr | grep $VPNNETNAME | sed -ne 's|^.* inet \([^/]*\)/.* scope global.*$|\1|p')
sleep 1
VPN_IP_BASE=$(echo "$VPNIP" | cut -d "." -f1-3)
VPNIP_GW=$(echo "$VPN_IP_BASE".1)
VPNIP_NETWORK=$(echo "$VPN_IP_BASE".0/24)

# Automated  Bridge IP
BRIDGEIP=$(ip -4 addr | grep $BRIDGE_NAME | sed -ne 's|^.* inet \([^/]*\)/.* scope global.*$|\1|p')
sleep 1
BRIDGE_IP_BASE=$(echo "$BRIDGEIP" | cut -d "." -f1-3)
BRIDGE_GW=$(echo "$BRIDGE_IP_BASE".1)
BRIDGE_NETWORK=$(echo "$BRIDGE_IP_BASE".0/24)

#### Automated Routing PART
sleep 15
/sbin/ip route add $BRIDGE_NETWORK dev $BRIDGE_NAME table $DEF_TABLE
sleep 1
/sbin/ip route add $VPNIP_NETWORK dev $VPNNETNAME table $DEF_TABLE
sleep 1
/sbin/ip route add default via $VPNIP_GW dev $VPNNETNAME table $DEF_TABLE
sleep 2

#/sbin/ip route show table $DEF_TABLE
/sbin/ip rule add iif $VPNNETNAME lookup $DEF_TABLE
sleep 1
/sbin/ip rule add iif $BRIDGE_NAME lookup $DEF_TABLE
sleep 2

#/sbin/ip rule | grep $DEF_TABLE
/sbin/iptables -t nat -A POSTROUTING -s $BRIDGE_NETWORK -o $VPNNETNAME -j MASQUERADE
```

### Corntab autostar at reboot

```
@reboot bash /ExtremeDOT/dhcp_route.sh
```

