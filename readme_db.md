<p align="center"><img width="100" src="http://php.net/images/logos/php-logo.svg"></p>

## 设置在数据库中存储会话

### 为了将Session数据存储在数据库中，我们需要一个表

``` php
CREATE TABLE IF NOT EXISTS sessions (
id varchar(32) NOT NULL,
access int(10) unsigned DEFAULT NULL,
data text,
PRIMARY KEY (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

### 会话类

- 创建Class

``` php
class Session {

}
```

- 数据库对象声明一个属性

``` php
/**
* Session
*/
class Session {

/**
* Db Object
*/
private $db;

}
```

- 实例化数据库类的副本并将其存储在db属性中

``` php
public function __construct(){
// Instantiate new Database object
$this->db = new Database;
}
```

- 覆盖Session处理程序来告诉PHP我们想要使用我们自己的方法来处理Sessions

``` php
// Set handler to overide SESSION
session_set_save_handler(
array($this, "_open"),
array($this, "_close"),
array($this, "_read"),
array($this, "_write"),
array($this, "_destroy"),
array($this, "_gc")
);
```

- 覆盖Session处理程序来告诉PHP我们想要使用我们自己的方法来处理Sessions

``` php
// Set handler to overide SESSION
session_set_save_handler(
array($this, "_open"),
array($this, "_close"),
array($this, "_read"),
array($this, "_write"),
array($this, "_destroy"),
array($this, "_gc")
);
```

- 启动Session

``` php
// Start the session
session_start();
```