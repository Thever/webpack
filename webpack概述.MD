### webpack是啥？
Webpack是一个模块打包工具(module bundler)，因为平常多用来对前端工程打包，所以也是一个前端构建工具。其最主要的功能就是模块打包，官方对这个打包过程的描述不太容易理解。
模块打包，通俗地说就是：找出模块之间的依赖关系，按照一定的规则把这些模块组织合并为一个JavaScript文件。
Webpack认为一切都是模块，JS文件、CSS文件、jpg/png图片等等都是模块。Webpack会把所有的这些模块都合并为一个JS文件，这是它最本质的工作。当然，我们可能并不想要它把这些合并成一个JS文件，这个时候我们可以通过一些规则或工具来改变它。

### webpack的安装打包

    初始化wepack
    npm init

    安装webpack和webpack-cli
    npm install --save-dev webpack@4.43.0    webpack-cli@3.3.12  

    编写引用文件b.js
    // ES6的模块化语法
    export var name = 'Jack';

    在a.js中使用b.js
    // ES6的模块化语法
    import { name } from './b.js';  
    console.log(name);

    //  打包a.js输出成bundle.js
    npx webpack a.js -o bundle.js

### webpack的配置文件
默认的配置文件是webpack.config.js,也可通过 --config 来指定打包时用的配置文件。

    "scripts": {
        "build": "webpack --config webpack.prod.js --mode production",
        "dev": "webpack-dev-server --hot --config webpack.dev.js --mode development"
    }

配置文件一般包含entry(入口)，output(输出)，mode(模式)，还可使用module加载各种loader,plugins加载各种插件。

### Webpack 安装
就是全部安装和本地安装，作用范围不一样罢了。

### Webpack loader
Loader是Webpack生态里一个重要的组成，我们一般称之为预处理器。
webpack只能处理js文件，要处理其他文件比如css,sass等我们需要使用loader插件来处理。

    //  在webpack配置文件中的module.rules中配置
    module: {
        rules: [{
            test: /\.css$/,
            use: ['style-loader', 'css-loader']
        }]
    }

### webpack的入口与出口
入口是指webpack要打包的入口文件，出口是指webpack打包后的输出文件。

### 模块化import与require及区别
JS在指定ES6提出了ES6 module(ES6模块化),经过发展，目前JS模块化使用的主要是ES6 Module和CommonJS这两种。
ES6 Module:import同步导入模块,import()异步导入模块，export输出模块。

    //  import 
    import moduleC from './c.js'

    //  import()
    import('../../index.js')

    //  export
    export var name = 'Jack';
    export var age = 18;
    export function add(a, b) {
        return a + b;
    }

CommonJS使用module.exports导出模块，require('…')导入模块,主要在Node.js中使用

    //  module.exports
    module.exports = {
        name: 'Gola',
        age: 25
    }
    //  require
    var person = require('./g.js')
    console.log(person)  // 输出{name: 'Gola',age: 25}

### webpack入口entry
entry表示webpack打包的入口。
基础目录context,如果不设置就为项目根目录。


    var path = require('path');  
    module.exports = {
        //  指定基础目录
        context: path.resolve(__dirname, './src'),
        entry: './js/a.js',  // 入口文件的实际位置是 ./src/js/a.js
        output: {
            path: path.resolve(__dirname, ''),
            filename: 'bundle.js'
        },
        mode: 'none'
    };

entry赋值选择：
入口entry是字符串形式  ==>  指定导报的入口JS文件
入口entry是数组形式  ==>  数组最后一项为入口文件，其余文件会先构建到入口文件中

    module.exports = {
        entry: ['core-js/stable', 'regenerator-runtime/runtime', './a.js'],
    };

入口entry是对象形式  ==>  打包生成多个js文件, n个属性名生成n个打包文件,属性值引用参照赋值形式

    var path = require('path');  
    module.exports = {
        entry: {
            //  把core-js/stable,regenerator-runtime/runtime放到a.js中再打包
            app: ['core-js/stable', 'regenerator-runtime/runtime', './a.js'],
            //  打包./vendor.js
            vendor: './vendor'
        },
        output: {
            //  指定打包输出路径
            path: path.resolve(__dirname, ''),
            //  指定输出的文件名
            filename: '[name].js'
        },
        mode: 'none'
    };

