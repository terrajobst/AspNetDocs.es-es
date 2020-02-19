---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
title: Agregar validación al modelo | Microsoft Docs
author: Rick-Anderson
description: 'Nota: hay disponible una versión actualizada de este tutorial que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro, mucho más fácil de seguir y demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 5d9a2999-fcc4-4c45-a018-271fddf74a3b
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: c9f6699c5d3500d4c1fcade9252aeb9dd92983da
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77455963"
---
# <a name="adding-validation-to-the-model"></a>Agregar la validación al modelo

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > [Hay disponible una](../../getting-started/introduction/getting-started.md) versión actualizada de este tutorial que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro, mucho más fácil de seguir y demuestra más características.

En esta sección, agregará la lógica de validación al modelo de `Movie` y se asegurará de que las reglas de validación se apliquen siempre que un usuario intente crear o editar una película mediante la aplicación.

## <a name="keeping-things-dry"></a>Mantener todo seco

Uno de los principios básicos del diseño de ASP.NET MVC está seco (&quot;Una vez y solo una&quot;). ASP.NET MVC le anima a que especifique la funcionalidad o el comportamiento solo una vez y, a continuación, haga que se refleje en todas partes en una aplicación. Esto reduce la cantidad de código que necesita escribir y hace que el código que se escribe sea menos propenso a errores y más fácil de mantener.

La compatibilidad de validación proporcionada por ASP.NET MVC y Entity Framework Code First es un buen ejemplo del principio seco en acción. Puede especificar mediante declaración las reglas de validación en un solo lugar (en la clase de modelo) y las reglas se aplican en todas partes de la aplicación.

Echemos un vistazo a cómo puede aprovechar esta compatibilidad de validación en la aplicación de películas.

## <a name="adding-validation-rules-to-the-movie-model"></a>Agregar reglas de validación al modelo de película

Empezará agregando una lógica de validación a la clase `Movie`.

Abra el archivo *Movie.cs*. Agregue una instrucción `using` en la parte superior del archivo que hace referencia al espacio de nombres [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) :

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

Observe que el espacio de nombres no contiene `System.Web`. DataAnnotations proporciona un conjunto integrado de atributos de validación que puede aplicar mediante declaración a cualquier clase o propiedad.

Ahora, actualice la clase `Movie` para aprovechar los atributos de validación [`Required`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [`StringLength`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)y [`Range`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) integrados. Use el código siguiente como ejemplo de dónde aplicar los atributos.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs?highlight=4,10,13,17)]

Ejecute la aplicación y volverá a obtener el siguiente error en tiempo de ejecución:

***El modelo de respaldo del contexto ' MovieDBContext ' ha cambiado desde que se creó la base de datos. Considere la posibilidad de usar Migraciones de Code First para actualizar la base de datos ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).***

Usaremos las migraciones para actualizar el esquema. Compile la solución y, a continuación, abra la ventana de la **consola del administrador de paquetes** y escriba los siguientes comandos:

[!code-console[Main](adding-validation-to-the-model/samples/sample3.cmd)]

Cuando finaliza este comando, Visual Studio abre el archivo de clase que define el nuevo `DbMigration` clase derivada con el nombre especificado (*AddDataAnnotationsMig*) y en el método `Up` puede ver el código que actualiza las restricciones de esquema. Los campos `Title` y `Genre` ya no aceptan valores NULL (es decir, debe escribir un valor) y el campo `Rating` tiene una longitud máxima de 5.

Los atributos de validación especifican el comportamiento que quiere aplicar en las propiedades del modelo al que se aplican. El atributo `Required` indica que una propiedad debe tener un valor; en este ejemplo, una película tiene que tener valores para las propiedades `Title`, `ReleaseDate`, `Genre`y `Price` para ser válidos. El atributo `Range` restringe un valor a un intervalo determinado. El atributo `StringLength` permite establecer la longitud máxima de una propiedad de cadena y, opcionalmente, su longitud mínima. Los tipos intrínsecos (como `decimal, int, float, DateTime`) son necesarios de forma predeterminada y no necesitan el atributo `Required`.

Code First garantiza que las reglas de validación que especifique en una clase de modelo se aplican antes de que la aplicación guarde los cambios en la base de datos. Por ejemplo, el código siguiente producirá una excepción cuando se llame al método `SaveChanges`, porque faltan algunos valores de propiedad `Movie` obligatorios y el precio es cero (que está fuera del intervalo válido).

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs?highlight=7-8)]

El hecho de que las reglas de validación las aplique automáticamente el .NET Framework ayuda a que la aplicación sea más sólida. También nos permite asegurarnos de que todo se valida y que no nos dejamos ningún dato incorrecto en la base de datos accidentalmente.

