<p align="center"><img width="100" src="http://php.net/images/logos/php-logo.svg"></p>

## 设置在Redis中存储会话

### 最简单设置

``` php
ini_set("session.save_handler","redis");
ini_set("session.save_path","tcp://127.0.0.1:6379");
```

### 会话管理操作

<p>由于我们将管理自己的会话数据而不是让PHP为我们处理它，因此我们编写的任何代码都需要解决六个会话管理任务中的每一个。</p>

- open - 打开用于保存会话信息的存储机制的资源。
- close - 关闭存储资源并启动任何其他必要的清理活动。
- read - 从存储机制中检索以前保存的会话数据。
- write - 存储应用程序需要记住的新会话数据。
- destroy - 重置会话并丢弃其中的任何信息。
- gc - 在存储机制变得陈旧且不再需要之后从存储机制中清除数据。

### Code

``` php
<?php
class RedisSessionHandler implements SessionHandlerInterface
{
    public $ttl = 1800; // 30 minutes default
    protected $db;
    protected $prefix;

    public function __construct(PredisClient $db, $prefix = 'PHPSESSID:') {
        $this->db = $db;
        $this->prefix = $prefix;
    }

    public function open($savePath, $sessionName) {
        // No action necessary because connection is injected
        // in constructor and arguments are not applicable.
    }

    public function close() {
        $this->db = null;
        unset($this->db);
    }

    public function read($id) {
        $id = $this->prefix . $id;
        $sessData = $this->db->get($id);
        $this->db->expire($id, $this->ttl);
        return $sessData;
    }

    public function write($id, $data) {
        $id = $this->prefix . $id;
        $this->db->set($id, $data);
        $this->db->expire($id, $this->ttl);
    }

    public function destroy($id) {
        $this->db->del($this->prefix . $id);
    }

    public function gc($maxLifetime) {
        // no action necessary because using EXPIRE
    }
}
```

``` php
<?php
$db = new PredisClient();
$sessHandler = new RedisSessionHandler($db);
session_set_save_handler($sessHandler);
session_start();

```