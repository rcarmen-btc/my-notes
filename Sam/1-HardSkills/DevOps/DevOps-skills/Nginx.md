Начальная настройка
```bash
dnf install nginx

systemctl start nginx
systemctl enable nginx

```

Конфиг nginx:
```bash
cat /etc/nginx/nginx.conf
```

Команда проверки синтаксиса конфига:
```bash
nginx -t
```

Создание site-enabled & site-availble, но я буду всё делать в conf.d
```bash
vim /etc/nginx/conf.d/(site-name).conf
```

На всякий случай как делать ссылку:
```bash 
ln -s /etc/nginx/sites-availables/(site-name) /etc/nginx/sitas-enabled/(site-name)
```

Сама конфигурация:
```bash 
server {
 
  listen 80 default_server; # директива listen указывает, на каком порту # будет работать виртуальный хост
  server_name locallesson.com; # домен или название виртуального хоста
 
  root /var/www/locallesson.com; # корневая директория
  index index.html; # файл, который будет открываться по умолчанию
 
  location / { # указывается корневая директория сайта
    try_files $uri $uri/ = 404; # проверка на наличие файла, если нет - 							   #  показывает ошибку 404
  }
}
```

Для более тонкой настройки репозиториев:
```bash
dnf install yum-utils

dnf install php74 php74-php-fpm php74-php-mysqlnd php74-php-opcache php74-php-xml php74-php-xmlrpc php74-php-gd php74-php-mbstring php74-php-json
```

Включаем нужный модуль:
```bash
systemctl start php74-php-fpm
systemctl enable php74-php-fpm
```
