### 提示：
1. 将使用到的 canvas 写成了一个组件，可以直接在项目中引入并使用
2. 在使用该组件时需要传递两个参数:
> graphPoints = [[0, 0], [100, 0], [100, 100], [0, 100]]
> circlePoints = [50, 50]
3. 包含了 **组件加载时的渲染** 以及 **数据更新后的渲染** 的功能


### 代码如下：

```
import react, { Component } from 'react'

class CanvasBox extends Component {
    constructor(props) {
        super(props);
        this.canvas = react.createRef();
    }

    componentDidMount() {
        // "graphPoints" : [[0, 0], [100, 0], [100, 100], [0, 100]],
        // "circlePoints": [50, 50],
        this.updateCanvas(this.props.graphPoints, this.props.circlePoints)
    }

    componentDidUpdate() {
        this.updateCanvas(this.props.graphPoints, this.props.circlePoints)
    }

    render() {
        return (
            <canvas
                ref={this.canvas}
                width="200"
                height="200"
                // 直接将 width 和 height 作为元素属性写上，使用 css 来限定宽高的话，会有其他问题
            >
            </canvas>
        )
    }

    updateCanvas(graphPoints, circlePoints) {
        const canvas = this.canvas.current;
        const context = canvas.getContext('2d');    // 获取上下文

        // 清除原来绘制的内容
        context.clearRect(0, 0, canvas.width, canvas.height);
        // 使用 context.stroke() 画线，如果不添加 context.beginPath()，
        // 则 context.clearRect(0, 0, canvas.width, canvas.height); 这行代码并不能清除之前画出来的线。

        this.drawPolygon(context, graphPoints)
        this.drawCircle(context, circlePoints)
    }

    drawPolygon(context, graphPoints) {
        context.beginPath()
        graphPoints.forEach((e, index) => {
            if (index == 0) {
                context.moveTo(e[0], e[1])
            } else {
                context.lineTo(e[0], e[1])
            }
        });
        context.closePath()

        context.fillStyle= "#9a9a9a"    // 填充颜色
        context.fill()      // 填充

        // context.lineWidth = 3;
        context.strokeStyle = "#747474";
        context.stroke()    // 画线
    }

    drawCircle(context, circlePoints) {
        context.beginPath();
        context.arc(circlePoints[0], circlePoints[1], 1, 0, 2*Math.PI, false);
        // context.fillStyle= "#f00"
        // context.fill()
        context.strokeStyle = "#f00";
        context.stroke();
    }
}

export default CanvasBox
```
