### network
1. [interfaces](interfaces)
2. [netplan](netplan)
3. [system-connections](system-connections)

```ruby
# sudo ip route { add | del | change} <dest_network> [via <gateway_ip>] [dev <interface>] [proto <protocol>]

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



