babel script.js -o xxx.js
babel script.js -w -o xxx.js
babel script.js -w -o xxx.js -s

编译目录:
babel src -d dist
编译整个 src 目录并将其输出到单个文件中。
npx babel src --out-file xyz.js
 



babel笔记以及.babelrc相关配置

2017-3-20    分类: mac,nodejs笔记
Babel是一个转化器，将es6代码转化成es5。也就是说你可以放心大胆的写es6的语法不必考虑兼容性，
Babel可以将你的代码转化成兼容性好的es5代码。

// 转码前
input.map(item => item + 1);

// 转码后
input.map(function (item) {
 return item + 1;
});
上面的原始代码用了箭头函数，这个特性还没有得到广泛支持，Babel将其转为普通函数，就能在现有的JavaScript环境执行了。

 

配置.babelrc

要使用Babel转码前要对.babelrc脚本进行配置：

{
   "presets": [
      "stage-3",
      "es2015"
    ],
    "plugins": [
      [
       "transform-runtime",
         {
            "polyfill": false,
            "regenerator": true
         }
      ]
    ]
}
以下方法选择其中一种即可，但前提是要配置好.babelrc

 

方法一：

//将指定目录或者文件转码

安装：

npm install --save-dev babel-cli
 

基本用法如下：

# 转码结果输出到标准输出
 $ babel example.js

 # 转码结果写入一个文件
 # --out-file 或 -o 参数指定输出文件
 $ babel example.js --out-file compiled.js
 # 或者
 $ babel example.js -o compiled.js

 # 整个目录转码
 # --out-dir 或 -d 参数指定输出目录
 $ babel src --out-dir lib
 # 或者
 $ babel src -d lib

 # -s 参数生成source map文件
 $ babel src -d lib -s
 

编辑：package.json

{
  // ...
  "devDependencies": {
     "babel-cli": "^6.0.0"
  },
  "scripts": {
     "build": "babel src -d lib" //将src目录下的所有js文件转码输出到lib目录下
  },
 }
 

执行命令

npm run build
 

 

方法二：

//也可以不需要转码，让代码运行在支持es6的环境中运行
安装在当前项目目录下：

npm install --save-dev babel-cli
要让代码能跑在es6的环境中就需要babel-node，它不用单独安装，而是随babel-cli一起安装。然后，执行babel-node就进入PEPL环境。

babel-node es6.js //让es6.js代码在es6环境中运行
 

编辑package.json

{
  "scripts": {
    "start": "babel-node ./bin/www"
  }
}
 

启动命令：

npm run start
 

 

方法三：

//每当使用require加载.js、.jsx、.es和.es6后缀名的文件，就会先用Babel进行转码。

npm install --save-dev babel-register
以express支持async/await、promise为例，在bin目录下新建1.js内容为：

require("babel-register");
require('./www');
 

以后启动express时候就运行

node ./bin/1.js即可

# mybabel
mybabel

CLI
如何使用 CLI 工具。
GitHub   ·   npm   ·   编辑此页面

Babel 内置一个 CLI，可通过命令行操作来编译文件。

安装

虽然你 可以 在你的机器上全局安装 Babel CLI, 但根据单个项目进行本地安装会更好一些。

这样做有两个主要的原因：

同一机器上的不同的项目可以依赖不同版本的 Babel, 这允许你一次更新一个项目。
这意味着在你的工作环境中没有隐含的依赖项。它将使你的项目更方便移植、更易于安装。
我们可以通过以下命令本地安装 Babel CLI:


Copy
npm install --save-dev babel-cli
注意： 如果你没有一个 package.json, 在安装之前请新建一个。这可以保证 npx 命令产生合适的交互。
在完成安装之后，你的 package.json 文件应该包括：


Copy
{
  "devDependencies": {
+   "babel-cli": "^6.0.0"
  }
}
babel

注意： 以下操作多采用 npx 命令来运行本地安装的可执行文件。 你可以将其放在 npm run script 中，也可以改为使用相对路径执行 ./node_modules/.bin/babel
编译文件
编译 script.js 并输出到 stdout


