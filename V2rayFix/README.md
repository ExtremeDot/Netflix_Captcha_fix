
***

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
