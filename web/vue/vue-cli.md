#### **安装vue-cli并构建项目**

-----
##### 一、安装vue-cli    

安装node.js的方法就不再赘述，安装完成后打开CMD窗口。

```
npm install -g vue-cli
```

>1. npm是Node.js的包管理安装工具。    
>2. install 是 npm的安装命令。
>3. -g 是全局安装的意思
>4. vue-cli就是要安装的模块的名称

遵循以上的格式，可以使用npm命令来安装各种Node.js中可以使用的包。

>使用npm来安装很多时候会很慢，甚至安装不上，可以使用cnpm镜像。
>```
>npm install -g cnpm --registry=https://registry.npm.taobao.org 
>```
>将前面的npm命令替换成cnpm即可

安装完成后，可以在cmd中输入命令来检测以下Vue-cli是不是安装了。
```
vue -V  #注意大写
```
CMD会显示vue现在的版本好，说明安装成功。

##### 二、使用vue-cli构建一个项目    

打开CMD 进入到要创建的文件夹的目录下。

>windows下，可以直接在资源管理器中打开要创建的文件目录文件夹，在文件路径的地址栏中输入CMD直接进入目录，不用使用cd命令来进入。

```
vue init webpack flowmap
```
使用vue-cli的vue中的init命令，初始化创建了项目名称叫做flowmap的vue项目，使用的是webpack模版。

在一番下载之后，CMD中就开始弹出选项，来设置的项目了
```
? Project name (flowmap)
```
询问项目名称，如果是括号里的直接回车即可，不是请键入项目名称后回车。
```
? Project description (A Vue.js project)
```
询问项目描述，可以省略不写，直接回车。
```
? Author (lolo)
```
询问项目的作者，可以直接回车，或者输入你的昵称。
```
? Vue build (Use arrow keys)
❯ Runtime + Compiler: recommended for most users
  Runtime-only: about 6KB lighter min+gzip, but templates (or any Vue-specific HTML) are ONLY allowed in .vue files -
render functions are required elsewhere
```
询问是否安装编译器，选择第一个。回车
```
? Install vue-router? (Y/n)
```
询问是否安装vue-router，这个是管理路由的重要组件，输入Y，回车。确认安装。
>大写的字符表示如果不输入的话，使用的默认结果。  
```
? Pick an ESLint preset (Use arrow keys)
❯ Standard (https://github.com/feross/standard)
  Airbnb (https://github.com/airbnb/javascript)
  none (configure it yourself)
```
询问是否使用ESlint来检查代码格式，选择第一个标准类型，回车。
```
? Setup unit tests with Karma + Mocha? (Y/n)
```
询问是否安装测试功能，这里我们选否，输入n,回车。
```
? Setup e2e tests with Nightwatch? (Y/n)
```
询问测试内容是否安装，这里我们选否，输入n,回车。

最终，会完成设置的，可以看到有提示让输入：
```
cd flowmap
npm install
npm run dev
```
按照这个步骤进入文件夹，安装项目依赖，测试运行项目，会看到一个
```
DONE Compiled successfully in 2495ms
I Your application is running here: http://localhost:8080
```
这个时候去浏览器输入这个地址，打开后就可以看到用vue-cli创建的vue实例项目。

##### 三、vue-cli3

需要安装新的vue-cli3的手脚架来使用：

```
npm install @vue/cli -g
```

安装项目的时候，如果不希望使用vue-cli3新的工程模板，需要安装

```
npm install @vue/cli-init -g
# 安装完成后
vue init webpack your-project-name
```

有了这个搭桥工具就可以使用原来vue-cli2的工程模版来初始化工程了。

如果使用vue-cli3的新的工程模板直接使用：

```
vue create your-project-name
```

随后浏览器会弹出一个工程配置页面，可视化的。逐步配置即可。