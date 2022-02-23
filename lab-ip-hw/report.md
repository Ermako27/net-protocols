# Топология сети
![](before-routes.png)
# Назначеие IP-адресов
Содержимое файла настройки протокола ip маршрутизатора r1
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.0.20.1
netmask 255.255.255.0
up ip r add 10.0.40.0/24 via 10.0.20.2 dev eth0
up ip r add 10.0.50.0/24 via 10.0.20.2 dev eth0
down ip r del 10.0.40.0/24
down ip r del 10.0.50.0/24

auto eth1
iface eth1 inet static
address 10.0.10.1
netmask 255.255.255.0
up ip r add 10.0.30.0/24 via 10.0.10.2 dev eth1
down ip r del 10.0.30.0/24
```

Содержимое файла настройки протокола ip маршрутизатора r2
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.0.30.1
netmask 255.255.255.0
up ip r add 10.0.40.0/24 via 10.0.30.2 dev eth0
down ip r del 10.0.40.0/24

auto eth1
iface eth1 inet static
address 10.0.10.2
netmask 255.255.255.0
up ip r add 10.0.20.0/24 via 10.0.10.1 dev eth1
up ip r add 10.0.50.0/24 via 10.0.10.1 dev eth1
down ip r del 10.0.20.0/24
down ip r del 10.0.50.0/24
```

Содержимое файла настройки протокола ip маршрутизатора r3
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.0.20.2
netmask 255.255.255.0
up ip r add 10.0.10.0/24 via 10.0.20.1 dev eth0
down ip r del 10.0.10.0/24

auto eth1
iface eth1 inet static
address 10.0.40.1
netmask 255.255.255.0
up ip r add 10.0.30.0/24 via 10.0.40.2 dev eth1
down ip r del 10.0.30.0/24

auto eth2
iface eth2 inet static
address 10.0.50.1
netmask 255.255.255.0
```

Содержимое файла настройки протокола ip маршрутизатора r4
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.0.40.2
netmask 255.255.255.0
up ip r add 10.0.20.0/24 via 10.0.40.1 dev eth0
up ip r add 10.0.50.0/24 via 10.0.40.1 dev eth0
down ip r del 10.0.20.0/24
down ip r del 10.0.50.0/24

auto eth1
iface eth1 inet static
address 10.0.30.2
netmask 255.255.255.0
up ip r add 10.0.10.0/24 via 10.0.30.1 dev eth1
down ip r del 10.0.10.0/24
```

Содержимое файла настройки протокола ip рабочей станции ws1
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.0.10.3
netmask 255.255.255.0
gateway 10.0.10.2
```

Содержимое файла настройки протокола ip рабочей станции ws2
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.0.50.2
netmask 255.255.255.0
gateway 10.0.50.1
```
# Таблицы маршрутизации
Для маршрутизатора r1
```
r1:~# ip r
10.0.20.0/24 dev eth0  proto kernel  scope link  src 10.0.20.1 
10.0.50.0/24 via 10.0.20.2 dev eth0 
10.0.30.0/24 via 10.0.10.2 dev eth1 
10.0.40.0/24 via 10.0.20.2 dev eth0 
10.0.10.0/24 dev eth1  proto kernel  scope link  src 10.0.10.1
```

Для маршрутизатора r2
```
r2:~# ip r
10.0.20.0/24 via 10.0.10.1 dev eth1 
10.0.50.0/24 via 10.0.10.1 dev eth1 
10.0.30.0/24 dev eth0  proto kernel  scope link  src 10.0.30.1 
10.0.40.0/24 via 10.0.30.2 dev eth0 
10.0.10.0/24 dev eth1  proto kernel  scope link  src 10.0.10.2 

```

Для маршрутизатора r3
```
r3:~# ip r
10.0.20.0/24 dev eth0  proto kernel  scope link  src 10.0.20.2 
10.0.50.0/24 dev eth2  proto kernel  scope link  src 10.0.50.1 
10.0.30.0/24 via 10.0.40.2 dev eth1 
10.0.40.0/24 dev eth1  proto kernel  scope link  src 10.0.40.1 
10.0.10.0/24 via 10.0.20.1 dev eth0 
```