Copy
npx babel script.js
# output...
如果你想输出编译结果到单个文件，你可以使用 --out-file 或 -o。


Copy
npx babel script.js --out-file script-compiled.js
想要在修改文件后编译文件，请使用 --watch 或 -w 选项：


Copy
npx babel script.js --watch --out-file script-compiled.js
编译并输出 Source Map 文件
如果你想添加 source map 文件 你可以用 --source-maps 或者 -s。了解更多关于 source maps


Copy
npx babel script.js --out-file script-compiled.js --source-maps
如果你想使用 内联的 source map，你可以使用 --source-maps inline。


Copy
npx babel script.js --out-file script-compiled.js --source-maps inline
编译目录
编译整个 src 目录并将其输出到 lib 目录。 你可以使用 --out-dir 或 -d。 这不会覆盖 lib 中的任何其他文件或目录。


Copy
npx babel src --out-dir lib
编译整个 src 目录并将其输出到单个文件中。


Copy
npx babel src --out-file script-compiled.js
忽略文件
忽略 spec 和 test 文件


Copy
npx babel src --out-dir lib --ignore spec.js,test.js
复制文件
复制不需要编译的文件


Copy
npx babel src --out-dir lib --copy-files
使用管道符
通过管道符读取文件并编译输出到 script-compiled.js


Copy
npx babel --out-file script-compiled.js < script.js
使用插件
使用 --plugins 选项来指定编译中要使用的插件


Copy
npx babel script.js --out-file script-compiled.js --plugins=transform-runtime,transform-es2015-modules-amd
使用 Presets
使用 --presets 选项指定编译中要使用的插件


Copy
npx babel script.js --out-file script-compiled.js --presets=es2015,react
忽略 .babelrc 文件
忽略项目中 .babelrc 文件的配置并使用 cli 选项，例如


Copy
npx babel --no-babelrc script.js --out-file script-compiled.js --presets=es2015,react
高级用法
在 babel CLI 中还有更多选项可用，请参阅 options， babel --help 以及其他章节了解更多信息。

babel-node

不建议在生产环境下直接使用
你不应该在生产环境中使用 babel-node，编译中的缓存数据存储在内存中，会造成不必要的内存占用过高。而整个应用程序需要即时编译，你会一直面临应用启动的性能问题。

查看示例 Node.js server with Babel，了解如何在生产部署中使用 Babel
ES6 风格的模块加载无法正常工作
由于技术上的限制，babel-node REPL 中不完全支持 ES6 风格的模块加载。
babel 提供了第二个 CLI，其功能与 Node.js 的 CLI 完全相同，只是它会在运行之前编译 ES6 代码。

启动 REPL (Read-Eval-Print-Loop)。


Copy
npx babel-node
执行字符串格式的代码。


Copy
npx babel-node -e "class Test { }"
编译并运行 test.js。


Copy
npx babel-node test
提示：使用 rlwrap 获取具有输入历史记录的 REPL


Copy
npx rlwrap babel-node
在某些平台（如OSX）上， rlwrap 可能需要额外的参数才能正常工作，例如：


Copy
NODE_NO_READLINE=1 npx rlwrap --always-readline babel-node
使用

Copy
babel-node [options] [ -e script | script.js ] [arguments]
当用户脚本的参数名称与 node 中的原生参数选项冲突时，可以在脚本名称之前加双破折号来避免歧义


Copy
npx babel-node --debug --presets es2015 -- script.js --debug
选项
选项	Default	描述
-e, --eval [script]	 	执行字符串格式的代码
-p, --print	 	执行字符串格式的代码并且打印结果
-i, --ignore [regex]	node_modules	使用 require hook 时，忽略与此正则表达式匹配的所有文件
-x, --extensions	".js",".jsx",".es6",".es"	可识别的拓展名列表
--presets	[]	加载和使用以逗号分隔的 presets （一组插件）。
--plugins	[]	加载和使用以逗号分隔的 plugins 列表。
