# Docker Laradock PHPStorm XDebug bundle setup

## Docker setting

### terminal
Check firewall status
```bash
sudo ufw status
sudo ufw verbose
sudo ufw enable
```
> Need to check current IP address. For example: `ifconfig`

Allow for IP: `172.16.0.0/12` as example
```bash
sudo ufw allow in from 172.16.0.0/12 to any port 9001 comment xDebug9001
sudo ufw allow in from 172.16.0.0/12 to any port 9003 comment xDebug9003
sudo ufw allow in from 172.16.0.0/12 to any port 9000 comment xDebug9000
```
Allow for IP: `192.168.0.0/12` as example
```bash
sudo ufw allow in from 192.168.0.0/12 to any port 9001 comment xDebug9001
sudo ufw allow in from 192.168.0.0/12 to any port 9002 comment xDebug9002
sudo ufw allow in from 192.168.0.0/12 to any port 9003 comment xDebug9003
```
***

### .env  
```dotenv
WORKSPACE_INSTALL_XDEBUG=true
PHP_FPM_INSTALL_XDEBUG=true

PHP_FPM_XDEBUG_PORT=9003
PHP_IDE_CONFIG=serverName=laradock
```
***

### Go to php-fpm console  
```bash
docker exec -it laradock_php-fpm_1 bash
```

Install Vim and open xdebug.ini
```bash
apt-get update && apt-get install vim -y
vim /usr/local/etc/php/conf.d/xdebug.ini
```

Or install Nano and open xdebug.ini
```bash
apt-get update && apt-get install nano -y
nano /usr/local/etc/php/conf.d/xdebug.ini
```

|~~does not work~~|
```bash
#vim /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
```
***

#### ~~Go to php-fpm consol (alternative. does not work!)~~
```bash
#docker exec -it laradock_workspace_1 bash
#nano /etc/php/7.4/cli/conf.d/20-xdebug.ini
```
***

### xdebug.ini  

```ini
xdebug.client_host="host.docker.internal"
; xdebug.discover_client_host=false
; xdebug.client_port=9003
xdebug.client_port=9000
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
***

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
***

### php-fpm need to restart
```bash
service php-fpm restart
```
> Or you can do it manually in PHPStorm -> Services -> Docker -> php-fpm -> container -> right_mouse_button -> restart
***

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
***

## PhpStorm setting

### Server configuration
> Create in root `.run/Laradock.run.xml`
```xml
<component name="ProjectRunConfigurationManager">
  <configuration default="false" name="Laradock" type="PhpRemoteDebugRunConfigurationType" factoryName="PHP Remote Debug" filter_connections="FILTER" server_name="laradock" session_id="PHPSTORM">
    <method v="2" />
  </configuration>
</component>
```
***

### Settings -> PHP -> Debug
Debug port: `9000,9003`  

***

### Settings -> PHP -> Debug -> Validate
Path: `_path_to_site_dir_`  
URL: `http://127.0.0.1`  
***

### Settings -> PHP -> Servers
Name: `_name_server_`  
Host: `localhost`  
Port: `80`  
Debuger: `Xdebug`  
File Directory: `_local_path_to_site_`  
Absolute path: `var/www/_project_dir_`  
***

### Run/Debug Configuration
Name: `_name_server_`  
Server: `_name_server_`  
IDE key: `PHPSTORM`  
***