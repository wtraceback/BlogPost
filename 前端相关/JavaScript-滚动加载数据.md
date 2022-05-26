在做网站时，由于数据不能一下子全部加载出来，因此在滚动条滚动到某一个位置时，就向后端发送请求，请求新的数据。这就是通过滚动加载数据。

# 一 滚动加载
在做滚动加载的时候有两点需要注意的地方：
1. 设定滚动条滚动到某个位置，然后发起请求
2. 如何保证加载数据的时候，不会出现重复加载的问题

## （一） 设定滚动条滚动到某个位置，然后发起请求
&nbsp;&nbsp;&nbsp;&nbsp;由于业务需求的不同，因此会存在滚动方向的不同，一般分为 **向上滚动** 和 **向下滚动**

### 1. 向上滚动 - 滚动到顶部时加载
应用场景：**聊天室**
默认显示最新的消息，设定滚动条初始位置在最底下，然后向上滚动到顶部，就加载旧的消息，即自下往上的形式。（滚动到顶部即 scrollTop === 0 的时候）
```
var changeScrollSlide = function() {
    var el = document.querySelector('.content')

    // 如果滚动到最顶端，则向后端发送请求，获取旧的消息
    if (el.scrollTop === 0) {
        console.log('向后端发送请求');
    }
}

//启动滚动监听，监听滚动条的滚动
window.addEventListener('scroll', changeScrollSlide, true)
/*
    如果设置的是内滚动，然后设置了 body 的 css 为： body {overflow: hidden}，那么直接使用
    window.addEventListener('scroll', changeScrollSlide)，是无法监听到滚动事件的
    需要为 addEventListener 添加第三个参数，使用 捕获 的方式获取
*/
```
### 2. 向下滚动 - 滚动到底部时加载
&nbsp;&nbsp;&nbsp;&nbsp;应用场景：**商城商品的展示**
&nbsp;&nbsp;&nbsp;&nbsp;默认滚动条在最顶上，然后向下滚动，滚动到底部就加载更多的数据
```
var changeScrollSlide = function() {
    var el = document.querySelector('.content')

    // 如果滚动到最底部，则向后端发送请求，获取更多的信息
    if (el.scrollHeight - el.clientHeight - el.scrollTop === 0) {
        console.log('向后端发送请求');
    }
}

// 启动滚动监听，监听滚动条的滚动
window.addEventListener('scroll', changeScrollSlide, true)
/*
    如果设置的是内滚动，然后设置了 body 的 css 为： body {overflow: hidden}，那么直接使用
    window.addEventListener('scroll', changeScrollSlide)，是无法监听到滚动事件的
    需要为 addEventListener 添加第三个参数，使用 捕获 的方式获取
*/
```
## （二） 如何保证加载数据的时候，不会出现重复加载的问题
&nbsp;&nbsp;&nbsp;&nbsp;创建一个请求标记，用来记录是否正在请求，在发出请求后，即使再次滚动到需要发送请求的位置，也不进行新的请求
- 请求中则设置 is_requesting = true;
- 请求成功则将请求标记设为可以重新请求 is_requesting = false;
- 请求失败则将 page 和 is_requesting  设置为原来的值
&nbsp;&nbsp;&nbsp;&nbsp;page = page - 1; is_requesting = false;
```
// 标记，避免重复请求和重复加载信息
var page = 1
var is_requesting = false
var changeScrollSlide = function() {
    // 如果滚动到最顶端且没有正在请求中，则向后端发送请求，获取旧的消息
    if ($('.content').scrollTop() === 0 && is_requesting === false) {
        page++
        is_requesting = true

        // 向后端发送请求
        $.ajax({
            url: '/api/test_url',
            type: 'GET',
            data: { page: page, perpage: 10, },
            success: function(data) {
                // TODO：将获取的数据渲染到页面
                is_requesting = false
            },
            error: function() {
                page--
                is_requesting = false
            }
        })
    }
}

window.addEventListener('scroll', changeScrollSlide, true)
/*
    如果设置的是内滚动，然后设置了 body 的 css 为： body {overflow: hidden}，那么直接使用
    window.addEventListener('scroll', changeScrollSlide)，是无法监听到滚动事件的
    需要为 addEventListener 添加第三个参数，使用 捕获 的方式获取
*/
```

# 二 示例代码下载
&nbsp;&nbsp;&nbsp;&nbsp;[向上滚动加载](https://github.com/wtraceback/Learning-Front-End/blob/master/%E5%8D%9A%E5%AE%A2%E7%A4%BA%E4%BE%8B%E4%BB%A3%E7%A0%81/%E5%90%91%E4%B8%8A%E6%BB%9A%E5%8A%A8%E5%8A%A0%E8%BD%BD.html)
&nbsp;&nbsp;&nbsp;&nbsp;[向下滚动加载](https://github.com/wtraceback/Learning-Front-End/blob/master/%E5%8D%9A%E5%AE%A2%E7%A4%BA%E4%BE%8B%E4%BB%A3%E7%A0%81/%E5%90%91%E4%B8%8B%E6%BB%9A%E5%8A%A8%E5%8A%A0%E8%BD%BD.html)
&nbsp;&nbsp;&nbsp;&nbsp;[设置请求标记避免重复请求](https://github.com/wtraceback/Learning-Front-End/blob/master/%E5%8D%9A%E5%AE%A2%E7%A4%BA%E4%BE%8B%E4%BB%A3%E7%A0%81/%E6%BB%9A%E5%8A%A8%E5%8A%A0%E8%BD%BD-%E8%AE%BE%E7%BD%AE%E8%AF%B7%E6%B1%82%E6%A0%87%E8%AE%B0%E9%81%BF%E5%85%8D%E9%87%8D%E5%A4%8D%E8%AF%B7%E6%B1%82.html)
