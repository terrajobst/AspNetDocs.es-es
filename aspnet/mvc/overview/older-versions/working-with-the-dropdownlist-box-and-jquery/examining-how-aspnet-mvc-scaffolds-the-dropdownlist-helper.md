---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: Examinar cómo ASP.NET MVC scaffolding la aplicación auxiliar de DropDownList | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 275b20ad964b3e8ddc272a7448f0740ed0891eff
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457614"
---
# <a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a>Examinar cómo ASP.NET MVC agrega un scaffold al asistente DropDownList

por [Rick Anderson](https://twitter.com/RickAndMSFT)

En **Explorador de soluciones**, haga clic con el botón secundario en la carpeta *Controllers* y seleccione **Agregar controlador**. Asigne al controlador el nombre **StoreManagerController**. Establezca las opciones del cuadro de diálogo **Agregar controlador** , tal como se muestra en la imagen siguiente.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

Edite la vista *StoreManager\Index.cshtml* y quite `AlbumArtUrl`. Quitar `AlbumArtUrl` hará que la presentación sea más legible. A continuación se muestra el código completado.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

Abra el archivo *Controllers\StoreManagerController.CS* y busque el método `Index`. Agregue la cláusula `OrderBy` para que los álbumes se ordenen por precio. A continuación se muestra el código completo.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

La ordenación por precio facilitará la prueba de los cambios en la base de datos. Cuando se prueban los métodos Edit y Create, se puede usar un precio bajo, por lo que los datos guardados aparecerán en primer lugar.

Abra el archivo *StoreManager\Edit.cshtml* . Agregue la siguiente línea justo después de la etiqueta de leyenda.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

En el código siguiente se muestra el contexto de este cambio:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

El `AlbumId` es necesario para realizar cambios en un registro del álbum.

Presione CTRL+F5 para ejecutar la aplicación. Seleccione el vínculo **admin (Administrador** ) y, a continuación, seleccione el vínculo **crear nuevo** para crear un nuevo álbum. Compruebe que se ha guardado la información del álbum. Edite un álbum y compruebe que los cambios realizados son persistentes.

### <a name="the-album-schema"></a>El esquema del álbum

El controlador de `StoreManager` creado por el mecanismo de scaffolding de MVC permite el acceso CRUD (crear, leer, actualizar, eliminar) a los álbumes en la base de datos del almacén de música. A continuación se muestra el esquema para la información del álbum:

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

La tabla `Albums` no almacena el género y la descripción del álbum, sino que almacena una clave externa en la tabla `Genres`. La tabla `Genres` contiene el nombre y la descripción del género. Del mismo modo, la tabla `Albums` no contiene el nombre de los intérpretes del álbum, sino una clave externa para la tabla `Artists`. La tabla `Artists` contiene el nombre del artista. Si examina los datos de la tabla de `Albums`, puede ver que cada fila contiene una clave externa para la tabla de `Genres` y una clave externa para la tabla de `Artists`. En la imagen siguiente se muestran algunos datos de tabla de la tabla `Albums`.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a>Etiqueta de selección HTML

El elemento `<select>` HTML (creado por la aplicación auxiliar de [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) de HTML) se usa para mostrar una lista completa de valores (como la lista de géneros). Para editar formularios, cuando se conoce el valor actual, la lista de selección puede mostrar el valor actual. Vimos esto anteriormente al establecer el valor seleccionado en **comedia**. La lista de selección es ideal para Mostrar datos de categoría o de clave externa. El elemento `<select>` de la clave externa Genre muestra la lista de posibles nombres de género, pero al guardar el formulario la propiedad Genre se actualiza con el valor de clave externa Genre, no con el nombre de género mostrado. En la imagen siguiente, el género seleccionado es **disco** y el artista es **Donna verano**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a>Examinar el código con scaffolding de ASP.NET MVC

Abra el archivo *Controllers\StoreManagerController.CS* y busque el método `HTTP GET Create`.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

El método `Create` agrega dos objetos [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) al `ViewBag`, uno para contener la información del género y otro para contener la información del intérprete. La sobrecarga del constructor [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) utilizada anteriormente toma tres argumentos:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. *elementos*: objeto [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) que contiene los elementos de la lista. En el ejemplo anterior, la lista de géneros devuelta por `db.Genres`.
2. *dataValueField*: el nombre de la propiedad en la lista **IEnumerable** que contiene el valor de clave. En el ejemplo anterior, `GenreId` y `ArtistId`.
3. *DataTextField*: el nombre de la propiedad en la lista **IEnumerable** que contiene la información que se va a mostrar. En la tabla artistas y género, se utiliza el campo `name`.

Abra el archivo *Views\StoreManager\Create.cshtml* y examine el marcado de la aplicación auxiliar `Html.DropDownList` para el campo Genre.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

La primera línea muestra que la vista de creación toma un modelo de `Album`. En el método `Create` mostrado anteriormente, no se pasó ningún modelo, por lo que la vista obtiene un modelo de `Album` **null** . En este momento, vamos a crear un nuevo álbum para que no tengamos ningún `Album` datos para él.

La sobrecarga [HTML. DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) mostrada anteriormente toma el nombre del campo que se va a enlazar al modelo. También usa este nombre para buscar un objeto **ViewBag** que contenga un objeto [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) . Con esta sobrecarga, es necesario asignar un nombre al objeto **ViewBag SelectList** `GenreId`. El segundo parámetro (`String.Empty`) es el texto que se muestra cuando no se selecciona ningún elemento. Esto es exactamente lo que queremos al crear un nuevo álbum. Si quitó el segundo parámetro y utilizó el siguiente código:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

La lista de selección tendría como valor predeterminado el primer elemento o rock en nuestro ejemplo.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

Examinando el método de `HTTP POST Create`.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

Esta sobrecarga del método `Create` toma un objeto `album`, creado por el sistema de enlace del modelo MVC de ASP.NET a partir de los valores de formulario enviados. Cuando se envía un nuevo álbum, si el estado del modelo es válido y no hay ningún error de base de datos, se agrega la base de datos al nuevo álbum. En la imagen siguiente se muestra la creación de un nuevo álbum.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

Puede usar la [herramienta Fiddler](http://www.fiddler2.com/fiddler2/) para examinar los valores de formulario publicados que usa el enlace de modelos MVC de ASP.net para crear el objeto Album.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).

### <a name="refactoring-the-viewbag-selectlist-creation"></a>Refactorización de la creación de ViewBag SelectList

Tanto los métodos `Edit` como el método `HTTP POST Create` tienen código idéntico para configurar **SelectList** en **ViewBag**. En el espíritu de [seco](http://en.wikipedia.org/wiki/Don't_repeat_yourself), se refactoriza este código. Más adelante usaremos este código refactorizado.

Cree un nuevo método para agregar un género y un artista **SelectList** a **ViewBag**.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

Reemplace las dos líneas estableciendo el `ViewBag` en cada uno de los métodos `Create` y `Edit` con una llamada al método `SetGenreArtistViewBag`. A continuación se muestra el código completado.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

Cree un nuevo álbum y edite un álbum para comprobar que los cambios funcionan.

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a>Pasar explícitamente SelectList a DropDownList

Las vistas de creación y edición creadas por el scaffolding de ASP.NET MVC usan la siguiente sobrecarga de **DropDownList** :

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

A continuación se muestra el marcado de `DropDownList` para la vista crear.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

Dado que la propiedad `ViewBag` del `SelectList` se llama `GenreId`, la aplicación auxiliar de **DropDownList** usará el `GenreId`**SelectList** en **ViewBag**. En la siguiente sobrecarga de **DropDownList** , el `SelectList` se pasa explícitamente.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

Abra el archivo *Views\StoreManager\Edit.cshtml* y cambie la llamada de **DropDownList** para pasar explícitamente el **SelectList**con la sobrecarga anterior. Haga esto para la categoría género. A continuación se muestra el código completado:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

Ejecute la aplicación y haga clic en el vínculo **admin** , desplácese a un álbum de jazz y seleccione el vínculo **Editar** .

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

En lugar de mostrar jazz como el género seleccionado actualmente, se muestra Rock. Cuando el argumento de cadena (la propiedad que se va a enlazar) y el objeto **SelectList** tienen el mismo nombre, no se utiliza el valor seleccionado. Cuando no se proporciona ningún valor seleccionado, los exploradores tienen como valor predeterminado el primer elemento del **SelectList**(que es **Rock** en el ejemplo anterior). Se trata de una limitación conocida de la aplicación auxiliar de **DropDownList** .

Abra el archivo *Controllers\StoreManagerController.CS* y cambie los nombres de objeto **SelectList** a `Genres` y `Artists`. A continuación se muestra el código completado:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

Los nombres géneros y artistas son mejores nombres para las categorías, ya que contienen más que el identificador de cada categoría. La refactorización que hicimos anteriormente se pagó. En lugar de cambiar **ViewBag** en cuatro métodos, los cambios se aislaron en el método `SetGenreArtistViewBag`.

Cambie la llamada de **DropDownList** en las vistas de creación y edición para usar los nuevos nombres de **SelectList** . A continuación se muestra el nuevo marcado para la vista de edición:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

La vista Create requiere una cadena vacía para evitar que se muestre el primer elemento del SelectList.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

Cree un nuevo álbum y edite un álbum para comprobar que los cambios funcionan. Pruebe el código de edición seleccionando un álbum con un género distinto de rock.

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a>Usar un modelo de vista con la aplicación auxiliar de DropDownList

Cree una nueva clase en la carpeta ViewModels denominada `AlbumSelectListViewModel`. Reemplace el código de la clase `AlbumSelectListViewModel` por lo siguiente:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

El constructor de `AlbumSelectListViewModel` toma un álbum, una lista de artistas y géneros, y crea un objeto que contiene el álbum y una `SelectList` para los géneros y artistas.

Compile el proyecto para que el `AlbumSelectListViewModel` esté disponible al crear una vista en el paso siguiente.

Agregue un método `EditVM` a la `StoreManagerController`. A continuación se muestra el código completado.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

Haga clic con el botón derecho en `AlbumSelectListViewModel`, seleccione **resolver**y, a continuación, **use MvcMusicStore. ViewModels;** .

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

Como alternativa, puede Agregar la siguiente instrucción using:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

Haga clic con el botón derecho en `EditVM` y seleccione **Agregar vista**. Use las opciones que se muestran a continuación.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

Seleccione **Agregar**y, a continuación, reemplace el contenido del archivo *Views\StoreManager\EditVM.cshtml* por lo siguiente:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

El marcado `EditVM` es muy similar al marcado de `Edit` original con las excepciones siguientes.

- Las propiedades del modelo en la vista `Edit` tienen el formato `model.property`(por ejemplo, `model.Title`). Las propiedades del modelo en la vista `EditVm` tienen el formato `model.Album.property`(por ejemplo, `model.Album.Title`). Esto se debe a que la vista de `EditVM` se pasa a un contenedor para un `Album`, no a un `Album` como en la vista de `Edit`.
- El segundo parámetro de **DropDownList** procede del modelo de vista, no de **ViewBag**.
- La aplicación auxiliar **BeginForm** en la `EditVM` vista se devuelve explícitamente al método de acción `Edit`. Al volver a la acción de `Edit`, no es necesario escribir una acción de `HTTP POST EditVM` y puede reutilizar la acción `Edit` `HTTP POST`.

Ejecute la aplicación y edite un álbum. Cambie la dirección URL para utilizar `EditVM`. Cambie un campo y presione el botón **Guardar** para comprobar que el código funciona.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a>¿Qué enfoque debería usar?

Los tres enfoques mostrados son aceptables. Muchos desarrolladores prefieren pasar explícitamente el `SelectList` al `DropDownList` mediante el `ViewBag`. Este enfoque tiene la ventaja adicional de ofrecerle la flexibilidad de usar un nombre más adecuado para la colección. Una advertencia es que no puede asignar al objeto `ViewBag SelectList` el mismo nombre que la propiedad del modelo.

Algunos desarrolladores prefieren el enfoque de ViewModel. Otros consideran el marcado más detallado y el HTML generado del enfoque de ViewModel como una desventaja.

En esta sección hemos aprendido tres enfoques para usar el **DropDownList** con datos de categoría. En la siguiente sección, mostraremos cómo agregar una nueva categoría.

> [!div class="step-by-step"]
> [Anterior](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [Siguiente](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)
