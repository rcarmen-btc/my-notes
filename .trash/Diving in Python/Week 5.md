# Week 5

Date: March 4, 2022 8:54 PM

### Key words | Questions

`**os.getpid()**`

`**top**`

`**ps aux**`

`**head -1**`

`**lsof -p 4949**`

### Notes

# Процесс

---

- Идентификатор процесса, PID
- Объём оперативной памяти
- Стек
- Список откртых файлов
- Ввод/вывод

## Команды для мониторинга процессов

---

```python
top # htop
ps aux | head -[lines num] # процессы
sudo strace -p [pid] # системные вызовы
lsof -p [pid] # открытые файлы 
```

## Создание процессов

---

```python
import time
import os

foo = "bar"

pid = os.fork()
if pid == 0:
    # Child process
    while True:
				foo = "baz"
        print("Child: ", foo)
        time.sleep(5)
else:
    # Parent process
    print("Parent: ", foo)
    os.wait()

# Parent: bar
# Childe: baz
```

- Память копируется целиком
- Файловые дескипторы тоже копируются, это никак не влияет на родительский процесс

## Multiprocessing

---

- Можно создавать процессы с помощью функиций:
    
    ```python
    from multiprocessing import Process
    import time
    
    def f(name):
        print("Hello", name, time.time())
    
    p = Process(target=f, args=("Mike",))
    j = Process(target=f, args=("Jake",))
    
    p.start()
    j.start()
    
    p.join()
    j.join()
    ```
    
- А также с помощью наследования

# Потоки

---

- Похож на процесс
- У потока своя последовательность иструкций
- У потока есть стек
- Все потоки выполняются в рамках процесса
- Потоками управляет ОС
- У потоков свои ограничения

## Создание потоков

---

- Можно создавать процессы с помощью функиций:
    
    ```python
    from threading import Thread
    
    def f(name):
        print("Hello, ", name)
    
    th = Thread(target=f, args=("Bob", ))
    
    th.start()
    th.join()
    ```
    
- А также с помощью наследования

### Пул потоков, `cuncurrent.futures.Future`

---

```python
from concurrent.futures import ThreadPoolExecutor, as_completed
import time

def timer(func):
    def wrapper():
        start = time.time()
        func()
        print ('Time:', time.time() - start)
        # return res
    return wrapper

def f(a):
    return a ** 2000

@timer
def pool():
    with ThreadPoolExecutor(max_workers=10) as pool:
        result = [pool.submit(f, i) for i in range(10)]

        for future in as_completed(result):
            print(future.result())
    

pool()
```

## Очереди, модуль Queue

---

# Работа с сетью, сокеты

---

- Создание сокета, сервер:
    
    ```python
    import socket
    
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.bind(("127.0.0.1", 1001))
    sock.listen(socket.SOMAXCONN)
    
    conn, addr = sock.accept()
    while True:
        data = conn.recv(1024)
        if not data:
            break
        print(data.decode('utf8'))
    
    conn.close()
    sock.close()
    ```
    
- Создание сокета, клиент:
    
    ```python
    import socket 
    
    sock = socket.socket()
    sock.connect(('127.0.0.1', 10001))
    sock.sendall(b'hello')
    sock.close()
    
    # более короткая запись
    
    sock1 = socket.create_connection(('127.0.0.1', 10001))
    sock1.sendall(b'ping')
    sock1.close()
    ```
    

---

- Через контекстные менеджеры, **сервер**
    
    ```python
    import socket 
    
    with socket.socket() as sock:
        sock.bind('', 10001)
        sock.listen()
    
        with True:
            conn, addr = sock.accept()
            with conn:
                while True:
                    data = conn.recv(1024)
                    if not data:
                        break
                    print(data.decode())
    ```
    
- Через контекстные менеджеры, **клиент**
    
    ```python
    with socket.create_connection('127.0.0.1', 10001) as sock:
        sock.sendall('pint'.encode())
    ```
    

---

# Ассинхронное программирование

---

- Модуль select, механизмы опроса файловых дескрипторов
- Неблокирующий ввод/вывод (Мультиплексирование ввода/вывода)
    
    ```python
    import socket
    import select
    
    sock = socket.socket()
    sock.bind(('', 10002))
    sock.listen()
    
    conn1, addr = sock.accept()
    conn2, addr = sock.accept()
    
    conn1.setblocking(0)
    conn2.setblocking(0)
    
    epoll = select.epoll()
    epoll.register(conn1.fileno(), select.EPOLLIN | select.EPOLLOUT)
    epoll.register(conn2.fileno(), select.EPOLLIN | select.EPOLLOUT)
    
    conn_map = {
        conn1.fileno(): conn1,
        conn2.fileno(): conn2,
    }
    
    while True:
        events = epoll.poll(1)
        for fileno, event in events:
            if event & select.EPOLLIN:
                data = conn_map[fileno].recv(1024) 
                print(data.decode('utf8'))
            elif event & select.EPOLLOUT:
                conn_map[fileno].send('ping'.encode('utf8'))
    ```
    

# Итераторы и генераторы (Ещё раз). В чем разница?

---

```python
"""
Hello
"""

class MyIt:
    """
    It's my iterator...
    """

    def __init__(self, stop) -> None:
        self.stop = stop
        self.curr = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.curr >= self.stop:
            raise StopIteration

        curr = self.curr
        self.curr += 1
        return curr

for i in MyIt(9):
    print(i)

######### Genetators

def MyRangeGen(top):
    current = 0
    while current < top:
        yield current
        current += 1
    
for i in MyRangeGen(4):
    print(i)
```

# Генераторы и сопрограммы(корутины)

---

```python
# сопрограммы (корутины)

def grep(pattern):
    print("start grep")
    while True:
        line = yield
        if pattern in line:
            print(line)

g = grep('python') 
next(g)
g.send('I love u, Anya')
g.send('python is simple')
```

<aside>
📌 **SUMMARY:**

</aside>