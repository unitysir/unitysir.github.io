安装Visual Studio Code
下载并安装Visual Studio Code，打开Unity编辑器，选择菜单 Edit → Preferences → External Tools，修改Unity外部代码编辑器路径，选择Visual Studio Code安装路径。

安装插件
安装插件方法：点击左侧菜单栏 Extensions(快捷键: Ctrl+Shif+X)，然后搜索需要的插件，选择安装。

一、Unity开发插件
C# for Visual Studio Code
Debugger for Unity （Unity的调试工具）
Unity Tools （Unity 相关的工具：查看文档等）
Unity Snippets
Unity Code Snippets （自动补全代码）
ShaderlabVSCode
Shader languages support for VS Code
vscode-lua-format （lua代码格式化,格式化快捷键Shift Alt F）

二、其他插件
Code Runner (万能语言运行环境,让你不用搭建各种语言的开发环境，直接通过此插件就可以直接运行对应语言的代码)
Chinese (Simplified) Language Pack for Visual Studio Code （简体中文语言包）
Auto-Using for C# （自动添加命名空间）
Bracket Pair Colorizer （让你的括号有属于自己的颜色提高开发效率）
C# XML Documentation Comments （快速生成代码注释头）
Code Spell Checker （代码拼写检查）
Linux Themes for VS Code （主题）
Trailing Spaces （空格高亮）
VSCode Great Icons （改变文件夹的图标）

常见问题
Visual Studio Code编辑器识别不了UnityEngine.UI命名空间
删掉项目csproj文件，Unity中选择Microsoft Visual Studio启动（注：要勾选Generate all .csproj files），Unity会生成需要的csproj，之后就可以切回Visual Studio Code快乐的开发了。

Visual Studio Code隐藏Unity工程中meta文件
打开setting.json(设置->文本编辑器->文件->在settings.json中编辑)文件，添加如下代码


    "files.exclude": {
        "**/*.meta": true,
        "**/*.csproj": true
    },
