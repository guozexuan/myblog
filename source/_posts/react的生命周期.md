---
title: react的生命周期
date: 2018-01-21 05:37:27
tags:
---


在ES6中，一个React组件是用一个class来表示的
```
// 定义一个TodoList的React组件，通过继承React.Component来实现
class TodoList extends React.Component {
  ...
}
```


# 组件生命周期
- 一个React组件被渲染的过程有三个阶段。这个过程就叫做组件的生命周期。每个React组件都会经历这个过程。为了使这个过程可操控，React提供了一些方法，在组件生命周期过程中，通过这些方法我们可以得到某个阶段发生的通知。这些方法就叫作组件的生命周期方法，它们按特定顺序被调用。

- 方法中带有前缀 will 的在特定环节之前被调用，而带有前缀 did 的方法则会在特定环节之后被调用。

# 组件的生命周期分成三个状态：
- Mounting：已插入真实 DOM （实例化）
- Updating：正在被重新渲染  （存在期）
- Unmounting：已移出真实 DOM （销毁时）

1.  ### Mounting
当组件在客户端被实例化，第一次被创建时，以下方法依次被调用：

- constructor()
- componentWillMount()  
- render()
- componentDidMount()

2.  ### Updating
属性或状态的改变会触发一次更新。当一个组件在被重渲时，这些方法将会被调用：

> componentWillReceiveProps()
> shouldComponentUpdate()
> componentWillUpdate()
> render()
> componentDidUpdate()

3.  ### Unmounting

每当React使用完一个组件，这个组件必须从 DOM 中卸载后被销毁，此时 componentWillUnmout 会被执行，完成所有的清理和销毁工作，在 componentDidMount 中添加的任务都需要在该方法中撤销，如创建的定时器或事件监听器。

- componentWillUnmount()


> ###  参考

###  render()
最基本的，React组件需要一个render()方法，因此它至少会返回null或者false。render()负责告诉React怎样渲染这个React组件。它可以返回null，表示不渲染任何内容。它也可以返回一个组件。 

render方法需要满足下面几点：

- 只能通过 this.props 和 this.state 访问数据（不能修改）
    - this.props存储的是从父级传过来的只读数据。它属于父级，并且不能被它的子元素改变。这个数据应当认为是不可改变的。 
    - this.state存储的数据是组件私有的。它能被组件修改。当state更新后组件会自动重新渲染（一般情况下）。 

- 可以返回 null,false 或者任何React组件

- 只能出现一个顶级组件，不能返回一组元素（React15.4.0）

- 不能改变组件的状态（props，state），render方法必须是一个纯函数。也就是每次调用，这个方法都要返回同样的结果。

- 不能修改DOM的输出


render方法返回的结果并不是真正的DOM元素，而是一个虚拟的表现，类似于一个DOM tree的结构的对象。react之所以效率高，就是这个原因。


### constructor()

- React组件的构造函数将会在挂载之前被调用。当为一个React.Component子类定义构造函数时，你应该在任何其他的表达式之前调用super(props)。否则，this.props在构造函数中将是未定义，并可能引发异常。

- 构造函数是初始化状态的合适位置。若你不初始化状态且不绑定方法，那你也不需要为你的React组件定义一个构造函数。

```
constructor(props) {
  super(props);
  this.state = {
    color: props.color,
    text: '初始化'
  };
}
```


### componentWillMount()

- componentWillMount()在挂载发生前被立刻调用。其在render()之前被调用，因此在这方法里同步地设置状态将不会触发重渲。

- 在 render 方法调用之前修改 state 的最后一次机会

- 在React的生命周期中，这个函数只会被执行一次



###  componentDidMount()

- 它在React将组件插入到DOM之后立即被调用。更新后的DOM现在可以被访问，**这意味着这个方法是初始化其他需要访问这些DOM的JavaScript库的最佳地方。**React的子组件也有componentDidMount函数，并且会在父组件的componentDidMount函数之前被调用。

- 该方法被调用时，已经渲染出真实的 DOM。由于组件并不是真实的 DOM 节点，而是存在于内存之中的一种数据结构，叫做虚拟 DOM。只有当它插入文档以后，才会变成真实的 DOM 。有时需要从组件获取真实 DOM 的节点，这时就要用到 ref 属性