Для маршрутизатора r4
```
r4:~# ip r
10.0.20.0/24 via 10.0.40.1 dev eth0 
10.0.50.0/24 via 10.0.40.1 dev eth0 
10.0.30.0/24 dev eth1  proto kernel  scope link  src 10.0.30.2 
10.0.40.0/24 dev eth0  proto kernel  scope link  src 10.0.40.2 
10.0.10.0/24 via 10.0.30.1 dev eth1 
```

Для рабочей станции ws1
```
ws1:~# ip r
10.0.10.0/24 dev eth0  proto kernel  scope link  src 10.0.10.3 
default via 10.0.10.1 dev eth0 
```

Для рабочей станции ws2
```
ws2:~# ip r
10.0.50.0/24 dev eth0  proto kernel  scope link  src 10.0.50.2 
default via 10.0.50.1 dev eth0 
```
# Проверка настройки сети
```
ws1:~# traceroute -n 10.0.50.2
traceroute to 10.0.50.2 (10.0.50.2), 64 hops max, 40 byte packets
 1  10.0.10.1  8 ms  0 ms  0 ms
 2  10.0.20.2  11 ms  0 ms  1 ms
 3  10.0.50.2  12 ms  3 ms  2 ms
```
# MAC-адреса
маршрутизатор r1
```
r1:~# ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 16436 qdisc noqueue 
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: teql0: <NOARP> mtu 1500 qdisc noop qlen 100
    link/void 
3: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast qlen 1000
    link/ether 0e:ab:f8:0c:10:4b brd ff:ff:ff:ff:ff:ff
4: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast qlen 1000
    link/ether fa:de:dc:30:96:57 brd ff:ff:ff:ff:ff:ff
```

маршрутизатор r2
```
r2:~# ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 16436 qdisc noqueue 
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: teql0: <NOARP> mtu 1500 qdisc noop qlen 100
    link/void 
3: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast qlen 1000
    link/ether 3a:40:ee:31:9e:cd brd ff:ff:ff:ff:ff:ff
4: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast qlen 1000
    link/ether 12:3e:e2:7d:e3:87 brd ff:ff:ff:ff:ff:ff
```

маршрутизатор r3
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

маршрутизатор r4
```
r4:~# ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 16436 qdisc noqueue 
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: teql0: <NOARP> mtu 1500 qdisc noop qlen 100
    link/void 
3: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast qlen 1000
    link/ether 4a:e4:d9:3b:f2:04 brd ff:ff:ff:ff:ff:ff
4: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast qlen 1000
    link/ether 42:9b:97:db:b0:a6 brd ff:ff:ff:ff:ff:ff
```

рабочая станиция ws1
```
ws1:~# ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 16436 qdisc noqueue 
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: teql0: <NOARP> mtu 1500 qdisc noop qlen 100
    link/void 
3: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast qlen 1000
    link/ether a6:f9:52:b6:1e:69 brd ff:ff:ff:ff:ff:ff
```

рабочая станиция ws2
```
ws2:~# ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 16436 qdisc noqueue 
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: teql0: <NOARP> mtu 1500 qdisc noop qlen 100
    link/void 
3: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast qlen 1000
    link/ether da:53:12:09:ea:4e brd ff:ff:ff:ff:ff:ff
```
# Маршрутизация
Опыт проведен при пустом ARP кеше. Для проверки работы маршрутизации был сделан эхо-запрос с рабочей станции ws1 на рабочую станицю ws2
```
ws1:~# ping 10.0.50.2 -c 1
PING 10.0.50.2 (10.0.50.2) 56(84) bytes of data.
64 bytes from 10.0.50.2: icmp_seq=1 ttl=62 time=31.4 ms

--- 10.0.50.2 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 31.407/31.407/31.407/0.000 ms
```

Перехваченный пакет на маршрутизаторе r1
```
r1:~# tcpdump -tne -i eth1
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on eth1, link-type EN10MB (Ethernet), capture size 96 bytes
a6:f9:52:b6:1e:69 > ff:ff:ff:ff:ff:ff, ethertype ARP (0x0806), length 42: arp who-has 10.0.10.1 tell 10.0.10.3
fa:de:dc:30:96:57 > a6:f9:52:b6:1e:69, ethertype ARP (0x0806), length 42: arp reply 10.0.10.1 is-at fa:de:dc:30:96:57
a6:f9:52:b6:1e:69 > fa:de:dc:30:96:57, ethertype IPv4 (0x0800), length 98: 10.0.10.3 > 10.0.50.2: ICMP echo request, id 18178, seq 1, length 64
fa:de:dc:30:96:57 > a6:f9:52:b6:1e:69, ethertype IPv4 (0x0800), length 98: 10.0.50.2 > 10.0.10.3: ICMP echo reply, id 18178, seq 1, length 64
fa:de:dc:30:96:57 > a6:f9:52:b6:1e:69, ethertype ARP (0x0806), length 42: arp who-has 10.0.10.3 tell 10.0.10.1
a6:f9:52:b6:1e:69 > fa:de:dc:30:96:57, ethertype ARP (0x0806), length 42: arp reply 10.0.10.3 is-at a6:f9:52:b6:1e:69
```

