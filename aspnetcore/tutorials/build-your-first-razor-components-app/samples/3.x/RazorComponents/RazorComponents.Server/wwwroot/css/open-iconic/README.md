---
ms.openlocfilehash: 3965884c8e0d7116f5d8a42e6aefb0dc5be94f1e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57064852"
---
<a name="open-iconic-v111httpuseiconiccomopen"></a>[Open Iconic v1.1.1](http://useiconic.com/open)
===========

### <a name="open-iconic-is-the-open-source-sibling-of-iconichttpuseiconiccom-it-is-a-hyper-legible-collection-of-223-icons-with-a-tiny-footprintmdashready-to-use-with-bootstrap-and-foundation-view-the-collectionhttpuseiconiccomopenicons"></a>Open Iconic es el elemento relacionado de código abierto de [Iconic](http://useiconic.com). Es una colección hiperlegible de 223 iconos con una superficie diminuta&mdash;listos para usar con Bootstrap y Foundation. [Vista de la colección](http://useiconic.com/open#icons)



## <a name="whats-in-open-iconic"></a>¿Qué hay en Open Iconic?

* 223 iconos diseñados para ser legibles hasta en 8 píxeles
* Archivos SVG superligeros - 61,8 para todo el conjunto 
* Sprite SVG&mdash;el reemplazo moderno de las fuentes de iconos
* Formatos de fuentes web (EOT, OTF, SVG, TTF, WOFF), PNG y WebP
* Hojas de estilo de fuentes web (incluidas las versiones para Bootstrap y Foundation) en formatos CSS, LESS, SCSS y Stylus
* Imágenes de trama PNG y WebP en 8 px, 16 px, 24 px, 32 px, 48 px y 64 px.


## <a name="getting-started"></a>Introducción

#### <a name="for-code-samples-and-everything-else-you-need-to-get-started-with-open-iconic-check-out-our-iconshttpuseiconiccomopenicons-and-referencehttpuseiconiccomopenreference-sections"></a>Para ver ejemplos de código y todo lo que necesita para comenzar con Open Iconic, consulte las secciones [Iconos](http://useiconic.com/open#icons) y [Referencia](http://useiconic.com/open#reference).

### <a name="general-usage"></a>Uso general

#### <a name="using-open-iconics-svgs"></a>Uso de SVG de Open Iconic

Nos gustan los SVG y creemos que son la forma de mostrar iconos en la web. Dado que Open Iconic consta solo de SVG básicos, le sugerimos que los muestre como lo haría con cualquier otra imagen (no se olvide del atributo `alt`).

```
<img src="/open-iconic/svg/icon-name.svg" alt="icon name">
```

#### <a name="using-open-iconics-svg-sprite"></a>Uso de sprite SVG de Open Iconic

Open Iconic también incluye un sprite de SVG que permite mostrar todos los iconos del conjunto con una sola solicitud. Es como una fuente de iconos, sin ser un método alternativo.

Agregar un icono desde un sprite SVG es algo diferente a lo que está acostumbrado, pero sigue siendo un juego de niños. *Sugerencia: Para que se pueda aplicar fácilmente el estilo a los iconos, le sugerimos que agregue una clase general a la etiqueta* `<svg>` *y un nombre de clase único para cada icono diferente en la etiqueta* `<use>` *.*  

```
<svg class="icon">
  <use xlink:href="open-iconic.svg#account-login" class="icon-account-login"></use>
</svg>
```

Para ajustar el tamaño de los iconos, solo se necesita un CSS básico. Todos los iconos están en formato cuadrado, así que solo tiene que configurar la etiqueta `<svg>` con las mismas dimensiones de ancho y alto.

```
.icon {
  width: 16px;
  height: 16px;
}
```

Dar color a los iconos es incluso más fácil. Todo lo que tiene que hacer es establecer la regla `fill` en la etiqueta `<use>`.

```
.icon-account-login {
  fill: #f00;
}
```

Para más información sobre los sprites SVG, consulte la [guía de Chris Coyier](http://css-tricks.com/svg-sprites-use-better-icon-fonts/).

#### <a name="using-open-iconics-icon-font"></a>Con la fuente Icon de Open Iconic...


##### <a name="with-bootstrap"></a>…con Bootstrap

Puede encontrar nuestras hojas de estilo de Bootstrap en `font/css/open-iconic-bootstrap.{css, less, scss, styl}`


```
<link href="/open-iconic/font/css/open-iconic-bootstrap.css" rel="stylesheet">
```


```
<span class="oi oi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="with-foundation"></a>…con Foundation

Puede encontrar nuestras hojas de estilo Foundation en `font/css/open-iconic-foundation.{css, less, scss, styl}`

```
<link href="/open-iconic/font/css/open-iconic-foundation.css" rel="stylesheet">
```


```
<span class="fi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="on-its-own"></a>…por su cuenta

Puede encontrar nuestras hojas de estilo predeterminadas en `font/css/open-iconic.{css, less, scss, styl}`

```
<link href="/open-iconic/font/css/open-iconic.css" rel="stylesheet">
```

```
<span class="oi" data-glyph="icon-name" title="icon name" aria-hidden="true"></span>
```


## <a name="license"></a>Licencia

### <a name="icons"></a>Iconos

Todo el código (incluido el marcado SVG) está bajo la [licencia MIT](http://opensource.org/licenses/MIT).

### <a name="fonts"></a>Fuentes

Todas las fuentes están bajo la [licencia SIL](http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web).
