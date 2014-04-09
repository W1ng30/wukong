# Wukong [*悟空*] [![Build Status](https://travis-ci.org/fundon/wukong.svg)](https://travis-ci.org/fundon/wukong)

[Metalsmith][] inspired, pluggable static site generator using generators via [co][] and [co-ware][].

In Wukong (likes in Metalsmith)

```js
Wukong(__dirname)
  .use(function *() {
    var css = '', file;

    // Input files.
    var files = this.files;

    for (var name in files) {
      if ('.css' != extname(name)) continue;
      css += files[name].contents.toString();
      delete files[name];
    }

    css = myth(css);

    // Output index.css
    this.files['index.css'] = {
      contents: new Buffer(css)
    };

    yield next;
  })
  .build();
```

### Plugins

Convert Metalsmith's plugin into Wukong's plugin

#### Example

```js
var thunkify = require('thunkify');
var permalinks = require('metalsmith-permalinks');

module.exports = function (options) {
  var links = thunkify(permalinks(options));
  return function *permalinks(next) {
    yield links(this.files, this.wukong);
    yield next;
  };
};
```


[co]: https://github.com/visionmedia/co
[co-ware]: https://github.com/fundon/co-ware
[metalsmith]: https://github.com/segmentio/metalsmith
