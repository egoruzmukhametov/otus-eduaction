# Лабораторная работа. Просмотр таблицы MAC-адресов коммутатора 

![](https://github.com/egoruzmukhametov/otus-eduaction/blob/main/lesson4/topology2.png)
## Таблица адресации

  Устройство  |   Интерфейс   | IP-адрес / префикс
------------- | ------------- | -------------
sw1           | VLAN 1        | 192.168.11.251/24
sw2           | VLAN 1        | 192.168.11.252/24
PC1           | eth0          | 192.168.11.1/24
PC2           | eth0          | 192.168.11.2/24

## Конфигурация sw1
<details>
    <summary>Show run</summary>
####################################################
##############Authorized access only################
####################################################

User Access Verification

Password: 
sw1>ena
sw1>enable  
Password: 
sw1#sho
sw1#show r
sw1#show ru
sw1#show running-config 
Building configuration...

Current configuration : 1074 bytes
!
! Last configuration change at 06:52:26 EST Thu Nov 3 2022
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
line vty 0 4
 password 7 025756085F5359
 login
!
!
end
</details>
