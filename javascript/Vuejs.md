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

