---
title: Agregar una vista a una aplicación MVC
author: Rick-Anderson
description: Agregar una vista a una aplicación MVC
ms.author: riande
ms.date: 01/23/2019
uid: mvc/overview/getting-started/introduction/adding-a-view
ms.openlocfilehash: 4b369028aca1e8a6cace60466b8049ccc02a2ec2
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519068"
---
# <a name="adding-a-view"></a>Agregar una vista

por [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](index.md)]

En esta sección, va a modificar la clase `HelloWorldController` para usar los archivos de plantilla de vista con el fin de encapsular correctamente el proceso de generación de respuestas HTML a un cliente. 

Creará un archivo de plantilla de vista mediante el [motor de vistas de Razor](../../../../web-pages/overview/getting-started/introducing-razor-syntax-c.md). Las plantillas de vista basadas en Razor tienen una extensión de archivo *. cshtml* y proporcionan una forma elegante de crear la C#salida HTML mediante. Razor minimiza el número de caracteres y pulsaciones de teclas necesarios al escribir una plantilla de vista y habilita un flujo de trabajo de codificación de fluido rápido.

Actualmente, el método `Index` devuelve una cadena con un mensaje que está codificado de forma rígida en la clase de controlador. Cambie el método `Index` para llamar al método de [vista](/dotnet/api/microsoft.aspnetcore.mvc.controller.view#Microsoft_AspNetCore_Mvc_Controller_View) de controladores, como se muestra en el código siguiente:

[!code-csharp[Main](adding-a-view/samples/sample1.cs?highlight=1,3)]

El método `Index` anterior utiliza una plantilla de vista para generar una respuesta HTML al explorador. Los métodos de controlador (también conocidos como [métodos de acción](http://rachelappel.com/asp.net-mvc-actionresults-explained)), como el método `Index` anterior, normalmente devuelven un [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (o una clase derivada de [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), no tipos primitivos como cadena.

Haga clic con el botón secundario en la carpeta *Views\HelloWorld* y haga clic en **Agregar**; a continuación, haga clic en **Página de vista de MVC 5 con diseño (Razor)** .
  
![](adding-a-view/_static/image1.png)   
  
En el cuadro de diálogo **especificar nombre para el elemento** , escriba *Índice*y, a continuación, haga clic en **Aceptar**.  
  
![](adding-a-view/_static/image2.png)  
  
En el cuadro de diálogo **seleccionar una página de diseño** , acepte el valor predeterminado **\_layout. cshtml** y haga clic en **Aceptar**.  
  
![](adding-a-view/_static/image3.png)  
  
En el cuadro de diálogo anterior, la carpeta *Views\Shared* está seleccionada en el panel izquierdo. Si tiene un archivo de diseño personalizado en otra carpeta, puede seleccionarlo. Hablaremos sobre el archivo de diseño más adelante en el tutorial

Se creará el archivo *MvcMovie\Views\HelloWorld\Index.cshtml* .

![](adding-a-view/_static/image4.png)

Agregue el siguiente marcado resaltado.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml?highlight=4-11)]

Haga clic con el botón derecho en el archivo *index. cshtml* y seleccione **ver en el explorador**.

![PI](adding-a-view/_static/image5.png)

También puede hacer clic con el botón derecho en el archivo *index. cshtml* y seleccionar **ver en inspector de página.** Consulte el [tutorial de inspector de página](../../views/using-page-inspector-in-aspnet-mvc.md) para obtener más información.

Como alternativa, ejecute la aplicación y vaya al controlador de `HelloWorld` (`http://localhost:xxxx/HelloWorld`). El método `Index` del controlador no hace mucho trabajo; simplemente ejecutó la instrucción `return View()`, que especificaba que el método debe utilizar un archivo de plantilla de vista para representar una respuesta al explorador. Dado que no especificó explícitamente el nombre del archivo de plantilla de vista que se va a usar, ASP.NET MVC tenía como valor predeterminado el archivo de vista *index. cshtml* en la carpeta *\Views\HelloWorld* . En la imagen siguiente se muestra la cadena &quot;Hello de nuestra plantilla de vista.&quot; codificado de forma rígida en la vista.

![](adding-a-view/_static/image6.png)

Parece bastante bueno. Sin embargo, tenga en cuenta que la barra de título del explorador muestra "index-My ASP.NET Application" y el vínculo Big situado en la parte superior de la página indica "nombre de la aplicación". En función del tamaño de la ventana del explorador, es posible que tenga que hacer clic en las tres barras de la esquina superior derecha para ver los vínculos a **Inicio**, **acerca**de, **contacto**, **registro** e **Inicio de sesión** .

## <a name="changing-views-and-layout-pages"></a>Cambiar vistas y páginas de diseño

En primer lugar, desea cambiar el nombre de la aplicación &quot;&quot; vínculo en la parte superior de la página. Ese texto es común a todas las páginas. Realmente se implementa en un solo lugar del proyecto, aunque aparezca en todas las páginas de la aplicación. Vaya a la carpeta */views/Shared* en **Explorador de soluciones** y abra el archivo *\_layout. cshtml* . Este archivo se denomina *Página de diseño* y se encuentra en la carpeta compartida que usan todas las demás páginas.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Las plantillas de diseño permiten especificar el diseño del contenedor HTML del sitio en un solo lugar y, después, aplicarlo en varias páginas del sitio. Busque la línea `@RenderBody()`. `RenderBody` es un marcador de posición donde se mostrarán todas las páginas específicas de vista que cree, &quot;encapsuladas&quot; en la página de diseño. Por ejemplo, si selecciona el vínculo **About** , la vista *Views\Home\About.cshtml* se representa dentro del método `RenderBody`.

Cambie el contenido del elemento de título. Cambie el [ActionLink](https://msdn.microsoft.com/library/dd504972(v=vs.108).aspx) en la plantilla de diseño de &quot;nombre de aplicación&quot; a &quot;&quot; de películas MVC y el controlador de `Home` a `Movies`. A continuación se muestra el archivo de diseño completo:

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=6,20)]

