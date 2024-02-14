## Первоначальная настройка сервера

Создаём пользователя и добавляем его в группу sudo
```shell
sudo adduser user_name
```
```shell
sudo usermod -aG sudo user_name
```
```shell
groups user_name
```

задаем пароль
passwd user_name

Для повышения безопасности рекомендуется отключить возможность прямого входа под пользователе root. 
В Ubuntu, как и во многих других дистрибутивах Linux, это можно сделать, изменив файл /etc/ssh/sshd_config и установив параметр PermitRootLogin в значение no
```shell
sudo nvim /etc/ssh/sshd_config
```
```shell
PermitRootLogin no
```
```shell
sudo service ssh restart
```


Создаём SSH-ключ (если не созздан ранее) и отправляем его на сервер
```shell
ssh-keygen -t rsa -b 2048
```
```shell
ssh-copy-id username@your_server_ip
```

Запрещаем вход по паролю и задаем только по SSH-ключу
```shell
sudo nano /etc/ssh/sshd_config
```
```shell
PasswordAuthentication no
```
```shell
ChallengeResponseAuthentication no
```
```shell
sudo service ssh restart
```

По умолчанию, сервер автоматически разрешит вход по ключу. Однако, убедитесь, что в файле /etc/ssh/sshd_config опция PubkeyAuthentication установлена в yes. Если это значение уже установлено (что часто бывает по умолчанию), вам не нужно ничего менять
```shell
sudo service ssh restart
```
```shell
ssh username@your_server_ip
```

