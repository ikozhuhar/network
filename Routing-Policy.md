_Политики маршрутизации_

Шаг 1: Добавить таблицы маршрутизации

```ruby
# В файл /etc/iproute2/rt_tables добавить:
50 eth0_table
51 eth1_table
```

Шаг 2: Netplan

```ruby
network:
  renderer: networkd
  ethernets:
    ens192:
      addresses:
        - 192.168.13.67/24
      routes:
        - to: default
          via: 192.168.13.1
          metric: 99
        - to: default
          via: 192.168.13.1
          table: 50
          metric: 100
      nameservers:
        addresses:
          - 192.168.2.3
          - 192.168.2.6
      routing-policy:
        - from: 192.168.13.67
          table: 50
    ens224:
      addresses:
        - 10.78.65.230/29
      routes:
        - to: default
          via: 10.78.65.225
          table: 51
          metric: 200
      routing-policy:
        - from: 10.78.65.230
          table: 51
```
