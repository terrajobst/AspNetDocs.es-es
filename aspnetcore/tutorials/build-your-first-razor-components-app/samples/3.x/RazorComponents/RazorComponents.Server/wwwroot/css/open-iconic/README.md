---
ms.openlocfilehash: 3965884c8e0d7116f5d8a42e6aefb0dc5be94f1e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57064852"
---
<a name="open-iconic-v111httpuseiconiccomopen"></a>[<span data-ttu-id="b4f4d-101">Open Iconic v1.1.1</span><span class="sxs-lookup"><span data-stu-id="b4f4d-101">Open Iconic v1.1.1</span></span>](http://useiconic.com/open)
===========

### <a name="open-iconic-is-the-open-source-sibling-of-iconichttpuseiconiccom-it-is-a-hyper-legible-collection-of-223-icons-with-a-tiny-footprintmdashready-to-use-with-bootstrap-and-foundation-view-the-collectionhttpuseiconiccomopenicons"></a><span data-ttu-id="b4f4d-102">Open Iconic es el elemento relacionado de código abierto de [Iconic](http://useiconic.com).</span><span class="sxs-lookup"><span data-stu-id="b4f4d-102">Open Iconic is the open source sibling of [Iconic](http://useiconic.com).</span></span> <span data-ttu-id="b4f4d-103">Es una colección hiperlegible de 223 iconos con una superficie diminuta&mdash;listos para usar con Bootstrap y Foundation.</span><span class="sxs-lookup"><span data-stu-id="b4f4d-103">It is a hyper-legible collection of 223 icons with a tiny footprint&mdash;ready to use with Bootstrap and Foundation.</span></span> [<span data-ttu-id="b4f4d-104">Vista de la colección</span><span class="sxs-lookup"><span data-stu-id="b4f4d-104">View the collection</span></span>](http://useiconic.com/open#icons)



## <a name="whats-in-open-iconic"></a><span data-ttu-id="b4f4d-105">¿Qué hay en Open Iconic?</span><span class="sxs-lookup"><span data-stu-id="b4f4d-105">What's in Open Iconic?</span></span>

* <span data-ttu-id="b4f4d-106">223 iconos diseñados para ser legibles hasta en 8 píxeles</span><span class="sxs-lookup"><span data-stu-id="b4f4d-106">223 icons designed to be legible down to 8 pixels</span></span>
* <span data-ttu-id="b4f4d-107">Archivos SVG superligeros - 61,8 para todo el conjunto</span><span class="sxs-lookup"><span data-stu-id="b4f4d-107">Super-light SVG files - 61.8 for the entire set</span></span> 
* <span data-ttu-id="b4f4d-108">Sprite SVG&mdash;el reemplazo moderno de las fuentes de iconos</span><span class="sxs-lookup"><span data-stu-id="b4f4d-108">SVG sprite&mdash;the modern replacement for icon fonts</span></span>
* <span data-ttu-id="b4f4d-109">Formatos de fuentes web (EOT, OTF, SVG, TTF, WOFF), PNG y WebP</span><span class="sxs-lookup"><span data-stu-id="b4f4d-109">Webfont (EOT, OTF, SVG, TTF, WOFF), PNG and WebP formats</span></span>
* <span data-ttu-id="b4f4d-110">Hojas de estilo de fuentes web (incluidas las versiones para Bootstrap y Foundation) en formatos CSS, LESS, SCSS y Stylus</span><span class="sxs-lookup"><span data-stu-id="b4f4d-110">Webfont stylesheets (including versions for Bootstrap and Foundation) in CSS, LESS, SCSS and Stylus formats</span></span>
* <span data-ttu-id="b4f4d-111">Imágenes de trama PNG y WebP en 8 px, 16 px, 24 px, 32 px, 48 px y 64 px.</span><span class="sxs-lookup"><span data-stu-id="b4f4d-111">PNG and WebP raster images in 8px, 16px, 24px, 32px, 48px and 64px.</span></span>


## <a name="getting-started"></a><span data-ttu-id="b4f4d-112">Introducción</span><span class="sxs-lookup"><span data-stu-id="b4f4d-112">Getting Started</span></span>

#### <a name="for-code-samples-and-everything-else-you-need-to-get-started-with-open-iconic-check-out-our-iconshttpuseiconiccomopenicons-and-referencehttpuseiconiccomopenreference-sections"></a><span data-ttu-id="b4f4d-113">Para ver ejemplos de código y todo lo que necesita para comenzar con Open Iconic, consulte las secciones [Iconos](http://useiconic.com/open#icons) y [Referencia](http://useiconic.com/open#reference).</span><span class="sxs-lookup"><span data-stu-id="b4f4d-113">For code samples and everything else you need to get started with Open Iconic, check out our [Icons](http://useiconic.com/open#icons) and [Reference](http://useiconic.com/open#reference) sections.</span></span>

### <a name="general-usage"></a><span data-ttu-id="b4f4d-114">Uso general</span><span class="sxs-lookup"><span data-stu-id="b4f4d-114">General Usage</span></span>

#### <a name="using-open-iconics-svgs"></a><span data-ttu-id="b4f4d-115">Uso de SVG de Open Iconic</span><span class="sxs-lookup"><span data-stu-id="b4f4d-115">Using Open Iconic's SVGs</span></span>

<span data-ttu-id="b4f4d-116">Nos gustan los SVG y creemos que son la forma de mostrar iconos en la web.</span><span class="sxs-lookup"><span data-stu-id="b4f4d-116">We like SVGs and we think they're the way to display icons on the web.</span></span> <span data-ttu-id="b4f4d-117">Dado que Open Iconic consta solo de SVG básicos, le sugerimos que los muestre como lo haría con cualquier otra imagen (no se olvide del atributo `alt`).</span><span class="sxs-lookup"><span data-stu-id="b4f4d-117">Since Open Iconic are just basic SVGs, we suggest you display them like you would any other image (don't forget the `alt` attribute).</span></span>

