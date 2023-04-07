
https://github.com/fscarmen/warp

```
wget -N https://raw.githubusercontent.com/fscarmen/warp/main/menu.sh && bash menu.sh
```


![image](https://user-images.githubusercontent.com/120102306/230666365-0c73e725-8ceb-4317-aafa-c51e2ea9006d.png)

***

```
5.  Install CloudFlare Client and set mode to Proxy (bash menu.sh c) 
```

enter the id if you have 

enter the proxy port you wanna run on it

![image](https://user-images.githubusercontent.com/120102306/230666650-eb7033cc-e07b-42b1-8427-ae81ea80b946.png)

mine is = `31596`

***

check the proxy port

mine was 31596

```
curl -4 myip.wtf/json --socks5 socks5://localhost:31596
```

![image](https://user-images.githubusercontent.com/120102306/230667098-b2951e38-4700-43a6-b58c-3a010bcfbb42.png)


***
### commands

```
Run again with warp [option] [lisence], such as
 
 warp h (help)
 warp n (Get the WARP IP)
 warp o (Turn off WARP temporarily)
 warp u (Turn off and uninstall WARP interface and Socks5 Linux Client)
 warp b (Upgrade kernel, turn on BBR, change Linux system)
 warp a (Change account to Free, WARP+ or Teams)
 warp p (Getting WARP+ quota by scripts)
 warp v (Sync the latest version)
 warp r (Connect/Disconnect WARP Linux Client)
 warp 4/6 (Add WARP IPv4/IPv6 interface)
 warp d (Add WARP dualstack interface IPv4 + IPv6)
 warp c (Install WARP Linux Client and set to proxy mode)
 warp l (Install WARP Linux Client and set to WARP mode)
 warp i (Change the WARP IP to support Netflix)
 warp e (Install Iptables + dnsmasq + ipset solution)
 warp w (Install WireProxy solution)
 warp y (Connect/Disconnect WireProxy socks5)
 warp s 4/6/d (Set stack proiority: IPv4 / IPv6 / VPS default)
 ```
 
