pylint 是一个PY是一个格式化代码的工具。

安装
windows下  
```
pip install pylint
```

vscode中使用pylint  
默认的setting.json中是开启了pylint的检测的  

pylint的格式化  
生成配置文件：
```  
pylint --generate-rcfile > pylint.conf
```
使用配置文件   
```
pulint --rcfile = pylint.conf
```
解决模块内部的变量也要遵循UPCASE的编写要求：  
配置文件中设置：const-rgx = [A-Za-z_][A-Za-z0-9_]*$

解决不能扩展白名单问题：   
配置文件中设置：unsafe-load-any-extension = yes

屏蔽一些不需要的警告：  
1. E1101：对第三方模块引用的报错。
2. R0903：
3. W0621：