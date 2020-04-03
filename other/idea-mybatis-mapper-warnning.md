###  Idea在mybatis的mapper的XML文件提示两个警告

Mybatis的Mapper.xml映射文件在IDEA中出现警告。

1. SQL dialect is not configured. 
2. No data sources are configured to run this SQL and provide advanced code assistance. Disable this inspection via problem menu (Alt+Enter). 



解决办法

1. 在IDEA中File - Setting - Editor - Inspections - SQL - SQl dialect detection 勾选取消。

2. 在IDEA中配置一个Mysql数据源。

   如果正常需要在IDEA中使用数据源就按照正常配置。

   如果不需要在IDEA中使用只需要直接点击新增后按照默认点击apply即可。

