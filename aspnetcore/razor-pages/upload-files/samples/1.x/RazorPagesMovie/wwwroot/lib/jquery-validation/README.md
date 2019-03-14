---
ms.openlocfilehash: 1b563a8c0674d51f893415221ca8fab574b78265
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57046552"
---
<a name="--"></a>--
================================

[![Estado de la compilación](https://secure.travis-ci.org/jzaefferer/jquery-validation.png)](http://travis-ci.org/jzaefferer/jquery-validation)
[![Estado de devDependency](https://david-dm.org/jzaefferer/jquery-validation/dev-status.png?theme=shields.io)](https://david-dm.org/jzaefferer/jquery-validation#info=devDependencies)

El complemento de validación de jQuery proporciona una validación inmediata para los formularios existentes y además facilita significativamente la realización de todo tipo de personalizaciones para adaptar su aplicación.

## <a name="help-the-projecthttppledgiecomcampaigns18159"></a>[Ayuda para el proyecto](http://pledgie.com/campaigns/18159)

[![Ayuda para el proyecto](http://www.pledgie.com/campaigns/18159.png?skin_name=chrome)](http://pledgie.com/campaigns/18159)

Este proyecto necesita ayuda. [Puede realizar una donación para la campaña de Pledgie en curso](http://pledgie.com/campaigns/18159) y ayudar a correr la voz. Si ha usado el complemento o pretende hacerlo, considere la opción de hacer una donación; cualquier cantidad ayudará.

Puede encontrar el plan de inversión del dinero en la [página de Pledgie](http://pledgie.com/campaigns/18159).

## <a name="get-started"></a>Primeros pasos

### <a name="downloading-the-prebuilt-files"></a>Descarga de los archivos creados previamente

Los archivos creados previamente se pueden descargar desde http://jqueryvalidation.org/.

### <a name="downloading-the-latest-changes"></a>Descarga de los últimos cambios

Los archivos de desarrollo no publicados se pueden obtener de la siguiente forma:

 1. [Descarga](https://github.com/jzaefferer/jquery-validation/archive/master.zip) o bifurcación de este repositorio
 2. [Configuración de la compilación](CONTRIBUTING.md#build-setup)
 3. Ejecución de `grunt` para crear los archivos compilados en el directorio "dist"

### <a name="including-it-on-your-page"></a>Inclusión en la página

Incluya jQuery y el complemento en una página. A continuación, seleccione un formulario para validar el método `validate` e invocarlo.

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

También puede incluir jQuery y el complemento con requirejs en el módulo.

```js
define(["jquery", "jquery.validate"], function( $ ) {
    $("form").validate();
});
```

Para obtener más información sobre cómo configurar reglas y personalizaciones, [consulte la documentación](http://jqueryvalidation.org/documentation/).

## <a name="reporting-issues-and-contributing-code"></a>Notificación de problemas y código de contribución

Consulte las [directrices de contribución](CONTRIBUTING.md) para obtener información.

**NOTA IMPORTANTE SOBRE LA VALIDACIÓN DE CORREO ELECTRÓNICO**. A partir de la versión 1.12.0, este complemento usa la misma expresión regular que la [especificación HTML5 sugiere para que los exploradores la usen](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address). Seguiremos el mismo ejemplo y usaremos la misma comprobación. Si cree que la especificación es incorrecta, notifíqueles el problema. Si sus requisitos son distintos, considere la opción de [usar un método personalizado](http://jqueryvalidation.org/jQuery.validator.addMethod/).

## <a name="license"></a>Licencia
Copyright &copy; Jörn Zaefferer<br>
Autorización sujeta a la licencia MIT.
