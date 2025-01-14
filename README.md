### network
1. [interfaces](interfaces)
2. [netplan](netplan)
3. [system-connections](system-connections)

## Конфигурационные файлы в дистрибутивах Linux

Смотрим названия сетевых интерфейсов
```
ls /sys/class/net
```

```
nano /etc/net/ifaces/eno1/options
nano /etc/sysconfig/network-scripts/ifcfg-eth0
nano /etc/NetworkManager/system-connection/Wired_connection1.nmconnection
nano /etc/netplan/config.yaml
```

![image](https://github.com/user-attachments/assets/d85aae53-7928-4cc3-87aa-d9fe85e6f417)


## Настройка на CentOS/AlmaLinux/Rockylinux

```
sudo nano /etc/sysconfig/network-scripts/ifcfg-eth0
sudo systemctl status network
sudo systemctl restart network
```

```
ip address {add|change|replace} IFADDR dev IFNAME [ LIFETIME ]
ip address add 10.104.100.156/24 dev eth0
ip route add default via 10.104.100.251 dev eth0
```
![image](https://github.com/user-attachments/assets/1e094677-bd78-4a46-8234-4d2ae09de2e2)

![image](https://github.com/user-attachments/assets/331d1133-498e-4044-b302-014d8008b9fa)




| Command | Description |
| ------- | ----------- |
| `git status` | text text text text |
| `lern text` | some text, some text, some text |
| `lern text text` | some text, some text, some text, some text, some text, some text |
| `lern text` | some text, some text, some text |
| `lern text` | some text, some text, some text |



