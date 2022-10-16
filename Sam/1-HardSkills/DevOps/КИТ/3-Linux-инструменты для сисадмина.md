## Современный сервис
Сервис - состоящая из N узлов.
Система может быть распределённая.
Узкие места могут быть как у узла, так и у системы в целом.

Узел:
- Процессор
- Память
- Диск

Система:
- Сеть

## Роль сисадмина
- Планировать ёмкость системы и строить архитектуру сервиса и инфраструктуру;
- Производить деплой и обнавление программ;
- Поиск узких мест и оптимизация как приложения так и ОС;
- Реагировение на возникающие инциденты;

## Документация
- `man \<command>`
- `info / pinfo`
- `/usr/share/doc`
- Код
- Инструменты отладки (`ldd`, `strace`, `ltrace`, `gdb`, `objdump` and etc.)

### `file`
The Linux **`file`** command helps determine the type of a file and its data. The command doesn't take the file extension into account, and instead runs a series of tests to discover the type of file data.
```bash
> file [option] [file name]

> file /bin/ls
#/bin/ls: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.32, BuildID[sha1]=c8ada1f7095f6b2bb7ddc848e088c2d615c3743e, stripped

```
In the syntax above, **`file name`** represents the name of the file you want to test. The **`file`** command performs three sets of tests trying to determine the file type, in this order:


### `ldd`
Показывает какие библиотеки подгружаются в момент запуска какой-либо программы.
```bash
> ldd /bin/ls
	# linux-vdso.so.1 =>  (0x00007ffce0548000)
	# libselinux.so.1 => /lib64/libselinux.so.1 (0x00007f26e3682000)
	# libcap.so.2 => /lib64/libcap.so.2 (0x00007f26e347d000)
	# libacl.so.1 => /lib64/libacl.so.1 (0x00007f26e3274000)
	# libc.so.6 => /lib64/libc.so.6 (0x00007f26e2ea6000)
	# libpcre.so.1 => /lib64/libpcre.so.1 (0x00007f26e2c44000)
	# libdl.so.2 => /lib64/libdl.so.2 (0x00007f26e2a40000)
	# /lib64/ld-linux-x86-64.so.2 (0x00007f26e38a9000)
	# libattr.so.1 => /lib64/libattr.so.1 (0x00007f26e283b000)
	# libpthread.so.0 => /lib64/libpthread.so.0 (0x00007f26e261f000)
```

### `ltrace`
Показывает все строки с библиотечными вызовами в программе.
```bash
> ltrace /bin/ls
```

### `strace`
Показывает все строки с системными вызовами в программе.
```bash
> strace /bin/ls
```


## Shell оболочка
- `sh`
- `bash`
- `zsh`
- `dash`

## Вниешние команды
- `grep` 
- `sed`
- `awk`

 ## Эмуляторы терминала
 - `screen`
 - `tmux`

## Протокол доступа
 - VPN
 -` ssh/ssh -X`
 
![[Pasted image 20221014121654.png]]

![[Pasted image 20221014122202.png]]
## Типы инструментов 
- Счётчики
- Трассровка
- Профайлинг - серия снимков системы целиком или процесса

## Счётчики
Бесплатные.
Как правило целочисленные беззнаковые, которые увеличиваются при настплении события. К примеру, счётчики кол-ва принятых сетевых пакетов, чколько было произведено дисковых операций и системных вызовов.

### Общесистемные 
На основе счётчиков из /proc, /sys
- `vmstat`
- `iostat`
- `netstat`
- `sar`

### На каждый процесс
На основе /proc
- `ps`
- `top`

## Трассировка
Сбор данных для дальнейшего анализа.

### Общесистемные
Собирают данные при помощи libpcap, tracepoints, kprobes,  ftrace
- `tcpdump`
- `blktrace`
- `SystemTap`
- `perf`

### На каждый процесс
- `strace`
- `gdb` - анализ почему умер процесс

## Профилирование 
- `oprofile`
- `perf`
- `SystemTap`

## CPU

CPU делят на User-Time/Kernel-Time

- `uptime` - показывает load average для понимания характера нагрузки на процессор и систему , up time.
- `vmstat` - запустив утилиту с интервалом в 1 секунду, можно, к примеру, увидеть сколько запаса по процессору осталось. Лучше это, чем топ, если система под нагрузкой.
```bash
> vmstat [interval] [times]
```
- `top` - показывает какой процесс и пользователь потребляет больше всех процессорного времени.
	- клавиша **1** показывает кол-во ядер.
	- клавиша **h** показывает справку.
- `pidstat` - показывает как именно используется процессор в резрезе user и system time
```bash
> pidstat -p [pid] [interval] [times]
```
- `sar -q` - показывает величину очереди на выполнение (run-queue). Можно посмотреть всё что угодно.
- *runq-sz* - кол-во процессов в очереди
- *plist-sz* - кол-во процессов
- *ldavg-1/5/15* - load average
```bash
> sar -q [interval] [times]
```

## Память 
- Виртуальная - память процесс запросил себе и он видит.
- Физическая - та память, которая физически выделена ядром процессу.
- Page cache - такая память, которую ядро использует для улучшения, ускорения работы с дисками.

- `vmstat` -  выводит общесистемную статистику использования виртуальной и физической памяти.
- `sar`:
	- `-B` - статистика paging
	- `-H` - сататистика huge pages
	- `-r` - использование памяти
```
> sar [flag] [interval] [times]
```
- `ps`
- `top`
- `free -m`
- `valgrind`
- `watch -n 0.2` - оч классная штука

### Счётчики
- */proc/meminfo* - общесистемные счётчики памяти
- */proc/zoneinfo* - статистика по зонам памяти

## Дисковое IO
![[Pasted image 20221014151319.png]]
![[Pasted image 20221014151556.png]]

- `iostat`
- `iotop`
- `smartctl`
- `blkid` - показывает каждый диска и его UUID
- `blockdev`
- `sar -d`
- `pidstat -d`
- `blktrace` - трасировка блочного I/O устройства
- MegaCli - статистика и настройка LSI RAID контролера

## Сеть
![[Pasted image 20221014153124.png]]

![[Pasted image 20221014153247.png]]
![[Pasted image 20221014154351.png]]
![[Pasted image 20221014154713.png]]
![[Pasted image 20221014154819.png]]![[Pasted image 20221014154912.png]]
![[Pasted image 20221014155045.png]]
![[Pasted image 20221014155129.png]]
**![[Pasted image 20221014155241.png]]**