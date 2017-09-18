#TypeScript2NodeJs

最近在学习typescript的过程中，想到也许可以使用ts来开发node.js项目。在网上搜了一下，其实已经有很多开发者实践了这方面的内容。这里，我记录一下自己搭建开发环境的简单过程。

> 关于我，欢迎关注  
  微信：[macwink]()  

#简介
本文将简述如何使用vscode [Visual Studio Code]开发工具来搭建一套TypeScript的开发环境，主要的目的是落地留痕，同时也希望能对一些刚入门的小伙伴有一定的参考价值。[注意：Windows，Linux，OS X在操作上基本上一致，只是工具的安装有所不同，这里仅以Windows平台作为本次教程的演示环境]

TypeScript是一种由微软开发的自由和开源的编程语言，通常我们认为其是JavaScript的一个超集，且本质上向这个语言添加了可选的静态类型和基于类的面向对象编程。安德斯·海尔斯伯格，C#的首席架构师，已工作于TypeScript的开发。
TypeScript为大型应用之开发而设计，可以编译成javascript来确保兼容性。

#安装Node.js
一方面提供一个开发的Runtime；另一方面提供的npm工具，我们可以利用这个工具来安装TypeScript。

TypeScript2NodeJs
|---build                 // js目录
|  |---server.js          // tsc 编译生成的js文件
|---src                   // ts目录
|  |---server.ts
|---node_modules          // npm install --save-dev @types/node
|---typings               // typings install dt~node --global --save
|---package.json          // npm init
|---tsconfig.json         // tsc --init
|---typings.json          // typings init
|---README.md

1.首先安装NodeJS和NPM 注：npm install npm -g 安装npm，nodejs 可以在官网下载
2.mkdir TypeScript2NodeJs cd TypeScript2NodeJs\
3.npm init 创建 package.json 文件
4.tsc --init 创建 tsconfig.json 文件并修改 注：如果没有安装，可以通过 npm install -g typescript， 可以通过tsc --version 检查是否安装成功

{
    "compilerOptions": {
        "module": "commonjs",   //指定生成哪个模块系统代码
        "target": "es6",        //目标代码类型
        "noImplicitAny": false, //在表达式和声明上有隐含的'any'类型时报错。
        "sourceMap": false,     //用于debug   
        "rootDir":"./src",      //仅用来控制输出的目录结构--outDir。
        "outDir":"./build",     //重定向输出目录。   
        "watch":true            //在监视模式下运行编译器。会监视输出文件，在它们改变时重新编译。
    },
    "include":[
        "./src/**/*"
    ],
    "exclude":[
        "node_modules"
    ]
}

"compilerOptions"是编译选项
"module"是用来指定设置编译后的js代码，使用何种模块规范。由于是开发node.js项目，所以选择commonjs。
"target"是编译后的js代码遵循何种规范，可以是es3/es5/es6等等，这里为了对比ts 2.0代码和es6代码的不同，使用了"es6"。
"rootDir"是一个需要注意的地方，它会告诉编译器，此目录下的文件需要经过编译。
"include" 和 "exclude" 属性指定一个文件glob匹配模式列表。表明需要包含的文件目录或文件，以及需要过滤掉的文件或目录（也可以使用"files"配置项，不过需要一个一个文件录入,"files" 属性明确指定的文件却总是会被包含在内，不管 "exclude" 如何设置。），详见官方文档说明。
所以，添加"./src/**/*"到"include"所指向的数组，就可以指定./src下的所有文件，是我们真正需要被编译的，其他目录将会被排除。
"outDir" 指向了编译后的js代码输出的地方。在文档中也有"outFile"选项，可以把所有的ts文件按照一定顺序规则打包成一个文件，具体可以参考文档。在这里，我们优先使用outDir。

5.typings init 创建 typings.json 文件  注：如果没有安装，可以通过 npm install typings -g全局安装， 可以通过typings --version 检查是否安装成功
6.项目根目录执行 typings install dt~node --global --save
7.项目根目录执行 npm install --save-dev @types/node
8.现在在src创建一个ts文件，在通过CTRL+SHIFT+B编译，看是否能正常在build目录下生成对应的JS文件;
9.创建launch.json文件并修改：
{
    // Use IntelliSense to learn about possible Node.js debug attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "type": "node",
            "request": "launch",
            "name": "Launch Program",
            "program": "${file}",  // 修改成你自己的生成的主JS文件 我们这里是 ${workspaceRoot}/build/server.js
            "outFiles": [
                "${workspaceRoot}/out/**/*.js"
            ]
        }
    ]
}

接下来编译运行，你就可以用ts写nodejs代码同时也可以在ts层调试了，最主要的是你终于有静态检查了；
