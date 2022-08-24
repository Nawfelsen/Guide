 ## How to: Run Unit Test

![vite migration logo](../assets/vitest-logo.png)

> **Note:** Before we migrate our test, I need to tell you it's recommended to relocate your tests in the same folder of the origin file

### Unit Test

To refer that you configure **vitest** in your **vite.config.js** file, add the following line at the top of the file:

```js
// vite.config.js

/// <reference types="vitest" />

import { defineConfig } from 'vite'
....
```

To be sure that the dom is correctly recognized, you need to add jsdom in you devDependencies:

```shell
npm install jsdom --save-dev
```

Obviously, we need to add the vitest dependencies:

```shell
npm install vitest --save-dev
```

Finally, we can add the jsdom environment and set the globals parameters: 

```js
// vite.config.js

export default defineConfig({
  ....
  test: {
    environment: 'jsdom',
    globals: true
  },
  ....
```

> **Note:** globals allow you to use vitest library without import

Now we are going to configure our test. So, we need to connect our vite.config.js to the config file needed.
Let's add the setupFiles tab to the vite.config.js file:

```js
// vite.config.js

export default defineConfig({
  ...
  test: {
    ...
    setupFiles: [
      './node_modules/es6-promise/dist/es6-promise.auto.js',
      './node_modules/phantomjs-polyfill-find/find-polyfill.js',
      './node_modules/phantomjs-polyfill-includes/includes-polyfill.js',
      './node_modules/phantomjs-polyfill-find-index/findIndex-polyfill.js',
      './src/index.js',
      './src/config.js'
    ]
  },
  ...
})
```

> <span style="color:red">**Warning:**</span> Be sure you import the correct file in the setupFiles, it's a non-exhaustive list here

For now, in index.js we use "require.import", but with vite it's no longer available. So we are going to use a different way to import all tests files with vite ([Click here to see how to import multiple file with vite](https://vitejs.dev/guide/features.html#glob-import))


### Coverage 

Vitest allow us to easily added the coverage scripts

Firstly, we need to add **"c8"** dependencies:

```shell
npm install -D c8
```

Once done, we need to add the following scripts in package.json:

```json
// package.json

"coverage": "vitest run --coverage"
```

Finally, we need to update our "vite.config.js" file and add the following content:

```js
// vite.config.js

export default defineConfig({
  ...
  test: {
    ...
    coverage: {
      reporter: ['text', 'json', 'html'],
    },
    ...
  },
  ...
})
```

Now we can run:

```shell
npm run coverage
```