### webpack出口output
output就是webpack资源出口配置项。output是一个对象，它有几个重要的属性filename、path、publicPath和chunkFilename。

var path = require('path');  
module.exports = {
    entry: './a.js',
    output: {
        //  输出的文件路径(打包后文件的输出位置)，webpack4默认为dist目录
        path: path.resolve(__dirname, ''),
        //  指定入口文件输出名称
        filename: '[hash].js',
        //  资源访问路径(浏览器访问的路径)
        publicPath:'' // 资源的访问地址是https://www.apple.com/ipad/bundle-3fa2.js
        /*相对路径*/
        //  output.publicPath以"./"或"../"等开头，表示要访问的资源以当前页面url作为基础路径
        //  publicPath: "../dist/"  // 资源的访问地址是https://www.apple.com/dist/bundle-3fa2.js
        //  publicPath: "./dist/"  // 资源的访问地址是https://www.apple.com/ipad/dist/bundle-3fa2.js
        //  output.publicPath以"/"开头，表示要访问的资源以当前页面的服务器地址作为基础路径。
        publicPath: "/"  // 资源的访问地址是https://www.apple.com/bundle-3fa2.js
        publicPath: "/dist/"  // 资源的访问地址是https://www.apple.com/dist/bundle-3fa2.js
        /*绝对路径*/
        //  publicPath: "https://cdn.apple.net/"  // 资源的访问地址是https://cdn.apple.net/bundle-3fa2.js
        //  publicPath: "http://cdn.apple.net/"  // 资源的访问地址是http://cdn.apple.net/bundle-3fa2.js
        //  publicPath: "//cdn.apple.net/dist/"  // 资源的访问地址是https://cdn.apple.net/dist/bundle-3fa2.js
        //  指定非入口文件的输出名称，通常在使用异步模块的时候，会生成非入口文件的chunk
        chunkFilename:'[contenthash].js'
    },
    mode: 'none'
};

### hash、chunkhash和contenthash区别
hash、chunkhash和contenthash这三者都是根据文件内容计算出的hash值，只是它们所计算的文件不一样。
hash是根据打包中所有的文件计算出的hash值。在一次打包中，所有出口文件的filename获得的[hash]都是一样的。
chunkhash是根据打包过程中当前chunk计算出的hash值。如果Webpack配置是多入口配置，那么通常会生成多个chunk，每个chunk对应的出口filename获得的[chunkhash]是不一样的。这样可以保证打包后每一个JS文件名都不一样（这么说不太严谨，但有助于理解）。
contenthash有点像chunkhash，是根据打包时CSS内容计算出的hash值。一般在使用提取CSS的插件的时候，我们使用contenthash。例如下面的配置，我们生成的CSS文件名可能会是main.3aa2e3c6.css。

    hash根据所有的打包文件生成。(打包全体公用，区分整体上不同的打包内容)
    chunkhash会在多入口文件配置生成多个chunk中使用，每个chunk的chunkhash不一样,同一个chunk内的chunkHash一致(用来区分chunk)
    contenthash根据css内容生成，不同css内容生成不同的contenthash(用来去人内容)

### webpack预处理器loader
webpack预处理器loader，loader这个词也可以翻译成加载器。
预处理器loader本质是一个函数，它接受一个资源模块，然后将其处理成Webpack核心能使用的形式。
webpack默认只能处理js,需要各种loader才能处理css,img等其他格式文件。

### loader配置
const path = require('path');
module.exports = {
    entry: './a.js',  // a.js里引入了CSS文件
    output: {
        path: path.resolve(__dirname, ''),
        filename: 'bundle.js'
    },
    module: {
        rules: [
            {
            //  匹配.css结尾文件
            test: /\.css$/,
            //  use内容从后往前使用，先使用css-loader加载css文件,再用style-loader,用style标签把样式加载到html文件中
            use: ['style-loader', 'css-loader'],
            //  排除/node_modules/下的匹配路径
            //  exclude: /node_modules/,
            //  只处理src下的路径
            //  include: /src/,
            //  include: [resolve('src'), resolve('test')], // 指定多文件路径
            //  一般而言，exclude排除用于匹配.js处理，include与exclude一般只使用一个够了
            //  enforce: 'pre', // 'pre'优先执行，'post'最后执行
            }
        ]
    },
    mode: 'none'
};

