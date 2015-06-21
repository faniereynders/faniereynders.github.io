---
layout: post
title: 'For the lazy programmer: Using Excel as a code generator'
date: '2014-07-14T15:42:00.001+02:00'
author: Fanie Reynders
tags:
- tip
- Excel
- Lazy programming
- code generation
- Formula
modified_time: '2014-07-14T15:49:41.211+02:00'
thumbnail: http://4.bp.blogspot.com/-jM23RSk85pk/U8Pal2TNcqI/AAAAAAAAAuY/MgsUnYfXR9s/s72-c/excel.png
blogger_id: tag:blogger.com,1999:blog-2911174713310708108.post-2171788802224506183
blogger_orig_url: http://www.faniereynders.com/2014/07/for-lazy-programmer-using-excel-as-code.html
---

As a 'lazy' programmer, I tend to find creative ways to generate code. One of today's challenges required me to create Enums (each with a description) from code & description pairs of unit measures found inside an Excel spreadsheet containing millions of records (okay I may be exaggerating).

Immediately, my brain started to think about possible solutions like <a href="http://en.wikipedia.org/wiki/Text_Template_Transformation_Toolkit" target="_blank">T4 generation</a> or find & replace. Luckily, I remembered I have Excel on my machine. This made things so much easier.

<!--more-->

In the spreadsheet given to me (after removing all the colour formatting and other noise), columns A & B contain the unit code and description respectively.

<a href="http://4.bp.blogspot.com/-jM23RSk85pk/U8Pal2TNcqI/AAAAAAAAAuY/MgsUnYfXR9s/s1600/excel.png" imageanchor="1"><img src="http://4.bp.blogspot.com/-jM23RSk85pk/U8Pal2TNcqI/AAAAAAAAAuY/MgsUnYfXR9s/s1600/excel.png" /></a>

Now, applying a simple Excel formula in column C could do the trick:

```
="[Description(""" & B1 & """)]" & A1 & ","
```

After copying the formula down to all the needed rows, the result is quite pleasing:

<a href="http://2.bp.blogspot.com/-iDO64rtJREo/U8PffH4XxlI/AAAAAAAAAvA/NFzni3MkzFA/s1600/result21.png" imageanchor="1">  <img src="http://2.bp.blogspot.com/-iDO64rtJREo/U8PffH4XxlI/AAAAAAAAAvA/NFzni3MkzFA/s1600/result21.png" /></a>

Back in Visual Studio, I've created an empty Enum called "Types":

<a href="http://3.bp.blogspot.com/-6Ma23zteWqg/U8PdOxZtuxI/AAAAAAAAAus/OYPDi3D6-z4/s1600/emptyEnum.png" imageanchor="1" ><img src="http://3.bp.blogspot.com/-6Ma23zteWqg/U8PdOxZtuxI/AAAAAAAAAus/OYPDi3D6-z4/s1600/emptyEnum.png" /></a>

The only thing left to do is to copy the values of column C into the body and apply document formatting (press Ctrl + D, E):

<a href="http://3.bp.blogspot.com/-abis4nPQCXg/U8Pf6rMZb1I/AAAAAAAAAvI/SY0M_7uvXuU/s1600/final.png" imageanchor="1" ><img src="http://3.bp.blogspot.com/-abis4nPQCXg/U8Pf6rMZb1I/AAAAAAAAAvI/SY0M_7uvXuU/s1600/final.png" /></a>

> Remember to resolve references!

And that's all it takes. A little Excel goes a long way.

*Till next time!*