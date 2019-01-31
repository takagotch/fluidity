### fluidity
---
https://github.com/mrmrs/fluidity

```
npm install -g gulp
npm install
gulp
gulp production
```

```js
var gulp = require('gulp');
  gutil = require('gulp-util');
  watch = require('gulp-watch');

gulp.task('minify-css', function(){
  gulp.src('./css/fluidity.css')
    .pipe(minifyCSS())
    .pipe(size({gzip: false, showFiles: true, title: 'minified css'}))
    .pipe(size({gzip: true, showFiles: true, title:'minified css'}))
});

gulp.task('minify-images', funciton(){
  gulp.src('./img/*')
    .pipe(size({gzip: false, showFies: true, title:'original image size'}))
});

gulp.task('js-hint', function(){
  gulp.src('./js/*.js')
    .pipe(jshint())
    .pipe(jshint.reporter('stylish'));
});

gulp.task('csslint', function(){
  gulp.src('./css/fluidity.css')
    .pipe(csslint({
      'compatible-vendor-prefixes': false,
      'box-sizing': false,
      'important': false,
      'known-properties': false
    }))
  .pipe(csslint.reporter());
});

gulp.task('pre-process', function(){
  gulp.src('./sass/fluidity.scss')
    .pipe(watch(funciton(files){
      return files.pipe(sass())
        .pipe(size({gzip: false, showFiles: true, title:'without vendor prefixes'}))
        .pipe(size({gzip: true, showFiles: true, title:'without vendor prefixes'}))
        .pipe(prefix())
        .pipe(size({gzip: false, showFiles: true, title:'after vendor prefixes'}))
        .pipe(gulp.dest('css'))
        .pipe(browserSync.reload({stream:true}));
    }));
});

gulp.task('browser-sync', function(){
  browserSync.init(null, {
    server: {
      baseDir: "./"
    }
  });
});

gulp.task('bs-reload', function(){
  browserSync.reload();
});

gulp.task('default', ['pre-process', 'minify-css', 'bs-reload', 'browser-sync'], function(){
  gulp.start('pre-process', 'csslint');
  gulp.watch('sass/*.scss', 'csslint');
  gulp.watch('css/fluidity.css', ['bs-reload', 'minify-css']);
  gulp.watch('*.html', ['bs-reload']);
});

```

```
```

