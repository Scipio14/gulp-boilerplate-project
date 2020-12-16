# Prueba

Esta es una prueba dos para ver si funciona la configuración del **gulp** task manager.

1. Hay que probarlo
2. Hay que saborearlo
3. Hay que olerlo bien

## La configuración que se obtine es:

```
const gulp = require("gulp");
const sass = require("gulp-sass");
const markdown = require("gulp-markdown");
const browserSync = require("browser-sync").create();

//compile scss into css

function style() {
  //1. Where is my scss file
  return (
    gulp
      .src("./scss/**/*.scss")

      //2. Pass that file through sass compiler
      .pipe(sass())
      //3.Where do I save my compiled css?
      .pipe(gulp.dest("./css"))
      //4. Stream changes to all browsers
      .pipe(browserSync.stream())
  );
}

async function md() {
  await gulp
    .src("./markdown/**/*.md")
    //
    .pipe(markdown())
    //
    .pipe(gulp.dest("./articles"))
    .pipe(browserSync.stream());
}

function watch() {
  browserSync.init({
    server: {
      baseDir: "./",
    },
    port:4000
  });
  gulp.watch("./scss/**/*.scss", style);
  gulp.watch("./markdown/**/*.md", md);
  gulp.watch("./*.html").on("change", browserSync.reload);
  gulp.watch("./js/**/*.js").on("change", browserSync.reload);
}

exports.style = style;
exports.md = md;
exports.watch = watch;

```
