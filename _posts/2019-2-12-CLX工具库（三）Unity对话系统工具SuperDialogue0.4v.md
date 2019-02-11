---
layout:     post
title:      CLX工具库（三）Unity对话系统工具SuperDialogue 0.4v
subtitle:   CLX Toolkit SuperDialogue 0.4v
date:       2019-2-12
author:     BY LX
header-img: img/blog-004/bg.png
catalog: true
tags:
    - Toolkit
    - Unity
    - 工具说明
    - 对话系统
---
<p>http://pmgnzie4l.bkt.clouddn.com/music/%E4%B8%8A%E6%9D%BE%E7%AF%84%E5%BA%B7%20-%20%E6%BA%A2%E7%88%B1.mp3</p>

##  主要内容  
>##### 前言  
>##### 说明  

##  正文

### 1. 前言  
由于第0.3v(0.2v)的工具在接入性方面有所欠缺，若工程中接入本工具时会产生大量耦合，且功能相对来说并不完善。
因此写下0.4v的工具，本版本搭建了如下系统：  
* 对白事件系统
* 特殊对白解析系统  

接下来会对这两个系统进行举例以便各位了解使用方法。在下面附上相关链接。
[0.2版本介绍](https://catliuxin.github.io/2019/01/19/CLX%E5%B7%A5%E5%85%B7%E5%BA%93-%E4%BA%8C-Unity%E5%AF%B9%E8%AF%9D%E7%B3%BB%E7%BB%9F%E5%B7%A5%E5%85%B7SuperDialogue0.2v/)  
附上工具下载链接：
[0.4版本下载](https://github.com/CatLiuXin/Super-Dialogue/raw/master/unitypackages/Super%20Dialogue%200.4v.unitypackage)  

### 2. 说明

下面进行的说明会根据项目工程下 `Assets/CLX Toolkit/Super Dialogue/Samples/0.4v` 文件夹内的两个场景进行说明。
各位可以进入工程直接进行学习。

1) 对白事件系统（Dialogue Event System）  
* 在工程中，我们接入某些对话的时候，可能会想在对白执行前、每句对白执行时、对白执行后触发某些事件。  
为此对 `Dialogue` 类开放了一个 `RegisterAction(DIALOGUE_EVENT type, Action action)` 接口。其中 `DIALOGUE_EVENT` 是一个枚举类型如下：  

```csharp
public enum DIALOGUE_EVENT
{
    ON_BEGIN, /// 对白执行前
    ON_STEP,  /// 每句对白执行时
    ON_END    /// 对白执行后
}
```  

* 使用如下代码，实现：  
1. 对白执行前：禁用启用对白按钮，输出 “Dialogue Begin!”
2. 每句对白执行时：输出 “Dialogue Step”  
3. 对白执行后：输出“Dialogue End!” ，启用对白按钮。  
具体代码如下：  

```csharp
using UnityEngine;
using CLX;/// Dilogue的命名空间
using UnityEngine.UI;/// 与UI打交道要加的命名空间

namespace DialogueGuidance {
    public class DilogueEventSystemTest : MonoBehaviour
    {
        [SerializeField]
        Dialogue dialogue;

        bool shouldCheck = false;

        private void Start()
        {
            #region 注册事件
            dialogue.RegisterAction(Dialogue.DIALOGUE_EVENT.ON_BEGIN, () => {
                Debug.Log("Dialogue Begin!");
                GetComponent<Button>().interactable = false;
                shouldCheck = true;
            });
            dialogue.RegisterAction(Dialogue.DIALOGUE_EVENT.ON_STEP, () => {
                Debug.Log("Dialogue Step");
            });
            dialogue.RegisterAction(Dialogue.DIALOGUE_EVENT.ON_END, () => {
                Debug.Log("Dialogue End!");
                GetComponent<Button>().interactable = true;
                shouldCheck = false;
            });
            #endregion
        }

        public void TriggerDialogue()
        {
            DialogueManager.Instance.StartDialogue(dialogue);
        }

        private void Update()
        {
            if(shouldCheck)
            {
                if(Input.GetKeyDown(KeyCode.Space))
                {
                    DialogueManager.Instance.DisplayNextSentence();
                }
            }
        }

    }
}
```  

* 效果如下所示：  
1. 启用前：  
![启用前](https://raw.githubusercontent.com/CatLiuXin/CatLiuXin.github.io/master/img/blog-004/pic01.png)
2. 点击对白按钮，对白按钮被禁用，输出 “Dialogue Begin!”。同时，对白被执行，输出 “Dialogue Step”  
![被禁用](https://raw.githubusercontent.com/CatLiuXin/CatLiuXin.github.io/master/img/blog-004/pic02.png)  
![输出](https://raw.githubusercontent.com/CatLiuXin/CatLiuXin.github.io/master/img/blog-004/pic03.png)
3. 按下空格，会切换到下一句对白，并且输出“Dialogue Step”  
![切换对白](https://raw.githubusercontent.com/CatLiuXin/CatLiuXin.github.io/master/img/blog-004/pic04.png)  
![输出](https://raw.githubusercontent.com/CatLiuXin/CatLiuXin.github.io/master/img/blog-004/pic05.png)
4. 对白结束，启用对白按钮，输出“Dialogue End!” 。  
![启用](https://raw.githubusercontent.com/CatLiuXin/CatLiuXin.github.io/master/img/blog-004/pic06.png)  
![输出](https://raw.githubusercontent.com/CatLiuXin/CatLiuXin.github.io/master/img/blog-004/pic07.png)

2) 特殊对白解析系统
* 在开发各种各样的对白的时候，有部分对白会想不仅包括纯文本的信息，而是通过解析那些文本得到一些对应的信息，处理这些信息来得到想要的结果。
例如：当我们希望通过读取Dialogue信息时，去让我们对白的颜色根据信息的不同而变化。为了实现这类功能，添加了一个 `SpecialEvent` 委托类型，其代码如下：  

```csharp
    /// <summary>
    /// 特殊文本转换成对白
    /// 以及根据特殊文本产生对应执行事件
    /// </summary>
    /// <param name="text"></param>
    /// <param name="sText"></param>
    /// <returns></returns>
    public delegate System.Action SpecialEvent(string text,out string sText);
```  

* 另外，对DialogueManager的 `StartDialogue` 函数也进行了更改，现在其签名为 `public void StartDialogue(Dialogue dialogue,bool isSpecial = false,SpecialEvent sEvent = null)` ，当要使用特殊对白解析系统时，请保证写一个 SpecialEvent 类型的委托，
然后传参调用 StartDialogue 方法即可。  
示例代码如下：  

```csharp
using CLX;
using UnityEngine.UI;
using UnityEngine;

namespace DialogueGuidance
{
    public class SpecialDialogueTest : MonoBehaviour
    {
        Text mText;
        [SerializeField]
        Dialogue dialogue;
        bool shouldCheck = false;

        /// <summary>
        /// 示例特殊文本规则为：颜色（可选） 实际文本
        /// 例如：Red 文本（这个文本展现出来的就是红色）
        /// </summary>
        /// <param name="text"></param>
        /// <param name="sText"></param>
        /// <returns></returns>
        System.Action SpecialEvent(string text, out string sText)
        {
            string[] texts = text.Split(' ');
            if(texts.Length == 1)
            {
                sText = texts[0];
                return () => {
                    mText.color = Color.black;
                };
            }
            System.Action action = null;
            switch(texts[0])
            {
                case "Red":
                    {
                        action = () => {
                            mText.color = Color.red;
                            Debug.Log("Red Text!");
                        };
                        break;
                    }
                case "Blue":
                    {
                        action = () => {
                            mText.color = Color.blue;
                            Debug.Log("Blue Text!");
                        };
                        break;
                    }
            }
            sText = texts[1];
            return action;
        }

        public void TriggerDialogue()
        {
            DialogueManager.Instance.StartDialogue(dialogue,true,SpecialEvent);
        }

        private void Start()
        {
            mText = GameObject.Find("DialogueText").GetComponent<Text>();

            #region 注册事件
            dialogue.RegisterAction(Dialogue.DIALOGUE_EVENT.ON_BEGIN, () => {
                GetComponent<Button>().interactable = false;
                shouldCheck = true;
            });
            dialogue.RegisterAction(Dialogue.DIALOGUE_EVENT.ON_END, () => {
                GetComponent<Button>().interactable = true;
                shouldCheck = false;
            });
            #endregion
        }
    }
}
```
* 对应的XML文件如下：  
```xml
<?xml version="1.0" encoding="utf-8"?>
<Dialogues>
  <Dialogue>
    <name>Box</name>
    <info>Red 我是红色哒！</info>
  </Dialogue>
  <Dialogue>
    <name>Player</name>
    <info>Blue 我是蓝色哒！</info>
  </Dialogue>
  <Dialogue>
    <name>Box</name>
    <info>我假装没有颜色</info>
  </Dialogue>
</Dialogues>
```  

* 点击对白按钮，对白按钮被禁用，对白颜色变成红色。  
![Red](https://raw.githubusercontent.com/CatLiuXin/CatLiuXin.github.io/master/img/blog-004/pic08.png)

* 点击next按钮，查看下一句对白，对白颜色变成蓝色。  
![Blue](https://raw.githubusercontent.com/CatLiuXin/CatLiuXin.github.io/master/img/blog-004/pic09.png)

* 点击next按钮，查看最后一句对白，对白颜色变成黑色。  
![Black](https://raw.githubusercontent.com/CatLiuXin/CatLiuXin.github.io/master/img/blog-004/pic10.png)



















