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
