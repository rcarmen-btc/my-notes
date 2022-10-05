```bash
sudo dnf install nftables
```

Весь набор правил по-умолчаню.
```bash
nft list ruleset 
```

Сущеструет 6 семейств таблиц:
1. IP - 
2. IP6 - 
4. ARP - 
3. INET - 
5. BRIDGE - 
6. NETDEV - 

Добавление таблицы  my_table 
```bash
nft add table inet my_table
```

Добавить цепочку в таблицу
```bash
nft add chain inet my_table my_filter_chain { type filter hook input priority 0 \; } 
```

Добавить правило для фаервола
```bash
nft add rule inet my_table my_filter_chain tcp dport ssh accept
```

`insert` если хочешь добавить правило в начало

Добавим в начало правило разрешающее траФик с протоколом HTTP
```bash
nft insert rule inet my_table my_filter_chain tcp dport http accept
```

`index` указать индекс правила

Добавим правило разрешающее трафик с протоколом nfs
```bash
nft add rule inet my_table my_filter_chain index 1 tcp dport nfs accept 
```

**Handle** - уникальный номер правил
```bash
nft --handle list ruleset  
```

Добавим правило разрешающее трафик с протоколом nfs через handle
```bash
nft add rule inet my_table my_filter_chain handle 1 tcp dport nfs accept 
```

Удаление правил
```bash
nft delete rule inet my_table my_filter_chain handle 5
```

nftables поддерживает наборы правил, создание набора
```bash 
nft add rule inet my_table my_filter_chain tcp dport { ssh, http } accept 
```

Создать изменяемые наборы
```bash
nft add set inet my_table my_set { type ipv4_addr \; }
```

Добавить в набор правило
```bash 
nft add rule inet my_table my_filter_chain ip saddr @my_set drop 
```

Добавить в набор элементы
```bash 
nft add element inet my_table my_set { 10.10.10.22, 10.10.10.33 }
```

Показать изменяемые наборы
```bash
nft list sets
```