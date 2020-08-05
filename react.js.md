# React.js 教程

## 概览

React 是一个声明式, 高效且灵活的用于构建用户界面的 JavaScript 库, 使用 React 可以将一些简短、独立的代码片段组合成复杂的 UI 界面，这些代码片段被称作“组件”

## 核心概念

### JSX

JSX是一个JavaScript的语法扩展，在JSX语法中，可以在大括号内放置任何有效的JavaScript表达式

**JSX也是一个表达式**，在编译之后，JSX表达式会被转为普通JavaScript函数调用，并且对其取值后得到 JavaScript 对象，即可以将JSX赋值给变量，把JSX当作参数传入，以及从函数中返回JSX

JSX指定属性值，用双引号指定字符串字面量，或使用大括号插入JavaScript表达式

**JSX防止注入攻击**，React DOM在渲染所有输入内容之前，默认会进行转义，可以有效防止XSS(cross-site-scriping, 跨站脚本)攻击

**JSX表示对象**

Babel会把JSX转译成一个名为React.createElement()函数调用，以下两种示例代码等效

```react
const element = (
 	<h1 className="greeting">
  	Hello, world!
  </h1>
);
```

```react
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

React.createElement()会预先执行一些检查，帮助编写无错代码，实际这步操作创建了对象

```react
const element = {
 	type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```

这些对象成为"React元素"，其描述了所要展示的内容，React通过读取这些对象，使用它们来构建DOM以及随时保持更新

### 元素渲染

**元素时构成React应用的最小砖块**，元素描述了你在屏幕上想看到的内容，React元素是创建开销极小的普通对象，React DOM会负责更新DOM来与React元素保持一致

将设有一个<div>

```html
<div id="root"></div>
```

成为"根"节点，想要将一个React元素渲染到根DOM节点中，只需把它们一起传入ReactDOM.render()

```react
const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'))      
```

**更新已渲染的元素**，React元素是<u>**不可变对象**</u>，一旦被创建，就无法改变它的子元素或者属性，更新UI唯一的方式是创建一个全新的元素，并将其传入

**React只更新它需要更新的部分**，React DOM 会将元素和它的子元素与它们之前的状态进行比较，并只会进行必要的更新来使 DOM 达到预期的状态

### 组件&Props

**函数组件**

```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

class定义组件

```javascript
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

上述两个组件在 React 里是等效的

**注意**: **组件名称必须以大写字母开头**

##### Props的只读性

组件无论是使用**<u>函数声明还是通过 class 声明</u>**，都决不能修改自身的 props

**"纯函数"**，该函数不会尝试更改入参，且多次调用下相同的入参始终返回相同的结果

```javascript
function sum(a, b) {
  return a + b;
}
```

下面的函数不是"纯函数"

```javascript
function withdraw(account, amount) {
  account.total -= amount;
}
```

**所有 React 组件都必须像纯函数一样保护它们的 props 不被更改**

### State & 生命周期

**export**，可以多个，import时必须指定名称

**export default**，import时不必指定名称

当组件第一次被渲染到 DOM 中的时候, 这在 React 中被称为"**挂载**（mount）"

同时，当 DOM 中组件被删除的时候，这在 React 中被称为"**卸载**（unmount）"

```javascript
constructor(props) { }

componentDidMount() { }

componentWillUnmount() { }

render() { }
```

这些方法叫做"**生命周期方法**"

**componentDidMount()** 方法会在组件已经被渲染到 DOM 中后运行

constructor -> render -> componentDidmount -> render -> ... -> componentWillUnmount

##### 正确地使用 State

**不要直接修改 State，而是应该使用 setState()，构造函数是唯一可以给 `this.state` 赋值的地方**

```javascript
// Wrong
this.state.comment = 'Hello';

// Correct
this.setState({comment: 'Hello'});
```

**State 的更新可能是异步的**

出于性能考虑，React 可能会把多个 `setState()` 调用合并成一个调用

**State 的更新会被合并**

```javascript
constructor(props) {
   	super(props);
    this.state = {
      posts: [],
      comments: []
    };
}
```

分别调用 `setState()` 来单独地更新它们

```javascript
componentDidMount() {
    fetchPosts().then(response => {
      this.setState({
        posts: response.posts
      });
    });

    fetchComments().then(response => {
      this.setState({
        comments: response.comments
      });
    });
  }
```

这里的合并是浅合并，所以 `this.setState({comments})` 完整保留了 `this.state.posts`， 但是完全替换了 `this.state.comments`

##### 数据是向下流动的

任何的 state 总是所属于特定的组件，而且从该 state 派生的任何数据或 UI 只能影响树中“低于”它们的组件

### 事件处理

React 事件的命名采用**小驼峰式**（camelCase），而不是纯小写

使用 JSX 语法时你需要传入一个函数作为事件处理函数，而不是一个字符串

```javascript
// HTML
<button onclick="activateLasers()">
  Activate Lasers
</button>

// React
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

在 React 中另一个不同点是你不能通过返回 `false` 的方式阻止默认行为。你必须显式的使用 `preventDefault`

```javascript
// HTML
<a href="#" onclick="console.log('The link was clicked.'); return false">
  Click me
</a>

// React
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```

在这里，e 是一个合成事件，React 根据 W3C 规范来定义这些合成事件，所以不需要担心跨浏览器的兼容性问题

必须谨慎对待 JSX 回调函数中的 this，在 JavaScript 中，class 的方法默认不会绑定 this，如果你忘记绑定 this.handleClick 并把它传入了 onClick，当你调用这个函数的时候 this 的值为 undefined

这并不是 React 特有的行为；这其实与 JavaScript 函数工作原理有关，通常情况下，如果你没有在方法后面添加 ()，例如 onClick={this.handleClick}，你应该为这个方法绑定 this，如果觉得使用 bind 很麻烦，这里有两种方式可以解决，如果你正在使用实验性的 public class fields 语法，你可以使用 class fields 正确的绑定回调函数：

```javascript
class LoggingButton extends React.Component {
  // 此语法确保 `handleClick` 内的 `this` 已被绑定。
  // 注意: 这是 *实验性* 语法。
  handleClick = () => {
    console.log('this is:', this);
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    );
  }
}
```

如果你没有使用 class fields 语法，你可以在回调中使用箭头函数：

```javascript
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // 此语法确保 `handleClick` 内的 `this` 已被绑定。
    return (
      <button onClick={() => this.handleClick()}>
        Click me
      </button>
    );
  }
}
```

### 表单

##### 受控组件

在 HTML 中，表单元素（如、 和 ）之类的表单元素通常自己维护 state，并根据用户输入进行更新。而在 React 中，可变状态（mutable state）通常保存在组件的 state 属性中，并且只能通过使用 setState()来更新，我们可以把两者结合起来，使 React 的 state 成为“唯一数据源”，渲染表单的 React 组件还控制着用户输入过程中表单发生的操作。被 React 以这种方式控制取值的表单输入元素就叫做“**受控组件**”。

在 HTML 中，`` 允许用户从存储设备中选择一个或多个文件，将其上传到服务器，或通过使用 JavaScript 的 File API 进行控制

```
<input type="file" />
```

因为它的 value 只读，所以它是 React 中的一个**非受控**组件

包含验证、追踪访问字段以及处理表单提交的完整解决方案，使用 [Formik](https://jaredpalmer.com/formik) 是不错的选择

在 React 应用中，任何可变数据应当只有一个相对应的唯一“数据源”，通常，state 都是首先添加到需要渲染数据的组件中去，然后，如果其他组件也需要这个 state，那么你可以将它提升至这些组件的最近共同父组件中，**你应当依靠自上而下的数据流，而不是尝试在不同组件间同步 state**

虽然提升 state 方式比双向绑定方式需要编写更多的“样板”代码，但带来的好处是，排查和隔离 bug 所需的工作量将会变少，由于“存在”于组件中的任何 state，仅有组件自己能够修改它，因此 bug 的排查范围被大大缩减了，此外，你也可以使用自定义逻辑来拒绝或转换用户的输入

如果某些数据可以由 props 或 state 推导得出，那么它就不应该存在于 state 中

#### `state` 和 `props` 之间的区别是什么？

props（“properties” 的缩写）和 state 都是普通的 JavaScript 对象。它们都是用来保存信息的，这些信息可以控制组件的渲染输出，而它们的一个重要的不同点就是：props 是传递给组件的（类似于函数的形参），而 state 是在组件内被组件自己管理的（类似于在一个函数内声明的变量）