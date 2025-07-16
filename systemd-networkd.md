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

# Что происходит?
[Match] — говорит: «Работай только с интерфейсом eth0».
[Network] — «Используй DHCP для получения IP».

# Активируем и проверяем
sudo systemctl restart systemd-networkd
ip addr show eth0 
```

Пример 2. Статический IP: Когда DHCP — это хаос, а вам нужен порядок

```ruby
# Пример 2. Статический IP: Когда DHCP — это хаос, а вам нужен порядок
sudo nano /etc/systemd/network/20-static.network

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
# Если нужно использовать DHCP, оставить только параметр DHCP=yes  
```

В секции `[Match]` можно указать имя сетевого интерфейса (можно получить с помощью команды `ip a`). В одной секции `[Match]` может быть несколько условий — сетевой профиль применится, если выполнены все условия из этой секции.

Включить службу systemd-networkd: `sudo systemctl enable systemd-networkd`.  
Запустить службу: `sudo systemctl start systemd-networkd`.  
Проверить статус сетевых интерфейсов: `networkctl list`.  
Вывести сетевые настройки: `networkctl status`.  
После изменений в конфигурационных файлах перезапускаем: `sudo systemctl restart systemd-networkd`.
