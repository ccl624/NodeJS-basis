## Schema（模式对象

#### 1.加载mongoose


```
var mongoose = require("mongoose");
mongoose.connect("mongodb://127.0.0.1:27017/admin",{useMongoClient: true});
mongoose.connection.once("open",function () {
  console.log("已经连接到MongoDB数据库~~~");
});//具体参见 http://localhost:8080/wordpress/?p=35
```

#### 2. 获取mongoose中的Schema对象


```
/*
  Schema（模式对象）
    - 原生的MongoDB对文档没有任何约束，集合中爱存什么就存什么，
      也就是说集合中可以存储这样的文档{name:"admin",pwd:"123123"}
      也可以存储这样的文档{name:true,pwd:[123,345]}
      但是如果真的这么存储文档将会导致开发起来非常的麻烦，系统不好维护

    - 为了解决这个问题在Mongoose中提供了Schmea对象，
      通过Schema可以对一个集合中的文档进行约束限制，可以规定文档中必须具有某个字段，
        也可以规定字段的具体的类型

    - 要使用Mongoose必须先创建一个模式对象
*/
var Schema = mongoose.Schema;
```

#### 3. 创建Schema对象


```
var stuSchema = new Schema(
    {
  name:String,
  age:Number,
  gender:{
    type:String,
    default:"male"//默认值
  },
  address:String
    }
);
```

## Model(模型)

Model(模型)
   - 模型就相当于数据库中的集合，通过模型可以对集合中的数据做增删改查的操作
   - 需要通过Schema对象来获取Model对象
#### 1.根据Schema对象创建模型对象


```
/*
   mongoose.model() 用来根据Schema来创建模型对象，需要两个参数
   第一个参数数据库中集合的名字  第二个参数约束集合文档的Schema
   mongoose会自动将集合名变成复数
*/
var StudentModel = mongoose.model("student" , stuSchema);
```

#### 2.通过模型对象操作数据库--增删改查

##### 1. 创建文档并将其插入到数据库中
```
StudentModel.create({
    name:"孙悟空",
    age:18,//此处18是number类型，根据其约束集合文档的Schema，在添加到数据库的过程中会默认将其转化为Schema中设置的String类型
    address:"花果山"
},function(err,doc){ //doc返回的是插入的数据对象
   if(!err){
     console.log("数据已经插入到数据库中~~~")
   }
});//此处并没有加入gender，但是根据其约束集合文档的Schema会默认添加默认值gender:"male"
```
##### 2. 查询文档
```
StudentModel.find({},function(err,docs){
    if(!err){
       console.log(docs);//docs返回的是查询后的对象数组，find()查询后的结果无论是否只有一个都是数组
    }
});
/*
   参数find(conditions, projection, options, callback)
      conditions:查询的条件
      projection：镜像条件--eg: {_id:0, name:1, age:1}或者是字符串"-_id, name, age"(此处的_id不能写成id),当无此条件时设为null
      options:查询限制--eg: {limit:10, skip:10}
      callback:回调函数
*/
StudentModel.find({},function(err,docs){
    if(!err){
       console.log(docs);//docs返回的是查询后的对象数组，find()查询后的结果无论是否只有一个都是数组
    }
});
//findOne()，参数和find()的一样
StudentModel.findOne({},function(err,docs){
    if(!err){
       console.log(docs);//docs返回的是查询后的对象,findOne()查询后的结果是数据库中第一个匹配的对象，只有一个，不是数组
    }
});
//findById(id, projection, options, callback),参数第一个是id其他和find()的参数一样
StudentModel.findOne("596efc7f69eb910a2c17c3ea",function(err,docs){
    if(!err){
       console.log(docs);//docs返回的是查询后的对象,查询后的结果是数据库的对象，只有一个，不是数组
    }
});
//查询匹配文档的数量count(),count(conditions, callback)
StudentModel.count({},function (err, count) {
  if(!err){
    console.log(count);
  }
})
```
##### 3. 修改文档
```
//update(conditions, doc, options, callback)，除了回调函数，结构与直接在数据库中查找的方式一致
StudentModel.update({name:"sunwukong"},{$set:{age:30}},function (err) {
    if(!err){
        //console.log("修改成功");
        console.log(arguments);//参数有{ '0': null, '1': { n: 0, nModified: 0, ok: 1 } }
    }
});
```
##### 4. 删除文档
```
/*
  remove(conditions, callback)
 	deleteOne(conditions, callback)
  deleteMany(conditions, callback)
*/
/*
        在开发中删除语句要慎用，甚至可以不用
 	一般如果需要做删除的操作都会在文档中添加一个新的字段 比如可以添加一个 isDel:false
 	如果要删除一个文档，可以将其的isDel设置为true即可
*/
//这三种方法调用结构大体一致，除了callback函数结构与直接在数据库中查找的方式一致，下面只列举了一个例子
StudentModel.deleteOne({name:"zhubajie"},function (err) {
  if(!err){
    console.log("删除成功");
  }
});
```

