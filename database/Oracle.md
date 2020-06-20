win10安装oracle11g提示环境不足，去介质包内修改配置文件database\stage\cvu_prereq.xml添加：
```xml
<OPERATING_SYSTEM RELEASE="6.2">
        <VERSION VALUE="3"/>
        <ARCHITECTURE VALUE="32-bit"/>
        <NAME VALUE="Windows 10"/>
        <ENV_VAR_LIST>
            <ENV_VAR NAME="PATH" MAX_LENGTH="1023" />
        </ENV_VAR_LIST>
</OPERATING_SYSTEM>
```

linux数据库启动关闭
```shell
关闭：
切换到oracle用户
su - oracle
进入数据库：
sqlplus / as sysdba
关闭数据库：
shutdown immediate

开启：
切换到oracle用户：
su - oracle
进入数据库：
sqlplus / as sysdba
启动数据库：
startup
退出数据库：
exit
启动监听：
lsnrctl start
查看监听状态：
lsnrctl status
如果是没有listener什么的
sqlplus / as sysdba
alter system register
```

创建表空间
```sql
create tablespace DATASPACE
logging  
datafile 'X:\XXXXX\XXXXX\DATASPACE.dbf' 
size 50m  
autoextend on  
next 50m maxsize 2048m  
extent management local; 
```

创建用户授予表空间
```sql
create user USERNAME identified by USERPASSWORD
default tablespace DATASPACE
```

授予新用户DBA权限
```sql
grant dba to dw_hos  #授与DBA权限
```

取消用户过期时间
```sql
alter profile default limit PASSWORD_LIFE_TIME unlimited;
```

增
```sql
INSERT INTO Persons (LastName, Address) VALUES ('Wilson', 'Champs-Elysees')
INSERT  into MC_USER_RESOURCE(USER_ID,RESOURCE_ID,OPERATE_TYPE)SELECT  a.ID,b.ID,'any'from MC_USER a ,MC_RESOURCE b

```

删
```sql
DELETE FROM Person WHERE LastName = 'Wilson'  删除语句的标准格式 
```

改
```sql
UPDATE Person SET FirstName = 'Fred' WHERE LastName = 'Wilson' 
UPDATE t_ys_dm SET （ysdm,ysmc） = (SELECT ysdm，ysmc FROM gy_ysdm)
```

年龄段分组
```sql
select floor(months_between(sysdate,csny)/12) from t_ms_brda_dm1 算年龄

update t_ms_brda_dm1 a
set nld = (
case 
when a.nl between 0 and 9 then '0~10'
when a.nl between 10 and 19 then '10~20'
when a.nl between 20 and 29 then '20~30'
when a.nl between 30 and 39 then '30~40'
when a.nl between 40 and 49 then '40~50'
when a.nl between 50 and 59 then '50~60'
when a.nl between 60 and 69 then '60~70'
when a.nl between 70 and 79 then '70~80'
when a.nl between 80 and 89 then '80~90'
when a.nl >= 90 then '90~++'
end
)
```

TOP语法
```sql
SELECT TOP number|percent column_name(s) FROM table_name

oracle中的TOP使用方式是： 
select  * form t_date_dm where  rownum < n ;  返回N行以前的数据
TOP PERCENT : SELECT TOP 50 PERCENT colunm_name  from table_name ; 返回前50%的数据
```

备份一张表
```sql
create table dept2 as select * from dept;
```

转换时间为数字
```sql
select sysdate from daul; 查询系统时间
select to_number(to_char(sysdate,'yyyymmdd'))  from daul;将系统时间的年月日转换为数字
```

cmd中导入DMP
```sql
imp dw_hos/dw_hos@连接串 file=D\jb.dmp fromuser=dw_hos touser=dw_hos 
```

修改表格
```sql
NAME VARCHAR2(20))ALTER TABLE SCOTT.TEST RENAME TO TEST1--修改表名
ALTER TABLE SCOTT.TEST RENAME COLUMN NAME TO NAME1 --修改表列名
ALTER TABLE SCOTT.TEST MODIFY NAME1 NUMBER(20) --修改字段类型
ALTER TABLE SCOTT.TEST ADD RESS VARCHAR2(40) --添加表列
ALTER TABLE SCOTT.TEST DROP COLUMN RESS--删除表列，
ALTER TABLE gtsysusr.SCHEDULE_CONTENTS MODIFY CONTENTS_ID NVARCHAR2(64)
另建一个表，把varchar改成date，然后用SQL转一下插入，然后删除原表，然后改目标表名字。应该可以了
```

插入一个sys_guid
```sql
insert into table (id) values (sys_guid())
```

PLSQL注册码
```
Product Code(产品编号)：jtrexa75fat2mgcetfhx767laqrbtssqrk
serial Number(序列号)：335566
password(口令)：xs374ca
```