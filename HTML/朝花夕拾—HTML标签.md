# 实用标签

## \<a\>

### 介绍

可以创建通向其他网页、文件、同一页面内的位置、电子邮件地址或任何其他 URL 的超链接。

### 属性

1. href: 直译名叫超链接
    1. 作用：
       * 跳转到特定网页
       * 跳转到页面内锚点
       * 跳转到电话或邮箱

    2. 值：
       * 网址：  
          "http://google.com"  
          推荐写法："//google.com"
       * 路径：  
          "/a/b/file"  
          "./index.thml"
       * 伪协议：  
          javascript代码："javascript:alert(1);"  
          mailto：邮箱  
          tel：手机号
       * id：主要作为页面内锚点的用法
2. target：
    * 自有属性值：  
      _blank：打开一个新的标签页  
      _top：加载页面进入最顶层  
      _parent：加载页面进入上一层  
      _self：默认值，当前页面加载
    * 使用自命名：  
      window的name：可在已打开的标签页或打开新标签页加载页面  
      iframe的name：在指定的iframe中加载页面

### 细节

a标签href为空值时会导致本页面刷新，href值为#时会回到当前页面最顶部  
当想要一个a标签什么也不做或执行自己写的新功能而不产生多余操作，可以这么写`<a href="javascript:;">点击查看</a>`

## \<table>

### 介绍

HTML5中，使用`<thead>,<tbody>,<tfoot>`三个标签来区分表格内容，这三个标签内通过`<th>,<tr>,<td>`添加表格行项  
三个标签在页面中展示的位置是固定的，不会受书写顺序影响（表格结构在页面上一定是先thaed再tbody最后tfoot）

### 表格样式

1. table-layout:  
    auto: 表格及单元格的宽度取决于其包含的内容  
    fixed: 表格和列的宽度通过表格的宽度来设置，某一列的宽度仅由该列首行的单元格决定。在当前列中，该单元格所在行之后的行并不会影响整个列宽。
2. border-collapse
3. border-spacing  
    以上两种属性配合使用，使得表格的单元格线能够符合正常审美

    ```css
    table {
      border-collapse: collapse;
      border-spacing: 0;
    }
    ```

## \<img>

### 介绍

img标签会发出get请求，展示返回的图片资源

### 属性

1. alt：用于图片资源加载失败后展示的说明文字
2. height/width：一般只规定一个值后，另一个值会进行等比例缩放然后展示
3. src：图片资源的路径
4. max-width：css样式用于移动端响应式展示
