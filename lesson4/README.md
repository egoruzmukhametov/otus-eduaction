# Лабораторная работа. Просмотр таблицы MAC-адресов коммутатора 

![](https://github.com/egoruzmukhametov/otus-eduaction/blob/main/lesson4/topology2.png)
## Таблица адресации

  Устройство  |   Интерфейс   | IP-адрес / префикс  | mac-адрес интерфейса
------------- | ------------- | ------------------- | ---------------------
sw1           | VLAN 1        | 192.168.11.251/24   | aa:bb:cc:80:02:00 
sw2           | VLAN 1        | 192.168.11.252/24   | aa:bb:cc:80:03:00 
PC1           | eth0          | 192.168.11.1/24     | 50:00:00:01:00:00
PC2           | eth0          | 192.168.11.2/24     | 50:00:00:05:00:00

## Конфигурация sw1
<details>
  <summary>sw1#show running-config</summary>
     
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


## Конфигурация sw2
<details>
  <summary>sw2#show running-config</summary>

    Building configuration...    

    Current configuration : 1020 bytes
    !
    ! Last configuration change at 06:52:28 EST Thu Nov 3 2022
    !
    version 15.2
    service timestamps debug datetime msec
    service timestamps log datetime msec
    service password-encryption
    service compress-config
    !
    hostname sw2
    !
    boot-start-marker
    boot-end-marker
    !
    !
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
     ip address 192.168.11.252 255.255.255.0
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
    ###############################################
    #########Authorized access only!!##############
    ###############################################
    ^C
    !
    line con 0
     password 7 055A545C751918
     logging synchronous
     login
    line aux 0
     login
    line vty 0 4
     password 7 1446405858517C
     login
    !
    !
    end
</details>

## Конфигурация сети на PC1
<details>
  <summary>cat /etc/network/interfaces</summary>
    # This file describes the network interfaces available on your system
    # and how to activate them. For more information, see interfaces(5).    

    # The loopback network interface
    auto lo
    iface lo inet loopback    

    # The normal eth0
    auto eth0
    allow-hotplug eth0
    iface eth0 inet static
        address 192.168.11.1/24
        gateway 192.168.11.254
        hwaddress ether 50:00:00:01:00:00
        mtu 1500    
    

    # Additional interfaces, just in case we're using
    # multiple networks
    allow-hotplug eth1
    iface eth1 inet dhcp    

    allow-hotplug eth2
    iface eth2 inet dhcp    

    # Set this one last, so that cloud-init or user can
    # override defaults.
    source /etc/network/interfaces.d/*

</details>



## Конфигурация сети на PC2
<details>
  <summary>cat /etc/network/interfaces</summary>
    # This file describes the network interfaces available on your system
    # and how to activate them. For more information, see interfaces(5).    

    # The loopback network interface
    auto lo
    iface lo inet loopback    

    # The normal eth0
    auto eth0
    allow-hotplug eth0
    iface eth0 inet static
        address 192.168.11.2/24
        gateway 192.168.11.254
        mtu 1500    

    # Additional interfaces, just in case we're using
    # multiple networks
    allow-hotplug eth1
    iface eth1 inet dhcp    

    allow-hotplug eth2
    iface eth2 inet dhcp    

    # Set this one last, so that cloud-init or user can
    # override defaults.
    source /etc/network/interfaces.d/*

</details>


#Таблица МАС-адресов коммутатора
    sw1#show mac address-table
              Mac Address Table
    -------------------------------------------    

    Vlan    Mac Address       Type        Ports
    ----    -----------       --------    -----
       1    5000.0001.0000    DYNAMIC     Et0/0
       1    5000.0004.0000    DYNAMIC     Et0/3
       1    5000.0005.0000    DYNAMIC     Et0/1
       1    aabb.cc00.0310    DYNAMIC     Et0/1
       1    aabb.cc80.0300    DYNAMIC     Et0/1
    Total Mac Addresses for this criterion: 5
Здесь мы видим mac-адреса устройств подключенных напрямую к портам коммутатора, таким как Et0/0, Et0/3, Et0/1, а также mac-адреса полученные в результате broadcast-запроса, которые находятся в одной сети, но за одним интерфейсом Et0/1 
После выполнения команды очистки, видны только адреса непосредственно подключенные к коммутатору
    sw1#show mac address-table
              Mac Address Table
    -------------------------------------------    

    Vlan    Mac Address       Type        Ports
    ----    -----------       --------    -----
       1    5000.0004.0000    DYNAMIC     Et0/3
       1    aabb.cc00.0310    DYNAMIC     Et0/1
       1    aabb.cc80.0200    DYNAMIC     Et0/1
#Эхо-запросы с комьютера PC2
По мере того как я пинговал разные устройства в моей сети, я получал новые записи arp, по итогу получив следующую запись:
    root@pc2:~# arp -a
    ? (192.168.11.252) at aa:bb:cc:80:03:00 [ether] on eth0
    ? (192.168.11.251) at aa:bb:cc:80:02:00 [ether] on eth0
    ? (192.168.11.200) at 50:00:00:04:00:00 [ether] on eth0
    ? (192.168.11.1) at 50:00:00:01:00:00 [ether] on eth0
