
# 1.前言

Vue并不限制你的代码结构。但是，它规定了一些需要遵守的规则：
1、应用层级的状态应该集中到单个store对象中。
2、提交mutation是更改状态的唯一方法，并且这个过程是同步的。
3、异步逻辑都应该封装到action里面。

# 2.项目文件目录显示如下

![img](https://images2018.cnblogs.com/blog/854419/201807/854419-20180717145312224-1516196875.png)

# 3.项目文件解析

## 3.1 build文件

build：文件夹下存放webpack的一些配置，webpack是前端网站的一种项目编译、运行、打包工具。
build.js：是我们完成项目之后需要运行的， 可以将我们的项目文件打包成静态文件，存放在项目根目录的dist文件夹中（现在目录里还没有这个文件夹，build的时候会自动生成）。
check-versions.js：主要是检查一些所依赖的工具的版本是否适用，如nodejs、npm，若版本太低则会提示出来。
logo.png：存放的是vuelogo图片。
utils.js：提供工具函数，包括生成处理各种样式语言的loader，获取资源文件存放路径的工具函数。
vue-loader.conf.js： 引入了utils.js ，应该是用于切换开发模式和生产模式的文件，以便于用不同模式来解析loader。
webpack.base.conf.js：此配置文件是vue开发环境的wepack相关配置文件，主要用来处理各种文件的配置。
webpack.dev.conf.js：在webpack.base.conf的基础上增加完善了开发环境下面的配置。
webpack.prod.conf.js：构建的时候用到的webpack配置来自webpack.prod.conf.js，该配置同样是在webpack.base.conf基础上的进一步完善。

文件示例如下所示：

![img](https://images2018.cnblogs.com/blog/854419/201807/854419-20180717145827110-1167537022.png)

## 3.2 config文件

config：文件包含webpack环境配置文件。
index.js：描述了开发和构建两种环境下的配置。
dev.env.js，prod.env.js，test.env.js：三个文件简单设置了环境变量。

文件示例如下所示：

![img](https://images2018.cnblogs.com/blog/854419/201807/854419-20180717150814875-1765470727.png)

## 3.3 node_moules文件

node_moules：存放的是npm加载的项目依赖模块 ，以后要添加依赖也是放在这个里面。

文件示例如下所示：

![img](https://images2018.cnblogs.com/blog/854419/201807/854419-20180717150936968-182378864.png)

## 3.4 src文件

src：放置组件和入口文件。
assets：主要存放一些静态图片资源的目录。
components：这里存放的是开发需要的的各种组件，各个组件联系在一起组成一个完整的项目。
router：存放了项目路由文件。
App.vue：是项目主组件，也是项目所有组件和路由的出口，之后它会被渲染到项目根目录的 index.html 中显示出来，我们可以在这里写一些适合全局的css样式。
main.js：入口文件，引入了vue模块和app.vue组件以及路由router，我们需要在全局使用的一些东西也可以定义在这里面。

文件示例如下所示：

![img](https://images2018.cnblogs.com/blog/854419/201807/854419-20180717151225194-19537250.png)

## 3.5 static文件

static:存放的是项目的静态文件，用于存放需要使用的一些外部的js、css文件，需要使用的时候从这里引到文件内。

文件示例如下所示：

![img](https://images2018.cnblogs.com/blog/854419/201807/854419-20180717151340162-1224391164.png)

## 3.5 test文件

test：初始测试目录。
unit：单元测试，可以为每个组件编写单元测试，放在 test/unit/specs 目录下面，单元测试用例的目录结构建议和测试的文件保持一致（相对于src），每个测试用例文件名以 .spec.js结尾。 执行 npm run unit 时会遍历所有的 spec.js 文件，产出测试报告在 test/unit/coverage 目录。
e2e：e2e或者端到端（end-to-end）或者UI测试是一种测试方法，它用来测试一个应用从头到尾的流程是否和设计时候所想的一样。

文件示例如下所示：

![img](https://images2018.cnblogs.com/blog/854419/201807/854419-20180717152022625-1097609527.png)

## 3.6 其他文件

.babelrc：ES6语法编译配置。
.editorconfig：代码编写规格。
.eslintignore：项目的根目录中创建文件来告诉ESLint忽略特定的文件和目录，该文件是纯文本文件。
.eslintrc.js：eslint的配置文件，eslint是用来管理和检测js代码风格的工具，可以和编辑器搭配使用，如vscode的eslint插件。当有不符合配置文件内容的代码出现就会报错或者警告。
.gitignore：忽略的文件。
.postcssrc.js：兼容选项（如果已经安装postcss，需要一大堆loader配置，这时项目根目录会生成“.postcssrc.js”文件）。
index.html：项目文件入口。
package.json：项目及工具的依赖配置文件。
README.md：项目说明。

文件示例如下所示：

![img](https://images2018.cnblogs.com/blog/854419/201807/854419-20180717152238613-1133785528.png)