Esta es una lista de código completa para el archivo *Movie.CS* actualizado:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Interfaz de usuario de error de validación en ASP.NET MVC

Vuelva a ejecutar la aplicación y vaya a la dirección URL de */Movies* .

Haga clic en el vínculo **crear nuevo** para agregar una nueva película. Rellene el formulario con algunos valores no válidos y, a continuación, haga clic en el botón **crear** .

![8_validationErrors](adding-validation-to-the-model/_static/image1.png)

> [!NOTE]
> para admitir la validación de jQuery para configuraciones regionales distintas del inglés que usan una coma (&quot;,&quot;) para un separador decimal, debe incluir *Globalization. js* y el archivo de *referencias culturales/Globalization. culturas. js* (de [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) y JavaScript específicos para usar `Globalize.parseFloat`. En el código siguiente se muestran las modificaciones en el archivo Views\Movies\Edit.cshtml para trabajar con el &quot;fr-FR&quot; referencia cultural:

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Observe cómo el formulario ha utilizado automáticamente un color de borde rojo para resaltar los cuadros de texto que contienen datos no válidos y ha emitido un mensaje de error de validación adecuado junto a cada uno de ellos. Los errores se aplican en el lado cliente (con JavaScript y jQuery) y en el lado servidor (cuando un usuario tiene JavaScript deshabilitado).

Una ventaja real es que no necesitaba cambiar una sola línea de código en la clase `MoviesController` o en la vista *Create. cshtml* para habilitar esta interfaz de usuario de validación. El controlador y las vistas que creó en pasos anteriores de este tutorial seleccionaron automáticamente las reglas de validación que especificó mediante atributos de validación en las propiedades de la clase del modelo `Movie`.

Es posible que haya observado que las propiedades `Title` y `Genre`, el atributo necesario no se aplica hasta que envíe el formulario (pulse el botón **crear** ) o escriba texto en el campo de entrada y lo elimine. Para un campo que está inicialmente vacío (como los campos de la vista de creación) y que solo tiene el atributo required y ningún otro atributo de validación, puede hacer lo siguiente para desencadenar la validación:

1. En el campo.
2. Escriba algún texto.
3. Presione TAB para salir del campo.
4. Vuelva a la pestaña en el campo.
5. Quite el texto.
6. Presione TAB para salir del campo.

La secuencia anterior desencadenará la validación necesaria sin pulsar el botón Enviar. Al pulsar el botón Enviar sin escribir ninguno de los campos, se desencadenará la validación del lado cliente. Los datos del formulario no se envían al servidor hasta que no hay ningún error de validación del lado cliente. Para probar esto, coloque un punto de interrupción en el método HTTP POST o use la [herramienta Fiddler](http://fiddler2.com/fiddler2/) o las [herramientas de desarrollo](https://msdn.microsoft.com/ie/aa740478)de IE 9 F12.

![](adding-validation-to-the-model/_static/image2.png)

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Cómo se produce la validación en el método Create View y Create Action

Tal vez se pregunte cómo se generó la validación de la interfaz de usuario sin actualizar el código en el controlador o las vistas. La siguiente lista muestra el aspecto de los métodos de `Create` en la clase `MovieController`. No se modifican de la forma en que se crearon anteriormente en este tutorial.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs?highlight=12,15)]

El primer método de acción `Create` (HTTP GET) muestra el formulario de creación inicial. La segunda versión (`[HttpPost]`) controla el envío de formulario. El segundo método `Create` (la versión `HttpPost`) llama a `ModelState.IsValid` para comprobar si la película tiene errores de validación. Al llamar a este método se evalúan todos los atributos de validación que se hayan aplicado al objeto. Si el objeto tiene errores de validación, el método `Create` vuelve a mostrar el formulario. Si no hay ningún error, el método guarda la nueva película en la base de datos. En nuestro ejemplo de película, **el formulario no se publica en el servidor cuando se detectan errores de validación en el lado cliente;** **nunca se llama**al segundo método `Create`. Si deshabilita JavaScript en el explorador, la validación del cliente está deshabilitada y el método HTTP POST `Create` llama `ModelState.IsValid` para comprobar si la película tiene errores de validación.

Puede establecer un punto de interrupción en el método `HttpPost Create` y comprobar si nunca se llama al método. La validación del lado cliente no enviará los datos del formulario cuando se detecten errores de validación. Si deshabilita JavaScript en el explorador y después envía el formulario con errores, se alcanzará el punto de interrupción. Puede seguir obteniendo validación completa sin JavaScript. En la imagen siguiente se muestra cómo deshabilitar JavaScript en Internet Explorer.

