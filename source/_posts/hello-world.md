---
title: Hello World! My first hexo post! (hexo的常用命令及优化)
date: 2018-10-18 17:02:06
tags: [杂类,Hello world]
categories: 奇怪的东西
---

# 关于这里

哈哈哈，第一次尝试用Hexo来搭建一个博客，放在github page上，尝试了一下。**以后这里主要用来记录我的学习笔记和一些探索总结吧**。

话说觉得Hexo真的好神奇，脱离了数据库，用一个个md文档充当文章的内容，在本地写好md文档后编译成Html,再推上服务器。实现了纯静态的访问，蛮有意思的XD

### 那就这样吧，接下来开始我的记录咯。

``` bash
$ hexo new "My First Note！"
```

个人博客: [我们的小确幸](https://we.sharelove.site/)
# hexo的常用命令
```bash
# 新建文章
$ hexo new [layout] <title>
# 生成静态文件
$ hexo g
# 部署网站
$ hexo d
# 清除缓存文件 (db.json) 和已生成的静态文件 (public)。
$ hexo clean
```
# hexo的优化
虽然hexo已经很优秀了，但考虑到毕竟是放在github page上的，还是稍微做一些优化。
## 静态资源优化
主要是压缩html,css,js等等静态资源，可以适当减少请求的数据量，主要用到gulp来实现。
1. 安装gulp

```bash
# 全局安装gulp工具 以yarn工具为例
$ yarn global add gulp-cli
# 给项目安装各种gulp插件 分别负责压缩html 图片 css js
$ yarn add gulp gulp-htmlclean gulp-htmlmin gulp-imagemin gulp-minify-css gulp-uglify
```

2. 配置文件

在根目录下添加`gulpfile.js`

```js
var gulp = require('gulp');
var minifycss = require('gulp-minify-css');
var uglify = require('gulp-uglify');
var htmlmin = require('gulp-htmlmin');
var htmlclean = require('gulp-htmlclean');
var imagemin = require('gulp-imagemin');

// 压缩html
gulp.task('minify-html', function() {
    return gulp.src('./public/**/*.html')
        .pipe(htmlclean())
        .pipe(htmlmin({
            removeComments: true,
            minifyJS: true,
            minifyCSS: true,
            minifyURLs: true,
        }))
        .pipe(gulp.dest('./public'))
});
// 压缩css
gulp.task('minify-css', function() {
    return gulp.src('./public/**/*.css')
        .pipe(minifycss({
            compatibility: 'ie8'
        }))
        .pipe(gulp.dest('./public'));
});
// 压缩js
gulp.task('minify-js', function() {
    return gulp.src('./public/js/**/*.js')
        .pipe(uglify())
        .pipe(gulp.dest('./public'));
});
// 压缩图片
gulp.task('minify-images', function() {
    return gulp.src('./public/img/**/*.*')
        .pipe(imagemin(
        [imagemin.gifsicle({'optimizationLevel': 3}), 
        imagemin.jpegtran({'progressive': true}), 
        imagemin.optipng({'optimizationLevel': 7}), 
        imagemin.svgo()],
        {'verbose': true}))
        .pipe(gulp.dest('./public/img'))
});

//4.0以前的写法 
//gulp.task('default', [
  //  'minify-html', 'minify-css', 'minify-js'，'minify-images'
//]);

// 默认任务 gulp4以上写法 gulp.parallel 任务同时进行
gulp.task('default', gulp.parallel(
    'minify-html',
    'minify-css',
    'minify-js',
    'minify-images'
));
```

3. 在hexo g生成文件后运行gulp
```bash
$ hexo g
$ gulp
$ hexo d
```
## 生成sitemap文件
生成sitemap文件然后提交给搜索引擎，对于SEO很有帮助，hexo有相关的sitemap插件。

安装这俩插件后，以后每次hexo g都会生成sitemap.xml和baidusitemap.xml文件并自动帮你放到public目录。

```bash
$ yarn add hexo-generator-sitemap hexo-generator-baidu-sitemap
```