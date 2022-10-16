![[Pasted image 20221015232056.png]]
![[Pasted image 20221015232454.png]]

**Созадние пользователя с помощью Ad-hoc команды**
```bash
ansible -i hosts.ini -m user -a "name=riser state=present" -b -K demo 
```

**Созадние пользователя с помощью Ad-hoc команды с переменными окружения (экстра переменными)**
```bash
ansible -i hosts.ini -m user -a "name=riser state=present" -e "ansible_become=true ansible_become_password=riser12345" demo  
```

**Созадние пользователя с помощью Ad-hoc команды с переменными окружения (экстра переменными)**
```bash
писать экстра переменные выше это в ini файл, но лучше так не делать
```

**Удаление пользователя с помощью Ad-hoc команды**
```bash
ansible -i hosts.ini -m user -a "name=riser state=absent" -b -K demo
```

