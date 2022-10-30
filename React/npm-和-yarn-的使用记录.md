npm 和 yarn 是目前非常流行的 NodeJS 包管理器。
以下是个人使用包管理器的记录，该用 npm 还是用 yarn 还是应该按照自己的来。

##1. npm 相关
**设置 国内镜像 来提升下载速度**
- 查看当前镜像
```npm config get registry```
- 提升下载速度-设置国内镜像-淘宝镜像
```npm config set registry https://registry.npm.taobao.org/```
- 设置官方镜像
```npm config set registry https://registry.npmjs.org/```

**npm 包管理工具存在的缺陷：**
- 下载速度慢
- 安装过程中，会出现丢包的情况
有一次从 GitHub 上拉取之前写好的 react 项目，然后使用 npm install 安装依赖环境，会出现 **react-scripts** 或 **react** 找不到的情况
- 放飞自我，和 package.json 里面给定的版本无法保持一致性
- 以上原因带来了项目安装包的不确定性，因此后来改用了 yarn

##2. yarn 相关（推荐使用）
**设置 国内镜像 来提升下载速度**
- 查看当前镜像
```yarn config get registry```
- 设置国内镜像-淘宝镜像
```yarn config set registry https://registry.npm.taobao.org --global```
- 设置官方镜像
```yarn config set registry https://registry.yarnpkg.com```

**yarn 的优点：**
- 并行下载，所以安装速度快
- 安装版本统一
- 缓存了每个下载过的包，所以再次使用时无需重复下载

**yarn 的安装和使用：**
- yarn 的安装
```npm install -g yarn```
- 初始化一个新项目
```yarn init```
- 添加依赖包
```yarn add [package]```
```yarn add [package]@[version]```
```yarn add [package]@[tag]```
- 将依赖项添加到不同依赖项类别中
```yarn add [package] --dev```
```yarn add [package] --peer```
```yarn add [package] --optional```
- 升级依赖包
```yarn upgrade [package]```
```yarn upgrade [package]@[version]```
```yarn upgrade [package]@[tag]```
- 移除依赖包
```yarn remove [package]```
- 安装项目的全部依赖
```yarn``` 或 ```yarn install ```

##3. 相关链接
[yarn 中文文档](https://yarn.bootcss.com/)
[npm 和 yarn 的区别](https://www.jianshu.com/p/254794d5e741)
