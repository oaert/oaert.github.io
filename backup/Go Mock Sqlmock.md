## Go mock 数据库

#### 写TestMain初始化
```go
func TestMain(m *testing.M) {
	//把匹配器设置成相等匹配器，不设置默认使用正则匹配
	db, mock, err = sqlmock.New(sqlmock.QueryMatcherOption(sqlmock.QueryMatcherEqual))
	if err != nil {
		panic(err)
	}
	tool.DB, err = gorm.Open(mysql.New(mysql.Config{
		Conn:                      db,
		SkipInitializeWithVersion: true,
	}), &gorm.Config{})

	// m.Run 是调用包下面各个Test函数的入口
	os.Exit(m.Run())
}
```
#### 使用期望，查询不用begin cimmit事务，创建更新需要开始事务
```go
//示例有事务

mock.ExpectBegin()
 mock.ExpectExec("INSERT INTO `users` (`username`,`secret`,`created_at`,`updated_at`) VALUES (?,?,?,?)").
  WithArgs(user.UserName, user.Secret, user.CreatedAt, user.UpdatedAt).
  WillReturnResult(sqlmock.NewResult(1, 1))
 mock.ExpectCommit()

//示例无事务

 mock.ExpectQuery("SELECT * FROM `users`  WHERE (username = ? AND secret = ?) "+
  "ORDER BY `users`.`id` ASC LIMIT 1").
  WithArgs(user.UserName, user.Secret).
  WillReturnRows(
   // 这里要跟结果集包含的列匹配，因为查询是 SELECT * 所以表的字段都要列出来
   sqlmock.NewRows([]string{"id", "username", "secret", "created_at", "updated_at"}).
    AddRow(1, user.UserName, user.Secret, user.CreatedAt, user.UpdatedAt))
```

示例教程
[sqlmock](https://cloud.tencent.com/developer/article/2009073)