Ejecute la aplicación y observe que ahora dice &quot;&quot;de película de MVC. Haga clic en el vínculo **About (acerca de** ) y verá cómo la página muestra &quot;&quot;de películas MVC también. Pudimos realizar el cambio una vez en la plantilla de diseño y hacer que todas las páginas del sitio reflejen el nuevo título.

![](adding-a-view/_static/image8.png)

La primera vez que creamos el archivo *Views\HelloWorld\Index.cshtml* , contiene el código siguiente:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

El código Razor anterior establece explícitamente la página de diseño. Examine las *vistas\\archivo _ViewStart. cshtml* que contiene exactamente el mismo marcado de Razor. Las *[vistas\\archivo _ViewStart. cshtml](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)* definen el diseño común que usarán todas las vistas, por lo que puede comentar o quitar ese código del archivo *Views\HelloWorld\Index.cshtml* .

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml?highlight=1-3)]

Puede usar la propiedad `Layout` para establecer una vista de diseño diferente o establecerla en `null` para que no se use ningún archivo de diseño.

Ahora, vamos a cambiar el título de la vista de índice.

Abra *MvcMovie\Views\HelloWorld\Index.cshtml*. Hay dos lugares para hacer un cambio: en primer lugar, el texto que aparece en el título del explorador y, a continuación, en el encabezado secundario (el elemento `<h2>`). Haremos que sean algo diferentes para que pueda ver qué parte del código cambia cada área de la aplicación.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml?highlight=2,5)]

Para indicar el título HTML que se va a mostrar, el código anterior establece una propiedad `Title` del objeto `ViewBag` (que se encuentra en la plantilla de vista *index. cshtml* ). Tenga en cuenta que la plantilla de diseño ( *Views\Shared\\_Layout. cshtml* ) usa este valor en el elemento `<title>` como parte de la sección `<head>` del código HTML que se modificó anteriormente.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml?highlight=6)]

Con este enfoque de `ViewBag`, puede pasar fácilmente otros parámetros entre la plantilla de vista y el archivo de diseño.

Ejecute la aplicación. Tenga en cuenta que el título del explorador, el encabezado principal y los encabezados secundarios han cambiado. (Si no ve los cambios en el explorador, es posible que esté viendo contenido almacenado en caché. Presione Ctrl + F5 en el explorador para forzar que se cargue la respuesta del servidor). El título del explorador se crea con el `ViewBag.Title` que se establece en la plantilla de vista *index. cshtml* y la aplicación &quot;-Movie adicional&quot; agrega en el archivo de diseño.

Observe también cómo el contenido de la plantilla de vista *index. cshtml* se combinó con la plantilla de vista *\_layout. cshtml* y se envió una única respuesta HTML al explorador. Con las plantillas de diseño es realmente fácil hacer cambios para que se apliquen en todas las páginas de la aplicación.

![](adding-a-view/_static/image9.png)

Nuestro poco de &quot;de datos&quot; (en este caso, el &quot;Hola de nuestra plantilla de vista!&quot; mensaje) está codificado de forma rígida. La aplicación MVC tiene un&quot; de &quot;V (vista) y tiene un&quot; de &quot;C (controlador), pero todavía no &quot;&quot; M (modelo). En breve, veremos cómo crear una base de datos y recuperar los datos del modelo.

## <a name="passing-data-from-the-controller-to-the-view"></a>Pasar datos del controlador a la vista

