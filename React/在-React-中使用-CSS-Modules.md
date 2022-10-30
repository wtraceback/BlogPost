### 1. 为什么使用 CSS Modules？
在 React 中，如果你直接在文件中引入 index.css，当父组件和子孙组件的 class 属性相同时，那么会发生 CSS 样式覆盖的问题。如果解决样式覆盖的问题呢？
- 使用 BEM 来解决命名冲突，人为手动为每个 class 属性起一个唯一的名字
- 使用 CSS Modules，自动对 CSS 的 class 属性重命名，保证每一个 class 属性的唯一性

手动维护 class 属性的唯一性太累了，所以使用 CSS Modules 是最方便的，CSS Modules 的出现，就是为了解决 CSS 在 React 中的样式冲突和覆盖等问题的。
CSS Modules 的功能很简单，就只是加入了局部作用域和模块依赖。

### 2. CSS Modules 在项目中的使用
由于一般的脚手架都默认集成了 CSS Modules，比如 React 官方的脚手架：create-react-app，已经将 CSS Modules 集成进来了，我们可以直接使用。

#### （1） 创建一个名为 ```index.module.css``` 的样式文件（CSS Modules 在 React 的用法，和普通 css 区分开）
```
index.module.css
```
#### （2） 在 ```index.module.css``` 样式文件中编写 CSS 选择器和普通的 CSS 文件一样（CSS Modules 只是在编译的时候会自动转化为唯一的类名）
```
.类名 {
    color: #ccc;
    background-color: #0ff;
}
```
#### （3） 在组件中导入样式文件
```
import styles from './index.module.css'
```
#### （4） 通过 styles 对象访问对象中的样式名来设置样式
```
<div className={styles.类名}>react</div>
```
#### （5） 如果想要改变全局样式，也就是使用原始的类名，则需要通过 :global() 来进行设置
```
// index.module.css
:global(.类名) {

}
```
#### （6） 全局样式中，派生类选择器（父元素为 CSS Modules，而子元素为原始的类名）
```
// index.module.css
.fater_element :global(.child_element) {

}
```
#### （7） 全局样式中，多类选择器（一个是 CSS Modules，一个是原始的类名）
```
// index.module.css
.adjacent1:global(.adjacent2) {

}
```
