# Log Rocket

## Introduction 
Before we understand what Create React App solves, let's first learn what a toolchain is.

A Toolchain is described as "a set of distinct software development tools that are linked (or chained) together by specific stages."

In other words, any software development framework is made up of a bunch of supporting tools optimized to do specific functions. 

In React development, different toolchaings satisfy different requirements for product development. For instance, Next.js is great for building server-rendered websites.
And GatsbyJS is optimized for static, content-oriented websites like blogs and newsletters. 

## What is Create React App?

Create React App is also a toolchain. It is specifically recommended by the React community for building single-page applications (SPAs) and for learning React ("hello world" applications).

It sets up your development environment so that you can use the latest JavaScript features, provides a nice developer experience, and optimizes your app for production.

## Getting started with Create React App

### Installation
```bash 
  npx create-react-app rate-restaurants 
```

This command runs for a few seconds and exits happily after creating a bare-bones React application under a new directory called "rate-restaurants". 

### Each Folder

#### node_modules
This folder is part of the npm system. npm puts local installs of packages in `./node_modules` of the current package root. Basically, the packages you want to use by calling an "import" statement go here. 

#### public 
This folder contains the `index.html` and `manifest.json` files.

##### index.html
This `index.html` serves as a template for generating `build/index.html`, which is ultimately the main file that gets served on the browser. 

What are "bundled scripts"?

In order to understand this, we need to learn about one more concept in the world of modern JavaScript toolchains, which is webpack. 
Think of webpack as a tool that bundles up all of your source files (.js, .css, etc.) and creates a single `bundle.js` file that can be served from the `index.html` file inside a `<script>` tag.

This way, the number of HTTP requests made within the app is significantly reduced, which directly improves the app's performance on the network. Besides, webpack also halps in making the code modular and flexible. 

When webpack compiles the assets, it produces a single bundle (or several, if you use code splitting). It makes their final paths available to all plugins -- one such plugin is for injecting scripts into HTML.

##### manifest.json

Web app manifests are part of a collection of web technologies called "progressive web apps", which are websites that can be installed to a device's homescreen without an app store. 

Unlike regular web apps with simple homescreen links or bookmarks, PWAs can be downloaded in advance and can work offline, as well as use regular Web APIs

A web application manifest provides information about a web application in a JSON text file, necessary for the web app to be downloaded and be presented to the user similarly to a native app.

Basically, the information read from this file is used to populate the web app's icons, colors, names, etc. 

A typical React application has a root component, `<App>`, that imports other child components, which in turn import other child components. 
Data flows from root to children through React properties (called "props") and flows back up using callback functions. This is the design pattern used by any basic React application.




