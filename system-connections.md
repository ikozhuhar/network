```ruby
# Wired connection 1.nmconnection при настройке через GUI

[connection]
id=Wired connection 1
uuid=11107624-a30b-3c2f-9af6-d227a2844011
type=ethernet
autoconnect-priority=-999
interface-name=enp4s0f0np0
timestamp=1726230612

[ethernet]

[ipv4]
address1=172.17.45.253/24,172.17.45.1
dns=172.16.101.101;8.8.8.8;
method=manual

[ipv6]
addr-gen-mode=stable-privacy
method=auto

[proxy]
```

Реальный пример на CentOS 9


```ruby
# vi /etc/NetworkManager/system-connections/ens192.nmconnection

[connection]
id=ens192
uuid=e659404e-d93e-331c-a872-0d11e3215c4a
type=ethernet
autoconnect-priority=-999
interface-name=ens192

[ethernet]

[ipv4]
address1=192.168.160.33/24
dns=192.168.2.3;192.168.2.6;
dns-search=mip.ru;
gateway=192.168.160.1
method=manual

[ipv6]
addr-gen-mode=eui64
method=auto

[proxy]
```
