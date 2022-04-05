## VimForCpp

帮助对vim配置方法不熟悉的新手封装的一键式vim环境安装包. 主要针对终端vim用户, 适合远程ssh连接Linux服务器进行开发的场景(例如使用阿里云服务器或者腾讯云服务器等).

### 特点

---

* 安装速度快(使用码云而不是github作为源). 网络畅通情况下, 几分钟内完成 vim 插件安装.
* 无需编译直接使用 YouCompleteMe(直接下载预编译好的 ycm_core.so).
* 一键式安装. 真正做到一键式安装. 不光能一键式安装 Vim 配置, 同时也会安装依赖的程序(包括 git, neovim, ctags等)

### 支持环境

---

目前只支持 Centos7 x86_64. 

### 安装方法

---

在 shell 中执行指令(想在哪个用户下让vim配置生效, 就在哪个用户下执行这个指令. 强烈 "不推荐" 直接在 root 下执行):

```shell
curl -sLf https://gitee.com/HGtz2222/VimForCpp/raw/master/install.sh -o ./install.sh && bash ./install.sh
```

需要按照提示输入 root 密码. 您的 root 密码不会被上传, 请放心输入.

### 卸载方法

---

在安装了 VimForCpp 的用户下执行:

```shell
bash ~/.VimForCpp/uninstall.sh
```

