### 1. 问题点
- 平常从客户端上传文件到服务器端，只需要读取 input 的 File 对象，然后将其塞到 FormData 对象中，然后使用 ajax 发送到服务器端，服务器端会有配套的读文件和写文件的操作。

- 现在遇到一个问题，就是在本地一个进程中，浏览器和其他代码间数据的发送，因为不经过 HTTP，这时候只有将文件的信息读取出来，然后通过二进制或是 base64 编码的格式发送。

- HTML5 的 [FileReader API](https://developer.mozilla.org/zh-CN/docs/Web/API/FileReader) 可以在浏览器对用户本地文件进行读取，但是在使用的过程中又遇到了新的问题，比如通过 FileReader.readAsDataURL() ，使用时它会读取一整个文件的信息，加载到内存中，50MB 以内的文件基本不会遇到什么问题，但是当文件达到 300MB 甚至是 1G 以上，直接读取一整个文件，会导致内存超出上限，浏览器这时候就会崩溃，因此如果想要处理大的文件，我们只能将大文件切成一块一块的，将其变成小文件后再去处理，FileReader 也提供了相关的 API，即 FileReader.readAsArrayBuffer() ，下面就看一下使用 FileReader.readAsArrayBuffer() 进行切块的处理吧。


### 2. 大文件切块处理的思路
1. 使用 FileReader.readAsArrayBuffer() 读取文件，在读取成功后 result 属性中将包含一个 ArrayBuffer 对象以表示所读取文件的数据。
2. 对读取到的 ArrayBuffer 对象进行拆分，放到一个数组中
3. 使用 Promise.all 来顺序的处理拆分后的数组（保证顺序传输）
4. 使用 Uint8Array 将 ArrayBuffer 拆后后的数组生成 二进制字节数组
5. 最后将二进制字节数组转为 base64 编码的字符串
6. 传输的是 base64 编码的字符串，到达目的地后，需要使用 base64 解码，才能得到原始的二进制字节信息


### 3. 以下代码实现的功能：
1. 在特定区域识别拖拽的文件
2. 有一个配置项：
小于 20 MB 算小文件，直接整个处理；
大于 20 MB 算大文件，进行切块处理；
3. 大文件切块后，将依次按照切块的顺序上传
4. 注意事项：最后从浏览器传输文件数据的时候，传输的是 base64 编码的字符串，因此传送到目的地后，需要 base64 解码后再进行操作


### 4. 代码实现
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            padding: 0;
            margin: 0;
        }

        body {
            width: 100vw;
            height: 100vh;
        }

        .drag-area-container {
            width: 800px;
            height: 800px;
            border: 1px solid #0f0;
        }

        .highlight {
            border: 10px solid #0f0;
        }
    </style>
