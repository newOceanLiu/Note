## Vue instance

* Each Vue instance **proxies** all the properties found in its `data` object

* properties and methods of vue instance are prefixed with `$` to differentiate from proxy data properties

  ```javascript
  vm.$data === data // -> true
  vm.$el === document.getElementById('example')
  vm.$watch('a', function (newVal, oldVal) {
    // this callback will be called when `vm.a` changes
  })
  ```

* **DON'T** use arrow function on instance property or callback `vm.$watch('a', newVal => this.myMethod())`

### Instance Lifecycle Hooks

* All lifecycle hooks are called with their `this` context pointing to the Vue instance invoking it

  ```javascript
  var vm = new Vue({
    data: {
      a: 1
    },
    created: function () {
      // `this` points to the vm instance
      console.log('a is: ' + this.a)
    }
  })
  ```

  ​

## Tamplate 

### Interpolation

* we use {{}} mustaches for text
* mustaches can't be used inside HTML attributes, instead use a v-bind directive  `<div v-bind:id="dynamicId"></div>`
* support js expression inside all data binding `{{ numbder + 1}}`
  * only one single expression for each binding

### Directives

* attributes with `v-` prefix, reactively apply side effect to DOM when the value of the expression changes
* argument: `<a v-bind:href="url"></a>` here `href` is the argument to `v-bind`
* modifer: denoted by a dot

### Filters

* used for mustache interpolation and `v-bind` : `{{ message | capitalize}}`
* filters can be chained
* filters can take arugments (first argument always the value of the expression)

### Shorthands

* `v-bind:` -> `:`
* `v-on:` -> `@`


## Computed Properties

* computed properties will be **only** updated when the value it depends on changes, it is cached based on thier dependency
* a method invocation in comparison will always run the function whenever the re-rendering happens
* it is also a good idea to use computed properties for setter/getter, and it is normally most elegant compare to `watch`
* `watch`  is also supported, and it is more useful when you want to perform async or expensive operations in response to changing data

## List Rendering

* `v-for` has a higher priority than `v-if`, so `v-if` can be run on each iteration of loop seperatlely

* you can generate a unique id for each element of `v-for` by using `v-bind`, it is recommended to use `key` with `v-bind` whenever possible 

  ```javascript
  <div v-for="item in items" :key="item.id">
  </div>
  ```

* Vue vm will update when the array mutate in the ways like `push(), pop(), sort(), splice…`, but it will not update when you directly set item by index or modify the length of the array

  * to overcome, you can add a `splice` after the above operation to force it to re-render

    ```javascript
    Vue.set(example1.items, indexOfItem, newValue)
     > example1.items.splice(indexOfItem, 1, newValue)
    ```

### Event Handling

* `v-on` to listen to DOM events 
* you can use event modifer to restrict on event
  * `.stop, .prevent, .capture, .self, .once`

### Form input bindings

* `v-model` to create two way binding on form input
  * will handle edge cases automatically
  * it does not update during IME (Chinese), use `input` event if you need it
* `v-model` bind values are usually static strings, if we want to bind the value to a dynamic property on Vue instance, we can use `v-bind` to achieve that

## Components

* register global component: `Vue.component(tagName, {....})`

* register local component: 

  ```javascript
  new Vue({
    components: {
      ...
    }
  })
  ```

* `data` in a component must be a function

### Composing Components

**"props down, events up"**

* Props
* Custom Event