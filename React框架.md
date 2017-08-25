### 1. ReactJS是什么？	
- Facebook开源的JS库
- 一个用于动态构建用户页面的JS库
- React的特点
	- Declarative(声明式编码)
	- Component—Based（组件化编码）
	- 支持客户端和服务器端的渲染
	- 高效
	- 单向数据流
	- 官网：http://facebook.github.io/react/
	- Github地址：https://github.com/facebook/react
### 2. React高效的原因
- 虚拟（virtual）DOM，不总是直接操作DOM
- 高效的DOM Diff算法，最小化页面重新绘制
### 3. 模块与组件
- 模块
	- 理解：向外提供特定功能的js程序
	- 作用：简化js的编写，提高运行效率
- 组件
	- 理解：用来实现特定功能效果的代码集合（html/css/js）
	- 作用：提高复用性，简化项目编码，提高运行效率
### 4. 模块化与组件化
- 模块化
	- 当应用的js文件都是以模块来编写的，这个应用就是一个模块化应用
- 组件化
	- 当应用是以多组件的方式实现的应用，这个应用就是一个组件化应用
### 5. React相关JS库
- react.js：React的核心库
- react-dom.js：提供操作DOM的扩展库
- babel.js：解析JSX和ES6代码转换为JS和ES5代码的库
### 6. 在页面中导入React的JS库
- 先导入react.j
```
<script type="text/javascript" src="../js/react.js"></script>
<script type="text/javascript" src="../js/react-dom.js"></script>
<script type="text/javascript" src="../js/babel.min.js"></script>
```
### 7. 编码
- 示例
```
<div id="example"></div>
<script type="text/babel"> //必须用babel
    ReactDOM.render(<h1>Hello, React!</h1>, document.getElementById('example'));
</script>
```
### 8. 自定义组件
- 定义组件

```
//方式1: 工厂(无状态)函数(最简洁, 推荐使用)
function MyComponent1() {
  return <h1>自定义组件标题11111</h1>
}
//方式2: ES6类语法(复杂组件, 推荐使用)
class MyComponent3 extends React.Component {
  render () {
    return <h1>自定义组件标题33333</h1>
  }
}
//方式3: ES5老语法(不推荐使用了)
var MyComponent2 = React.createClass({
  render () {
    return <h1>自定义组件标题22222</h1>
  }
})
```
- 渲染组件标签（示例）

```
ReactDOM.render(<MyComponent/>, document.getElementById('example'));
```
- 注意：
    - 返回的组件类首字母必须大写
    - 虚拟DOM元素必须只有一个根元素
    - 虚拟DOM元素必须有结束标签
- ReactDOM.render()渲染组件标签的具体流程
    - React内部会创建组件实例对象
    - 得到包含的虚拟DOM并解析为真实DOM
    - 插入到指定的页面元素内部
### 9. 组件的属性
- props属性
    - 每个组件对象都会有props（properties的简写）属性
    - 组件标签的的所有属性都保存在props中
    - 内部读取属性值：this.props.propertyName
    - 作用：通过标签属性从组建外向组建内传递数据（只读）
    - 对props中的属性值进行类型和必要性限制
    ```
    //示例
    Person.propTypes = {
      name: React.PropTypes.string.isRequired,
      age: React.PropTypes.number.isRequired
    }
    ```
    - 扩展属性将对象的所有属性通过props传递
    
    ```
    //示例
    <Person {...person}/>
    ```
    - 设置默认属性值
    
    ```
    //示例
    Person.defaultProps = {
      name: 'Mary'
    }
    ```
    - 组件类的构造函数
    
    ```
    //示例
    constructor (props) {
        super(props)
        console.log(props)//查看所有属性
    }
    ```
    - 案例：
    
    ```
    class Person extends React.Component {
    constructor (props) {
        super(props)
        console.log(props) // 查看所有属性
      }
    render(){
      return (
        <ul>
          <li>{this.props.name}</li>
          <li>{this.props.age}</li>
          <li>{this.props.sex}</li>
      </ul>
      )
    }
  }
  Person.defaultProps ={
    age:18,
    sex:"male"
  }
  Person.propTypes = {
    name:React.PropTypes.string.isRequired
  }
  let person = {
    name:"zhangsan"
  }
  ReactDOM.render(<Person {...person}/>,document.getElementById('example'))
    ```
