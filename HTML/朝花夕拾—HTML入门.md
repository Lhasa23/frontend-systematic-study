# HTML入门笔记

1. HTML 历史介绍

    HTML的首个公开描述出现于一个名为“HTML标签”的文件中，由**蒂姆·伯纳斯-李**于1991年底提及。它描述18个元素，包括HTML初始的、相对简单的设计。

    1991：HTML  
    1995：HTML2.0  
    1997.01月: HTML3.0(完全由W3C开发并标准化的版本)  
    1997年底: HTML4.0  
    2014年: HTML5
2. HTML 的声明

    ```html
    <!DOCTYPE html> <!-- 文档类型声明，让浏览器知道正在解析的文件是HTML文件 -->
    <html lang="en"> <!-- 元素的语言：不可编辑元素所在的语言，或者应该由用户编辑的可编辑元素的语言 -->
    <head> <!-- 一般是一些不可见元素所在 -->
      <meta charset="UTF-8"> <!-- 编码方式 -->
      <meta name="viewport" content="width=device-width, initial-scale=1.0"> <!-- 移动端页面显示适配 -->
      <title>Document</title> <!-- 页面标题，一般展示在浏览器标签页上的标题 -->
    </head>
    <body> <!-- 编写页面内容 -->

    </body>
    </html>
    ```

3. 表章节的标签（h1~h6、section、article、main、aside 等等）

    1. header: html文件的头部，一般会固定在页面顶部，可以理解为整个文件的抬头或页眉
    2. h1~h6: 正文的标题，从1到6字体大小逐渐变小
    3. main: 页面的主要内容，可以理解为文章的正文
    4. aside: 页面的次要内容，或者可以用于放置整个页面的导航栏等说明性文本
    5. section: 可以理解为正文的章节，例如小说中的第一章第二章
    6. article: 用于放置每一章节的文章内容
    7. footer: 一般会固定在页面底部，可以理解为整个文件的页脚
4. 全局属性

    1. class: 给标签一个类名，可以用`.ClassName`选择进行修改样式
    2. id: 给标签一个类名，可以用`#Id`选择进行修改样式，在js中可以直接使用Id来选择对应DOM
    3. contenteditable: 该属性使得元素内容可在网页上进行编辑
    4. hidden: 用于隐藏元素
    5. style: 标签内联样式，可直接在行内修改样式
    6. tabindex: 指示元素是否可聚焦，是否应该参与顺序键盘导航：
        * 负值表示该元素应该是可聚焦的，但不应通过顺序键盘导航到达;
        * 0 表示元素应通过顺序键盘导航可聚焦和可到达，但其相对顺序由平台约定定义;
        * 正值意味着元素应该可以通过顺序键盘导航进行聚焦和访问;元素聚焦的顺序是tabindex的增加值。如果多个元素共享相同的tabindex，则它们的相对顺序遵循它们在文档中的相对位置。
    7. title: 包含表示与其所属元素相关信息的文本，鼠标悬浮后所展示的内容
5. 内容标签（a、strong、em、code、pre 等等）

    1. ol/ul+li: 列表元素，ol为有序列表(有1.2.3.4.的列表项)，ul为无序列表(列表项为····)
    2. dl+dt+dd: 是一个包含术语定义以及描述的列表，通常用于展示词汇表或者元数据 (键-值对列表)
    3. hr: 表示段落级元素之间的主题转换（万恶的分割线）
    4. br: 在文本中生成一个换行（回车）符号
    5. a: 可以创建通向其他网页、文件、同一页面内的位置、电子邮件地址或任何其他 URL 的超链接
    6. strong: 粗体显示**文本**，重要的是文本内容
    7. em: 标记出需要用户着重阅读的**内容**
    8. code: 呈现一段代码。默认情况下，它以浏览器的默认等宽字体显示
    9. pre: 元素表示预定义格式文本
    10. blockquote: 代表其中的文字是引用内容。通常在渲染时，这部分的内容会有一定的缩进，cite属性可以设置引用内容的URL

## 附录：清除默认样式

```css
*{margin: 0; padding: 0; box-sizing: border-box;}
*::after, *::before{box-sizing: border-box;}
h1, h2, h3, h4, h5, h6{font-weight: normal;}

a{color: inherit; text-decoration: none;}
ul, ol{list-style: none;}
table{border-collapse: collapse; border-spacing: 0;}
```
