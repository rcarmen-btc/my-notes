## systemd
**Systemd** - это демон для запуска других демонов, которые управляются через команду *systemctl*.
**Units** - это описаение сервиса в текстовом виде, где указаны опреации, которые должны быть выполнены до и после запуска службы, описание праметров системы инициализации.

Каталог юнитов администратора 
```bash
/etc/systemd/system
```

Каталог юнитов  из rpm 
```bash
/usuir/lib/system/system
```

Юниты runtime 
```bash
/run/systemd/system
```

> Иногда вместо *systemctl* используют *service*, это рудимент установленный для совместимости

### Systemctl
Проверка статуса службы 
```bash
systemctl status (service)
```

Запуск службы
```bash
systemctl start (service)
```

Остановка службы 
```bash
systemctl start (service)
```

Перезагрузка службы 
```bash
systemctl restart (service)
```

Добавить службу в автозапуск
```bash
systemctl enable (service)
```

Убрать службу из автозапуска
```bash
systemctl disable (service)
```

Узнать есть ли служба в автозапуске
```bash
systemctl is-enabled sshd
```

Узнать список всех служб
```
systemctl list-unit-files
```

Проверить что все запущенные службы находятся в автозагрузке
```bash
systemctl list-units | awk '{print $1;system("systemctl is-enabled " $1)}'
```