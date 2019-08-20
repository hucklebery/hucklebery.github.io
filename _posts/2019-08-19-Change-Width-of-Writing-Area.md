---
layout: post
title: Change Width of Writing Area
categories: Typora
description: Talk is cheap, give me code.
keywords: Width, Typora
---

# Auto Numbering for Headings

> Some of following CSS style will work for latest version of Typora (>= 0.9.9.6 on macOS, and >=0.9.13 on Windows).
>
> About where to put those CSS, please follow [Add Custom CSS](https://support.typora.io/Add-Custom-CSS/).

Add following to your base.user.css or [theme].user.css under theme folder. In my PC, the theme folder is <u>==C:\Users\XiaolinHu\AppData\Roaming\Typora\themes==</u>

The default setting is :

```
#write {
    max-width: 860px;
  	margin: 0 auto;
  	padding: 30px;
    padding-bottom: 100px;
}
```

We change 'max-width' to 1800px:

```
#write {
  max-width: 1800px; /*adjust writing area position*/
}
```
You could also use other css styles like `padding-left` or `padding-right` to adjust the writing area.

To change the width of source code mode:

```
#typora-source .CodeMirror-lines {
  max-width: auto; /*or 1000px*/
}
```