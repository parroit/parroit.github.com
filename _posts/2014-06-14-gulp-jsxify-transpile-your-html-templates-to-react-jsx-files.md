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

```
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
to vanilla javascript. Here is an [example of input and output](http://facebook.github.io/react/jsx-compiler.html)

It someway resemble Scala inline XML, but applied to javascript.
__If you don't know Scala lang, here you'll find [an excellent post on it's XML inline feature](http://www.eishay.com/2009/05/scala-and-xml-part-1.html)__

Jsx is thought to be paired with React, allowing you to insert your template markup directly into your
main javascript react files. You can then transpile your jsx files to vanilla js 
using [this tool from react](http://fb.me/JSXTransformer-0.10.0.js).

## React workflow

I start using React for a side-project of mines some week ago. 
I find it really awesome, but after have done some work, I start feeling
bad about using jsx inside my files.

I first try to use vanilla js to build my templates, but find it to be unpractical.

So I thought of moving template markup to separate files.
After all, they compile to vanilla javascript, so it's possible to move them away to their
own commonjs modules, exporting a simple function that return the template markup,
given used parameter as argument.

### The gulp plugin idea

So, after moving some template to its own file, the gulp plugin idea arise.
Each template file has the same structure: it export a function containing jsx,
and I already configure gulp to transpile theme to javascript, using gulp-react plugin.

What if I could automate all boilerplate with a new plugin? What it has to do
is simply take jsx code, wrap it in a function, insert the jsx preable `/** @jsx React.DOM */`
and needed requires for external React widgets.

It appear the last thing was the most difficult to do, because it has to be 
configurable by the user of the plugin.

I think to make it configurable using an option I pass to the plugin
in my `gulp.js` file. This option is an object that contains a key-value pair 
for each tag used: the key is the name of tag, the value is the path to the module.

I then build requires looping though this object. It appar to be practical  enough,
So I start 

