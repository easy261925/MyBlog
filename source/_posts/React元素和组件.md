---
title: React元素和组件
date: 2018-01-31 09:03:54
tags: React
categories: React
---
![](http://p2oiag4fy.bkt.clouddn.com/React.jpeg)
# 一 、属性和状态

### (一)什么是属性？

props=properties 

#### 1、属性的含义和用法

含义 ：一个事物的性质与关系，属性往往是与生俱来的、无法自己改变的。

3种用法 ：

(1)键值对

传入一个字符串：

```js
'Hello,World!' 或 {'Hello,World!'}
```

传入一个数组：

```js
{[Array1,Array2,Array3]}
```

传入一个变量：

```js
{variable}
```

(2)展开对象形式

```js
var props = {
	one:"123",
	two:321
	}
<HelloWorld {...props}/>
```

React提供展开语法…,使用…加对象,react就会把对象中的属性和值，当成是属性的赋值。

(3)setProps形式：

通过组件更新属性，不能在组件内部中修改属性的，因为会违背组件设计原则（尽量避免）

```js
<script type="text/babel">
    var HelloWorld = React.createClass({
        render: function(){
            return <p>Hello, {this.props.name}</p>
        }
    });
    var instance = ReactDOM.render(<HelloWorld></HelloWorld>, document.querySelector("#example"));
    instance.setProps({name: "William"});
</script>
```

setProps接收的参数是一个对象，但是react不推荐改变组件的属性,可以通过父组件向子组件传入的方式。

### (二)什么是状态？

state

#### 1、状态的含义和用法

含义 ：事物所处的状况。状态是由事物自行处理、不断变化的，是事物的私有属性。 

用法：

getInitialState:初始化每个实例特有的状态 
setState:更新组件状态 
使用setState——启用diff算法——有变化，更新DOM

```js
	//使用状态 state，实现时钟
    class Time extends React.Component{
        //构造器函数（初始化函数）：初始化
        constructor(props) {
            super(props);
            //初始化状态
            this.state={
                date:new Date()
            }
            //修改状态:每隔一秒，修改状态
            setInterval(()=>{
                console.log('定时器执行');
                this.setState({
                    date:new Date()
                })
            },1000);
        }
        render(){
            console.log('时间',this.state.date);
            return(
                //使用
                <p>当前时间:  {this.state.date.toLocaleTimeString()}</p>
            );
        }
    }
    ReactDOM.render(
        <Time />,
        document.getElementById('root')
        );
```

### (三)属性和状态对比 

属性和状态作为组件之间数据流动的途径，非常容易混用，下面对属性和状态进行对比，介绍两者的相同点、不同点以及使用方法。

#### 1、相同点

- 都是纯JS对象
- 都会触发render更新
- 都具有确定性

#### 2、对比

| -            | 属性( props) | 状态(state) |
| ------------ | :--------: | :-------: |
| 能否从父组件获取初始值  |     √      |     ×     |
| 能否由父组件修改     |     √      |     ×     |
| 能否在组件内部设置默认值 |     √      |     √     |
| 能否在组件内部修改    |     ×      |     √     |
| 能否设置子组件的初始值  |     √      |     ×     |
| 能否修改子组件的值    |     √      |     ×     |

状态只和组件本身相关，组件不能修改属性

#### 3、区分

- 状态和属性都会触发render更新，都是纯JS对象

- 状态：是和自己相关的，既不受父组件也不受子组件影响

- 属性：本身是不能自己去修改的，只能从父组件获取属性，父组件也能修改它的属性。

  根本的区别：组件在运行时需要去修改维护的就是状态

  ​

# 二、元素和组件

### (一)、什么是元素？ 

元素用来描述你在屏幕上看到的内容

与浏览器的 DOM 元素不同，React 当中的元素事实上是普通的对象，React DOM 可以确保 浏览器 DOM 的数据内容与 React 元素保持一致。

React 元素（React element），它是 React 中最小基本单位，我们可以使用 JSX 语法轻松地创建一个 React 元素:

```js
const element = <div className="element">I'm element</div>
```

React 元素不是真实的 DOM 元素，它仅仅是 js 的普通对象（plain objects），所以也没办法直接调用 DOM 原生的 API。上面的 JSX 转译后的对象大概是这样的：

```json
{
    _context: Object,
    _owner: null,
    key: null,
    props: {
    className: 'element'，
    children: 'I'm element'
  },
    ref: null,
    type: "div"
}
```

只有在这个元素渲染被完成后，才能通过选择器的方式获取它对应的 DOM 元素。不过，按照 React 有限状态机的设计思想，应该使用状态和属性来表述组件，要尽量避免 DOM 操作，即便要进行 DOM 操作，也应该使用 React 提供的接口`ref`和`getDOMNode()`。一般使用 React 提供的接口就足以应付需要 DOM 操作的场景了，因此像 jQuery 强大的选择器在 React 中几乎没有用武之地了。

除了使用 JSX 语法，我们还可以使用 `React.createElement()` 和 `React.cloneElement()` 来构建 React 元素。

#### 1、React.createElement() 创建元素

JSX 语法就是用`React.createElement()`来构建 React 元素的。它接受三个参数，第一个参数可以是一个标签名。如`div`、`span`，或者 React 组件。第二个参数为传入的属性。第三个以及之后的参数，皆作为组件的子组件。

```js
React.createElement(
    type,
    [props],
    [...children]
)
```

该方法创建并返回一个`ReactElement`对象，其参数如下：

- `type`，可以是一个`HTML标签`或是一个`React组件`（`ReactClass`）
- `props`，可选参数，表示对象的属性
- `children`，第三个参数及其后的参数都会被认为是元素的子元素
- 返回值：`ReactElement`对象

示例，创建一个如下结构的组件：

```html
<div className="myClass">
  <h2>itbilu.com</h2><hr/>
</div>

```

使用`createElement()`方法操作如下：

```js
ReactDOM.render(
  React.createElement('div', {className:'myClass'},  
    React.createElement('h2', null, 'itbilu.com'),
    React.createElement('hr')
  ),
  document.getElementById('example')
);

// itbilu.com
```



#### 2、React.cloneElement() 元素克隆

`React.cloneElement()`与`React.createElement()`相似，不同的是它传入的第一个参数是一个 React 元素，而不是标签名或组件。新添加的属性会并入原有的属性，传入到返回的新元素中，而旧的子元素将被替换。

```js
React.cloneElement(
  element,
  [props],
  [...children]
)
```

该方法会从已有的`ReactElement`中复制，并返回一个新的`ReactElement`对象，其参数如下：

- `element`，一个`React元素`（`ReactElement`）
- `props`，可选参数，表示对象的属性
- `children`，第三个参数及其后的参数都会被认为是元素的子元素
- 返回值：`ReactElement`对象

示例，已有如下元素：

```js
React.createElement('div');
```

使用`cloneElement()`复制这个元素，并最终生前面示例中的HTML。复制方法如下:

```js
var div = React.createElement('div');

ReactDOM.render(
  React.cloneElement(div, {className:'myClass'},  
    React.createElement('h2', null, 'itbilu.com'),
    React.createElement('hr')
  ),
  document.getElementById('example')
);
```

### (二)、什么是组件？

组件可以将UI切分成一些的独立的、可复用的部件，这样你就只需专注于构建每一个单独的部件。

组件从概念上看就像是函数，它可以接收任意的输入值（称之为“props”），并返回一个需要在页面上展示的React元素。

**特点 **：

1. 组件就是函数
2. 组件编写时，首字母大写
3. 组件可以嵌套组合使用 ，遵循是W3C的规范

定义一个组件最简单的方式是使用JavaScript函数：

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

#### 1、函数定义组件(写法一)

```js
function Welcome(props) {
        return <h1>Welcome,{props.name}</h1>
    }
```

#### 2、类定义组件(写法二)

```js
class Member extends React.Component{
        render(){
            return <h1>Welcome,{this.props.name}</h1>
        }
    };
```

#### 3、组合组件

```js
function Welcome(props) {
        return <h1>Hello,{props.name}</h1>
    }
    function App() {
        return(
            // 组件的返回值只能有一个根元素。
            // 这也是我们要用一个<div>来包裹所有<Welcome />元素的原因。
            <div>
                <Welcome name='Sara' />
                <Welcome name='Cahal' />
                <Welcome name='Edite' />
            </div>
        )
    }
    ReactDOM.render(
        <App />,
        document.getElementById('root')
    );
```

#### 4、提取组件

**意义** :

1. 可重用性
2. 可读性
3. 可维护性
4. 便于调试与测试

```js
        function Avatar(props) {
            return(
                <img
                    className='Avatar' 
                    src={props.user.avator} 
                    alt={props.user.name} 
                />
            );
        }
        function UserInfo(props) {
            return(
                <div className="UserInfo">
                    <Avatar user={props.user} />
                    <div className="UserInfoName">
                        {props.user.name}
                    </div>
                </div>
            );
        }
        function Comment(props) { 
            return(
                <div className="Comment">
                    <UserInfo user={props.user} /> 
                    <div className="CommentText">
                        {props.text}
                    </div>
                    <div className="CommentDate">
                        {props.date}
                    </div>
                </div>
            ) 
        } 
        //提取Avatar组件 
        ReactDOM.render(
            <h1>Hello,World</h1>, 
            document.getElementById('root') 
        );
```

### (三)元素和组件的区别

组件是由元素构成的。元素数据结构是普通对象，而组件数据结构是类或纯函数。

打个不恰当的比喻，React 组件是`MyComponent`，React 元素就是`<MyComponent />`。

# 三、组件的类型及其区别

React推出后，出于不同的原因先后出现三种定义react组件的方式，殊途同归；具体的三种方式：

1. 函数式定义的`无状态组件`
2. es5原生方式`React.createClass`定义的组件
3. es6形式的`extends React.Component`定义的组件

虽然有三种方式可以定义react的组件，那么这三种定义组件方式有什么不同呢？或者说为什么会出现对应的定义方式呢？下面就简单介绍一下。

### (一)无状态函数式组件

创建[无状态函数式组件形式](https://facebook.github.io/react/blog/2015/10/07/react-v0.14.html#stateless-functional-components)是从`React 0.14`版本开始出现的。它是为了创建纯展示组件，这种组件只负责根据传入的`props`来展示，不涉及到要`state`状态的操作。具体的*无状态函数式组件*，其官方指出：

```
   在大部分React代码中，大多数组件被写成无状态的组件，通过简单组合可以构建成其他的组件等；这种通过多个简单然后合并成一个大应用的设计模式被提倡。
```

无状态函数式组件形式上表现为一个只带有一个`render`方法的组件类，通过函数形式或者ES6 arrow function的形式在创建，并且该组件是无`state`状态的。具体的创建形式如下：

```js
function HelloComponent(props, /* context */) {
  return <div>Hello {props.name}</div>
}
ReactDOM.render(<HelloComponent name="Sebastian" />, mountNode) 
```

无状态组件的创建形式使代码的可读性更好，并且减少了大量冗余的代码，精简至只有一个render方法，大大的增强了编写一个组件的便利，除此之外无状态组件还有以下几个显著的特点：

1. **组件不会被实例化，整体渲染性能得到提升**
   因为组件被精简成一个render方法的函数来实现的，由于是无状态组件，所以无状态组件就不会在有组件实例化的过程，无实例化过程也就不需要分配多余的内存，从而性能得到一定的提升。
2. **组件不能访问this对象**
   无状态组件由于没有实例化过程，所以无法访问组件this中的对象，例如：`this.ref`、`this.state`等均不能访问。若想访问就不能使用这种形式来创建组件
3. **组件无法访问生命周期的方法**
   因为无状态组件是不需要组件生命周期管理和状态管理，所以底层实现这种形式的组件时是不会实现组件的生命周期方法。所以无状态组件是不能参与组件的各个生命周期管理的。
4. **无状态组件只能访问输入的props，同样的props会得到同样的渲染结果，不会有副作用**

无状态组件被鼓励在大型项目中尽可能以简单的写法来分割原本庞大的组件，未来React也会这种面向无状态组件在譬如无意义的检查和内存分配领域进行一系列优化，所以**只要有可能，尽量使用无状态组件**。

### (二)React.createClass

```
`React.createClass`是react刚开始推荐的创建组件的方式，这是ES5的原生的JavaScript来实现的React组件，其形式如下：
```

```js
var InputControlES5 = React.createClass({
    propTypes: {//定义传入props中的属性各种类型
        initialValue: React.PropTypes.string
    },
    defaultProps: { //组件默认的props对象
        initialValue: ''
    },
    // 设置 initial state
    getInitialState: function() {//组件相关的状态对象
        return {
            text: this.props.initialValue || 'placeholder'
        };
    },
    handleChange: function(event) {
        this.setState({ //this represents react component instance
            text: event.target.value
        });
    },
    render: function() {
        return (
            <div>
                Type something:
                <input onChange={this.handleChange} value={this.state.text} />
            </div>
        );
    }
});
InputControlES6.propTypes = {
    initialValue: React.PropTypes.string
};
InputControlES6.defaultProps = {
    initialValue: ''
};
```

与无状态组件相比，`React.createClass`和后面要描述的`React.Component`都是创建有状态的组件，这些组件是要被实例化的，并且可以访问组件的生命周期方法。但是随着React的发展，`React.createClass`形式自身的问题暴露出来：

- React.createClass会自绑定函数方法（不像React.Component只绑定需要关心的函数）导致不必要的性能开销，增加代码过时的可能性。
- React.createClass的mixins不够自然、直观；React.Component形式非常适合高阶组件（Higher Order Components--HOC）,它以更直观的形式展示了比mixins更强大的功能，并且HOC是纯净的JavaScript，不用担心他们会被废弃。HOC可以参考[无状态组件(Stateless Component) 与高阶组件](http://www.jianshu.com/p/63569386befc)。

### (三)React.Component

`React.Component`是以ES6的形式来创建react的组件的，是React目前极为推荐的创建有状态组件的方式，最终会取代`React.createClass`形式；相对于 `React.createClass`可以更好实现代码复用。将上面`React.createClass`的形式改为`React.Component`形式如下：

```js
class InputControlES6 extends React.Component {
    constructor(props) {
        super(props);

        // 设置 initial state
        this.state = {
            text: props.initialValue || 'placeholder'
        };

        // ES6 类中函数必须手动绑定
        this.handleChange = this.handleChange.bind(this);
    }

    handleChange(event) {
        this.setState({
            text: event.target.value
        });
    }

    render() {
        return (
            <div>
                Type something:
                <input onChange={this.handleChange}
               value={this.state.text} />
            </div>
        );
    }
}
InputControlES6.propTypes = {
    initialValue: React.PropTypes.string
};
InputControlES6.defaultProps = {
    initialValue: ''
};
```

### (四)React.createClass与React.Component区别

根据上面展示代码中二者定义组件的语法格式不同之外，二者还有很多重要的区别，下面就描述一下二者的主要区别。

### 1、函数this自绑定

`React.createClass`创建的组件，其每一个成员函数的this都有React自动绑定，任何时候使用，直接使用`this.method`即可，函数中的`this`会被正确设置。

```js
const Contacts = React.createClass({  
  handleClick() {
    console.log(this); // React Component instance
  },
  render() {
    return (
      <div onClick={this.handleClick}></div>
    );
  }
});
```

`React.Component`创建的组件，其成员函数不会自动绑定this，需要开发者手动绑定，否则this不能获取当前组件实例对象。

```js
class Contacts extends React.Component {  
  constructor(props) {
    super(props);
  }
  handleClick() {
    console.log(this); // null
  }
  render() {
    return (
      <div onClick={this.handleClick}></div>
    );
  }
```

当然，`React.Component`有三种手动绑定方法：可以在构造函数中完成绑定，也可以在调用时使用`method.bind(this)`来完成绑定，还可以使用arrow function来绑定。拿上例的`handleClick`函数来说，其绑定可以有：

```js
    constructor(props) {
       super(props);
       this.handleClick = this.handleClick.bind(this); //构造函数中绑定
  }
```

```js
    <div onClick={this.handleClick.bind(this)}></div> //使用bind来绑定
```

```js
    <div onClick={()=>this.handleClick()}></div> //使用arrow function来绑定
```

### 2、组件属性类型propTypes及其默认props属性defaultProps配置不同

`React.createClass`在创建组件时，有关组件props的属性类型及组件默认的属性会作为**组件实例的属性**来配置，其中defaultProps是使用`getDefaultProps`的方法来获取默认组件属性的

```
const TodoItem = React.createClass({
    propTypes: { // as an object
        name: React.PropTypes.string
    },
    getDefaultProps(){   // return a object
        return {
            name: ''    
        }
    }
    render(){
        return <div></div>
    }
})
```

`React.Component`在创建组件时配置这两个对应信息时，他们是作为**组件类的属性**，不是组件实例的属性，也就是所谓的**类的静态属性**来配置的。对应上面配置如下：

```
class TodoItem extends React.Component {
    static propTypes = {//类的静态属性
        name: React.PropTypes.string
    };

    static defaultProps = {//类的静态属性
        name: ''
    };

    ...
}
```

### 3、组件初始状态state的配置不同

`React.createClass`创建的组件，其状态state是通过`getInitialState`方法来配置组件相关的状态；
`React.Component`创建的组件，其状态state是在`constructor`中像初始化组件属性一样声明的。

```
const TodoItem = React.createClass({
    // return an object
    getInitialState(){ 
        return {
            isEditing: false
        }
    }
    render(){
        return <div></div>
    }
})
```

```
class TodoItem extends React.Component{
    constructor(props){
        super(props);
        this.state = { // define this.state in constructor
            isEditing: false
        } 
    }
    render(){
        return <div></div>
    }
}
```

### 4、Mixins的支持不同

[`Mixins`](https://facebook.github.io/react/docs/reusable-components-zh-CN.html#mixins)(混入)是面向对象编程OOP的一种实现，其作用是为了复用共有的代码，将共有的代码通过抽取为一个对象，然后通过`Mixins`进该对象来达到代码复用。具体可以参考[React Mixin的前世今生](http://www.w3ctech.com/topic/1599)。

`React.createClass`在创建组件时可以使用`mixins`属性，以数组的形式来混合类的集合。

```
var SomeMixin = {  
  doSomething() {

  }
};
const Contacts = React.createClass({  
  mixins: [SomeMixin],
  handleClick() {
    this.doSomething(); // use mixin
  },
  render() {
    return (
      <div onClick={this.handleClick}></div>
    );
  }
});
```

但是遗憾的是`React.Component`这种形式并不支持`Mixins`，至今React团队还没有给出一个该形式下的官方解决方案；但是React开发者社区提供一个全新的方式来取代`Mixins`,那就是**Higher-Order Components**，具体细节可以参考[这篇文章](https://leozdgao.me/chushi-hoc/)

### (五)如何选择哪种方式创建组件

由于React团队[已经声明](https://facebook.github.io/react/blog/2015/03/10/react-v0.13.html)React.createClass最终会被React.Component的类形式所取代。但是在找到`Mixins`替代方案之前是不会废弃掉`React.createClass`形式。所以：

```
能用React.Component创建的组件的就尽量不用React.createClass形式创建组件。
```

除此之外，创建组件的形式选择还应该根据下面来决定：

```
1、只要有可能，尽量使用无状态组件创建形式。

2、否则（如需要state、生命周期方法等），使用`React.Component`这种es6形式创建组件
```

**补充一点**

> 无状态组件内部其实是可以使用`ref`功能的，虽然不能通过`this.refs`访问到，但是可以通过将ref内容保存到无状态组件内部的一个本地变量中获取到。

例如下面这段代码可以使用ref来获取组件挂载到dom中后所指向的dom元素：

```js
function TestComp(props){
    let ref;
    return (<div>
        <div ref={(node) => ref = node}>
            ...
        </div>
    </div>)
}
```

### 参考文献

- [React 组件构造方法: ES5 (createClass) 还是 ES6 (class)？](http://www.w3cplus.com/react/react-es5-createclass-vs-es6-classes.html)
- [React.createClass 对比 extends React.Component](http://www.peachis.me/react-createclass-versus-extends-react-component/)
- [应该如何选择：React.createClass, ES6 Classes, 无状态函数式组件](https://medium.com/@kingzs70/%E5%BA%94%E8%AF%A5%E5%A6%82%E4%BD%95%E9%80%89%E6%8B%A9-react-createclass-es6-classes-%E6%97%A0%E7%8A%B6%E6%80%81%E5%87%BD%E6%95%B0%E5%BC%8F%E7%BB%84%E4%BB%B6-52f07dd70e75#.3m2m0t62r)
- [React中函数式声明组件](https://segmentfault.com/a/1190000006180667)
- [React Mixin 的前世今生](http://www.w3ctech.com/topic/1599)