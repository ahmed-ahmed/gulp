# gulp
## adding gulp to the project
```sh
npm install -g gulp --save-dev
touch gulpfile.js
```

## first task 
add the below code to the gulpfile.js   
```javascript
var gulp  = require('gulp');

gulp.task('hello-world', function() {
    console.log('This is the first task');
}
```
to run the task type the this command
```sh
gulp hello-world
```
## adding code styling and code analysis
js hint : code analysis  - .jshintrc
js cs: code style for java script - 

installing jshint and jscs
```sh
npm install -g jshint
npm install jshint gulp-jshint gulp-jscs jshint-stylish --save-dev
npm install gulp-util       #used for logging and other staff
```

```js
var gulp = require('gulp');
var jscs = require('gulp-jscs');
var jshint = require('gulp-jshint');
var util = require('gulp-util');

gulp.task('vet', function () { 	//vetting out our code
	gulp.src(['./src/**/*.js'])        
	.pipe(jscs())
	.pipe(jshint())
	.pipe(jshint.reporter('jshint-stylish', {verbose:true}));  //using reporter
	
});
```
### adding logging to our tasks 
```sh
npm install gulp-util       #used for logging and other staff
```
The code after adding the logging
```js
var gulp = require('gulp');
var jscs = require('gulp-jscs');
var jshint = require('gulp-jshint');
var util = require('gulp-util');

gulp.task('vet', function () { 	//vetting out our code
    log('Analyzing the source code using jscs and jshint');
    gulp.src(['./src/**/*.js'])        
	.pipe(jscs())
	.pipe(jshint())
	.pipe(jshint.reporter('jshint-stylish', {verbose:true}));  //using reporter
	
});

//loggin
function log(msg) {
    if(typeof(msg) === 'object'){
    }
    else{
        util.log(util.colors.blue(msg));
    }
}
```
### fail the task if jshint error exists
```javascript 
gulp.task('vet', function () {  //vetting out our code
    gulp.src(['./src/**/*.js'])        
    .pipe(jscs())
    .pipe(jshint())
    .pipe(jshint.reporter('jshint-stylish', {verbose:true}))
    .pipe(jshint.reporter('fail'));             //fail 
```

### printing the files names 
if you want to print all the file names that are piped to make sure that all files are included 

```sh 
npm install --save-dev gulp-print
```
```javascript 
var gulp = require('gulp');
var gulpPrint = require('gulp-print');
gulp.task('vet', function () {  //vetting out our code
    gulp.src(['./src/**/*.js'])
    .pipe(gulpPrint());
```
you may want to print the files only for debugging so you will need to use gulp-if and yargs

```sh 
npm install --save-dev yargs gulp-if
```

```javascript
var gulp = require('gulp');
var args = require('yargs').argv;
var gulpif = require('gulp-if');

gulp.task('vet', function () { 	//vetting out our code
	gulp.src(['./src/**/*.js'])
	.pipe(gulpif(args.verbose,gulpprint()));
});
```
### getting all plugins 
instead of create a variable for each plugin we will use the "load-all-plugin" to help us with this teadous task
```sh
npm install --save-dev gulp-load-plugins
```
```javascript
var gulp = require('gulp');
var args = require('yargs').argv;
var $ = require('gulp-load-plugins')({lazy: true});     //lazy: load plugin ondemand

// the convention is to use the name of the plugin after removing "gulp"
// gulp-jshint will be $.jshint | gulpprint will be $.print
gulp.task('vet', function () { 	//vetting out our code
	log('Analyzing the source code using jscs and jshint');
	gulp.src(['./src/**/*.js'])
	.pipe($.if(args.verbose,$.print()))
	.pipe($.jscs())
	.pipe($.jshint())
	.pipe($.jshint.reporter('jshint-stylish', {verbose:true}))
	.pipe($.jshint.reporter('fail'));
	
});
```

### organizing the files 
you need to organize your gulp file and not to use the majic string and use configurable options instead

```sh 
touch gulp.config.js
```

gulp.config.js
```javasctipt
var confing = {
    alljs = ['./src/**/*.js']
}
module.exports = config;
```
```javascript
var gulp = require('gulp');
var args = require('yargs').argv;
var $ = require('gulp-load-plugins')({lazy: true});     //lazy: load plugin ondemand
var config = require('./gulp.config');
// the convention is to use the name of the plugin after removing "gulp"
// gulp-jshint will be $.jshint | gulpprint will be $.print
gulp.task('vet', function () { 	//vetting out our code
	log('Analyzing the source code using jscs and jshint');
	gulp.src(config.alljs)
	.pipe($.if(args.verbose,$.print()))
	.pipe($.jscs())
	.pipe($.jshint())
	.pipe($.jshint.reporter('jshint-stylish', {verbose:true}))
	.pipe($.jshint.reporter('fail'));
	
});
```


















