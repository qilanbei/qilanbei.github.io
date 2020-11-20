---
title: 关于react问题整理
date: 2018-08-16 19:56:10
categories:
- react
tags: 
- 关于react问题整理
toc: true 文章目录
---

+ 对接触react过程中遇到的问题进行整理
<!-- more -->
<The rest of contents | 余下全文>

### 1. 如何绑定一个函数到一个组件实例

有A, B, C 三种绑定方式

A: 第一种绑定方式，官网推荐，这样绑定对于react底层设计友好

```
class Foo extends Component {
  constructor(props) {
    super(props);
    <!--A:-->
    this.handleClick = this.handleClick.bind(this);
  }
  handleClick() {
    console.log('Click happened');
  }
  render() {
    return <button onClick={this.handleClick}>Click Me</button>;
  }
}
```

B：第二种绑定方式：在render方法中使用Function.prototype.bind

```
render() {
	但是会在每次组件渲染时创建一个新的函数，可能会影响性能
    return <button onClick={this.handleClick.bind(this)}>Click Me</button>;
    
    也可以在Render中的使用箭头函数，此语法问题在于每次渲染组件时都会创建不同的回调函数。在大多数情况下，
    这没什么问题，但如果该回调函数作为 prop 传入子组件时，这些组件可能会进行额外的重新渲染。
    我们通常建议在构造器中绑定或使用 class fields 语法来避免这类性能问题
    直接箭头函数是向回调函数传递参数的最简单的办法，如下：
    
    return <button onClick={() => this.handleClick()}>Click Me</button>;
  }
```

C: 第三种绑定方式, 使用箭头函数的方式
  为什么这样可以
  箭头函数本身没有this ,但是它会捕捉其所在上下文的this来作为自己的this
  ES6中的箭头函数会直接调用的this是继承父级的this
```
 handleClick = () => {
    console.log('Click happened');
  }
  
```

### 2. 为什么我的函数每次组件渲染时都会被调用

在你传递函数给组件时：
```

render() {
  // Wrong: handleClick is called instead of passed as a reference!
  return <button onClick={this.handleClick()}>Click Me</button>
}

```
确保你没有调用函数, 正确做法是，传递函数本身（不带括号）：
```
render() {
  // Correct: handleClick is passed as a reference!
  return <button onClick={this.handleClick}>Click Me</button>
}
```
### 3. 如何传递参数给事件处理器或回调

可以使用箭头函数包裹事件处理器，并传递参数：

```
render() {
    return (
      <div>
        <ul>
          {this.state.letters.map(letter =>
            <li key={letter} onClick={() => this.handleClick(letter)}>
              {letter}
            </li>
          )}
        </ul>
      </div>
    )
  }
```

或者使用.bind()

```

<li key={letter} onClick={this.handleClick.bind(this, letter)}>{letter}</li>

```

通过data-属性传递参数: 

```
handleClick(e) {
    this.setState({
      justClicked: e.target.dataset.letter
    });
  }
render() {
    return (
      <div>
        Just clicked: {this.state.justClicked}
        <ul>
          {this.state.letters.map(letter =>
            <li key={letter} data-letter={letter} onClick={this.handleClick}>
              {letter}
            </li>
          )}
        </ul>
      </div>
    )
  }

```

### 4. 为什么我setState数据之后，打印出来还是原来的值

#### 首先你要知道setState到底做了什么

setState() 用于安排一个组件的 state 对象的一次更新的时间表。当状态改变时，组件通过重新渲染来响应

setState 的调用是异步的——不要紧接在调用 setState 之后，依赖 this.state 来反射新值。

```
incrementCount() {
  // Note: this will *not* work as intended.
  this.setState({count: this.state.count + 1});
}

handleSomething() {
  // Let's say `this.state.count` starts at 0.
  this.incrementCount();
  this.incrementCount();
  this.incrementCount();
  
  你期望的是变成3  但是每次都是1
}
```

#### 那如何用依赖于当前状态的值更新状态

如果你需要基于当前状态计算值，则传递一个更新函数而不是一个对象（请参阅下面的详细信息）

关于setState，其实官网示例是这样的：setState(updater, [callback])，

所以传递一个函数而不是一个对象给 setState 来确保调用总是使用最新的状态，
传递一个更新函数允许你在更新器中访问当前的状态值，
由于 setState 调用是批处理的，这允许你链式更新并确保它们一个建立在另一个之上，而不是产生冲突

将setState()认为是一次请求而不是一次立即执行的命令来更新组件。
为了更为可观的性能，React可能会推迟它，稍后会一次性更新这些组件。React不能保证状态改变被应用是立刻地

这使得在调用setState()后立刻读取this.state变成一个潜在陷阱

代替地，使用componentDidUpdate或一个setState回调（setState(updater, callback)），当中的每个方法都会保证在更新被应用之后触发。

updater函数接收到的state 和 props保证都是最新的。updater的输出被浅合并到state中

setState()的第二个参数是一个可选的回调函数，其执行将是在一旦setState完成，并且组件被重新渲染之后。

通常，对于这类逻辑，我们推荐使用componentDidUpdate

```

incrementCount() {
  this.setState((state) => {
    // Important: read `state` instead of `this.state` when updating.
    return {count: state.count + 1}
  });
}

```

#### 为什么React不是同步地更新this.state

这个问题也可以参考我的另一篇blog: [react入门学习](https://qilanbei.github.io/2017/10/27/react-init/) 中关于react的diff算法的简述提到

其实，React故意”等待”直到所有的组件都调用setState()在他们的事件处理器中才开始重新渲染。这提升了性能，避免了不必要的重新渲染

### 5. 无法以异步方式访问event.target

```javascript
function onClick(event) {
  console.log(event); // => nullified object.
  console.log(event.type); // => "click"
  const eventType = event.type; // => "click"

  setTimeout(function() {
    console.log(event.type); // => null
    console.log(eventType); // => "click"
  }, 0);

  // Won't work. this.state.clickEvent will only contain null values.
  this.setState({clickEvent: event});

  // You can still export event properties.
  this.setState({eventType: event.type});
}
```
