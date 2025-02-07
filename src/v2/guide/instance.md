---
title: Vue Meselemi
type: guide
order: 3
---

## Vue Meselemini döretmek

Her bir Vue programmasy `Vue` funksiýasy bilen **Vue meselemini** döretmek arkaly başlaýar:

```js
var vm = new Vue({
  // saýlawlar
})
```

Dogrydan [MVVM şablony](https://en.wikipedia.org/wiki/Model_View_ViewModel) bilen gabat gelmese-de, Vue'nyň dizaýny onuň käbir böleklerinden ylham alyndy. Umumy düşünjelerden, Vue meselemini almak üçin, biz köplenç ýagdaýda `vm` (ViewModel-iň gysgaldylmasy) üýtgeýän ululygyny ulanýarys.

Vue meselemi döredilende, oňa parametr edip **saýlawlar obýekti** berilýär. Bu goldanmanyň köp bölegi, siziň islegleriňize görä şol saýlawlary nähili ulanyp boljakdygyny düşündirýär. Salgylanma üçin, saýlawlaryň ählisini [API salgylanmasy](../api/#Options-Data) sahypasynda görüp bilersiňiz.

A Vue application consists of a **kök Vue meselemi** created with `new Vue`, optionally organized into a tree of nested, reusable components. For example, a todo app's component tree might look like this:

```
Root Instance
└─ TodoList
   ├─ TodoItem
   │  ├─ TodoButtonDelete
   │  └─ TodoButtonEdit
   └─ TodoListFooter
      ├─ TodosButtonClear
      └─ TodoListStatistics
```

We'll talk about [the component system](components.html) in detail later. For now, just know that all Vue components are also Vue instances, and so accept the same options object (except for a few root-specific options).

## Data and Methods

When a Vue instance is created, it adds all the properties found in its `data` object to Vue's **reactivity system**. When the values of those properties change, the view will "react", updating to match the new values.

```js
// Our data object
var data = { a: 1 }

// The object is added to a Vue instance
var vm = new Vue({
  data: data
})

// Getting the property on the instance
// returns the one from the original data
vm.a == data.a // => true

// Setting the property on the instance
// also affects the original data
vm.a = 2
data.a // => 2

// ... and vice-versa
data.a = 3
vm.a // => 3
```

When this data changes, the view will re-render. It should be noted that properties in `data` are only **reactive** if they existed when the instance was created. That means if you add a new property, like:

```js
vm.b = 'hi'
```

Then changes to `b` will not trigger any view updates. If you know you'll need a property later, but it starts out empty or non-existent, you'll need to set some initial value. For example:

```js
data: {
  newTodoText: '',
  visitCount: 0,
  hideCompletedTodos: false,
  todos: [],
  error: null
}
```

The only exception to this being the use of `Object.freeze()`, which prevents existing properties from being changed, which also means the reactivity system can't _track_ changes.

```js
var obj = {
  foo: 'bar'
}

Object.freeze(obj)

new Vue({
  el: '#app',
  data: obj
})
```

```html
<div id="app">
  <p>{{ foo }}</p>
  <!-- this will no longer update `foo`! -->
  <button v-on:click="foo = 'baz'">Change it</button>
</div>
```

In addition to data properties, Vue instances expose a number of useful instance properties and methods. These are prefixed with `$` to differentiate them from user-defined properties. For example:

```js
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch is an instance method
vm.$watch('a', function (newValue, oldValue) {
  // This callback will be called when `vm.a` changes
})
```

In the future, you can consult the [API reference](../api/#Instance-Properties) for a full list of instance properties and methods.

## Instance Lifecycle Hooks

<div class="vueschool"><a href="https://vueschool.io/lessons/understanding-the-vuejs-lifecycle-hooks?friend=vuejs" target="_blank" rel="sponsored noopener" title="Free Vue.js Lifecycle Hooks Lesson">Watch a free lesson on Vue School</a></div>

Each Vue instance goes through a series of initialization steps when it's created - for example, it needs to set up data observation, compile the template, mount the instance to the DOM, and update the DOM when data changes. Along the way, it also runs functions called **lifecycle hooks**, giving users the opportunity to add their own code at specific stages.

For example, the [`created`](../api/#created) hook can be used to run code after an instance is created:

```js
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` points to the vm instance
    console.log('a is: ' + this.a)
  }
})
// => "a is: 1"
```

There are also other hooks which will be called at different stages of the instance's lifecycle, such as [`mounted`](../api/#mounted), [`updated`](../api/#updated), and [`destroyed`](../api/#destroyed). All lifecycle hooks are called with their `this` context pointing to the Vue instance invoking it.

<p class="tip">Don't use [arrow functions](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) on an options property or callback, such as `created: () => console.log(this.a)` or `vm.$watch('a', newValue => this.myMethod())`. Since an arrow function doesn't have a `this`, `this` will be treated as any other variable and lexically looked up through parent scopes until found, often resulting in errors such as `Uncaught TypeError: Cannot read property of undefined` or `Uncaught TypeError: this.myMethod is not a function`.</p>

## Lifecycle Diagram

Below is a diagram for the instance lifecycle. You don't need to fully understand everything going on right now, but as you learn and build more, it will be a useful reference.

![The Vue Instance Lifecycle](/images/lifecycle.png)
