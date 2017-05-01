## <center>mysql进阶</center>

### 修改数据表
- ##### 针对字段的操作
首先我再创建一个数据表
```mysql
CREATE TABLE t1(
    -> username VARCHAR(20) NOT NULL,
    -> age TINYINT,
    -> salary FLOAT(8,2)
    -> );
```

- 添加字段 ALTER TABLE tablename ADD newcol_name col_definition [FIRST|AFTER col_name];
在刚才创建的t1表中新增一个id字段，并且把该字段放到字段首部。  
`ALTER TABLE t1 ADD id SMALLINT UNSIGNED AUTO_INCREMENT PRIMARY KEY FIRST;`  
添加多列，ALTER TABLE table_name ADD (col_name1 col_definition1, col_name2 col_definition2);
- 删除字段 ALTER TABLE tablename DROP col_name;
把刚才创建的id字段再删除。  
`ALTER TABLE t1 DROP id;`  
- 修改列名称 ALTER TABLE tablename CHANGE id uid col_newdefinition [FIRST|AFTER col_name];
把刚才的id列名称和列定义修改  
`ALTER TABLE t4 CHANGE id uid TINYINT UNSIGNED NOT NULL;`  
- 修改列定义 ALTER TABLE tablename MODIFY col_name col_definition [FIRST|AFTER col_name];  
`ALTER TABLE t4 CHANGE uid newid;`
	
- ##### 针对约束的操作
	- 添加默认约束 ALTER TABLE tablename ALTER col_name SET DEFAULT 默认值;
	给t1表的age字段添加默认值
	
	`ALTER TABLE t1 ALTER age SET DEFAULT 20;`
	- 删除默认约束 ALTER TABLE tablename ALTER col_name DROP DEFAULT;
	
	`ALTER TABLE tablename ALTER age DROP DEFAULT;`
	- 添加主键约束 ALTER TABLE tablename ADD [CONSTRAINT [symbol]] PRIMARY KEY [index_type(默认是BTREE，还有HASH索引的)](index_col_name,...);
	
	`ALTER TABLE t1 ADD CONSTRAINT newid PRIMARY KEY (newid);`
	newid之前需不是带主键约束的
	- 添加唯一约束 ALTER TABLE tablename ADD UNIQUE [INDEX|KEY] [index_name] [index_type](index_col_name,...);
	给username添加唯一约束
	
	`ALTER TABLE t1 ADD UNIQUE (username);`
	- 添加外键约束 ALTER TABLE tablename ADD FOREIGN KEY [index_name] (index_col_name,...) reference_definition;
	给t1表的newid字段添加另外一个province表的pid外键约束
	
	`ALTER TABLE t1 ADD FOREIGN KEY (newid) REFERENCES province (pid);`
- ##### 针对数据表的操作
	- 数据表更名(两种方式)

- 记录的操作
注意：下文中记录和数据是同义的
	- 插入(insert)数据(增)
	
	方法一：`INSERT [INTO] tablename[(col_name1,col_name2...)] {VALUES|VALUE}(col_data1...)[,()]`

	insert 插入记录是可以省略列名称，但赋值时要与列数目对应，如果要插入多条记录，括号()之间要加逗号(,)。在插入数据时注意default和NULL的用法，若某个字段为自动编号(自增长)，在插入时，可以用NULL或者default来代替，若某个字段已经设置了default，再插入时可以用default来代替默认值,系统自动填写默认值。列值可以传入数值，表达式或者是函数，如密码加密的函数md5()函数(求字符串的哈希值函数)，例如md5("123")。

	方法二：`INSERT [INTO] tablename SET col_name={col_data|DEFAULT}`

	直接来通过set来设置一条数据的各列的值

	方法三：`INSERT [INTO] tablename[(col_name,...)] SELECT ...`

	这种方法可以将从一张数据表中查询的结果插入到另一张指定的数据表，实现多条数据的插入

	- 更新(update)数据(改)

	单表更新：可以将一条记录(数据)修改(不加条件)，也可以将多条记录(数据)修改(指定条件)

	`UPDATE tablename SET col_name1={col_data1|DEFAULT}[,col_name2={col_data2|DEFAULT}]... [WHERE where_condition]`
	
	- 删除(delete)数据(删)

	单表删除：可以删除一条记录(数据)也可以删除多条(记录)

	`DELETE FROM tablename [WHERE where_condition]`

	注意：删除数据后再插入数据，如果数据表的字段有自增长(AUTO_INCREMENT)的列，则将列从下一个值从删除的数据的值往上加，而不是填补删除的值，如自增长的id列的使用

	- 查询(select)数据(查)

	查询操作是数据库操作的重要操作，查询的方式也比较多，全部查询 `SELECT * FROM tablename` ，查询某些列的数据(选择某些列出现的名称的顺序是会影响结果的顺序的，并且选择列名的别名也会影响结果中的名字) `SELECT col_name1,col_name2 FROM tablename` ， `SELECT tablename.col_name1,tablename.col_name2` (这种方式可用于多表查询时，两张数据表有相同的列名) ，别名查询 `SELECT col_name AS col_newname(别名) FROM tablename` (AS是可以省略的，但建议加上，便于辨认)
	查询表达式：
	```mysql
	SELECT col_name1[,col_name2...]
	[
		FROM tablename
		[WHERE where_condition]
		[GROUP BY {col_name|position(查询列在语句中的位置，不建议使用)} [ASC(升序)|DESC(降序)],...]
		[HAVING where_condition]
		[ORDER BY {col_name|position|exp(表达式)} [ASC|DESC],...]
		[LIMIT {[offset,]row_count|row_count OFFSET offset}]
	]
	```
	例如之前在前面用到的 `SELECT version(); //查看当前mysql的版本` `SELECT now(); //查询当前系统的时间` 后面跟的就是函数表达式  
	WHERE表达式：对记录进行过滤，如果没有指定WHERE子句，则显示所有记录。在WHERE表达式中，可以使用mysql支持的函数或运算符。  
	GROUP BY 对查询的结果进行分组，并且可以指定查询结果的排列顺序  
	HAVING 可以对分组的条件指定
	ORDER BY 对查询的结果进行排序，根据一列或者多列的条件进行排序
	LIMIT 对查询的结果条数进行限制，注意返回的结果是从0开始编号的