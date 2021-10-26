dockerHub拉去镜像

```shell
docker pull mysql
```



docker 启动镜像

```shell
docker run -itd --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql
```



docker 进入镜像

```shell
docker exec -it mysql bash
```



mysql 登入

```shell
mysql -u root -p
```



MySQL 修改密码

```mysql
ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';
```



MySQL 添加用户

```mysql
CREATE USER 'mybatis'@'localhost' IDENTIFIED WITH mysql_native_password BY '159951';
```



MySQL 修改用户登入方式

```mysql
RENAME USER 'schwarzeni'@'localhost' TO 'schwarzeni'@'%';
```



MySQL 授予用户权限

```mysql
GRANT ALL PRIVILEGES ON *.* TO 'mybatis'@'%';
```

