#### 1.Node.js
  - Node.js简称Node
  
  - Node简单来说可以使JS在服务器端运行，而不仅仅局限在浏览器中
    我们也可以通过JavaScript来开发WEB服务器。
    
  - Node是一个ECMAScript标准的实现，所有的ECMASCript标准中定义的内容在Node中都可以直接使用：
    String Boolean 。。。。 Object 。。。 Function RegExp Date Math 
    但是在Node不能使用BOM和DOM中的对象
    
  - Node底层是通过chrome的v8引擎来实现的

  - Node的功能已经不局限在服务器，他已经形成了一个生态环境并且建立了一个良好的开源的社区
  
  - Node特点：
    1. 非阻塞，异步I/O
    2. 基于事件和回调函数
    3. 单线程（主线程单线程），后台（I/O线程池）
    4. 跨平台
    
#### 2.模块化（Module）
  - 模块化就是讲一个完整的程序分割成一个一个模块的结构，以降低代码之间的耦合，方便代码的复用
  - 原生的js中没有模块化的机制，所以在Node中引入CommonJS规范，通过该规范在node中成功的引入模块化
    CommonJS 主要用于node中的模块化规范
    AMD CMD 浏览器端的模块化规范
  - 模块化使用
    1.模块的引用
      - 使用 require() 引入外部模块
      - 例子：
        var fs = require("fs");
        var math = require("math");
        var m = require("./math");
        
    2.模块的定义
      - 在node中一个js文件就是一个模块，
        但是在js文件中编写的代码，并不是直接写在全局中的
        所以默认情况下模块中的代码对于外部都是不可见
        
      - 暴露方法和属性
        - 使用 exports
          - 可以将需要暴露的方法和属性设置exports的属性，即可暴露这些内容
          
        - module.exports
          - 也可以通过该属性来暴露内容，并且可以直接为module.exports进行赋值
    
    3.模块的标识
      - 模块的标识就是模块的名字
        - 对于核心模块，可以直接使用模块的名字，比如：fs express  mongoose
        - 对于文件模块（自定义模块），需要通过文件的路径来引入 比如: ./hello
        
#### 3.包(package)
  - 一个模块只是一个单一的功能，将多个模块组合为一个更强大的功能便形成了包
    - 包的规范同样也是有CommonJS来规定：
    1.包结构
      bin 用于存放可执行二进制文件的目录
      lib 用于存放JavaScript代码的目录
      doc 用于存放文档的目录
      test 用于存放单元测试用例的代码

    2.包描述文件
      - 用来描述包的功能、名字、作者、版本、依赖等内容
      - 包描述文件的名字：package.json
        - 常见的属性：
          name 包名
          version 包的版本
          main 包的主要js文件（程序的入口）
          repository 仓库的地址（github的地址）
          dependencies 包的依赖
          ....
          
#### 4.NPM(Node Package Manager)
  - Node的包管理器
  - 提供了对Node的包的管理，相当于360中的软件管家
    它负责node中的包的上传、下载、搜索等功能
  - 按照Node时，NPM会自动安装	
  - npm的指令：
    npm -v 查看版本
    npm search 包名 搜索指定的包
    npm install 安装当前模块所需要的依赖模块
    npm install 包名 安装指定的包
      - 默认情况下会直接将包安装到当前目录下的node_module文件夹下
    npm install 包名 -save 在当前模块中安装指定包，并将其添加到依赖中
    
  - npm install 包名 -g 全局安装包
      将包安装到系统中C:\Users\lilichao\AppData\Roaming\npm一般全局安装在编写服务器的时候用的不多，全局安装的模块多是工具模块
  - npm init 初始化一个新的模块（创建一个package.json）