---
title: 【React】React入门
description: '-'
tags:
  - React
abbrlink: 1718214b
date: 2019-6-12 10:52:10
---



## 基础结构

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8" />
<title>React</title>
<script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
<script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
<script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
</head>
<body>
<div id="example"></div>

<script type="text/babel">
	// 下面react实例代码放在这里
</script>

</body>
</html>
```



## JSX

### 使用JSX

 　使用JSX，可以极大的简化React元素的创建，JSX抽象化了React.createElement()函数的使用，其语法风格类似于HTML语法风格。对比如下代码可以让你更好的理解这一点。

```javascript
// 使用React.createElement()
return React.createElement('div',null,'Hello',this.props.name);

//使用JSX
return <div>Hello {this.props.name}</div>
```


　　通过Babel，JSX会把转换为使用React.createElement()类的ES5的语句，以使得其能被现今的浏览器引擎识别。

　　不过我们应该明白，**使用React并非必须使用JSX**，JSX只是一种直观的创建React nodes的方法，它是对React.createElement（）方法的抽象，通过Babel，JSX语句也可以直接在浏览器中运行了。

### 转换JSX

使用browser.js(Babel5.8.23)在浏览器中转换JSX

> 在运行时引用babel.js虽然容易使用而且还很方便，不过并不是一种好的方案，因为需要转换，所以更加耗时，这一缺点在产品阶段显得更加明显。类似于JSFiddle这样的工具，在线转换React用的就是这种方法。

Babel是React团队选择的在使用React过程中转换ES*和JSX为ES5语句的工具，可以从Babel handbook了解Babel详细的用法。

实际上，Babel的主要用途并非一个JSX语句转换器。Babel主要用途是一个**JavaScript转换器**，它可以转换各种ES*代码为浏览器可识别的ES代码。就目前来说，Babel主要会转换ES6和ES7语句为ES5语句，转换JSX看起来倒像是其的一个附加功能。

|        引入的JS        |    对应的script type     |
| :--------------------: | :----------------------: |
|   jsxTransformer.js    |  script type=”text/jsx”  |
| babel.js \| browser.js | script type=”text/babel” |

### babel.js与browser.js的关系

babel的浏览器版本为browser.js（未精简）和browser.min.js（已精简）



## React Props

state 和 props 主要的区别在于 props 是不可变的，而 state 可以根据与用户交互来改变。这就是为什么有些容器组件需要定义 state 来更新和修改数据。 而子组件只能通过 props 来传递数据。

### 默认 Props

```javascript
class HelloMessage extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}
 
HelloMessage.defaultProps = {
  name: 'Runoob'
};
 
const element = <HelloMessage/>;
 
ReactDOM.render(
  element,
  document.getElementById('example')
);
```

### Props 验证

React.PropTypes 在 React v15.5 版本后已经移到了 prop-types 库。

<script src="https://cdn.bootcss.com/prop-types/15.6.1/prop-types.js"></script>
Props 验证使用 propTypes，它可以保证我们的应用组件被正确使用，React.PropTypes 提供很多验证器 (validator) 来验证传入数据是否有效。当向 props 传入无效数据时，JavaScript 控制台会抛出警告。

```javascript
class HelloMessage extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}
 
HelloMessage.defaultProps = {
  name: 'Runoob'
};

HelloMessage.propTypes = {
    // 可以声明 prop 为指定的 JS 基本数据类型，默认情况，这些数据是可选的
    optionalArray: React.PropTypes.array,
    optionalBool: React.PropTypes.bool,
    optionalFunc: React.PropTypes.func,
    optionalNumber: React.PropTypes.number,
    optionalObject: React.PropTypes.object,
    optionalString: React.PropTypes.string,
 
    // 可以被渲染的对象 numbers, strings, elements 或 array
    optionalNode: React.PropTypes.node,
 
    //  React 元素
    optionalElement: React.PropTypes.element,
 
    // 用 JS 的 instanceof 操作符声明 prop 为类的实例。
    optionalMessage: React.PropTypes.instanceOf(Message),
 
    // 用 enum 来限制 prop 只接受指定的值。
    optionalEnum: React.PropTypes.oneOf(['News', 'Photos']),
 
    // 可以是多个对象类型中的一个
    optionalUnion: React.PropTypes.oneOfType([
      React.PropTypes.string,
      React.PropTypes.number,
      React.PropTypes.instanceOf(Message)
    ]),
 
    // 指定类型组成的数组
    optionalArrayOf: React.PropTypes.arrayOf(React.PropTypes.number),
 
    // 指定类型的属性构成的对象
    optionalObjectOf: React.PropTypes.objectOf(React.PropTypes.number),
 
    // 特定 shape 参数的对象
    optionalObjectWithShape: React.PropTypes.shape({
      color: React.PropTypes.string,
      fontSize: React.PropTypes.number
    }),
 
    // 任意类型加上 `isRequired` 来使 prop 不可空。
    requiredFunc: React.PropTypes.func.isRequired,
 
    // 不可空的任意类型
    requiredAny: React.PropTypes.any.isRequired,
 
    // 自定义验证器。如果验证失败需要返回一个 Error 对象。不要直接使用 `console.warn` 或抛异常，因为这样 `oneOfType` 会失效。
    customProp: function(props, propName, componentName) {
      if (!/matchme/.test(props[propName])) {
        return new Error('Validation failed!');
      }
    }
}
 
