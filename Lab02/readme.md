
# Lab02

## Топология:
![](https://github.com/Etherne1/otus_network_engineer/blob/main/Lab01/Pasted%20image%2020241010212925.png?raw=true)

### Цели:
Часть 1. Создание сети и настройка основных параметров устройства
Часть 2. Выбор корневого моста
Часть 3. Наблюдение за процессом выбора протоколом STP порта, исходя из стоимости портов
Часть 4. Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов

### Решение.

##### Предварительная настройка:
```
conf t
no ip domain-lookup
banner motd "unauthorized access is prohibited"
line vty 0 4
 login local
 password cisco
 line con 0 
 password cisco
 logging syn
service password-encryption
```

Далее меняем кост на S1
```
S1(config)# **interface f0/2**

S1(config-if)# **spanning-tree cost 18**
```
Затев возвращаем обратно:
```
S1(config)# **interface f0/2**

S1(config-if)# no spanning-tree cost 18
```

У меня данная команда в packet tracer никакого эффекта не дала, поэтому вручную выставил значение на дефолт. Но из конфига строку вручную убрал, чтобы при проверке не отображалось ошибкой.

```
S1(config)# **interface f0/2**

S1(config-if)# spanning-tree cost 19
```

[Конфиг по частям 1-2.](https://github.com/Etherne1/otus_network_engineer/tree/main/Lab02/config%20before%20part%203)

[Конфиг после выполнения части 3.](https://github.com/Etherne1/otus_network_engineer/tree/main/Lab02/config%20after%20part%203)

[Файл лабораторной работы, с заполненными ответами.](https://github.com/Etherne1/otus_network_engineer/blob/main/Lab02/3.1.2.12_Lab___Building_a_Switched_Network_with_Redundant_Links.docx)
