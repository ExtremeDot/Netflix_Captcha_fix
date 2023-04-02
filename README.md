### Netflix and Goofle reCaptcha fix for v2ray Server


```sh
bash <(curl -Lso- https://git.io/oneclick)
```

Select English

```sh
1. Install linux kernel,  bbr plus kernel, WireGuard and Cloudflare WARP. Unlock Netflix geo restriction and avoid Google reCAPTCHA
```

```sh
21. Install warp-go by fscarmen. Enable IPv6, avoid Google reCAPTCHA and unlock Netflix geo restriction
```

```sh
1.English (default) 
```
![image](https://user-images.githubusercontent.com/120102306/229346262-34533a24-37a8-45ce-a97c-dc53aa214afc.png)





```sh
4. Add WARP IPv6 global network interface for IPv4 only, IPv6 priority (bash warp-go.sh 6)

```

![image](https://user-images.githubusercontent.com/120102306/229346294-3a100d5d-c697-40d9-a97f-528a1ab32646.png)

```sh
 3. Use free account (default)
 
```


* note the port of running warp 

```sh
extremeDOT
option 84
test the port with 82 option
```

edit Outband configs on v2ray server
add running port on it, ex: port = 31596

```sh
{
            "tag": "WARP_OUT",
            "protocol": "socks",
            "settings": {
                "servers": [
                    {
                        "address": "127.0.0.1",
                        "port": 31596
                    }
                ]
            },
            "streamSettings": {
                "network": "tcp"
            }
        }
```

the final result for outbands is 

```sh
[
  {
    "protocol": "freedom",
    "settings": {}
  },
  {
    "protocol": "blackhole",
    "settings": {},
    "tag": "blocked"
  },
  {
    "tag": "WARP_OUT",
    "protocol": "socks",
    "settings": {
      "servers": [
        {
          "address": "127.0.0.1",
          "port": 31596
        }
      ]
    },
    "streamSettings": {
      "network": "tcp"
    }
  }
]
```

***

### ROUTING SECTION

```sh
{
                "type": "field",
                "outboundTag": "WARP_OUT",
                "domain": ["geosite:netflix", "geosite:disney", "geosite:spotify", "geosite:youtube", "geosite:hulu", "geosite:hbo", "geosite:bbc", "geosite:fox", "geosite:google" ]
			}
```

***

### THE FINAL

```sh
{
  "api": {
    "services": [
      "HandlerService",
      "LoggerService",
      "StatsService"
    ],
    "tag": "api"
  },
  "inbounds": [
    {
      "listen": "127.0.0.1",
      "port": 62789,
      "protocol": "dokodemo-door",
      "settings": {
        "address": "127.0.0.1"
      },
      "tag": "api"
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    },
    {
      "protocol": "blackhole",
      "settings": {},
      "tag": "blocked"
    },
    {
      "tag": "WARP_OUT",
      "protocol": "socks",
      "settings": {
        "servers": [
          {
            "address": "127.0.0.1",
            "port": 31596
          }
        ]
      },
      "streamSettings": {
        "network": "tcp"
      }
    }
  ],
  "policy": {
    "system": {
      "statsInboundDownlink": true,
      "statsInboundUplink": true
    }
  },
  "routing": {
    "rules": [
      {
        "inboundTag": [
          "api"
        ],
        "outboundTag": "api",
        "type": "field"
      },
      {
        "type": "field",
        "outboundTag": "WARP_OUT",
        "domain": [
          "geosite:netflix",
          "geosite:disney",
          "geosite:spotify",
          "geosite:youtube",
          "geosite:hulu",
          "geosite:hbo",
          "geosite:bbc",
          "geosite:fox",
          "geosite:google"
        ]
      },
      {
        "ip": [
          "geoip:private"
        ],
        "outboundTag": "blocked",
        "type": "field"
      },
      {
        "outboundTag": "blocked",
        "protocol": [
          "bittorrent"
        ],
        "type": "field"
      }
    ]
  },
  "stats": {}
}
```
