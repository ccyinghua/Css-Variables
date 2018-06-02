##  CSS Variables  

> CSS原生变量(CSS自定义属性)

#### 一、css原生变量的基础用法

变量声明使用两根连词线`--`表示变量，`$color`是属于Sass的语法，`@color`是属于Less的语法，为避免冲突css原生变量使用`--`

```
// 声明变量
--color:#000;

// 读取变量
var(--color)
```
注：<br>
1、变量声明不能包含$，[，^，(，%等字符，普通字符局限在只要是“数字[0-9]”“字母[a-zA-Z]”“下划线_”和“短横线-”这些组合，但是可以是中文，日文或者韩文
<br>
2、变量的值可以是颜色、字符串、多个值的组合等

示例：

```html
<h3>css variables基础使用</h3>
<div class="btn_box">
    <button type="button" class="login_btn">登录</button>
</div>
```

```css
/* css variables基础使用 */
:root{
	--content1:"abc";
	--content2:"efg";
	--width:calc(100px + 200px);
	--btn-bg:#279cff;
	--字体:18px;
}
.btn_box:before{
	content:var(--content1)' with add';
	display:block;
	line-height: 50px;
}
.btn_box:after{
	content:var(--content1)','var(--content2);
	display:block;
	line-height: 50px;
}
.login_btn{
	width:var(--width);
	height:50px;
	border-radius:30px;
	border:0;
	background: var(--btn-bg);
	box-shadow: 0 5px 5px rgba(39,156,255,.42);
	text-align: center;
	font-size:var(--字体);
	line-height: 50px;
	color:#fff;
	cursor:pointer;
	outline:none;
}
```
![image](https://github.com/ccyinghua/Css-Variables/blob/master/assets/1.jpg?raw=true)

#### 二、作用域

1、变量是遵循CSS语法的优先级高低的 `Id > class > 标签 > *`<br>
2、注意并无!important这种用法；<br> 
3、如果变量所在的选择器和使用变量的元素没有交集,是没有效果的。

```html
<div>蓝色</div>
<div class="divbox">绿色</div>
<div class="divbox" id="alert">红色</div>
```

```css
:root { --color: blue; }
.divbox { --color: green; }
#alert { --color: red; }
div{
	color: var(--color);
	width:300px;
	line-height: 50px;
	text-align: center;
}
```
![image](https://github.com/ccyinghua/Css-Variables/blob/master/assets/2.jpg?raw=true)

#### 三、响应式

```css
div {
  --color: #7F583F;
  --bg: #F7EFD2;
}

.mediabox {
  color: var(--color);
  background: var(--bg);
}

@media screen and (min-width: 768px) {
  body {
    --color:  #F7EFD2;
    --bg: #7F583F;
  }
}
```

#### 四、注意事项

1、属性名(例：width/height/margin....等)不可以走变量

```css
.divbox {
    --side: margin-top;
    /* 无效 */
    var(--side): 20px;
}
```

2、var()的完整的写法是`var(<自定义属性名> [, <默认值 ]?)`,在变量的名字后面可以有一个默认值，如果引用的变量没有定义（注意，仅限于没有定义），则使用后面的值作为元素的属性值

```css
body {
    background:var(--bg,skyblue);
}
```

3、如果变量值是不合法的，例如下面设置背景色background只能是色值而不能是像素，则使用背景色属性的默认值代替。

```css
body {
  --bg: 20px;
  background-color: #369;
  background-color: var(--bg, #cd0000);
}
```
等同于
```css
body {
    --bg: 20px;
    background-color: #369;
    background-color: transparent;
}
```
4、CSS变量设置数值
<br><br>
(1)
```css
h3 {
  --size: 30;   
  font-size: var(--size)px;
}
```
结果h3元素的字体大小就是本身的默认大小
<br><br>
(2)

```css
h3 {
  --size: 30px;   
  font-size: var(--size);
}

等于
h3 {
    font-size:30px;
}
```
<br>

(3)使用CSS3 calc()计算：

```css
h3 {
  --size: 30;   
  font-size: calc(var(--size) * 1px);
}
等于
h3 {
    font-size:30px;
}
```

5、如果变量值带有单位，就不能写成字符串。

```css
/* 无效 */
.divbox {
    --size: '30px';
    font-size: var(--size);
}

/* 有效 */
.divbox {
    --size: 30px;
    font-size: var(--size);
}
```
6、进行calc()运算时，最好能提供默认值: calc(var(--base-line-height, 0) * 1rem)

7、不能作为媒体查询值使用：

```css
@media screen and (min-width: var(--desktop-breakpoint) {})
```

8、图片地址，如url(var(--image-url)) ，不会生效

#### 五、兼容性处理

检测浏览器是否支持CSS自定义属性的方法。
```css
/*css*/

@supports ( (--a: 0)) {
    /* supported */
}

@supports ( not (--a: 0)) {
    /* not supported */
}
```

```javascript
// Js

if (window.CSS && window.CSS.supports && window.CSS.supports('--a', 0)) {
    alert('CSS properties are supported');
} else {
    alert('CSS properties are NOT supported');
}
```

#### 六、JS操作变量

CSS 变量可以和 JS 互相交互

```css
:root{
  --testMargin:75px;
}
```

```javascript
//  读取
var root = getComputedStyle(document.documentElement);
var cssVariable1 = root.getPropertyValue('--testMargin').trim();
console.log(cssVariable1); // '75px'
 
// 写入
document.documentElement.style.setProperty('--testMargin', '100px');
var cssVariable2 = root.getPropertyValue('--testMargin').trim();
console.log(cssVariable2);  // '100px'

// 删除
document.documentElement.style.removeProperty('--testMargin');
var cssVariable3 = root.getPropertyValue('--testMargin').trim();
console.log(cssVariable3); // '75px'
```
javascript可以把任意值存入css变量，可以读取变量的值，实现javascript与css的通信。


#### 七、CSS variables与预处理器的不同

1、预处理器变量不是实时的

```css
$color:#7F583F;

@media screen and (min-width: 768px) {
	$color: #F7EFD2;
}

.mediabox {
  	background: $color;
}
```
编译结果
```css
.mediabox {
  background: #7F583F; 
}
  
```
2、预处理器不能限定作用域

```css
$zcolor:blue;
.ulbox {
	$zcolor:red;
}
ul{
	color: $zcolor;
}
```
编译为

```css
ul {
  color: blue; 
}
```
3、预处理器变量不可互操作

原生的CSS自定义属性可以与任何CSS预处理器或纯CSS文件一起使用。

4、总结

- 相较于传统的 LESS 、SASS 等预处理器变量，CSS 变量的优点在于:
- CSS 变量的动态性，能在页面运行时更改，而传统预处理器变量编译后无法更改
- CSS 变量能够继承，能够组合使用，具有作用域
- 配合 Javascript 使用，可以方便的从 JS 中读/写


#### 八、CSS原生变量的兼容性

[https://caniuse.com/#search=css%20var](https://caniuse.com/#search=css%20var)







