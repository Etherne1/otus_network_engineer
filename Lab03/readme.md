Т.к. нумерация интерфейсоd в образах устройств отличается от реального оборудования, вместо топологии:
![](https://github.com/Etherne1/otus_network_engineer/blob/main/Lab03/Pasted%20image%2020241020151723.png?raw=true)  


Получилась следующая:  
![](https://github.com/Etherne1/otus_network_engineer/blob/main/Lab03/Pasted%20image%2020241020152404.png?raw=true)

Т.е. нумерация интерфейсов на роутерах сменилась с формата Gi0/0/1 на e0/1; нумерация интерфейсов на свитчах с F0/18 на e8/1 и с F0/5 на e5/0 соответственно.
Логически схема не изменилась, но форму топологии поменял, для наглядности.

### Выполнение ДЗ:


|Device|Interface|IP Address|Subnet Mask|Default Gateway|
|---|---|---|---|---|
|R1|G0/0/0|10.0.0.1|255.255.255.252|N/A|
|R1|G0/0/1|N/A|N/A|N/A|
|R1|G0/0/1.100|192.168.1.1|255.255.255.196|N/A|
|R1|G0/0/1.200|192.168.1.65|255.255.255.224|N/A|
|R1|G0/0/1.1000|N/A|N/A|N/A|
|R2|G0/0/0|10.0.0.2|255.255.255.252|N/A|
|R2|G0/0/1|192.168.1.97|255.255.255.240|N/A|
|S1|VLAN 200|192.168.1.66|255.255.255.224|192.168.1.65|
|S2|VLAN 1|blank|blank|blank|
|PC-A|NIC|DHCP|DHCP|DHCP|
|PC-B|NIC|DHCP|DHCP|DHCP|



<details>
  <summary> Step 1: Establish an addressing scheme</summary>
 Subnet the network 192.168.1.0/24 to meet the following requirements: 

a. One subnet, “Subnet A”, supporting 58 hosts (the client VLAN at R1).
Subnet A: 
```
192.168.1.0/26
```

Record the first IP address in the Addressing Table for R1 G0/0/1.100.

b. One subnet, “Subnet B”, supporting 28 hosts (the management VLAN at R1).
Subnet B: 
```
192.168.1.64/27
```

Record the first IP address in the Addressing Table for R1 G0/0/1.200. Record the second IP address in the Address Table for S1 VLAN 200 and enter the associated default gateway.

c. One subnet, “Subnet C”, supporting 12 hosts (the client network at R2).
Subnet C: 
```
192.168.1.96/28
```

Record the first IP address in the Addressing Table for R2 G0/0/1.
  
</details>


<details>
  <summary> Step 2: Cable the network as shown in the topology.</summary>
  Done.
 </details>
 
<details>
 <summary> Step 3: Configure basic settings for each router.</summary>


```
ena
clock set 14:40:00 20 october 2024
conf t
no ip domain-lookup
banner motd "unauthorized access is prohibited"
line vty 0 4
 login local
 password cisco
 line con 0 
 password cisco
 logging syn
enable secret cisco
service password-encryption
end
wr
```

</details>

<details>
  <summary> Step 4:  Configure Inter-VLAN Routing on R1</summary>
  
a. Activate interface G0/0/1 on the router.

```
Router(config)#int eth 0/0
Router(config-if)#no sh
Router(config-if)#
*Oct 20 16:21:22.079: %LINK-3-UPDOWN: Interface Ethernet0/0, changed state to up
*Oct 20 16:21:23.081: %LINEPROTO-5-UPDOWN: Line protocol on Interface Ethernet0/0, changed state to up
```


b. Configure sub-interfaces for each VLAN as required by the IP addressing table. All sub-interfaces use 802.1Q encapsulation and are assigned the first usable address from the IP address pool you have calculated. Ensure the sub-interface for the native VLAN does not have an IP address assigned. Include a description for each sub-interface.

c. Verify the sub-interfaces are operational.
  </details>