University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)  
Course: [IP-telephony](https://github.com/itmo-ict-faculty/ip-telephony)  
Year: 2023/2024  
Group: K34202  
Author: Samsonov Stanislav
Lab: Lab2  
Date of create: 04.03.2024  
Date of finished: - 

---
# Отчет по лабораторной работе №2  
## "Конфигурация voip в среде Сisco packet tracer"  

### Цель работы  
Изучить построение сети IP-телефонии с помощью маршрутизатора Cisco 2811, коммутатора Cisco catalyst 3560 и IP телефонов Cisco 7960.  

## Ход работы  
### Часть 1
В Сisco packet tracer была собрана схема с роутером, коммутатором и 3 телефонами:  
![.](https://github.com/Slabhide/2023_2024-ip-telephony-k34202-Samsonov-Stanislav/blob/main/lab2/img/1.jpg)  
В конфигурационном режиме было изменено название маршрутизатора на CMERouter командой ```hostname CMERouter```.  
Синтаксис ввода слов от DNS серверов был отключен командой ```no ip domain-lookup```.  
Для защиты маршрутизатора в удаленном режиме и в режиме консоли были заданы пароли командами:  
```
line vty 0 4
password 123
login
exit
lie console 0
password 123
login
```
Далее был настрен интерфейс fa0/0 на маршрутизаторе: выдан IP-адрес и маска, переведен в режим On. Также был настроен DHCP пул с указанием сети и выданного роутеру адреса 192.168.0.1, и задана опция 150 для работы IP-телефонии командами:  
```
ip dhcp pool voice
network 192.168.0.0 255.255.255.0
default-router 192.168.0.1
option 150 ip 192.168.0.1
```
На роутере также был создан сервис IP-телефонии, вмещающий 10 устройств, задан порт маршрутизатора и количество линий командами:  
```
telephony-service
max-dn 10
max-ephones 10
ip source-address 192.168.0.1 port 3100
auto assign 1 to 19
```  
Далее был создан VLAN на коммутаторе для всех занятых портов командами:  
```
interface range fa0/1-4
switchport mode access
switchport voice vlan 1
```
На роутере была продолжена настройка телефонии:  
```
ephone-dn 1
number 001
exit
ephone-dn 2
number 002
exit
ephone-dn 3
number 003
```  
Телефоны были подключены к блоку питания в окне физического представления. Работоспособность была проверена путем звонка:  
![.](https://github.com/Slabhide/2023_2024-ip-telephony-k34202-Samsonov-Stanislav/blob/main/lab2/img/2.jpg)  
### Часть 2
Далее была собрана схема соединения как в 1 части, но с добавлением 3 PC, подключенных к телефонам:  
![.](https://github.com/Slabhide/2023_2024-ip-telephony-k34202-Samsonov-Stanislav/blob/main/lab2/img/3.jpg)  
На коммутаторе были созданы VLANы для взаимодействия коммутатора с маршрутизатором, телефонами и компьютерами командами:  
```
vlan 10
name data
exit
vlan 20
name voice
exit
vlan 30
name router
```
Интерфейсы, ведущие к телефонам были переведены в режим access и подключены к VLANам для телефонов и компьютеров командами:  
```
interface range fa0/2-4
switchport mode access
switchport access vlan 10
switchport voice vlan 20
```  
Интерфейс, ведущий к роутеру был переведен в режим trunk, а VLAN был включен и был выдан адрес из подсети 192.168.1.0/24 командами:  
```
interface vlan 30
ip address 192.168.2.1 255.255.255.0
no shutdown
interface fa0/1
switchport mode trunk
switchport trunk native vlan 30
```  
Также был задан маршрут по умолчанию на сеть 192.168.2.0 командой ```ip default-network 192.168.2.0```.  
На роутере были созданы логические сабинтерфейсы для VLANов командами (x – номер VLAN):  
```
interface FastEthernet0/0.x
encapsulation dot1Q x
ip add 192.168.x.10 255.255.255.0
no shutdown
```
Далее был наспроен DHCP для передачи голоса и данных командами:  
```
ip dhcp excluded-address 192.168.10.10
ip dhcp excluded-address 192.168.20.10
ip dhcp pool data
network 192.168.10.0 255.255.255.0
default-router 192.168.10.10
exit
ip dhcp pool voice
network 192.168.20.0 255.255.255.0
default-router 192.168.20.10
option 150 ip 192.168.20.10
```  
После данных команд, были подключены 3 телефона, которым сразу были выданы адреса из указанной подсети:  
![.](https://github.com/Slabhide/2023_2024-ip-telephony-k34202-Samsonov-Stanislav/blob/main/lab2/img/4.jpg)  
Также был создан сервис IP-телефонии, вмещающий 3 устройства, командами:  
```
telephony-service
max-dn 3
max-ephones 3
ip source-address 192.168.20.10 port 2000
```
На роутере была продолжена настройка телефонии, были выданы номера 001, 002 и 003 командами:  
```
ephone-dn 1
number 001
exit
ephone-dn 2
number 002
exit
ephone-dn 3
number 003
```
В результате все устройства получили IP-адреса из заданых подсетей, а телефоны еще и номера. С роутера пинги на все устройства прошли успешно:  
![.](https://github.com/Slabhide/2023_2024-ip-telephony-k34202-Samsonov-Stanislav/blob/main/lab2/img/5.jpg)  
Пинги с одного PC на другой также прошли успешно:  
![.](https://github.com/Slabhide/2023_2024-ip-telephony-k34202-Samsonov-Stanislav/blob/main/lab2/img/6.jpg)  
Звонки по номеру телефона тоже были успешны:  
![.](https://github.com/Slabhide/2023_2024-ip-telephony-k34202-Samsonov-Stanislav/blob/main/lab2/img/7.jpg)  

### Вывод  
В ходе лабораторной работы были собраны представленные схемы сети IP-телефонии с маршрутизатором, коммутатором и IP-телефонами, настроены устройства, в результате чего успешно установлена связь между IP-телефонами в первой части, а во второй части – еще и между компьютерами.
