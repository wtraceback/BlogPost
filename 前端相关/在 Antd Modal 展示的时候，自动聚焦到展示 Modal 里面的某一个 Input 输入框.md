### 1. 问题点
在做项目的过程中，遇到一个需求，需要在 Antd Modal 展示的时候，自动聚焦到展示 Modal 里面的某一个 Input 输入框

### 2. 解决方案
- a. \<Modal>\</Modal> 需要添加 destroyOnClose 属性（关闭时销毁 Modal 里的子元素）
- b. \<Input /> 中添加自动对焦

### 3. 代码示例
#### （1） 示例一
```html
<Modal
    destroyOnClose={true}
>
    <Input
        autoFocus={true}
    />
</Modal>
```

#### （2） 示例二
```html
<Modal
    destroyOnClose={true}
>
    <Input
        ref={ (input) => input && input.focus() }
    />
</Modal>
```