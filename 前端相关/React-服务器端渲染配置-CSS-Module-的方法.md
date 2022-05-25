##### 模块的版本
```
"css-loader": "^6.3.0",
"isomorphic-style-loader": "^5.3.2",
"style-loader": "^3.3.0"
```

##### 1. 客户端 CSS 的 webpack 配置
```
{
    module: {
        rules: [
            {
                test: /\.css?$/,
                use: ['style-loader', {
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

##### 2. 服务器端 CSS 的 webpack 配置
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
                        esModule: false,
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
