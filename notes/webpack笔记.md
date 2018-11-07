### 核心概念
- 入口（entry）
- 出口 （output）
- loader
- 插件（plugins）

1. 入口
  
    入口是webpack作为内部依赖构件图的开始。进入入口起点后，webpack会在入口文件中查找对应直接或者间接依赖的模块。每个依赖依次去处理，最后输出到出口制定的文件中
    
    **webpack.config.js**
    ```javascript
    module.exports = {
      entry: './path/to/my/entry/file.js'
    };
    ```

2. 出口

    output 告诉webpack在哪输出创建的 bundles，以及如何命名这些文件

    **webpack.config.js**
    ```javascript
      const path = require('path');

      module.exports = {
        entry: './path/to/my/entry/file.js',
        output: {
          path: path.resolve(__dirname, 'dist'),
          filename: 'my-first-webpack.bundle.js'
        }
      };
    ```
    在上面的示例中，我们通过 output.filename 和 output.path 属性，来告诉 webpack bundle 的名称，以及我们想要 bundle 生成(emit)到哪里。

3. loader

    因为 webpack 只能理解 javascript 文件，所以，一些非 javascript 文件就需要用 loader 来处理了，loader 能将所有类型的文件转换成webpack能够处理的有效模块。

    在 webpack 的配置中 loader 有两个目标：

    1. test 属性，用于标识出应该被对应的 loader 进行转换的某个或某些文件。
    2. use 属性，表示进行转换时，应该使用哪个 loader。

    **webpack.config.js**
    ```javascript
    const path = require('path');

    const config = {
      output: {
        filename: 'my-first-webpack.bundle.js'
      },
      module: {
        rules: [
          { test: /\.txt$/, use: 'raw-loader' }
        ]
      }
    };

    module.exports = config;
    ```
    以上配置中，对一个单独的 module 对象定义了 rules 属性，里面包含两个必须属性：test 和 use。这告诉 webpack 编译器(compiler) 如下信息：

    >“嘿，webpack 编译器，当你碰到「在 require()/import 语句中被解析为 '.txt' 的路径」时，在你对它打包之前，先使用 raw-loader 转换一下。”


4. 插件

    新定义环境中的变量。插件接口功能极其强大，可以用来处理各种各样的任务

    想要使用一个插件，你只需要 require() 它，然后把它添加到 plugins 数组中。多数插件可以通过选项(option)自定义。你也可以在一个配置文件中因为不同目的而多次使用同一个插件，这时需要通过使用 new 操作符来创建它的一个实例

    **webpack.config.js**
    ```javascript
    const HtmlWebpackPlugin = require('html-webpack-plugin'); // 通过 npm 安装
    const webpack = require('webpack'); // 用于访问内置插件

    const config = {
      module: {
        rules: [
          { test: /\.txt$/, use: 'raw-loader' }
        ]
      },
      plugins: [
        new HtmlWebpackPlugin({template: './src/index.html'})
      ]
    };

    module.exports = config;
    ```

5. 模式

    通过选择 development 或 production 之中的一个，来设置 mode 参数，你可以启用相应模式下的 webpack 内置的优化
    ```javascript
    module.exports = {
      mode: 'production'
    };
    ```