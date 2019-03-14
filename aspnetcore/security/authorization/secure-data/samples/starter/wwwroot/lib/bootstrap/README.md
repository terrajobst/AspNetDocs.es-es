---
ms.openlocfilehash: 3be420696add54094f24424285a905e7e7963bad
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57050752"
---
# <a name="bootstraphttpgetbootstrapcom"></a>[Bootstrap](http://getbootstrap.com)

[![Slack](https://bootstrap-slack.herokuapp.com/badge.svg)](https://bootstrap-slack.herokuapp.com)
![Versión de Bower ](https://img.shields.io/bower/v/bootstrap.svg)
[![Versión de npm](https://img.shields.io/npm/v/bootstrap.svg)](https://www.npmjs.com/package/bootstrap)
[![Estado de compilación](https://img.shields.io/travis/twbs/bootstrap/master.svg)](https://travis-ci.org/twbs/bootstrap)
[![Estado de devDependency](https://img.shields.io/david/dev/twbs/bootstrap.svg)](https://david-dm.org/twbs/bootstrap#info=devDependencies)
[![NuGet](https://img.shields.io/nuget/v/bootstrap.svg)](https://www.nuget.org/packages/Bootstrap)
[![Estado de prueba de Selenium](https://saucelabs.com/browser-matrix/bootstrap.svg)](https://saucelabs.com/u/bootstrap)

Bootstrap es un marco front-end elegante, intuitivo y eficaz para un desarrollo web más rápido y sencillo, creado por [Mark Otto](https://twitter.com/mdo) y [Jacob Thornton](https://twitter.com/fat), y de cuyo mantenimiento se encarga el [equipo principal](https://github.com/orgs/twbs/people) con el apoyo y la participación masivos de la comunidad.

Para empezar, eche un vistazo a <http://getbootstrap.com>.


## <a name="table-of-contents"></a>Tabla de contenido

* [Inicio rápido](#quick-start)
* [Errores y solicitudes de características](#bugs-and-feature-requests)
* [Documentación](#documentation)
* [Contribución](#contributing)
* [Community](#community)
* [Control de versiones](#versioning)
* [Creadores](#creators)
* [Copyright y licencia](#copyright-and-license)


## <a name="quick-start"></a>Inicio rápido

Existen varias opciones de inicio rápido:

* [Descargue la última versión](https://github.com/twbs/bootstrap/archive/v3.3.7.zip).
* Clone el repositorio: `git clone https://github.com/twbs/bootstrap.git`.
* Realice la instalación con [Bower](http://bower.io): `bower install bootstrap`.
* Realice la instalación con [npm](https://www.npmjs.com): `npm install bootstrap@3`.
* Realice la instalación con [Meteor](https://www.meteor.com): `meteor add twbs:bootstrap`.
* Realice la instalación con [Composer](https://getcomposer.org): `composer require twbs/bootstrap`.

Lea la [página de introducción](http://getbootstrap.com/getting-started/) para obtener información sobre el contenido, las plantillas y los ejemplos del marco, entre otros aspectos.

### <a name="whats-included"></a>Qué se incluye

En la descarga, encontrará los siguientes directorios y archivos, agrupando recursos comunes de forma lógica y proporcionando variaciones compiladas y reducidas. Verá algo parecido a esto:

```
bootstrap/
├── css/
│   ├── bootstrap.css
│   ├── bootstrap.css.map
│   ├── bootstrap.min.css
│   ├── bootstrap.min.css.map
│   ├── bootstrap-theme.css
│   ├── bootstrap-theme.css.map
│   ├── bootstrap-theme.min.css
│   └── bootstrap-theme.min.css.map
├── js/
│   ├── bootstrap.js
│   └── bootstrap.min.js
└── fonts/
    ├── glyphicons-halflings-regular.eot
    ├── glyphicons-halflings-regular.svg
    ├── glyphicons-halflings-regular.ttf
    ├── glyphicons-halflings-regular.woff
    └── glyphicons-halflings-regular.woff2
```

Proporcionamos CSS y JS compilados (`bootstrap.*`), así como CSS y JS compilados y reducidos (`bootstrap.min.*`). Los [mapas de origen](https://developer.chrome.com/devtools/docs/css-preprocessors) de CSS (`bootstrap.*.map`) están disponibles para usarlos con determinadas herramientas de desarrollo de los exploradores. Se incluyen las fuentes de Glyphicons, ya que se trata de un tema opcional de Bootstrap.


## <a name="bugs-and-feature-requests"></a>Errores y solicitudes de características

¿Tiene un error o una solicitud de característica? Lea primero las [directrices sobre problemas](https://github.com/twbs/bootstrap/blob/master/CONTRIBUTING.md#using-the-issue-tracker) y busque problemas cerrados y existentes. Si su problema o idea no se ha abordado aún, [abra un nuevo problema](https://github.com/twbs/bootstrap/issues/new).

Tenga en cuenta que las **solicitudes de características deben destinarse a [Bootstrap v4](https://github.com/twbs/bootstrap/tree/v4-dev),** porque Bootstrap v3 se encuentra ahora en modo de mantenimiento y no acepta nuevas características. Este es el motivo por el que centramos nuestros esfuerzos en Bootstrap v4.


## <a name="documentation"></a>Documentación

La documentación de Bootstrap, incluida en este repositorio en el directorio raíz, se ha creado con [Jekyll](http://jekyllrb.com) y se hospeda de forma pública en las páginas de GitHub, en <http://getbootstrap.com>. Los documentos también deben ejecutarse localmente.

### <a name="running-documentation-locally"></a>Ejecución local de la documentación

1. Si es necesario, [instale Jekyll](http://jekyllrb.com/docs/installation) y otras dependencias de Ruby con `bundle install`.
   **Nota para usuarios de Windows:** Lectura [en esta guía no oficial](http://jekyll-windows.juthilo.com/) para poner en funcionamiento Jekyll sin problemas.
2. Desde el directorio raíz `/bootstrap`, ejecute `bundle exec jekyll serve` en la línea de comandos.
4. Abra `http://localhost:9001` en el explorador y listo.

Lea esta [documentación](http://jekyllrb.com/docs/home/) para obtener más información sobre el uso de Jekyll.

### <a name="documentation-for-previous-releases"></a>Documentación para versiones anteriores

Por el momento, la documentación de v2.3.2 se encuentra disponible en <http://getbootstrap.com/2.3.2/> mientras los usuarios realizan la transición a Bootstrap 3.

Las [versiones anteriores](https://github.com/twbs/bootstrap/releases) y su documentación también se encuentran disponibles para descarga.


## <a name="contributing"></a>Contribución

Lea las [directrices de contribución](https://github.com/twbs/bootstrap/blob/master/CONTRIBUTING.md). También se incluyen instrucciones para abrir problemas, estándares de codificación y notas de desarrollo.

Además, si la solicitud de incorporación de cambios contiene revisiones o características de JavaScript, debe incluir [pruebas unitarias relevantes](https://github.com/twbs/bootstrap/tree/master/js/tests). Todo el contenido HTML y CSS debe cumplir la [guía de código](https://github.com/mdo/code-guide), de cuyo mantenimiento se encarga [Mark Otto](https://github.com/mdo).

**Bootstrap v3 ahora no acepta nuevas características.** Se encuentra en modo de mantenimiento, por lo que hemos centrado todos nuestros esfuerzos en [Bootstrap v4](https://github.com/twbs/bootstrap/tree/v4-dev), el futuro del marco. Las solicitudes de incorporación de cambios que agrega nuevas características (en lugar de corregir errores) deben destinarse a [Bootstrap v4 (la rama de Git `v4-dev`)](https://github.com/twbs/bootstrap/tree/v4-dev).

Las preferencias del editor se encuentran disponibles en la [configuración del editor](https://github.com/twbs/bootstrap/blob/master/.editorconfig) para que el uso resulte fácil en los editores de texto comunes. Obtenga más información y descargue complementos en <http://editorconfig.org>.


## <a name="community"></a>comunidad

Obtenga actualizaciones sobre el desarrollo de Bootstrap y chatee con los encargados del mantenimiento del proyecto y los miembros de la comunidad.

* Siga a [@getbootstrap en Twitter](https://twitter.com/getbootstrap).
* Lea el [blog oficial de Bootstrap](http://blog.getbootstrap.com) y suscríbase a él.
* Únase a la [sala oficial de Slack](https://bootstrap-slack.herokuapp.com).
* Chatee con los fieles seguidores de Bootstrap en IRC. En el servidor `irc.freenode.net`, en el canal `##bootstrap`.
* La ayuda de implementación se puede encontrar en Stack Overflow (etiquetado como [`twitter-bootstrap-3`](https://stackoverflow.com/questions/tagged/twitter-bootstrap-3)).
* Los desarrolladores deben usar la palabra clave `bootstrap` en los paquetes que se modifican o agregan a la funcionalidad de Bootstrap cuando se distribuye a través de [npm](https://www.npmjs.com/browse/keyword/bootstrap) o de mecanismos de entrega similares para una máxima detectabilidad.


## <a name="versioning"></a>Control de versiones

Para preservar la transparencia en nuestro ciclo de lanzamiento y para tratar de mantener la compatibilidad con versiones anteriores, Bootstrap se atiene a las [directrices de Versionamiento Semántico](http://semver.org/). A veces nos equivocamos, pero nos adheriremos a estas reglas siempre que sea posible.

Vea la [sección de versiones de nuestro proyecto de GitHub](https://github.com/twbs/bootstrap/releases) para obtener información sobre los registros de cambios de cada versión de Bootstrap. Las entradas de anuncios de lanzamiento del [blog oficial de Bootstrap](http://blog.getbootstrap.com) contienen resúmenes de los cambios más importantes realizados en cada versión.


## <a name="creators"></a>Creadores

**Mark Otto**

* <https://twitter.com/mdo>
* <https://github.com/mdo>

**Jacob Thornton**

* <https://twitter.com/fat>
* <https://github.com/fat>


## <a name="copyright-and-license"></a>Copyright y licencia

Copyright de documentación y código 2011-2016 Twitter, Inc. Código publicado bajo la [licencia MIT](https://github.com/twbs/bootstrap/blob/master/LICENSE). Documentos publicados sujetos a la licencia de [Creative Commons](https://github.com/twbs/bootstrap/blob/master/docs/LICENSE).
