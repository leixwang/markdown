# Markdown
Markdown 是一种轻量级标记语言，创始人为約翰·格魯伯（John Gruber）。 它允许人们“使用易读易写的纯文本格式编写文档，然后转换成有效的XHTML(或者HTML)文档”。

John Gruber 在 2004 年创造了 Markdown 语言，在语法上有很大一部分是跟 Aaron Swartz 共同合作的。这个语言的目的是希望大家使用“易于阅读、易于撰写的纯文字格式，并选择性的转换成有效的 XHTML (或是HTML)”。 其中最重要的设计是可读性，也就是说这个语言应该要能直接在字面上的被阅读，而不用被一些格式化指令标记 (像是 RTF 与 HTML)。 因此，它是现行电子邮件标记格式的惯例，虽然它也借镜了很多早期的标记语言，如：setext、Texile、reStructuredText。 许多网站都使用 Markdown 或是其变种，例如：GitHub、reddit、Diaspora、Stack Exchange、OpenStreetMap 与 SourceForge 让用户更利于讨论。



## Github MarkDown


Markdown 标记转成HTML的样式每个网站有自己的风格, 但整体的标记格式是统一的. 我们以github来保存相关的文档, 所以我们以github的为样式为标准.


## 标题

使用`#`，可表示1-6级标题。
> \# 一级标题   
> \## 二级标题   
> \### 三级标题   
> \#### 四级标题   
> \##### 五级标题   
> \###### 六级标题    

效果：
> # 一级标题   
> ## 二级标题   
> ### 三级标题   
> #### 四级标题   
> ##### 五级标题   
> ###### 六级标题 

## 文字修饰符
看一下粗体字, 斜体字的标记.

```
*This text will be italic*
_This will also be italic_

**This text will be bold**
__This will also be bold__

~~This text will be delete~~
_You **can** combine them_
```

效果:

*This text will be italic*

_This will also be italic_

**This text will be bold**

__This will also be bold__

~~This text will be delete~~

_You **can** combine them_



## 列表
###无序列表
主要使用`-`和`*`来标记无序列表

```
- George Washington
- John Adams
* Thomas Jefferson
```

效果:

- George Washington
- John Adams
* Thomas Jefferson



### 有序列表

```
1. James Madison
2. James Monroe
3. John Quincy Adams
```

效果:

1. James Madison
2. James Monroe
3. John Quincy Adams

```
1. James Madison
1. James Monroe
1. John Quincy Adams
```

效果:

1. James Madison
1. James Monroe
1. John Quincy Adams


```
1. Make my changes
  1. Fix bug
  2. Improve formatting
    * Make the headings bigger
2. Push my commits to GitHub
3. Open a pull request
  * Describe my changes
  * Mention all the members of my team
    * Ask for feedback
```

效果:

1. Make my changes
  1. Fix bug
  2. Improve formatting
    * Make the headings bigger
2. Push my commits to GitHub
3. Open a pull request
  * Describe my changes
  * Mention all the members of my team
    * Ask for feedback



### 任务列表

```
- [x] Finish my changes
- [ ] Push my commits to GitHub
- [ ] Open a pull request
```

效果:

- [x] Finish my changes
- [ ] Push my commits to GitHub
- [ ] Open a pull request



## 段落
段落的前后要有空行，所谓的空行是指没有文字内容。若想在段内强制换行的方式是使用**两个以上**空格加上回车（引用中换行省略回车）。

### 区块引用
在段落的每行或者只在第一行使用符号`>`,还可使用多个嵌套引用，如：
> \> 区块引用  
> \>> 嵌套引用  

效果：
> 区块引用  
>> 嵌套引用 

## 链接

```
[github](http://github.com)
```

效果:

[github](http://github.com)

## 图片

```
If you want to embed images, this is how you do it:

![Image of Yaktocat](https://octodex.github.com/images/yaktocat.png)
```

效果:

If you want to embed images, this is how you do it:

![Image of Yaktocat](https://octodex.github.com/images/yaktocat.png)


## 整体样式

```
# Structured documents

Sometimes it's useful to have different levels of headings to structure your documents. Start lines with a `#` to create headings. Multiple `##` in a row denote smaller heading sizes.

### This is a third-tier heading

You can use one `#` all the way up to `######` six for different heading sizes.

If you'd like to quote someone, use the > character before the line:

> Coffee. The finest organic suspension ever devised... I beat the Borg with it.
> - Captain Janeway
```

效果:

# Structured documents

Sometimes it's useful to have different levels of headings to structure your documents. Start lines with a `#` to create headings. Multiple `##` in a row denote smaller heading sizes.

### This is a third-tier heading

You can use one `#` all the way up to `######` six for different heading sizes.

If you'd like to quote someone, use the > character before the line:

> Coffee. The finest organic suspension ever devised... I beat the Borg with it.
> - Captain Janeway





## 代码块

\`\`\`c    
\#include <stdio.h>    
int main(void){    
    printf("hello world!");    
    return 0;    
}    
\```


效果:

```c
#include <stdio.h>
int main(void){
  printf("hello world!");
  return 0;
}
```


## 支持Emoji表情
```
@octocat :+1: This PR looks great - it's ready to merge! :shipit:
```

效果:

@octocat :+1: This PR looks great - it's ready to merge! :shipit:



* [Emoji表情](http://www.webpagefx.com/tools/emoji-cheat-sheet/)



## 表格

```
标题 | 内容 | 备注
-----|------|-----
今天 | 很热 | 少穿
昨天 | 下雨 | 打伞
```

效果:

标题 | 内容 | 备注
-----|------|-----
今天 | 很热 | 少穿
昨天 | 下雨 | 打伞



# 参考文档
* [维基百科，自由的百科全书 - Markdown](https://zh.wikipedia.org/wiki/Markdown)
* [Mastering Markdown](https://guides.github.com/features/mastering-markdown/)
* [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
