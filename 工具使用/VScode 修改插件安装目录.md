### Windows链接



管理员权限打开cmd，利用mklink来创建目录符号链接

把C:\Users\{username}\.vscode\extensions(默认插件位置)的extensions文件夹整个剪切到你想换的位置

在cmd中输入

mklink /D "C:\Users\{username}\.vscode\extensions" "剪切后的路径"

(username即为你的账户的用户名)

类似于Linux中的软链接，仅仅是指向另一个目录的链接，并不占用空间。

**注意**：**不要将新的路径放在VS Code安装目录**，VSCode每次更新都会刷新安装目录，会导致非安装时创建的文件夹全部删除，插件也会全部丢失，extensions链接不过去会导致VS Code启动不了(双击，cmd输入code，右键->通过code打开等操作均无反应)

