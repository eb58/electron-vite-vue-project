# electron-vite-vue

🥳 Really simple `Electron` + `Vue` + `Vite` boilerplate.

<!-- [![awesome-vite](https://awesome.re/mentioned-badge.svg)](https://github.com/vitejs/awesome-vite) -->
<!-- [![Netlify Status](https://api.netlify.com/api/v1/badges/ae3863e3-1aec-4eb1-8f9f-1890af56929d/deploy-status)](https://app.netlify.com/sites/electron-vite/deploys) -->
<!-- [![GitHub license](https://img.shields.io/github/license/caoxiemeihao/electron-vite-vue)](https://github.com/electron-vite/electron-vite-vue/blob/main/LICENSE) -->
<!-- [![GitHub stars](https://img.shields.io/github/stars/caoxiemeihao/electron-vite-vue?color=fa6470)](https://github.com/electron-vite/electron-vite-vue) -->
<!-- [![GitHub forks](https://img.shields.io/github/forks/caoxiemeihao/electron-vite-vue)](https://github.com/electron-vite/electron-vite-vue) -->
[![GitHub Build](https://github.com/electron-vite/electron-vite-vue/actions/workflows/build.yml/badge.svg)](https://github.com/electron-vite/electron-vite-vue/actions/workflows/build.yml)
[![GitHub Discord](https://img.shields.io/badge/chat-discord-blue?logo=discord)](https://discord.gg/sRqjYpEAUK)

## Features

📦 Out of the box  
🎯 Based on the official [template-vue-ts](https://github.com/vitejs/vite/tree/main/packages/create-vite/template-vue-ts), less invasive  
🌱 Extensible, really simple directory structure  
💪 Support using Node.js API in Electron-Renderer  
🔩 Support C/C++ native addons  
🖥 It's easy to implement multiple windows  

## Quick Start

```sh
npm create electron-vite
```

<!-- [![quick-start](https://asciinema.org/a/483731.svg)](https://asciinema.org/a/483731) -->

![electron-vite-vue.gif](/electron-vite-vue.gif)

## Debug

![electron-vite-react-debug.gif](https://github.com/electron-vite/electron-vite-react/blob/main/electron-vite-react-debug.gif?raw=true)

## Directory

```diff
+ ├─┬ electron
+ │ ├─┬ main
+ │ │ └── index.ts    entry of Electron-Main
+ │ └─┬ preload
+ │   └── index.ts    entry of Preload-Scripts
  ├─┬ src
  │ └── main.ts       entry of Electron-Renderer
  ├── index.html
  ├── package.json
  └── vite.config.ts
```

<!--
## Be aware

🚨 By default, this template integrates Node.js in the Renderer process. If you don't need it, you just remove the option below. [Because it will modify the default config of Vite](https://github.com/electron-vite/vite-plugin-electron-renderer#config-presets-opinionated).

```diff
# vite.config.ts

export default {
  plugins: [
-   // Use Node.js API in the Renderer-process
-   renderer({
-     nodeIntegration: true,
-   }),
  ],
}
```
-->

## FAQ

- [C/C++ addons, Node.js modules - Pre-Bundling](https://github.com/electron-vite/vite-plugin-electron-renderer#dependency-pre-bundling)
- [dependencies vs devDependencies](https://github.com/electron-vite/vite-plugin-electron-renderer#dependencies-vs-devdependencies)

## Testen des Beispielprogramms

```sh
npm run dev
```

# How to switch to electron version 19.1.9

 package.json -> devDependencies ->     "electron": "19.1.9",
 HelloWorld.vue -> const winax = require("./winax/winax-for-electron-19.1.9/winax");
 npm i
 npm run devDependencies

# How to switch to electron version  20.3.8

 package.json -> devDependencies ->     "electron": ""20.3.8"",
 HelloWorld.vue -> const winax = require("./winax/winax-for-electron-20.3.8/winax");
 npm i
 npm run devDependencies

# How to switch to electron version  that ist not already compiled and provided in ./winax-folder

First find wanted version in <https://releases.electronjs.org/releases/stable> for example 27.1.0
 package.json -> devDependencies ->     "electron": "27.1.0",
 npm i
   npm rebuild winax --runtime=electron --target=27.1.0 --dist-url=<https://electronjs.org/headers> --build-from-source

 copy ./winax/winax-for-electron-20.3.8 -> ./winax/winax-for-electron-27.1.0
 copy from ./node_modules/winax/build/release/node_axtivex.node -> . ./winax/winax-for-electron-27.1.0/build/release/node_axtivex.node

 HelloWorld.vue -> const winax = require("./winax/winax-for-electron-27.1.0/winax");

 npm run dev

## Bauen winax für die verschiedenen electron-Versionen

```bash
npm rebuild winax --runtime=electron --target=16.2.8  --dist-url=https://electronjs.org/headers --build-from-source
npm rebuild winax --runtime=electron --target=17.4.11 --dist-url=https://electronjs.org/headers --build-from-source
npm rebuild winax --runtime=electron --target=18.3.15 --dist-url=https://electronjs.org/headers --build-from-source
npm rebuild winax --runtime=electron --target=20.3.8  --dist-url=https://electronjs.org/headers --build-from-source
npm rebuild winax --runtime=electron --target=22.3.6  --dist-url=https://electronjs.org/headers --build-from-source
npm rebuild winax --runtime=electron --target=27.1.0  --dist-url=https://electronjs.org/headers --build-from-source
npm rebuild winax --runtime=electron --target=31.3.1  --dist-url=https://electronjs.org/headers --build-from-source
```
