# Vite migration

![vite logo](../assets/vite-logo.jpg)

## Introduction

Hello ! Welcome to the Vite migration guide ! 

Created by **Evan You**, the Vue creator, **Vite** is the new CLI recommended for your new Vue project. This guide will help you migrate your Vue CLI to Vite. And I will try my best to help you, thanks to the experience I gain during the migration of **Partner**.

> <span style="color:green">**Good news:**</span> Vite can run with Vue2 ! I will tell you the difference between Vite+Vue2 and Vite+Vue3. So don't worry about that.

In this guide, I will give you some instructions to help you. The different issues that I encountered can save you a lot of time. If you encountered any other issues during your migration, be free to update the guide with your different troubleshooting.

> <span style="color:red">**Warning:**</span> Vite is only supported by moderns browsers (Chrome, Mozilla, etc...)

Ready ? Let's Go !

## Table of Contents

- [Vite migration](#vite-migration)
  - [Introduction](#introduction)
  - [Table of Contents](#table-of-contents)
  - [How to: Run the Project with Vite](#how-to-run-the-project-with-vite)
    - [Step 1: Update Dependencies](#step-1-update-dependencies)
    - [Step 2: Add Vite Config File](#step-2-add-vite-config-file)
    - [Step 3: Moving the index.html](#step-3-moving-the-indexhtml)
    - [Step 4: Update the Scripts](#step-4-update-the-scripts)
    - [Step 5: Last Adjustments](#step-5-last-adjustments)
    - [Step 6: Enjoy Vite](#step-6-enjoy-vite)
  - [Resources](#resources)

## How to: Run the Project with Vite

### Step 1: Update Dependencies 

For the moment, our CLI is "Vue CLI" so we need to remove all its dependencies:

```json
// package.json

"@vue/cli-plugin-babel": "~4.5.0", // remove
"@vue/cli-plugin-eslint": "~4.5.0", // remove
"@vue/cli-plugin-router": "~4.5.0", // remove
"@vue/cli-plugin-vuex": "~4.5.0", // remove
"@vue/cli-service": "~4.5.0", // remove
```

> **Note:** If your CLI is @vue/cli v1.x.x, there's none @vue/cli dependencies in your package.json, everything is managed by webpack

The **sass-loader** and **node-sass** dependencies are no longer useful because Vite provides built in support, so we can remove them:

```json
// package.json

"sass-loader": "^8.0.2" // remove
"node-sass": "^4.14.1" // remove
```

Instead you need to add the **sass** dependencies:

```json
// package.json

"sass": "^1.26.5"
```

Finally, we need our new CLI dependencies, so let's add the Vite dependencies:

```json
// package.json

  "devDependencies": {
    ...
    "@vitejs/plugin-vue": "^1.6.1",
    "vite": "^2.5.4",
    "vite-plugin-vue2" : "1.9.0" // only for Vue 2
    ...
  }
```

> <span style="color:red">**Warning:**</span> Be sure to have the good version of **vue-router** to avoid future issues

```json
"vue-router": "3.0.0", // for vue 2
"vue-router": "4.0.0", // for vue 3
```

### Step 2: Add Vite Config File 

Now we need to configure Vite. So we are going to create a file named "**vite.config.js**" at the root directory:

```js
// vite.config.js

import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue' // for vue 3
import { createVuePlugin as vue } from "vite-plugin-vue2" // for vue 2
const path = require("path");

export default defineConfig({
  plugins: [vue()],
  server: {
    port: '8092'
  },
  resolve: {
    extensions: ['.mjs', '.js', '.ts', '.jsx', '.tsx', '.json', '.vue'],
    alias: {
      "@": path.resolve(__dirname, "./src"),
      'stream': 'stream-browserify',
      vue: 'vue/dist/vue.js'
    },
  },
})

```

> <span style="color:red">**Warning:**</span> Be sure you import the correct plugin according to your Vue version


The .vue at the end of each import is now necessary and the extension list in "vite.config.js" is temporary because it's not recommended to let your import like this:

```js
import HelloWorld from "@/components/HelloWorld";
```

Now, you need to import vue components like this:

```js
import HelloWorld from "@/components/HelloWorld.vue";
```

> **Note:** But for the moment you can just let the extension list in vite.config.js


### Step 3: Moving the index.html

**index.html** has to be at the **root directory**. So make sure it's at the right place.

In the index.html file, we are no longer able to use the following syntax:

```html
// index.html

<!--remove-->
<title><%= htmlWebpackPlugin.options.title %></title> 
```

Instead, use **hard code** like this:

```html
// index.html

<!--add-->
<title>Hard Coded Title</title>
```

> **Note:** title tag is just an example, apply the same change for others tags

Finally, we need to import main.js at the end of the body:

```html
// index.html

<script type="module" src="/src/main.js"></script>
```

### Step 4: Update the Scripts

The scripts needs to be update. So remove all scripts based on **node** or **@vue/cli** and add the following scripts:

```json
// package.json

"dev": "vite",
"build": "vite build",
"serve": "vite preview",
```

### Step 5: Last Adjustments

In each file, delete "lang=html" on your template.

And finally, require is not recognized anymore so you need to replace the require by import

### Step 6: Enjoy Vite

Now, we can run:

```shell
npm install && npm run dev
```

And enjoy the benefits of **Vite** !

## Resources

- More information on [Vue.js](https://vuejs.org/)
- More information on [Vite](https://vitejs.dev/)