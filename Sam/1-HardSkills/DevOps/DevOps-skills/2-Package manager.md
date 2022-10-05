# Package manager
### Пакетный менеджер *dnf* 
Можно установить сразу несколько программ за одну команду:
```bash
dnf install mc wget git net-tools -y
```

#### Установленные репозитории
```bash
(dnf|yum) repolist
```

##### Репозитории epel, remi
Установка epel
```bash 
sudo dnf install epel-release
```

Файлы репозиториев
```bash
ls -lah /etc/yum.repos.d
```

Установка remi
```bash
sudo dnf install -y http://rpms.remirepo.net/enterprise/remi-release-8.rpm
```

#### Обновление dnf
```bash
sudo dnf update
```

