# Network
## Начальная настройка сети
Пытаемся подключиться к ya.ru, но хост не знает на какой ip-адрес указывает этот домен. Нужно выполнить базовую нстройку сети, так как пока у нас нет внешней сети:
1. Для начала нужно понять настроени ли сетевой интерфейс.
2. Используем `ip a` для того, чтобы узнать какие сетевые интерфейсы есть и какие настройки в них указаны. 
3. Видим, что сетевой интерфейс *enp0s3* не настроен. Нужно его настроить через DHCP.
4. Для настройки DHCP есть утилита `dhclient -v`. При запуске dhclient читает _dhclient.conf_ для получения инструкций по настройке. Затем он получает список всех сетевых интерфейсов, которые настроены в текущей системе. Для каждого интерфейса он пытается настроить интерфейс, используя протокол DHCP.
5. `route -n` выводит таблицу роутинга.
6. Используем утилиту **NetworkManager** `nmtui` для редактировния параметров сетевых интерфейсов. В поле Address из `ip a`, в поле  Gateway  из `route -n` и DNS servers 8.8.8.8
7. После настройки мы перезапускаем сервис NetworkManager: 
	```bash
	systemctl restart NetworkManager
	```

## NetworkManager
Запуск NetworkManager 
```bash
sudo systemctl start NetworkManage
sudo systemctl status NetworkManage
```

Состояние всех интерфейсов на сервере
```bash
nmcli general status
```

Отключить wi-fi
```bash
sudo nmcli radio wifi off 
```

Статус wi-fi
```bash
nmcli radio wifi
```

Все сетевые интерфейсы
``` bash
nmcli connection show
```

Все устройства подключерные к серверу
```bash
nmcli device show
```

Создание DHCP соединение по ethernet
```bash
sudo nmcli connection add con-name "dhcp" type ethernet ifname enp0s3
```

Удаление DHCP соединения
```bash
sudo nmcli connection del dhcp 
```

Добавление статического подключения вместо DHCP
```bash
sudo nmcli connection add con-name "static" ifname enp0s3 autoconnect no type ethernet ip4 192.168.43.101 gw4 192.168.43.1  # адрес внешнего сетевого устройства потом шлюз, "ip a && route -n"
```

Добавить в подключение DNS
```bash
sudo nmcli connection modify "static" ipv4.dns 8.8.8.8  
```

Активация интерфейса, по-умолчанию
```bash
nmcli con up "static"
```
