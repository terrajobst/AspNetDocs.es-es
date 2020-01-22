---
uid: mvc/overview/getting-started/introduction/adding-validation
title: Agregando validación | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/06/2019
ms.assetid: 9f35ca15-e216-4db6-9ebf-24380b0f31b4
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-validation
msc.type: authoredcontent
ms.openlocfilehash: 67df1a473cd13a651c1276054b93f34323479082
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519029"
---
# <a name="adding-validation"></a>Agregar una validación

por [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](index.md)]

En esta sección, agregará la lógica de validación al modelo de `Movie` y se asegurará de que las reglas de validación se apliquen siempre que un usuario intente crear o editar una película mediante la aplicación.

## <a name="keeping-things-dry"></a>Mantener todo seco

Uno de los principios básicos del diseño de ASP.NET MVC está [seco](http://en.wikipedia.org/wiki/Don't_repeat_yourself) (&quot;una vez y solo una&quot;). ASP.NET MVC le anima a que especifique la funcionalidad o el comportamiento solo una vez y, a continuación, haga que se refleje en todas partes en una aplicación. Esto reduce la cantidad de código que necesita escribir y hace que el código que se escribe sea menos propenso a errores y más fácil de mantener.

La compatibilidad de validación proporcionada por ASP.NET MVC y Entity Framework Code First es un buen ejemplo del principio seco en acción. Puede especificar mediante declaración las reglas de validación en un solo lugar (en la clase de modelo) y las reglas se aplican en todas partes de la aplicación.

Echemos un vistazo a cómo puede aprovechar esta compatibilidad de validación en la aplicación de películas.

## <a name="adding-validation-rules-to-the-movie-model"></a>Agregar reglas de validación al modelo de película

Empezará agregando una lógica de validación a la clase `Movie`.

Abra el archivo *Movie.cs*. Observe que el espacio de nombres [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) no contiene `System.Web`. DataAnnotations proporciona un conjunto integrado de atributos de validación que puede aplicar mediante declaración a cualquier clase o propiedad. (También contiene atributos de formato como [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) que ayudan a dar formato y no proporcionan ninguna validación).

Ahora, actualice la clase `Movie` para aprovechar los atributos de validación [`Required`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [`StringLength`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)y [`Range`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) integrados. Reemplace la `Movie` clase por lo siguiente:

[!code-csharp[Main](adding-validation/samples/sample1.cs?highlight=5,13-15,18-19,22-23)]

El atributo [`StringLength`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) establece la longitud máxima de la cadena y establece esta limitación en la base de datos, por lo que el esquema de la base de datos cambiará. Haga clic con el botón derecho en la tabla **películas** en el **Explorador de servidores** y haga clic en **abrir definición de tabla**:

![](adding-validation/_static/image1.png)

En la imagen anterior, puede ver que todos los campos de cadena están establecidos en [nvarchar (Max)](https://technet.microsoft.com/library/ms186939.aspx). Usaremos las migraciones para actualizar el esquema. Compile la solución y, a continuación, abra la ventana de la **consola del administrador de paquetes** y escriba los siguientes comandos:

[!code-console[Main](adding-validation/samples/sample2.cmd)]

Cuando finaliza este comando, Visual Studio abre el archivo de clase que define el nuevo `DbMigration` clase derivada con el nombre especificado (`DataAnnotations`) y, en el método `Up`, puede ver el código que actualiza las restricciones de esquema:

[!code-csharp[Main](adding-validation/samples/sample3.cs)]

El campo `Genre` ya no acepta valores NULL (es decir, debe escribir un valor). El campo `Rating` tiene una longitud máxima de 5 y `Title` tiene una longitud máxima de 60. La longitud mínima de 3 en `Title` y el intervalo en `Price` no creó cambios de esquema.

Examine el esquema de la película:

![](adding-validation/_static/image2.png)

Los campos de cadena muestran los nuevos límites de longitud y `Genre` ya no se comprueban como valores NULL.

Los atributos de validación especifican el comportamiento que quiere aplicar en las propiedades del modelo al que se aplican. Los atributos `Required` y `MinimumLength` indican que una propiedad debe tener un valor, pero nada evita que un usuario escriba espacios en blanco para satisfacer esta validación. El atributo [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) se usa para limitar los caracteres que se pueden introducir. En el código anterior, `Genre` y `Rating` solamente pueden usar letras (no se permiten espacios en blanco, números ni caracteres especiales). El atributo [`Range`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) restringe un valor a dentro de un intervalo especificado. El atributo `StringLength` permite establecer la longitud máxima de una propiedad de cadena y, opcionalmente, su longitud mínima. Los tipos de valor (como `decimal, int, float, DateTime`) son inherentemente necesarios y no necesitan el atributo `Required`.

Code First garantiza que las reglas de validación que especifique en una clase de modelo se aplican antes de que la aplicación guarde los cambios en la base de datos. Por ejemplo, el código siguiente producirá una excepción [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception(v=vs.103).aspx) cuando se llame al método `SaveChanges`, porque faltan varios valores de propiedad `Movie` necesarios:

[!code-csharp[Main](adding-validation/samples/sample4.cs)]

El código anterior produce la siguiente excepción:

*Error de validación de una o más entidades. Vea la propiedad ' EntityValidationErrors ' para obtener más detalles.*

El hecho de que las reglas de validación las aplique automáticamente el .NET Framework ayuda a que la aplicación sea más sólida. También nos permite asegurarnos de que todo se valida y que no nos dejamos ningún dato incorrecto en la base de datos accidentalmente.

## <a name="validation-error-ui-in-aspnet-mvc"></a>Interfaz de usuario de error de validación en ASP.NET MVC

Ejecute la aplicación y vaya a la dirección URL de */Movies* .

Haga clic en el vínculo **crear nuevo** para agregar una nueva película. Rellene el formulario con algunos valores no válidos. En cuanto la validación del lado cliente de jQuery detecta el problema, muestra un mensaje de error.

![8_validationErrors](adding-validation/_static/image3.png)

> [!NOTE]
> para admitir la validación de jQuery para configuraciones regionales distintas del inglés que usan una coma (",") para un separador decimal, debe incluir la globalización de NuGet tal como se describió anteriormente en este tutorial.

Observe cómo el formulario ha utilizado automáticamente un color de borde rojo para resaltar los cuadros de texto que contienen datos no válidos y ha emitido un mensaje de error de validación adecuado junto a cada uno de ellos. Los errores se aplican en el lado cliente (con JavaScript y jQuery) y en el lado servidor (cuando un usuario tiene JavaScript deshabilitado).

Una ventaja real es que no necesitaba cambiar una sola línea de código en la clase `MoviesController` o en la vista *Create. cshtml* para habilitar esta interfaz de usuario de validación. El controlador y las vistas que creó en pasos anteriores de este tutorial seleccionaron automáticamente las reglas de validación que especificó mediante atributos de validación en las propiedades de la clase del modelo `Movie`. Pruebe la aplicación mediante el método de acción `Edit` y se aplicará la misma validación.

Los datos del formulario no se envían al servidor hasta que no hay ningún error de validación del lado cliente. Puede comprobarlo colocando un punto de interrupción en el método HTTP POST, mediante la [herramienta Fiddler](http://fiddler2.com/fiddler2/)o las herramientas de [desarrollo F12](https://msdn.microsoft.com/ie/aa740478)de IE.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Cómo se produce la validación en el método Create View y Create Action

Tal vez se pregunte cómo se generó la validación de la interfaz de usuario sin actualizar el código en el controlador o las vistas. La siguiente lista muestra el aspecto de los métodos de `Create` en la clase `MovieController`. No se modifican de la forma en que se crearon anteriormente en este tutorial.

[!code-csharp[Main](adding-validation/samples/sample5.cs)]

El primer método de acción `Create` (HTTP GET) muestra el formulario de creación inicial. La segunda versión (`[HttpPost]`) controla el envío de formulario. El segundo método `Create` (la versión `HttpPost`) comprueba `ModelState.IsValid` para ver si la película tiene errores de validación. Al obtener esta propiedad se evalúan todos los atributos de validación que se han aplicado al objeto. Si el objeto tiene errores de validación, el método `Create` vuelve a mostrar el formulario. Si no hay ningún error, el método guarda la nueva película en la base de datos. En nuestro ejemplo de película, **el formulario no se publica en el servidor cuando se detectan errores de validación en el lado cliente;** **nunca se llama**al segundo método `Create`. Si deshabilita JavaScript en el explorador, la validación del cliente está deshabilitada y el método HTTP POST `Create` obtiene `ModelState.IsValid` para comprobar si la película tiene errores de validación.

Puede establecer un punto de interrupción en el método `HttpPost Create` y comprobar si nunca se llama al método. La validación del lado cliente no enviará los datos del formulario cuando se detecten errores de validación. Si deshabilita JavaScript en el explorador y después envía el formulario con errores, se alcanzará el punto de interrupción. Puede seguir obteniendo validación completa sin JavaScript. En la imagen siguiente se muestra cómo deshabilitar JavaScript en Internet Explorer.

![](adding-validation/_static/image5.png)

![](adding-validation/_static/image6.png)

En la siguiente imagen se muestra cómo deshabilitar JavaScript en el explorador Firefox.

![](adding-validation/_static/image7.png)

En la siguiente imagen se muestra cómo deshabilitar JavaScript en el explorador Chrome.

![](adding-validation/_static/image8.png)

A continuación se muestra la plantilla de vista *Create. cshtml* que se ha aplicado con scaffolding anteriormente en el tutorial. Los métodos de acción que se muestran arriba la usan para mostrar el formulario inicial y para volver a mostrarlo en caso de error.

[!code-cshtml[Main](adding-validation/samples/sample6.cshtml?highlight=16-17)]

Observe cómo el código utiliza una aplicación auxiliar de `Html.EditorFor` para generar el elemento `<input>` para cada propiedad `Movie`. Junto a esta aplicación auxiliar es una llamada al método auxiliar `Html.ValidationMessageFor`. Estos dos métodos auxiliares funcionan con el objeto de modelo que pasa el controlador a la vista (en este caso, un objeto `Movie`). Buscan automáticamente los atributos de validación especificados en el modelo y muestran los mensajes de error según corresponda.

Lo realmente bueno de este enfoque es que ni el controlador ni la plantilla de vista `Create` saben que las reglas de validación actuales se están aplicando ni conocen los mensajes de error específicos que se muestran. Las reglas de validación y las cadenas de error solo se especifican en la clase `Movie`. Estas mismas reglas de validación se aplican automáticamente a la vista `Edit` y a cualquier otra vista de plantillas creada que edite el modelo.

Si desea cambiar la lógica de validación más adelante, puede hacerlo en exactamente un lugar agregando atributos de validación al modelo (en este ejemplo, la clase `movie`). No tendrá que preocuparse de que diferentes partes de la aplicación sean incoherentes con el modo en que se aplican las reglas: toda la lógica de validación se definirá en un solo lugar y se usará en todas partes. Esto mantiene el código muy limpio y hace que sea fácil de mantener y evolucionar. Además, significa que se respetará por completo el principio *seco* .

## <a name="using-datatype-attributes"></a>Uso de atributos DataType

Abra el archivo *Movie.cs* y examine la clase `Movie`. El espacio de nombres [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) proporciona atributos de formato además del conjunto integrado de atributos de validación. Ya hemos aplicado un valor de enumeración [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) a la fecha de lanzamiento y a los campos de precio. En el código siguiente se muestran las propiedades `ReleaseDate` y `Price` con el atributo [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) adecuado.

[!code-csharp[Main](adding-validation/samples/sample7.cs)]

Los atributos [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) solo proporcionan sugerencias para que el motor de vista aplique formato a los datos (y proporcione atributos como `<a>` para las direcciones URL y `<a href="mailto:EmailAddress.com">` para el correo electrónico. Puede usar el atributo [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) para validar el formato de los datos. El atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) se utiliza para especificar un tipo de datos más específico que el tipo intrínseco de la base de datos, ***no*** son atributos de validación. En este caso solo se quiere realizar el seguimiento de la fecha, no de la fecha y la hora. La [enumeración DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) proporciona muchos tipos de datos, como *Date, Time, PhoneNumber, Currency, EmailAddress* , etc. El atributo `DataType` también puede permitir que la aplicación proporcione automáticamente características específicas del tipo. Por ejemplo, se puede crear un vínculo de `mailto:` para [DataType. EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)y se puede proporcionar un selector de fecha para [DataType. Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) en exploradores compatibles con [HTML5](http://html5.org/). Los atributos [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) emiten atributos [de datos](http://ejohn.org/blog/html-5-data-attributes/) HTML 5 (pronunciados con *guiones de datos*) que los exploradores HTML 5 pueden comprender. Los atributos [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) no proporcionan ninguna validación.

`DataType.Date` no especifica el formato de la fecha que se muestra. De forma predeterminada, el campo de datos se muestra según los formatos predeterminados basados en el objeto [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx)del servidor.

El atributo `DisplayFormat` se usa para especificar el formato de fecha de forma explícita:

[!code-csharp[Main](adding-validation/samples/sample8.cs)]

El valor `ApplyFormatInEditMode` especifica que el formato especificado también debe aplicarse cuando el valor se muestra en un cuadro de texto para su edición. (Es posible que no desee que en algunos campos, por ejemplo, para los valores de moneda, no desee que el símbolo de moneda en el cuadro de texto se edite).

Puede usar el atributo [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) por sí solo, pero normalmente es conveniente usar también el atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) . El atributo `DataType` transmite la *semántica* de los datos en lugar de cómo representarlos en una pantalla y ofrece las siguientes ventajas que no se obtienen con `DisplayFormat`:

- El explorador puede habilitar características de HTML5 (por ejemplo, para mostrar un control de calendario, el símbolo de divisa adecuado para la configuración regional, los vínculos de correo electrónico, etc.).
- De forma predeterminada, el explorador representará los datos con el formato correcto en función de la [configuración regional](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- El atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) puede habilitar MVC para elegir la plantilla de campo correcta para representar los datos (el [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) , si se usa por sí solo, usa la plantilla de cadena). Para obtener más información, consulte [las plantillas de ASP.NET MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html)de Brad Wilson. (Aunque está escrito para MVC 2, este artículo se aplica a la versión actual de ASP.NET MVC).

Si utiliza el atributo `DataType` con un campo de fecha, tendrá que especificar el atributo `DisplayFormat` también para asegurarse de que el campo se representa correctamente en los exploradores Chrome. Para obtener más información, vea [este subproceso de stackoverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

> [!NOTE]
> la validación de jQuery no funciona con el atributo [Range](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) y la [fecha y hora](https://msdn.microsoft.com/library/system.datetime.aspx). Por ejemplo, el código siguiente siempre muestra un error de validación del lado cliente, incluso cuando la fecha está en el intervalo especificado:
> 
> [!code-csharp[Main](adding-validation/samples/sample9.cs)]
> 
> Tendrá que deshabilitar la validación de fecha de jQuery para usar el atributo de [intervalo](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) con [fecha y hora](https://msdn.microsoft.com/library/system.datetime.aspx). Por lo general, no es recomendable compilar fechas rígidas en los modelos, por lo que no se recomienda usar el atributo de [intervalo](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) y la [fecha y hora](https://msdn.microsoft.com/library/system.datetime.aspx) .

El código siguiente muestra la combinación de atributos en una línea:

[!code-csharp[Main](adding-validation/samples/sample10.cs?highlight=4,6,10,12)]

En la siguiente parte de la serie de tutoriales, revisaremos la aplicación y realizaremos algunas mejoras a los métodos `Details` y `Delete` generados automáticamente.

> [!div class="step-by-step"]
> [Anterior](adding-a-new-field.md)
> [Siguiente](examining-the-details-and-delete-methods.md)