### resource与issuer
Resource的中文意思是资源，issuer中文意思是发行人。在Webpack中被加载的模块我们称之为resource，而实施加载的模块我们称之为issuer。
app.js里引入了reset.css，这里app.js是issuer，reset.css是resource。

    // app.js
    import './src/reset.css'

在我们之前使用过的loader配置里，其实已经使用了resource，那些test和exclude等配置是使用了默认resource。下面的配置是等效的

    //  全局匹配.css文件，使用css-loader, style-loader处理,除了/node_modules/ 目录下
    //  文件名称匹配导向使用
    rules: [{
        test: /\.css$/,
        use: ['style-loader', 'css-loader'],
        exclude: /node_modules/
    }]

    //  resource中寻找对应文件使用
    rules: [{
        //  指定使用的loader
        use: ['style-loader', 'css-loader'],
        //  指定要处理的resource模块
        resource: {
            //  匹配.css文件结尾
            test: /\.css$/,
            //  排除/node_modules/目录
            exclude: /node_modules/
        }
    }]

同样的,我们也能指定issuer发行人来匹配使用loader,下面配置是一样的

    //  匹配.css结尾文件，排除/node_modules/目录下，在src目录下被.js使用的，用css-loader,style-loader来处理
    rules: [{
        test: /\.css$/,
        use: ['style-loader', 'css-loader'],
        exclude: /node_modules/,
        issuer: {
            test: /\.js$/,
            include: /src/
        }
    }]

    //  匹配在src下被.js文件结尾应用的，不在/node_modules/目录下，以.css结尾的文件，用css-loader,style-loader处理
    rules: [{
        use: ['style-loader', 'css-loader'],
        resource: {
            test: /\.css$/,
            exclude: /node_modules/
        },
        issuer: {
            test: /\.js$/,
            include: /src/
        }
    }]

### babel-loader
babel-loader是babel在webpack是制作的处理JS兼容的loader,使用它一般需要安装@babel/core babel-loader @babel/preset-env作为支持。

    //  使用babel-loader
    const path = require('path');
    module.exports = {
        entry: './a.js',
        output: {
            path: path.resolve(__dirname, ''),
            filename: 'bundle.js'
        },
        module: {
            rules: [
            {
                test: /\.js$/,
                exclude: /node_modules/,
                use: {
                    loader: 'babel-loader',
                    options: {
                        //  开启缓存
                        cacheDirectory: true,
                        //  指定要兼容的浏览器环境
                        presets: ['@babel/preset-env']
                    }
                }
            }
            ]
        },
        mode: 'none'
    };

### file-loader
file-loader从名字就上大概可以猜测出来，是一个通用文件处理loader。file-loader在Webpack中的作用是，处理文件导入地址并替换成其访问地址，并把文件输出到相应位置。其中导入地址包括了JavaScript和CSS等导入语句的地址，例如JS的import和CSS的url()。
file-loader不仅仅可以处理图片资源，它本质功能是复制资源文件并替换访问地址，音视频等资源也可以使用它。

file-loader处理js引入的图片

    // a.js引入图片
    import img from './sky.jpg';
    console.log(img);
    var dom = `<img src='${img}' />`;
    window.onload = function () {
        document.getElementById('main').innerHTML = dom;
    }

    //  安装对应资源
    npm install webpack webpack-cli --save-dev
    npm install file-loader@6.0.0

    // webpack.config.js 配置文件
    const path = require('path');
    module.exports = {
        entry: './a.js',
        output: {
            path: path.resolve(__dirname, ''),
            filename: 'bundle.js'
        },
        module: {
            rules: [{
                test: /\.jpg$/,
                use: 'file-loader'
            }]
        },
        mode: 'none'
    };

    //  新建html文件引入编译后的js文件
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <script src="bundle.js"></script>
    </head>
    <body>
        <div id="main"></div>
    </body>
    </html>

file-loader处理CSS引入的图片

    //  配置b.css文件
    body {
        background: red;
    }

    // a.js引入样式文件
    import './b.css'

    //  安装对应资源
    npm install webpack webpack-cli --save-dev
    npm install file-loader@6.0.0

    //  配置webpack.config.js
    const path = require('path');
    module.exports = {
        entry: './a.js',
        output: {
            path: path.resolve(__dirname, ''),
            filename: 'bundle.js'
        },
        module: {
            rules: [
                {
                    test: /\.css$/,
                    use: ['style-loader', 'css-loader']
                },
                {
                    test: /\.jpg$/,
                    use: 'file-loader'
                }
            ]
        },
        mode: 'none'
    };

    //  新建html文件引入编译后的文件
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <script src="bundle.js"></script>
    </head>
    <body>
    <div class="hello">Hello, Loader</div>
    </body>
    </html>