Перехваченный пакет на маршрутизаторе r3
```
r3:~# tcpdump -tne
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), capture size 96 bytes
0e:ab:f8:0c:10:4b > ff:ff:ff:ff:ff:ff, ethertype ARP (0x0806), length 42: arp who-has 10.0.20.2 tell 10.0.20.1
ee:97:f2:ab:47:0c > 0e:ab:f8:0c:10:4b, ethertype ARP (0x0806), length 42: arp reply 10.0.20.2 is-at ee:97:f2:ab:47:0c
0e:ab:f8:0c:10:4b > ee:97:f2:ab:47:0c, ethertype IPv4 (0x0800), length 98: 10.0.10.3 > 10.0.50.2: ICMP echo request, id 18178, seq 1, length 64
ee:97:f2:ab:47:0c > 0e:ab:f8:0c:10:4b, ethertype IPv4 (0x0800), length 98: 10.0.50.2 > 10.0.10.3: ICMP echo reply, id 18178, seq 1, length 64
ee:97:f2:ab:47:0c > 0e:ab:f8:0c:10:4b, ethertype ARP (0x0806), length 42: arp who-has 10.0.20.1 tell 10.0.20.2
0e:ab:f8:0c:10:4b > ee:97:f2:ab:47:0c, ethertype ARP (0x0806), length 42: arp reply 10.0.20.1 is-at 0e:ab:f8:0c:10:4b
```

Принятый пакет на рабочей станции ws2
```
ws2:~# tcpdump -tne
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), capture size 96 bytes
c2:57:e2:f3:3f:00 > ff:ff:ff:ff:ff:ff, ethertype ARP (0x0806), length 42: arp who-has 10.0.50.2 tell 10.0.50.1
da:53:12:09:ea:4e > c2:57:e2:f3:3f:00, ethertype ARP (0x0806), length 42: arp reply 10.0.50.2 is-at da:53:12:09:ea:4e
c2:57:e2:f3:3f:00 > da:53:12:09:ea:4e, ethertype IPv4 (0x0800), length 98: 10.0.10.3 > 10.0.50.2: ICMP echo request, id 18178, seq 1, length 64
da:53:12:09:ea:4e > c2:57:e2:f3:3f:00, ethertype IPv4 (0x0800), length 98: 10.0.50.2 > 10.0.10.3: ICMP echo reply, id 18178, seq 1, length 64
da:53:12:09:ea:4e > c2:57:e2:f3:3f:00, ethertype ARP (0x0806), length 42: arp who-has 10.0.50.1 tell 10.0.50.2
c2:57:e2:f3:3f:00 > da:53:12:09:ea:4e, ethertype ARP (0x0806), length 42: arp reply 10.0.50.1 is-at c2:57:e2:f3:3f:00
```
# Продолжительность жизни пакета
Для того чтобы отследить продолжительность жизни отправленного пакета на маршрутизаторе r3 был выключен интерфей eth2 в сети 10.0.50.0/24 и добавлен неверный маршрут
```
r3:~# ip link set eth2 down
r3:~# ip route add 10.0.50.0/24 via 10.0.20.1 dev eth0
``` 

Таблица маршрутизации в r3
```
r3:~# ip r
10.0.20.0/24 dev eth0  proto kernel  scope link  src 10.0.20.2 
10.0.50.0/24 via 10.0.20.1 dev eth0 
10.0.30.0/24 via 10.0.40.2 dev eth1 
10.0.40.0/24 dev eth1  proto kernel  scope link  src 10.0.40.1 
10.0.10.0/24 via 10.0.20.1 dev eth0 
```

