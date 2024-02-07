- 'sudo adduser ilia': Создаём пользователя пользователя
- 'sudo usermod -aG sudo ilia': Добавляем пользователя в группу sudo
- 'groups newuser': Проверяем, что пользователь добавлен в группу sudo

### Для повышения безопасности рекомендуется отключить возможность прямого входа под пользователем root. В Ubuntu, как и во многих других дистрибутивах Linux, это можно сделать, изменив файл /etc/ssh/sshd_config и установив параметр PermitRootLogin в значение no.

- 'sudo nvim /etc/ssh/sshd_config'
- 'PermitRootLogin no'
- 'sudo service ssh restart'

### Чтобы запретить вход по паролю и разрешить вход только по ключу SSH, выполните следующие шаги:

- 'sudo nano /etc/ssh/sshd_config'
- 'PasswordAuthentication no'
- 'ChallengeResponseAuthentication no'
- 'sudo service ssh restart'

### Для настройки доступа по SSH-ключу, выполните следующие шаги:
- 'ssh-keygen -t rsa -b 2048'
- 'ssh-copy-id username@your_server_ip'

### По умолчанию, сервер автоматически разрешит вход по ключу. Однако, убедитесь, что в файле /etc/ssh/sshd_config опция PubkeyAuthentication установлена в yes. Если это значение уже установлено (что часто бывает по умолчанию), вам не нужно ничего менять.

- 'sudo service ssh restart'

- 'ssh username@your_server_ip'

