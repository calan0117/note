[qlik help v3.2](https://help.qlik.com/zh-CN/sense/3.2/Content/Home.htm)

## 部署和配置

### 安装Qlik  
1.点击Custom install,选择安装为center，指定好安装目录  
2.为Qlik设置一个密码。例如Admin123  
3.勾选enter user information for starting the services。输入用户名为pcname\username。例：DESKTOP-P91L30F\bi,输入该ROOT用户密码。下一步到安装结束。  
4.输入License

### 用户分配  
1.进入pcname/qmc。例如：https://DESKTOP-P91L30F/qmc  
2.在MANGE RESOURCES中选择License and tokens,右侧选择Users access allocations  
3.下方选择 +Allocate，选择要启用的用户。确定后在框体内见到Status为Allocated即可。
 
### 局域网访问配置
1.进入pcname/qmc。例如：https://DESKTOP-P91L30F/qmc  
2.在CONFIGURE SYSYTEM中选择virtual proxies  
3.选中Central Proxy(Default)，点击下面的Edit.  
4.选中右侧的Advanced,在下面的 Host white list中添加IP地  址。在配置好机器地址后，即可从局域网访问。
 
### 文件路径:
sense存放js，css等文件的 D:\workapps\Qlik\Sense\Client  安装目录下的Client的文件 

## 平台迁移 

### 插件迁移  
1.做好的插件打包成.zip格式，进入QMC页面  
2.在MANGE RESOURCES中选择Extensions,点击下方的+import
 如果在线修改的插件内容的话在：  
插件目录：   C:\ProgramData\Qlik\Sense\Repository\Extensions  
打包插件后再去新的导入
 
### Mashup迁移
1.插件目录：C:\ProgramData\Qlik\Sense\Repository\Extensions也存放着Mashup文件，可以拷贝出来做备份。
2.新平台建好页面后将各个Mashup内容复制出来，放入。
3.修改每个Mashup.js的文件中对应的APPID，来关联。
 
### APP迁移  
1.进入QMC，从原平台导出APP  
2.进去QMC ,从新的平台导入  
3.APP的加载脚本在换了数据库的饿时候要更新数据连接。
 


## 模块功能：
### 数据权限
```
LIB CONNECT TO 'Oracle_192.168.200.100 (desktop-p91l30f_bi)'; #这里是数据库连接串

section access; #加载脚本头部，头部和尾部一定是需要的

#静态的数据加载
LOAD * inline [
ACCESS, USERID,YSBZDM, OMIT
USER, WIN-CBEA4QBBGAR\qlik,*,         # *只能匹配所有被加载进来的数据，例如：全集{1,2,3} 实际加载｛1，2｝
USER, WIN-CBEA4QBBGAR\zx_user,1,      # 那么*所匹配的是就是{1,2}不是设想的{1，2，3}
USER, WIN-CBEA4QBBGAR\ez_user, 2,
];

#多表源的数据加载
LOAD "ACCESS_" AS ACCESS,
     "USERID_" AS USERID,
     "BZDM" AS YSBZDM;
SQL select "ACCESS_", 
       upper(user_id) AS "USERID_",  #USERID_一定要大写
       "BZDM"
from "DW_HOS"."MC_DATA_ROLE_TEST1" a, "DW_HOS"."T_YS_DM" c
where (a.role_level_id = 1 and a.fy_id = c.fyid) or (a.role_level_id = 2 and a.fy_id = c.fyid and a.ks_id = c.ksdm)
or (a.role_level_id = 3 and a.fy_id = c.fyid and a.ks_id = c.ksdm and a.xz_id = c.xzxh);

#解决*不能通用匹配的问题我们只需要去union一个全集用户即可
union
SQL select "ACCESS_", 
       upper(user_id) AS "USERID_",  #USERID_一定要大写
       "BZDM"
from "DW_HOS"."MC_DATA_ROLE_TEST1" a, "DW_HOS"."T_YS_DM" c

section application;  #加载脚本的尾部,不论动态静态都需要

[T_YS_DM]:
SELECT "BZDM" as YSBZDM, 
	"FYID", 
    ...
FROM "DW_HOS"."T_YS_DM";
```
 
### 菜单栏的分离
1)涉及的文件：  
E_program\src\js\mod\subnav.js    菜单和头部替换文件  
E_program\src\html\subnav.html   菜单的HTML，修改菜单就修改这里  
E_program\src\html\header.html   页面头部的HTML  
2)修改mashup的html文件
```
<!-- subNav mod start -->
<div id="header"></div>
<div class="wrapper">
<div class="mc-subNav"></div>
<!-- subNav mod end -->
```
3)在尾部引用文件
```
<scriptsrc="../../resources/E_program/src/js/mod/subnav.js"></script>
```

>TIPS：  
    1.mashup的个性化CSS文件在117.c:\program files\qilk\sense\client  
    2.在页面的CSS文件中添加来控制饼图的大小  
    ```
    .qv-object-wrapper, .qv-object, .qv-object-content-container, .qv-object-content{height: 100%;}
    ```
    3.所有直方图要有高度才可以显示  
    4.去掉各图的显示标题来对准空间。  
    5.mashup迁移的时候，APPID会变更，要去手动修改。