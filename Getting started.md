# Настройка окружения
## macOS high sierra
### Установить Homebrew
`/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

### Установить php 5.6
`brew install php@5.6`

`brew install brew-php-switcher`

`brew-php-switcher 5.6`

Проверить версию в терминале `php -v`

### Установить memcached

`brew install libmemcached`

`pecl install memcached-2.2.0`

### Установить composer

`brew install composer`

### Установить зависимости

`cd /path/to/gambit`

`composer install`

`npm i`

`grunt`

### Запустить сервер php в фоне

`sudo vim /Library/LaunchDaemons/homebrew.mxcl.php56.plist`

Добавить и сохранить:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
    <dict>
        <key>KeepAlive</key>
        <true/>
        <key>Label</key>
        <string>homebrew.mxcl.php56</string>
        <key>ProgramArguments</key>
        <array>
            <string>/usr/local/bin/php</string>
            <string>-S</string>
            <string>localhost:8000</string>
            <string>-t</string>
            <string>/path/to/gambit/web</string>
        </array>
        <key>RunAtLoad</key>
        <true/>
        <key>LaunchOnlyOnce</key>
        <true/>
    </dict>
</plist>
```

Заменить путь `/path/to/gambit/web` на актуальный и сохранить `:wq`

Выполнить
`sudo launchctl load /Library/LaunchDaemons/homebrew.mxcl.php56.plist`

### Установить nginx

`brew install nginx`

`brew services list`

`brew services start nginx`

`sudo apachectl stop`

`sudo cp -v /usr/local/opt/nginx/*.plist /Library/LaunchDaemons/`

`sudo chown root:wheel /Library/LaunchDaemons/homebrew.mxcl.nginx.plist`

`sudo launchctl load /Library/LaunchDaemons/homebrew.mxcl.nginx.plist`

`sudo nginx`
Проверить по адресу http://localhost/

### Отредактировать конфиг `/usr/local/etc/nginx/nginx.conf`

```
worker_processes 1;

events {
    worker_connections 1024;
}

http {
    include mime.types;
    default_type application/octet-stream;
    sendfile on;
    keepalive_timeout 65;

    server {
        listen 80;
        root /path/to/gambit/web;
        server_name localhost gam.bit;

        location / {
            index index.php index.html index.htm;
        }

        location ~ \.php$ {
            proxy_pass http://localhost:8000;
        }
    }

    include servers/*;
}
```

Заменить путь `/path/to/gambit/web` на актуальный

### Добавить симлинк в hosts
`sudo echo "127.0.0.1 gam.bit" >> /private/etc/hosts`

### Перезапустить сервер
`brew services restart nginx`
