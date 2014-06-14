---
layout: post
title: "gulp-jsxify: transpile your html templates to react jsx files"
description: ""
category: 
tags: ['gulp','tools','react','jsx']
---
{% include JB/setup %}


# [gulp](http://gulpjs.com)-jsxify

## TL;DR

Hi people, as promised yesterday, here is a post about my last published npm module: 
gulp-jsxify, a module to transpile html files to [Facebook jsx](http://facebook.github.io/react/docs/jsx-in-depth.html) format.

* Find it on [npmjs here](https://www.npmjs.org/package/gulp-jsxify)
* To install it: npm install --save-dev gulp-jsxify
* Usage: 


```javascript
var gulp = require('gulp');
var jsxify = require('gulp-jsxify');

gulp.task('default', function() {
    
    return gulp.src('template.html')
        .pipe(jsxify({
            requires:{
                AnotherWidget: './another-widget'
            }
        }))
        .pipe(gulp.dest('./jsx-files'));
});

```


## A jsx introduction

Jsx is a transpiler from Facebook that allow to include xml inside javascript, and have it compiled
to vanilla javascript. It someway resemble Scala inline XML, but applied to javascript.

__If you don't know Scala lang, here you'll find [an excellent post on it's XML inline feature](http://www.eishay.com/2009/05/scala-and-xml-part-1.html)__

Jsx is thought to be paired with React, allowing you to insert your template markup directly into your
main javascript react files.


