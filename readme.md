<p align="center"><img src="http://php.net/images/logos/php-logo.svg"></p>

## PHP项目环境配置

### Apache (SetEnv APP_ENV testing)

``` shell
<VirtualHost *:80>
    DocumentRoot "D:\web\test"
    ServerName www.test.com
    ServerAlias www.test.com
  <Directory "D:\web\test">
      Options FollowSymLinks ExecCGI
      AllowOverride All
      Order allow,deny
      Allow from all
     Require all granted
  </Directory>
SetEnv APP_ENV testing
</VirtualHost>
```

### Nginx(fastcgi_param  APP_ENV production)

``` shell
location ~ \.php$ {
         add_header     X_server 226;
         fastcgi_pass   php_customerorderpluscom;
         fastcgi_param  APP_ENV production;
         fastcgi_index  $php_index;
         fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
         include        fastcgi_params;
     }

```

### 项目使用示例-不同环境加载各自的DB配置文件

``` shell
if(isset($_SERVER['APP_ENV']) && $_SERVER['APP_ENV'] == 'dev'){
	$config	= require 'db_config_dev.php';
} elseif(isset($_SERVER['APP_ENV']) && $_SERVER['APP_ENV'] == 'testing'){
	$config	= require 'db_config_testing.php';
} elseif(isset($_SERVER['APP_ENV']) && $_SERVER['APP_ENV'] == 'production'){
	$config	= require 'db_config.php';
} else{
	$config	= require 'db_config.php';
}
```