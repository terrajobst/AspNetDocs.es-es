---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Validación de la entrada del usuario en sitios de ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: En este artículo se describe cómo validar la información que obtiene de los usuarios &mdash; es decir, para asegurarse de que los usuarios escriben información válida en formularios HTML en un AS...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: e6f8e1051d09d11f1756bfada44a73ba7c2a1db2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78454303"
---
# <a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a>Validación de la entrada del usuario en sitios de ASP.NET Web Pages (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se describe cómo validar la información que obtiene de los usuarios &mdash; es, para asegurarse de que los usuarios escriben información válida en formularios HTML en un sitio de ASP.NET Web Pages (Razor).
> 
> Temas que se abordarán:
> 
> - Cómo comprobar que la entrada de un usuario coincide con los criterios de validación que se definen.
> - Cómo determinar si se han superado todas las pruebas de validación.
> - Cómo mostrar los errores de validación (y cómo darles formato).
> - Cómo validar los datos que no proceden directamente de los usuarios.
> 
> Estos son los conceptos de programación de ASP.NET que se incluyen en el artículo:
> 
> - Aplicación auxiliar de `Validation`.
> - Los métodos `Html.ValidationSummary` y `Html.ValidationMessage`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2.

Este artículo contiene las siguientes secciones:

- [Información general sobre la validación de entradas de usuario](#Overview_of_User_Input_Validation)
- [Validación de los datos proporcionados por el usuario](#Validating_User_Input)
- [Agregar la validación del lado cliente](#Adding_Client-Side_Validation)
- [Dar formato a errores de validación](#Formatting_Validation_Errors)
- [Validar datos que no proceden directamente de los usuarios](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a>Información general sobre la validación de entradas de usuario

Si pide a los usuarios que escriban información en una página (por ejemplo, en un formulario), es importante asegurarse de que los valores que especifican son válidos. Por ejemplo, no desea procesar un formulario en el que falta información crítica.

Cuando los usuarios escriben valores en un formulario HTML, los valores que especifican son cadenas. En muchos casos, los valores que necesita son otros tipos de datos, como enteros o fechas. Por lo tanto, también tiene que asegurarse de que los valores especificados por los usuarios se pueden convertir correctamente en los tipos de datos adecuados.

También puede tener determinadas restricciones en los valores. Incluso si los usuarios escriben correctamente un entero, por ejemplo, puede que tenga que asegurarse de que el valor está dentro de un intervalo determinado.

![Errores de validación que usan clases de estilo CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> **Importante** La validación de los datos proporcionados por el usuario también es importante para la seguridad. Al restringir los valores que los usuarios pueden especificar en los formularios, se reduce la posibilidad de que alguien pueda especificar un valor que pueda poner en peligro la seguridad de su sitio.

<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a>Validar los datos introducidos por el usuario

En ASP.NET Web Pages 2, puede usar la aplicación auxiliar de `Validator` para probar los datos proporcionados por el usuario. El enfoque básico consiste en hacer lo siguiente:

1. Determine qué elementos de entrada (campos) desea validar.

    Normalmente, se validan los valores de `<input>` elementos de un formulario. Sin embargo, se recomienda validar todas las entradas, incluso las que proceden de un elemento restringido como una lista de `<select>`. Esto ayuda a asegurarse de que los usuarios no omiten los controles en una página y envían un formulario.
2. En el código de página, agregue comprobaciones de validación individuales para cada elemento de entrada mediante métodos de la aplicación auxiliar de `Validation`.

    Para comprobar los campos obligatorios, utilice `Validation.RequireField(field, [error message])` (para un campo individual) o `Validation.RequireFields(field1, field2, ...))` (para obtener una lista de campos). Para otros tipos de validación, use `Validation.Add(field, ValidationType)`. Por `ValidationType`, puede usar estas opciones:

    `Validator.DateTime ([error message])`  
   `Validator.Decimal([error message])`  
   `Validator.EqualsTo(otherField [, error message])`  
   `Validator.Float([error message])`  
   `Validator.Integer([error message])`  
   `Validator.Range(min, max [, error message])`  
   `Validator.RegEx(pattern [, error message])`  
   `Validator.Required([error message])`  
   `Validator.StringLength(length)`  
   `Validator.Url([error message])`
3. Cuando se envíe la página, compruebe si la validación se ha superado comprobando `Validation.IsValid`:

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    Si hay algún error de validación, se omite el procesamiento de páginas normal. Por ejemplo, si el propósito de la página es actualizar una base de datos, no lo hace hasta que se hayan corregido todos los errores de validación.
4. Si hay errores de validación, muestre los mensajes de error en el marcado de la página mediante `Html.ValidationSummary` o `Html.ValidationMessage`, o ambos.

En el ejemplo siguiente se muestra una página que muestra estos pasos.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

Para ver cómo funciona la validación, ejecute esta página y realice deliberadamente errores. Por ejemplo, este es el aspecto de la página si se olvida de escribir un nombre de curso, si escribe un y especifica una fecha no válida:

![Errores de validación en la página representada](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a>Agregar la validación del lado cliente

De forma predeterminada, los datos proporcionados por el usuario se validan después de que los usuarios envíen la página; es decir, la validación se realiza en el código del servidor. Una desventaja de este enfoque es que los usuarios no saben que han realizado un error hasta después de enviar la página. Si un formulario es largo o complejo, los errores de informes solo después de que se envíe la página pueden ser inadecuados para el usuario.

Puede Agregar compatibilidad para realizar la validación en el script de cliente. En ese caso, la validación se realiza a medida que los usuarios trabajan en el explorador. Por ejemplo, supongamos que especifica que un valor debe ser un entero. Si un usuario escribe un valor no entero, el error se indica en cuanto el usuario deja el campo de entrada. Los usuarios obtienen comentarios inmediatos, lo que resulta cómodo para ellos. La validación basada en el cliente también puede reducir el número de veces que el usuario tiene que enviar el formulario para corregir varios errores.

> [!NOTE]
> Incluso si utiliza la validación del lado cliente, la validación siempre se realiza también en el código del servidor. La realización de la validación en el código del servidor es una medida de seguridad, en caso de que los usuarios omitan la validación basada en el cliente.

1. Registre las siguientes bibliotecas de JavaScript en la página:  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   Dos de las bibliotecas se pueden cargar desde una red de entrega de contenido (CDN), por lo que no es necesario que las tenga en el equipo o servidor. Sin embargo, debe tener una copia local de *jQuery. Validate. discreto. js*. Si aún no está trabajando con una plantilla de WebMatrix (como el **sitio inicial** ) que incluye la biblioteca, cree un sitio de páginas web basado en el **sitio de inicio**. A continuación, copie el archivo *. js* en el sitio actual.
2. En el marcado, para cada elemento que se está validando, agregue una llamada a `Validation.For(field)`. Este método emite atributos que se usan en la validación del lado cliente. (En lugar de emitir código JavaScript real, el método emite atributos como `data-val-...`. Estos atributos admiten la validación discreta del cliente que usa jQuery para realizar el trabajo.

En la página siguiente se muestra cómo agregar características de validación de cliente al ejemplo mostrado anteriormente.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

No todas las comprobaciones de validación se ejecutan en el cliente. En concreto, la validación de tipo de datos (entero, fecha, etc.) no se ejecuta en el cliente. Las siguientes comprobaciones funcionan tanto en el cliente como en el servidor:

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

En este ejemplo, la prueba de una fecha válida no funcionará en el código de cliente. Sin embargo, la prueba se realizará en el código del servidor.

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a>Dar formato a errores de validación

Puede controlar cómo se muestran los errores de validación definiendo las clases CSS que tienen los siguientes nombres reservados:

- `field-validation-error`Operador Define la salida del método `Html.ValidationMessage` cuando se muestra un error.
- `field-validation-valid`Operador Define la salida del método `Html.ValidationMessage` cuando no hay ningún error.
- `input-validation-error`Operador Define cómo se representan los elementos de `<input>` cuando hay un error. (Por ejemplo, puede usar esta clase para establecer el color de fondo de una &lt;entrada&gt; elemento en un color diferente si su valor no es válido). Esta clase CSS solo se usa durante la validación de cliente (en ASP.NET Web Pages 2).
- `input-validation-valid`Operador Define la apariencia de los elementos de `<input>` cuando no hay ningún error.
- `validation-summary-errors`Operador Define la salida del método `Html.ValidationSummary` se muestra una lista de errores.
- `validation-summary-valid`Operador Define la salida del método `Html.ValidationSummary` cuando no hay ningún error.

En el siguiente bloque de `<style>` se muestran reglas para las condiciones de error.

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

Si incluye este bloque de estilo en las páginas de ejemplo de anteriormente en el artículo, la presentación del error tendrá el aspecto siguiente:

![Errores de validación que usan clases de estilo CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> Si no está utilizando la validación de cliente en ASP.NET Web Pages 2, las clases CSS para los elementos `<input>` (`input-validation-error` y `input-validation-valid` no tienen ningún efecto.

### <a name="static-and-dynamic-error-display"></a>Presentación de errores estáticos y dinámicos

Las reglas de CSS se encuentran en pares, como `validation-summary-errors` y `validation-summary-valid`. Estos pares permiten definir reglas para ambas condiciones: una condición de error y una condición "normal" (sin errores). Es importante comprender que el marcado de la presentación de errores siempre se representa, incluso si no hay ningún error. Por ejemplo, si una página tiene un método `Html.ValidationSummary` en el marcado, el origen de la página contendrá el marcado siguiente incluso cuando se solicite la página por primera vez:

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

En otras palabras, el método `Html.ValidationSummary` siempre representa un elemento `<div>` y una lista, incluso si la lista de errores está vacía. Del mismo modo, el método `Html.ValidationMessage` siempre representa un elemento `<span>` como un marcador de posición para un error de campo individual, incluso si no hay ningún error.

En algunas situaciones, mostrar un mensaje de error puede hacer que la página se retransmita y puede hacer que los elementos de la página se desplacen. Las reglas de CSS que acaban en `-valid` permiten definir un diseño que puede ayudar a evitar este problema. Por ejemplo, puede definir `field-validation-error` y `field-validation-valid` tienen el mismo tamaño fijo. De este modo, el área de visualización del campo es estática y no cambiará el flujo de la página si se muestra un mensaje de error.

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a>Validar datos que no proceden directamente de los usuarios

A veces, es necesario validar la información que no procede directamente de un formulario HTML. Un ejemplo típico es una página en la que se pasa un valor en una cadena de consulta, como en el ejemplo siguiente:

`http://server/myapp/EditClassInformation?classid=1022`

En este caso, querrá asegurarse de que el valor que se pasa a la página (aquí, 1022 para el valor de `classid`) es válido. No se puede usar directamente la aplicación auxiliar de `Validation` para realizar esta validación. Sin embargo, puede usar otras características del sistema de validación, como la capacidad de mostrar mensajes de error de validación.

> [!NOTE] 
> 
> **Importante** Valide siempre los valores que obtiene de *cualquier* origen, incluidos los valores de campo de formulario, los valores de cadena de consulta y los valores de cookie. Es fácil para los usuarios cambiar estos valores (quizás con fines malintencionados). Por lo tanto, debe comprobar estos valores para proteger la aplicación.

En el ejemplo siguiente se muestra cómo se puede validar un valor que se pasa en una cadena de consulta. El código prueba que el valor no está vacío y es un entero.

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

Tenga en cuenta que la prueba se realiza cuando la solicitud no es un envío de formulario (`if(!IsPost)`). Esta prueba se pasaría la primera vez que se solicita la página, pero no cuando la solicitud es un envío de formulario.

Para mostrar este error, puede Agregar el error a la lista de errores de validación mediante una llamada a `Validation.AddFormError("message")`. Si la página contiene una llamada al método `Html.ValidationSummary`, el error se muestra allí, como si se tratara de un error de validación de entrada del usuario.

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Recursos adicionales

[Trabajar con formularios HTML en sitios de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkID=202892)
