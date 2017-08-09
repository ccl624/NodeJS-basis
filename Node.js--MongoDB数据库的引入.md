#### 1.安装mongoose模块

```
//在终端中进入目标路径下
npm install mongoose --save
```

#### 2.在项目中引入mongoose


```
var mongoose = require("mongoose")
```

#### 3.连接至MongoDB服务器


```
//连接数据库
//路径：mongodb://ip地址:端口号/数据库名字
mongoose.connect("mongodb://127.0.0.1:27017/admin");//27017是服务器地址端口号如果是默认的可以不写，admin是目标数据库
```

#### 4.监听数据库的连接状态


```
//在mongoose中具有一个属性 connection（数据库连接）
//该对象表示当前的数据库连接，通过它可以来监听数据库的连接状态
mongoose.connection.once("open",function () {
  console.log("已经连接到MongoDB数据库~~~");
  mongoose.disconnect();//此处只为测试，断开数据库连接(一般不使用)
});
```

#### 5.监听数据库断开


```
mongoose.connection.once("close",function () {
  console.log("已经和数据库断开连接~~~");
});
```
