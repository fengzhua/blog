### 增强了属性选择器

##### E[att^=val] 匹配元素`E`，且元素`E`定义了属性`att`，并且`att`的值是以`val`开头

```css
span[class^=icon]{
    background: green;
    color:#fff;
}
span[hh$=pdf]{
    background: orange;
    color: #fff;
}
span[title*=more]{
    background: blue;
    color: #fff;
}        
```

```html
<span class="iconI">我类名是icon</span>
<span hh="ssdpdf">我链接的是PDF文件</span>
<span title="我的title是moreTime">我的title是more</span>
```
效果：
![](https://fengzhua-1300368835.cos.ap-chengdu.myqcloud.com/WechatIMG21950.png)

##### :root选择器用匹配文档的根元素
在HTML中根元素始终是HTML元素。
简单点说： `:root{background:orange}` `html {background:orange;}` 得到的效果等同。 建议使用`:root`方法。
```css
:root{
    background-color: red;
}
```

##### :not(selector) 选择器匹配非指定元素/选择器的每个元素。
:not选择器称为否定选择器，和jQuery中的:not选择器一模一样，可以选择除某个元素之外的所有元素。就拿form元素来说，比如说你想给表单中除submit按钮之外的input元素添加红色边框，CSS代码可以写成：

```css
input:not([type="submit"]){
  border:1px solid red;
}
```

##### :empty 
:empty选择器表示的就是空。用来选择没有任何内容的元素，这里没有内容指的是一点内容都没有，哪怕是一个空格。 示例显示： 比如说，你的文档中有三个段落p元素，你想把没有任何内容的P元素隐藏起来。我们就可以使用“:empty”选择器来控制。

```html
<div>我是一个div</div>
<div> </div>
<div></div>​
```

```css
div{
 background: orange;
 min-height: 30px;
}
div:empty {
  display: none;
}​
```
效果：
![](https://fengzhua-1300368835.cos.ap-chengdu.myqcloud.com/WechatIMG76.png)

##### first-child 选择器用于选取属于其父元素的首个子元素的指定选择器
`:first-child`选择器表示的是选择父元素的第一个子元素的元素E。简单点理解就是选择元素中的第一个子元素，记住是子元素，而不是后代元素。 大家容易对这个选择器造成误解，如下例子：
```html
<div class="demo">
    <p>一个段落</p>
    <a>一个链接-红色吗？</a>
    <a>一个链接</a>
    <a>一个链接</a>
    <a>一个链接</a>
</div>
```
```css
div a:first-child{color: red;}
```
很多人认为 `一个链接-红色吗？`会变为红色,实际上它并没有变成红色，所以的子元素都没有变为红色。正确的理解应该是：只要E元素是它的父级的第一个子元素，就选中。它需要同时满足两个条件，一个是“第一个子元素”，另一个是“这个子元素刚好是E”。
