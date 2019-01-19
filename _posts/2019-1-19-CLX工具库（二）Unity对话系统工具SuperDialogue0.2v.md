---
layout:     post
title:      CLX工具库（二）Unity对话系统工具SuperDialogue 0.2v
subtitle:   CLX Toolkit SuperDialogue 0.2v
date:       2019-1-19
author:     BY LX
header-img: img/blog-002/bg.jpg
catalog: true
tags:
    - Toolkit
    - Unity
    - 工具说明
    - 对话系统
---

##  主要内容  
>##### 前言  
>##### 界面
>##### 使用方法  

## 正文  
### 1. 前言  
由于0.1版本的工具在某些地方并不直观且容易出错，在0.2版本中添加了文本编辑器以帮助开发。不过毕竟制作时间不长，难免会有bug或者反人类的地方，请各位见谅。  
附上0.1版本博客：
[0.1版本](https://catliuxin.github.io/2019/01/17/CLX%E5%B7%A5%E5%85%B7%E5%BA%93-%E4%B8%80-Unity%E5%AF%B9%E8%AF%9D%E7%B3%BB%E7%BB%9F%E5%B7%A5%E5%85%B7SuperDialogue/)  
附上工具下载链接：
[0.2版本下载](https://github.com/CatLiuXin/Super-Dialogue)  

### 2. 界面
![界面图01](https://raw.githubusercontent.com/CatLiuXin/CatLiuXin.github.io/master/img/blog-002/pic01.png)  
![界面图02](https://raw.githubusercontent.com/CatLiuXin/CatLiuXin.github.io/master/img/blog-002/pic02.png)  

### 3. 使用方法  
* 导入插件后，点击CLX\Super Dialogue Text Design 打开文本编辑窗口如下：  
![Text Design 01](https://raw.githubusercontent.com/CatLiuXin/CatLiuXin.github.io/master/img/blog-002/pic03.png)  
![Text Design 02](https://raw.githubusercontent.com/CatLiuXin/CatLiuXin.github.io/master/img/blog-002/pic01.png)  

* 点击打开按钮，选择要编辑的文件（`TXT/XML`类型的都可以），此时可以新建一个文件。不过选择的文件必须在 `Resources/Text/Dialogue/` 文件夹内。  
![Text Design 03](https://raw.githubusercontent.com/CatLiuXin/CatLiuXin.github.io/master/img/blog-002/pic04.png)  
![Text Design 04](https://raw.githubusercontent.com/CatLiuXin/CatLiuXin.github.io/master/img/blog-002/pic02.png)   

* 点击类型枚举可以更改角色。  
![Text Design 05](https://raw.githubusercontent.com/CatLiuXin/CatLiuXin.github.io/master/img/blog-002/pic05.png)   

* 灰暗的文本框是当前这个角色的文本预览，在下面的文本框里可以编辑文字。编辑好后，按 `采用此对话` 可以将文本框里编辑的文字替换掉灰暗的文本框内文字。  
![Text Design 06](https://raw.githubusercontent.com/CatLiuXin/CatLiuXin.github.io/master/img/blog-002/pic06.png)   
![Text Design 07](https://raw.githubusercontent.com/CatLiuXin/CatLiuXin.github.io/master/img/blog-002/pic07.png)   

* 按 `采用此对话` 后，按 `重置此对话` 后灰暗的文本框又会恢复成原来的文字。  
![Text Design 08](https://raw.githubusercontent.com/CatLiuXin/CatLiuXin.github.io/master/img/blog-002/pic08.png)   

* 按 `采用此对话` 后，若按 `保存此对话` 可以将此对话进行暂时保存，也就是按 `重置此对话` 后不会进行恢复。  

* 按 `上一个对话` 或者 `下一个对话` 会切换对话。  
![Text Design 09](https://raw.githubusercontent.com/CatLiuXin/CatLiuXin.github.io/master/img/blog-002/pic09.png)   

* 切换对话时，若前或者后没有对话的话，会进行提示。  
![Text Design 11](https://raw.githubusercontent.com/CatLiuXin/CatLiuXin.github.io/master/img/blog-002/pic11.png)   

* 按 `保存到文件` 会将修改保存到文件。此时打开之前打开的文件后，就可以看到转换成了XML格式的了。  
![Text Design 10](https://raw.githubusercontent.com/CatLiuXin/CatLiuXin.github.io/master/img/blog-002/pic10.png)   
