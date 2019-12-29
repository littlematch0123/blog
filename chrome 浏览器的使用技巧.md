![chrome_thumb_new.jpg](https://pic.xiaohuochai.site/blog/chrome/chrome_thumb_new.jpg)

前端工程师大部分工作成果需要在浏览器中查看，使用 chrome 浏览器的频率非常高。更好更优雅地使用 chrome，将 chrome 配置成趁手的浏览器，将极大提升编程效率。本文将详细介绍 chrome 浏览器的使用技巧


### 调试工具

下面介绍一些冷门但很有用的调试工具使用方法

【控制台】

在 chrome 开发者工具中的任何一个面板下(除 console 面板外)，按住 `esc` 都可以在该面板下方新生成一个 console 面板

 通常情况下，控制台只提供单行输入，可以用分号做分割符来执行多个 javascript 语句；而如果需要多行代码的话，可以通过组合键 `shift+enter` 来实现换行，在这种情况下代码不会被立即执行

![tool](https://pic.xiaohuochai.site/blog/chrome/chrome_tool4.png)

【代码片段】


chrome 在 `sources` 页面提供 `snippets` 一栏，这里我们可以随时编写 Javascript 代码，运行结果会打印到控制台。代码是全局保存的，在任何页面，包括新建标签页，都可以查看或运行这些代码

![tool](https://pic.xiaohuochai.site/blog/chrome/chrome_tool7.png)

【展开 DOM】

在 Elements 标签页中，如果要查看一个元素中曾经很深的子元素，可以按住 `option` 键，点击元素，则元素的 DOM 结构会完全展开

![tool](https://pic.xiaohuochai.site/blog/chrome/chrome_tool8.gif)

【搜索】

  在 `elements` 标签下使用 `ctrl+f` 搜索功能，可以使用 css 选择器，如`'.test'`，可以搜索到所有类名为 `test` 的元素
  
![tool](https://pic.xiaohuochai.site/blog/chrome/chrome_tool5.png)


【跳转位置】

  在 `elements` 标签下，选择元素节点，点击右键菜单中的 `scroll into view`，可以滚动浏览器到元素所处位置
  
![tool](https://pic.xiaohuochai.site/blog/chrome/chrome_tool6.png)

【快速定位】

 在 `Sources` 标签下的代码文件中，使用 `ctrl + p` 呼出搜索框，然后以 `:行号:列号` 的形式输入并回车，可以将代码快速定位到该位置
 
 ![tool](https://pic.xiaohuochai.site/blog/chrome/chrome_tool9.png)
  
【刷新】

一般地，人们对于刷新的印象只是停留在使用F5快捷键上。但实际上，刷新包括三种。在开发者调试工具打开的情况下，长按刷新按钮，会出现这三种刷新选项

![tool](https://pic.xiaohuochai.site/blog/chrome/chrome_tool1.png)

【代码反压缩】

  一般地，线上的 javascript 代码都是经过压缩的，基本上无法直接阅读。点击下方的大括号`｛｝`图标，浏览器会反压缩过重新排版美化当前的 javascript 代码
  
![tool](https://pic.xiaohuochai.site/blog/chrome/chrome_tool2.gif)


【计算样式】

  通过点击 `elements` 标签右侧的 `computed` 子标签，可以查看元素计算后的样式
  
![tool](https://pic.xiaohuochai.site/blog/chrome/chrome_tool3.png)



### 插件

下面将详细介绍常用的一些 chrome 插件

【Web 前端助手】

FEHelper(Web 前端助手)包含多个独立小应用，比如：Json工具、代码美化、代码压缩、二维码、Postman、markdown、网页油猴、便签笔记、信息加密与解密、随机密码生成、Crontab等，这样安装一个 FEHelper 相当于安装了多个插件

![fehelper](https://pic.xiaohuochai.site/blog/chrome/chrome_tool10.jpg)

【字符编码】

前端开发时，经常出现乱码的情况。但是，新版本的 chrome 浏览器已经没有更改字符编码的设置选择，这时要用到 `set character Encoding` 插件

　　在页面空白处，点击右键，即可选择需要的编码格式
  

![set_character_Encoding](https://pic.xiaohuochai.site/blog/chromePlug8.png)




【OneTab】

　　经常有开 10 个以上的标签页经历，更痛苦的是，有太多的标签页时，Chrome 有卡顿现象出现！使用OneTab插件，不仅节省高达 95% 的内存，还能减轻标签页混乱现象！单击 OneTab 图标，所有标签页转换成一个列表。需要再次访问标签页时，可以单独或全部恢复它们

![oneTab](https://pic.xiaohuochai.site/blog/chrome/chrome_oneTab.jpg)
 

【测量】

　　Page Ruler 可以直接查看网页一些图片的详细像素大小、具体位置等，非常实用
  
![ruler](https://pic.xiaohuochai.site/blog/chromePlug6.png)
 

### 快捷键

多记几个快捷键，就可以少点几次鼠标，提升工作效率

【标签页】
```
Command + W 关闭标签页
Command + T  打开标签页
Command + Shift + T 重新打开关闭的标签页
Command + 左方向键 后退
Command + 右方向键 前进
Command + Option + 右方向键 跳转到后一个标签页
Command + Option + 左方向键 跳转到前一个标签页
Command + 1(到 8) 跳转到特定标签页
Command + 9 跳转到最右侧标签页
```

【功能】
```
Command + Option + I 打开“开发者工具”
Command + Shift + J 打开“下载内容”页
Command + Shift + Delete 打开“清除浏览数据”选项
Command + Shift + C 打开“检查元素”选项
```

【网页】
```
Command + R 刷新
Command + L 将光标置于地址栏
Command + + 网页放大
Command + - 网页缩小
Command + 0 缩放 100%
空格键 向下滚动网页，一次一个屏幕
Shift + 空格键 向上滚动网页，一次一个屏幕
```


### 小功能

chrome 浏览器中隐藏了一些小功能

【文本编辑器】

在地址栏里，输入`data:text/html,<html contenteditable>`，当前的标签页将变成一个文本编辑器，可以用来临时记录一些数据

![notePad](https://pic.xiaohuochai.site/blog/chrome/chrome_notepad.png)

【小恐龙游戏】

无法连接网络，网页出现小恐龙的时候，按空格，就可以玩游戏了

在地址栏里，输入`chrome://dino/`，回车后，按空格，也可以出现小恐龙游戏

![game](https://pic.xiaohuochai.site/blog/chrome/chrome_game.gif)