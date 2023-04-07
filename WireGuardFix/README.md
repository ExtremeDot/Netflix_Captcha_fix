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

***
### Troubleshooting

  - check if routing is works

```
iptables-save -t nat
```

![image](https://user-images.githubusercontent.com/120102306/230672984-0b1e733e-f4e9-46b3-bc31-65fabbf4058e.png)

```
-A POSTROUTING -s 10.36.88.0/24 -o t2s_warp -j MASQUERADE
```

  - ip rule
  
  ```
  ip rule | grep 700
  ```
  it must return this
  
  ```
  32765:  from 10.36.88.0/24 lookup 700
  ```
  
  - routing table

```
ip route show table 700
```
   the result must be like this
   
   ```
   default via 10.40.80.1 dev t2s_warp proto static
   ```
   
   
  




