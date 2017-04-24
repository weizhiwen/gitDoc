## <center>mysql入门</center>

### 1.数据类型
根据项目的实际需要使用合适的数据类型，这也是数据优化的操作

- <strong>整型</strong>

|类型|存储范围|字节数|
|:---:|:---:|:---:|
|TINYINT|有符号值：-128 到 127 (-2<sup>7</sup> 到 2<sup>7</sup>-1)<br>无符号值：0 到 255 (0 到 2<sup>8</sup>-1)|1|
|SMALLINT|有符号值：-32768 到 32767 (-2<sup>15</sup> 到 2<sup>15</sup>-1)<br>无符号值：0 到 65535 (0 到 2<sup>16</sup>-1)|2|
|MEDIUMINT|有符号值：-8388608 到 8388607 (-2<sup>23</sup> 到 2<sup>23</sup>-1)<br>无符号值：0 到 16777215 (0 到 2<sup>24</sup>-1)|3|
|INT|有符号值：-2147483648 到 2147483647 (-2<sup>31</sup> 到 2<sup>31</sup>-1)<br>无符号值：0 到 4294967295 (0 到 2<sup>32</sup>-1)|4|
|BIGINT|有符号值：-9223372036854775808 到9223372036854775807 (-2<sup>63</sup> 到 2<sup>63</sup>-1)<br>无符号值：0 到 18446744073709551615 (0 到 2<sup>64</sup>-1)|8|

- <strong>浮点型</strong>

|类型|存储范围|字节数|
|:---:|:---:|:---:|
|FLOAT[(M,D)]|-3.402823466E+38 到 -1.175494351E-38、 0 和 1.175494351E-38 到 3.402823466E+38。<br>M是数字的总位数，D是小数点后面的位数。如果M和D被省略，则根据硬件允许的限制来保存值。单精度浮点数精确到大约7位小数位。|4|
|DOUBLE[(M,D)]|-1.7976931348623157E+308 到 -2.2250738585072014E-308、 0 和 2.2250738585072014E-308 到 1.7976931348623157E+308|8|

- <strong>字符型</strong>

|类型|存储需求|用途|
|:---:|:---:|:---:|
|CHAR(M)|M个字节，0 <= M <= 255|定义字符串|
|VARCHAR(M)|L+1个字节，其中L+1 <= M 且 0 <= M <= 65535|变长字符串|
|TINYTEXT|L+1个字节，其中L < 2<sup>8</sup>|短文本字符串|
|TEXT|L+2个字节，其中L < 2<sup>16</sup>|长文本数据|
|MEDIUMTEXT|L+3个字节，其中L < 2<sup>24</sup>|二进制形式的中等长度文本数据|
|LONGTEXT|L+4个字节，其中L < 2<sup>32</sup>|极大文本数据|
|ENUM('value1','value2',...)|1或2个字节，取决于枚举值的个数(最多65535个值)|
|SET('value1','value2',...)|1、2、3、4或者8个字节，取决于set成员的数目(最多64个成员)|

- <strong>时间类型</strong>

|类型|存储范围|字节数|格式|用途|
|:---:|:---:|:---:|:---:|:---:|
|DATE|1000-01-01 / 9999-12-31|3|YYYY-MM-DD|完整的日期值|
|TIME|'-838:59:59' / '838:59:59'|3|HH:MM:SS|时间值或持续时间|
|YEAR|1901 / 2155|1|YYYY|年份值|
|DATETIME|1000-01-01 00:00:00 / 9999-12-31 23:59:59|8|YYYY-MM-DD HH:MM:SS|混合日期和时间值|
|TIMESTAMP|1970-01-01 00:00:00 / 2037 年某时|8|YYYY-MM-DD HH:MM:SS|混合日期和时间值，时间戳|

### 2.数据表的操作
关键字最好大写，这样便于阅读。可以用windows的cmd运行工具对数据库操作，前提是mysql的安装目录的子目录bin的路径添加导论系统变量PATH中，`mysql -v`可以查看数据库版本。登录：mysql -u用户名 -p密码，例如 `mysql -uroot -p123456` 。

- 查看所有数据库 `SHOW DATABASES;`
- 使用数据库 `USE 数据库名`
- 查看当前使用的数据库 `SELECT database();`
- 查看当前的用户 `SELECT user();`
- 查看当前的时间 `SELECT now();`
- 查看当前数据库的数据表 `SHOW TABLES;`
- 查看其他数据库的数据表 `SHOW TABLES FROM 数据库名;`
- 创建数据表 
```mysql
CREATE TABLE 数据表名(
字段 类型 设定,
字段 类型 设定
);
```
- 查看数据表的结构 `SHOW COLUMNS FROM 数据表名`
- 查看数据表中的全部数据 	`SELECT * FROM 数据表名`

### 3.数据记录操作
- 创建一个简单的数据表
```mysql
CREATE TABLE t1(
    -> username VARCHAR(20) NOT NULL,
    -> age TINYINT,
    -> salary FLOAT(8,2)
    -> );
```
- 查看刚才常见数据表的结构
`SHOW COLUMNS FROM t1;`

