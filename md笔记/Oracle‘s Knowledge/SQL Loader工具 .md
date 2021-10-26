##### 字符255长度报错 - field in data file exceeds maximum length

控制文件CTL中，默认的字段为char型，且长度默认为255，若sqlldr导入的数据中超过了255，无论数据库中字段长度设定多大，依然会报错。因此需要将报错的字段添加长度，即：

**name char(512)**

长度按照实际表字段设定即可。

