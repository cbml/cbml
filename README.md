CBML
===

代码块标记语言（英文：Code Block Markup Language，CBML）是为标记代码区域设计的一种标记语言。

## 背景

为什么要标记代码区域？

### 目前标记代码主要功能

+ 表示一段代码约定的含义

> 如 新浪首页 源代码，广告部分

```html
<!-- 乐居广告脚本 begin-->
<script src="http://d5.sina.com.cn/litong/zhitou/leju/leju.js"></script>
<!-- 乐居广告脚本 end-->
```

+ 指定一段代码编译器处理的方式

> 如 C 语言中的宏定义

```c
#include "headfile"
#ifdef DEBUG
fprintf("variant=%d", variant);
#endif
```

+ 告诉编辑器这段代码可以折叠

> 如 C# 中 `#region`

```
#region safe
if (arr.length < 2) {
    return;
}
#endregion
```

+ 告诉代码质检工具相应规则

> 如 jslint

```js
console.log('hello'); // jshint ignore:line

/*jshint unused:true, eqnull:true */
```

```
/* jshint ignore:start */
var define;
var require;
var esl;
/* jshint ignore:end */
```

+ 表示这段代码的映射表

> 如脚本编译后的 sourceMappingURL

```
/*# sourceMappingURL=page.css.map */
```

**总之就是为了给一段代码添加额外的信息**

相信未来还有抱着各种各样代码块标记需求产生，面对这些复杂并未知的变化我们能不能把这件事做得优雅，所以我想到了 `CBML`。

## 现有定义代码块的问题

+ 不容易学习和记忆；

> 是前缀还是后缀？是 `begin` 还是 `start`

```
<!-- 乐居广告脚本 begin-->
/* jshint ignore:start */
```
+ 是否存在闭合不明显；

> `/*jshint unused:true, eqnull:true */` 要不要闭合？

+ 各语言间没有统一的解决方案。

> C# 里用 `#region`，sourceMappingURL 用 `/*# sourceMappingURL=...*/`

## 代码块标记基本需求

* 不影响开发期代码调试

* 学习成本低

* 支持各种主流代码文件

* 扩展性高

* 歧义小

## 思路

XML 就已经是很好的标记语言了，基于 XML 扩展，就能满足如上各种基本需求。

> 所以思路很简单：**将 XML 标记放到各种语言的多行注释里！**（机智）
> 实用性上考虑以扩展 HTML 为主。

## 代码块定义

### 类 C 语言

```c
/*<tag attrs />*/

/*<tag attrs>*/
int i = 1024;
/*</tag>*/

/*<tag attrs>
int i = 4096;
</tag>*/
```

### Python

```python
'''<tag attrs />'''

'''<tag attrs>'''
i = 1024
'''</tag>'''

'''<tag attrs>
i = 4096
</tag>'''
```

### 类 Pascal 语言

```pascal
(*<tag attrs />*)

(*<tag attrs>*)
const
    I: Integer = 1024;
(*</tag>*)

(*<tag attrs>
const
    I: Integer = 4096;
</tag>*)
```

### 类 Lua 语言

```lua
--[[<tag attrs />]]

--[[<tag attrs>]]
list = {}
--[[</tag>]]

--[[<tag attrs>
list = {}
</tag>]]
```

### 类 XML 语言

> 这样看上去有点累赘

```xml
<!--<tag attrs />-->

<!--<tag attrs>-->
<h1>1024</h1>
<!--</tag>-->

<!--<tag attrs>
<h1>4096</h1>
</tag>-->
```

> 所以选择了这样：

```xml
<!--tag attrs /-->

<!--tag attrs-->
<h1>1024</h1>
<!--/tag-->

<!--tag attrs>
<h1>4096</h1>
</tag-->
```

## 使用

只要 tag 不要冲突，代码块之间就能和平相处了

> 示例

```js
/*<jshint ignore>*/
var define;
var require;
/*</jshint>*/

/*<editor region="safe">*/
if (arr.length < 2) {
    return;
}
/*</editor>*/

/*<jdists encoding="aaencode">
/*</jdists>*/
```

## 后续

+ CBML 全局 tag 注册
+ CBML 相关工具
+ 社区建设
+ 运营推广
+ 文档全球化

## 参考文献

+ [Extensible Markup Language (XML)](http://www.w3.org/XML/)
+ [#region (C# Reference)](http://msdn.microsoft.com/en-us//library/9a1ybwek.aspx)
