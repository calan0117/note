### VUE项目的代码格式化配置vetur、eslint、prettier的故事

-------

#### 准备
在vscode中写vue页面是一件很快乐的事情。     
在使用vue-cli创建一个vue项目的时候我们多会选择一个eslint来对我们的代码风格和样式做一个监控的样子。   
可是无尽的报错让人脑壳疼，所以用插件来搞定这些问题。方法可能会用到一下插件：     

1. eslint     
这个插件主要是可以在写的时候就提醒，代码中所存在的不符合要求的地方。

2. vetur     
写vue必备的神器道具。后面用来这是相关的格式工具

3.  prettier - code formatter        
用来格式化的格式化工具针对vue需要配置一下。

------

#### 让vue代码完美符合vue-cli的eslint要求有2种方法： 

1. ##### vuter + eslint      

  调用vuter默认的prettier格式化代码的时候，没有设置的情况下会有单引号和尾部跟;符号的问题。
  如果按照网上的一些方法将vuter的js格式器改为vscode-typescript就可以解决。
```json
"vetur.format.defaultFormatter.js": "vscode-typescript",
"javascript.format.insertSpaceBeforeFunctionParenthesis": true
```
这样就解决了VUE中的格式化问题，但是它不会更正你已经存在的字符串的双引号问题。例如：
```js
export default{
  name: "test"
}
```
eslint的autofix其实很强，可以自行解决双引号，尾部;的问题。按照eslint的规则去自动修复一些问题。     
```json
"eslint.validate": ["javascript", "javascriptreact", {"language":"vue","autoFix": true}],
"eslint.autoFixOnSave": true,
```
这样配置好，就可以快乐的使用了。    

---------

那有小伙伴觉得我不能放弃prettier,我还有其他的东西要格式化，那么看下一种：     
2. ##### vuter + eslint + prettier

在vscode中setting.json做一些设置：使用下面的配置就可以直接格式化vue文件。

eslint设置 {"language":"vue","autoFix": true} 第一可以检测到vue,第二用eslint解决space-before-function-parent。
```json
"eslint.validate": ["javascript", "javascriptreact", {"language":"vue","autoFix": true}],
"eslint.autoFixOnSave": true,
```

vuter需要的设置
```json
"vetur.format.defaultFormatter.html": "prettyhtml",
"vetur.format.defaultFormatter.js": "prettier",
```
**vuter引用格式化工具的时候的配置，例如prettier是要在这里配置,而直接在setting.json中对prettier的设置是无效的。**      
这里的配置让prettier直接就格式化完了基本所有的东西，包括双引号，尾部;的问题，eslint的autofix只是解决了function参数前要插入一个空格的问题。
```json    
"vetur.format.defaultFormatterOptions": {
  "prettier": {
    "singleQuote": true,
    "semi": false,
  }
},
```
配置好就可以快乐使用格式化了。    
其实仔细看会注意到了这个选项：

```json
"vetur.format.defaultFormatter.js": "prettier-eslint"
```
但是使用因为prettier-eslint无法格式化vue中的script,可能后期作者会改进。我们目前只有使用eslint来解决这个问题，如果后期能够支持的话，如下面的设置一样，prettier就可以像在外部设置直接按照eslint规则来格式化vue中的scirpt内容。
```json    
"vetur.format.defaultFormatterOptions": {
  "prettier": {
    "singleQuote": true,
    "semi": false,
    "eslintIntegration": true
  }
},
```

最后，祝愿小伙伴们千行无BUG。