|Field|Type|Null|Key|Default|Extra|
|---|---|---|---|---|---|
|username|varchar(20)|NO| |NULL| |
|age|tinyint(4)|YES| |NULL| |
|salary|float(8,2)|YES| |NULL| |

- 插入一行数据
`INSERT INTO t1 VALUES('Tom',20,56310.54);` 
`INSERT t1(username,salary) VALUES('Jon',75654.35);`

<font color="blue">INTO关键字可以省略，在表名后面不跟字段，默认数据要插入所有字段的数据，否则会报错</font>

- 查看刚才插入数据库的所有数据
`SELECT * FROM t1;`

|username|age|salary|
|---|---:|---|
|Tom|20|56310.54|
|Jon|NULL|75654.35|

- 创建自增长且主键约束和唯一约束的数据表
```mysql
 CREATE TABLE t2(
    -> id SMALLINT AUTO_INCREMENT PRIMARY KEY,
    -> username VARCHAR(20) NOT NULL UNIQUE,
    -> age TINYINT UNSIGNED,
    -> sex ENUM('1','2','3') DEFAULT 3
    -> );
```

<font color="red">AUTO_INCREMENT 自动编号，必须与主键 PRIMARY KEY组合使用，默认情况下，起始值为1，每次的增量为1。但是使用主键时，不一定用到自动编号</font>
- 查看刚才常见数据表的结构
`SELECT * FROM t2;`

|Field|Type|Null|Key|Default|Extra|
|---|---|---|---|---|---|
|id|smallint(6)|NO|PRI|NULL|auto_increment|
|username|varchar(20)|NO|UNI|NULL| |
|age|tinyint(3) unsigned|YES| |NULL| |
|sex|enum('1','2','3')|YES| |3| |

<font color="green">唯一约束 UNIQUE 指的是该字段数据不能有重复，比如上面的数据表中就不能插入两条username都为’tom‘的数据，即使这两个人只是同名</font>
#### 五种约束
- NOT NULL 非空约束
- PRIMARY KEY 主键约束
- UNIQUE KEY 唯一约束
- DEFAULT 默认约束
- NOT NULL 非空约束

### 外键约束
- 1.父表和子表必须使用相同的存储引擎，而且禁止使用临时表。
- 2.数据表的存储引擎只能为InnoDB。
- 3.外键列和参照列必须具有相似的数据类型。其中数字的长度或是有符号位都必须相同；而字符的长度则可以不同。
- 4.外键列和参照列必须创建索引。如果外键列不存在索引的话，MySQL将自动创建索引。

- ##### 创建一个省份表
```mysql
 CREATE TABLE province(
    -> id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    -> pname VARCHAR(20) NOT NULL
    -> );
```

- ##### 查看创建表时的命令
`SHOW CREATE TABLE province\G;`
后面加上\G,可以过滤不必要的信息，方便查看。
```mysql
			Table: province
Create Table: CREATE TABLE `province` (
  `id` smallint(5) unsigned NOT NULL AUTO_INCREMENT,
  `pname` varchar(20) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
```
可以看出符合存储引擎为InnoDB的外键约束要求

- ##### 创建一个子表用户表
```
CREATE TABLE user(
    -> id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    -> username VARCHAR(10) NOT NULL,
    -> pid SMALLINT UNSIGNED,
    -> FOREIGN KEY (pid) REFERENCES province (id)
    -> );
```
pid 作为外键列，province作为父表，其id为参照列，如果pid的数据类型和参照列的数据类型不同或者符号位不同，那么会报出150错误，创建子表失败。

- ##### 查看数据表的索引
`SHOW INDEX FROM province\G;`
```
*************************** 1. row ***************************
        Table: province
   Non_unique: 0
     Key_name: PRIMARY
 Seq_in_index: 1
  Column_name: id
    Collation: A
  Cardinality: 0
     Sub_part: NULL
       Packed: NULL
         Null:
   Index_type: BTREE
      Comment:
Index_comment:
```
`SHOW INDEX FROM user\G;`
```
*************************** 1. row ***************************
        Table: user
   Non_unique: 0
     Key_name: PRIMARY
 Seq_in_index: 1
  Column_name: id
    Collation: A
  Cardinality: 0
     Sub_part: NULL
       Packed: NULL
         Null:
   Index_type: BTREE
      Comment:
Index_comment:
*************************** 2. row ***************************
        Table: user
   Non_unique: 1
     Key_name: pid
 Seq_in_index: 1
  Column_name: pid
    Collation: A
  Cardinality: 0
     Sub_part: NULL
       Packed: NULL
         Null: YES
   Index_type: BTREE
      Comment:
Index_comment:
```
- ##### 查看user的数据表创建命令
`SHOW CREATE TABLE user\G;`
```
*************************** 1. row ***************************
       Table: user
Create Table: CREATE TABLE `user` (
  `id` smallint(5) unsigned NOT NULL AUTO_INCREMENT,
  `username` varchar(10) NOT NULL,
  `pid` smallint(5) unsigned DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `pid` (`pid`),
  CONSTRAINT `user_ibfk_1` FOREIGN KEY (`pid`) REFERENCES `province` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
```
通过查看user表的创建命令，可以发现系统自动为子表的pid创建了索引id，参考(reference)province表的id。