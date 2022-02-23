# Таблица маршрутизации
```
r3:~# ip r
10.0.20.0/24 dev eth0  proto kernel  scope link  src 10.0.20.2 
10.0.50.0/24 dev eth2  proto kernel  scope link  src 10.0.50.1 
10.0.30.0/24 via 10.0.40.2 dev eth1 
10.0.40.0/24 dev eth1  proto kernel  scope link  src 10.0.40.1 
10.0.10.0/24 via 10.0.20.1 dev eth0 
```
на стр 47

src - источник записи (запись появилось из-за наличия этого адреса)

scope - область действия. значение link означает, что эта запись
верна только в пределах указанного в ней сетевого интерфейса, поскольку указанная сеть достижима именно через него. 

локальный интерфейс не показывается в таблице потому что он должен быть изолирован

Пометка proto kernel в таблице маршрутизации означает, что эта
строка появилась в силу действий ядра ОС

**Ссылки**
1)Маршрутизатор по умолчанию (внизу есть ссылки на rfc) https://ru.wikipedia.org/wiki/%D0%A8%D0%BB%D1%8E%D0%B7_%D0%BF%D0%BE_%D1%83%D0%BC%D0%BE%D0%BB%D1%87%D0%B0%D0%BD%D0%B8%D1%8E

# Маршрутизация

```
r3:~# ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 16436 qdisc noqueue 
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: teql0: <NOARP> mtu 1500 qdisc noop qlen 100
    link/void 
3: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast qlen 1000
    link/ether ee:97:f2:ab:47:0c brd ff:ff:ff:ff:ff:ff
4: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast qlen 1000
    link/ether d2:90:43:d2:95:19 brd ff:ff:ff:ff:ff:ff
5: eth2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast qlen 1000
    link/ether c2:57:e2:f3:3f:00 brd ff:ff:ff:ff:ff:ff
```
lo - loopback интерфейс

teql0 - Виртуальный интерфейс teqlo0 служит для объединения нескольких интерфейсов в один и использования его как единого интерфейса. стр38

qdisc - сокращение от 'queueing discipline' (дисциплина очередности) и является базовым для понимания контроля трафика. Всякий раз, когда ядру требуется отправить пакет на интерфейс, этот пакет ставится в очередь к qdisc, настроенный для этого интерфейса. Сразу же после этого ядро пытается получить сколько можно пакетов из qdisc для передачи их драйверу сетевого адаптера.
Простейший QDISC является очередью 'pfifo' без обработки, лишь реализующей принцип 'First In, First Out' (первый вошел - первый вышел). Однако он сохраняет трафик в очереди, если сетевой интерфейс не может обработать данные сразу же.

**Ссылки**
1) loopback https://ru.wikipedia.org/wiki/Loopback
2) qdisc https://www.opennet.ru/cgi-bin/opennet/man.cgi?topic=tc&category=8
3) qdisc тут лучше и понятнее - https://habr.com/ru/post/133076/
4) флаг LOWER_UP - https://qastack.ru/unix/335077/ip-link-and-ip-addr-output-meaning , флаг означает что есть сигнал на физическом уровне

# ARP
как выглядит arp кеш
```
r1:~# ip n
10.0.10.3 dev eth1 lladdr a6:f9:52:b6:1e:69 REACHABLE
10.0.20.2 dev eth0 lladdr ee:97:f2:ab:47:0c REACHABLE
```

в конце icmp сообщений делает еще один arp запрос, чтобы обновить данные в кеше (стр 44 последний абзац)  

**Ссылки**
1) https://habr.com/ru/post/80364/ - тут коротко и ясно про arp таблицу и определение маршрута
2) стр 41 

# IP фрагментация