Netplan (Ubuntu/Debian) – `это фронтенд` (генератор конфигураций), который преобразует YAML-файлы (/etc/netplan/*.yaml) в настройки `для низкоуровневых сетевых демонов`. Может работать с разными бэкендами, включая `systemd-networkd и NetworkManager`, но по умолчанию в серверных дистрибутивах (например, Ubuntu Server) он использует именно systemd-networkd. 


#### :white_check_mark: Основные низкоуровневые сетевые демоны

`1`. systemd-networkd  
`2`. NetworkManager (в режиме без GUI)  
`3`. dhcpcd  
`4`. wpa_supplicant  
`5`. ifupdown (networking.service)  


#### :white_check_mark: Как Netplan взаимодействует с демонами?

В зависимости от указанного рендерера (renderer), Netplan передаёт управление:

- renderer: networkd → настройки применяются через systemd-networkd (стандартный выбор для серверов).
- renderer: NetworkManager → настройки передаются NetworkManager (обычно используется в десктопных системах).

**Пример конфигурации Netplan для systemd-networkd**

_Файл /etc/netplan/01-netcfg.yaml (Ubuntu Server):_

```ruby
# https://netplan.readthedocs.io/en/stable/examples/
# Static IP address assignment

yaml
network:
  version: 2
  renderer: networkd  # использует systemd-networkd
  ethernets:
    eth0:
      dhcp4: true
      addresses:
        - 10.10.10.2/24
      routes:
        - to: default
          via: 10.10.10.1
      nameservers:
          search: [mydomain, otherdomain]
          addresses: [10.10.10.1, 1.1.1.1]
```	  
	  
#### :white_check_mark: Как проверить, что Netplan использует systemd-networkd?

Посмотрите рендерер в конфиге Netplan:

```ruby
cat /etc/netplan/*.yaml | grep "renderer:"
```

#### :white_check_mark: Проверьте статус systemd-networkd:

```ruby
systemctl status systemd-networkd
```

#### :white_check_mark: Убедитесь, что сетевые интерфейсы управляются systemd-networkd:

```ruby
networkctl list
```

#### :white_check_mark: Важные моменты

Если в Netplan указан renderer: networkd, но systemd-networkd не запущен, сеть не будет работать.

NetworkManager и systemd-networkd не должны работать одновременно из-за возможных конфликтов. 

Признаки конфликта:  
- Сеть работает нестабильно (то есть, то нет).
- Интерфейсы могут не подниматься или пропадать.

После изменения конфига Netplan применяйте изменения:

```ruby
sudo netplan try
sudo netplan apply
sudo netplan generate

# Если вы хотите видеть более подробную информацию, используйте опцию --debug:
sudo netplan --debug apply

# Если вы хотите видеть более подробную информацию, используйте опцию --debug:
sudo netplan --debug generate

sudo systemctl restart systemd-networkd.service
sudo systemctl restart NetworkManager
sudo /etc/init.d/networking restart
```

#### :white_check_mark: Вывод

Netplan ≠ systemd-networkd, но они тесно связаны: Netplan генерирует конфиги, а systemd-networkd их исполняет. Если вы используете Netplan с рендерером networkd, убедитесь, что systemd-networkd активен, а NetworkManager отключён.