### url-loader
url-loader是上一节讲的 file-loader 的增强版，它支持file-loader的所有功能，还能计算出文件的base64编码。

    //  使用base64编码的好处是可以减少一次网络请求，提升页面加载速度。

    //  使用a.js载入2张图片文件，sky.jpg(24KB),chrome.png(4KB)
    import img1 from './sky.jpg';
    import img2 from './chrome.png';
    console.log(img1);
    console.log(img2);
    var dom1 = `<img src='${img1}' />`;
    var dom2 = `<img src='${img2}' />`;
    window.onload = function () {
        document.getElementById('img1').innerHTML = dom1;
        document.getElementById('img2').innerHTML = dom2;
    }

    //  配置webpack.config.js
    const path = require('path');
    module.exports = {
        entry: './a.js',
        output: {
            path: path.resolve(__dirname, ''),
            filename: 'bundle.js'
        },
        module: {
            rules: [
                {
                test: /\.(jpg|png)$/,
                use: {
                        loader: 'url-loader',
                        options: {
                            //  小于8kb图片会被压缩
                            //  通常会在3KB-20KB选择一个适合当前项目使用。
                            limit: 1024 * 8,
                        }
                    }
                }
            ]
        },
        mode: 'none'
    };

    //  编译后用html加载编译后的资源
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <script src="bundle.js"></script>
    </head>
    <body >
        <div id="img1"></div>
        <div id="img2"></div>
    </body>
    </html>

    ==> 编译后chrome.png(4KB)会被转换成base64，减少图片请求。

file-loader与url-loader处理后的资源名称？output.filename可以指定输出。
file-loader生成的文件默认的文件名是"[contenthash].[ext]"，contenthash是资源内容hash值，ext是文件扩展名。我们可以通过设置name项来修改生成文件的名字。
file-loader除了[contenthash]和[ext]这两个常用的占位符，还有[hash]和[name]，[hash]也是根据内容计算出的hash值，[name]是文件的原始名称。

file-loader与url-loader处理后的资源路径?
ile-loader默认使用output.publicPath作为资源访问地址。

    // webpack.config.js
    const path = require('path');
    module.exports = {
        entry: './a.js',
        output: {
            path: path.resolve(__dirname, 'dist'),
            filename: 'bundle.js'
        },
        module: {
            rules: [
                {
                test: /\.(jpg|png)$/,
                use: {
                    loader: 'url-loader',
                    options: {
                        limit: 1024 * 8,
                        name: '[name]-[contenthash:8].[ext]',
                        publicPath: './dist/'
                        }
                    }
                }
            ]
        },
        mode: 'none'
    };

注意,打包输出的文件路径为./dist,url-loader指定的资源路径也要设置成改路径，即publicPath: './dist/'

### Webpack插件plugin
顾名思义，插件是用来扩展Webpack功能的，本章重点讲解一些常用的扩展插件。
注意：loader只要安装了就能使用,插件需要安装，然后在配置文件引入后才能使用(new调用)。

### clean-webpack-plugin
clean-webpack-plugin是一个清除文件的插件。在每次打包后，磁盘空间会存有打包后的资源，在再次打包的时候，我们需要先把本地已有的打包后的资源清空，来减少它们对磁盘空间的占用。插件clean-webpack-plugin就可以帮我们做这个事情。
==> 自动清除原本打包后的资源的插件

    安装Webpack以及插件clean-webpack-plugin
    npm install --save-dev webpack@4.46.0  webpack-cli@3.3.12    
    npm install --save-dev clean-webpack-plugin@3.0.0 

    配置使用
    var path = require('path');
    //  引入插件
    var { CleanWebpackPlugin } = require('clean-webpack-plugin');
    module.exports = {
        entry: './a.js',
        output: {
            path: path.resolve(__dirname, 'dist'),
            filename: 'bundle.js'
        },
        plugins:[
            //  new调用
            new CleanWebpackPlugin() 
        ],
        mode: 'none'
    };

