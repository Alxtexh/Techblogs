##  MIKROTIK - HOTSPOT CONFIGRATION


```bash

/system identity set name="Script"
    
/interface bridge add name=driftHotspotBridge

/ip address add address=192.168.2.1/24 interface=HotspotBridge
  
/ip pool add name=”drift-Hotspot-Pool” ranges=192.168.2.1-192.168.2.200
  
/ip dns set allow-remote-requests=yes cache-max-ttl=1w cache-size=2048KiB max-udp-packet-size=4096 servers=8.8.8.8
  
ip dhcp-server add address-pool=drift-Hotspot-Pool authoritative=yes bootp-support=static disabled=no interface=driftHotspotBridge lease-time=1h name=drift-Hotspot-dhcp
  
/ip dhcp-server config set store-leases-disk=5m
  
/ip dhcp-server network add address=192.168.2.0/24 comment="drift hotspot Network" gateway=192.168.2.1
  
/ip hotspot profile add dns-name="drift.net" hotspot-address=192.168.2.1 html-directory=hotspot http-cookie-lifetime=3d http-proxy=0.0.0.0:0 login-by=cookie,http-chap,https,http-pap,mac-cookie name=driftServerProfile rate-limit="" smtp-server=0.0.0.0 split-user-domain=no use-radius=yes

/ip hotspot add address-pool=drift-Hotspot-Pool addresses-per-mac=1 disabled=no idle-timeout=5m interface=driftHotspotBridge keepalive-timeout=none name=driftHotspotServer profile=driftServerProfile
  
/ip hotspot user profile add address-pool=drift-Hotspot-Pool advertise=no idle-timeout=none keepalive-timeout=2m name="drift-User-Profile" open-status-page=always shared-users=unlimited status-autorefresh=1m transparent-proxy=yes
  
/ip hotspot service-port set ftp disabled=yes ports=21
  
/ip hotspot walled-garden ip add action=accept disabled=no dst-host=umeskiasoftwares.com 
  
/ip firewall nat add action=masquerade chain=srcnat disabled=no
 
/radius add address=3.98.248.202 secret=UMS273HYOS26510WSDR service=hotspot timeout=3000ms

/radius incoming set accept=yes
  ```
