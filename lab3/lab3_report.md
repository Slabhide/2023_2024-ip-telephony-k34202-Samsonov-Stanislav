University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)  
Course: [IP-telephony](https://github.com/itmo-ict-faculty/ip-telephony)  
Year: 2023/2024  
Group: K34202  
Author: Samsonov Stanislav
Lab: Lab2  
Date of create: 04.03.2024  
Date of finished: - 

# Лабораторная работа №3
# "Использование Asterisk в качестве SIP proxy"

**Цель работы**  
Изучить программный комплекс Asterisk. Настройка Asterisk для локальных звонков.

## Ход работы

### Конфигурация Asterisk
1. Был установлен Asterisk
```
sudo apt install asterisk
```
2. Далее были настроены файлы конфигураций sip.conf и extensions.conf
```
[1001]
type=friend
host=dynamic
secret=111
context=ext_1001

[1002]
type=friend
host=dynamic
secret=222
context=ext_1002

[internal_calls]
exten => 1001,1,Dial(SIP/1002)
exten => 1002,1,Dial(SIP/1001)

[default]
include => internal_calls
```
-

```
[ext_1001]
exten => _XXXX,1,Dial(SIP/${EXTEN})

[ext_1002]
exten => _XXXX,1,Dial(SIP/${EXTEN})
```

2. Для применения настроек Asterisk был перезагружен:
```
sudo astrisk -r
relad
```
3. Далее была выполнена проверка созданных подключений:
```
sip show peer
```

### Установка Zoiper и MicroSIP

Далее в качестве soft-телефона с официального сайта был установлен Zoiper.
![.]()  
Далее была произведена настройка MicroSIP:
![.]()  
Для проверки успешной настройки связи был совершен звонок на телефон 1000 с телефона 1001. Как видно, соединение было установлено, на экране виден номер звонящего телефона:
![.]()  
## Вывод 
В ходе лабораторной работы был настроен Asterisk для локальных звонков, в результате чего успешно установлена связь между софтфонами.




