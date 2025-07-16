### Настройка сети в systemd-networkd

Пример 1: Простая DHCP-настройка

```ruby
sudo nano /etc/systemd/network/20-wired.network
```

```ruby
[Match]
Name=eth0

[Network]
DHCP=yes
```

```ruby
# Что происходит?
[Match] — говорит: «Работай только с интерфейсом eth0».
[Network] — «Используй DHCP для получения IP».

# Активируем и проверяем
sudo systemctl restart systemd-networkd
ip addr show eth0 
```

Пример 2. Статический IP: Когда DHCP — это хаос, а вам нужен порядок

```ruby
sudo nano /etc/systemd/network/20-static.network
```

```ruby
sudo mcedit /etc/systemd/network/lan-static.network  
[Match]  
Name=ens33 

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
