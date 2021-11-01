# Getting Started

## Installation

``` bash
npm install vue vue-server-renderer --save
```

We will be using NPM throughout the guide, but feel free to use [Yarn](https://yarnpkg.com/en/) instead.

#### Notes

- It's recommended to use Node.js version 6+.
- `vue-server-renderer` and `vue` must have matching versions.
- `vue-server-renderer` relies on some Node.js native modules and therefore can only be used in Node.js. We may provide a simpler build that can be run in other JavaScript runtimes in the future.

## Rendering a Vue Instance

``` js
// Step 1: Create a Vue instance
const Vue = require('vue')
const app = new Vue({
  template: `<div>Hello World</div>`
})

// Step 2: Create a renderer
const renderer = require('vue-server-renderer').createRenderer()

// Step 3: Render the Vue instance to HTML
renderer.renderToString(app, (err, html) => {
  if (err) throw err
  console.log(html)
  // => <div data-server-rendered="true">Hello World</div>
})

// in 2.5.0+, returns a Promise if no callback is passed:
renderer.renderToString(app).then(html => {
  console.log(html)
}).catch(err => {
  console.error(err)
})
```

## Integrating with a Server

It is pretty straightforward when used inside a Node.js server, for example [Express](https://expressjs.com/):

``` bash
npm install express --save
```
---
``` js
const Vue = require('vue')
const server = require('express')()
const renderer = require('vue-server-renderer').createRenderer()

server.get('*', (req, res) => {
  const app = new Vue({
    data: {
      url: req.url
    },
    template: `<div>The visited URL is: {{ url }}</div>`
  })

  renderer.renderToString(app, (err, html) => {
    if (err) {
      res.status(500).end('Internal Server Error')
      return
    }
    res.end(`
      <!DOCTYPE html>
      <html lang="en">
        <head><title>Hello</title></head>
        <body>${html}</body>
      </html>
    `)
  })
})

server.listen(8080)
```

## Using a Page Template

When you render a Vue app, the renderer only generates the markup of the app. In the example we had to wrap the output with an extra HTML page shell.

To simplify this, you can directly provide a page template when creating the renderer. Most of the time we will put the page template in its own file, e.g. `index.template.html`:

``` html
<!DOCTYPE html>
<html lang="en">
  <head><title>Hello</title></head>
  <body>
    <!--vue-ssr-outlet-->
  </body>
</html>
```

Notice the `<!--vue-ssr-outlet-->` comment -- this is where your app's markup will be injected.

We can then read and pass the file to the Vue renderer:

``` js
const renderer = createRenderer({
  template: require('fs').readFileSync('./index.template.html', 'utf-8')
})

renderer.renderToString(app, (err, html) => {
  console.log(html) // will be the full page with app content injected.
})
```

### Template Interpolation

The template also supports simple interpolation. Given the following template:

``` html
<html>
  <head>
    <!-- use double mustache for HTML-escaped interpolation -->
    <title>{{ title }}</title>

    <!-- use triple mustache for non-HTML-escaped interpolation -->
    {{{ meta }}}
  </head>
  <body>
    <!--vue-ssr-outlet-->
  </body>
</html>
```

We can provide interpolation data by passing a "render context object" as the second argument to `renderToString`:

``` js
const context = {
  title: 'hello',
  meta: `
    <meta ...>
    <meta ...>
  `
}

renderer.renderToString(app, context, (err, html) => {
  // page title will be "Hello"
  // with meta tags injected
})
```

The `context` object can also be shared with the Vue app instance, allowing components to dynamically register data for template interpolation.

In addition, the template supports some advanced features such as:

- Auto injection of critical CSS when using `*.vue` components;
- Auto injection of asset links and resource hints when using `clientManifest`;
- Auto injection and XSS prevention when embedding Vuex state for client-side hydration.

We will discuss these when we introduce the associated concepts later in the guide.







# VUE基本DEMO

频繁用 vue 创建新的项目, 基本功能模块基本一样, 所以为了不必每个项目都重新搭建基本框架. 也为快速的创建一个上期而做的 DEMO. 下面记录搭建基本框架的步骤和方法. 随着软件包的更新而更新.



## 创建vue项目



```vue
$ vue create vue-demo
```













## 增加多语言

1. 在项目增加vue-i18插件

   ```
   $ npm install vue-i18n --save
   ```

   

2. 增加多语言文档

   ```
   vim .src/common/lang/zh.js
   ```

   ```json
   export default {
       menu: {
           title: '标题'
       },
       info: {
           info: '信息'
   
       }
   }
   ```

   

3. 配置多语言文档, 将element-ui. 需要安装 element-ui 控件.

   ```diff
    import Vue from 'vue'
    import App from './App.vue'
   +import VueI18n from 'vue-i18n'
   +import zh from './common/lang/zh'
   +import en from './common/lang/en'
   +import enLocale from 'element-ui/lib/locale/lang/en'
   +import zhLocale from 'element-ui/lib/locale/lang/zh-CN'
   +import ElementUI from 'element-ui'
   +import 'element-ui/lib/theme-chalk/index.css'
   +import 'element-ui/lib/theme-chalk/display.css'
   +
   +Vue.use(VueI18n)
   +
   +const i18n = new VueI18n({
   +  locale: 'en',
   +  messages: {
   +    'zh': {
   +      ...zh,
   +      ...zhLocale
   +    },
   +    'en': {
   +      ...en,
   +      ...enLocale
   +    }
   +  }
   +})
   +Vue.use(ElementUI, {
   +  i18n: (key, value) => i18n.t(key, value)
   +})
   +
   
    Vue.config.productionTip = false
   
    new Vue({
   +  i18n,
      render: h => h(App),
    }).$mount('#app')
   ```