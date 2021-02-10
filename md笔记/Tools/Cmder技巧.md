**1, Cmder常用快捷键**

利用Tab，自动路径补全；

利用Ctrl+T建立新页签；利用Ctrl+W关闭页签;

利用Ctrl+Tab切换页签;

Alt+F4：关闭所有页签

Alt+Shift+1：开启cmd.exe

Alt+Shift+2：开启powershell.exe

Alt+Shift+3：开启powershell.exe (系统管理员权限)

Ctrl+1：快速切换到第1个页签

Ctrl+n：快速切换到第n个页签( n值无上限)

Alt + enter： 切换到全屏状态；

Ctr+r 历史命令搜索



| tab操作         | 快捷键                |
| :-------------- | :-------------------- |
| 新建tab         | Ctrl + t              |
| 关闭tab         | Ctrl + w              |
| 切换Tab         | Ctrl+Tab或Ctrl+1,2... |
| 新建CMD         | Shift + Alt + 1       |
| 新建 PowerShell | Shift + Alt + 2       |
| 全屏操作        | Alt + Enter           |



##配置快捷键

### 设置aliases及分屏打开vscode

用文本编辑器打开安装路径下 -> config -> user-aliases.cmd
添加相应的命令， 使得可以自定义一些短命令来替代某些长命令：

```
gc = git commit -am $1



sublilme = "E:\Microsoft VS Code\Code.exe" $1 -new_console:s50H 
```

其中`$1`代表`gc`命令后面添加的参数， 并且`=`后的命令可以使用`&`连接，使得gc可以一次完成多条命令任务。
这样子设置以后，使用`gc "first commit"`就会替代 `git commit -am"first commit"`时。
键入命令 `sublime` 就可直接在窗口右边50%横向打开vscode，若是想纵向打开则更改参数(new_console:s50V)，当中的数字作为百分比。（注意cmder窗口要足够大小才能分栏显示）

设置aliases

ps: 这仅仅是设置了cmd下的aliases， 如果想更改powershell下的，需要打开vendor/profile.ps1

```
Set-Alias sublime "C:\Program Files\Sublime Text 3\sublime_text.exe"
```

pss: 如果想打开sublime， 可能配置会麻烦一些， 可以参考该文章： [再见2015 再见cmd](https://link.jianshu.com/?t=http://imweb.io/topic/56b072d05c49f9d377ed8ee2)