- refs属性
    - 组件内的标签都可以定义ref属性来标识自己
    - 在组件中可以通过this.refs.refname来得到真实的DOM对象
    - 作用：用于操作指定的red属性的DOM元素对象
    
    ```
    //示例
    <input ref="username"/>
    this.refs.username//返回input对象
    ```
- 事件的处理
    - 通过onXxx属性指定组件的事件处理函数，注意大小写
        - React使用的是自定义的（合成）事件，而不是使用的DOM事件
        - React中的事件是通过委托方式处理的（委托给组件最外层的元素）
    - 通过event.target得到发生事件的DOM元素对象
    
    ```
    //示例
    <input onFocus={this.handleClick}/>
    handleFocus(event) {
      event.target  //返回input对象
    }
    
    ```
- 注意
    - 组件内置的方法中的this为组件对象
    - 在组件中自定义的方法中的this为null
        - 强制绑定this（bind()方法）
        - 箭头函数(ES6模块化编码时才能使用)
- state属性
    - 组件被称为"状态机", 通过更新组件的状态值来更新对应的页面显示(重新渲染)
    - 初始化状态:
    
    ```
    constructor (props) {
        super(props)
        this.state = {
          stateProp1 : value1,
          stateProp2 : value2
        }
      }
    ```
    - 读取某个状态值
    
    ```
    this.state.statePropertyName
    ```
    - 更新状态---->组件界面更新
    
    ```
    this.setState({
        stateProp1 : value1,
        stateProp2 : value2
      })
    ```
- 组件化案例

```
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>07_state_demo</title>
</head>
<body>
  <div>
    <h2>Simple TODO List</h2>
    <input type="text">
    <button>Add #4</button>
    <ul>
      <li>吃饭</li>
      <li>睡觉</li>
      <li>打豆豆</li>
    </ul>
  </div>
  <hr>

  <div id="example">在此实现页面效果和功能</div>

  <script src="../js/react.js"></script>
  <script src="../js/react-dom.js"></script>
  <script src="../js/babel.min.js"></script>
  <script type="text/babel">
    /*
    功能: 组件化实现此功能
      1. 界面显示如上所示
      2. 输入文本, 点击按钮显示到下面列表的首位, 并清除输入的文本
     */
    /*
    分解组件:
      列表: TodoList
        props: todos
      应用: App
        state: todos, inputTodo
     */
    /*
    问题: 完善功能
      1). 如果没有输入或输入的只是空格, 不添加到列表
      2). 如果输入的文本两边有空格, 去掉空格后添加
    */

    class App extends React.Component{
      constructor(props){
        super(props)

        this.state = {
          todoList:['吃饭','睡觉','打豆豆'],
          addValue:''
        }
        this.addTodo = this.addTodo.bind(this)
      }

      addTodo () {
        if(this.refs.msg.value.trim() == ''){
          return
        }
        let addValue = this.refs.msg.value.trim()
        this.state.todoList.unshift(addValue)
        this.setState({
          addValue,
        })
        this.refs.msg.value = ''
      }
      
      render(){
        return (
          <div>
            <h2>Simple TODO List</h2>
            <input type="text" ref="msg"/>
            <button onClick={this.addTodo}>Add #{this.state.todoList.length}</button>
            <TodoList todolist={this.state.todoList}/>
          </div>
        )
      }
    }

    class TodoList extends React.Component {
      render () {
      console.log(this.props.todolist)
        return (
            <ul>
              {this.props.todolist.map((item, index)=><li key={index}>{item}</li>)}
            </ul>
          )
      }
    }
    ReactDOM.render(<App />,document.getElementById('example'))
  </script>
</body>
</html>


```











    

