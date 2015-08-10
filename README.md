# vue-async-data

> Async data loading plugin for Vue.js

### Install

``` bash
npm install vue-async-data
```

### Usage

``` js
// assuming CommonJS
var Vue = require('vue')
var VueAsyncData = require('vue-async-data')

// use globally
// you can also just use `VueAsyncData.mixin` where needed
Vue.use(VueAsyncData)
```

Then, in your component options, provide an `asyncData` function:

``` js
Vue.component('example', {
  data: function {
    return {
      msg: 'not loaded yet...'
    }
  },
  asyncData: function (resolve, reject) {
    // load data and call resolve(data)
    // or call reject(reason) if something goes wrong
    setTimeout(function () {
      // this will call `vm.$set('msg', 'hi')` for you
      resolve({
        msg: 'hi'
      })
    }, 1000)
  }
})
```

You can also return a promise that resolves to the data to be set:

``` js
Vue.component('example', {
  // ...
  asyncData: function () {
    return someServiceThatReturnsPromise.get(12345)
      .then(function (msg) {
        return {
          msg: msg
        }
      })
  }
})
```

Parallel fetching with `Promise.all` and ES6:

``` js
Vue.component('example', {
  // ...
  asyncData() {
    return Promise.all([
      serviceA.get(123),
      serviceB.get(234)
    ]).then(([a, b]) => ({a, b}))
  }
})
```

Your component automatically gets a `$loadingAsyncData` meta property, which allows you to display a loading state before the data is loaded:

``` html
<div v-if="$loadingAsyncData">Loading...</div>
<div v-if="!$loadingAsyncData">Loaded. Put your real content here.</div>
```

Or, if you prefer to wait until data loaded to display the component, you can use `wait-for` to listen for the `async-data` event, which is automatically emitted when the data is loaded:

``` html
<example wait-for="async-data"></example>
```