![](adding-validation-to-the-model/_static/image3.png)

![](adding-validation-to-the-model/_static/image4.png)

En la siguiente imagen se muestra cómo deshabilitar JavaScript en el explorador Firefox.

![](adding-validation-to-the-model/_static/image5.png)

En la imagen siguiente se muestra cómo deshabilitar JavaScript con el explorador Chrome.

![](adding-validation-to-the-model/_static/image6.png)

A continuación se muestra la plantilla de vista *Create. cshtml* que se ha aplicado con scaffolding anteriormente en el tutorial. Los métodos de acción que se muestran arriba la usan para mostrar el formulario inicial y para volver a mostrarlo en caso de error.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample8.cshtml?highlight=22-23,30-31,38-39,46-47)]

Observe cómo el código utiliza una aplicación auxiliar de `Html.EditorFor` para generar el elemento `<input>` para cada propiedad `Movie`. Junto a esta aplicación auxiliar es una llamada al método auxiliar `Html.ValidationMessageFor`. Estos dos métodos auxiliares funcionan con el objeto de modelo que pasa el controlador a la vista (en este caso, un objeto `Movie`). Buscan automáticamente los atributos de validación especificados en el modelo y muestran los mensajes de error según corresponda.

Lo que es muy útil para este enfoque es que ni el controlador ni la plantilla de creación de vistas saben nada sobre las reglas de validación reales que se aplican o sobre los mensajes de error específicos que se muestran. Las reglas de validación y las cadenas de error solo se especifican en la clase `Movie`. Estas mismas reglas de validación se aplican automáticamente a la vista de edición y a cualquier otra plantilla de vistas que se puede crear y editar el modelo.

Si desea cambiar la lógica de validación más adelante, puede hacerlo en exactamente un lugar agregando atributos de validación al modelo (en este ejemplo, la clase `movie`). No tendrá que preocuparse de que diferentes partes de la aplicación sean incoherentes con el modo en que se aplican las reglas: toda la lógica de validación se definirá en un solo lugar y se usará en todas partes. Esto mantiene el código muy limpio y hace que sea fácil de mantener y evolucionar. También significa que respeta totalmente el principio DRY.

## <a name="adding-formatting-to-the-movie-model"></a>Agregar formato al modelo de película

Abra el archivo *Movie.cs* y examine la clase `Movie`. El espacio de nombres [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) proporciona atributos de formato además del conjunto integrado de atributos de validación. Ya hemos aplicado un valor de enumeración [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) a la fecha de lanzamiento y a los campos de precio. En el código siguiente se muestran las propiedades `ReleaseDate` y `Price` con el atributo [`DisplayFormat`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) adecuado.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

Los atributos [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) no son atributos de validación, sino que se usan para indicar al motor de vistas cómo representar el código HTML. En el ejemplo anterior, el atributo `DataType.Date` muestra las fechas de la película solo como fechas, sin hora. Por ejemplo, los siguientes atributos de [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) no validan el formato de los datos:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Los atributos enumerados anteriormente solo proporcionan sugerencias para que el motor de vistas dé formato a los datos (y proporcione atributos como &lt;un&gt; para las direcciones URL y &lt;a href =&quot;mailto:EmailAddress. com&quot;&gt; para el correo electrónico. Puede usar el atributo [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) para validar el formato de los datos.

Un enfoque alternativo al uso de los atributos de `DataType`, podría establecer explícitamente un valor de [`DataFormatString`](https://msdn.microsoft.com/library/system.string.format.aspx) . En el código siguiente se muestra la propiedad Date Release con una cadena de formato de fecha (es decir, &quot;d&quot;). Se usaría para especificar que no desea tiempo como parte de la fecha de lanzamiento.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample11.cs)]

A continuación se muestra la clase `Movie` completa.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample12.cs)]

Ejecute la aplicación y vaya al controlador de `Movies`. La fecha y el precio de lanzamiento tienen un formato correcto. En la imagen siguiente se muestra la fecha y el precio de lanzamiento mediante &quot;&quot; fr-FR como referencia cultural.

![8_format_SM](adding-validation-to-the-model/_static/image7.png)

En la imagen siguiente se muestran los mismos datos que se muestran con la referencia cultural predeterminada (Inglés de EE. UU.).

![](adding-validation-to-the-model/_static/image8.png)

En la siguiente parte de la serie de tutoriales, revisaremos la aplicación y realizaremos algunas mejoras a los métodos `Details` y `Delete` generados automáticamente.

> [!div class="step-by-step"]
> [Anterior](adding-a-new-field-to-the-movie-model-and-table.md)
> [Siguiente](examining-the-details-and-delete-methods.md)
