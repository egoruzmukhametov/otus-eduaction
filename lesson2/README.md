# Лабораторная работа. Базовая настройка коммутатора 
## Топология
![](https://github.com/egoruzmukhametov/otus-eduaction/blob/main/lesson2/topology.png)
## Таблица адресации

  Устройство  |   Интерфейс   | IP-адрес / префикс
------------- | ------------- | -------------
S1            | VLAN 1        | 192.168.11.251/24
Linux         | NIC           | 192.168.11.1/24

## Команды настройки S1

    Switch>enable 
    Switch#configure terminal 
    Enter configuration commands, one per line.  End with CNTL/Z.
    Switch(config)#hostname sw1
    sw1(config)#line console 0
    sw1(config-line)#password 123456
    sw1(config-line)#login
    sw1(config-line)#exit
    sw1(config)#line vty 0 4
    sw1(config-line)#password 123456
    sw1(config-line)#login
    sw1(config-line)#end
    sw1#
    *Nov  1 15:23:04.312: %SYS-5-CONFIG_I: Configured from console by console
    sw1#configure terminal 
    Enter configuration commands, one per line.  End with CNTL/Z.
    sw1(config)#line aux 0
    sw1(config-line)#login
    % Login disabled on line 1, until 'password' is set
    sw1(config-line)#exit
    sw1(config)#service password-encryption 
    sw1(config)#enable secret 123456
    sw1(config)#banner motd \Authorized access only!\
    sw1(config)#interface vlan 1
    sw1(config-if)#ip address 192.168.11.251 255.255.255.0
    sw1(config-if)#no shutdown 
    sw1(config-if)#exit
    sw1#copy running-config startup-config
    Destination filename [startup-config]? 
    Building configuration...
    Compressed configuration from 945 bytes to 685 bytes[OK]

## Конфигурация сети на Linux

![](https://github.com/egoruzmukhametov/otus-eduaction/blob/main/lesson2/linux_settings.png)

## Конфигурация S1

    sw1#show running-config 
    Building configuration...    

    Current configuration : 1081 bytes
    !
    ! Last configuration change at 07:37:48 EST Tue Nov 1 2022
    !
    version 15.2
    service timestamps debug datetime msec
    service timestamps log datetime msec
    service password-encryption
    service compress-config
    !
    hostname sw1
    !
    boot-start-marker
    boot-end-marker
    !
    !
    enable secret 5 $1$w74O$qpg6khGaz8LNmhzfuYJRU0
    !
    no aaa new-model
    clock timezone EST -5 0
    !
    !
    !         
    !         
    !         
    !         
    !         
    !         
    ip cef    
    no ipv6 cef
    !         
    !         
    spanning-tree mode pvst
    spanning-tree extend system-id
    !         
    vlan internal allocation policy ascending
    !         
    !         
    !         
    !         
    !         
    !         
    !
    !         
    !         
    !         
    interface Ethernet0/0
    !         
    interface Ethernet0/1
    !         
    interface Ethernet0/2
    !         
    interface Ethernet0/3
    !         
    interface Vlan1
     ip address 192.168.11.251 255.255.255.0
    !         
    ip forward-protocol nd
    !         
    no ip http server
    no ip http secure-server
    !         
    !         
    !         
    !         
    !         
    !         
    control-plane
    !         
    banner motd ^C
    ####################################################
    ##############Authorized access only################
    ####################################################^C
    !         
    line con 0
     password 7 101F5B4A514244
     logging synchronous
     login    
    line aux 0
     login    
    line vty 0 4
     password 7 025756085F5359
     login    
    !         
    !         
    end       

### Эхо запрос с sw1 до сервера Linux

    sw1>ping 192.168.11.1 
    Type escape sequence to abort.
    Sending 5, 100-byte ICMP Echos to 192.168.11.1, timeout is 2 seconds:
    !!!!!
    Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
    sw1>

### Получение доступа по telnet с сервера Linux на sw1

![](https://github.com/egoruzmukhametov/otus-eduaction/blob/main/lesson2/telnet.png)

# Тестовое окружение:
Лаборатория на базе eve-ng