```
<img src="/open-iconic/svg/icon-name.svg" alt="icon name">
```

#### <a name="using-open-iconics-svg-sprite"></a><span data-ttu-id="b4f4d-118">Uso de sprite SVG de Open Iconic</span><span class="sxs-lookup"><span data-stu-id="b4f4d-118">Using Open Iconic's SVG Sprite</span></span>

<span data-ttu-id="b4f4d-119">Open Iconic también incluye un sprite de SVG que permite mostrar todos los iconos del conjunto con una sola solicitud.</span><span class="sxs-lookup"><span data-stu-id="b4f4d-119">Open Iconic also comes in a SVG sprite which allows you to display all the icons in the set with a single request.</span></span> <span data-ttu-id="b4f4d-120">Es como una fuente de iconos, sin ser un método alternativo.</span><span class="sxs-lookup"><span data-stu-id="b4f4d-120">It's like an icon font, without being a hack.</span></span>

<span data-ttu-id="b4f4d-121">Agregar un icono desde un sprite SVG es algo diferente a lo que está acostumbrado, pero sigue siendo un juego de niños.</span><span class="sxs-lookup"><span data-stu-id="b4f4d-121">Adding an icon from an SVG sprite is a little different than what you're used to, but it's still a piece of cake.</span></span> <span data-ttu-id="b4f4d-122">*Sugerencia: Para que se pueda aplicar fácilmente el estilo a los iconos, le sugerimos que agregue una clase general a la etiqueta* `<svg>` *y un nombre de clase único para cada icono diferente en la etiqueta* `<use>` *.*</span><span class="sxs-lookup"><span data-stu-id="b4f4d-122">*Tip: To make your icons easily style able, we suggest adding a general class to the* `<svg>` *tag and a unique class name for each different icon in the* `<use>` *tag.*</span></span>  

```
<svg class="icon">
  <use xlink:href="open-iconic.svg#account-login" class="icon-account-login"></use>
</svg>
```

<span data-ttu-id="b4f4d-123">Para ajustar el tamaño de los iconos, solo se necesita un CSS básico.</span><span class="sxs-lookup"><span data-stu-id="b4f4d-123">Sizing icons only needs basic CSS.</span></span> <span data-ttu-id="b4f4d-124">Todos los iconos están en formato cuadrado, así que solo tiene que configurar la etiqueta `<svg>` con las mismas dimensiones de ancho y alto.</span><span class="sxs-lookup"><span data-stu-id="b4f4d-124">All the icons are in a square format, so just set the `<svg>` tag with equal width and height dimensions.</span></span>

```
.icon {
  width: 16px;
  height: 16px;
}
```

