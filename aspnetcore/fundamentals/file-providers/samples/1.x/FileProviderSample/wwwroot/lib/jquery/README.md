---
ms.openlocfilehash: 610b16b181422978dc143b95f28658bbdf177fdd
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57028282"
---
# <a name="jquery"></a>jQuery

> jQuery es una biblioteca de JavaScript rápida, pequeña y repleta de características.

Para información sobre cómo empezar a trabajar con jQuery y cómo usarla, consulte la [documentación de jQuery](http://api.jquery.com/).
Para archivos de código fuente y problemas, visite el [repositorio de jQuery](https://github.com/jquery/jquery).

Si está actualizando, consulte la [entrada de blog para 3.3.1](https://blog.jquery.com/2017/03/20/jquery-3.3.1-now-available/). Esto incluye importantes diferencias con respecto a la versión anterior y un registro de cambios más legible.

## <a name="including-jquery"></a>Inclusión de jQuery

A continuación se muestran algunas de las formas más comunes para incluir jQuery.

### <a name="browser"></a>Explorador

#### <a name="script-tag"></a>Etiqueta script

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
```

#### <a name="babel"></a>Babel

[Babel](http://babeljs.io/) es un compilador de JavaScript de la siguiente generación. Una de las características es la capacidad para usar los módulos ES6/ES2015 ahora, aunque los exploradores aún no admiten esta característica de forma nativa.

```js
import $ from "jquery";
```

#### <a name="browserifywebpack"></a>Browserify y Webpack

Hay varias formas de usar [Browserify](http://browserify.org/) y [Webpack](https://webpack.github.io/). Para más información sobre el uso de estas herramientas, consulte la documentación del proyecto correspondiente. En el script, la inclusión de jQuery normalmente será similar a esto...

```js
var $ = require("jquery");
```

#### <a name="amd-asynchronous-module-definition"></a>AMD (definición asincrónica de módulos)

AMD es un formato de módulo creado para el explorador. Para más información, recomendamos la [documentación de require.js](http://requirejs.org/docs/whyamd.html).

```js
define(["jquery"], function($) {

});
```

### <a name="node"></a>Nodo

Para incluir jQuery en [Node](nodejs.org), instale primero con npm.

```sh
npm install jquery
```

Para que JQuery funcione en Node, se requiere una ventana con un documento. Puesto que no existe tal ventana de forma nativa en Node, se puede simular una mediante herramientas como [jsdom](https://github.com/tmpvar/jsdom). Esto puede ser útil para fines de prueba.

```js
require("jsdom").env("", function(err, window) {
    if (err) {
        console.error(err);
        return;
    }

    var $ = require("jquery")(window);
});
```
