---
ms.openlocfilehash: 610b16b181422978dc143b95f28658bbdf177fdd
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57028282"
---
# <a name="jquery"></a><span data-ttu-id="37abf-101">jQuery</span><span class="sxs-lookup"><span data-stu-id="37abf-101">jQuery</span></span>

> <span data-ttu-id="37abf-102">jQuery es una biblioteca de JavaScript rápida, pequeña y repleta de características.</span><span class="sxs-lookup"><span data-stu-id="37abf-102">jQuery is a fast, small, and feature-rich JavaScript library.</span></span>

<span data-ttu-id="37abf-103">Para información sobre cómo empezar a trabajar con jQuery y cómo usarla, consulte la [documentación de jQuery](http://api.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="37abf-103">For information on how to get started and how to use jQuery, please see [jQuery's documentation](http://api.jquery.com/).</span></span>
<span data-ttu-id="37abf-104">Para archivos de código fuente y problemas, visite el [repositorio de jQuery](https://github.com/jquery/jquery).</span><span class="sxs-lookup"><span data-stu-id="37abf-104">For source files and issues, please visit the [jQuery repo](https://github.com/jquery/jquery).</span></span>

<span data-ttu-id="37abf-105">Si está actualizando, consulte la [entrada de blog para 3.3.1](https://blog.jquery.com/2017/03/20/jquery-3.3.1-now-available/).</span><span class="sxs-lookup"><span data-stu-id="37abf-105">If upgrading, please see the [blog post for 3.3.1](https://blog.jquery.com/2017/03/20/jquery-3.3.1-now-available/).</span></span> <span data-ttu-id="37abf-106">Esto incluye importantes diferencias con respecto a la versión anterior y un registro de cambios más legible.</span><span class="sxs-lookup"><span data-stu-id="37abf-106">This includes notable differences from the previous version and a more readable changelog.</span></span>

## <a name="including-jquery"></a><span data-ttu-id="37abf-107">Inclusión de jQuery</span><span class="sxs-lookup"><span data-stu-id="37abf-107">Including jQuery</span></span>

<span data-ttu-id="37abf-108">A continuación se muestran algunas de las formas más comunes para incluir jQuery.</span><span class="sxs-lookup"><span data-stu-id="37abf-108">Below are some of the most common ways to include jQuery.</span></span>

### <a name="browser"></a><span data-ttu-id="37abf-109">Explorador</span><span class="sxs-lookup"><span data-stu-id="37abf-109">Browser</span></span>

#### <a name="script-tag"></a><span data-ttu-id="37abf-110">Etiqueta script</span><span class="sxs-lookup"><span data-stu-id="37abf-110">Script tag</span></span>

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
```

#### <a name="babel"></a><span data-ttu-id="37abf-111">Babel</span><span class="sxs-lookup"><span data-stu-id="37abf-111">Babel</span></span>

<span data-ttu-id="37abf-112">[Babel](http://babeljs.io/) es un compilador de JavaScript de la siguiente generación.</span><span class="sxs-lookup"><span data-stu-id="37abf-112">[Babel](http://babeljs.io/) is a next generation JavaScript compiler.</span></span> <span data-ttu-id="37abf-113">Una de las características es la capacidad para usar los módulos ES6/ES2015 ahora, aunque los exploradores aún no admiten esta característica de forma nativa.</span><span class="sxs-lookup"><span data-stu-id="37abf-113">One of the features is the ability to use ES6/ES2015 modules now, even though browsers do not yet support this feature natively.</span></span>

```js
import $ from "jquery";
```

#### <a name="browserifywebpack"></a><span data-ttu-id="37abf-114">Browserify y Webpack</span><span class="sxs-lookup"><span data-stu-id="37abf-114">Browserify/Webpack</span></span>

<span data-ttu-id="37abf-115">Hay varias formas de usar [Browserify](http://browserify.org/) y [Webpack](https://webpack.github.io/).</span><span class="sxs-lookup"><span data-stu-id="37abf-115">There are several ways to use [Browserify](http://browserify.org/) and [Webpack](https://webpack.github.io/).</span></span> <span data-ttu-id="37abf-116">Para más información sobre el uso de estas herramientas, consulte la documentación del proyecto correspondiente.</span><span class="sxs-lookup"><span data-stu-id="37abf-116">For more information on using these tools, please refer to the corresponding project's documention.</span></span> <span data-ttu-id="37abf-117">En el script, la inclusión de jQuery normalmente será similar a esto...</span><span class="sxs-lookup"><span data-stu-id="37abf-117">In the script, including jQuery will usually look like this...</span></span>

```js
var $ = require("jquery");
```

#### <a name="amd-asynchronous-module-definition"></a><span data-ttu-id="37abf-118">AMD (definición asincrónica de módulos)</span><span class="sxs-lookup"><span data-stu-id="37abf-118">AMD (Asynchronous Module Definition)</span></span>

<span data-ttu-id="37abf-119">AMD es un formato de módulo creado para el explorador.</span><span class="sxs-lookup"><span data-stu-id="37abf-119">AMD is a module format built for the browser.</span></span> <span data-ttu-id="37abf-120">Para más información, recomendamos la [documentación de require.js](http://requirejs.org/docs/whyamd.html).</span><span class="sxs-lookup"><span data-stu-id="37abf-120">For more information, we recommend [require.js' documentation](http://requirejs.org/docs/whyamd.html).</span></span>

```js
define(["jquery"], function($) {

});
```

### <a name="node"></a><span data-ttu-id="37abf-121">Nodo</span><span class="sxs-lookup"><span data-stu-id="37abf-121">Node</span></span>

<span data-ttu-id="37abf-122">Para incluir jQuery en [Node](nodejs.org), instale primero con npm.</span><span class="sxs-lookup"><span data-stu-id="37abf-122">To include jQuery in [Node](nodejs.org), first install with npm.</span></span>

```sh
npm install jquery
```

<span data-ttu-id="37abf-123">Para que JQuery funcione en Node, se requiere una ventana con un documento.</span><span class="sxs-lookup"><span data-stu-id="37abf-123">For jQuery to work in Node, a window with a document is required.</span></span> <span data-ttu-id="37abf-124">Puesto que no existe tal ventana de forma nativa en Node, se puede simular una mediante herramientas como [jsdom](https://github.com/tmpvar/jsdom).</span><span class="sxs-lookup"><span data-stu-id="37abf-124">Since no such window exists natively in Node, one can be mocked by tools such as [jsdom](https://github.com/tmpvar/jsdom).</span></span> <span data-ttu-id="37abf-125">Esto puede ser útil para fines de prueba.</span><span class="sxs-lookup"><span data-stu-id="37abf-125">This can be useful for testing purposes.</span></span>

```js
require("jsdom").env("", function(err, window) {
    if (err) {
        console.error(err);
        return;
    }

    var $ = require("jquery")(window);
});
```
