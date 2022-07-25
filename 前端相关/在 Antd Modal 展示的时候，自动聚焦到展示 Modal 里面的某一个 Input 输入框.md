### 1. 问题点
在做项目的过程中，遇到一个需求，需要在 Antd Modal 展示的时候，自动聚焦到展示 Modal 里面的某一个 Input 输入框

### 2. 解决方案
- a. \<Modal>\</Modal> 需要添加 destroyOnClose 属性（关闭时销毁 Modal 里的子元素）
- b. \<Input /> 中添加自动对焦

### 3. 代码示例
#### （1） 示例一
```js
<Modal
    destroyOnClose={true}
>
    <Input
        autoFocus={true}
    />
</Modal>
```

#### （2） 示例二
```js
<Modal
    destroyOnClose={true}
>
    <Input
        ref={ (input) => input && input.focus() }
    />
</Modal>
```


#### （3） 示例三
将要聚焦的元素封装成一个组件中，然后在 Modal 中调用这个组件，这样就可以在新的组件中为 DOM 元素 \<input> 添加 ref
```js
<Modal
    destroyOnClose={true}
>
    <PersonalInput />
</Modal>


class PersonalInput extends React.Component {
    constructor(props) {
        super(props)
        this.inputRef = React.createRef()
    }

    componentDidMount() {
        // 打开的时候就自动聚焦
        console.log("inputRef = ", this.inputRef)
        this.inputRef.current.focus()
    }

    render() {
        return (
            <input
                type="text"
                ref={this.inputRef}
            />
        )
    }
}
```