### MySQL 的安全性



#### SQL 查询的安全方案

1. 使用预处理语句防 SQL 注入 (prepare)
2. 写入数据库的数据要进行特殊字符的转义
3. 查询错误信息不要返回给用户，将错误记录到日志



注意：PHP 端尽量使用PDO对数据库进行相关操作，PDO 拥有对预处理语句很好的支持的方法，MySQLi 也有，但是可扩展性不如 PDO，效率略高于 PDO ，MySQL 函数在新版本中已经趋向于淘汰，所以不建议使用，而且他没有很好的致辞预处理的方法。



#### MySQL 的其他安全设置：

1. 定期做数据备份
2. 不给查询用户 `root` 权限，合理分配权限
3. 关闭远程访问数据库权限
4. 修改 `root` 口令，不默认口令，使用比较复杂的口令
5. 删除多余的用户
6. 改变 `root` 用户名称
7. 限制一般用户浏览其他库
8. 限制用户对数据文件的访问权限 