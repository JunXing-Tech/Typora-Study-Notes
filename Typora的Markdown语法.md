---
元数据块
---

### Typora的Markdown语法



### 块元素

#### 段落和换行符

​	段落只是一行或多行连续的文本。

#### 标题

# 		一级标题

## 		二级标题

### 		三级标题

#### 		四级标题

##### 		五级标题

###### 	六级标题

#### 引用文字

> Markdown 使用电子邮件样式>字符进行块引用。

>这是引用文字

> 引用文字要顶格写

#### 列表

>输入 `* list item 1` 将创建一个无序列表

> 输入 `1. list item 1` 将创建一个有序列表

* 无序列表01
* 无序列表02
* 无须列表03
* 无需列表 * + 空格 

1. 有序列表01
2. 有序列表02
3. 有序列表03
4. 有序列表 1. + 空格

#### 任务列表

> \- [ ] 这是一个任务列表项

- [ ] 这是一个任务列表项
- [ ] 没完成
- [x] 完成了

#### （栅栏式代）代码块

Typora仅支持Github Flavored Markdown中的栅栏式代码块

> 使用栅栏式代码块很简单：输入```之后输入一个可选的语言标识符，然后按`return键后输入代码

```java
//这是Java注释
```

#### 数学公式块

> 可以使用 **MathJax** 渲染 *LaTeX* 数学表达式。

> 输入 `$$`, 然后按“return”键将触发一个接受*Tex / LaTex*源代码的输入区域。

$$
\mathbf{V}_1 \times \mathbf{V}_2 = \begin{vmatrix} 
\mathbf{i} & \mathbf{j} & \mathbf{k} \\
\frac{\partial X} {\partial u} & \frac{\partial Y} {\partial u} & 0 \\
\frac{\partial X} {\partial v} & \frac{\partial Y} {\partial v} & 0 \\
\end{vmatrix}
$$

#### 表格

>输入 `| First Header | Second Header |` 并按下 `return` 键将创建一个包含两列的表。

> 创建表后，焦点在该表上将弹出一个表格工具栏，您可以在其中调整表格，对齐或删除表格。您还可以使用上下文菜单来复制和添加/删除列/行。

| Firsst Header |      | Second Header |
| ------------- | ---- | ------------- |
|               |      |               |

#### 注脚

> 您可以像这样创建脚注[^footnote]. 

[^footnote]: Here is the **text** of the ***\*footnote\****.
[^footnote]:Here is the *text* of the  **footnote**.

#### 水平线

> 输入 `***` 或 `---` 在空行上按 `return` 键将绘制一条水平线。

---

#### YAML Front Matter

> Typora 现在支持 [YAML Front Matter](http://jekyllrb.com/docs/frontmatter/) 。 在文章顶部输入 `---` 然后按 `Enter` 键将引入一个，或者从菜单中插入一个元数据块。

#### 目录（TOC）

> 输入 `[toc]` 然后按 `Return` 键将创建一个“目录”部分，自动从文档内容中提取所有标题，其内容会自动更新。

[toc]

#### 图表（Sequence, Flowchart and Mermaid）

> Typora 支持, [sequence](https://bramp.github.io/js-sequence-diagrams/), [flowchart](http://flowchart.js.org/) and [mermaid](https://knsv.github.io/mermaid/#mermaid), 使用前要先从偏好设置面板启用该功能。

#### Span元素

> 在您输入后Span元素会被立即解析并呈现。在这些span元素上移动光标会将这些元素扩展为markdown源代码。以下将解释这些span元素的语法。

##### 连接

> 要创建内联链接，请在链接文本的结束方括号后立即使用一组常规括号。在常规括号内，输入URL地址，以及可选的用引号括起来的链接标题。

This is  [an example](http://example.com "Title") inline link.

[This link](http://example.net) has no title attribute.

#####  内部连接

> **您可以将常规括号内的 href 设置为文档内的某一个标题**，这将创建一个书签，允许您在单击后跳转到该部分。

> "[此链接](#块元素)将跳转到标题**块元素**处

[此链接](#块元素)将跳转到标题**块元素**处

##### 参考连接

> 参考样式链接使用第二组方括号，在其中放置您选择的标签以标识链接
>
> This is [an example][id] reference-style link. 然后，在文档中的任何位置，您可以单独定义链接标签，如下所示： [id]: http://example.com/  "Optional Title Here"

This is [baidu][1] reference-style link.

[1]:http://www.baidu.com "Baidu"

##### URL网址

> Typora允许您将 URL 作为链接插入，用 `<`括号括起来`>`。

<http://www.baidu.com>

> 自动邮箱连接：<address@网址>

<address@www.baidu.com>

##### 图片

> ![替代文字](/path/to/img.jpg) 
>
> ![替代文字](/path/to/img.jpg "可选标题")

！[百度图片](https://cn.bing.com/images/search?q=%E5%9B%BE%E7%89%87&FORM=IQFRBA&id=D90445296573CB6447F802A006C81E8FDC961CAC)

##### 强调

> Markdown 将星号 (`*`) 和下划线(`_`) 视为强调的指示。用一个 `*` or `_` 包裹文本将使用HTML `<em>` 标签包裹文本

*单个星号*

_单个下划线_

> 要在用作强调分隔符的位置生成文字星号或下划线，可以用反斜杠转义

\*这个文字被文字星号包围\*

##### 粗体

> 用两个 * 或 _ 包裹的文本将使用HTML `<strong>` 标签包裹

**双星号**

__双重下划线__

##### 代码

> 要指示代码范围，请使用反引号（`）进行包裹。与预格式化的代码块不同，代码跨度表示正常段落中的代码。

`printf()`

##### 删除线

>GFM通过添加语法来创建删除线文本，标准的Markdown中缺少该文本。

~~错误的文字~~

##### 下划线

> 下划线由原始HTML提供支持<u>

<u>下划线</u>

##### 表情符号

:smile:

##### 内联数学公式

> 要使用此功能，首先，请在 `偏好设置` 面板 -> `Markdown扩展语法` 选项卡中启用它。然后使用 `$` 来包裹TeX命令，例如： `$\lim_{x \to \infty} \exp(-x) = 0$` 将呈现为LaTeX命令。

$\lim_{x \to \infty} \exp(-x) = 0$

##### 下标

> 然后用 `~` 来包裹下标内容，例如： `H~2~O`, `X~long\ text~`/

H~2~O, X~long\ text~/

#####  上标

> 用 `^` 来包裹上标内容

X^2^

##### 高亮

> 用 `==` 来包裹高亮内容

==hightlight==

#### HTML

##### 嵌入内容

```html
<iframe height='265' scrolling='no' title='Fancy Animated SVG Menu' src='http://codepen.io/jeangontijo/embed/OxVywj/?height=265&theme-id=0&default-tab=css,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'></iframe>
```

##### 视频

<video src="xxx.mp4"/>

