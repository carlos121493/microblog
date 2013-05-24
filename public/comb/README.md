# 

A browser-side script loader,compatible with the de facto standards(NodeJS/CommonJS).
It's very simple and small,only 2.3kb gzipped.

---

## public api for browser

### define

```js
//path/to/file.js
define('path/to/file',function(require,exports,module){
  var file = 'path/to/file.js';
  exports.filename = 'file.js';
  exports.filedir = 'path/to/';
  /**
   * module.exports = {
   *    filename:'file.js',
   *    filedir:'path/to'
   * };
   */
});
```

### require

```js
//path/to/app.js
define('path/to/app',function(require,exports,module){
  var file = require('path/to/file');
  console.log(file.filename); //file.js
  console.log(file.filedir) //path/to
  module.exports = 'i am app.js';
});
```
### lithe.use

```js
//anywhere
lithe.use('path/to/app',function(app){
    console.log(app); // i am app.js
});
//or
lithe.use('a.js','b.js',function(a,b){
  //a and b has required;
});
```

### config

```js
define('config',function(require,exports,module){
   module.exports = {
      base:'http://localhost/debug/path', 
      //it will replace the real BASEPATH
      timestamp:new Date().valueOf(), 
      //for debug or update timestamp ? All javascript modules will be used
      alias:{
        'app':'path/to/app',
        'file':'path/to/file'
      }
      //logogram
   };
});

define('someOtherJs',function(){
  var app = require('app'),
  file = require('file');
  //now the path/to/app and path/to/file has required
});
```

## public api for node

```js
//npm install lithe
var lithe = require('lithe'),
tool = lithe.tool,
hfs = lithe.hfs,
options = tool.options;

options.basepath = 'your project base dir';
options.uglifyPath = 'your uglifyjs dir';
options.alias = {}; //your short alias config

```

### tool.findJsAllrequires([filepath]) 

```js
var requires = tool.findJsAllrequires('../app.js');
//app.js's requires are findout and alias will be replaced  
```
### tool.concatFile([files],[target])

```js
tool.concatFile(['/path/to/file1.js','/path/to/file2.js'],'/path/to/file1&file2.js');
//file1 and file2 will be merger
```

### tool.uglifyJs([filepath],[target])

```js
tool.uglifyJs('/path/to/file1&file2.js','path/to/file1&file2-min.js');
//Equivalent uglifyjs --reserved-names require -o path/to/file1&file2-min.js /path/to/file1&file2.js
```

### hfs.cpdirSync([sourceDir],[targetDir])

```js
hfs.cpdirSync('/path/to/dir1','/path/to/dir2');
//copy the folder by sync,if targetDir not exist it will be created.
//.git and .svn will be continue.
```

### hfs.delSync([path])

```js
hfs.delSync('/path/to/dir');
hfs.delSync('/path/to/file');
//del the folder or file sync
//.git and .svn will be continue.
```

### hfs.mkdirSync([target])

```js
hfs.mkdirSync('/path/dir')
//if the '/path' folder not exist,it will be created.
```

### hfs.walk([path],[callback],[options])

```js
hfs.walk('/path/',function(files){
    console.log(files); 'return path folder all js files';    
},{
    filter:function(file){
	  if (path.extname(el).indexOf('.js') > - 1) return true;
    }
});
//it's worked sync
```

### hfs.writeFileSync([filepath],[data],[encoding = utf8])

```js
hfs.writeFileSync('/path/file',"abcd");
//if path folder not exist,it will be created.
```

## How to deploy in the web browser��

```html
<script src="/lithe.js"
        data-config="/config.js"
        data-path="http://domain.com/"
        data-main="app.js">
</script>
```
## License

BSD license


