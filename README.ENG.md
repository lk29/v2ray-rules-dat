# Introduction

[**V2Ray**](https://github.com/v2fly/v2ray-core) Enhanced version of routing rule file, can replace V2Ray official `geoip.dat` and `geosite.dat`, compatible with [Shadowsocks-windows](https://github.com/shadowsocks/shadowsocks-windows), [Xray-core](https://github.com/XTLS/Xray-core), [Trojan-Go](https://github.com/p4gefau1t/trojan-go) and [leaf](https://github.com/eycorsican/leaf). Use GitHub Actions to automatically build at 6 am Beijing time every day to keep the rules up to date.

## Rules file generation method

### geoip.dat

- Generated via repository [@Loyalsoldier/geoip](https://github.com/Loyalsoldier/geoip)
- The global IP addresses (IPv4 and IPv6) are derived from [MaxMind GeoLite2](https://dev.maxmind.com/geoip/geoip2/geolite2/), and the IPv4 addresses under the `CN` (Mainland China) category are fused with [ ipip.net](https://github.com/17mon/china_ip_list) and [@gaoyifan/china-operator-ip](https://github.com/gaoyifan/china-operator-ip), `CN`( The IPv6 addresses under the Mainland China) category are a fusion of [MaxMind GeoLite2](https://dev.maxmind.com/geoip/geoip2/geolite2/) and [@gaoyifan/china-operator-ip](https://github.com/gaoyifan/china-operator-ip)
- New category (convenient for users with special needs):
   - `geoip:cloudflare`
   - `geoip:cloudfront`
   - `geoip:facebook`
   - `geoip:fastly`
   - `geoip:google`
   - `geoip:netflix`
   - `geoip:telegram`
   - `geoip:twitter`

> Want to customize the `geoip.dat` file? Check out the repository [@Loyalsoldier/geoip](https://github.com/Loyalsoldier/geoip).

### geosite.dat

- Based on [@v2fly/domain-list-community/data](https://github.com/v2fly/domain-list-community/tree/master/data) data, via warehouse [@Loyalsoldier/domain-list-custom ](https://github.com/Loyalsoldier/domain-list-custom) generated
- **Join a large number of Chinese mainland domains, Apple domains and Google domains**:
   - [@felixonmars/dnsmasq-china-list/accelerated-domains.china.conf](https://github.com/felixonmars/dnsmasq-china-list/blob/master/accelerated-domains.china.conf) added to In the `geosite:cn` category
   - [@felixonmars/dnsmasq-china-list/apple.china.conf](https://github.com/felixonmars/dnsmasq-china-list/blob/master/apple.china.conf) added to `geosite:geolocation-!cn` category (If you want to directly connect the Apple domain name in this file, please refer to the following [routing configuration method of geosite](https://github.com/Loyalsoldier/v2ray-rules-dat#geositedat-1)）
   - [@felixonmars/dnsmasq-china-list/google.china.conf](https://github.com/felixonmars/dnsmasq-china-list/blob/master/google.china.conf) added to `geosite:geolocation-!cn` category (If you want the Google domain name in this file to be directly connected, please refer to the following [routing configuration method of geosite](https://github.com/Loyalsoldier/v2ray-rules-dat#geositedat-1)）
- **Join GFWList Domains**:
   - Generated from repository [@cokebar/gfwlist2dnsmasq](https://github.com/cokebar/gfwlist2dnsmasq) based on [@gfwlist/gfwlist](https://github.com/gfwlist/gfwlist) data
   - Added to `geosite:gfw` category for users who are used to PAC mode and want to use [GFWList](https://github.com/gfwlist/gfwlist)
   - Also added to `geosite:geolocation-!cn` category
- **Add blocked domains detected by Greatfire Analyzer**:
   - Obtain [Greatfire Analyzer](https://zh.greatfire.org/analyzer) through the warehouse [@Loyalsoldier/cn-blocked-domain](https://github.com/Loyalsoldier/cn-blocked-domain) detected of domains blocked in mainland China
   - Added to the `geosite:greatfire` category, which can be used together with the above `geosite:gfw` category to achieve the effect of a domain name blacklist
   - Also added to `geosite:geolocation-!cn` category
- **Join EasyList and EasyListChina advertising domains**: Obtain through [@AdblockPlus/EasylistChina+Easylist.txt](https://easylist-downloads.adblockplus.org/easylistchina+easylist.txt) and add to `geosite:category -ads-all` category
- **Join AdGuard DNS Filter advertising domain name**: Obtain and add to [@AdGuard/DNS-filter](https://kb.adguard.com/en/general/adguard-ad-filters#dns-filter) `geosite:category-ads-all` category
- **Join Peter Lowe Ads & Privacy Tracking Domains**: Acquired by [@PeterLowe/adservers](https://pgl.yoyo.org/adservers) and added to the `geosite:category-ads-all` category
- **Join Dan Pollock Ads Domains**: Acquired via [@DanPollock/hosts](https://someonewhocares.org/hosts) and added to `geosite:category-ads-all` category
- **Join Windows operating system related system upgrade and privacy tracking domain name**:
   - Based on [@crazy-max/WindowsSpyBlocker](https://github.com/crazy-max/WindowsSpyBlocker/tree/master/data/hosts) data
   - [**Use with caution**] Privacy tracking domain name used by Windows operating system[@crazy-max/WindowsSpyBlocker/hosts/spy.txt](https://github.com/crazy-max/WindowsSpyBlocker/blob/master/data/hosts/spy.txt) into the `geosite:win-spy` category
   - [**Use with caution**] System upgrade domain name used by Windows operating system[@crazy-max/WindowsSpyBlocker/hosts/update.txt](https://github.com/crazy-max/WindowsSpyBlocker/blob/master/data/hosts/update.txt) into the `geosite:win-update` category
   - [**Use with caution**] Additional privacy tracking domain name for Windows OS [@crazy-max/WindowsSpyBlocker/hosts/extra.txt](https://github.com/crazy-max/WindowsSpyBlocker/blob/master/data/hosts/extra.txt) into the `geosite:win-extra` category
   - For how to use these three categories, please refer to the following [routing configuration method of geosite](https://github.com/Loyalsoldier/v2ray-rules-dat#geositedat-1)
- **Custom direct connection, proxy and advertising domain names can be added**: Due to the slow update of the upstream domain name list or the lack of some domain names, a list of **domain names that need to be added** is introduced. The three files `direct.txt`, `proxy.txt` and `reject.txt` in [`hidden branch`](https://github.com/Loyalsoldier/v2ray-rules-dat/tree/hidden), Separately store custom direct connection, proxy, and advertising domain names that need to be added, and finally add them to `geosite:cn`, `geosite:geolocation-!cn` and `geosite:category-ads-all` categories
- **Custom direct connection, proxy and advertising domain names can be removed**: Since there are domain names that need to be removed in the upstream domain name list, a list of **Domain Names that need to be removed** is introduced. Three files `direct-need-to-remove.txt`, `proxy-need-to-remove.txt` and `reject-need-to-remove.txt`, store custom needs from `direct-list` (directly connected domain name list), `proxy-list` (proxy domain name list) and `reject-list` (list of ad domains) Domains to remove

## How to download and use rule files

**download link**:

> If the domain `raw.githubusercontent.com` cannot be accessed, the second address (`cdn.jsdelivr.net`) can be used, but there will be a 12-hour delay in content updates.

- **geoip.dat**:
   - [https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geoip.dat](https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geoip.dat)
   - [https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/geoip.dat](https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/geoip.dat)
- **geosite.dat**:
   - [https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geosite.dat](https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geosite.dat)
   - [https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/geosite.dat](https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/geosite.dat)
- **Direct domain name list direct-list.txt**:
   - [https://raw.githubusercontent.com/Loyalsoldier/v2ray-rules-dat/release/direct-list.txt](https://raw.githubusercontent.com/Loyalsoldier/v2ray-rules-dat/release/direct-list.txt)
   - [https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/direct-list.txt](https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/direct-list.txt)
- **Proxy domain name list proxy-list.txt**:
   - [https://raw.githubusercontent.com/Loyalsoldier/v2ray-rules-dat/release/proxy-list.txt](https://raw.githubusercontent.com/Loyalsoldier/v2ray-rules-dat/release/proxy-list.txt)
   - [https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/proxy-list.txt](https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/proxy-list.txt)
- **Ad domain name list reject-list.txt**:
   - [https://raw.githubusercontent.com/Loyalsoldier/v2ray-rules-dat/release/reject-list.txt](https://raw.githubusercontent.com/Loyalsoldier/v2ray-rules-dat/release/reject-list.txt)
   - [https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/reject-list.txt](https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/reject-list.txt)
- **Apple's list of domain names that can be directly connected in mainland China apple-cn.txt**:
   - [https://raw.githubusercontent.com/Loyalsoldier/v2ray-rules-dat/release/apple-cn.txt](https://raw.githubusercontent.com/Loyalsoldier/v2ray-rules-dat/release/apple-cn.txt)
   - [https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/apple-cn.txt](https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat @release/apple-cn.txt)
- **Google's list of directly connectable domain names in mainland China google-cn.txt**:
   - [https://raw.githubusercontent.com/Loyalsoldier/v2ray-rules-dat/release/google-cn.txt](https://raw.githubusercontent.com/Loyalsoldier/v2ray-rules-dat/release/google-cn.txt)
   - [https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/google-cn.txt](https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/google-cn.txt)
- **GFWList domain name list gfw.txt**:
   - [https://raw.githubusercontent.com/Loyalsoldier/v2ray-rules-dat/release/gfw.txt](https://raw.githubusercontent.com/Loyalsoldier/v2ray-rules-dat/release/gfw.txt )
   - [https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/gfw.txt](https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/gfw.txt)
- **Greatfire Domain List greatfire.txt**:
   - [https://raw.githubusercontent.com/Loyalsoldier/v2ray-rules-dat/release/greatfire.txt](https://raw.githubusercontent.com/Loyalsoldier/v2ray-rules-dat/release/greatfire.txt )
   - [https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/greatfire.txt](https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/greatfire.txt)
- **List of privacy tracking domain names used by Windows OS win-spy.txt**:
   - [https://raw.githubusercontent.com/Loyalsoldier/v2ray-rules-dat/release/win-spy.txt](https://raw.githubusercontent.com/Loyalsoldier/v2ray-rules-dat/release/win-spy.txt)
   - [https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/win-spy.txt](https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/win-spy.txt)
- **Windows system upgrade domain name list win-update.txt**:
   - [https://raw.githubusercontent.com/Loyalsoldier/v2ray-rules-dat/release/win-update.txt](https://raw.githubusercontent.com/Loyalsoldier/v2ray-rules-dat/release/win-update.txt)
   - [https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/win-update.txt](https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/win-update.txt)
- **List of additional privacy tracking domain names used by Windows OS win-extra.txt**:
   - [https://raw.githubusercontent.com/Loyalsoldier/v2ray-rules-dat/release/win-extra.txt](https://raw.githubusercontent.com/Loyalsoldier/v2ray-rules-dat/release/win-extra.txt)
   - [https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/win-extra.txt](https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/win-extra.txt)

**How to use**:

1. Install the client for your operating system
2. Download `geoip.dat` and `geosite.dat` for this project
3. Put the downloaded `geoip.dat` and `geosite.dat` into the rule file directory of the client, and replace the original `geoip.dat` and `geosite.dat`
4. If you are using the V2Ray v4 client, please refer to the configuration below 👇👇👇

## Reference configuration (only for V2Ray v4 version)

### geoip.dat

It is configured in the same way as V2Ray official `geoip.dat`.

**Routing configuration method**:

```json
"routing": {
   "rules": [
     {
       "type": "field",
       "outboundTag": "Direct",
       "ip": [
         "223.5.5.5/32",
         "119.29.29.29/32",
         "180.76.76.76/32",
         "114.114.114.114/32",
         "geoip:cn",
         "geoip:private"
       ]
     },
     {"type": "field",
       "outboundTag": "Proxy",
       "ip": [
         "1.1.1.1/32",
         "1.0.0.1/32",
         "8.8.8.8/32",
         "8.8.4.4/32",
         "geoip:us",
         "geoip:ca",
         "geoip:telegram"
       ]
     }
   ]
}
```

### geosite.dat

It is configured in the same way as V2Ray official `geosite.dat`. Classes specific to this project compared to the official `geosite.dat` file:

- `geosite:apple-cn`: contains [@felixonmars/dnsmasq-china-list/apple.china.conf](https://github.com/felixonmars/dnsmasq-china-list/blob/master/apple.china.conf) file, for users who want to connect directly to the Apple domain name (without proxy).
- `geosite:google-cn`: contains [@felixonmars/dnsmasq-china-list/google.china.conf](https://github.com/felixonmars/dnsmasq-china-list/blob/master/google.china.conf) file, for users who want to connect directly with Google domain names (without proxying).
- [**Use with caution**] `geosite:win-spy`: Contains [@crazy-max/WindowsSpyBlocker/hosts/spy.txt](https://github.com/crazy-max/WindowsSpyBlocker/blob/master/data/hosts/spy.txt) file, for users who wish to shield Windows operating system privacy tracking domain names.
- [**Use with caution**] `geosite:win-update`: contains [@crazy-max/WindowsSpyBlocker/hosts/update.txt](https://github.com/crazy-max/WindowsSpyBlocker/blob/master/data/hosts/update.txt) file for users who wish to block automatic updates of the Windows operating system.
- [**Use with caution**] `geosite:win-extra`: Contains [@crazy-max/WindowsSpyBlocker/hosts/extra.txt](https://github.com/crazy-max/WindowsSpyBlocker/blob/master/data/hosts/extra.txt) file for users who wish to block additional privacy tracking domain names for Windows operating systems.

> ⚠️ Note: In the routing configuration, the higher the category (top), the higher the priority, so `geosite:apple-cn` and `geosite:google-cn` should be placed before `geosite:geolocation-!cn` (above) to take effect.

#### Advanced usage

v2fly/domain-list-community project [data](https://github.com/v2fly/domain-list-community/tree/master/data) Some rules in the list in the directory will be marked such as `@cn` The attribute (as shown below) means that the domain name has an access point in mainland China and can be directly connected.

```
steampowered.com.8686c.com@cn
steamstatic.com.8686c.com@cn
```

For users who want to directly connect to games in the national region of Steam, you can set the category `geosite:steam@cn` as direct connection, which means [steam](https://github.com/v2fly/domain-list-community /blob/master/data/steam) All rules (domain names) marked with `@cn` attribute in the list are set to direct connection. Similarly, since the [category-games](https://github.com/v2fly/domain-list-community/blob/master/data/category-games) list contains `steam`, `ea`, `blizzard` , `epicgames` and `nintendo` and other common game manufacturers. Set the category `geosite:category-games@cn` to direct connection, which can save a lot of server traffic.

> ⚠️ Note: In the Routing configuration, the higher the category (top), the higher the priority, so `geosite:category-games@cn` and all rules with `@cn` attribute must be placed in `geosite: The geolocation-!cn` front (upper) face will take effect.
>
> The rules (domain names) in the `category-games` list may have omissions, please pay attention to the rule hits. If you find any omissions, welcome to [issue](https://github.com/v2fly/domain-list-community/issues) feedback on the project v2fly/domain-list-community.

#### Configuration reference below 👇👇👇

**Whitelist Mode Routing Configuration Method**:

```json
"routing": {
   "rules": [
     {
       "type": "field",
       "outboundTag": "Reject",
       "domain": ["geosite:category-ads-all"]
     },
     {
       "type": "field",
       "outboundTag": "Direct",
       "domain": [
         "geosite:private",
         "geosite:apple-cn",
         "geosite:google-cn",
         "geosite:tld-cn",
         "geosite:category-games@cn"
       ]
     },
     {
       "type": "field",
       "outboundTag": "Proxy",
       "domain": ["geosite:geolocation-!cn"]
     },
     {
       "type": "field",
       "outboundTag": "Direct",
       "domain": ["geosite:cn"]
     },
     {
       "type": "field",
       "outboundTag": "Proxy",
       "network": "tcp,udp"
     }
   ]
}
```

** Blacklist mode Routing configuration method: **

```json
"routing": {
   "rules": [
     {
       "type": "field",
       "outboundTag": "Reject",
       "domain": ["geosite:category-ads-all"]
     },
     {
       "type": "field",
       "outboundTag": "Proxy",
       "domain": ["geosite:gfw", "geosite:greatfire"]
     },
     {
       "type": "field",
       "outboundTag": "Proxy",
       "ip": ["geoip:telegram"]
     },
     {
       "type": "field",
       "outboundTag": "Direct",
       "network": "tcp,udp"
     }
   ]
}
```

**DNS configuration method**:

```json
"dns": {
   "hosts": {
     "dns.google": "8.8.8.8",
     "dns.pub": "119.29.29.29",
     "dns.alidns.com": "223.5.5.5",
     "geosite:category-ads-all": "127.0.0.1"
   },
   "servers": [
     {
       "address": "https://1.1.1.1/dns-query",
       "domains": ["geosite:geolocation-!cn"],
       "expectIPs": ["geoip:!cn"]
     },
     "8.8.8.8",
     {
       "address": "114.114.114.114",
       "port": 53,
       "domains": ["geosite:cn", "geosite:category-games@cn"],
       "expectIPs": ["geoip:cn"],
       "skipFallback": true
     },
     {
       "address": "localhost",
       "skipFallback": true
     }
   ]
}
```

### V2Ray v4 version client configuration for personal use (not applicable to V2Ray v5 and newer versions)

Precautions:

- Since the DNS of the following client configuration uses the `skipFallback` option, you must use v4.37.2 or newer version of [V2Ray](https://github.com/v2fly/v2ray-core/releases)
- The following client configuration enables V2Ray to enable SOCKS proxy (listen to port 1080) and HTTP proxy (listen to port 2080) locally, allowing other devices in the LAN to connect and use the proxy
- All BT traffic is directly connected (actually, some BT traffic will still go through the proxy, if the service provider prohibits BT download, please do not set up a proxy for the download software)
- Finally, all requests and traffic that do not hit any routing rules will go through the proxy
- The configuration in the first curly braces in `outbounds` is the configuration of the V2Ray proxy service. Please modify it according to your own needs, and complete it by referring to [Configuration > Outbounds > OutboundObject](https://www.v2fly.org/config/outbounds.html#outboundobject) in the V2Ray official website configuration document

```jsonc
{
   "log": {
     "loglevel": "warning"
   },
   "dns": {
     "hosts": {
       "dns.google": "8.8.8.8",
       "dns.pub": "119.29.29.29",
       "dns.alidns.com": "223.5.5.5",
       "geosite:category-ads-all": "127.0.0.1"
     },
     "servers": [
       {"address": "https://1.1.1.1/dns-query",
        "domains": ["geosite:geolocation-!cn", "geosite:google@cn"],
        "expectIPs": ["geoip:!cn"]
      },
      "8.8.8.8",
      {
        "address": "114.114.114.114",
        "port": 53,
        "domains": [
          "geosite:cn",
          "geosite:icloud",
          "geosite:category-games@cn"
        ],
        "expectIPs": ["geoip:cn"],
        "skipFallback": true
      },
      {
        "address": "localhost",
        "skipFallback": true
      }
    ]
  },
  "inbounds": [
    {
      "protocol": "socks",
      "listen": "0.0.0.0",
      "port": 1080,
      "tag": "Socks-In",
      "settings": {
        "ip": "127.0.0.1",
        "udp": true,
        "auth": "noauth"
      },
      "sniffing": {
        "enabled": true,
        "destOverride": ["http", "tls"]
      }
    },
    {
      "protocol": "http",
      "listen": "0.0.0.0",
      "port": 2080,
      "tag": "Http-In",
      "sniffing": {
        "enabled": true,
        "destOverride": ["http", "tls"]
      }
    }
  ],
  "outbounds": [
    {
      //In the following line, the protocol type should be changed to socks, shadowsocks, vmess or vless, etc. (remember to delete the text description of this line)
      "protocol": "Protocol Type",
      "settings": {},
      // In the following line, the tag value corresponds to the outboundTag in Routing, here is Proxy (remember to delete the text description of this line)      "tag": "Proxy",
      "streamSettings": {},
      "mux": {}
    },
    {
      "protocol": "dns",
      "tag": "Dns-Out"
    },
    {
      "protocol": "freedom",
      "tag": "Direct",
      "settings": {
        "domainStrategy": "UseIPv4"
      }
    },
    {
      "protocol": "blackhole",
      "tag": "Reject",
      "settings": {
        "response": {
          "type": "http"
        }
      }
    }
  ],
  "routing": {
    "domainStrategy": "IPIfNonMatch",
    "domainMatcher": "mph",
    "rules": [
      {
        "type": "field",
        "outboundTag": "Direct",
        "protocol": ["bittorrent"]
      },
      {
        "type": "field",
        "outboundTag": "Dns-Out",
        "inboundTag": ["Socks-In", "Http-In"],
        "network": "udp",
        "port": 53
      },
      {
        "type": "field",
        "outboundTag": "Reject",
        "domain": ["geosite:category-ads-all"]
      },
      {
        "type": "field",
        "outboundTag": "Proxy",
        "domain": [
          "full:www.icloud.com",
          "domain:icloud-content.com",
          "geosite:google"
        ]
      },
      {
        "type": "field",
        "outboundTag": "Direct",
        "domain": [
          "geosite:tld-cn",
          "geosite:icloud",
          "geosite:category-games@cn"
        ]
      },
      {
        "type": "field",
        "outboundTag": "Proxy",
        "domain": ["geosite:geolocation-!cn"]
      },
      {
        "type": "field",
        "outboundTag": "Direct",
        "domain": ["geosite:cn", "geosite:private"]
      },
      {
        "type": "field",
        "outboundTag": "Direct",
        "ip": ["geoip:cn", "geoip:private"]
      },
      {
        "type": "field",
        "outboundTag": "Proxy",
        "network": "tcp,udp"
      }
    ]
  }
}
```

## Projects using this project

- [@Loyalsoldier/clash-rules](https://github.com/Loyalsoldier/clash-rules)
- [@Loyalsoldier/surge-rules](https://github.com/Loyalsoldier/surge-rules)

## thank you

- [@Loyalsoldier/geoip](https://github.com/Loyalsoldier/geoip)
- [@v2fly/domain-list-community](https://github.com/v2fly/domain-list-community)
- [@Loyalsoldier/domain-list-custom](https://github.com/Loyalsoldier/domain-list-custom)
- [@felixonmars/dnsmasq-china-list](https://github.com/felixonmars/dnsmasq-china-list)
- [@gfwlist/gfwlist](https://github.com/gfwlist/gfwlist)
- [@cokebar/gfwlist2dnsmasq](https://github.com/cokebar/gfwlist2dnsmasq)
- [@Loyalsoldier/cn-blocked-domain](https://github.com/Loyalsoldier/cn-blocked-domain)
- [@AdblockPlus/EasylistChina+Easylist.txt](https://easylist-downloads.adblockplus.org/easylistchina+easylist.txt)
- [@AdGuard/DNS-filter](https://kb.adguard.com/en/general/adguard-ad-filters#dns-filter)
- [@PeterLowe/adservers](https://pgl.yoyo.org/adservers)
- [@DanPollock/hosts](https://someonewhocares.org/hosts)
- [@crazy-max/WindowsSpyBlocker](https://github.com/crazy-max/WindowsSpyBlocker)

## Project Star Number Growth Trend

[![Stargazers over time](https://starchart.cc/Loyalsoldier/v2ray-rules-dat.svg)](https://starchart.cc/Loyalsoldier/v2ray-rules-dat)
