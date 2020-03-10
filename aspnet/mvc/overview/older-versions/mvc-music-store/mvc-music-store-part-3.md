---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Parte 3: vistas y ViewModels | Microsoft Docs'
author: jongalloway
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 3 cubre las vistas y ViewModels.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 3fcfc816cde22c697a78bab2c9ea7ace1bf68501
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78451135"
---
# <a name="part-3-views-and-viewmodels"></a>Parte 3: vistas y ViewModels

por [Jon Galloway](https://github.com/jongalloway)

> El almacén de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> MVC Music Store es una implementación ligera de almacén de ejemplo que vende álbumes musicales en línea e implementa la funcionalidad básica de administración de sitios, Inicio de sesión de usuario y carro de la compra.  
>   
> En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 3 cubre las vistas y ViewModels.

Hasta ahora hemos devuelto cadenas desde las acciones del controlador. Esta es una buena manera de hacerse una idea de cómo funcionan los controladores, pero no es cómo desea crear una aplicación web real. Vamos a querer una manera mejor de volver a generar HTML en los exploradores que visitan nuestro sitio: uno en el que podemos usar archivos de plantilla para personalizar más fácilmente el contenido HTML devuelto. Eso es exactamente lo que hacen las vistas.

## <a name="adding-a-view-template"></a>Agregar una plantilla de vista

Para usar una plantilla de vista, cambiaremos el método de índice de HomeController para devolver un ActionResult y hacer que devuelva la vista (), como se indica a continuación:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

El cambio anterior indica que en lugar de devolver una cadena, queremos usar una "vista" para volver a generar un resultado.

Ahora vamos a agregar una plantilla de vista adecuada a nuestro proyecto. Para ello, colocaremos el cursor de texto en el método de acción de índice, luego haga clic con el botón derecho y seleccione "agregar vista". Se abrirá el cuadro de diálogo Agregar vista:

![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)

El cuadro de diálogo "agregar vista" nos permite generar archivos de plantilla de vista de forma rápida y sencilla. De forma predeterminada, el cuadro de diálogo "agregar vista" rellena previamente el nombre de la plantilla de vista que se va a crear para que coincida con el método de acción que la usará. Dado que usamos el menú contextual "agregar vista" dentro del método de acción index () de nuestro HomeController, el cuadro de diálogo "agregar vista" anterior tiene "index" como el nombre de la vista que se ha rellenado previamente de forma predeterminada. No es necesario cambiar ninguna de las opciones de este cuadro de diálogo, así que haga clic en el botón Agregar.

Al hacer clic en el botón Agregar, Visual Web Developer creará una nueva plantilla de vista index. cshtml para nosotros en el directorio \Views\Home y creará la carpeta si aún no existe.

![](mvc-music-store-part-3/_static/image2.png)

El nombre y la ubicación de la carpeta del archivo "index. cshtml" es importante y sigue las convenciones de nomenclatura predeterminadas de ASP.NET MVC. El nombre del directorio, \Views\Home, coincide con el controlador, que se denomina HomeController. El nombre de la plantilla de vista, índice, coincide con el método de acción del controlador que mostrará la vista.

ASP.NET MVC nos permite evitar tener que especificar explícitamente el nombre o la ubicación de una plantilla de vista cuando usamos esta Convención de nomenclatura para devolver una vista. De forma predeterminada, la plantilla de vista \Views\Home\Index.cshtml se representa cuando se escribe código como el siguiente en el HomeController:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

Visual Web Developer creó y abrió la plantilla de vista "index. cshtml" después de hacer clic en el botón "agregar" del cuadro de diálogo "agregar vista". A continuación se muestra el contenido de index. cshtml.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

Esta vista usa el sintaxis Razor, que es más conciso que el motor de vista de formularios Web Forms usado en los formularios Web Forms de ASP.NET y versiones anteriores de ASP.NET MVC. El motor de vista de formularios Web Forms sigue estando disponible en ASP.NET MVC 3, pero muchos desarrolladores descubren que el motor de vistas de Razor se adapta al desarrollo de ASP.NET MVC realmente bien.

Las tres primeras líneas establecen el título de la página mediante ViewBag. title. Veremos cómo funciona con más detalle pronto, pero primero vamos a actualizar el texto del encabezado de texto y ver la página. Actualice la etiqueta &lt;H2&gt; para decir "esta es la Página principal", como se muestra a continuación.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

La ejecución de la aplicación muestra que el nuevo texto está visible en la Página principal.

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a>Usar un diseño para los elementos comunes del sitio

La mayoría de los sitios web tienen contenido que se comparte entre muchas páginas: navegación, pies de página, imágenes de logotipo, referencias de hoja de estilos, etc. El motor de vistas de Razor facilita la administración mediante el uso de una página denominada \_layout. cshtml que se ha creado automáticamente en la carpeta/Views/Shared.

![](mvc-music-store-part-3/_static/image4.png)

Haga doble clic en esta carpeta para ver el contenido, que se muestra a continuación.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

El contenido de nuestras vistas individuales se mostrará con el comando @RenderBody() y cualquier contenido común que desee que aparezca fuera de, se puede Agregar al marcado \_layout. cshtml. Queremos que nuestra tienda de música de MVC tenga un encabezado común con vínculos a nuestra página principal y área de almacenamiento en todas las páginas del sitio, por lo que agregaremos a la plantilla directamente encima de la instrucción @RenderBody().

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a>Actualizar la hoja de estilos

La plantilla de proyecto vacía incluye un archivo CSS muy simplificado que solo incluye estilos que se usan para mostrar los mensajes de validación. Nuestro diseñador ha proporcionado algunos CSS e imágenes adicionales para definir la apariencia y el funcionamiento de nuestro sitio, por lo que los agregaremos en este momento.

El archivo CSS actualizado y las imágenes se incluyen en el directorio de contenido de MvcMusicStore-Assets. zip, que está disponible en [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store). Seleccionaremos ambos en el explorador de Windows y los colocaremos en la carpeta de contenido de la solución en Visual Web Developer, como se muestra a continuación:

![](mvc-music-store-part-3/_static/image5.png)

Se le pedirá que confirme si desea sobrescribir el archivo site. CSS existente. Haga clic en Sí.

![](mvc-music-store-part-3/_static/image6.png)

La carpeta de contenido de la aplicación aparecerá ahora de la siguiente manera:

![](mvc-music-store-part-3/_static/image7.png)

Ahora vamos a ejecutar la aplicación y ver cómo se ven los cambios en la Página principal.

![](mvc-music-store-part-3/_static/image8.png)

- Vamos a revisar lo que ha cambiado: el método de acción de índice de HomeController encontró y mostraba la plantilla \Views\Home\Index.cshtmlView, aunque nuestro código llamara "Return View ()", ya que nuestra plantilla de vista siguió la Convención de nomenclatura estándar.
- La Página principal muestra un mensaje de bienvenida sencillo que se define dentro de la plantilla de vista \Views\Home\Index.cshtml.
- La Página principal usa nuestra plantilla \_layout. cshtml y, por tanto, el mensaje de bienvenida está incluido en el diseño HTML del sitio estándar.

## <a name="using-a-model-to-pass-information-to-our-view"></a>Usar un modelo para pasar información a nuestra vista

Una plantilla de vista que solo muestra HTML codificado no va a crear un sitio web muy interesante. Para crear un sitio web dinámico, en su lugar desearemos pasar información de nuestras acciones del controlador a nuestras plantillas de vista.

En el modelo modelo-vista-controlador, el término modelo hace referencia a los objetos que representan los datos de la aplicación. A menudo, los objetos de modelo se corresponden con las tablas de la base de datos, pero no tienen que hacerlo.

Los métodos de acción del controlador que devuelven un ActionResult pueden pasar un objeto de modelo a la vista. Esto permite a un controlador empaquetar correctamente toda la información necesaria para generar una respuesta y, a continuación, pasar esta información a una plantilla de vista para usar para generar la respuesta HTML adecuada. Esto es más fácil de entender si se ve en acción, así que vamos a empezar.

En primer lugar, crearemos algunas clases de modelo para representar los géneros y los álbumes de nuestra tienda. Comencemos por crear una clase Genre. Haga clic con el botón secundario en la carpeta "models" del proyecto, elija la opción "Agregar clase" y asigne al archivo el nombre "Genre.cs".

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

A continuación, agregue una propiedad de nombre de cadena pública a la clase que se creó:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

*Nota: en caso de que se pregunte, la notación {Get; Set;} está haciendo uso C#de la característica de propiedades implementadas automáticamente. Esto nos ofrece las ventajas de una propiedad sin necesidad de declarar un campo de respaldo.*

A continuación, siga los mismos pasos para crear una clase album (denominada Album.cs) que tenga un título y una propiedad Genre:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

Ahora podemos modificar StoreController para usar vistas que muestren información dinámica de nuestro modelo. Si, en este momento, se llamara a nuestros álbumes en función del identificador de solicitud, podríamos Mostrar esa información como en la siguiente vista.

![](mvc-music-store-part-3/_static/image10.png)

Comenzaremos cambiando la acción Store details para que muestre la información de un solo álbum. Agregue una instrucción "Using" a la parte superior de la clase **StoreControllers** para incluir el espacio de nombres MvcMusicStore. Models, por lo que no es necesario escribir MvcMusicStore. Models. album cada vez que queremos usar la clase Album. La sección "Using" de esa clase debe aparecer ahora como se indica a continuación.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

A continuación, actualizaremos la acción del controlador de detalles para que devuelva un ActionResult en lugar de una cadena, como hicimos con el método index de HomeController.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

Ahora podemos modificar la lógica para devolver un objeto album a la vista. Más adelante en este tutorial se van a recuperar los datos de una base de datos de, pero, de momento, vamos a usar "datos ficticios" para comenzar.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

*Nota: Si no está familiarizado con C#, puede suponer que el uso de var significa que la variable de álbum está enlazada en tiempo de ejecución. Esto no es correcto: el C# compilador usa la inferencia de tipos en función de lo que estamos asignando a la variable para determinar que el álbum es de tipo album y compilando la variable de álbum local como un tipo de álbum, por lo que obtenemos la comprobación en tiempo de compilación y la compatibilidad con el editor de código de Visual Studio.*

Ahora vamos a crear una plantilla de vista que use nuestro álbum para generar una respuesta HTML. Antes de hacerlo, debemos compilar el proyecto para que el cuadro de diálogo Agregar vista Conozca nuestra clase de álbum recién creada. Puede compilar el proyecto seleccionando el elemento de menú Depurar ⇨ compilación MvcMusicStore (para crédito adicional, puede usar el método abreviado CTRL-SHIFT-B para compilar el proyecto).

![](mvc-music-store-part-3/_static/image11.png)

Ahora que hemos configurado nuestras clases de soporte técnico, estamos preparados para crear nuestra plantilla de vista. Haga clic con el botón derecho en el método de detalles y seleccione "agregar vista..." en el menú contextual.

![](mvc-music-store-part-3/_static/image12.png)

Vamos a crear una nueva plantilla de vista como hicimos antes con el HomeController. Dado que se crea a partir de StoreController, se generará de forma predeterminada en un archivo \Views\Store\Index.cshtml.

A diferencia de antes, vamos a activar la casilla "crear una vista fuertemente tipada". A continuación, vamos a seleccionar la clase "album" en el Drop-downlist de "View Data-Class". Esto hará que el cuadro de diálogo "agregar vista" cree una plantilla de vista que espera que se le pase un objeto de álbum para usar.

![](mvc-music-store-part-3/_static/image13.png)

Cuando hacemos clic en el botón "agregar", se creará la plantilla de vista \Views\Store\Details.cshtml, que contiene el código siguiente.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

Observe la primera línea, que indica que esta vista está fuertemente tipada a nuestra clase Album. El motor de vistas de Razor entiende que se ha pasado un objeto de álbum, por lo que podemos acceder fácilmente a las propiedades del modelo e incluso tener la ventaja de IntelliSense en el editor de Visual Web Developer.

Actualice la etiqueta del&gt; de &lt;H2 para que muestre la propiedad title del álbum modificando esa línea para que aparezca de la manera siguiente.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

Observe que IntelliSense se desencadena cuando se escribe el punto después de la palabra clave @Model, que muestra las propiedades y los métodos que admite la clase Album.

Ahora vamos a ejecutar el proyecto y a visitar la dirección URL de/Store/Details/5. Veremos los detalles de un álbum como el que se muestra a continuación.

![](mvc-music-store-part-3/_static/image14.png)

Ahora crearemos una actualización similar para el método de acción de búsqueda en el almacén. Actualice el método para que devuelva un ActionResult y modifique la lógica del método para que cree un nuevo objeto Genre y lo devuelva a la vista.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

Haga clic con el botón derecho en el método de exploración y seleccione "agregar vista..." en el menú contextual, agregue una vista fuertemente tipada agregue una fuertemente tipada a la clase Genre.

![](mvc-music-store-part-3/_static/image15.png)

Actualice el elemento&gt; de &lt;H2 en el código de vista (en/Views/Store/Browse.cshtml) para mostrar la información del género.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

Ahora vamos a volver a ejecutar nuestro proyecto y a buscar el/Store/Browse? Genre = dirección URL de disco. Veremos la página examinar como se muestra a continuación.

![](mvc-music-store-part-3/_static/image16.png)

Por último, vamos a crear una actualización ligeramente más compleja en el método de acción de índice de la **tienda** y la vista para mostrar una lista de todos los géneros de nuestra tienda. Lo haremos mediante una lista de géneros como objeto del modelo, en lugar de un solo género.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

Haga clic con el botón derecho en el método de acción almacenar índice y seleccione Agregar vista como antes, seleccione género como clase de modelo y presione el botón Agregar.

![](mvc-music-store-part-3/_static/image17.png)

En primer lugar, cambiaremos la declaración de @model para indicar que la vista esperará varios objetos de género en lugar de uno solo. Cambie la primera línea de/Store/Index.cshtml para que quede como sigue:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

Esto indica al motor de vistas de Razor que va a trabajar con un objeto de modelo que puede contener varios objetos de género. Estamos usando un género IEnumerable&lt;&gt; en lugar de una lista&lt;género&gt; ya que es más genérico, lo que nos permite cambiar el tipo de modelo más adelante a cualquier tipo de objeto que admita la interfaz IEnumerable.

A continuación, vamos a recorrer los objetos de género en el modelo, tal como se muestra en el código de vista completado a continuación.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

Tenga en cuenta que tenemos compatibilidad completa con IntelliSense a medida que especificamos este código, de modo que cuando escribamos "@Model". vemos todos los métodos y las propiedades que admite un IEnumerable de tipo Genre.

![](mvc-music-store-part-3/_static/image18.png)

En nuestro bucle "foreach", Visual Web Developer sabe que cada elemento es de tipo Genre, por lo que vemos IntelliSense para cada tipo de género.

![](mvc-music-store-part-3/_static/image19.png)

A continuación, la característica de scaffolding examinó el objeto Genre y determinó que cada uno tendrá una propiedad Name, de modo que recorra y escriba en bucle. También genera vínculos de edición, detalles y eliminación a cada elemento individual. Nos beneficiaremos de esto más adelante en nuestro administrador de tienda, pero por ahora nos gustaría tener una lista simple en su lugar.

Cuando ejecutamos la aplicación y desplazamos hasta/Store, vemos que se muestra el recuento y la lista de géneros.

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a>Agregar vínculos entre páginas

En la dirección URL de/Store que muestra géneros se enumeran actualmente los nombres de género simplemente como texto sin formato. Vamos a cambiar esto para que, en lugar de texto sin formato, los nombres de los géneros se vinculen a la dirección URL de/Store/Browse adecuada, de modo que al hacer clic en un género musical como "disco" vaya a la dirección URL de/Store/Browse? Genre = disco. Podríamos actualizar nuestra plantilla de vista de \Views\Store\Index.cshtml para generar estos vínculos con código como **el siguiente (no lo escribas en)** :

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

Eso funciona, pero podría provocar problemas más adelante, ya que se basa en una cadena codificada. Por ejemplo, si deseáramos cambiar el nombre del controlador, es necesario buscar en nuestro código los vínculos que deben actualizarse.

Un enfoque alternativo que se puede utilizar es aprovechar un método auxiliar HTML. ASP.NET MVC incluye métodos auxiliares HTML que están disponibles en nuestro código de plantilla de vista para realizar diversas tareas comunes de la misma manera. El método auxiliar HTML. ActionLink () es un método especialmente útil y facilita la compilación de HTML &lt;un&gt; vínculos y se encarga de los detalles molestos, como asegurarse de que las rutas de acceso URL están codificadas correctamente como dirección URL.

HTML. ActionLink () tiene varias sobrecargas diferentes para permitir especificar toda la información que necesita para los vínculos. En el caso más simple, proporcionará solo el texto del vínculo y el método de acción que se va a usar cuando se haga clic en el hipervínculo en el cliente. Por ejemplo, podemos crear un vínculo al método index () de "/Store/" en la página de detalles de la tienda con el texto del vínculo "ir al índice de la tienda" mediante la siguiente llamada:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

*Nota: en este caso, no es necesario especificar el nombre del controlador porque simplemente estamos vinculando a otra acción dentro del mismo controlador que representa la vista actual.*

Sin embargo, los vínculos a la página examinar deberán pasar un parámetro, por lo que usaremos otra sobrecarga del método html. ActionLink que toma tres parámetros:

- 1. Texto del vínculo, que mostrará el nombre del género
- 2. Nombre de acción del controlador (examinar)
- 3. Valores de parámetro de la ruta, que especifican el nombre (género) y el valor (nombre de género)

En conjunto, aquí se muestra cómo escribiremos esos vínculos en la vista de índice de tienda:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

Ahora, al ejecutar el proyecto de nuevo y acceder a la dirección URL de/Store/, se verá una lista de géneros. Cada género es un hipervínculo, cuando se hace clic en él nos llevará a nuestra dirección URL de/Store/Browse? Genre = *[Genre]* .

![](mvc-music-store-part-3/_static/image3.jpg)

El código HTML de la lista género tiene el siguiente aspecto:

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]

> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-2.md)
> [Siguiente](mvc-music-store-part-4.md)
