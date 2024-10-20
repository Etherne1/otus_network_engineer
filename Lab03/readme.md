Т.к. нумерация интерфейсоd в образах устройств отличается от реального оборудования, вместо топологии 
![](https://github.com/Etherne1/otus_network_engineer/blob/main/Lab03/Pasted%20image%2020241020151723.png?raw=true)  


Получилась следующая:  
![](https://github.com/Etherne1/otus_network_engineer/blob/main/Lab03/Pasted%20image%2020241020152404.png?raw=true)

Т.е. нумерация интерфейсов на роутерах сменилась с формата Gi0/0/1 на e0/1; нумерация интерфейсов на свитчах с F0/18 на e8/1 и с F0/5 на e5/0 соответственно.
Логически схема не изменилась, но форму топологии поменял, для наглядности.

### Выполнение ДЗ:

#####  Step 1: Establish an addressing scheme



### Предварительная конфигурация устройств


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



```xml
<details> 
  <summary>Q1: What is the best Language in the World? </summary>
   A1: JavaScript 
</details>
```