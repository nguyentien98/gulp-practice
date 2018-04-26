# Bài tập Trainee Colombo 2018

## Bài chuẩn bị 2.6

Tìm hiểu và cài đặt gulp để compile sass sang css

Người thực hiện: [ Tiến Nguyễn ](https://github.com/tiennguyen98)

## Tóm tắt các bước làm trong tài liệu
1. Cài gulp: `npm install gulp -g`
2. Cài module node vào project: `npm init`
3. Cài gulp vào project: `npm install gulp --save-dev`
4. Tạo cấu trúc thư mục
5. Tạo file gulpfile.js rồi nhúng gulp vào `var gulp = require('gulp');`
6. Cài plugin để compile SASS sang CSS: `npm install gulp-sass --save-dev` rồi nhúng plugin vào file js `var sass = require('gulp-sass');`

Thêm vào task sass: 
``` gulp.task('sass', function() {
  return gulp.src('app/scss/**/*.scss') // Gets all files ending with .scss in app/scss
    .pipe(sass())
    .pipe(gulp.dest('app/css'))
    .pipe(browserSync.reload({
      stream: true
    }))
});
```

7. Cài browser.sysc: `npm install browser-sync --save-dev`  nhúng vào `var browserSync = require('browser-sync').create();`

Thêm task watch để tự động thực thi khi có thay đổi trong file:
``` gulp.task('watch', ['browserSync', 'sass'], function (){
  gulp.watch('app/scss/**/*.scss', ['sass']); 
  // Reloads the browser whenever HTML or JS files change
  gulp.watch('app/*.html', browserSync.reload); 
  gulp.watch('app/js/**/*.js', browserSync.reload); 
});
```



8. Cài useref để nén các file sang mục dist: `npm install gulp-useref --save-dev` nhúng `var useref = require('gulp-useref');`

Thêm task:

```
gulp.task('useref', function(){
  return gulp.src('app/*.html')
    .pipe(useref())
    .pipe(gulp.dest('dist'))
});
```


Cài uglify để nén file js: `npm install gulp-uglify --save-dev `

```
var uglify = require('gulp-uglify');
var gulpIf = require('gulp-if');

gulp.task('useref', function(){
  return gulp.src('app/*.html')
    .pipe(useref())
    // Minifies only if it's a JavaScript file
    .pipe(gulpIf('*.js', uglify()))
    .pipe(gulp.dest('dist'))
});
```

Cài cssnano để nén css: `npm install gulp-cssnano` nhúng file `var cssnano = require('gulp-cssnano');`

Thêm vào useref:

```
gulp.task('useref', function(){
  return gulp.src('app/*.html')
    .pipe(useref())
    .pipe(gulpIf('*.js', uglify()))
    // Minifies only if it's a CSS file
    .pipe(gulpIf('*.css', cssnano()))
    .pipe(gulp.dest('dist'))
});
```

Thêm vào html file css/js muốn nén:
```
<!--build:css css/main.min.css-->
<link rel="stylesheet" href="css.css">
<!--endbuild-->
```


Cài imagemin: `npm install gulp-imagemin --save-dev` nhúng `var imagemin = require('gulp-imagemin');`

```
gulp.task('images', function(){
  return gulp.src('app/images/**/*.+(png|jpg|gif|svg)')
  .pipe(imagemin())
  .pipe(gulp.dest('dist/images'))
});
```

Cú pháp để chạy nhiều task trong một lệnh: 

`gulp.task('build', ['sass', 'useref', 'images', 'fonts']);`

## Mục tiêu
* [Link Gitpage](https://tiennguyen98.github.io/gulp-practice/dist/)
* Biết được gulp và cách dùng
* Cấu trúc cơ bản của 1 project html với sass

## Credit
* SublimeText 3
* Chrome
* Git
* [Gulp for Beginners](https://css-tricks.com/gulp-for-beginners)
* [Cấu trúc thư mục cơ bản](http://vth8.com/cau-truc-thu-muc-co-ban-cua-project)