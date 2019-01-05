# 使用create-react-app脚手架搭建项目
主要内容

> * 准备工作
> * 相关技术栈 webpack配置搭建


## 准备工作
    编译器下载 && 设置
    [vscode下载](https://code.visualstudio.com/Download)
    设置语言 javascript 下载 插件 React Native Tools

    [node 下载](https://nodejs.org/zh-cn/)
    检查安装成功 node -v

## 搭建
### 1.用create-react-app 新建项目

    npm install -g create-react-app
    create-react-app 你的项目名
    cd 你的项目名
    npm run start

    启动 localhost:3000

### 2.使用react-app-rewired 扩展create-react-app的webpack配置
    npm install react-app-rewired --save-dev
    在项目根目录中创建config-overrides.js文件用于修改默认配置


    ```sh
    /* config-overrides.js */
    module.exports = function override(config, env) {
    //do stuff with the webpack config...
    return config;
    }
    ```





