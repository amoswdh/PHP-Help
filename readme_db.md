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

- 所以你的构造方法应该如下所示：

``` php
public function __construct(){
// Instantiate new Database object
$this->db = new Database;

// Set handler to overide SESSION
session_set_save_handler(
array($this, "_open"),
array($this, "_close"),
array($this, "_read"),
array($this, "_write"),
array($this, "_destroy"),
array($this, "_gc")
);

// Start the session
session_start();
}
```

## 接下来，我们需要创建处理Session数据的每个方法。这些方法中的每一种都非常简单。

- open

<p>我们需要创建的第一个方法只是检查是否有可供使用的数据库连接。</p>

``` php
/**
* Open
*/
public function _open(){
// If successful
if($this->db){
// Return True
return true;
}
// Return False
return false;
}
```

<p>在这里，我们只是检查是否存在数据库连接。如果有，我们可以返回true，否则我们返回false。</p>

- Close

<p>与Open方法非常相似，Close方法只是检查连接是否已关闭。</p>

``` php
/**
* Close
*/
public function _close(){
// Close the database connection
// If successful
if($this->db->close()){
// Return True
return true;
}
// Return False
return false;
}
```

- Read

<p>Read方法获取会话ID并查询数据库。此方法是我们将数据绑定到查询的第一个示例。通过将id绑定到：id占位符，而不是直接使用该变量，我们使用PDO方法来防止SQL注入。</p>

``` php
 /**
 * Read
 */
 public function _read($id){
 // Set query
 $this->db->query(‘SELECT data FROM sessions WHERE id = :id’);

 // Bind the Id
 $this->db->bind(‘:id’, $id);

 // Attempt execution
 // If successful
 if($this->db->execute()){
 // Save returned row
 $row = $this->db->single();
 // Return the data
 return $row[‘data’];
 }else{
 // Return an empty string
 return ”;
 }
 }
```

- Write

<p>每当Session更新时，都需要Write方法。Write方法从全局会话数组中获取会话ID和会话数据。访问令牌是当前时间戳。</p>

``` php
 /**
 * Write
 */
 public function _write($id, $data){
 // Create time stamp
 $access = time();

 // Set query
 $this->db->query(‘REPLACE INTO sessions VALUES (:id, :access, :data)’);

 // Bind data
 $this->db->bind(‘:id’, $id);
 $this->db->bind(‘:access’, $access);
 $this->db->bind(‘:data’, $data);

 // Attempt Execution
 // If successful
 if($this->db->execute()){
 // Return True
 return true;
 }

 // Return False
 return false;
 }
```

- Destroy

<p>Destroy方法只是根据它的Id删除一个Session。</p>

``` php
 /**
 * Destroy
 */
 public function _destroy($id){
 // Set query
 $this->db->query(‘DELETE FROM sessions WHERE id = :id’);

 // Bind data
 $this->db->bind(‘:id’, $id);

 // Attempt execution
 // If successful
 if($this->db->execute()){
 // Return True
 return true;
 }

 // Return False
 return false;
 }
```

<p>使用会话销毁全局函数时会调用此方法，如下所示：</p>

``` php
// Destroy session
session_destroy();
```

- 垃圾收集

<p>最后，我们需要一个垃圾收集功能。垃圾收集功能将由服务器运行，以清除在数据库中延迟的任何过期的会话。垃圾收集功能的运行取决于您在服务器上的几个设置。</p>

``` php
 /**
 * Garbage Collection
 */
 public function _gc($max){
 // Calculate what is to be deemed old
 $old = time() – $max;

 // Set query
 $this->db->query(‘DELETE * FROM sessions WHERE access < :old’);

 // Bind data
 $this->db->bind(‘:old’, $old);

 // Attempt execution
 if($this->db->execute()){
 // Return True
 return true;
 }

 // Return False
 return false;
 }
```

<p>垃圾收集基于服务器上的session.gc_probability和session.gc_divisor设置运行。例如，概率设置为1000，除数为1.这意味着对于每个页面请求，将有0.01％的机会运行垃圾收集方法。</p>