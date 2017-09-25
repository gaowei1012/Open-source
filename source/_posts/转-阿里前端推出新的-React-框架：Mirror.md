---
title: (转)阿里前端推出新的 React 框架：Mirror
date: 2017-09-05 11:37:15
tags: [框架]
---

Mirror 是一款基于 React、Redux 和 react-router 的前端框架，简洁高效、灵活可靠。

## 为什么？

我们热爱 React 和 Redux。但是，Redux 中有太多的样板文件，需要很多的重复劳动，这一点令人沮丧；更别提在实际的 React 应用中，还要集成 react-router 的路由了。

一个典型的 React/Redux 应用看起来像下面这样：

### actions.js
``` javascript
export const ADD_TODO = 'todos/add'
export const COMPLETE_TODO = 'todos/complete'

export function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  }
}

export function completeTodo(id) {
  return {
    type: COMPLETE_TODO,
    id
  }
}
```
### reducers.js
``` javascript
import { ADD_TODO, COMPLETE_TODO } from './actions'

let nextId = 0

export default function todos(state = [], action) {
  switch (action.type) {
    case ADD_TODO:
      return [...state, {text: action.text, id: nextId++}]
    case COMPLETE_TODO:
      return state.map(todo => {
        if (todo.id === action.id) todo.completed = true
        return todo
      })
    default:
      return state
  }
}
```
### Todos.js
``` javascript 
import { addTodo, completeTodo } from './actions'

// ...

// 在某个事件处理函数中
dispatch(addTodo('a new todo'))

// 在另一个事件处理函数中
dispatch(completeTodo(42))
```

看起来是不是有点繁冗？这还是没考虑 异步 action 的情况呢。如果要处理异步 action，还需要引入 middleware（比如 redux-thunk 或者 redux-saga），那么代码就更繁琐了。

## 使用 Mirror 重写

### Todos.js

``` javascript
import mirror, { actions } from 'mirrorx'

let nextId = 0

mirror.model({
  name: 'todos',
  initialState: [],
  reducers: {
    add(state, text) {
      return [...state, {text, id: nextId++}]
    },
    complete(state, id) {
      return state.map(todo => {
        if (todo.id === id) todo.completed = true
        return todo
      })
    }
  }
})

// ...

// 在某个事件处理函数中
actions.todos.add('a new todo')

// 在另一个事件处理函数中
actions.todos.complete(42)
```

看起来是不是有点繁冗？这还是没考虑 异步 action 的情况呢。如果要处理异步 action，还需要引入 middleware（比如 redux-thunk 或者 redux-saga），那么代码就更繁琐了。

## 使用 Mirror 重写

### Todos.js
``` javascript
import mirror, { actions } from 'mirrorx'

let nextId = 0

mirror.model({
  name: 'todos',
  initialState: [],
  reducers: {
    add(state, text) {
      return [...state, {text, id: nextId++}]
    },
    complete(state, id) {
      return state.map(todo => {
        if (todo.id === id) todo.completed = true
        return todo
      })
    }
  }
})

// ...

// 在某个事件处理函数中
actions.todos.add('a new todo')

// 在另一个事件处理函数中
actions.todos.complete(42)
```

是不是就简单很多了？只需一个方法，即可定义所有的 action 和 reducer（以及 异步 action）。

而且，这行代码：

``` javascript 
actions.todos.add('a new todo')
```

完全等同于这行代码：

``` javascript 
dispatch({
  type: 'todos/add',
  text: 'a new todo'
})
```

完全不用关心具体的 action type，不用写大量的重复代码。简洁，高效。

## 异步 action

上述代码示例仅仅针对同步 action。

事实上，Mirror 对异步 action 的处理，也同样简单：

``` javascript 
mirror.model({
  // 省略前述代码
  effects: {
    async addAsync(data, getState) {
      const res = await Promise.resolve(data)
      // 调用 `actions` 上的方法 dispatch 一个同步 action
      actions.todos.add(res)
    }
  }
})
```

没错，这样就定义了一个异步 action。上述代码的效果等同于如下代码：

``` javascript 
actions.todos.addSync = (data, getState) => {
  return dispatch({
    type: 'todos/addAsync',
    data
  })
}
```

调用 actions.todos.addSync 方法，则会 dispatch 一个 type 为 todos/addAsync 的 action。

你可能注意到了，处理这样的 action，必须要借助于 middleware。不过你完全不用担心，使用 Mirror 无须引入额外的 middleware，你只管定义 action/reducer，然后简单地调用一个函数就行了.

## 更加单的路由

Mirror 完全按照 react-router 4.x 的接口和方式定义路由，因此没有任何新的学习成本。

更方便的是，Mirror 的 Router 组件，其 history 对象以及跟 Redux store 的联结是自动处理过的，所以你完全不用关心它们，只需关心你自己的各个路由即可。

而且，手动更新路由也非常简单，调用 actions.routing 对象上的方法即可更新。

## 理念

Mirror 的设计理念是，在尽可能地避免发明新的概念，并保持现有开发模式的前提下，减少重复劳动，提高开发效率。

Mirror 总共只提供了 4 个新的 API，其余仅有的几个也都是已存在于 React/Redux/react-router 的接口，只不过做了封装和强化。

所以，Mirror 并没有“颠覆” React/Redux 开发流，只是简化了接口调用，省去了样板代码：

https://github.com/GwStart/GwStart.github.io/blob/master/images/Mirror.jpg

在对路由的处理上，也是如此。

## 如何使用？

使用 [create-react-app](https://github.com/facebookincubator/create-react-app) 创建一个新的 app：

```
$ npm i -g create-react-app
$ create-react-app my-app

```

穿件之后，从你npm安装MIrror:

```
cd my-app
npm i --save mirrorx
npm start
```

查看 [文档](https://github.com/mirrorjs/mirror) 了解更多。


[原文地址](https://qianduan.guru/posts/5997b695852d815d019c1659)