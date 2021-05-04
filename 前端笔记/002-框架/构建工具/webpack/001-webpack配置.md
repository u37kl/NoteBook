> 1. yarn add -D webpack-dev-server，开发环境中运行项目的服务器，使用该包时，在开发环境下打包完成后自动开启服务器运行。这个包在运行项目时为`live reload`实时刷新，即修改源代码后它会自动编译然后刷新浏览器，非常方便。

package.json中的`dependencies`和`devDependencies`属性，如何区分安装包时，应该放在`devDependencies`中还是放在另一个中？

1. `devDependencies`：开发依赖，不要理解为字面意思，应该是编译时以来，也就是说在项目进行编译打包时所依赖的包，例如语法检查、webpack、js代码混淆、js语法转换ES6转ES5等。
2. `dependencies`：生产依赖，不要理解为上线时依赖，而是理解为运行依赖，项目在开发时运行或者上线时在服务器运行时，所依赖的包，例如vue框架、axios库、dayjs这些业务逻辑有关的库。



package.json中的`dependencies`和`devDependencies`属性有啥用，它是依赖声明，你可以将这两个属性删除，项目打包和项目的运行仍然正常，为啥？

这两个属性是这个项目的依赖声明，用在从git库中克隆项目时，此时克隆下来的项目没有依赖库，执行`yarn install`命令，根据package.json的这两个属性去npm服务器中下载这些依赖库。只是用在这种情况。也就是说用在新同事进入项目组的这种情况。

注意一定要维护好这个两个属性，否则会出现新同事根据package.json的这两个属性下载依赖包后一运行出现依赖包存在的现象，出现这个问题由两种情况

1. 下载包时，由于网络情况丢包了。
2. 项目依赖的包没有添加到依赖声明(即添加到这两个属性中的一个)，导致没有下载下来。`yarn install根据package.json的这两个属性去npm服务器中下载，如果没有配置在该json文件中，那就会导致项目运行不起来。



## serve包

> 当项目在上线前，开发人员需要验证一下，将其安装到服务器上跑一跑，因此npm有一个`serve`包，可以在本地快速搭建一个本地服务器，方便开发人员查看项目的效果。
>
> `npm -i serve -g`，全局安装。



## 使用的第三方包

> 编译项目中的文件：
>
> 1. babel-loader：有两个作用，将jsx语法编译成ES6，将ES6编译成ES6。
> 2. ：vue-loader：将vue文件编译成js代码，这个vue-loader包含了三个东西。
>    1. 包含vue-loader
>    2. 包含一个插件
>    3. 包含了vue-style-loader
> 3. css-loader：由于使用webpack打包项目，webpack只能处理js与json文件，因此对于其他文件需要转成模块文件才能让webpack处理，css-loader就是为了将css文件中的内容转成对象来让webpack处理。
> 4. url-loader与file-loader：url-loader是对file-loader的封装，用来处理项目中的其他不需要编译的资源，例如字体文件、图片、音视频，将其放到打包目录下，并调整资源的路径，保证可以正确的被引用。
> 5. style-loader：将引入到模块中的css内容，放到页面的style标签下。