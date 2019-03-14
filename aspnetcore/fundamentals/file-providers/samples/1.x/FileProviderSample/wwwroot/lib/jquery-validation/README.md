---
ms.openlocfilehash: 24c36493284988aa45b15c78e2577c0ef96aba56
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029992"
---
<a name="jquery-validation-pluginhttpsjqueryvalidationorg---form-validation-made-easy"></a><span data-ttu-id="e0ec3-101">[Complemento de validación de jQuery](https://jqueryvalidation.org/): la validación de formularios, más fácil</span><span class="sxs-lookup"><span data-stu-id="e0ec3-101">[jQuery Validation Plugin](https://jqueryvalidation.org/) - Form validation made easy</span></span>
================================

<span data-ttu-id="e0ec3-102">[![Estado de la compilación](https://secure.travis-ci.org/jquery-validation/jquery-validation.svg)](https://travis-ci.org/jquery-validation/jquery-validation)
[![Estado de devDependency](https://david-dm.org/jquery-validation/jquery-validation/dev-status.svg?theme=shields.io)](https://david-dm.org/jquery-validation/jquery-validation#info=devDependencies)</span><span class="sxs-lookup"><span data-stu-id="e0ec3-102">[![Build Status](https://secure.travis-ci.org/jquery-validation/jquery-validation.svg)](https://travis-ci.org/jquery-validation/jquery-validation)
[![devDependency Status](https://david-dm.org/jquery-validation/jquery-validation/dev-status.svg?theme=shields.io)](https://david-dm.org/jquery-validation/jquery-validation#info=devDependencies)</span></span>

<span data-ttu-id="e0ec3-103">El complemento de validación de jQuery proporciona una validación inmediata para los formularios existentes y además facilita significativamente la realización de todo tipo de personalizaciones para adaptar su aplicación.</span><span class="sxs-lookup"><span data-stu-id="e0ec3-103">The jQuery Validation Plugin provides drop-in validation for your existing forms, while making all kinds of customizations to fit your application really easy.</span></span>

## <a name="getting-started"></a><span data-ttu-id="e0ec3-104">Introducción</span><span class="sxs-lookup"><span data-stu-id="e0ec3-104">Getting Started</span></span>

### <a name="downloading-the-prebuilt-files"></a><span data-ttu-id="e0ec3-105">Descarga de los archivos creados previamente</span><span class="sxs-lookup"><span data-stu-id="e0ec3-105">Downloading the prebuilt files</span></span>

<span data-ttu-id="e0ec3-106">Los archivos creados previamente se pueden descargar desde https://jqueryvalidation.org/.</span><span class="sxs-lookup"><span data-stu-id="e0ec3-106">Prebuilt files can be downloaded from https://jqueryvalidation.org/</span></span>

### <a name="downloading-the-latest-changes"></a><span data-ttu-id="e0ec3-107">Descarga de los últimos cambios</span><span class="sxs-lookup"><span data-stu-id="e0ec3-107">Downloading the latest changes</span></span>

<span data-ttu-id="e0ec3-108">Los archivos de desarrollo no publicados se pueden obtener de la siguiente forma:</span><span class="sxs-lookup"><span data-stu-id="e0ec3-108">The unreleased development files can be obtained by:</span></span>

 1. <span data-ttu-id="e0ec3-109">[Descarga](https://github.com/jquery-validation/jquery-validation/archive/master.zip) o bifurcación de este repositorio</span><span class="sxs-lookup"><span data-stu-id="e0ec3-109">[Downloading](https://github.com/jquery-validation/jquery-validation/archive/master.zip) or Forking this repository</span></span>
 2. [<span data-ttu-id="e0ec3-110">Configuración de la compilación</span><span class="sxs-lookup"><span data-stu-id="e0ec3-110">Setup the build</span></span>](CONTRIBUTING.md#build-setup)
 3. <span data-ttu-id="e0ec3-111">Ejecución de `grunt` para crear los archivos compilados en el directorio "dist"</span><span class="sxs-lookup"><span data-stu-id="e0ec3-111">Run `grunt` to create the built files in the "dist" directory</span></span>

### <a name="including-it-on-your-page"></a><span data-ttu-id="e0ec3-112">Inclusión en la página</span><span class="sxs-lookup"><span data-stu-id="e0ec3-112">Including it on your page</span></span>

<span data-ttu-id="e0ec3-113">Incluya jQuery y el complemento en una página.</span><span class="sxs-lookup"><span data-stu-id="e0ec3-113">Include jQuery and the plugin on a page.</span></span> <span data-ttu-id="e0ec3-114">A continuación, seleccione un formulario para validar el método `validate` e invocarlo.</span><span class="sxs-lookup"><span data-stu-id="e0ec3-114">Then select a form to validate and call the `validate` method.</span></span>

```html
<form>
    <input required>
</form>
<script src="jquery.js"></script>
<script src="jquery.validate.js"></script>
<script>
$("form").validate();
</script>
```

<span data-ttu-id="e0ec3-115">También puede incluir jQuery y el complemento con requirejs en el módulo.</span><span class="sxs-lookup"><span data-stu-id="e0ec3-115">Alternatively include jQuery and the plugin via requirejs in your module.</span></span>

```js
define(["jquery", "jquery.validate"], function( $ ) {
    $("form").validate();
});
```

<span data-ttu-id="e0ec3-116">Para obtener más información sobre cómo configurar reglas y personalizaciones, [consulte la documentación](https://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="e0ec3-116">For more information on how to setup a rules and customizations, [check the documentation](https://jqueryvalidation.org/documentation/).</span></span>

## <a name="reporting-issues-and-contributing-code"></a><span data-ttu-id="e0ec3-117">Notificación de problemas y código de contribución</span><span class="sxs-lookup"><span data-stu-id="e0ec3-117">Reporting issues and contributing code</span></span>

<span data-ttu-id="e0ec3-118">Consulte las [directrices de contribución](CONTRIBUTING.md) para obtener información.</span><span class="sxs-lookup"><span data-stu-id="e0ec3-118">See the [Contributing Guidelines](CONTRIBUTING.md) for details.</span></span>

<span data-ttu-id="e0ec3-119">**NOTA IMPORTANTE SOBRE LA VALIDACIÓN DE CORREO ELECTRÓNICO**.</span><span class="sxs-lookup"><span data-stu-id="e0ec3-119">**IMPORTANT NOTE ABOUT EMAIL VALIDATION**.</span></span> <span data-ttu-id="e0ec3-120">A partir de la versión 1.12.0, este complemento usa la misma expresión regular que la [especificación HTML5 sugiere para que los exploradores la usen](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span><span class="sxs-lookup"><span data-stu-id="e0ec3-120">As of version 1.12.0 this plugin is using the same regular expression that the [HTML5 specification suggests for browsers to use](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span></span> <span data-ttu-id="e0ec3-121">Seguiremos el mismo ejemplo y usaremos la misma comprobación.</span><span class="sxs-lookup"><span data-stu-id="e0ec3-121">We will follow their lead and use the same check.</span></span> <span data-ttu-id="e0ec3-122">Si cree que la especificación es incorrecta, notifíqueles el problema.</span><span class="sxs-lookup"><span data-stu-id="e0ec3-122">If you think the specification is wrong, please report the issue to them.</span></span> <span data-ttu-id="e0ec3-123">Si sus requisitos son distintos, considere la opción de [usar un método personalizado](https://jqueryvalidation.org/jQuery.validator.addMethod/).</span><span class="sxs-lookup"><span data-stu-id="e0ec3-123">If you have different requirements, consider [using a custom method](https://jqueryvalidation.org/jQuery.validator.addMethod/).</span></span>
<span data-ttu-id="e0ec3-124">En caso de que necesite ajustar los patrones de expresiones regulares de validación integrados, [siga la documentación](https://jqueryvalidation.org/jQuery.validator.methods/).</span><span class="sxs-lookup"><span data-stu-id="e0ec3-124">In case you need to adjust the built-in validation regular expression patterns, please [follow the documentation](https://jqueryvalidation.org/jQuery.validator.methods/).</span></span>

<span data-ttu-id="e0ec3-125">**NOTA IMPORTANTE SOBRE EL MÉTODO REQUERIDO**.</span><span class="sxs-lookup"><span data-stu-id="e0ec3-125">**IMPORTANT NOTE ABOUT REQUIRED METHOD**.</span></span> <span data-ttu-id="e0ec3-126">A partir de la versión 1.14.0, este complemento detiene el recorte de espacios en blanco del valor del elemento adjunto.</span><span class="sxs-lookup"><span data-stu-id="e0ec3-126">As of version 1.14.0 this plugin stops trimming white spaces from the value of the attached element.</span></span> <span data-ttu-id="e0ec3-127">Si desea conseguir el mismo resultado, puede usar el [`normalizer`](https://jqueryvalidation.org/normalizer/) que puede usarse para transformar el valor de un elemento antes de la validación.</span><span class="sxs-lookup"><span data-stu-id="e0ec3-127">If you want to achieve the same result, you can use the [`normalizer`](https://jqueryvalidation.org/normalizer/) that can be used to transform the value of an element before validation.</span></span> <span data-ttu-id="e0ec3-128">Esta característica estaba disponible desde `v1.15.0`.</span><span class="sxs-lookup"><span data-stu-id="e0ec3-128">This feature was available since `v1.15.0`.</span></span> <span data-ttu-id="e0ec3-129">En otras palabras, puede hacer algo así:</span><span class="sxs-lookup"><span data-stu-id="e0ec3-129">In other words, you can do something like this:</span></span>
``` js
$("#myForm").validate({
    rules: {
        username: {
            required: true,
            // Using the normalizer to trim the value of the element
            // before validating it.
            //
            // The value of `this` inside the `normalizer` is the corresponding
            // DOMElement. In this example, `this` references the `username` element.
            normalizer: function(value) {
                return $.trim(value);
            }
        }
    }
});
```

## <a name="license"></a><span data-ttu-id="e0ec3-130">Licencia</span><span class="sxs-lookup"><span data-stu-id="e0ec3-130">License</span></span>
<span data-ttu-id="e0ec3-131">Copyright &copy; Jörn Zaefferer</span><span class="sxs-lookup"><span data-stu-id="e0ec3-131">Copyright &copy; Jörn Zaefferer</span></span><br>
<span data-ttu-id="e0ec3-132">Autorización sujeta a la licencia MIT.</span><span class="sxs-lookup"><span data-stu-id="e0ec3-132">Licensed under the MIT license.</span></span>
