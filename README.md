# Start to construct a webpack-babel-react applicationi

## Configure webpack

### Install Webpack
```shell
npm install -D webpack webpack-cli html-webpack-plugin
```

### Create `webpack.config.js`
```js
const path = require("path");

module.exports = {
  mode: "development",
  entry: "./src/index.js",
  output: {
    clean: true,
    filename: "bundle.js",
    path: path.join(__dirname, "dist"),
  },
  plugins: [new HtmlWebpackPlugin()], // This plugin will generate index.html automatic
};
```

Create file `src/index.js`
```js
const say = () => "Hello World!";
document.body.append(say());
```
Test webpack with `npx webpack`. There'll be two file generated: `dist/bundle.js` and `dist/index.html`.

### Add `package.json` scripts
```diff
{
  //...
  "scripts": {
+    "build": "webpack",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  //...
}
```

Now use `npm run build` to build a bundle.

## Configure development server

### Install Webpack Dev Server
```shell
npm install -D webpack-dev-server
```

### Edit `webpack.config.js`
```diff
export.module = {
  //...
+  devServer: {
+    port: 3000,
+    static: path.join(__dirname, "dist"),
+  },
}
```

### Add start statement to `package.json`
```diff
  "scripts": {
+    "dev": "webpack && webpack serve",
    "build": "webpack",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
```

Try `npm run dev`, and access http://localhost:3000/ in your browser.


## Configure Babel
>Why use Babel?

Because Webpack usually compile for common js, css, html files. But we have JSX in React, Webpack is unable to recognize these syntax, but Babel can do this.

### Install
```shell
npm install -D babel-loader @babel/core @babel/preset-env @babel/preset-react
```

### Configure `babel-loader`
```js
export.modeuls = {
  //...
  module: {
    rules: [
      {
        test: /\.m?jsx?$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
          options: {
            presets: [
              "@babel/preset-env",
              [
                "@babel/preset-react",
                {
                  runtime: "automatic",
                },
              ],
            ],
          },
        },
      },
    ],
  },
};
```

## Configure React
### Install
// fix -D or -S
```shell
npm i -D react react-dom
```

### Create `src/App.jsx`
```jsx
export default function App() {
    let say = () => 'Hello World!';
    return (
        <h1>{say()}</h1>
    )
}
```
### Edit `index.js`
```diff
- const say = () => "Hello World!";
- document.body.append(say());
import { createRoot } from "react-dom/client";
import App from "./App";

// Create root element
document.body.innerHTML = "<div id='app'></div>";

const root = createRoot(document.getElementById("app"));
root.render(<App />);
```

### Resolve `xxx\src\App doesn't exist`
```diff
module.exports = {
  // ...
+  resolve: {
+    extensions: [".js", ".jsx", ".json"],
+  },
}
```
