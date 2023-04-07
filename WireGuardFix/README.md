# Server Details:

  - WireGuard Server Installed with this specification:
    - Wireguard Server Script:   Angristan
    - Wireguard Interface Name:  wg0
    - IPv4 address for wg0:      10.36.88.1
 
 ***
 
```
nano /ExtremeDot/warp2wg.sh 
```

```
#!/bin/bash
echo "/ExtremeDot/warp2wg.sh START" >> /runn
echo "1" > /proc/sys/net/ipv4/ip_forward
sysctl -w net.ipv4.ip_forward=1
sysctl -p
/usr/bin/systemctl start iptables
sleep 10

IP_BIN=/sbin/ip
IPTABLESBIN=/usr/sbin/iptables
WIREGUARD_HOP=10.36.88.0/24
TABLE_VPN=700
WARP_GW=10.40.80.1
WARP_INTERFACE=t2s_warp

$IP_BIN rule add from $WIREGUARD_HOP lookup $TABLE_VPN
sleep 1
$IP_BIN route add default via $WARP_GW dev $WARP_INTERFACE proto static table $TABLE_VPN
sleep 1
$IPTABLESBIN -t nat -A POSTROUTING -s $WIREGUARD_HOP -o $WARP_INTERFACE -j MASQUERADE

```

```
chmod +x /ExtremeDot/warp2wg.sh 
```

test the routing

```
bash /ExtremeDot/warp2wg.sh
```

### add to crontab
```
crontab -e
```
@reboot sudo bash /ExtremeDot/warp2wg.sh
```