- 这是一个适合实现网络请求的地方。在该方法里设置状态将会触发重渲。



### componentWillReceiveProps()

当 props 发生改变，componentWilReceiveProps 会被调用。可以在这个方法里更新 state,以触发 render 方法重新渲染组件。


```
componentWillReceiveProps ( nextProps ) {
    if ( nextProps.bool ) {
        this.setState({
            bool: true
        });
    }
}
```


### shouldComponentUpdate()

- 组件挂载之后，每次调用setState后都会调用shouldComponentUpdate判断是否需要重新渲染组件。默认返回true，需要重新render。

- 返回false表示跳过后续的生命周期方法，通常不需要使用以避免出现bug。

- 在首次渲染期间或者调用了forceUpdate方法后，该方法不会被调用。


```
shouldComponentUpdate(nextProps, nextState) {
  return true;
}
```

- 在比较复杂的应用里，有一些数据的改变并不影响界面展示，可以在这里做判断，优化渲染效率。它是在重新渲染过程开始前触发的。

```
//例如想让组件只在props.color或者state.count的值变化时重新渲染
shouldComponentUpdate(nextProps, nextState) {
    if (this.props.color !== nextProps.color) {
      return true;
    }
    if (this.state.count !== nextState.count) {
      return true;
    }
    return false;
}
```


### componentWillUpdate()


```
componentWillUpdate(nextProps, nextState)
```
- componentWillUpdate()方法在React即将更新DOM之前被调用。该方法并不会在初始化渲染时调用。它得到以下两个参数：
    - nextProps：下一个属性对象
    - nextState：下一个状态对象

> 注意：
> 
> 你不能在这调用this.setState()，若你需要更新状态响应属性的调整，使用componentWillReceiveProps()代替。



### componentDidUpdate()

- componentDidUpdate()方法在React更新DOM之后会被立即调用。该方法并不会在初始化渲染时调用。它得到如下两个参数：
    - prevProps：上一个属性对象
    - prevState：上一个状态对象


- 当组件被更新时，使用该方法是操作DOM的一次机会。这也是一个适合发送请求的地方，要是你对比了当前属性和之前属性（例如，如果属性没有改变那么请求也就没必要了）。

> componentWillMount、componentDidMount和componentWillUpdate、componentDidUpdate可以对应起来。区别在于，前者只有在挂载的时候会被调用；而后者在以后的每次更新渲染之后都会被调用。

### componentWillUnmount()

componentWillUnmount()在组件被卸载和销毁之前立刻调用。可以在该方法里处理任何必要的清理工作，例如解绑定时器，取消网络请求，清理任何在componentDidMount环节创建的DOM元素。



> ##  其他API 

### forceUpdate()
- 默认情况，当你的组件或状态发生改变，你的组件将会重渲。若你的render()方法依赖其他数据，你可以通过调用forceUpdate()来告诉React组件需要重渲。

- 调用forceUpdate()将会导致组件的 render()方法被调用，并忽略shouldComponentUpdate()。这将会触发每一个子组件的生命周期方法，涵盖，每个子组件的shouldComponentUpdate() 方法。若当标签改变，React仅会更新DOM。

- 通常你应该尝试避免所有forceUpdate() 的用法并仅在render()函数里从this.props和this.state读取数据。


```
import React, {Component} from "react";

class Demo1 extends Component{
    constructor(props){
        super(props);
        this.state ={
          text:'hello',
          show: true
        }
    }

    componentDidMount() {
      this.setState({
        text: 'hello,world',
        show: false
      })
      this.forceUpdate();
    }

    shouldComponentUpdate() {
      return false;
    }

    render() {
      return (
          <div>
            {this.state.text}
          </div>
      );
    }
  }

export default Demo1;
```

## 更新render的方法
在react中，触发render的有4条路径。

以下假设shouldComponentUpdate都是按照默认返回true的方式。

- 首次渲染Initial Render
- 调用this.setState （并不是一次setState会触发一次render，React可能会合并操作，再一次性进行render）
- 父组件发生更新（一般就是props发生改变，但是就算props没有改变或者父子组件之间没有数据交换也会触发render）
- 调用this.forceUpdate

![image](https://upload-images.jianshu.io/upload_images/1814354-4bf62e54553a32b7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)