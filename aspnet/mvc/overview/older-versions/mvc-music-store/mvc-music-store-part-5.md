---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: 'Parte 5: edición de formularios y plantillas | Microsoft Docs'
author: jongalloway
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 5 trata la edición de formularios y plantillas.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: 20b99cbe57b5dfa623205838a5929733a6c2d70d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78450913"
---
# <a name="part-5-edit-forms-and-templating"></a>Parte 5: edición de formularios y plantillas

por [Jon Galloway](https://github.com/jongalloway)

> El almacén de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> MVC Music Store es una implementación ligera de almacén de ejemplo que vende álbumes musicales en línea e implementa la funcionalidad básica de administración de sitios, Inicio de sesión de usuario y carro de la compra.
> 
> En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 5 trata la edición de formularios y plantillas.

En el capítulo anterior, estábamos cargando datos de nuestra base de datos y mostrándolos. En este capítulo, también se habilitará la edición de los datos.

## <a name="creating-the-storemanagercontroller"></a>Crear StoreManagerController

Comenzaremos por crear un nuevo controlador denominado **StoreManagerController**. Para este controlador, se aprovecharán las características de scaffolding disponibles en la actualización de ASP.NET MVC 3 Tools. Establezca las opciones para el cuadro de diálogo Agregar controlador, como se muestra a continuación.

![](mvc-music-store-part-5/_static/image1.png)

Al hacer clic en el botón Agregar, verá que el mecanismo de scaffolding ASP.NET MVC 3 realiza una buena cantidad de trabajo:

- Crea el nuevo StoreManagerController con una variable local de Entity Framework
- Agrega una carpeta StoreManager a la carpeta views del proyecto.
- Agrega la vista Create. cshtml, DELETE. cshtml, details. cshtml, Edit. cshtml e index. cshtml, fuertemente tipada a la clase Album.

![](mvc-music-store-part-5/_static/image2.png)

La nueva clase de controlador StoreManager incluye acciones CRUD (crear, leer, actualizar, eliminar) controlador que saben cómo trabajar con la clase de modelo de álbum y usar el contexto de Entity Framework para el acceso a la base de datos.

## <a name="modifying-a-scaffolded-view"></a>Modificar una vista con scaffolding

Es importante recordar que, mientras que este código se generó para nosotros, es el código ASP.NET MVC estándar, al igual que hemos escrito en este tutorial. Está pensada para ahorrarle el tiempo dedicado a escribir código de controlador reutilizable y a crear las vistas fuertemente tipadas manualmente, pero este no es el tipo de código generado que puede haber visto precedido por advertencias de graves en los comentarios sobre cómo no debe cambiar el codifica. Este es el código y se espera que lo cambie.

Por lo tanto, comencemos con una edición rápida de la vista de índice de StoreManager (/Views/StoreManager/Index.cshtml). Esta vista mostrará una tabla que muestra los álbumes de nuestra tienda con los vínculos editar/detalles/eliminar e incluye las propiedades públicas del álbum. Vamos a quitar el campo AlbumArtUrl, ya que no es muy útil en esta pantalla. En &lt;sección&gt; de la tabla del código de vista, quite los elementos &lt;TH&gt; y &lt;TD&gt; que rodean las referencias de AlbumArtUrl, como se indica en las líneas resaltadas a continuación:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

El código de vista modificado aparecerá de la siguiente manera:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a>Un primer vistazo al administrador de la tienda

Ahora ejecute la aplicación y vaya a/StoreManager/. Esto muestra el índice del administrador de la tienda que acaba de modificar y muestra una lista de los álbumes en el almacén con vínculos para editar, detalles y eliminar.

![](mvc-music-store-part-5/_static/image3.png)

Al hacer clic en el vínculo editar, se muestra un formulario de edición con campos para el álbum, incluidos los menús desplegables de género y artista.

![](mvc-music-store-part-5/_static/image4.png)

Haga clic en el vínculo "volver a la lista" de la parte inferior y, a continuación, haga clic en el vínculo detalles de un álbum. Esto muestra la información detallada de un álbum individual.

![](mvc-music-store-part-5/_static/image5.png)

De nuevo, haga clic en el vínculo volver a la lista y, a continuación, haga clic en un vínculo eliminar. Esto muestra un cuadro de diálogo de confirmación, que muestra los detalles del álbum y pregunta si estamos seguros de que queremos eliminarlo.

![](mvc-music-store-part-5/_static/image6.png)

Al hacer clic en el botón eliminar en la parte inferior, se eliminará el álbum y se le devolverá a la página de índice, que muestra el álbum eliminado.

No estamos listos con el administrador de la tienda, pero tenemos el controlador de trabajo y el código de vista para que las operaciones CRUD empiecen a partir de.

## <a name="looking-at-the-store-manager-controller-code"></a>Examinar el código del controlador del administrador de la tienda

El controlador del administrador de la tienda contiene una buena cantidad de código. Vamos a repasarlo de arriba abajo. El controlador incluye algunos espacios de nombres estándar para un controlador de MVC, así como una referencia al espacio de nombres de los modelos. El controlador tiene una instancia privada de MusicStoreEntities, usada por cada una de las acciones de controlador para el acceso a los datos.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a>Acciones de índice y detalles de Store Manager

En la vista de índice se recupera una lista de álbumes, incluida la información de intérprete y el género a la que se hace referencia en el álbum, como vimos anteriormente al trabajar en el método de exploración de la tienda. La vista de índice sigue las referencias a los objetos vinculados para que pueda mostrar el nombre de género y el nombre del artista de cada álbum, de modo que el controlador sea eficaz y consulte la información de la solicitud original.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

La acción del controlador de detalles del controlador de StoreManager funciona exactamente igual que la acción de detalles del controlador de almacenamiento que hemos escrito anteriormente: consulta el álbum por identificador mediante el método Find () y, a continuación, lo devuelve a la vista.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a>Los métodos de acción Create

Los métodos de acción Create son ligeramente diferentes de los que hemos visto hasta ahora, ya que administran la entrada de formulario. Cuando un usuario visita por primera vez/StoreManager/Create/, se le mostrará un formulario vacío. Esta página HTML contendrá un formulario &lt;&gt; elemento que contiene los elementos de entrada DropDown y TextBox, donde pueden escribir los detalles del álbum.

Una vez que el usuario rellena los valores del formulario de álbum, puede presionar el botón "guardar" para enviar estos cambios de vuelta a la aplicación para guardarlos en la base de datos. Cuando el usuario presiona el botón "guardar", el formulario &lt;&gt; realizará una devolución de datos HTTP en la dirección URL de/StoreManager/Create/y enviará el &lt;formulario&gt; valores como parte de HTTP-POST.

ASP.NET MVC nos permite dividir fácilmente la lógica de estos dos escenarios de invocación de direcciones URL al permitirnos implementar dos métodos de acción "Create" independientes dentro de nuestra clase StoreManagerController: uno para controlar la búsqueda HTTP-GET inicial en la dirección URL de/StoreManager/Create/y otro para controlar la publicación HTTP de los cambios enviados.

### <a name="passing-information-to-a-view-using-viewbag"></a>Pasar información a una vista mediante ViewBag

Hemos usado ViewBag anteriormente en este tutorial, pero no ha hablado mucho de ello. ViewBag nos permite pasar información a la vista sin usar un objeto de modelo fuertemente tipado. En este caso, la acción editar controlador HTTP-GET debe pasar una lista de géneros y artistas al formulario para rellenar las listas desplegables, y la manera más sencilla de hacerlo es devolverlas como elementos de ViewBag.

ViewBag es un objeto dinámico, lo que significa que puede escribir ViewBag. foo o ViewBag. YourNameHere sin escribir código para definir esas propiedades. En este caso, el código del controlador utiliza ViewBag. GenreId y ViewBag. ArtistId para que los valores desplegables que se envían con el formulario sean GenreId y ArtistId, que son las propiedades del álbum que van a establecer.

Estos valores desplegables se devuelven al formulario mediante el objeto SelectList, que se crea solo para ese propósito. Esto se hace mediante código como el siguiente:

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

Como puede ver en el código del método de acción, se usan tres parámetros para crear este objeto:

- Lista de elementos que se mostrarán en la lista desplegable. Tenga en cuenta que esto no es solo una cadena: vamos a pasar una lista de géneros.
- El siguiente parámetro que se pasa a SelectList es el valor seleccionado. Esta es la forma en que el SelectList sabe cómo seleccionar previamente un elemento de la lista. Esto resultará más fácil de entender cuando examinemos el formulario de edición, que es bastante similar.
- El último parámetro es la propiedad que se va a mostrar. En este caso, esto indica que la propiedad Genre.Name es lo que se mostrará al usuario.

Con eso en mente, la acción de creación de HTTP-GET es bastante simple: se agregan dos SelectLists a ViewBag y no se pasa ningún objeto de modelo al formulario (ya que aún no se ha creado).

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a>Aplicaciones auxiliares de HTML para mostrar las listas desplegables en la vista de creación

Como hemos hablado de cómo se pasan los valores de la lista desplegable a la vista, echemos un vistazo a la vista para ver cómo se muestran esos valores. En el código de vista (/Views/StoreManager/Create.cshtml), verá que se realiza la siguiente llamada para mostrar la lista desplegable género.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

Esto se conoce como aplicación auxiliar HTML: un método de utilidad que realiza una tarea de vista común. Las aplicaciones auxiliares HTML son muy útiles para mantener el código de vista conciso y legible. ASP.NET MVC proporciona la aplicación auxiliar HTML. DropDownList pero, como veremos más adelante, es posible crear nuestras propias aplicaciones auxiliares para ver el código que vamos a usar en nuestra aplicación.

La llamada a HTML. DropDownList solo tiene que indicar dos cosas: Dónde obtener la lista para mostrar y qué valor (si existe) se debe preseleccionar. El primer parámetro, GenreId, indica a DropDownList que busque un valor denominado GenreId en el modelo o en ViewBag. El segundo parámetro se usa para indicar el valor que se va a mostrar como seleccionado inicialmente en la lista desplegable. Dado que este formulario es un formulario de creación, no hay ningún valor que se haya preseleccionado y se pasa String. Empty.

### <a name="handling-the-posted-form-values"></a>Controlar los valores de formulario publicados

Como hemos comentado antes, hay dos métodos de acción asociados a cada formulario. La primera controla la solicitud HTTP-GET y muestra el formulario. El segundo controla la solicitud HTTP-POST, que contiene los valores de formulario enviados. Observe que la acción del controlador tiene un atributo [HttpPost], que indica a ASP.NET MVC que solo debe responder a solicitudes HTTP POST.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

Esta acción tiene cuatro responsabilidades:

- 1. Leer los valores del formulario
- 2. Comprobar si los valores del formulario pasan las reglas de validación
- 3. Si el envío del formulario es válido, guarde los datos y muestre la lista actualizada.
- 4. Si el envío del formulario no es válido, vuelva a mostrar el formulario con errores de validación.

#### <a name="reading-form-values-with-model-binding"></a>Leer valores de formulario con el enlace de modelos

La acción de controlador está procesando un envío de formulario que incluye valores para GenreId y ArtistId (de la lista desplegable) y valores de cuadro de texto para título, precio y AlbumArtUrl. Aunque es posible acceder directamente a los valores de formulario, un enfoque mejor es usar las capacidades de enlace de modelos integradas en ASP.NET MVC. Cuando una acción de controlador toma un tipo de modelo como un parámetro, ASP.NET MVC intentará rellenar un objeto de ese tipo mediante entradas de formulario (así como valores de ruta y QueryString). Para ello, busca valores cuyos nombres coinciden con las propiedades del objeto de modelo; por ejemplo, al establecer el valor de GenreId del nuevo objeto album, busca una entrada con el nombre GenreId. Al crear vistas con los métodos estándar en ASP.NET MVC, los formularios siempre se representarán con los nombres de propiedad como nombres de campo de entrada, por lo que los nombres de campo solo coincidirán.

#### <a name="validating-the-model"></a>Validar el modelo

El modelo se valida con una llamada simple a ModelState. IsValid. Todavía no hemos agregado ninguna regla de validación a nuestra clase de álbum; lo haremos en un poco, de modo que ahora esta comprobación no tiene mucho que hacer. Lo importante es que esta comprobación de ModelStat. IsValid se adaptará a las reglas de validación que se colocan en nuestro modelo, por lo que los cambios futuros en las reglas de validación no requerirán ninguna actualización del código de acción del controlador.

#### <a name="saving-the-submitted-values"></a>Guardar los valores enviados

Si el envío del formulario pasa la validación, es el momento de guardar los valores en la base de datos. Con Entity Framework, eso solo requiere agregar el modelo a la colección de álbumes y llamar a SaveChanges.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

Entity Framework genera los comandos SQL adecuados para conservar el valor. Después de guardar los datos, redirigimos de nuevo a la lista de álbumes para que podamos ver la actualización. Para ello, se devuelve RedirectToAction con el nombre de la acción de controlador que se desea mostrar. En este caso, este es el método de índice.

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a>Mostrar envíos de formulario no válidos con errores de validación

En el caso de una entrada de formulario no válida, los valores de la lista desplegable se agregan a ViewBag (como en el caso HTTP-GET) y los valores del modelo enlazado se devuelven a la vista para su presentación. Los errores de validación se muestran automáticamente mediante la aplicación auxiliar HTML @Html.ValidationMessageFor.

#### <a name="testing-the-create-form"></a>Probar el formulario de creación

Para probar esto, ejecute la aplicación y vaya a/StoreManager/Create/, que le mostrará el formulario en blanco que ha devuelto el método StoreController Create HTTP-GET.

Rellene algunos valores y haga clic en el botón crear para enviar el formulario.

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a>Control de ediciones

El par de acciones de edición (HTTP-GET y HTTP-POST) son muy similares a los métodos de acción de creación que se acaban de examinar. Dado que el escenario de edición implica trabajar con un álbum existente, el método Edit HTTP-GET carga el álbum basándose en el parámetro "ID", que se pasa a través de la ruta. Este código para recuperar un álbum por AlbumId es el mismo que hemos examinado anteriormente en la acción del controlador de detalles. Al igual que con el método Create/HTTP-GET, los valores de la lista desplegable se devuelven a través de ViewBag. Esto nos permite devolver un álbum como nuestro objeto de modelo a la vista (que está fuertemente tipada a la clase album) mientras se pasan datos adicionales (por ejemplo, una lista de géneros) a través de ViewBag.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

La acción editar HTTP-POST es muy similar a la acción crear HTTP-POST. La única diferencia es que, en lugar de agregar un nuevo álbum a la base de BD. Álbumes, estamos buscando la instancia actual del álbum con dB. Entrada (álbum) y establecer su estado en modificado. Esto indica Entity Framework que estamos modificando un álbum existente en lugar de crear uno nuevo.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

Para probar esto, ejecute la aplicación y vaya a/StoreManger/y, a continuación, haga clic en el vínculo editar de un álbum.

![](mvc-music-store-part-5/_static/image9.png)

Esto muestra el formulario de edición que se muestra en el método Edit HTTP-GET. Rellene algunos valores y haga clic en el botón Guardar.

![](mvc-music-store-part-5/_static/image10.png)

De este modo, se envía el formulario, se guardan los valores y se devuelven a la lista de álbumes, donde se muestran los valores que se actualizaron.

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a>Controlar la eliminación

La eliminación sigue el mismo patrón que editar y crear, mediante una acción de controlador para mostrar el formulario de confirmación y otra acción de controlador para controlar el envío del formulario.

La acción HTTP-GET Delete Controller es exactamente la misma que la acción anterior del controlador de detalles del administrador de la tienda.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

Se muestra un formulario fuertemente tipado a un tipo de álbum mediante la plantilla eliminar contenido de vista.

![](mvc-music-store-part-5/_static/image12.png)

La plantilla de eliminación muestra todos los campos del modelo, pero se puede simplificar bastante. Cambie el código de vista de/Views/StoreManager/Delete.cshtml a lo siguiente.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

Esto muestra una confirmación de eliminación simplificada.

![](mvc-music-store-part-5/_static/image13.png)

Al hacer clic en el botón eliminar, el formulario se devuelve al servidor, que ejecuta la acción DeleteConfirmed.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

Nuestra acción HTTP-POST Delete Controller realiza las siguientes acciones:

- 1. Carga el álbum por identificador
- 2. Elimina el álbum y guarda los cambios.
- 3. Redirige al índice, mostrando que el álbum se ha quitado de la lista.

Para probar esto, ejecute la aplicación y vaya a/StoreManager. Seleccione un álbum de la lista y haga clic en el vínculo eliminar.

![](mvc-music-store-part-5/_static/image14.png)

Se mostrará la pantalla de confirmación de eliminación.

![](mvc-music-store-part-5/_static/image15.png)

Al hacer clic en el botón eliminar, se quita el álbum y se devuelve a la página de índice del administrador de la tienda, que muestra que el álbum se ha eliminado.

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a>Usar una aplicación auxiliar HTML personalizada para truncar el texto

Tenemos un posible problema con nuestra página de índice del administrador de la tienda. Las propiedades del título del álbum y el nombre del intérprete pueden ser lo suficientemente largas como para que se produzca el formato de la tabla. Crearemos una aplicación auxiliar HTML personalizada para que podamos truncar fácilmente estas y otras propiedades en nuestras vistas.

![](mvc-music-store-part-5/_static/image17.png)

La sintaxis de @helper de Razor ha facilitado la creación de sus propias funciones auxiliares para su uso en las vistas. Abra la vista/Views/StoreManager/Index.cshtml y agregue el código siguiente directamente después de la línea de @model.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

Este método auxiliar toma una cadena y la longitud máxima permitida. Si el texto proporcionado es más corto que la longitud especificada, el ayudante lo envía tal cual. Si es mayor, trunca el texto y representa "..." para el resto.

Ahora podemos usar nuestra aplicación auxiliar de truncamiento para asegurarse de que las propiedades del título del álbum y del nombre del intérprete tengan menos de 25 caracteres. A continuación se muestra el código de vista completo que usa nuestra nueva aplicación auxiliar de truncamiento.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

Ahora, cuando examinemos la dirección URL de/StoreManager/, los álbumes y los títulos se mantienen por debajo de las longitudes máximas.

![](mvc-music-store-part-5/_static/image18.png)

Nota: muestra el caso simple de crear y usar una aplicación auxiliar en una vista. Para obtener más información sobre la creación de aplicaciones auxiliares que puede usar en todo el sitio, consulte mi entrada de blog: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)

> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-4.md)
> [Siguiente](mvc-music-store-part-6.md)