Из-за истечения ttl появляется ошибка
```
ws1:~# ping 10.0.50.2
PING 10.0.50.2 (10.0.50.2) 56(84) bytes of data.
From 10.0.20.2 icmp_seq=1 Time to live exceeded
```
# IP-фрагментация
Установим в интерфейсах, подключенных к сети 10.0.20.0/24 MTU=576

Маршрутизатор r1
```
r1:~# ip link set dev eth0 mtu 576
r1:~# ip l
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 16436 qdisc noqueue 
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: teql0: <NOARP> mtu 1500 qdisc noop qlen 100
    link/void 
3: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 576 qdisc pfifo_fast qlen 1000
    link/ether 0e:ab:f8:0c:10:4b brd ff:ff:ff:ff:ff:ff
4: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast qlen 1000
    link/ether fa:de:dc:30:96:57 brd ff:ff:ff:ff:ff:ff
r1:~# 
```

Маршрутизатор r3
```
r3:~# ip link set dev eth0 mtu 576
r3:~# ip l
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 16436 qdisc noqueue 
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: teql0: <NOARP> mtu 1500 qdisc noop qlen 100
    link/void 
3: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 576 qdisc pfifo_fast qlen 1000
    link/ether ee:97:f2:ab:47:0c brd ff:ff:ff:ff:ff:ff
4: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast qlen 1000
    link/ether d2:90:43:d2:95:19 brd ff:ff:ff:ff:ff:ff
5: eth2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast qlen 1000
    link/ether c2:57:e2:f3:3f:00 brd ff:ff:ff:ff:ff:ff
```

Выключаем защиту от фрагментации PMTU на машине ws1
```
echo 1 > /proc/sys/net/ipv4/ip_no_pmtu_disc
```

Посылаем эхо запрос с пакетом величиной 1000 
```
ping -c 1 -s 1000 10.0.50.2
```

На маршрутизатор r1 приходит целый пакет
```
r1:~# tcpdump -tnv -i eth1 icmp
tcpdump: listening on eth1, link-type EN10MB (Ethernet), capture size 96 bytes
IP (tos 0x0, ttl 64, id 25781, offset 0, flags [none], proto ICMP (1), length 1028) 10.0.10.3 > 10.0.50.2: ICMP echo request, id 18946, seq 1, length 1008
IP (tos 0x0, ttl 62, id 28225, offset 0, flags [none], proto ICMP (1), length 1028) 10.0.50.2 > 10.0.10.3: ICMP echo reply, id 18946, seq 1, length 1008
```

Для дальнешей передаче пакета он фрагментируется на 2 пакета меньшего размера, это видно на маршрутизаторе r3 - туда пришло 2 пакета
```
r3:~# tcpdump -tnv -i eth0 icmp
tcpdump: listening on eth0, link-type EN10MB (Ethernet), capture size 96 bytes
IP (tos 0x0, ttl 63, id 25781, offset 0, flags [+], proto ICMP (1), length 572) 10.0.10.3 > 10.0.50.2: ICMP echo request, id 18946, seq 1, length 552
IP (tos 0x0, ttl 63, id 25781, offset 552, flags [none], proto ICMP (1), length 476) 10.0.10.3 > 10.0.50.2: icmp
IP (tos 0x0, ttl 63, id 28225, offset 0, flags [+], proto ICMP (1), length 572) 10.0.50.2 > 10.0.10.3: ICMP echo reply, id 18946, seq 1, length 552
IP (tos 0x0, ttl 63, id 28225, offset 552, flags [none], proto ICMP (1), length 476) 10.0.50.2 > 10.0.10.3: icmp
```

При передаче пакета на машину ws2 в сеть 10.0.50.0/24 пакет был дефрагментирован на маршрутизаторе r3, так как в интерфейсе, подключенному к этой сети, величина MTU=1500
```
ws2:~# tcpdump -tnv -i eth0 icmp
tcpdump: listening on eth0, link-type EN10MB (Ethernet), capture size 96 bytes
IP (tos 0x0, ttl 62, id 25781, offset 0, flags [none], proto ICMP (1), length 1028) 10.0.10.3 > 10.0.50.2: ICMP echo request, id 18946, seq 1, length 1008
IP (tos 0x0, ttl 64, id 28225, offset 0, flags [none], proto ICMP (1), length 1028) 10.0.50.2 > 10.0.10.3: ICMP echo reply, id 18946, seq 1, length 1008
```