### copy-webpack-plugin
插件copy-webpack-plugin是用来复制文件的。在我们使用Webpack的时候，有一些本地资源，例如图片和音视频，在打包过程中没有任何模块使用到它们，但我们却想要把它们放在打包后的资源输出目录。预处理器loader是不适合做这种事情的，这个时候需要使用插件，copy-webpack-plugin这个插件就可以帮助我们完成这个事情。
==> 把没使用的资源输出到资源输出目录的插件

安装Webpack以及插件copy-webpack-plugin
npm install --save-dev webpack@4.46.0  webpack-cli@3.3.12    
npm install --save-dev copy-webpack-plugin@6.4.1 

配置webpack.config.js
var path = require('path');
var CopyPlugin = require("copy-webpack-plugin");
module.exports = {
    entry: './a.js',
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle.js'
    },
    plugins:[
        new CopyPlugin({
            //  模式指定处理方法
            patterns: [
                //  从根目录/src/img/获取文件
                //  复制到根目录/dist/image/下
                { from: path.resolve(__dirname, 'src/img/'), to: path.resolve(__dirname, 'dist/image/') },
            ],
        }),
    ],
    mode: 'none'
};

### webpack开发环境配置
开发环境的配置目标，能运行项目，能方便调试。

    //  要解决的问题
    打包样式资源(style-loader,css-loader,less-loader)
    打包html资源( html-webnpack-plugi)
    打包图片资源(html-loader url-loader file-loader)
    打包其他资源(file-loader)
    配置devServer(devServer,npx)
    //  优化
    HMR(devServer中开启，hot:true)
    source-map(devtool中设置对应配置)

### webpack生产环境的配置
考虑代码的性能、兼容性，方便调试。

    提取css成单独文件(mini-css-extract-plugin)
    css兼容性处理(postcss-loader,postcss-preset-env)
    压缩css( optimize-css-assets-webpack-plugin)
    js语法检查(eslint eslint-loader eslint-config-airbnb-base eslint-plugin-import)
    js兼容性处理(babel@core babel-loader @babel/preset-env，并进行配置)
    js压缩(mode切换成production自动压缩)
    html压缩(html-webpack-plugin)
    //  优化打包构建速度
    oneOf(优化正则匹配)
    缓存(babel开启缓存， cacheDirectory: true)
    多进程打包(繁重任务用thread-loader进行多线程打包)
    externals(不打包三方库，使用三方cdn)
    dll(三方库单独打包)

    //  优化代码性能
    tree shaking(production模式，开启es6模块化)
    code split(多入口模式，配置optimization，import语法动态语法导入)
    lazy loading(懒加载，不加载不必要资源，可以使用预加载)
    pwa(workbox-webpack-plugin，serviceworker)

### webpack环境变量
环境变量，指的是设定程序运行环境的一些参数。这里的程序也包括操作系统，操作系统本质是一个大型程序。
在我们使用Webpack的过程中，会遇到两种环境变量。
Node.js环境里的环境变量  ==>  指的是用Node.js执行JavaScript代码时可以取到的环境变量，它都存放在process.env模块。
Webpack打包模块里的环境变量  ==>  指的是我们用Webpack打包的文件里的环境变量。

处理Node.js环境变量

    //  打印当前node.js环境变量
    console.log(process.env)

    //  windows设置环境变量 MY_ENV
    set MY_ENV=dev

    //  Linux设置环境变量 MY_ENV
    export MY_ENV=dev

    //  实际开发中可以安装使用cross-env来指定环境变量
    npm install --save-dev cross-env@7.0.3 

    "scripts": {
        "dev": "cross-env NODE_ENV=development",
        "pro": "cross-env NODE_ENV=production"
    }

webpack自带的DefinePlugin插件可以设置webpack打包模块的环境变量。

    //  webpack.config.js
    var webpack = require('webpack');
    ......
    plugins: [
        new webpack.DefinePlugin({
            IS_OLD: true,
            MY_ENV: JSON.stringify('dev'),
            NAME: "'Jack'",
        }),
    ],

    //  在被打包的js文件中可以获取这些变量
    a.js
    console.log(IS_OLD);        // true
    console.log(MY_ENV);        // dev
    console.log(NAME);          // Jack