```
extermeDOT
```

![image](https://user-images.githubusercontent.com/120102306/230674097-886e113f-1e27-446b-9657-7e0412bb7df8.png)



option 54

```
54) Install BADVPN-TUN2SOCKS & TUN2SOCKS
```

***

```
mkdir -p /ExtremeDot/
nano /ExtremeDot/warp2ts.sh
```

```
/usr/bin/ip tuntap add mode tun dev t2s_warp
sleep 2
/usr/bin/ip addr add 10.40.80.1/15 dev t2s_warp
sleep 2
/usr/bin/ip link set dev t2s_warp up
sleep 2
ifconfig >> /runn
/usr/bin/tun2socks -device t2s_warp -proxy socks5://127.0.0.1:31596

```

```
chmod +x /ExtremeDot/warp2ts.sh
```

### Add to crontab

```
crontab -e 
```

```
@reboot sudo bash /ExtremeDot/warp2ts.sh
```

