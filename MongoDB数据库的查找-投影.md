## 投影
在做查询操作时默认情况下会将文档中的所有字段返回，此时可以通过手动指定要返回的字段，即投影

#### 1. 数据库中直接查找--投影


```
db.createCollection("students")
db.students.insert(
   {
     name:"孙悟空",
     age:18,
     address:"花果山"，
     gender:"male"
   }
)
//查询时的第二个参数可以用来指定投影 
//0代表不返回此内容，1代表返回此内容
db.students.find({},{_id:0 , name:1 , age:1});
/*
返回结果：
{
    "name" : "孙悟空",
    "age" : 18.0
}
*/

db.students.find({},{_id:0 , name:0 , age:0});
/*
返回结果：
{
    "address" : "花果山",
    "gender" : "male"
}
*/

db.students.find({},{_id:0 , name:1 , age:0});//会报错
```

#### 2. node.js中数据库的查找--投影


```
//加载mongoose
var mongoose = require("mongoose");
//连接到数据库
mongoose.connect("mongodb://127.0.0.1:27017/admin",{useMongoClient: true});
mongoose.connection.once("open",function () {
  console.log("已经连接到MongoDB数据库~~~");
});

//获取mongoose中Schema对象
var Schema = mongoose.Schema;
//设置Schema规范
var stuSchema = new Schema({
  name:{
    type:String,
    unique:true
  },
  age:Number,
  gender:{
    type:String,
    default:"male"
  },
  address:String
});
//创建studentModel
var StudentModel = mongoose.model("student" , stuSchema);
//创建数据
StudentModel.create({
  name:"孙悟空",
  age:"18",
  gender:"m",
  address:"花果山"
},function (err) {
  if(!err){
    console.log("数据插入成功");
  }
});
```


#### 3. MongoDB中查找数据--投影

```
//方式1：在find()方法的第二个参数指定投影，方式和在数据库中的一样
StudentModel.find({},{_id:0,name:1,age;1},function(err , doc){
  if(!err){
    console.log(doc);
  }
});//输出结果：[ { name: '孙悟空', age: 18 } ]
```

```
//方式2：也是在find()方法的第二个参数指定投影，方式是加入一个字符串，表示要显示的项，中间用空格隔开，在指定的项前加-号表示不显示此项
StudentModel.find({},"-_id name age",function(err , doc){
  if(!err){
    console.log(doc);
  }
});//输出结果：[ { name: '孙悟空', age: 18 } ]
```

