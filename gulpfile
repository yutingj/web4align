var gulp=require('gulp');
var jshint=require('gulp-jshint');//js代码校验
var uglify=require('gulp-uglify'); //js压缩
var mincss=require('gulp-clean-css');//css压缩
var inline=require('gulp-inline-source');//js、css内联
var include=require('gulp-include');//html嵌入到html
var spritesmith=require('gulp.spritesmith');//雪碧图
var concat = require('gulp-concat');//js合并
var imagemin=require('gulp-imagemin');//图片合成

var sequence=require('gulp-sequence');
var useref=require('gulp-useref');
var gulpif=require('gulp-if');
var print=require('gulp-print');
var connect=require('gulp-connect');

//js代码校验

gulp.task('lint', function() {
    return gulp.src('./src/js/*.js')
        .pipe(jshint())
        .pipe(jshint.reporter('default'));
});


//css压缩
gulp.task('mincss',function () {
    return gulp.src('./src/css/*.css')
        .pipe(mincss())
        .pipe(gulp.dest('dist/css'))
});
//js压缩
gulp.task('minjs',function () {
    return gulp.src('./src/js/*.js')
        .pipe(uglify())
        .pipe(gulp.dest('dist/js'))
});
//图片压缩
gulp.task('images', function() {
  return gulp.src('images/*')
    .pipe(cache(imagemin({ optimizationLevel: 3, progressive: true, interlaced: true })))
    .pipe(gulp.dest('images'))
    .pipe(notify({ message: 'Images task complete' }));
});

//雪碧图
gulp.task('sprite', function () {
    return gulp.src('images/*.png')         //需要合并的图片地址
        .pipe(spritesmith({
            imgName: 'sprite.png',//保存合并后图片的地址
            cssName: 'css/sprite.css',//保存合并后对于css样式的地址
            padding:5,//合并时两个图片的间距
            algorithm: 'binary-tree',
            cssTemplate:"css/handlebarsStr.css"
        }))
        .pipe(gulp.dest('dist/'));
});
//js合并
gulp.task('scripts', function() {
    return gulp.src('./src/js/*.js')
        //合并js文件
        .pipe(concat('all.js'))
        //给文件添加.min后缀
        .pipe(rename({ suffix: '.min' }))
});
//html、js、css内联

gulp.task('html', function () {
    return gulp.src('./src/*.html')
        .pipe(inline())
        .pipe(include())
        .pipe(print())
        .pipe(useref())
        .pipe(gulpif('*.js',uglify()))
        .pipe(gulpif('*.css',mincss()))
        .pipe(connect.reload())
        .pipe(gulp.dest('dist'));
});
//本地服务器  支持自动刷新页面
gulp.task('connect', function() {
    connect.server({
        root: './dist', //本地服务器的根目录路径
        port:8080,
        livereload: true
    });
});
gulp.task('watchlist',function (cb) {
    sequence('clean',['mincss','minjs','html'])(cb)
});

gulp.task('watch',function () {
    gulp.watch(['./src/**'],['watchlist']);
});
gulp.task('default',function (cb) {
    sequence(['lint','images','scripts','mincss','minjs','html','connect'],'watch')(cb)
});