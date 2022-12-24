# PHP-FPM

## Build
Via GitHub repository
```bash
$ docker build --tag alireaza/php-fpm:$(date -u +%Y%m%d) --tag alireaza/php-fpm:latest https://github.com/alireaza/php-fpm.git
```

## Run
PHP-FPM + OPcache + Composer
```bash
$ docker run \
--interactive \
--tty \
--rm \
--env="TZ=$(cat /etc/timezone)" \
--mount="type=bind,source=$(pwd)/src,target=/var/www/html" \
--mount="type=bind,source=$(pwd)/udocker,target=/home/udocker" \
--publish="9000:9000" \
--name="php-fpm" \
alireaza/php-fpm
```

PHP-FPM + Xdebug + DBGp Proxy + Composer
```bash
$ docker run \
--interactive \
--tty \
--rm \
--env="TZ=$(cat /etc/timezone)" \
--mount="type=bind,source=$(pwd)/src,target=/var/www/html" \
--mount="type=bind,source=$(pwd)/udocker,target=/home/udocker" \
--publish="9000:9000" \
--publish="9001:9001" \
--publish="9003:9003" \
--env="PHP_OPCACHE_JIT=off" \
--env="PHP_XDEBUG_MODE=on" \
--name="php-fpm-xdebug" \
alireaza/php-fpm
```

```bash
$ docker exec --interactive --tty php-fpm-xdebug /usr/bin/dbgpProxy --client 0.0.0.0:9001 --server 0.0.0.0:9003
```