Sin embargo, antes de ir a una base de datos y hablar acerca de los modelos, vamos a hablar primero sobre cómo pasar información desde el controlador a una vista. Las clases de controlador se invocan en respuesta a una solicitud de dirección URL entrante. Una clase de controlador es donde se escribe el código que controla las solicitudes entrantes del explorador, recupera los datos de una base de datos y, en última instancia, decide qué tipo de respuesta devolver al explorador. Las plantillas de vista se pueden usar desde un controlador para generar y dar formato a una respuesta HTML en el explorador.

Los controladores son responsables de proporcionar los datos u objetos necesarios para que una plantilla de vista represente una respuesta al explorador. Procedimiento recomendado: **una plantilla de vista nunca debe realizar la lógica de negocios ni interactuar directamente con una base de datos**. En su lugar, una plantilla de vista debería funcionar solo con los datos que le proporciona el controlador. Mantener este &quot;la separación de preocupaciones&quot; ayuda a mantener el código limpio, comprobable y más fácil de mantener.

Actualmente, el método de acción `Welcome` de la clase `HelloWorldController` toma un `name` y un parámetro `numTimes` y, a continuación, genera los valores directamente en el explorador. En lugar de hacer que el controlador represente esta respuesta como una cadena, vamos a cambiar el controlador para usar una plantilla de vista en su lugar. La plantilla de vista generará una respuesta dinámica, lo que significa que debe pasar las partes de datos adecuadas desde el controlador a la vista para que se genere la respuesta. Para ello, haga que el controlador Coloque los datos dinámicos (parámetros) que necesita la plantilla de vista en un objeto `ViewBag` al que pueda tener acceso la plantilla de vista.

Vuelva al archivo *HelloWorldController.CS* y cambie el método `Welcome` para agregar un `Message` y `NumTimes` valor al objeto `ViewBag`. `ViewBag` es un objeto dinámico, lo que significa que puede colocar todo lo que desee en él. el objeto `ViewBag` no tiene ninguna propiedad definida hasta que coloque algo dentro de ella. El [sistema de enlace de modelos de ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) asigna automáticamente los parámetros con nombre (`name` y `numTimes`) de la cadena de consulta de la barra de direcciones a los parámetros del método. El archivo *HelloWorldController.cs* completo tiene este aspecto:

[!code-csharp[Main](adding-a-view/samples/sample8.cs)]

Ahora, el objeto de `ViewBag` contiene datos que se pasarán a la vista automáticamente. A continuación, necesitará una plantilla de vista de bienvenida. En el menú **compilar** , seleccione **compilar solución** (o Ctrl + Mayús + B) para asegurarse de que se compila el proyecto. Haga clic con el botón secundario en la carpeta *Views\HelloWorld* y haga clic en **Agregar**; a continuación, haga clic en **Página de vista de MVC 5 con diseño (Razor)** .
  
![](adding-a-view/_static/image10.png)   
  
En el cuadro de diálogo **especificar nombre para el elemento** , escriba *bienvenida*y haga clic en **Aceptar**.   
  
En el cuadro de diálogo **seleccionar una página de diseño** , acepte el valor predeterminado **\_layout. cshtml** y haga clic en **Aceptar**.  
  
![](adding-a-view/_static/image11.png)   

Se creará el archivo *MvcMovie\Views\HelloWorld\Welcome.cshtml* .

Reemplace el marcado en el archivo *welcome. cshtml* . Creará un bucle que indica &quot;Hello&quot; tantas veces como indique el usuario. A continuación se muestra el archivo *welcome. cshtml* completo.

[!code-cshtml[Main](adding-a-view/samples/sample9.cshtml)]

Ejecute la aplicación y vaya a la siguiente dirección URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Ahora se toman los datos de la dirección URL y se pasan al controlador mediante el [enlazador de modelos](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). El controlador empaqueta los datos en un objeto `ViewBag` y pasa ese objeto a la vista. A continuación, la vista muestra los datos como HTML al usuario.

![](adding-a-view/_static/image12.png)

En el ejemplo anterior, usamos un objeto `ViewBag` para pasar datos del controlador a una vista. Más adelante en el tutorial usaremos un modelo de vista para pasar datos de un controlador a una vista. El enfoque del modelo de vista para pasar datos generalmente es mucho más recomendable que el enfoque de vista de bolsa. Para obtener más información, vea la entrada de blog [vistas fuertemente tipadas de V](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) . 

Bueno, esto era un tipo de &quot;M&quot; para el modelo, pero no el tipo de base de datos. Vamos a aprovechar lo que hemos aprendido para crear una base de datos de películas.

> [!div class="step-by-step"]
> [Anterior](adding-a-controller.md)
> [Siguiente](adding-a-model.md)
