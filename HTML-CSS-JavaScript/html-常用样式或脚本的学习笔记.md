## （一） CSS 样式相关
### 1. 禁止拖动图片
```css
img {
    -webkit-user-drag: none;
}
```


### 2. 去除 select 默认下拉图标
```css
.select {
    appearance:none;
    -webkit-appearance: none;
    background-image: url('../../statics/tir.png');
    background-repeat: no-repeat;
    background-size: 12px;
    background-position: 50px center;
}
```


### 3. CSS 限制字数，超出部份显示点点点
```css
.text {
    width: 400px;               /*要显示文字的宽度*/
    text-overflow: ellipsis;    /*让截断的文字显示为点点。还有一个值是clip意截断不显示点点*/
    white-space: nowrap;        /*让文字不换行*/
    overflow: hidden;           /*超出要隐藏*/
}
```


### 4. 自定义滚动条
```css
/*滚动条整体样式*/
.main::-webkit-scrollbar {
    -webkit-appearance: none;
    width: 5px;
    height: 10px;
}

/*滚动条里面小方块*/
.main::-webkit-scrollbar-thumb {
    cursor: pointer;
    border-radius: 5px;
    background: #7d7ba0;
    -webkit-transition: color .2s ease;
    transition: color .2s ease;
}

.main::-webkit-scrollbar-thumb:hover {
    background: #7d7ba0;
}

/*滚动条里面轨道*/
.main::-webkit-scrollbar-track {
    -webkit-box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2);
    border-radius: 0;
    background: rgba(0, 0, 0, 0.1);
}
```


### 5. 清除浮动
```css
.main:after {
    content: "020";
    display: block;
    clear: both;
    visibility: hidden;
}
```


### 6. checkbox 图标的更换
```css
input[type="checkbox"] {
    width: 16px;
    height: 16px;
    appearance: none;
    background: url("../../statics/ic_checkbox_unchecked.png");
    outline: none;
    margin-right: 3px;
    cursor: pointer;
}

input[type=checkbox]:checked{
    background: url("../../statics/ic_checkbox_checked.png");
}
```


### 7. 文字不可选中
```css
.text {
    user-select: none;
}
```


### 8. 项目中初始化设置 index.css
```css
/* CSS Tools: Reset CSS */
html, body, div, span, applet, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
a, abbr, acronym, address, big, cite, code,
del, dfn, em, img, ins, kbd, q, s, samp,
small, strike, strong, sub, sup, tt, var,
b, u, i, center,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, embed,
figure, figcaption, footer, header, hgroup,
menu, nav, output, ruby, section, summary,
time, mark, audio, video {
    margin: 0;
    padding: 0;
    border: 0;
    font-size: 100%;
    font: inherit;
    vertical-align: baseline;
}
/* HTML5 display-role reset for older browsers */
article, aside, details, figcaption, figure,
footer, header, hgroup, menu, nav, section {
    display: block;
}
body {
    line-height: 1;
}
ol, ul {
    list-style: none;
}
blockquote, q {
    quotes: none;
}
blockquote:before, blockquote:after,
q:before, q:after {
    content: '';
    content: none;
}
table {
    border-collapse: collapse;
    border-spacing: 0;
}


/* customize */
*,
:after,
:before {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
    user-select: none;
    outline: none !important;
}

a {
    cursor: pointer;
    text-decoration: none;
}

body {
    width: 100% !important;
    overflow: hidden;
}

/* 禁止拖动图片 */
img {
    /* pointer-events: none; */
    -webkit-user-drag: none;
}
```


## （二） JavaScript 脚本相关
### 1. 禁止浏览器的鼠标右键
```css
document.oncontextmenu = function() {
    return false;
}
```


### 2. 禁止浏览器的拖拽默认事件
```javascript
globalDragFile() {
    var globalDragArea = document.querySelector('body')

    globalDragArea.addEventListener('dragover',function(e){
        e.preventDefault()
    })

    globalDragArea.addEventListener('drop', function(e) {
        e.preventDefault()
    })
}
```