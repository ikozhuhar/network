:white_check_mark: _Задача: <a name='1'>Реальный пример на CentOS 9</a>_

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
