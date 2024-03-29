University: [ITMO University](https://itmo.ru/ru/) <br/>
Faculty: [FICT](https://fict.itmo.ru) <br/>
Course: IP-telephony <br/>
Year: 2023/2024 <br/>
Group: K34202 <br/>
Author: Samsonov Stanislav <br/>
Lab: Lab1 <br/>
Date of create: 04.03.2024 <br/>
Date of finished: - <br/>

# Лабораторная работа №1 "Базовая настройка ip-телефонов в среде Сisco packet tracer"  

## Цель работы:  
   Изучить рабочую среду Cisco Packet Tracer, ознакомиться с интерфейсами основных устройств, типами кабелей, научиться собирать топологию. Изучить построение сети IP-телефонии с помощью маршрутизатора, коммутатора и IP телефонов Cisco 7960 в среде Packet tracer

## Ход работы:  
## Часть 1.  
   Первым делом была построена топология сети, которая включает компьютеры, коммутаторы.
   
![.](https://github.com/Slabhide/2023_2024-ip-telephony-k34202-Samsonov-Stanislav/blob/main/lab1/img/1.jpg)  

   Каждому компьютеру были назначены статитические IP-адреса 192.168.10.1-192.168.10.7/24.
   Далее проверяем доступность каждого узла.  
   
![.](https://github.com/Slabhide/2023_2024-ip-telephony-k34202-Samsonov-Stanislav/blob/main/lab1/img/2.jpg)  

## Часть 2.  
   Была простроена следующая топология.  
   
![.](https://github.com/Slabhide/2023_2024-ip-telephony-k34202-Samsonov-Stanislav/blob/main/lab1/img/3.jpg)  
   
Было изменено имя маршрутизатора командой hostname CMERouter.
Также был настроен интерфейс, идущий на коммутатор: включен и назначен адрес 192.168.0.1/24.
Далее был создан пул адресов для телефонов из подсети 192.168.0.0/24, был указан адрес маршрутизатора и задана опция 150 для работы IP-телефонии командами:
```
ip dhcp pool phones
network 192.168.0.0 255.255.255.0
default-router 192.168.0.1
option 150 ip 192.168.0.1
```
На роутере также был создан сервис IP-телефонии, вмещающий 10 устройств, порт маршрутизатора и количество линий командами:
```
max-dn 10
max-ephones 10
ip source-address 192.168.0.1 port 3100
auto assign 1 to 19
```
На коммутаторе была включена поддержка VoIP-интерфейсов:
```
interface range FastEthernet 0/1 - FastEthernet 0/3
switchport mode access
sitchport voice vlan 1
```
На роутере была продолжена настройка телефонии, были выданы номера:
```
ephone-dn 1
number 001
exit
ephone-dn 2
number 002
```
Телефоны были подключены к блоку питания в окне физического представления. Работоспособность была проверена путем звонка, также в физическом представлении был введен номер другого телефона:
![.](https://github.com/Slabhide/2023_2024-ip-telephony-k34202-Samsonov-Stanislav/blob/main/lab1/img/4.jpg)  
## Выводы: ##
В ходе лабораторной работы была построена топология сети, назначены IP-адреса и настроена IP-телефония.


