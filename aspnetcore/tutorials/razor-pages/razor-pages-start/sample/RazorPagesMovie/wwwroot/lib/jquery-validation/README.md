---
ms.openlocfilehash: 1b563a8c0674d51f893415221ca8fab574b78265
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57032952"
---
<a name="--"></a>--
================================

<span data-ttu-id="4e266-101">[![Estado de la compilación](https://secure.travis-ci.org/jzaefferer/jquery-validation.png)](http://travis-ci.org/jzaefferer/jquery-validation)
[![Estado de devDependency](https://david-dm.org/jzaefferer/jquery-validation/dev-status.png?theme=shields.io)](https://david-dm.org/jzaefferer/jquery-validation#info=devDependencies)</span><span class="sxs-lookup"><span data-stu-id="4e266-101">[![Build Status](https://secure.travis-ci.org/jzaefferer/jquery-validation.png)](http://travis-ci.org/jzaefferer/jquery-validation)
[![devDependency Status](https://david-dm.org/jzaefferer/jquery-validation/dev-status.png?theme=shields.io)](https://david-dm.org/jzaefferer/jquery-validation#info=devDependencies)</span></span>

<span data-ttu-id="4e266-102">El complemento de validación de jQuery proporciona una validación inmediata para los formularios existentes y además facilita significativamente la realización de todo tipo de personalizaciones para adaptar su aplicación.</span><span class="sxs-lookup"><span data-stu-id="4e266-102">The jQuery Validation Plugin provides drop-in validation for your existing forms, while making all kinds of customizations to fit your application really easy.</span></span>

## <a name="help-the-projecthttppledgiecomcampaigns18159"></a>[<span data-ttu-id="4e266-103">Ayuda para el proyecto</span><span class="sxs-lookup"><span data-stu-id="4e266-103">Help the project</span></span>](http://pledgie.com/campaigns/18159)

<span data-ttu-id="4e266-104">[![Ayuda para el proyecto](http://www.pledgie.com/campaigns/18159.png?skin_name=chrome)](http://pledgie.com/campaigns/18159)</span><span class="sxs-lookup"><span data-stu-id="4e266-104">[![Help the project](http://www.pledgie.com/campaigns/18159.png?skin_name=chrome)](http://pledgie.com/campaigns/18159)</span></span>

<span data-ttu-id="4e266-105">Este proyecto necesita ayuda.</span><span class="sxs-lookup"><span data-stu-id="4e266-105">This project is looking for help!</span></span> <span data-ttu-id="4e266-106">[Puede realizar una donación para la campaña de Pledgie en curso](http://pledgie.com/campaigns/18159) y ayudar a correr la voz.</span><span class="sxs-lookup"><span data-stu-id="4e266-106">[You can donate to the ongoing pledgie campaign](http://pledgie.com/campaigns/18159) and help spread the word.</span></span> <span data-ttu-id="4e266-107">Si ha usado el complemento o pretende hacerlo, considere la opción de hacer una donación; cualquier cantidad ayudará.</span><span class="sxs-lookup"><span data-stu-id="4e266-107">If you've used the plugin, or plan to use, consider a donation - any amount will help.</span></span>

<span data-ttu-id="4e266-108">Puede encontrar el plan de inversión del dinero en la [página de Pledgie](http://pledgie.com/campaigns/18159).</span><span class="sxs-lookup"><span data-stu-id="4e266-108">You can find the plan for how to spend the money on the [pledgie page](http://pledgie.com/campaigns/18159).</span></span>

## <a name="get-started"></a><span data-ttu-id="4e266-109">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="4e266-109">Get Started</span></span>

### <a name="downloading-the-prebuilt-files"></a><span data-ttu-id="4e266-110">Descarga de los archivos creados previamente</span><span class="sxs-lookup"><span data-stu-id="4e266-110">Downloading the prebuilt files</span></span>

<span data-ttu-id="4e266-111">Los archivos creados previamente se pueden descargar desde http://jqueryvalidation.org/.</span><span class="sxs-lookup"><span data-stu-id="4e266-111">Prebuilt files can be downloaded from http://jqueryvalidation.org/</span></span>

### <a name="downloading-the-latest-changes"></a><span data-ttu-id="4e266-112">Descarga de los últimos cambios</span><span class="sxs-lookup"><span data-stu-id="4e266-112">Downloading the latest changes</span></span>

<span data-ttu-id="4e266-113">Los archivos de desarrollo no publicados se pueden obtener de la siguiente forma:</span><span class="sxs-lookup"><span data-stu-id="4e266-113">The unreleased development files can be obtained by:</span></span>

 1. <span data-ttu-id="4e266-114">[Descarga](https://github.com/jzaefferer/jquery-validation/archive/master.zip) o bifurcación de este repositorio</span><span class="sxs-lookup"><span data-stu-id="4e266-114">[Downloading](https://github.com/jzaefferer/jquery-validation/archive/master.zip) or Forking this repository</span></span>
 2. [<span data-ttu-id="4e266-115">Configuración de la compilación</span><span class="sxs-lookup"><span data-stu-id="4e266-115">Setup the build</span></span>](CONTRIBUTING.md#build-setup)
 3. <span data-ttu-id="4e266-116">Ejecución de `grunt` para crear los archivos compilados en el directorio "dist"</span><span class="sxs-lookup"><span data-stu-id="4e266-116">Run `grunt` to create the built files in the "dist" directory</span></span>

### <a name="including-it-on-your-page"></a><span data-ttu-id="4e266-117">Inclusión en la página</span><span class="sxs-lookup"><span data-stu-id="4e266-117">Including it on your page</span></span>

<span data-ttu-id="4e266-118">Incluya jQuery y el complemento en una página.</span><span class="sxs-lookup"><span data-stu-id="4e266-118">Include jQuery and the plugin on a page.</span></span> <span data-ttu-id="4e266-119">A continuación, seleccione un formulario para validar el método `validate` e invocarlo.</span><span class="sxs-lookup"><span data-stu-id="4e266-119">Then select a form to validate and call the `validate` method.</span></span>

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

<span data-ttu-id="4e266-120">También puede incluir jQuery y el complemento con requirejs en el módulo.</span><span class="sxs-lookup"><span data-stu-id="4e266-120">Alternatively include jQuery and the plugin via requirejs in your module.</span></span>

```js
define(["jquery", "jquery.validate"], function( $ ) {
    $("form").validate();
});
```

<span data-ttu-id="4e266-121">Para obtener más información sobre cómo configurar reglas y personalizaciones, [consulte la documentación](http://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="4e266-121">For more information on how to setup a rules and customizations, [check the documentation](http://jqueryvalidation.org/documentation/).</span></span>

## <a name="reporting-issues-and-contributing-code"></a><span data-ttu-id="4e266-122">Notificación de problemas y código de contribución</span><span class="sxs-lookup"><span data-stu-id="4e266-122">Reporting issues and contributing code</span></span>

<span data-ttu-id="4e266-123">Consulte las [directrices de contribución](CONTRIBUTING.md) para obtener información.</span><span class="sxs-lookup"><span data-stu-id="4e266-123">See the [Contributing Guidelines](CONTRIBUTING.md) for details.</span></span>

<span data-ttu-id="4e266-124">**NOTA IMPORTANTE SOBRE LA VALIDACIÓN DE CORREO ELECTRÓNICO**.</span><span class="sxs-lookup"><span data-stu-id="4e266-124">**IMPORTANT NOTE ABOUT EMAIL VALIDATION**.</span></span> <span data-ttu-id="4e266-125">A partir de la versión 1.12.0, este complemento usa la misma expresión regular que la [especificación HTML5 sugiere para que los exploradores la usen](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span><span class="sxs-lookup"><span data-stu-id="4e266-125">As of version 1.12.0 this plugin is using the same regular expression that the [HTML5 specification suggests for browsers to use](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span></span> <span data-ttu-id="4e266-126">Seguiremos el mismo ejemplo y usaremos la misma comprobación.</span><span class="sxs-lookup"><span data-stu-id="4e266-126">We will follow their lead and use the same check.</span></span> <span data-ttu-id="4e266-127">Si cree que la especificación es incorrecta, notifíqueles el problema.</span><span class="sxs-lookup"><span data-stu-id="4e266-127">If you think the specification is wrong, please report the issue to them.</span></span> <span data-ttu-id="4e266-128">Si sus requisitos son distintos, considere la opción de [usar un método personalizado](http://jqueryvalidation.org/jQuery.validator.addMethod/).</span><span class="sxs-lookup"><span data-stu-id="4e266-128">If you have different requirements, consider [using a custom method](http://jqueryvalidation.org/jQuery.validator.addMethod/).</span></span>

## <a name="license"></a><span data-ttu-id="4e266-129">Licencia</span><span class="sxs-lookup"><span data-stu-id="4e266-129">License</span></span>
<span data-ttu-id="4e266-130">Copyright &copy; Jörn Zaefferer</span><span class="sxs-lookup"><span data-stu-id="4e266-130">Copyright &copy; Jörn Zaefferer</span></span><br>
<span data-ttu-id="4e266-131">Autorización sujeta a la licencia MIT.</span><span class="sxs-lookup"><span data-stu-id="4e266-131">Licensed under the MIT license.</span></span>
