---
layout:     post
title:      2019-1-17-CLX工具库（一）Unity对话系统工具SuperDialogue
subtitle:   CLX Toolkit SuperDialogue
date:       2019-1-17
author:     BY LX
header-img: img/blog-001/bg.jpg
catalog: true
tags:
    - Toolkit
    - Unity
    - 工具说明
    - 对话系统
---

##  主要内容  
>##### 前言  
>##### 功能简介
>##### 使用方法  
>##### 补充说明  

## 正文  
### 1. 前言  

本工具为了实现一个简易NPC对话系统，为尽量便捷开发者工作而设计。本想使用现成工具，然而因为部分原因，最终还是选择了自己搭建。虽然说自己搭建，但是对白的核心逻辑仍是参考了大佬的，附上我参考的视频： [How to make a Dialogue System in Unity](https://www.youtube.com/watch?v=_nRzoTzeyxU)，在此对大佬表示感激与崇敬。附上我的代码链接 [Super Dialogue](https://github.com/CatLiuXin/Super-Dialogue-0.1v)。  

### 2. 功能简介  
+ 从指定文件夹中读取XML文件读取文本信息  
+ 根据XML文件中的信息对数据进行读取通过UI输出  
+ 文本显示与图案显示  
+ 效果示意图  
![效果示意图](https://raw.githubusercontent.com/CatLiuXin/CatLiuXin.github.io/master/img/blog-001/sample.png)

### 3. 使用方法
1. 若要引入新角色（人物或者物品），则将其图案放在默认为 `Resources/Images/NPC` 文件夹内，并且请在 `NPCEnumMgr` 添加属于该角色的 `NPC_TYPE` 枚举，并且注意该枚举类型名即该角色的名称，图案和文本都要使用此处枚举的名字，否则会出错。
![放置图案](https://raw.githubusercontent.com/CatLiuXin/CatLiuXin.github.io/master/img/blog-001/sample_img.png)  
放置枚举示例：  
```csharp
public enum NPC_TYPE
{
    Player,
    Box,
}
```

2. 若要添加新剧情，请在 `Resources/Text/Dialogue/` 路径下进行添加XML文件，在 `Dialogs/Dialog` 的 `name` 结点内添加该角色的名称，也就是之前所说的添加的那个枚举。在 `Dialogs/Dialog` 的 `info` 结点内添加他说的话的文本，注意不要过长。  
![添加XML文件](https://raw.githubusercontent.com/CatLiuXin/CatLiuXin.github.io/master/img/blog-001/sample_text.png)  
XML文件内容示例：  
![XML内容](https://raw.githubusercontent.com/CatLiuXin/CatLiuXin.github.io/master/img/blog-001/sample_xml.png)

3. 在你要写的NPC的类里添加一个 `Dialogue` 变量，并且将此处对白对应的相对 `Resources/Text/Dialogue/`  的路径赋值给这个变量的 `event_name` 成员。

4. 在你会触发对白逻辑的物体上挂载 `DialogueTrigger` 脚本，并且将此处对白对应的相对 `Resources/Text/Dialogue/`  的路径赋值给这个脚本变量的 `Dialogue.event_name` 成员。触发对白时调用其 `TriggerDialogue` 方法即可。

5. 建议使用 `CLX Toolkit/Super Dialogue/Prefab/` 内的 `Super Dialogue Canvas` 预制体作为UI界面进行更改使用。

### 4. 补充说明
1. NPC的图片路径可在 `NPCImagesMgr` 脚本内进行更改（默认无需变更）。
2. 对白信息路径在 `DialogueManager` 脚本内进行更改。
3. 通过UI按钮或者按键调用 `DialogueManager.Instance.DisplayNextSentence ()` 方法即可阅读对白中的下一句对白。
4. `DialogueManager.Instance.StartDialogue(Dialogue dialogue)` 是用来调出UI界面并且将对白数据读入的。
5. UI界面的子物体要求请阅读 `EventDialogueUI` 脚本的注释。  
6. 通过修改 `EventDialogueUI` 脚本的 `SetDialogue` 和 `EndDialogue` 方法内容来切换动画播放。  
7. 可以参考 `CLX Toolkit/Super Dialogue/Main` 场景进行学习。
