由于工作需求，我需要将 js 的时间转换成 yyyy-MM-dd HH:mm:ss 的格式，但是 JavaScript 的标准库没有时间格式化的操作，我在 MDN 的 JavaScript 标准内置对象 > 日期对象 找到最接近我需要的，就是以下三个方法：
```
toLocaleDateString()
toLocaleString()
toLocaleTimeString()
```
效果如下：
```
>  var date = new Date()
<· date.toLocaleDateString()
<· "2021/4/5"
>  date.toLocaleString()
<· "2021/4/5 下午8:40:22"
>  date.toLocaleTimeString()
<· "下午8:40:22"
```

但是还是跟预期还是有些差距，因此就只好自己写代码来转换了
```
/*
    时间戳的 yyyy-MM-dd HH:mm:ss 格式化
 */
var dateFormat = function() {
    var date = new Date()
    var year = date.getFullYear()
    var month = date.getMonth() + 1
    var day = date.getDate()

    var hour = date.getHours()
    var minu = date.getMinutes()
    var sec = date.getSeconds()

    month = month >= 10 ? month : '0' + month
    day = day >= 10 ? day : '0' + day
    hour = hour >= 10 ? hour : '0' + hour
    minu = minu >= 10 ? minu : '0' + minu
    sec = sec >= 10 ? sec : '0' + sec

    return `${year}-${month}-${day} ${hour}:${minu}:${sec}`
}

>  dateFormat()
<· "2021-04-05 20:41:07"
```

上面的代码不仅可以转换当前时间，还可以将 时间戳（从 1970 年 1 月 1 日开始到当前时间所经过的秒数）当做参数传进去，如下所示：
```

var dateFormat = function(seconds) {
    // js 只认毫秒数，因此需要将 时间戳 * 1000
    var date = new Date(seconds * 1000)
    var year = date.getFullYear()
    var month = date.getMonth() + 1
    var day = date.getDate()

    var hour = date.getHours()
    var minu = date.getMinutes()
    var sec = date.getSeconds()

    month = month >= 10 ? month : '0' + month
    day = day >= 10 ? day : '0' + day
    hour = hour >= 10 ? hour : '0' + hour
    minu = minu >= 10 ? minu : '0' + minu
    sec = sec >= 10 ? sec : '0' + sec

    return `${year}-${month}-${day} ${hour}:${minu}:${sec}`
}
```
***
如果自己的工作中对日期时间处理的需求非常多样化，推荐使用 moment.js 这个 JavaScript 日期处理类库 

moment.js 的官方文档链接：
http://momentjs.cn/
