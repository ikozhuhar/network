### Настройка сети в systemd-networkd

Пример 1: Простая DHCP-настройка

```ruby
sudo nano /etc/systemd/network/10-ens160.network
```

```ruby
[Match]
Name=ens160

[Network]
DHCP=yes
```

```ruby
# Что происходит?
[Match] — говорит: «Работай только с интерфейсом eth0».
[Network] — «Используй DHCP для получения IP».

# Активируем и проверяем
sudo systemctl restart systemd-networkd
ip addr show ens160 
```

Пример 2. Статический IP: Когда DHCP — это хаос, а вам нужен порядок

```ruby
sudo nano /etc/systemd/network/10-ens160.network
```

```ruby
sudo mcedit /etc/systemd/network/10-ens160.network  
[Match]  
Name=ens160

[Network]  
Description=Local network  
Address=192.168.100.22/24  
Gateway=192.168.100.1  
DNS=192.168.100.10 192.168.110.10  
Domains=vmblog.local  
LinkLocalAddressing=no  
NTP=time.google.com  
```

```ruby
sudo systemctl restart systemd-networkd
networkctl list
networkctl status
```


### Продвинутое: Настройка VLAN

Дополнительно создаём файл для VLAN-интерфейса:

```ruby
# /etc/systemd/network/30-vlan100.network
[Match]
Name=ens160.100

[Network]
Address=192.168.100.10/24
```

```ruby
# Активируем:
sudo networkctl reload
```



### Траблшутинг: Когда сеть не работает

Проблема 1: Интерфейс не поднялся

```ruby
# Проверяем адрес
ip a

# Проверяем имя интерфейса
ip link show

# Смотрим логи
journalctl -u systemd-networkd -b
```

Проблема 2: Нет интернета, хотя IP есть

```ruby

# Если пинг есть → проблема в DNS. Проверяем /etc/resolv.conf.
# Если нет → проблема в маршруте. Смотрим: ip route 
ping 8.8.8.8

# Есть ли default via 192.168.1.1?
ip route
```

Проблема 3: Конфликт с NetworkManager

```ruby
sudo systemctl stop NetworkManager  
sudo systemctl disable NetworkManager
```
