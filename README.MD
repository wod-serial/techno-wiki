![img.png](img.png)


# Technocraty database

Это репозиторий для информации и настроек поднятия техновики.

В текущий момент она поднята в виде трёх docker контейнеров, документация по ним и docker compose лежит [тут](https://github.com/jehy/php-fpm-mediawiki).


### Перенос

Для того, чтобы перенести вики на другой сервер или для локальной отладки, нужно:

1. Остановить контейнеры (для этого есть красивая веб морда, спросите Джехи)
2. Перенести `/var/lib/docker/volumes/*` на другой сервер в такую же директорию
3. Развернуть на этом сервере `docker-compose.yml`, указав там два необходимых секрета (пароли к СУБД)
4. Поправить домен в `LocalSettings.php` на новый, если нужно
5. Выкинуть наружу порт от nginx, который слушает только `127.0.0.1:6602` - например, через другой nginx и proxy_pass
6. Всё, вы великолепны!

### Запуск служебных скриптов

```bash
# export sql backup
docker exec mysql /usr/bin/mysqldump -u tc --password=ПАРОЛЬ tc > backup.sql

# import SQL backup
cat backup.sql | docker exec -i mysql mysql -utc -pПАРОЛЬ tc

# rebuild images
docker exec -i php php /var/www/public_html/maintenance/rebuildImages.php

# enter container shell
docker exec -it php sh

#run update script
cd /var/www/public_html/maintenance/
php ./update.php

# reinstall composer deps which are broken in gzip
cd /var/www/public_html/
rm ./vendor/* -rf
composer install
```

### Тома этой конфигурации (volumes)
* `home` - файлики самой вики
* `images` - отдельно картинки
* `mysql` - данные СУБД
* `nginx-config` - настройки nginx
* `php-config` - настройки php-fpm

### Настройка почты

Почта перенастроена через SMTP по [этой инструкции](https://www.mediawiki.org/wiki/Manual:$wgSMTP). Можно использовать любой подходящий сервис, выдающий SMTP креды.

### AcessControl

Используется чуток поправленный стандартный [AccessControl](https://www.mediawiki.org/wiki/Extension:AccessControl) из [этого репозитория](https://github.com/wod-serial/AccessControl).

Теги доступа поправлены в дампе при помощи [этого скрипта](https://github.com/wod-serial/access-fixer).
