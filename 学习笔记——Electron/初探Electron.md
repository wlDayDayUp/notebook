@[TOC](目录)

# 概述

Electron是基于Node.js开发的桌面应用框架，可以使用HTML、CSS、Javascript来构建跨平台的桌面应用，并且同一份代码可以打包成Mac、Windows、Linux的应用。

Electron通过Chromium的Content API以及使用了Node的node_bindings将Chromium整合进来。

Electron有多个独立的Javascript上下文

	- 一个是后台进程负责启动运行应用的视窗（**main进程**）
	- 另一个负责具体的应用视窗（**renderer进程**）

Electron的入口文件是Javascript



# Hello World 应用

1. 安装Node.js，以及npm

2. 新建一个文件夹，并在该文件夹下执行，

   ```bash
   npm init
   ```

3. 生成一个package.json，在其中添加electron的依赖

   ```json
   {
     "name": "my_electron",
     "version": "1.0.0",
     "description": "学习electron",
     "main": "main.js",
     "scripts": {
       "start": "electron .",
       "test": "echo \"Error: no test specified\" && exit 1"
     },
     "author": "wlDayDayUp",
     "license": "ISC",
     "dependencies": {
       "electron": "5.0.8"
     }
   }
   ```

4. 新建main.js，作为应用的入口文件

   ```javascript
   'use strict';
   
   // 加载electron模块
   const electron = require('electron');
   
   // 创建electron应用对象的引用
   const app = electron.app;
   
   // 创建electron的BrowserWindow类的引用
   const BrowserWindow = electron.BrowserWindow;
   
   // 保存应用视窗的引用
   let mainWindow = null;
   
   // 监听所有视窗关闭事件，如果全部关闭了，则关闭应用（mac os不会触发该事件）
   app.on('window-all-closed', () => {
       if (process.platform !== 'darwin') app.quit();
   });
   
   // 监听应用准备运行时，告诉主视窗加载index.html，当视窗关闭后，将mainWindow置为null
   app.on('ready', () => {
       mainWindow = new BrowserWindow();
       mainWindow.loadURL(`file://${__dirname}/index.html`);
       mainWindow.on('closed', () => {
           mainWindow = null;
       })
   });
   ```

5. 新建index.html作为主界面

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Hello World</title>
   </head>
   <body>
   <h1>Hello World</h1>
   </body>
   </html>
   ```

6. 运行（在应用目录下）

   ```bash
   electron .
   ```

7. 运行结果

   