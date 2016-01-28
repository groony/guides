# Создание sftp пользователя с ограничением конкретной папкой

```
sudo adduser test
```

```
sudo chown root /home/test
```

```
sudo nano /etc/ssh/sshd_config
```

```
Match Group test
    ChrootDirectory %h
    X11Forwarding no
    AllowTcpForwarding no
    ForceCommand internal-sftp
    PasswordAuthentication yes
```

```
sudo service ssh restart
```

Если нужно сделать доступ на папку которая лежит в проекте:

* перемещаем папку в домашнюю директорию юзера
* и с того места делаем линк на перемещенную папку


Проверяем:

```
sftp test@host
```

DONE!
