**Readme**
-
**Nginx + PHP 5.6-FPM + Opcache + Extension**

**PHP PDO Extension** 
-
**Base on :: walofz/php5.6-fpm:lastest**
- mysql
- sqlite
- ldap
- dblib
- freetds

**NGINX Config**
-
- base from [nginx](https://www.digitalocean.com/community/tools/nginx)

**PHP Config**
-
- memory_limit = 512M
- max_execution_time = 600

**PHP-FPM Config**
-
- request_terminate_timeout = 900

**OP Cache**
-

- opcache.enable=1
- opcache.memory_consumption=512
- opcache.interned_strings_buffer=64
- opcache.max_accelerated_files=524521
- opcache.max_wasted_percentage=15
- opcache.save_comments=1
- opcache.mmap_base=0x20000000
