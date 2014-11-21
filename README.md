## ES6 Browserify Boilerplate

This is an boilerplate repo to make it easy to experiment with [ES6]. It also includes some additional features, such as annotations and run-time type checks. It's inspired by [angular-es6-boilerplate](https://github.com/davidjnelson/angular-es6-boilerplate) but transpiles modules to CommonJS syntax instead and bundles files with [Browserify](http://browserify.org/)


### Initial setup

```bash
# Clone the repo...
git clone https://github.com/thoughtram/es6-browserify-boilerplate.git.git
cd es6-browserify-boilerplate

# Then, you need to install all the dependencies...
npm install

# If you wanna be able to use global commands `karma` and `gulp`...
npm install -g gulp
```

### Running in the browser
```bash
gulp build
gulp serve

# If you wanna Gulp to re-build on every change...
gulp watch
```


### WTF is ES6?
Simply, the next version of JavaScript that contains some really cool features. You might check out some of these:

- https://wiki.mozilla.org/ES6_plans
- http://globaldev.co.uk/2013/09/es6-part-1/
- http://code.tutsplus.com/tutorials/eight-cool-features-coming-in-es6--net-33175


### What are all the pieces involved?

#### [Traceur]
Transpiles ES6 code into regular ES5 (today's JavaScript) so that it can be run in a today browser.

#### [CommonJS]
Traceur is configured to transpile ES6 modules into CommonJS syntax and we use browserify to bundle the code into one file to deliver it to the browser.

#### [Browserify]
Browserify walks through all files and traces down all `require()`s to bundle all files together.  

#### [Gulp]
Task runner to make defining and running the tasks simpler.

#### 1/ meta data annotations
```js
class SomeAnnotation {}
class AnotherAnnotation {}

@SomeAnnotation
class Foo {
  constructor(@AnotherAnnotation bar) {}
}
```
This is a very similar syntax to annotations in Java/Dart. It is just a nice declarative way to put additional meta data on classes/functions/parameters.

When `annotations: true`, Traceur transpiles the above code code into something like this:
```js
// ...

Foo.annotations = [new SomeAnnotation];
Foo.parameters = [[new AnotherAnnotation]];
```

Therefore you can easily achieve the same thing without transpiling the code. It just won't be as pretty ;-)

#### 2/ type annotations
```js
function request(url: String, data: Object, callback: Function) {
  // ...
}
```

The syntax is more-less the same as [TypeScript].

When `types: true, annotations: true`, Traceur transpiles this code into something like this:
```js
function request(url, data, callback) {
  // ...
}

// this code might change
request.parameters = [[String], [Object], [Function]];
```

When also `typeAssertions: true`, Traceur generates run-time assertions, such as:
```js
function request(url, data, callback) {
  assert.argumentTypes(
    url, String,
    data, Object,
    callback, Function
  );
}
```


[ES6]: http://wiki.ecmascript.org/doku.php?id=harmony:specification_drafts
[Traceur]: https://github.com/google/traceur-compiler
[CommonJS]: http://wiki.commonjs.org/wiki/CommonJS
[Browserify]: http://browserify.org/
[Gulp]: http://gulpjs.com/

