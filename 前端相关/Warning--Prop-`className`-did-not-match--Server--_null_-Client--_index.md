React 服务器端渲染，配置 CSS Module 的 webpack 打包编译，出现了已经警告，提示说服务器端的 className 和 客户端打包编译之后的 className 不一致

报错情况如下图所示：
![浏览器上显示的报错情况](./Warning--Prop-`className`-did-not-match--Server--_null_-Client--_index.assets/17731575-06d716b0f3fba5a1.png)


###### 服务器端 CSS 的 webpack 配置
```
{
    module: {
        rules: [
            {
                test: /\.css?$/,
                use: ['isomorphic-style-loader', {
                      loader: 'css-loader',
                      options: {
                        importLoaders: 1,
                        modules: {
                            // 自定义生成的类名
                            localIdentName: '[name]_[local]_[hash:base64:5]',
                        }
                      }
                }]
            }
        ],
    },
};

```

###### 正确的配置应该如下图所示
![正确的服务器端 CSS 的 webpack 配置](./Warning--Prop-`className`-did-not-match--Server--_null_-Client--_index.assets/17731575-cebdc10073ddd5cc.png)

* **配置后请重启服务**
* **强制刷新并清除浏览器的缓存**
