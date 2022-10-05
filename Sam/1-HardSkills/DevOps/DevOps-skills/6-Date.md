Узнать дату
```bash
date
```

#### Chrony
Для синхронизации мы используем chrony
```bash
sudo dnf install chrony
```

Конфиг
```bash
vim /etc/chrony.conf   
```

Изменить часавой пояс
```bash
sudo timedatectl set-timezone Europe/Moscow 
```

Добавление в автозагрузку
```bash
sudo systemctl start chronyd
sudo systemctl enable chronyd
```

Наблюдение за активными NTP-серверами
```bash
chronyc sources
```

