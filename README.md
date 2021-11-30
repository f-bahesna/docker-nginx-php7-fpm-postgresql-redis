# This is my documentation for setup docker

### docker-nginx-php7/php8-fpm-postgresql-redis
PHP-7.4 : Laravel v.6.18.3

PHP-8.0-alpine : Laravel v.8.7.3

I wish I could help for the configuration. cheers!




### **Errors:** if you had a problem in postgresql [ could not connect to server ] or redis [ redis connection refused ]
##### For mac : change from "127.0.0.1 / localhost" to "docker.for.mac.localhost"

##### Example:
###### DB_CONNECTION=pgsql
###### DB_HOST=docker.for.mac.localhost
###### DB_PORT=5432
  
###### REDIS_HOST=docker.for.mac.localhost
###### REDIS_PASSWORD=null
###### REDIS_PORT=6379
