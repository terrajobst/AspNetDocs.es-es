---
ms.openlocfilehash: 3b33bb9bcab1aa7e5438c6fe3f2d7ba0833e7756
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57060822"
---
--

## <a name="reporting-an-issue"></a>Notificación de un problema

1. Asegúrese de que el problema que intenta solucionar es reproducible.
2. Use http://jsbin.com o http://jsfiddle.net para proporcionar una página de prueba.
3. Indique en qué exploradores se puede reproducir el problema. **Nota: No se solucionar problemas del modo de compatibilidad de IE. Asegúrese de que realiza la prueba en un explorador real.**
4. Indique en qué versión del complemento se puede reproducir el problema. Señale también si se puede reproducir después de actualizar a la última versión.

También se realiza un seguimiento de los problemas de la documentación en el controlador de problemas de [validación de jQuery](https://github.com/jzaefferer/jquery-validation/issues).
Se aceptan las solicitudes de incorporación de cambios para mejorar los documentos en el repositorio de [documentos de validación de jQuery](https://github.com/jzaefferer/validation-content).

**NOTA IMPORTANTE SOBRE LA VALIDACIÓN DE CORREO ELECTRÓNICO**. A partir de la versión 1.12.0, este complemento usa la misma expresión regular que la [especificación HTML5 sugiere para que los exploradores la usen](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address). Seguiremos el mismo ejemplo y usaremos la misma comprobación. Si cree que la especificación es incorrecta, notifíqueles el problema. Si sus requisitos son distintos, considere la opción de [usar un método personalizado](http://jqueryvalidation.org/jQuery.validator.addMethod/).

## <a name="contributing-code"></a>Código de contribución

Gracias por contribuir. A continuación, se detallan algunas instrucciones para ayudarle a que su contribución quede reflejada.

1. Asegúrese de que el problema que intenta solucionar es reproducible. Use jsbin.com o jsfiddle.net para proporcionar una página de prueba.
2. Siga la [guía de estilo de jQuery](http://contribute.jquery.com/style-guides/js).
3. Agregue o actualice pruebas unitarias junto con la revisión. Ejecute las pruebas unitarias al menos en un explorador (vea la información correspondiente más abajo).
4. Ejecute `grunt` (vea la información correspondiente más abajo) para buscar errores y otros problemas.
5. Describa el cambio en el mensaje de confirmación y haga referencia al vale, similar al siguiente: "Demostraciones: Error de delegado corregido para demostración de totales dinámicos. Corrige #51". Si va a agregar un nuevo archivo de localización, use algo parecido a esto: "Localización: Localización de agregado croata (HR)"

## <a name="build-setup"></a>Configuración de la compilación

1. Instale [NodeJS](http://nodejs.org).
2. Instale el script Grunt CLI To install mediante la ejecución de `npm install -g grunt-cli`. Encontrará más detalles en su sitio web, http://gruntjs.com/getting-started.
3. Instale las dependencias de NPM mediante la ejecución de `npm install`.
4. Ahora puede llamar a la compilación mediante la ejecución de `grunt`.

## <a name="creating-a-new-additional-method"></a>Creación de un método adicional

Si escribió métodos personalizados con los que desea contribuir a additional-methods.js:

1. Crear una bifurcación
2. Agregue el método como un archivo nuevo en `src/additional`.
3. (Opcional) Agregue traducciones a `src/localization`.
4. Envíe una solicitud de incorporación de cambios a la bifurcación principal.

## <a name="unit-tests"></a>Pruebas unitarias

Para ejecutar pruebas unitarias, solo tiene que abrir `test/index.html` en el explorador. Asegúrese de que ejecutó `npm install` antes de que todas las dependencias requeridas estén disponibles.
Comience con un explorador mientras desarrolla la corrección y después, ejecútela en otros antes de confirmar. Normalmente, se usan las últimas versiones de Chrome, Firefox, Safari y Opera y, con menos frecuencia, IE.

## <a name="documentation"></a>Documentación

Notifique los problemas de documentación en el controlador de problemas de [validación de jQuery](https://github.com/jzaefferer/jquery-validation/issues).
En caso de que la solicitud de incorporación de cambios implemente o cambie la API pública, sería importante que realice una solicitud de incorporación de cambios en el repositorio de [documentos de validación de jQuery](https://github.com/jzaefferer/validation-content).

## <a name="linting"></a>Detección de errores

Para ejecutar JSHint y otras herramientas, use `grunt`.