const element = <HelloMessage />;
 
ReactDOM.render(
  element,
  document.getElementById('example')
);
```



## React 事件处理

React 元素的事件处理和 DOM 元素类似。但是有一点语法上的不同:

+ Rect 事件绑定属性的命名采用驼峰式写法，而不是小写。
+ 如果采用 JSX 的语法你需要传入一个函数作为事件处理函数，而不是一个字符串(DOM 元素的写法)

HTML 通常写法是：

```html
<button onclick="activateLasers()">
  激活按钮
</button>
```

React 中写法为：

```html
<button onClick={activateLasers}>
  激活按钮
</button>
```

在 React 中另一个不同是你不能使用返回 false 的方式阻止默认行为， 你必须明确的使用 **preventDefault**。

例如，通常我们在 HTML 中阻止链接默认打开一个新页面，可以这样写：

```html
<a href="#" onclick="console.log('点击链接'); return false">
  点我
</a>
```

在 React 的写法为：

```javascript
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('链接被点击');
  }

  return (
    <a href="#" onClick={handleClick}>
      点我
    </a>
  );
}
```

实例中 e 是一个合成事件。

使用 React 的时候通常你不需要使用 **addEventListener** 为一个已创建的 DOM 元素添加监听器。

**Warning:** [你必须谨慎对待 JSX 回调函数中的 this，类的方法默认是不会绑定 this 的。如果你忘记绑定 this.handleClick 并把它传入 onClick, 当你调用这个函数的时候 this 的值会是 undefined。点击查看如何处理此问题](https://www.runoob.com/react/react-event-handle.html)

### 向事件处理程序传递参数

通常我们会为事件处理程序传递额外的参数。例如，若是 id 是你要删除那一行的 id，以下两种方式都可以向事件处理程序传递参数：

```html
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button> 
```

上述两种方式是等价的。
上面两个例子中，参数 e 作为 React 事件对象将会被作为第二个参数进行传递。通过箭头函数的方式，事件对象必须显式的进行传递，但是通过 bind 的方式，事件对象以及更多的参数将会被隐式的进行传递。



## React 条件渲染

在 React 中，你可以创建不同的组件来封装各种你需要的行为。然后还可以根据应用的状态变化只渲染其中的一部分。

### if 操作符

```javascript
function UserGreeting(props) {
  return <h1>欢迎回来!</h1>;
}

function GuestGreeting(props) {
  return <h1>请先注册。</h1>;
}

function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  if (isLoggedIn) {
    return <UserGreeting />;
  }
  return <GuestGreeting />;
}
 
ReactDOM.render(
  <Greeting isLoggedIn={false} />,
  document.getElementById('example')
);
```

### 元素变量

```javascript
class LoginControl extends React.Component {
  constructor(props) {
    super(props);
    this.handleLoginClick = this.handleLoginClick.bind(this);
    this.handleLogoutClick = this.handleLogoutClick.bind(this);
    this.state = {isLoggedIn: false};
  }
 
  handleLoginClick() {
    this.setState({isLoggedIn: true});
  }
 
  handleLogoutClick() {
    this.setState({isLoggedIn: false});
  }
 
  render() {
    const isLoggedIn = this.state.isLoggedIn;
 
    let button = null;
    if (isLoggedIn) {
      button = <LogoutButton onClick={this.handleLogoutClick} />;
    } else {
      button = <LoginButton onClick={this.handleLoginClick} />;
    }
 
    return (
      <div>
        <Greeting isLoggedIn={isLoggedIn} />
        {button}
      </div>
    );
  }
}
 
