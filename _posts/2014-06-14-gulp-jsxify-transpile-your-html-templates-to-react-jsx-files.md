* Find it on [npmjs here](https://www.npmjs.org/package/gulp-jsxify)
* To install it: `npm install --save-dev gulp-jsxify`
* Usage: 


```js
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

I then build requires looping though this object. It appear to be practical  enough,
so I wrote the plugin, edit my gulp file, and start to move my templates code to 
html.

### First problem: circular dependency

The requires approach has a problem. It require configured React widgets in all
templates, even if not used. When you start to have a bunch of widgets, this start to cause
circular dependency, breaking my project.

So, I solve this problem by checking each file code for used tags, and only include
used one. It was easy, because each tag match a key in my plugin option.

Good, circular dependency resolved!


### Second problem: React handle all html5 tags

I lied, it was not resolved: in first iteration, I always include React as a default 
dependency in produced js files, because it is used to traspile all html5 tags.

This was fine, by it is now causing problem, because I now have a lot of tag that 
doesn't directly map to a key in my option file.

So I revert to check if a tag is one of html5 or a custom one:
To do this, I used the practical [html-tags npm module, by Sindre Sorhus](http://github.com/sindresorhus/html-tags)
that exports an array containing all defined html5 elements tags.

## Conclusion

Ok, it finally works. The plugin is now published on NPM, I hope that others will find it useful...

By the way, here is my project using it: [github.com/parroit/billpanel](https://github.com/parroit/billpanel)
you'll find there all the html templates, and the gulp file I used to generate them.








