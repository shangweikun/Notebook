# Mybatis架构



### 整体架构

MyBatis 的整体架构可以分为三层：



- 接口层：`SqlSession` 是我们平时与 MyBatis 完成交互的核心接口（包括后续整合 SpringFramework 后用到的 `SqlSessionTemplate` ）；
- 核心层：`SqlSession` 执行的方法，底层需要经过配置文件的解析、SQL 解析，以及执行 SQL 时的参数映射、SQL 执行、结果集映射，另外还有穿插其中的扩展插件；
- 支持层：核心层的功能实现，是基于底层的各个模块，共同协调完成的。





