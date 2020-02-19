---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
title: Agregar la validación al modelo (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web MVC de ASP.NET con Microsoft Visual Web Developer 2010 Express Service Pack 1, que es...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 878f6c31-972d-45f4-8849-5c633b511409
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 7f3f195bc30ed23a637b59f15e6fc8431e39e217
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457210"
---
# <a name="adding-validation-to-the-model-vb"></a>Agregar la validación al modelo (VB)

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web MVC de ASP.NET con Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos que se enumeran a continuación. Para instalar todos ellos, haga clic en el vínculo siguiente: [instalador de plataforma web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar individualmente los requisitos previos con los siguientes vínculos:
> 
> - [Requisitos previos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(compatibilidad con herramientas y tiempo de ejecución)
> 
> Si utiliza Visual Studio 2010 en lugar de Visual Web Developer 2010, instale los requisitos previos haciendo clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un proyecto de Visual Web Developer con código fuente de VB.NET está disponible para acompañar este tema. [Descargue la versión de VB.net](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si lo prefiere C#, cambie a la [ C# versión](../cs/adding-validation-to-the-model.md) de este tutorial.

En esta sección, agregará la lógica de validación al modelo de `Movie` y se asegurará de que las reglas de validación se apliquen siempre que un usuario intente crear o editar una película mediante la aplicación.

## <a name="keeping-things-dry"></a>Mantener todo seco

Uno de los principios de diseño principales de ASP.NET MVC está seco ("Una vez y solo una"). ASP.NET MVC le anima a que especifique la funcionalidad o el comportamiento solo una vez y, a continuación, haga que se refleje en todas partes en una aplicación. Esto reduce la cantidad de código que necesita escribir y hace que el código que se escribe sea mucho más fácil de mantener.

La compatibilidad de validación proporcionada por ASP.NET MVC y Entity Framework Code First es un buen ejemplo del principio seco en acción. Puede especificar mediante declaración las reglas de validación en un solo lugar (en la clase de modelo) y, a continuación, esas reglas se aplican en todas partes de la aplicación.

Echemos un vistazo a cómo puede aprovechar esta compatibilidad de validación en la aplicación de películas.

## <a name="adding-validation-rules-to-the-movie-model"></a>Agregar reglas de validación al modelo de película

Empezará agregando una lógica de validación a la clase `Movie`.

Abra el archivo *Movie. VB* . Agregue una instrucción `Imports` en la parte superior del archivo que hace referencia al espacio de nombres [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) :

[!code-vb[Main](adding-validation-to-the-model/samples/sample1.vb)]

El espacio de nombres forma parte de la .NET Framework. Proporciona un conjunto integrado de atributos de validación que puede aplicar mediante declaración a cualquier clase o propiedad.

Ahora, actualice la clase `Movie` para aprovechar los atributos de validación [`Required`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [`StringLength`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)y [`Range`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) integrados. Use el código siguiente como ejemplo de dónde aplicar los atributos.

[!code-vb[Main](adding-validation-to-the-model/samples/sample2.vb)]

Los atributos de validación especifican el comportamiento que quiere aplicar en las propiedades del modelo al que se aplican. El atributo `Required` indica que una propiedad debe tener un valor; en este ejemplo, una película tiene que tener valores para las propiedades `Title`, `ReleaseDate`, `Genre`y `Price` para ser válidos. El atributo `Range` restringe un valor a un intervalo determinado. El atributo `StringLength` permite establecer la longitud máxima de una propiedad de cadena y, opcionalmente, su longitud mínima.

Code First garantiza que las reglas de validación que especifique en una clase de modelo se aplican antes de que la aplicación guarde los cambios en la base de datos. Por ejemplo, el código siguiente producirá una excepción cuando se llame al método `SaveChanges`, porque faltan algunos valores de propiedad `Movie` obligatorios y el precio es cero (que está fuera del intervalo válido).

[!code-vb[Main](adding-validation-to-the-model/samples/sample3.vb)]

El hecho de que las reglas de validación las aplique automáticamente el .NET Framework ayuda a que la aplicación sea más sólida. También nos permite asegurarnos de que todo se valida y que no nos dejamos ningún dato incorrecto en la base de datos accidentalmente.

Esta es una lista de código completa para el archivo *Movie. VB* actualizado:

[!code-vb[Main](adding-validation-to-the-model/samples/sample4.vb)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Interfaz de usuario de error de validación en ASP.NET MVC

Vuelva a ejecutar la aplicación y vaya a la dirección URL de */Movies* .

Haga clic en el vínculo **crear película** para agregar una nueva película. Rellene el formulario con algunos valores no válidos y, a continuación, haga clic en el botón **crear** .

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

Observe cómo el formulario ha utilizado automáticamente un color de fondo para resaltar los cuadros de texto que contienen datos no válidos y ha emitido un mensaje de error de validación adecuado junto a cada uno de ellos. Los mensajes de error coinciden con las cadenas de error especificadas al anotar la clase `Movie`. Los errores se aplican tanto en el lado cliente (mediante JavaScript) como en el lado servidor (en caso de que un usuario tenga JavaScript deshabilitado).

Una ventaja real es que no necesitaba cambiar una sola línea de código en la clase `MoviesController` o en la vista *Create. vbhtml* para habilitar esta interfaz de usuario de validación. El controlador y las vistas que creó anteriormente en este tutorial seleccionaron automáticamente las reglas de validación que especificó mediante atributos en la clase de modelo `Movie`.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Cómo se produce la validación en el método Create View y Create Action

Tal vez se pregunte cómo se generó la validación de la interfaz de usuario sin actualizar el código en el controlador o las vistas. La siguiente lista muestra el aspecto de los métodos de `Create` en la clase `MovieController`. No se modifican de la forma en que se crearon anteriormente en este tutorial.

[!code-vb[Main](adding-validation-to-the-model/samples/sample5.vb)]

El primer método de acción muestra el formulario de creación inicial. El segundo controla el envío del formulario. El segundo método `Create` llama a `ModelState.IsValid` para comprobar si la película tiene errores de validación. Al llamar a este método se evalúan todos los atributos de validación que se hayan aplicado al objeto. Si el objeto tiene errores de validación, el método `Create` vuelve a mostrar el formulario. Si no hay ningún error, el método guarda la nueva película en la base de datos.

A continuación se muestra la plantilla de vista *Create. vbhtml* con scaffolding anterior en el tutorial. Los métodos de acción que se muestran arriba la usan para mostrar el formulario inicial y para volver a mostrarlo en caso de error.

[!code-vbhtml[Main](adding-validation-to-the-model/samples/sample6.vbhtml)]

Observe cómo el código utiliza una aplicación auxiliar de `Html.EditorFor` para generar el elemento `<input>` para cada propiedad `Movie`. Junto a esta aplicación auxiliar es una llamada al método auxiliar `Html.ValidationMessageFor`. Estos dos métodos auxiliares funcionan con el objeto de modelo que pasa el controlador a la vista (en este caso, un objeto `Movie`). Buscan automáticamente los atributos de validación especificados en el modelo y muestran los mensajes de error según corresponda.

Lo que es muy útil para este enfoque es que ni el controlador ni la plantilla de creación de vistas saben nada sobre las reglas de validación reales que se aplican o sobre los mensajes de error específicos que se muestran. Las reglas de validación y las cadenas de error solo se especifican en la clase `Movie`.

Si desea cambiar la lógica de validación más adelante, puede hacerlo en exactamente un lugar. No tendrá que preocuparse de que diferentes partes de la aplicación sean incoherentes con el modo en que se aplican las reglas: toda la lógica de validación se definirá en un solo lugar y se usará en todas partes. Esto mantiene el código muy limpio y hace que sea fácil de mantener y evolucionar. También significa que respeta totalmente el principio DRY.

## <a name="adding-formatting-to-the-movie-model"></a>Agregar formato al modelo de película

Abra el archivo *Movie. VB* . El espacio de nombres [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) proporciona atributos de formato además del conjunto integrado de atributos de validación. Aplicará el [`DisplayFormat`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atributo y un valor de enumeración de [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) a la fecha de lanzamiento y a los campos de precio. En el código siguiente se muestran las propiedades `ReleaseDate` y `Price` con el atributo [`DisplayFormat`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) adecuado.

[!code-vb[Main](adding-validation-to-the-model/samples/sample7.vb)]

Como alternativa, puede establecer explícitamente un valor de [`DataFormatString`](https://msdn.microsoft.com/library/system.string.format.aspx) . En el código siguiente se muestra la propiedad Date Release con una cadena de formato de fecha (es decir, "d"). Se usaría para especificar que no desea tiempo como parte de la fecha de lanzamiento.

[!code-vb[Main](adding-validation-to-the-model/samples/sample8.vb)]

En el código siguiente se da formato a la propiedad `Price` como Currency.

[!code-vb[Main](adding-validation-to-the-model/samples/sample9.vb)]

A continuación se muestra la clase `Movie` completa.

[!code-vb[Main](adding-validation-to-the-model/samples/sample10.vb)]

Ejecute la aplicación y vaya al controlador de `Movies`.

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

En la siguiente parte de la serie, revisaremos la aplicación y realizamos algunas mejoras en los métodos `Details` y `Delete` generados automáticamente.

> [!div class="step-by-step"]
> [Anterior](adding-a-new-field.md)
> [Siguiente](improving-the-details-and-delete-methods.md)
