## Hello World

### HelloWorld Repository
The easiest way to get started with Recycle is to use a [Hello World repository](https://github.com/recyclejs/HelloWorld).

It is a simple setup using `babel`, `webpack` and `webpack-dev-server`:

```bash
git clone https://github.com/recyclejs/HelloWorld.git
npm install
npm start
``` 
When finished, open http://localhost:8080 to view it in the browser.

### Manual Installation
The following guide assumes you have some sort of build set up using babel 
and/or bundler tool like webpack or browserify.

First, make sure you've installed Recycle:

```bash
npm i recyclejs --save
```

#### JSX pragma
By default, Recycle uses React and can be used with JSX. But in order to use it, you need
to pass a `pragma` option in your babel configuration.

Either by adding a comment in files where Recycle views are defined (useful if using Recycle in existing React app)

```javascript
/** @jsx jsx */
```   

or by defining it globally in your `.babelrc` configuration file:

For Babel 5 and prior.
```JSON
{ "jsxPragma": "jsx" }
```

For Babel 6:
```JSON
{
  "plugins": [
    ["transform-react-jsx", {"pragma": "jsx"}]
  ]
}
```

#### Creating a Recycle Instance
Let's start by creating a Recycle instance using `createRecycle`. 

Its required parameter is an `adapter` for which you can use a default one (with React and RxJS):

```javascript
import createRecycle from 'recyclejs'
import reactRxjs from 'recyclejs/adapter/react-rxjs'

const recycle = createRecycle({ 
  adapter: reactRxjs,
  plugins: []
})
```

#### Component View
All components in Recycle are functions which are returning an object with `view`, `actions` and `reducers`.
But for a simple "Hello World" example, it is sufficient to define just a `view`:

```javascript
function HelloWorld() {
  return {
    view (jsx) {
      <div>Hello World!</div>
    }
  }
}
```

The reason why `jsx` parameter is defined as first argument of the view function is because a transpiled version by babel actually looks like this: 

```javascript
function HelloWorld() {
  return {
    view (jsx) {
      jsx('div', null, 'Hello World!')
    }
  }
}
```

#### Rendering
The last step is to render our component to the DOM which can be done with `recycle.render`:

```javascript
recycle.render(HelloWorld, document.getElementById('app'))
```

or by converting a Recycle component into a React component and rendering it with `ReactDOM`:

```javascript
const HelloWorldReact = recycle.toReact(HelloWorld)

ReactDOM.render(HelloWorldReact, document.getElementById('app'))
```