</head>
<body>
    <div class="drag-area-container">
    </div>

    <script>
        (function() {
            // 禁止浏览器的拖拽默认事件
            var globalDragFile = function() {
                let globalDragArea = document.querySelector('body')

                globalDragArea.addEventListener('dragover',function(e){
                    e.preventDefault()
                })

                globalDragArea.addEventListener('drop', function(e) {
                    e.preventDefault()
                })
            }

            // 识别指定区域的拖动效果
            var localDragFile = function() {
                let localDragArea = document.querySelector('.drag-area-container')

                localDragArea.addEventListener('dragenter',function(e){
                    // 拖动文件到指定区域时，添加高亮
                    localDragArea.classList.add('highlight')
                    e.preventDefault()
                })

                localDragArea.addEventListener('dragover',function(e){
                    e.preventDefault()
                })

                localDragArea.addEventListener('drop', function(e) {
                    e.preventDefault()

                    // 去除高亮
                    localDragArea.classList.remove('highlight')

                    var file = e.dataTransfer.files[0]
                    console.log("file = ", file);
                    uploadFile(file)
                })
            }

            // 处理文件 && 上传文件
            var uploadFile = function(file) {
                // 使用 FileReader 读取文件的数据
                var reader = new FileReader()

                reader.onloadend = function() {
                    var file_result = this.result               // ArrayBuffer 数据对象
                    var file_length = file_result.byteLength

                    // 小于 20 MB 为小文件，则整个读取并上传
                    // 大于 20 MB 为大文件，则需要将它切成小块，分别上传
                    var step = 1024 * 1024 * 20

                    if (file_length < step) {
                        console.log("小文件，直接整个上传 ");
                        handleSmallFile(file_result)
                    } else {
                        console.log("大文件，切块分别上传 ");
                        var block_arr = splitBigFile(file_result, file_length, step)

                        handleBigFile(block_arr).then(function(results) {
                            console.log("大文件，切块上传成功 result = ", results)
                        })
                    }
                }

                reader.readAsArrayBuffer(file)
                /*
                    readAsArrayBuffer()        // 读取完成，result  属性中保存的将是被读取文件的 ArrayBuffer 数据对象。
                    readAsBinaryString()       // 读取完成，result  属性中将包含所读取文件的原始二进制数据。
                    readAsDataURL()            // 读取完成，result 属性中将包含一个 data: URL 格式的 Base64 字符串以表示所读取文件的内容。
                    readAsText()               // 读取完成，result 属性中将包含一个字符串以表示所读取的文件内容。
                */
            }

            var handleSmallFile = function(file_result) {
                // 先读取到 ArrayBuffer，再获取 ArrayBuffer 的 Uint8Array 字节数组形式，最后用 base64 编码字节数组用于传输。
                var unit8_data = new Uint8Array(file_result)            // 提取二进制字节数组，使用 Uint8Array 表示
                var base64_data = binary2base64(unit8_data)             // base64 编码

                console.log("==== handle upload start ====");
                console.log("data = ", base64_data);
                console.log("==== handle upload end ====");
            }

            // 根据指定的 step 大小，切出来指定的 step 大小的块
            var splitBigFile = function(file, file_length, step) {
                var step_times = Math.ceil(file_length / step)
                var start = 0
                var block_arr = []

                for (i = 0; i < step_times; i++) {
                    var block = file.slice(start, start + step)
                    start = start + step
                    block_arr.push(block)
                }

                return block_arr
            }

            var handleBigFile = async function(big_files) {
                return Promise.all([].map.call(big_files, function(file, index) {
                    return new Promise(function(resolve, reject) {
                        // 先读取到 ArrayBuffer，再获取 ArrayBuffer 的 Uint8Array 字节数组形式，最后用 base64 编码字节数组用于传输。
                        var view = new Uint8Array(file)             // 提取二进制字节数组，使用 Uint8Array 表示
                        var base64_data = binary2base64(view)       // base64 编码

                        console.log("==== handle upload start ====");
                        console.log("block index = ", index);
                        console.log("data = ", base64_data);
                        console.log("==== handle upload end ====");
                        resolve("Promise file")
                    })
                })).then(function(results) {
                    return results;
                })
            }

            // 二进制字节数组转 base64 编码的字符串
            var binary2base64 = function(bi) {
                let str = '';
                for (let i = 0, len = bi.length; i < len; i++) {
                    str += String.fromCharCode(bi[i]);
                }
                return btoa(str);
            }

            var __main = function() {
                // 禁止浏览器的拖拽默认事件
                globalDragFile()

                // 识别指定区域的拖动效果
                localDragFile()
            }

            __main()
        })()
    </script>
</body>
</html>
```


### 5. 参考
[使用FileReader.readAsArrayBuffer()在浏览器中处理大文件](https://joji.me/zh-cn/blog/processing-huge-files-using-filereader-readasarraybuffer-in-web-browser/)

[利用FileReader进行二进制文件传输](https://www.cnblogs.com/ggtc/p/14960410.html)