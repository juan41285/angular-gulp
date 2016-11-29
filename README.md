# Componentes en Angularjs 1.5

- Metodología para crear componentes con angular 1.5 en estructura de desarrollo basada en gulp

### Fuentes Consultadas
* [google](https://www.google.com.ar)
* [https://docs.angularjs.org/guide/component](https://docs.angularjs.org/guide/component)
* [https://carlosazaustre.es/blog/angular-js-directivas-como-componentes/](https://carlosazaustre.es/blog/angular-js-directivas-como-componentes/)
* [https://github.com/johnpapa/angular-styleguide/blob/master/a1/README.md](https://github.com/johnpapa/angular-styleguide/blob/master/a1/README.md)
* [http://www.desarrolloweb.com/articulos/componentes-angularjs-15.html](http://www.desarrolloweb.com/articulos/componentes-angularjs-15.html)
* [http://teropa.info/blog/2015/10/18/refactoring-angular-apps-to-components.html](http://teropa.info/blog/2015/10/18/refactoring-angular-apps-to-components.html)


****

```
PASO 1

- Estructura de Directorios: Como los componentes son indenpendientes de la aplicacion se agrupa  en una carpeta todos los elementos que necesita el componente para funcionar.

components
 ---searchBar
 	--searchBar.js
 	--searchBar.tpl.html
 	--searchBar.css


- Nombre de los componentes: Los componentes deben ser nombrados utilizando camelCase

```

```
PASO 2

- Adaptar Gulpfile para que procese los componentes.


```
```javascript
// Busca errores en el JS y nos los muestra por pantalla
gulp.task('jshint', function() {
  return gulp.src('./app/scripts/**/*.js','./app/components/**/*.js')
    .pipe(jshint('.jshintrc'))
    .pipe(jshint.reporter('jshint-stylish'))
    .pipe(jshint.reporter('fail'));
});

// Preprocesa archivos Stylus a CSS y recarga los cambios
gulp.task('css', function() {
  gulp.src('./app/css/**/*.css','./app/components/**/*.css')
    .pipe(connect.reload());
});

// Recarga el navegador cuando hay cambios en el HTML
gulp.task('html', function() {
  gulp.src('./app/**/*.html','./app/components/**/*.html')
    .pipe(connect.reload());
});

// Busca en las carpetas de estilos y javascript los archivos
// para inyectarlos en el index.html
gulp.task('inject', function() {
  return gulp.src('index.html', {cwd: './app'})
    .pipe(inject(
      gulp.src(['./app/scripts/**/*.js','./app/components/**/*.js']).pipe(angularFilesort()), {
      read: false,
      ignorePath: '/app'
    }))
    .pipe(inject(
      gulp.src(['./app/css/**/*.css','./src/components/**/*.css']), {
        read: false,
        ignorePath: '/app'
      }
    ))
    .pipe(gulp.dest('./app'));
});

gulp.task('templates2', function() {
  gulp.src(['./app/components/**/*.tpl.html'])
    .pipe(templateCache({
      root: 'components/',
      module: 'app.templates2',
      standalone: true
    }))
    .pipe(gulp.dest('./app/components'));
});

gulp.task('default', ['server', 'templates', 'templates2', 'inject', 'wiredep', 'watch','jshint','css']);
```