# antd 源码 vscode 调试demo

## 使用create-react-app 初始化react 项目，然后引入antd

```bash
yarn create react-app antd-debugger-demo # 创建项目

yarn add antd # 引入antd
```

设置启动端口,在package.json中 scripts start 添加 `set PORT=3003 && react-scripts start`

> 注意：windows 环境下使用的是 set PORT=3003, 如果跨系统，安装 cross-env 进行 port 设置

## 设置debugger 配置

点击左侧调试按钮，选择添加配置，选择chrome, 编辑配置

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "chrome",
      "request": "launch",
      "name": "Launch Chrome against localhost",
      "url": "http://localhost:3003", // 此处端口和项目启动端口一致
      "webRoot": "${workspaceFolder}"
    }
  ]
}
```

## 下载 antd 源码

### antd 源码本地安装依赖后，执行 `npm run dist` 命令，生成 dist 文件夹

1.在antd 源码 的 node_modules 下面找到 @ant-design/tools/lib/getWebpackConfig.js 文件，修改如下

```js
babelConfig.sourceMap = true; // 让babel 生成 sourceMap
const config = {
  devtool: 'cheap-module-source-map',
  // ...
}
```

2.修改tsconfig.json

```json
{
  "compilerOptions": {
    "sourceMap": true, // 让ts 生成 sourceMap
    // ...
  }
}
```

3.再通过antd源码执行 `npm run dist`

> 核心：
> 想要 sourcemap 映射到 tsx 源码，需要把 devtool 设置成 cheap-module-source-map，然后开启 babel-loader 和 ts-loader 的 sourcemap。
>
