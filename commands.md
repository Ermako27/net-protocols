## Работа с виртуальной машиной:
### Команды
1) vstart - старт виртуальной машины
```
vstart vm1 --eth0=net1 --eth1=net2
vstart vm2 --eth0=net2 --eth1=net3
vstart vm3 --eth0=net3 --eth1=net1
```
* vm1 - имя машины
* eth - етевой интерфейс машины
* net - сегмент сети
2) halt - завершение работы машины будучи на самой машине
3) vhalt имя машины- завершение работы машины будучи на своем компе
4) vcrash имя машины - если все зависло 
## Запуск лаб
1) ltabstart - запуск виртуальной сети
```
ltabstart -d net 
```
* net - имя каталога с лабой
2) lstart - если не рабоатет ltabstart
```
lstart -o "--con0=gnome-terminal" -d net
```
3) lhalt - завершить группу машин в составе лабы
4) lcrash - тоже самое что и lhalt, только удаляет образ диска

## Глава 2. Команды простейшей сети
1) ip link - получить сведения о сетевых интерфейсах
```
ws1:~# ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 16436 qdisc noqueue 
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: teql0: <NOARP> mtu 1500 qdisc noop qlen 100
    link/void 
3: eth0: <BROADCAST,MULTICAST> mtu 1500 qdisc noop qlen 1000
    link/ether a6:f9:52:b6:1e:69 brd ff:ff:ff:ff:ff:ff

```
2) ip addr - получение сведений об ip-адресах интерфейсов. сокращенно ip -4 a. -4 - только ipv4 адреса
```
ws1:~# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 16436 qdisc noqueue 
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: teql0: <NOARP> mtu 1500 qdisc noop qlen 100
    link/void 
3: eth0: <BROADCAST,MULTICAST> mtu 1500 qdisc noop qlen 1000
    link/ether a6:f9:52:b6:1e:69 brd ff:ff:ff:ff:ff:ff
ws1:~# ip -4 a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 16436 qdisc noqueue 
    inet 127.0.0.1/8 scope host lo
```
3) ping - отправка эхо-запроса к ip-адресу по протоколу сетевого уровня ICMP
```
ws1:~# ping 127.0.0.1
PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.225 ms
64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.181 ms
64 bytes from 127.0.0.1: icmp_seq=3 ttl=64 time=0.140 ms
64 bytes from 127.0.0.1: icmp_seq=4 ttl=64 time=0.382 ms
64 bytes from 127.0.0.1: icmp_seq=5 ttl=64 time=0.208 ms
64 bytes from 127.0.0.1: icmp_seq=6 ttl=64 time=0.263 ms
^C
--- 127.0.0.1 ping statistics ---
6 packets transmitted, 6 received, 0% packet loss, time 5031ms
rtt min/avg/max/mdev = 0.140/0.233/0.382/0.077 ms
```
4) назначение интерфейсу ip адреса и маски сети

ip a add 10.0.0.11/16 dev eth0  - назначить адрес\
ip l set eth0 up - поднять интерфейс

5) ip n - вывод ARP кеша
```
10.0.0.22 dev eth0 lladdr da:53:12:09:ea:4e STALE
```
## Глава 3

1) tcpdump -nt - перехват кадров канального уровня на сетевой карте
-n - простой вывод всех параметров
-t - отключение времени
-e - видеть полную информацию о mac адресах
2) ip n flush dev eth0 - очистить кеш протокола ARP
3) ip r - вывод таблицы маршрутизации
```
ws1:~# ip r
10.0.0.0/16 dev eth0  proto kernel  scope link  src 10.0.0.11 
```
4) tcpdump -tn -i eth0 - перехват трафика на определенном интерфейсе
5) tcpdump -tnve -i eth0 - перехват чтобы было видно ttl
6) ip link set dev eth1 mtu 576 - изменение величины mtu в маршрутизаторе
7) echo 1 > /proc/sys/net/ipv4/ip_no_pmtu_disc - отключение защиты от фрагментации ip-пакетов PMTU
8) ip link set eth1 down - отключить интерфейс
9) ifconfig eth1 down - отключить интерфейс