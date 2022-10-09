---
date: "2015-08-15T19:01:38Z"
tags:
- planetkde
- baloo
- nodejs
title: Baloo and NodeJs
layout: "@layouts/BlogPost.astro"
---

I took some time to write some very basic nodejs bindings for Baloo. They are available on [github](https://github.com/vHanda/node-baloo) and [npm](https://www.npmjs.com/package/baloo).

###### Example
It can be installed via **npm install baloo**

```node
var Baloo = require('baloo');

var query = new Baloo.Query('keyword');
query.exec(function(results) {
    results.forEach(function(filePath) {
        console.log(filePath);
    });
});
```

I'm aware of a few people who have deployed Baloo on a server. I'm hoping this will help them. It also paves the way for some interesting web-based user interfaces for file-searching.
