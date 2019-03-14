---
ms.openlocfilehash: 83a958fe6a8d61becf9f9062e8ad0ff69108b435
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57034992"
---
<a name="jquery-validation-pluginhttpjqueryvalidationorg---form-validation-made-easy"></a><span data-ttu-id="8513b-101">[Complemento de validación de jQuery](http://jqueryvalidation.org/): la validación de formularios, más fácil</span><span class="sxs-lookup"><span data-stu-id="8513b-101">[jQuery Validation Plugin](http://jqueryvalidation.org/) - Form validation made easy</span></span>
================================

<span data-ttu-id="8513b-102">[![Estado de la compilación](https://secure.travis-ci.org/jzaefferer/jquery-validation.png)](http://travis-ci.org/jzaefferer/jquery-validation)
[![Estado de devDependency](https://david-dm.org/jzaefferer/jquery-validation/dev-status.png?theme=shields.io)](https://david-dm.org/jzaefferer/jquery-validation#info=devDependencies)</span><span class="sxs-lookup"><span data-stu-id="8513b-102">[![Build Status](https://secure.travis-ci.org/jzaefferer/jquery-validation.png)](http://travis-ci.org/jzaefferer/jquery-validation)
[![devDependency Status](https://david-dm.org/jzaefferer/jquery-validation/dev-status.png?theme=shields.io)](https://david-dm.org/jzaefferer/jquery-validation#info=devDependencies)</span></span>

<span data-ttu-id="8513b-103">El complemento de validación de jQuery proporciona una validación inmediata para los formularios existentes y además facilita significativamente la realización de todo tipo de personalizaciones para adaptar su aplicación.</span><span class="sxs-lookup"><span data-stu-id="8513b-103">The jQuery Validation Plugin provides drop-in validation for your existing forms, while making all kinds of customizations to fit your application really easy.</span></span>

## <a name="help-the-projecthttppledgiecomcampaigns18159"></a>[<span data-ttu-id="8513b-104">Ayuda para el proyecto</span><span class="sxs-lookup"><span data-stu-id="8513b-104">Help the project</span></span>](http://pledgie.com/campaigns/18159)

<span data-ttu-id="8513b-105">[![Ayuda para el proyecto](http://www.pledgie.com/campaigns/18159.png?skin_name=chrome)](http://pledgie.com/campaigns/18159)</span><span class="sxs-lookup"><span data-stu-id="8513b-105">[![Help the project](http://www.pledgie.com/campaigns/18159.png?skin_name=chrome)](http://pledgie.com/campaigns/18159)</span></span>

<span data-ttu-id="8513b-106">Este proyecto necesita ayuda.</span><span class="sxs-lookup"><span data-stu-id="8513b-106">This project is looking for help!</span></span> <span data-ttu-id="8513b-107">[Puede realizar una donación para la campaña de Pledgie en curso](http://pledgie.com/campaigns/18159) y ayudar a correr la voz.</span><span class="sxs-lookup"><span data-stu-id="8513b-107">[You can donate to the ongoing pledgie campaign](http://pledgie.com/campaigns/18159) and help spread the word.</span></span> <span data-ttu-id="8513b-108">Si ha usado el complemento o pretende hacerlo, considere la opción de hacer una donación; cualquier cantidad ayudará.</span><span class="sxs-lookup"><span data-stu-id="8513b-108">If you've used the plugin, or plan to use, consider a donation - any amount will help.</span></span>

<span data-ttu-id="8513b-109">Puede encontrar el plan de inversión del dinero en la [página de Pledgie](http://pledgie.com/campaigns/18159).</span><span class="sxs-lookup"><span data-stu-id="8513b-109">You can find the plan for how to spend the money on the [pledgie page](http://pledgie.com/campaigns/18159).</span></span>

## <a name="getting-started"></a><span data-ttu-id="8513b-110">Introducción</span><span class="sxs-lookup"><span data-stu-id="8513b-110">Getting Started</span></span>

### <a name="downloading-the-prebuilt-files"></a><span data-ttu-id="8513b-111">Descarga de los archivos creados previamente</span><span class="sxs-lookup"><span data-stu-id="8513b-111">Downloading the prebuilt files</span></span>

<span data-ttu-id="8513b-112">Los archivos creados previamente se pueden descargar desde http://jqueryvalidation.org/.</span><span class="sxs-lookup"><span data-stu-id="8513b-112">Prebuilt files can be downloaded from http://jqueryvalidation.org/</span></span>

### <a name="downloading-the-latest-changes"></a><span data-ttu-id="8513b-113">Descarga de los últimos cambios</span><span class="sxs-lookup"><span data-stu-id="8513b-113">Downloading the latest changes</span></span>

<span data-ttu-id="8513b-114">Los archivos de desarrollo no publicados se pueden obtener de la siguiente forma:</span><span class="sxs-lookup"><span data-stu-id="8513b-114">The unreleased development files can be obtained by:</span></span>

 1. <span data-ttu-id="8513b-115">[Descarga](https://github.com/jzaefferer/jquery-validation/archive/master.zip) o bifurcación de este repositorio</span><span class="sxs-lookup"><span data-stu-id="8513b-115">[Downloading](https://github.com/jzaefferer/jquery-validation/archive/master.zip) or Forking this repository</span></span>
 2. [<span data-ttu-id="8513b-116">Configuración de la compilación</span><span class="sxs-lookup"><span data-stu-id="8513b-116">Setup the build</span></span>](CONTRIBUTING.md#build-setup)
 3. <span data-ttu-id="8513b-117">Ejecución de `grunt` para crear los archivos compilados en el directorio "dist"</span><span class="sxs-lookup"><span data-stu-id="8513b-117">Run `grunt` to create the built files in the "dist" directory</span></span>

### <a name="including-it-on-your-page"></a><span data-ttu-id="8513b-118">Inclusión en la página</span><span class="sxs-lookup"><span data-stu-id="8513b-118">Including it on your page</span></span>

<span data-ttu-id="8513b-119">Incluya jQuery y el complemento en una página.</span><span class="sxs-lookup"><span data-stu-id="8513b-119">Include jQuery and the plugin on a page.</span></span> <span data-ttu-id="8513b-120">A continuación, seleccione un formulario para validar el método `validate` e invocarlo.</span><span class="sxs-lookup"><span data-stu-id="8513b-120">Then select a form to validate and call the `validate` method.</span></span>

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

<span data-ttu-id="8513b-121">También puede incluir jQuery y el complemento con requirejs en el módulo.</span><span class="sxs-lookup"><span data-stu-id="8513b-121">Alternatively include jQuery and the plugin via requirejs in your module.</span></span>

```js
define(["jquery", "jquery.validate"], function( $ ) {
    $("form").validate();
});
```

<span data-ttu-id="8513b-122">Para obtener más información sobre cómo configurar reglas y personalizaciones, [consulte la documentación](http://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="8513b-122">For more information on how to setup a rules and customizations, [check the documentation](http://jqueryvalidation.org/documentation/).</span></span>

## <a name="reporting-issues-and-contributing-code"></a><span data-ttu-id="8513b-123">Notificación de problemas y código de contribución</span><span class="sxs-lookup"><span data-stu-id="8513b-123">Reporting issues and contributing code</span></span>

<span data-ttu-id="8513b-124">Consulte las [directrices de contribución](CONTRIBUTING.md) para obtener información.</span><span class="sxs-lookup"><span data-stu-id="8513b-124">See the [Contributing Guidelines](CONTRIBUTING.md) for details.</span></span>

<span data-ttu-id="8513b-125">**NOTA IMPORTANTE SOBRE LA VALIDACIÓN DE CORREO ELECTRÓNICO**.</span><span class="sxs-lookup"><span data-stu-id="8513b-125">**IMPORTANT NOTE ABOUT EMAIL VALIDATION**.</span></span> <span data-ttu-id="8513b-126">A partir de la versión 1.12.0, este complemento usa la misma expresión regular que la [especificación HTML5 sugiere para que los exploradores la usen](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span><span class="sxs-lookup"><span data-stu-id="8513b-126">As of version 1.12.0 this plugin is using the same regular expression that the [HTML5 specification suggests for browsers to use](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span></span> <span data-ttu-id="8513b-127">Seguiremos el mismo ejemplo y usaremos la misma comprobación.</span><span class="sxs-lookup"><span data-stu-id="8513b-127">We will follow their lead and use the same check.</span></span> <span data-ttu-id="8513b-128">Si cree que la especificación es incorrecta, notifíqueles el problema.</span><span class="sxs-lookup"><span data-stu-id="8513b-128">If you think the specification is wrong, please report the issue to them.</span></span> <span data-ttu-id="8513b-129">Si sus requisitos son distintos, considere la opción de [usar un método personalizado](http://jqueryvalidation.org/jQuery.validator.addMethod/).</span><span class="sxs-lookup"><span data-stu-id="8513b-129">If you have different requirements, consider [using a custom method](http://jqueryvalidation.org/jQuery.validator.addMethod/).</span></span>

## <a name="license"></a><span data-ttu-id="8513b-130">Licencia</span><span class="sxs-lookup"><span data-stu-id="8513b-130">License</span></span>
<span data-ttu-id="8513b-131">Copyright &copy; Jörn Zaefferer</span><span class="sxs-lookup"><span data-stu-id="8513b-131">Copyright &copy; Jörn Zaefferer</span></span><br>
<span data-ttu-id="8513b-132">Autorización sujeta a la licencia MIT.</span><span class="sxs-lookup"><span data-stu-id="8513b-132">Licensed under the MIT license.</span></span>