<span data-ttu-id="b4f4d-125">Dar color a los iconos es incluso más fácil.</span><span class="sxs-lookup"><span data-stu-id="b4f4d-125">Coloring icons is even easier.</span></span> <span data-ttu-id="b4f4d-126">Todo lo que tiene que hacer es establecer la regla `fill` en la etiqueta `<use>`.</span><span class="sxs-lookup"><span data-stu-id="b4f4d-126">All you need to do is set the `fill` rule on the `<use>` tag.</span></span>

```
.icon-account-login {
  fill: #f00;
}
```

<span data-ttu-id="b4f4d-127">Para más información sobre los sprites SVG, consulte la [guía de Chris Coyier](http://css-tricks.com/svg-sprites-use-better-icon-fonts/).</span><span class="sxs-lookup"><span data-stu-id="b4f4d-127">To learn more about SVG Sprites, read [Chris Coyier's guide](http://css-tricks.com/svg-sprites-use-better-icon-fonts/).</span></span>

#### <a name="using-open-iconics-icon-font"></a><span data-ttu-id="b4f4d-128">Con la fuente Icon de Open Iconic...</span><span class="sxs-lookup"><span data-stu-id="b4f4d-128">Using Open Iconic's Icon Font...</span></span>


##### <a name="with-bootstrap"></a><span data-ttu-id="b4f4d-129">…con Bootstrap</span><span class="sxs-lookup"><span data-stu-id="b4f4d-129">…with Bootstrap</span></span>

<span data-ttu-id="b4f4d-130">Puede encontrar nuestras hojas de estilo de Bootstrap en `font/css/open-iconic-bootstrap.{css, less, scss, styl}`</span><span class="sxs-lookup"><span data-stu-id="b4f4d-130">You can find our Bootstrap stylesheets in `font/css/open-iconic-bootstrap.{css, less, scss, styl}`</span></span>


```
<link href="/open-iconic/font/css/open-iconic-bootstrap.css" rel="stylesheet">
```


```
<span class="oi oi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="with-foundation"></a><span data-ttu-id="b4f4d-131">…con Foundation</span><span class="sxs-lookup"><span data-stu-id="b4f4d-131">…with Foundation</span></span>

<span data-ttu-id="b4f4d-132">Puede encontrar nuestras hojas de estilo Foundation en `font/css/open-iconic-foundation.{css, less, scss, styl}`</span><span class="sxs-lookup"><span data-stu-id="b4f4d-132">You can find our Foundation stylesheets in `font/css/open-iconic-foundation.{css, less, scss, styl}`</span></span>

```
<link href="/open-iconic/font/css/open-iconic-foundation.css" rel="stylesheet">
```


```
<span class="fi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="on-its-own"></a><span data-ttu-id="b4f4d-133">…por su cuenta</span><span class="sxs-lookup"><span data-stu-id="b4f4d-133">…on its own</span></span>

<span data-ttu-id="b4f4d-134">Puede encontrar nuestras hojas de estilo predeterminadas en `font/css/open-iconic.{css, less, scss, styl}`</span><span class="sxs-lookup"><span data-stu-id="b4f4d-134">You can find our default stylesheets in `font/css/open-iconic.{css, less, scss, styl}`</span></span>

```
<link href="/open-iconic/font/css/open-iconic.css" rel="stylesheet">
```

```
<span class="oi" data-glyph="icon-name" title="icon name" aria-hidden="true"></span>
```


## <a name="license"></a><span data-ttu-id="b4f4d-135">Licencia</span><span class="sxs-lookup"><span data-stu-id="b4f4d-135">License</span></span>

### <a name="icons"></a><span data-ttu-id="b4f4d-136">Iconos</span><span class="sxs-lookup"><span data-stu-id="b4f4d-136">Icons</span></span>

<span data-ttu-id="b4f4d-137">Todo el código (incluido el marcado SVG) está bajo la [licencia MIT](http://opensource.org/licenses/MIT).</span><span class="sxs-lookup"><span data-stu-id="b4f4d-137">All code (including SVG markup) is under the [MIT License](http://opensource.org/licenses/MIT).</span></span>

### <a name="fonts"></a><span data-ttu-id="b4f4d-138">Fuentes</span><span class="sxs-lookup"><span data-stu-id="b4f4d-138">Fonts</span></span>

<span data-ttu-id="b4f4d-139">Todas las fuentes están bajo la [licencia SIL](http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web).</span><span class="sxs-lookup"><span data-stu-id="b4f4d-139">All fonts are under the [SIL Licensed](http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web).</span></span>
