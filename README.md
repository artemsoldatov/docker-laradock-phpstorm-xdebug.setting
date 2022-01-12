# Docker Laradock PHPStorm XDebug bundle setup

### terminal  
```bash
sudo ufw status
sudo ufw verbose
sudo ufw enable
sudo ufw allow in from 172.16.0.0/12 to any port 9001 comment xDebug9001
sudo ufw allow in from 172.16.0.0/12 to any port 9003 comment xDebug9003
sudo ufw allow in from 172.16.0.0/12 to any port 9000 comment xDebug9000
```
---

### .env  
```dotenv
WORKSPACE_INSTALL_XDEBUG=true
PHP_FPM_INSTALL_XDEBUG=true

PHP_FPM_XDEBUG_PORT=9003
PHP_IDE_CONFIG=serverName=laradock
```
---

### Go to php-fpm console  
```bash
docker exec -it truelearn_php-fpm_1 bash

apt-get update && apt-get install vim -y

vim /usr/local/etc/php/conf.d/xdebug.ini
```
|~~does not work~~|
```bash
#vim /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
```
---

#### ~~Go to php-fpm consol (alternative. does not work!)~~
```bash
#docker exec -it truelearn_workspace_1 bash
#nano /etc/php/7.4/cli/conf.d/20-xdebug.ini
```
---

### xdebug.ini  

```ini
xdebug.client_host="host.docker.internal"
; xdebug.discover_client_host=false
xdebug.client_port=9001
xdebug.idekey=PHPSTORM

xdebug.start_with_request=yes
xdebug.mode=debug
xdebug.cli_color=1
; xdebug.profiler_enable=0
xdebug.output_dir="~/xdebug/phpstorm/tmp/profiling"

xdebug.remote_handler=dbgp
; xdebug.remote_mode=req

xdebug.var_display_max_children=-1
xdebug.var_display_max_data=-1
xdebug.var_display_max_depth=-1
```
---

#### ~~xdebug.ini (bad alternative)~~
```ini
zend_extension=xdebug 

 
xdebug.client_host=host.docker.internal
xdebug.client_port=9000
xdebug.mode=debug 
#xdebug.mode=develop 
xdebug.idekey=PHPSTORM 
xdebug.start_with_request=yes 
xdebug.log=/tmp/xdebug.log
```
---

### php-fpm need to restart
```bash
service php-fpm restart
```
> Or you can do it manually in PHPStorm -> Services -> Docker -> php-fpm -> container -> right_mouse_button -> restart
---

### docker-compose.yml  
```yaml
services:

### Workspace Utilities ##################################
workspace:
extra_hosts:
- "host.docker.internal:host-gateway"

### PHP-FPM ##############################################
php-fpm:
extra_hosts:
- "host.docker.internal:host-gateway"
```
---

