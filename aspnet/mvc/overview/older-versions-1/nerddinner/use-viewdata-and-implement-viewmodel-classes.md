---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: Usar ViewData e implementar clases ViewModel | Microsoft Docs
author: microsoft
description: En el paso 6 se muestra cómo habilitar la compatibilidad con escenarios de edición de formularios más completos y también se describen dos enfoques que se pueden usar para pasar datos de controladores a vistas:...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: ca9775417c2e25952511a73096fb76d5d4edaea2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78435535"
---
# <a name="use-viewdata-and-implement-viewmodel-classes"></a>Usar ViewData e implementar clases ViewModel

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 6 de un [tutorial de aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito en el que se explica cómo crear una aplicación web pequeña, pero completa, con ASP.NET MVC 1.
> 
> En el paso 6 se muestra cómo habilitar la compatibilidad con escenarios de edición de formularios más completos y también se describen dos enfoques que se pueden usar para pasar datos de controladores a vistas: ViewData y ViewModel.
> 
> Si usa ASP.NET MVC 3, se recomienda seguir los tutoriales [de la introducción con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o MVC [Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>NerdDinner paso 6: ViewData y ViewModel

Hemos explicado una serie de escenarios de envío de formulario y hemos explicado cómo implementar la compatibilidad con la creación, actualización y eliminación (CRUD). Ahora seguiremos nuestra implementación de DinnersController y habilitaremos la compatibilidad con escenarios de edición de formularios más completos. Al hacerlo, trataremos dos enfoques que se pueden usar para pasar datos de controladores a vistas: ViewData y ViewModel.

### <a name="passing-data-from-controllers-to-view-templates"></a>Pasar datos de los controladores a las plantillas de vista

Una de las características que definen el modelo de MVC es la estricta "separación de intereses" que ayuda a aplicar entre los distintos componentes de una aplicación. Los modelos, los controladores y las vistas tienen roles y responsabilidades bien definidos y se comunican entre sí de maneras bien definidas. Esto ayuda a promover la reutilización de código y la capacidad de prueba.

Cuando una clase de controlador decide volver a presentar una respuesta HTML a un cliente, es responsable de pasar explícitamente a la plantilla de vista todos los datos necesarios para representar la respuesta. Las plantillas de vista nunca deben realizar la recuperación de datos o la lógica de la aplicación, y en su lugar deben limitarse únicamente a tener el código de representación que se basa en el modelo o los datos que el controlador pasa.

En este momento, los datos del modelo que pasa nuestra clase DinnersController a nuestras plantillas de vista son sencillos y directos: una lista de objetos cenas en el caso de index () y un único objeto cena en el caso de Details (), Edit (), Create () y Delete (). A medida que se agregan más capacidades de la interfaz de usuario a nuestra aplicación, a menudo es necesario pasar más que solo estos datos para representar las respuestas HTML dentro de las plantillas de vista. Por ejemplo, es posible que desee cambiar el campo "Country" dentro de las vistas Editar y crear de un cuadro de texto HTML a un DropDownList. En lugar de codificar de forma rígida la lista desplegable de nombres de país en la plantilla de vista, es posible que desee generarlo a partir de una lista de países admitidos que se rellenan dinámicamente. Necesitaremos una forma de pasar el objeto cena *y* la lista de los países admitidos de nuestro controlador a nuestras plantillas de vista.

Veamos dos maneras de lograrlo.

### <a name="using-the-viewdata-dictionary"></a>Usar el Diccionario ViewData

La clase base del controlador expone una propiedad de diccionario "ViewData" que se puede usar para pasar elementos de datos adicionales de los controladores a las vistas.

Por ejemplo, para admitir el escenario en el que queremos cambiar el cuadro de texto "Country" dentro de nuestra vista de edición para que sea un cuadro de texto HTML para DropDownList, podemos actualizar el método de acción Edit () para que pase (además de un objeto cena) un objeto SelectList que se pueda usar como m odelo de un DropDownList de países.

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

El constructor de SelectList anterior acepta una lista de condados para rellenar el Drop-downlist con, así como el valor seleccionado actualmente.

A continuación, podemos actualizar nuestra plantilla de vista Edit. aspx para usar el método auxiliar HTML. DropDownList () en lugar del método auxiliar HTML. TextBox () que usamos previamente:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

El método auxiliar HTML. DropDownList () anterior toma dos parámetros. El primero es el nombre del elemento de formulario HTML que se va a mostrar. El segundo es el modelo "SelectList" que pasamos a través del diccionario ViewData. Usamos la C# palabra clave "as" para convertir el tipo dentro del diccionario como SelectList.

Y ahora, cuando ejecutemos la aplicación y accedamos a la dirección URL de */Dinners/Edit/1* en nuestro explorador, veremos que la interfaz de usuario de edición se ha actualizado para mostrar un DropDownList de países en lugar de un cuadro de texto:

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

Dado que también se presenta la plantilla de vista de edición desde el método de edición HTTP-POST (en escenarios en los que se producen errores), se recomienda asegurarse de que también se actualiza este método para agregar SelectList a ViewData cuando la plantilla de vista se representa en escenarios de error:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

Y ahora nuestro escenario de edición DinnersController es compatible con DropDownList.

### <a name="using-a-viewmodel-pattern"></a>Usar un patrón ViewModel

El enfoque del diccionario ViewData tiene la ventaja de ser bastante rápido y fácil de implementar. Sin embargo, algunos desarrolladores no desean usar diccionarios basados en cadenas, ya que los errores tipográficos pueden provocar errores que no se detectarán en tiempo de compilación. El Diccionario ViewData sin tipo también requiere el uso del operador "as" o la conversión de tipos cuando se usa un lenguaje fuertemente tipado como C# en una plantilla de vista.

Un enfoque alternativo que podríamos usar es a menudo conocido como el patrón "ViewModel". Al usar este patrón, creamos clases fuertemente tipadas que están optimizadas para nuestros escenarios de vista específicos y que exponen propiedades para los valores dinámicos y el contenido que necesitan nuestras plantillas de vista. A continuación, las clases de controlador pueden rellenar y pasar estas clases optimizadas para vistas a nuestra plantilla de vista que se va a usar. Esto permite la seguridad de tipos, la comprobación en tiempo de compilación y el IntelliSense del editor dentro de las plantillas de vista.

Por ejemplo, para habilitar escenarios de edición de formularios de cenas, podemos crear una clase "DinnerFormViewModel" como la siguiente que exponga dos propiedades fuertemente tipadas: un objeto cena y el modelo SelectList necesario para rellenar los países DropDownList:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

A continuación, podemos actualizar el método de acción Edit () para crear el DinnerFormViewModel con el objeto cena que recuperamos de nuestro repositorio y, a continuación, pasarlo a nuestra plantilla de vista:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

Después, actualizaremos la plantilla de vista para que espere un "DinnerFormViewModel" en lugar de un objeto "cena" cambiando el atributo "Inherits" en la parte superior de la página Edit. aspx de la siguiente manera:

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

Una vez hecho esto, el IntelliSense de la propiedad "Model" dentro de la plantilla de vista se actualizará para reflejar el modelo de objetos del tipo DinnerFormViewModel que se va a pasar:

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

A continuación, podemos actualizar el código de vista para que no funcione. Observe que, a continuación, se indica cómo no se cambian los nombres de los elementos de entrada que estamos creando (los elementos de formulario seguirán denominados "title", "Country"), pero estamos actualizando los métodos auxiliares HTML para recuperar los valores mediante la clase DinnerFormViewModel:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

También actualizaremos el método Edit post para usar la clase DinnerFormViewModel cuando se produzcan errores de representación:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

También podemos actualizar los métodos de acción Create () para volver a usar exactamente la misma clase *DinnerFormViewModel* para habilitar los países DropDownList en ellos. A continuación se muestra la implementación de HTTP-GET:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

A continuación se muestra la implementación del método de creación HTTP-POST:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

Y ahora las pantallas editar y crear admiten Drop-downlists para seleccionar el país.

### <a name="custom-shaped-viewmodel-classes"></a>Clases ViewModel de forma personalizada

En el escenario anterior, nuestra clase DinnerFormViewModel expone directamente el objeto de modelo cena como una propiedad, junto con una propiedad de modelo Supported SelectList. Este enfoque funciona bien en escenarios en los que la interfaz de usuario HTML que se desea crear dentro de nuestra plantilla de vista se corresponde relativamente estrechamente con los objetos del modelo de dominio.

En escenarios en los que este no es el caso, una opción que puede usar es crear una clase ViewModel de forma personalizada cuyo modelo de objetos esté más optimizado para el consumo por parte de la vista, y que pueda ser completamente diferente del objeto de modelo de dominio subyacente. Por ejemplo, podría exponer diferentes nombres de propiedad o propiedades de agregado recopiladas de varios objetos de modelo.

Las clases ViewModel de forma personalizada se pueden usar para pasar datos de los controladores a las vistas que se van a representar, así como para ayudar a controlar los datos de formulario devueltos al método de acción de un controlador. En este escenario posterior, es posible que tenga el método de acción actualizar un objeto ViewModel con los datos publicados en formularios y, a continuación, usar la instancia de ViewModel para asignar o recuperar un objeto de modelo de dominio real.

Las clases de modelo de vista personalizado pueden proporcionar una gran flexibilidad y son algo para investigar cualquier momento en el que encuentre el código de representación dentro de las plantillas de vista o el código de publicación de formularios dentro de los métodos de acción empezando a ser demasiado complicado. A menudo, esto es un signo de que los modelos de dominio no se corresponden correctamente con la interfaz de usuario que está generando y que una clase ViewModel intermedia de forma personalizada puede ser útil.

### <a name="next-step"></a>Paso siguiente

Veamos ahora cómo podemos usar las particiones y las páginas maestras para volver a usar y compartir la interfaz de usuario en nuestra aplicación.

> [!div class="step-by-step"]
> [Anterior](provide-crud-create-read-update-delete-data-form-entry-support.md)
> [Siguiente](re-use-ui-using-master-pages-and-partials.md)
