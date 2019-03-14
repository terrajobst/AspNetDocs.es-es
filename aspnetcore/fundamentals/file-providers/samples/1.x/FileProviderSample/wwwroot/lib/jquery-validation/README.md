---
ms.openlocfilehash: 24c36493284988aa45b15c78e2577c0ef96aba56
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029992"
---
<a name="jquery-validation-pluginhttpsjqueryvalidationorg---form-validation-made-easy"></a>[Complemento de validación de jQuery](https://jqueryvalidation.org/): la validación de formularios, más fácil
================================

[![Estado de la compilación](https://secure.travis-ci.org/jquery-validation/jquery-validation.svg)](https://travis-ci.org/jquery-validation/jquery-validation)
[![Estado de devDependency](https://david-dm.org/jquery-validation/jquery-validation/dev-status.svg?theme=shields.io)](https://david-dm.org/jquery-validation/jquery-validation#info=devDependencies)

El complemento de validación de jQuery proporciona una validación inmediata para los formularios existentes y además facilita significativamente la realización de todo tipo de personalizaciones para adaptar su aplicación.

## <a name="getting-started"></a>Introducción

### <a name="downloading-the-prebuilt-files"></a>Descarga de los archivos creados previamente

Los archivos creados previamente se pueden descargar desde https://jqueryvalidation.org/.

### <a name="downloading-the-latest-changes"></a>Descarga de los últimos cambios

Los archivos de desarrollo no publicados se pueden obtener de la siguiente forma:

 1. [Descarga](https://github.com/jquery-validation/jquery-validation/archive/master.zip) o bifurcación de este repositorio
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

Para obtener más información sobre cómo configurar reglas y personalizaciones, [consulte la documentación](https://jqueryvalidation.org/documentation/).

## <a name="reporting-issues-and-contributing-code"></a>Notificación de problemas y código de contribución

Consulte las [directrices de contribución](CONTRIBUTING.md) para obtener información.

**NOTA IMPORTANTE SOBRE LA VALIDACIÓN DE CORREO ELECTRÓNICO**. A partir de la versión 1.12.0, este complemento usa la misma expresión regular que la [especificación HTML5 sugiere para que los exploradores la usen](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address). Seguiremos el mismo ejemplo y usaremos la misma comprobación. Si cree que la especificación es incorrecta, notifíqueles el problema. Si sus requisitos son distintos, considere la opción de [usar un método personalizado](https://jqueryvalidation.org/jQuery.validator.addMethod/).
En caso de que necesite ajustar los patrones de expresiones regulares de validación integrados, [siga la documentación](https://jqueryvalidation.org/jQuery.validator.methods/).

**NOTA IMPORTANTE SOBRE EL MÉTODO REQUERIDO**. A partir de la versión 1.14.0, este complemento detiene el recorte de espacios en blanco del valor del elemento adjunto. Si desea conseguir el mismo resultado, puede usar el [`normalizer`](https://jqueryvalidation.org/normalizer/) que puede usarse para transformar el valor de un elemento antes de la validación. Esta característica estaba disponible desde `v1.15.0`. En otras palabras, puede hacer algo así:
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

## <a name="license"></a>Licencia
Copyright &copy; Jörn Zaefferer<br>
Autorización sujeta a la licencia MIT.
