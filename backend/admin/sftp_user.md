# Создание sftp пользователя с ограничением конкретной папкой

```
sudo adduser {{restricted_user}}
```

```
sudo chown root /home/{{restricted_user}}
```

```
sudo nano /etc/ssh/sshd_config
```

Добавьте следующие строки:

```
Match Group {{restricted_user}}
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

```
sudo mv {{folder}} /home/{{restricted_user}}/
```

* и с того места делаем линк на перемещенную папку

```
sudo ln -s {{path_to_folder}} {{link_name}}
```


Проверяем:

```
sftp {{restricted_user}}@host
```

DONE!
