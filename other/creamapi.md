[creamapi](https://cs.rin.ru/forum/viewtopic.php?f=29&t=70576)

一个来自欧洲东部西伯利亚的神秘力量，为一些本体自带DLC的游戏解锁DLC。

##### 使用方法：

1. 从上面的官网下载creamapi的本体。

   tips:官方解压密码：cs.rin.ru 

2. 下载游戏本体，把游戏根目录的 steam_api64.dll 重命名为 steam_api64_o.dll。

   tips:一定要命名成 steam_api64_o.dll。

3. 解压下载的本体文件，将nolog_build文件种的steam_api64.dll复制到游戏根目录。

   tips:与log_build的区别就是会不会生成日志文件。

4. chrome中安装[tampermonkey](https://chrome.google.com/webstore/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo)并启用。

5. 安装[GetDLCInfoFromSteamDB](https://github.com/Sak32009/GetDLCInfoFromSteamDB)的插件。

6. 在[steamdb](https://steamdb.info/)中找到游戏本体信息，并查看其现有的DLC。

7. 在页面的右下角点击插件的数据读取按钮，并允许访问进入creamapi生成页面。

8. 在页面中选中合适的creamapi版本点击convert，将生成的creamapi.ini保存到本地并放入游戏目录。

剩下的就只有启动游戏检查是否启用DLC即可。

