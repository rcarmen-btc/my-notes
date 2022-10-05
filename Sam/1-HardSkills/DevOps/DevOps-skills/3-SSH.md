# SSH
#### Настройка ssh
- Проверка статуса сервиса ssh:
```bash
systemctl status sshd -l
```

- Генерация ssh-ключа и в клиенте, и на сервере:
```bash
ssh-keygen
```

- Редактировние конфига /etc/ssh/sshd_config. Нужно изменить конфиг, как тут [[sshd_config_v1]]

- Создаём *authorized_keys* файл и записываем туда *.pub* ключ клиента:
```bash
cat id_rsa.pub > ~/.ssh/authorized_keys 

```

- Подключение по ssh
```bash
ssh (user-name)@(ip-adress) -p(port)
```
