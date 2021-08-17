# 插件初始化

## 说明
开始开发插件，如果不想自己搭建大部分初始必要文件，必然需要建立一个能够方便开发的脚手架文件夹，同时也可以保证文件夹树的统一和标准。  

## 方法
```bash
# 通过node.js安装脚手架工具yeoman，并添加vscode模板
npm install -g yo generator-code
# 初始化vscode模板
yo code


     _-----_     ╭──────────────────────────╮
    |       |    │   Welcome to the Visual  │
    |--(o)--|    │   Studio Code Extension  │
   `---------´   │        generator!        │
    ( _´U`_ )    ╰──────────────────────────╯
    /___A___\   /
     |  ~  |
   __'.___.'__
 ´   `  |° ´ Y `

? What type of extension do you want to create? New Color Theme
? Do you want to import or convert an existing TextMate color theme? No, start fresh
# 设置扩展相关
? What's the name of your extension? test
? What's the identifier of your extension? test
? What's the description of your extension? test
? What's the name of your theme shown to the user? test_theme
? Select a base theme: Dark
? Initialize a git repository? Yes
```