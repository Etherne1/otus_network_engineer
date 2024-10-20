 
 ### Уточнение по топологии
Нумерация интерфейсов в используемых образах отличается от реального оборудования, что создает изрядные проблемы при настройке.  Пришлось менять нумерацию интерфейсов и вместо топологии:
![](https://github.com/Etherne1/otus_network_engineer/blob/main/Lab03/Pasted%20image%2020241020151723.png?raw=true)  

Получилась следующая:  
![](https://github.com/Etherne1/otus_network_engineer/blob/main/Lab03/Pasted%20image%2020241020152404.png?raw=true)

Т.е. нумерация интерфейсов на роутерах сменилась с формата Gi0/0/1 на e0/1; нумерация интерфейсов на свитчах с F0/18 на e8/1 и с F0/5 на e5/0 соответственно.
Логически схема не изменилась, но форму топологии поменял, для наглядности.
В связи со сменой формата, пришлось менять ее и в таблицах(а так же в заданиях):

###  Addressing Table
|Device|Interface|IP Address|Subnet Mask|Default Gateway|
|---|---|---|---|---|
|R1|et0/0|10.0.0.1|255.255.255.252|N/A|
|R1|et0/1|N/A|N/A|N/A|
|R1|et0/1.100|192.168.1.1|255.255.255.196|N/A|
|R1|et0/1.200|192.168.1.65|255.255.255.224|N/A|
|R1|et0/1.1000|N/A|N/A|N/A|
|R2|et0/0|10.0.0.2|255.255.255.252|N/A|
|R2|et0/1|192.168.1.97|255.255.255.240|N/A|
|S1|VLAN 200|192.168.1.66|255.255.255.224|192.168.1.65|
|S2|VLAN 1|blank|blank|blank|
|PC-A|NIC|DHCP|DHCP|DHCP|
|PC-B|NIC|DHCP|DHCP|DHCP|

### VLAN Table
|VLAN|Name|Interface Assigned|
|---|---|---|
|1|N/A|S2: e8/1|
|100|Clients|S1: e6/0|
|200|Management|S1: VLAN 200|
|999|Parking_Lot|S1: all others|
|1000|Native|N/A|

<details>
  <summary> Step 1: Establish an addressing scheme</summary>
 Subnet the network 192.168.1.0/24 to meet the following requirements: 

a. One subnet, “Subnet A”, supporting 58 hosts (the client VLAN at R1).
Subnet A: 
```
192.168.1.0/26
```

Record the first IP address in the Addressing Table for R1 et0/1.100.

b. One subnet, “Subnet B”, supporting 28 hosts (the management VLAN at R1).
Subnet B: 
```
192.168.1.64/27
```

Record the first IP address in the Addressing Table for R1 et0/1.200. Record the second IP address in the Address Table for S1 VLAN 200 and enter the associated default gateway.

c. One subnet, “Subnet C”, supporting 12 hosts (the client network at R2).
Subnet C: 
```
192.168.1.96/28
```

Record the first IP address in the Addressing Table for R2 et0/1.
  
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
  <summary> Step 4:  Configure Inter-VLAN Routing on R1:</summary>
  
a. Activate interface et0/1 on the router.

```
Router(config)#int eth 0/1
Router(config-if)#no sh
Router(config-if)#
*Oct 20 16:21:22.079: %LINK-3-UPDOWN: Interface Ethernet0/1, changed state to up
*Oct 20 16:21:23.081: %LINEPROTO-5-UPDOWN: Line protocol on Interface Ethernet0/1, changed state to up
```


b. Configure sub-interfaces for each VLAN as required by the IP addressing table. All sub-interfaces use 802.1Q encapsulation and are assigned the first usable address from the IP address pool you have calculated. Ensure the sub-interface for the native VLAN does not have an IP address assigned. Include a description for each sub-interface.
```
int et0/1.100
enc dot 100
desc Clients
ip add 192.168.1.1 255.255.255.196
int et0/1.200
enc dot 200
desc Management
ip add 192.168.1.65 255.255.255.224
int et0/1.1000    
enc dot 1000
desc Native
```
c. Verify the sub-interfaces are operational.

  

```
Router#sh int desc | i 0/1
Et0/1                 up           up
Et0/1.100             up           up       Clients
Et0/1.200             up           up       Management
Et0/1.1000            up           up       Native
```
  </details>

<details>
  <summary> Step 5:  Configure et0/1 on R2, then et0/0 and static routing for both routers:</summary>


a. Configure et0/1 on R2 with the first IP address of Subnet C you calculated earlier.
```
R2(config)#int et0/1
R2(config-if)#ip add 192.168.1.97
R2(config-if)#ip add 192.168.1.97 255.255.255.240
R2(config-if)#no sh
*Oct 20 17:15:55.678: %LINK-3-UPDOWN: Interface Ethernet0/1, changed state to up
*Oct 20 17:15:56.679: %LINEPROTO-5-UPDOWN: Line protocol on Interface Ethernet0/1, changed state to up
```


b. Configure interface et0/0 for each router based on the IP Addressing table above.

```
R1(config)#int et0/0
R1(config-if)#ip add 10.0.0.1 255.255.255.252
R1(config-if)#no sh
*Oct 20 17:19:33.770: %LINK-3-UPDOWN: Interface Ethernet0/0, changed state to up
*Oct 20 17:19:34.770: %LINEPROTO-5-UPDOWN: Line protocol on Interface Ethernet0/0, changed state to up
```

```
R2(config)#int et0/0
R2(config-if)#ip add 10.0.0.2 255.255.255.252
R2(config-if)#no sh
*Oct 20 17:23:03.484: %LINK-3-UPDOWN: Interface Ethernet0/0, changed state to up
*Oct 20 17:23:04.488: %LINEPROTO-5-UPDOWN: Line protocol on Interface Ethernet0/0, changed state to up
```

c. Configure a default route on each router pointed to the IP address of et0/0 on the other router.

```
R1(config)#ip route 0.0.0.0 0.0.0.0 10.0.0.2
```

```
R2(config)#ip route 0.0.0.0 0.0.0.0 10.0.0.1
```

d. Verify static routing is working by pinging R2’s et0/1 address from R1.

```
R1#ping 10.0.0.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.0.0.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
```

e.  Save the running configuration to the startup configuration file.

```
R1#wr
Building configuration...
[OK]
```
```
R2#wr
Building configuration...
[OK]
```
  </details>



<details>
  <summary>  Step 6: Configure basic settings for each switch.</summary>

a.      Assign a device name to the switch.

b.      Disable DNS lookup to prevent the router from attempting to translate incorrectly entered commands as though they were host names.

c.      Assign **class** as the privileged EXEC encrypted password.

d.      Assign **cisco** as the console password and enable login.

e.      Assign **cisco** as the VTY password and enable login.

f.       Encrypt the plaintext passwords.

g.      Create a banner that warns anyone accessing the device that unauthorized access is prohibited.

h.      Save the running configuration to the startup configuration file.

i.        Set the clock on the switch to today’s time and date.

**Note**: Use the question mark (**?**) to help with the correct sequence of parameters needed to execute this command.

j.        Copy the running configuration to the startup configuration.


### Все это сделано в рамках п.3, т.к. кроме hostname, первоначальную конфигурацию вводил через MultiExec.


  </details>