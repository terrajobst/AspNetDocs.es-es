---
title: Less, Sass y Font Awesome en ASP.NET Core
author: ardalis
description: Obtenga información sobre cómo usar Less, Sass y Font Awesome en aplicaciones ASP.NET Core.
ms.author: tdykstra
ms.date: 10/14/2016
uid: client-side/less-sass-fa
ms.openlocfilehash: 2229c4e3b0238ff17c15e78f657b9acb10495c72
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57065562"
---
# <a name="less-sass-and-font-awesome-in-aspnet-core"></a>Less, Sass y Font Awesome en ASP.NET Core

Por [Steve Smith](https://ardalis.com/)

Los usuarios de aplicaciones web tienen expectativas cada vez más altos en cuanto a estilo y la experiencia general. Aplicaciones web modernas aprovechan con frecuencia completas herramientas y marcos de trabajo para definir y administrar su apariencia y comportamiento de una manera coherente. Los marcos de [Bootstrap](http://getbootstrap.com/) puede ir un largo camino para definir un conjunto común de los estilos y opciones de diseño para los sitios web. Sin embargo, la mayoría de los sitios no trivial también se beneficiarán de poder definir de forma eficaz y mantener archivos (CSS) de la hoja de estilos en cascada y estilos, así como tener un acceso fácil a los iconos que no sean image que ayudan a hacer más intuitiva interfaz de la carpeta del sitio. Es ahí donde lenguajes y herramientas que admiten [menos](http://lesscss.org/) y [Sass](http://sass-lang.com/), y las bibliotecas como [Font Awesome](http://fontawesome.io/), vienen en.

## <a name="css-preprocessor-languages"></a>Idiomas de preprocesador de CSS

Los idiomas que se compilan en otros lenguajes, con el fin de mejorar la experiencia de trabajar con el lenguaje subyacente, se conocen como preprocesadores. Hay dos preprocesadores populares de CSS: Menos y Sass.  Estos preprocesadores agregar características a CSS, como la compatibilidad con variables y reglas anidadas, lo que mejora la facilidad de mantenimiento de las hojas de estilo grandes y complejas. CSS como un lenguaje es muy básico, la falta de compatibilidad con incluso algo tan sencillo como variables, y esto tiende a hacer que los archivos CSS recargada y repetitivas. Agregar características de lenguaje de programación real a través de preprocesadores puede ayudar a reducir la duplicación y proporcionar una mejor organización de las reglas de estilo. Visual Studio proporciona compatibilidad integrada para ambos Less y Sass, así como las extensiones que pueden mejorar aún más la experiencia de desarrollo cuando se trabaja con estos lenguajes.

Un ejemplo rápido de cómo preprocesadores pueden mejorar la legibilidad y facilidad de mantenimiento de información de estilo, tenga en cuenta este CSS:

```css
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    color: black;
    font-weight: bold;
    font-size: 14px;
    font-family: Helvetica, Arial, sans-serif;
}
```

Con menor, esto se puede reescribir para eliminar la duplicación, todos con un *mixin* (denominados así porque permite "¡mix" propiedades de una clase o conjunto de reglas a otro):

```less
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    .header;
    font-size: 14px;
}
```

## <a name="less"></a>menos

El preprocesador de CSS menos se ejecuta mediante Node.js. Para instalar menor, usar Node Package Manager (npm) desde un símbolo del sistema (-g significa "global"):

```console
npm install -g less
```

Si usa Visual Studio, puede empezar a trabajar con menos agregando uno o más archivos Less al proyecto y, a continuación, configurando Gulp (o Grunt) para procesarlos en tiempo de compilación. Agregar un *estilos* carpeta al proyecto y, a continuación, agregue un nuevo archivo denominado Less *main.less* a esta carpeta.

![Agregar archivo less](less-sass-fa/_static/add-less-file.png)

Una vez agregado, la estructura de carpetas debe tener un aspecto similar al siguiente:

![Estructura de carpetas](less-sass-fa/_static/folder-structure.png)

Ahora puede agregar algún estilo básico para el archivo, que se compila en CSS y se implementan en la carpeta wwwroot por Gulp.

Modificar *main.less* para incluir el contenido siguiente, que crea una paleta de colores simple desde un único color base.

```less
@base: #663333;
@background: spin(@base, 180);
@lighter: lighten(spin(@base, 5), 10%);
@lighter2: lighten(spin(@base, 10), 20%);
@darker: darken(spin(@base, -5), 10%);
@darker2: darken(spin(@base, -10), 20%);

body {
    background-color:@background;
}
.baseColor  {color:@base}
.bgLight    {color:@lighter}
.bgLight2   {color:@lighter2}
.bgDark     {color:@darker}
.bgDark2    {color:@darker2}
```

`@base` y el otro @-prefixed elementos son variables. Cada una de ellas representa un color. Excepto para `@base`, establecerlos mediante funciones de color: aclarar, oscurecer y rotación. Aclarar y oscurecer hacer prácticamente todo lo que cabría esperar; número ajusta el tono de color por un número de grados (alrededor de la rueda de colores). El procesador de menos es lo suficientemente inteligente como para ignorar las variables que no se utilizan, necesitamos para demostrar cómo funcionan estas variables usarlos en algún lugar. Las clases `.baseColor`, etc. mostrará los valores calculados de cada una de las variables en el archivo CSS que se genera.

### <a name="get-started"></a>Primeros pasos

Crear un **archivo de configuración de npm** (*package.json*) en la carpeta del proyecto y editarlo para hacer referencia a `gulp` y `gulp-less`:

```json
{
  "version": "1.0.0",
  "name": "asp.net",
  "private": true,
  "devDependencies": {
    "gulp": "3.9.1",
    "gulp-less": "3.3.0"
  }
}
```

Instalar las dependencias ya sea en un símbolo del sistema en la carpeta del proyecto o en Visual Studio **el Explorador de soluciones** (**dependencias > npm > Restaurar paquetes**).

```console
npm install
```

![Restauración de paquetes de VS](less-sass-fa/_static/restore-packages.png)

En la carpeta del proyecto, cree un **archivo de configuración de Gulp** (*gulpfile.js*) para definir el proceso automatizado.  Agregue una variable en la parte superior del archivo para representar menos y una tarea se ejecute menor:

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less");

gulp.task("less", function () {
  return gulp.src('Styles/main.less')
    .pipe(less())
    .pipe(gulp.dest('wwwroot/css'));
});
```

Abra el **Task Runner Explorer** (**Vista > otras Windows > Explorador de ejecutores de tareas**). Entre las tareas, verá una nueva tarea denominada `less`. Es posible que deba actualizar la ventana.

Ejecute el `less` tarea y ver un resultado similar al que se muestra aquí:

![menos ejecutor de tareas.](less-sass-fa/_static/less-task-runner.png)

El *wwwroot/css* carpeta contiene ahora un nuevo archivo, *main.css*:

![css principal creado](less-sass-fa/_static/main-css-created.png)

Abra *main.css* y verá algo parecido a lo siguiente:

```css
body {
    background-color: #336666;
}
.baseColor {
    color: #663333;
}
.bgLight {
    color: #884a44;
}
.bgLight2 {
    color: #aa6355;
}
.bgDark {
    color: #442225;
}
.bgDark2 {
    color: #221114;
}
```

Agregar una página HTML sencilla para la *wwwroot* carpeta y referencia *main.css* para ver la paleta de colores en acción.

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <link href="css/main.css" rel="stylesheet" />
    <title></title>
</head>
<body>
    <div>
        <div class="baseColor">BaseColor</div>
        <div class="bgLight">Light</div>
        <div class="bgLight2">Light2</div>
        <div class="bgDark">Dark</div>
        <div class="bgDark2">Dark2</div>
    </div>
</body>
</html>
```

Puede ver que ponga el 180 grados en `@base` utilizado para generar `@background` dieron opuestas color de la rueda de colores `@base`:

![ejemplo menos prueba](less-sass-fa/_static/less-test-screenshot.png)

Menor también proporciona compatibilidad para las reglas anidadas, así como las consultas de medios anidados. Por ejemplo, definir las jerarquías anidadas, como menús pueden dar lugar a reglas CSS detalladas como estos:

```css
nav {
    height: 40px;
    width: 100%;
}
nav li {
    height: 38px;
    width: 100px;
}
nav li a:link {
    color: #000;
    text-decoration: none;
}
nav li a:visited {
    text-decoration: none;
    color: #CC3333;
}
nav li a:hover {
    text-decoration: underline;
    font-weight: bold;
}
nav li a:active {
    text-decoration: underline;
}
```

Lo ideal es que todas las reglas de estilo relacionados se colocarán juntos en el archivo CSS, pero en la práctica que no hay nada aplicar esta regla, excepto la convención y quizás los comentarios del bloque.

Definición de estas reglas con menor tiene este aspecto:

```less
nav {
    height: 40px;
    width: 100%;
    li {
        height: 38px;
        width: 100px;
        a {
            color: #000;
            &:link { text-decoration:none}
            &:visited { color: #CC3333; text-decoration:none}
            &:hover { text-decoration:underline; font-weight:bold}
            &:active {text-decoration:underline}
        }
    }
}
```

Tenga en cuenta que en este caso, todos los elementos subordinados de `nav` están dentro de su ámbito. Ya no hay ninguna repetición de elementos primarios (`nav`, `li`, `a`), y también se ha reducido el recuento total de línea (aunque algunas de que es el resultado de colocar los valores en las mismas líneas en el segundo ejemplo). Puede ser muy útil, una organización ver todas las reglas para un elemento determinado de la interfaz de usuario dentro de un ámbito delimitado explícitamente, en este caso activados del resto del archivo por llaves.

El `&` sintaxis es una característica menos selector, con & que representa el elemento primario del selector actual. Es así, dentro de la una {...} bloque, `&` representa un `a` etiqueta y, por tanto, `&:link` es equivalente a `a:link`.

Las consultas de medios, muy útiles para crear diseños con capacidad de respuesta, también pueden contribuir considerablemente a repeticiones y la complejidad en CSS. Menor permite que las consultas de medios se anida dentro de las clases, por lo que no necesita la definición de clase completa se repite en diferentes nivel superior `@media` elementos. Por ejemplo, aquí es CSS para un menú con capacidad de respuesta:

```css
.navigation {
    margin-top: 30%;
    width: 100%;
}
@media screen and (min-width: 40em) {
    .navigation {
        margin: 0;
    }
}
@media screen and (min-width: 62em) {
    .navigation {
        width: 960px;
        margin: 0;
    }
}
```

Esto se puede definir mejor en menos como:

```less
.navigation {
    margin-top: 30%;
    width: 100%;
    @media screen and (min-width: 40em) {
        margin: 0;
    }
    @media screen and (min-width: 62em) {
        width: 960px;
        margin: 0;
    }
}
```

Otra característica de menos que ya hemos visto es su compatibilidad para operaciones matemáticas, lo que permite a los atributos de estilo se construyera a partir de variables predefinidas. Esto facilita la actualización relacionados estilos mucho más fáciles, ya que se puede modificar la variable de base y todos los valores dependientes cambian automáticamente.

Los archivos CSS, especialmente para los sitios de gran tamaño (y especialmente si se utilizan las consultas de medios), tienden a aumentar bastante grande con el tiempo, lo difícil trabajar con ellas. Archivos less pueden definirse por separado, a continuación, extrae entre sí mediante `@import` directivas. También se puede usar menor para importar CSS archivos individuales, también, si lo desea.

*Mixins* puede aceptar parámetros, y menor es compatible con lógica condicional en forma de guardias mixin, que proporcionan una manera declarativa para definir cuándo determinados mixins surten efecto. Un uso común para las restricciones de mixin es ajustar los colores en función de cómo la luz u oscuro el color de origen. Dado un mixin que acepta un parámetro para el color, un guardia mixin puede usarse para modificar el mixin según ese color:

```less
.box (@color) when (lightness(@color) >= 50%) {
    background-color: #000;
}
.box (@color) when (lightness(@color) < 50%) {
    background-color: #FFF;
}
.box (@color) {
    color: @color;
}

.feature {
    .box (@base);
}
```

Según nuestro actual `@base` valor de `#663333`, esta secuencia de comandos menos generará el código CSS siguiente:

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

Menor proporciona una serie de características adicionales, pero esto debería proporcionarle una idea de la eficacia de este lenguaje de preprocesamiento.

## <a name="sass"></a>SASS

SASS es similar a un valor inferior, que proporciona compatibilidad con muchas de las mismas características, pero con una sintaxis ligeramente diferente. Se genera mediante Ruby, en lugar de JavaScript y, por lo que tiene requisitos de configuración diferentes. El lenguaje Sass original no usa llaves o punto y coma, sino que define el ámbito mediante espacios en blanco y sangría. En la versión 3 de Sass, se introdujo una nueva sintaxis, **SCSS** ("Sassy CSS"). SCSS es similar a CSS, ya que omite los espacios en blanco y los niveles de sangría y usa en su lugar, puntos y comas y llaves.

Para instalar Sass, normalmente se haría instala primero Ruby (preinstalado en Mac OS) y, a continuación, ejecute:

```console
gem install sass
```

Sin embargo, si ejecuta Visual Studio, puede comenzar con Sass prácticamente la misma manera como lo haría con menos. Abra *package.json* y agregar el paquete "gulp-sass" `devDependencies`:

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

A continuación, modifique *gulpfile.js* para agregar una variable sass y una tarea para compilar los archivos Sass y colocar los resultados en la carpeta wwwroot:

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less"),
  sass = require("gulp-sass");

// other content removed

gulp.task("sass", function () {
  return gulp.src('Styles/main2.scss')
    .pipe(sass())
    .pipe(gulp.dest('wwwroot/css'));
});
```

Ahora puede agregar el archivo Sass *main2.scss* a la *estilos* carpeta en la raíz del proyecto:

![Agregar archivo de scss](less-sass-fa/_static/add-scss-file.png)

Abra *main2.scss* y agregue lo siguiente:

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

Guarde todos los archivos. Ahora, cuando actualice **Task Runner Explorer**, verá un `sass` tarea. Ejecútelo y buscar en el */wwwroot/css* carpeta. Ahora hay un *main2.css* archivo con este contenido:

```css
body {
    background-color: #CC0000;
}
```

SASS admite el anidamiento prácticamente lo mismo era que menor no, lo que proporciona ventajas similares. Los archivos pueden dividirse por función e incluido mediante el `@import` directiva:

```sass
@import 'anotherfile';
```

SASS admite el uso de mixins así, la `@mixin` palabra clave para definirlos y `@include` para incluirlos, como en este ejemplo de [sass-lang.com](http://sass-lang.com):

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

Además de mixins, Sass también admite el concepto de herencia, lo que permite una clase puede ampliar otra. Es conceptualmente similar a un mixin, aunque los resultados en menos código CSS. Se logra mediante el `@extend` palabra clave. Para probar mixins, agregue lo siguiente a su *main2.scss* archivo:

```sass
@mixin alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @include alert;
    border-color: green;
}

.error {
    @include alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

Examine la salida en *main2.css* después de ejecutar el `sass` de tareas en **Task Runner Explorer**:

```css
.success {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    border-color: green;
 }

.error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    color: red;
    border-color: red;
    font-weight: bold;
}
```

Tenga en cuenta que todas las propiedades comunes de la alerta mixin se repiten en cada clase. El mixin hizo un buen trabajo para ayudar a eliminar la duplicación en tiempo de desarrollo, pero todavía se está creando CSS con una gran cantidad de duplicación, lo que produce mayores que los archivos necesarios de CSS - un posible problema de rendimiento.

Ahora, reemplace el mixin de alerta con un `.alert` clase y cambiar `@include` a `@extend` (teniendo en cuenta al ampliar `.alert`, no `alert`):

```sass
.alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @extend .alert;
    border-color: green;
}

.error {
    @extend .alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

Ejecute una vez más Sass y examine el CSS resultante:

```css
.alert, .success, .error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    border-color: green;
}

.error {
    color: red;
    border-color: red;
    font-weight: bold;
}
```

Ahora las propiedades se definen sólo como tantas veces como sea necesario y mejor se genera el CSS.

SASS también incluye funciones y operaciones de lógica condicional, similares a menor. De hecho, las capacidades de los dos lenguajes son muy similares.

## <a name="less-or-sass"></a>¿Menor o Sass?

Todavía no hay ningún consenso sobre si es suele ser mejor usar una cantidad menor o Sass (o incluso si se prefiere el Sass original o la sintaxis SCSS más reciente en Sass). Probablemente la decisión más importante es **utilizar una de estas herramientas**en lugar de simplemente-codificación manual los archivos CSS. Una vez que haya realizado que la decisión, ambos Less y Sass son opciones válidas.

## <a name="font-awesome"></a>Font Awesome

Además de los preprocesadores CSS, otro excelente recurso para las aplicaciones web modernas de estilo es Font Awesome. Font Awesome es un Kit de herramientas que ofrece más de 500 iconos vectoriales escalables que pueden usarse libremente en sus aplicaciones web. Se diseñó originalmente para trabajar con Bootstrap, pero no tiene dependencias en ese marco de trabajo o en las bibliotecas de JavaScript.

La manera más fácil empezar a trabajar con Font Awesome consiste en Agregar una referencia a él, mediante su ubicación de entrega de contenido público network (CDN):

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

También puede agregarlo al proyecto de Visual Studio, éste se agrega a la sección "dependencias" en *bower.json*:

```json
{
  "name": "ASP.NET",
  "private": true,
  "dependencies": {
    "bootstrap": "3.0.0",
    "jquery": "1.10.2",
    "jquery-validation": "1.11.1",
    "jquery-validation-unobtrusive": "3.2.2",
    "hammer.js": "2.0.4",
    "bootstrap-touch-carousel": "0.8.0",
    "Font-Awesome": "4.3.0"
  }
}
```

Una vez que tenga una referencia a Font Awesome en una página, puede agregar iconos a la aplicación aplicando clases Font Awesome, normalmente el prefijo "fa-", para los elementos HTML en línea (como `<span>` o `<i>`).  Por ejemplo, puede agregar iconos para listas simples y los menús con código similar al siguiente:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title></title>
    <link href="lib/font-awesome/css/font-awesome.css" rel="stylesheet" />
</head>
<body>
    <ul class="fa-ul">
        <li><i class="fa fa-li fa-home"></i> Home</li>
        <li><i class="fa fa-li fa-cog"></i> Settings</li>
    </ul>
</body>
</html>
```

Esto produce lo siguiente en el explorador: observe el icono junto a cada elemento:

![iconos de la lista](less-sass-fa/_static/list-icons-screenshot.png)

Puede ver una lista completa de los iconos disponibles aquí:

http://fontawesome.io/icons/

## <a name="summary"></a>Resumen

Aplicaciones web modernas demandan cada vez más fluidos, con capacidad de respuesta diseños que se limpia, intuitiva y fácil de usar desde una variedad de dispositivos. Administrar la complejidad de las hojas de estilo CSS necesarios para lograr estos objetivos se realiza mejor mediante un me gusta preprocesador menos o Sass. Además, kits de herramientas, como Font Awesome rápidamente proporcionan iconos conocidos para los menús de navegación textual y experimentan de los botones, mejorar la global del usuario de la aplicación.
