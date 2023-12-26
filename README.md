# geosite RU

geoip contains databases of IP addresses of Russia and Belarus(MaxMind), cloudflare, cloudfront, facebook, fastly, google, netflix, telegram, twitter.

## Example Sing-box route rules geosite.db
```json
"route": {
    "geosite": {
        "path": "/tmp/geosite.db",
        "download_url": "https://github.com/abdulradio/sing-geosite/releases/latest/download/geosite.db",
        "download_detour": "proxy"
    },
  "rules": [ 
    {
      "geosite": [
        "category-ads-all"
      ],
      "outbound": "block"
    },  
    {   
      "geosite": "category-ru-news",
      "outbound": "proxy"
    },  
    {
      "geosite": [
        "geolocation-ru",
        "youtube",
        "pinterest",
        "tld-ru"
      ],
        "outbound": "direct"
    }
  ],
  "final": "proxy"
}
```

## Example Sing-box DNS rules geosite.db
```json
"dns": {
    "servers": [
      {
          "tag": "dns-remote",
          "address": "tls://1.1.1.1/dns-query",
          "address_resolver": "dns-local",
          "strategy": "ipv4_only"
      },
      {
          "tag": "dns-direct",
          "address": "tls://1.1.1.1/dns-query",
          "address_resolver": "dns-local",
          "strategy": "ipv4_only",
          "detour": "direct"
      },
      {
          "tag": "dns-local",
          "address": "local",
          "strategy": "ipv4_only",
          "detour": "direct"
     },
     {
          "tag": "dns-block",
          "address": "rcode://success"
     }
   ],
   "rules": [
     {
         "geosite": "category-ads-all",
         "server": "dns-block"
     },
     {
         "geosite": [
           "tld-ru",
           "geolocation-ru"
         ],
         "server": "dns-direct"
     } 
   ],
   "independent_cache": true
}
```