ReactDOM.render(
  <LoginControl />,
  document.getElementById('example')
);
```

### 与运算符 &&

在 JavaScript 中，true && expression 总是返回 expression，而 false && expression 总是返回 false。因此，如果条件是 true，&& 右侧的元素就会被渲染，如果是 false，React 会忽略并跳过它。

```javascript
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>
          您有 {unreadMessages.length} 条未读信息。
        </h2>
      }
    </div>
  );
}
 
const messages = ['React', 'Re: React', 'Re:Re: React'];
ReactDOM.render(
  <Mailbox unreadMessages={messages} />,
  document.getElementById('example')
);
```

### 三目运算符

```javascript
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      {isLoggedIn ? (
        <LogoutButton onClick={this.handleLogoutClick} />
      ) : (
        <LoginButton onClick={this.handleLoginClick} />
      )}
    </div>
  );
}
```

### 阻止组件渲染

在极少数情况下，你可能希望隐藏组件，即使它被其他组件渲染。让 render 方法返回 null 而不是它的渲染结果即可实现。

```javascript
function WarningBanner(props) {
  if (!props.warn) {
    return null;
  }
 
  return (
    <div className="warning">
      警告!
    </div>
  );
}
 
class Page extends React.Component {
  constructor(props) {
    super(props);
    this.state = {showWarning: true}
    this.handleToggleClick = this.handleToggleClick.bind(this);
  }
 
  handleToggleClick() {
    this.setState(prevState => ({
      showWarning: !prevState.showWarning
    }));
  }
 
  render() {
    return (
      <div>
        <WarningBanner warn={this.state.showWarning} />
        <button onClick={this.handleToggleClick}>
          {this.state.showWarning ? '隐藏' : '显示'}
        </button>
      </div>
    );
  }
}
 
ReactDOM.render(
  <Page />,
  document.getElementById('example')
);
```

> 组件的 render 方法返回 null 并不会影响该组件生命周期方法的回调。例如，componentWillUpdate 和 componentDidUpdate 依然可以被调用。



## React 列表 & Keys

使用 map() 方法遍历数组生成列表，列表元素需要分配一个 key。

> - 一个元素的 key 最好是这个元素在列表中拥有的一个独一无二的字符串。通常，我们使用来自数据的 id 作为元素的 key.
>
> - 当元素没有确定的 id 时，你可以使用他的序列号索引 index 作为 key.

```javascript
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number,index) =>
    <li key={index}>
      {/* 也可以写成<li key={number.toString()}>，独一无二的值 */}
      {number}
    </li>
  );
  return (
    <ul>{listItems}</ul>
  );
}
 
const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('example')
);
```

### 用keys提取组件

Keys 可以在 DOM 中的某些元素被增加或删除的时候帮助 React 识别哪些元素发生了变化。因此你应当给数组中的每一个元素赋予一个确定的标识。

**元素的 key 只有在它和它的兄弟节点对比时才有意义, 元素的 key 在他的兄弟元素之间应该唯一**。比方说，如果你提取出一个 ListItem 组件，你应该把 key 保存在数组中的这个 <ListItem /> 元素上，而不是放在 ListItem 组件中的 <li> 元素上。

```javascript
function ListItem(props) {
  // 对啦！这里不需要指定key:
  return <li>{props.value}</li>;
}
 
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // 又对啦！key应该在数组的上下文中被指定
    <ListItem key={number.toString()}
              value={number} />
 
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}
 
const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('example')
);
```



## React 组件 API

+ 设置状态：setState
+ 替换状态：replaceState
+ 设置属性：setProps
+ 替换属性：replaceProps
+ 强制更新：forceUpdate
+ 获取DOM节点：findDOMNode
+ 判断组件挂载状态：isMounted

### 设置状态：setState

> setState(object nextState[, function callback])

+ 参数说明
  ``nextState``，将要设置的新状态，该状态会和当前的state合并
  ``callback``，可选参数，回调函数。该函数会在setState设置成功，且组件重新渲染后调用。
+ 作用描述：合并nextState和当前state，并重新渲染组件。**setState是React事件处理函数中和请求回调函数中触发UI更新的主要方法。**

注意：

+ **不能在组件内部通过this.state修改状态**，因为直接修改状态不会重新渲染组件。**构造函数是唯一能够初始化 this.state 的地方。**

```javascript
// Wrong
this.state.comment = 'Hello';
// Correct
this.setState({comment: 'Hello'});
```

+ **setState()并不会立即改变this.state**，React 可以将多个 setState() 调用合并成一个调用来提高性能。

  因为 this.props 和 this.state 可能是异步更新的，你不应该依靠它们的值来计算下一个状态。

  例如，此代码可能无法更新计数器：

  ```javascript
  // Wrong
  this.setState({
    counter: this.state.counter + this.props.increment,
  });
  要修复它，请使用第二种形式的 setState() 来接受一个函数而不是一个对象。 该函数将接收先前的状态作为第一个参数，将此次更新被应用时的props做为第二个参数：
  
  // Correct
  this.setState((prevState, props) => ({
    counter: prevState.counter + props.increment
  }));
  ```

  

+ **setState()总是会触发一次组件重绘**，除非在shouldComponentUpdate()中实现了一些条件渲染逻辑。

```javascript
class Counter extends React.Component{
  constructor(props) {
      super(props);
      this.state = {clickCount: 0};
      this.handleClick = this.handleClick.bind(this);
  }
  
