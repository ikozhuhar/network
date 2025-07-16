### network
1. [interfaces](interfaces)
2. [netplan](netplan)
3. [system-connections](system-connections)


### Сетевые службы и инструменты в Linux

В Linux существует множество сетвевых служб и инструментов для работы с интернетом, начиная от базовых сетевых настроек и заканчивая специализированными сервисами. Вот основные из них:

#### :white_check_mark: 1. Сетевые службы и инструменты

`1.1` `Systemd-networkd` – альтернатива NetworkManager для управления сетью через systemd. Пример конфига (/etc/systemd/network/20-wired.network).  
`1.2` `NetworkManager` – универсальный инструмент для управления проводными, беспроводными, VPN и мобильными подключениями. Поддерживает графический интерфейс и командную строку (nmcli).  
`1.3` `Networking` ifupdown – традиционный способ настройки сети через файл /etc/network/interfaces (используется в старых версиях Debian/Ubuntu).   
`1.4` `Netplan` (Ubuntu/Debian) – это фронтенд (генератор конфигураций), который преобразует YAML-файлы (/etc/netplan/*.yaml) в настройки для низкоуровневых сетевых демонов.   


#### :white_check_mark: 2. DNS-сервисы

`2.1` resolv.conf – файл для ручного указания DNS-серверов (/etc/resolv.conf).  
`2.2` systemd-resolved – системный DNS-резолвер, кэширующий запросы.  
`2.3` dnsmasq – лёгкий DNS- и DHCP-сервер, часто используется в роутерах.  


#### :white_check_mark: 3. Прокси и NAT

`3.1` Squid – прокси-сервер для кэширования трафика и контроля доступа.  
`3.2` iptables/nftables – настройка NAT и межсетевого экрана для общего доступа в интернет.  
`3.3` Firewalld – более удобный фронтенд для управления правилами firewall.  


#### :white_check_mark: 4. Беспроводные сети

`4.1` iwconfig/iwlist – инструменты для управления Wi-Fi (устаревшие, заменяются на iw).  
`4.2` wpa_supplicant – демон для подключения к защищённым Wi-Fi сетям.  
`4.3` NetworkManager – поддерживает настройку Wi-Fi через GUI и CLI.  


#### :white_check_mark: 5. Утилиты диагностики

`5.1` ping/traceroute – проверка доступности узлов и маршрутов.  
`5.2` dig/nslookup – DNS-запросы.  
`5.3` netstat/ss – мониторинг сетевых соединений.  
`5.4` mtr – комбинация ping и traceroute.  


#### Ключевые различия между NetworkManager, networking (ifupdown) и systemd-networkd:


#### :white_check_mark: 1. Systemd-networkd

_Для чего:_   
- Современный, легковесный менеджер сетей от systemd.

_Где используется:_  
- Ubuntu Server, Arch Linux, Fedora Server.

_Особенности:_  
- Интегрирован с systemd (использует systemd-resolved для DNS).
- Конфигурация через .network и .netdev файлы (в /etc/systemd/network/20-wired.network).
- Поддержка VLAN, мостов, bonding, DHCP (systemd-networkd включает встроенный DHCP-клиент).
- Нет GUI, только конфиги и networkctl.
- Работает вместе с systemd-resolved (кэширование DNS).
	
_Пример конфига (/etc/systemd/network/20-wired.network):_

```ruby
[Match]
Name=eth0

[Network]
DHCP=yes
# или статический IP:
# Address=192.168.1.10/24
# Gateway=192.168.1.1
# DNS=8.8.8.8
```

_Когда использовать?_  
- На серверах и в минималистичных системах.
- Если нужна простая, но мощная альтернатива NetworkManager.



#### :white_check_mark: 2. NetworkManager

_Для чего:_ 
- Управление сетью в десктопных и мобильных системах (удобен для Wi-Fi, VPN, PPPoE).

_Где используется:_ 
- Ubuntu Desktop, Fedora, RHEL Workstation.

_Особенности:_ 

- Поддержка графического интерфейса (GUI) и CLI (nmcli, nmtui).
- Автоматическое переподключение при сбоях.
- Управляет Wi-Fi, Bluetooth, модемами, VPN (OpenVPN, WireGuard, PPTP).
- Интегрируется с DHCP (dhclient или internal).
- Конфиги хранятся в /etc/NetworkManager/.
	
_Пример конфига (/etc/NetworkManager/system-connections/'Wired connection 1.nmconnection'):_

```ruby
[connection]
id=Wired connection 1
uuid=16048739-faa0-3967-b7e4-6e952473dc87
type=ethernet
autoconnect-priority=-999
interface-name=ens160
timestamp=1746605081
zone=public

[ethernet]

[ipv4]
address1=192.168.11.124/24,192.168.11.1
dns=192.168.2.3;192.168.2.6;
ignore-auto-dns=true
method=manual

[ipv6]
addr-gen-mode=stable-privacy
method=auto

[proxy]
```

_Когда использовать?_  
- На ноутбуках и ПК с динамическим подключением (Wi-Fi, мобильные сети).
- Если нужен удобный интерфейс для переключения сетей.



#### :white_check_mark: 3. Networking (Устарел, но ещё встречается в Debian/Ubuntu без systemd-networkd)

_Для чего:_  
- Традиционный способ настройки сети через скрипты.
	
_Где используется:_   
- Старые версии Debian/Ubuntu (до перехода на Netplan).

_Особенности:_  
- Работает через /etc/network/interfaces.
- Использует команды ifup, ifdown.
- Нет поддержки Wi-Fi (только базовый Ethernet, мосты, VLAN).
- Нет автоматического восстановления связи.
- Зависит от dhclient для DHCP.
	
_Пример конфига: /etc/network/interfaces_  

```ruby
auto eth0
iface eth0 inet dhcp
# или статический IP:
# iface eth0 inet static
# address 192.168.1.10
# netmask 255.255.255.0
# gateway 192.168.1.1
```

_Когда использовать?_  
- На серверах с простой статической настройкой.
- В минималистичных системах без NetworkManager.



#### :white_check_mark: 4. Какой выбрать?

- Для десктопа / ноутбука → NetworkManager (удобство, Wi-Fi, VPN).  
- Для сервера → systemd-networkd (легковесный, стабильный).  
- Для legacy-систем → networking (ifupdown) (если нет systemd).  

⚠️ Важно:  
- Нельзя использовать NetworkManager и systemd-networkd одновременно (будут конфликты).
- Netplan – это фронтенд, для сетевых демонов. Может работать с systemd-networkd и NetworkManager.
- Если сомневаетесь, на серверах лучше выбирать systemd-networkd, а на ПК – NetworkManager.




<br><br><br><br><br><br><br><br>




























```ruby
# ip route { add | del | change} <dest_network> [via <gateway_ip>] [dev <interface>] [proto <protocol>]

# Маршрут до сети 192.168.3.0/24 через шлюз 192.168.0.1 и интерфейс eth0 с метрикой 100
route add -net 192.168.3.0 netmask 255.255.255.0 gw 192.168.0.1 dev eth0 metric 100

# маршрут до Cisco в Космосе
ip route add 10.104.200.0/21 via 10.104.100.1 dev eno1

# маршрут до хоста через шлюз 10.104.100.251 и интерфейс eth0
ip route add 192.168.3.2 via 10.104.100.251 dev eno1 
```

## Конфигурационные файлы в дистрибутивах Linux

Смотрим названия сетевых интерфейсов
```ruby
ls /sys/class/net
```

```ruby
nano /etc/net/ifaces/eno1/options
nano /etc/sysconfig/network-scripts/ifcfg-eth0
nano /etc/NetworkManager/system-connection/Wired_connection1.nmconnection
nano /etc/netplan/config.yaml
```

![image](https://github.com/user-attachments/assets/d85aae53-7928-4cc3-87aa-d9fe85e6f417)


## Дополнительные маршруты в Альт Linux

```ruby
[connection]
id=Проводное подключение 1
uuid=cadf9c42-cfc8-30cc-8d5d-36114852d8eb
type=ethernet
autoconnect-priority=-999
interface-name=eno1
timestamp=1741088449

[ethernet]

[ipv4]
address1=10.104.100.156/24,10.104.100.251
dns=10.104.110.23;172.25.128.10;
method=manual
route1=10.104.200.0/21,10.104.100.1
route2=10.104.208.0/21,10.104.100.1

[ipv6]
addr-gen-mode=stable-privacy
method=auto

[proxy]

```

![image](https://github.com/user-attachments/assets/d70d2dea-fff3-4664-9cbe-b095efde28c7)



## Настройка на CentOS/AlmaLinux/Rockylinux

```ruby
sudo nano /etc/sysconfig/network-scripts/ifcfg-eth0
sudo systemctl status network
sudo systemctl restart network
```

```ruby
ip address {add|change|replace} IFADDR dev IFNAME [ LIFETIME ]
ip address add 10.104.100.156/24 dev eth0
ip route add default via 10.104.100.251 dev eth0
```
![image](https://github.com/user-attachments/assets/1e094677-bd78-4a46-8234-4d2ae09de2e2)

![image](https://github.com/user-attachments/assets/331d1133-498e-4044-b302-014d8008b9fa)




## Настройка на Astra Linux

![image](https://github.com/user-attachments/assets/f55df749-9d1c-41fd-a7f2-fa2ff1cecc0c)



**Маршрутизация в Linux** необходима, чтобы компьютеры могли определить, по какой цепочке должен пойти пакет, чтобы достигнуть цели. Маршруты можно настроить на уровне интерфейса или маршрутизатора.

**Для просмотра таблицы маршрутизации в Linux** можно использовать команду **route**. Более подробную информацию можно получить с помощью команды **routel**. Также удобный способ — команда **ip**.

**Для настройки маршрутов** также используется команда **ip**. Например, чтобы изменить маршрут по умолчанию, нужно выполнить: **ip route add default via 192.168.1.1**. Чтобы добавить маршрут для любого IP-адреса, например, для 243.143.5.25: **sudo ip route add 243.143.5.25 via 192.168.1.1**.

Такие маршруты будут активны только до перезагрузки, после перезагрузки компьютера они будут автоматически удалены. Чтобы маршруты сохранились, их нужно добавить в файл конфигурации.



| Command | Description |
| ------- | ----------- |
| `git status` | text text text text |
| `lern text` | some text, some text, some text |
| `lern text text` | some text, some text, some text, some text, some text, some text |
| `lern text` | some text, some text, some text |
| `lern text` | some text, some text, some text |



