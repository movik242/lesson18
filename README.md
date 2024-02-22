# lesson18
**ДЗ Архитектура сетей (Centos 7)**

построить следующую архитектуру

Сеть office1

192.168.2.0/26 - dev </br>
192.168.2.64/26 - test servers </br>
192.168.2.128/26 - managers </br>
192.168.2.192/26 - office hardware </br>

Сеть office2 </br>
192.168.1.0/25 - dev </br>
192.168.1.128/26 - test servers </br>
192.168.1.192/26 - office hardware </br>

Сеть central </br>
192.168.0.0/28 - directors </br>
192.168.0.32/28 - office hardware </br>
192.168.0.64/26 - wifi </br>
```
Office1 ---\ 
----> Central --IRouter --> internet </br>
Office2----/ 
```
```
Теоретическая часть
- Найти свободные подсети
- Посчитать сколько узлов в каждой подсети, включая свободные
- Указать broadcast адрес для каждой подсети
- проверить нет ли ошибок при разбиении

Практическая часть
- Соединить офисы в сеть согласно схеме и настроить роутинг
- Все сервера и роутеры должны ходить в инет черз inetRouter
- Все сервера должны видеть друг друга
- у всех новых серверов отключить дефолт на нат (eth0), который вагрант поднимает для связи
- при нехватке сетевых интервейсов добавить по несколько адресов на интерфейс
```

#ТЕОРИЯ
```
**Сеть office1**
- 192.168.2.0/26 - dev 
Network:   192.168.2.0/26
Broadcast: 192.168.2.63
HostMin:   192.168.2.1
HostMax:   192.168.2.62
Hosts: 62
- 192.168.2.64/26 - test servers 
Network:   192.168.2.64/26
Broadcast: 192.168.2.127
HostMin:   192.168.2.65
HostMax:   192.168.2.126
Hosts: 62
- 192.168.2.128/26 - managers
Network:   192.168.2.128/26
Broadcast: 192.168.2.191
HostMin:   192.168.2.129
HostMax:   192.168.2.190
Hosts: 62
- 192.168.2.192/26 - office hardware
Network:   192.168.2.192/26
Broadcast: 192.168.2.255
HostMin:   192.168.2.193
HostMax:   192.168.2.254
Hosts: 62

**Сеть office2**
- 192.168.1.0/25 - dev 
Network:   192.168.1.0/25
Broadcast: 192.168.1.127
HostMin:   192.168.1.1
HostMax:   192.168.1.126
Hosts: 126
- 192.168.1.128/26 - test servers
Network:   192.168.1.128/26
Broadcast: 192.168.1.191
HostMin:   192.168.1.129
HostMax:   192.168.1.190
Hosts: 62
- 192.168.1.192/26 - office hardware 
Network:   192.168.1.192/26
Broadcast: 192.168.1.255
HostMin:   192.168.1.193
HostMax:   192.168.1.254
Hosts: 62

**Сеть central**
- 192.168.0.0/28 - directors 
Network:   192.168.0.0/28
Broadcast: 192.168.0.15
HostMin:   192.168.0.1
HostMax:   192.168.0.14
Hosts: 14       

- 192.168.0.32/28 - office hardware
Network:   192.168.0.32/28
Broadcast: 192.168.0.47
HostMin:   192.168.0.33
HostMax:   192.168.0.46
Hosts: 14 

- 192.168.0.64/26 - wifi
Network:   192.168.0.64/26
Broadcast: 192.168.0.127
HostMin:   192.168.0.65
HostMax:   192.168.0.126
Hosts: 62

**Свободные подсети**

- 192.168.0.16/28 - Свободна
- Broadcast: 192.168.0.31

- 192.168.0.48/28 - Свободна
- Broadcast: 192.168.0.63

- 192.168.0.128/25 - Свободна
- Broadcast: 192.168.0.255
```
#ПРАКТИКА

![arch](https://github.com/movik242/lesson18/assets/143793993/0922786e-7acc-44a7-8e8f-8aeb208c86e6)

Выход и интернет через inetRouter

![image](https://github.com/movik242/lesson18/assets/143793993/637716af-1712-48b6-b234-d0972f4a6ea8)

В виде проверки, на скриншоте видно, что выход в инет настроен по разным ip на centralRouter

![image](https://github.com/movik242/lesson18/assets/143793993/bd2ac9d6-e0b3-4884-be51-d525cfef5d09)

![image](https://github.com/movik242/lesson18/assets/143793993/607135bc-457c-42a9-945e-c73462cff098)

Возникли трудности при выполнении ДЗ:

Если в vagrantfile указана строка, как в методичке
```
    box.vm.network("private_network", ip: ipconf[0], adapter: ipconf[1], netmask: ipconf[2], virtualbox__intnet: ipconf[3])
```
То при запуске vagrant up, выходит ошибка

![image](https://github.com/movik242/lesson18/assets/143793993/f7ed56e3-bef2-4d47-a77a-a2c21d300bcc)

Если в vagrantfile указана строка
```
    box.vm.network "private_network", ipconf
```
То при запуске vagrant up, выходит ошибка

![image](https://github.com/movik242/lesson18/assets/143793993/207c5878-8777-4c41-88a0-280063743cb1)

В итоге помогли с решением в чате телеграмм, следующая строка помогла решить возникающие ошибки при запуске vagrant
```
   box.vm.network("private_network", **ipconf)
```
Достаточно было добавить ** и установка прошла правильно