  handleClick() {
    this.setState(function(state) {
      return {clickCount: state.clickCount + 1};
    });
  }
  render () {
    return (<h2 onClick={this.handleClick}>点我！点击次数为: {this.state.clickCount}</h2>);
  }
}
ReactDOM.render(
  <Counter />,
  document.getElementById('example')
);
```



### 替换状态：replaceState

> replaceState(object nextState[, function callback])

+ 参数说明
  ``nextState``，将要设置的新状态，该状态会替换当前的state
  ``callback``，可选参数，回调函数。该函数会在replaceState设置成功，且组件重新渲染后调用。

+ 作用描述：replaceState()方法与setState()类似，但是方法只会保留nextState中状态，原state不在nextState中的状态都会被删除。



### 设置属性：setProps

> setProps(object nextProps[, function callback])

+ 参数说明
  ``nextProps``，将要设置的新属性，该属性会和当前的props合并
  ``callback``，可选参数，回调函数。该函数会在setProps设置成功，且组件重新渲染后调用。

+ 作用描述：设置组件属性，并重新渲染组件。

+ **props相当于组件的数据流，它总是会从父组件向下传递至所有的子组件中。**

+ 使用情景：

  当和一个外部的JavaScript应用集成时，我们可能会需要向组件传递数据或通知React.render()组件需要重新渲染，可以使用setProps()。

  更新组件，我可以在节点上再次调用React.render()，也可以通过setProps()方法改变组件属性，触发组件重新渲染。



### 替换属性：replaceProps

> replaceProps(object nextProps[, function callback])

+ 参数说明
  ``nextProps``，将要设置的新属性，该属性会替换当前的props。
  ``callback``，可选参数，回调函数。该函数会在replaceProps设置成功，且组件重新渲染后调用。
+ 作用描述：replaceProps()方法与setProps类似，但它会删除原有 props。



### 强制更新：forceUpdate

> forceUpdate([function callback])

+ 参数说明
  ``callback``，可选参数，回调函数。该函数会在组件render()方法调用后调用。
+ 作用描述：forceUpdate()方法会使组件调用自身的render()方法重新渲染组件，组件的子组件也会调用自己的render()。但是，组件重新渲染时，依然会读取this.props和this.state，如果状态没有改变，那么React只会更新DOM。
+ 使用情景：forceUpdate()方法适用于this.props和this.state之外的组件重绘（如：修改了this.state后），通过该方法通知React需要调用render()
+ **一般来说，应该尽量避免使用forceUpdate()，而仅从this.props和this.state中读取状态并由React触发render()调用。**



### 获取DOM节点：findDOMNode

> DOMElement findDOMNode()

+ 参数说明
  返回值：DOM元素DOMElement
+ 如果组件已经挂载到DOM中，该方法返回对应的本地浏览器 DOM 元素。当render返回null 或 false时，**this.findDOMNode()**也会返回null。从DOM 中读取值的时候，该方法很有用，如：获取表单字段的值和做一些 DOM 操作。



### 判断组件挂载状态：isMounted

> bool isMounted()

- 参数说明
  返回值：true或false，表示组件是否已挂载到DOM中
- isMounted()方法用于判断组件是否已挂载到DOM中。可以使用该方法保证了setState()和forceUpdate()在异步场景下的调用不会出错。

在 HTML 区块标签如 <div>、<table>、<pre>、<p>间的 Markdown 格式语法将不会被处理。比如，你在 HTML 区块内使用 Markdown 样式的```*强调*```会没有效果。

HTML 的区段（行内）标签如 <span>、<cite>、<del> 可以在 Markdown 的段落、列表或是标题里随意使用。依照个人习惯，甚至可以不用 Markdown 格式，而直接采用 HTML 标签来格式化。





