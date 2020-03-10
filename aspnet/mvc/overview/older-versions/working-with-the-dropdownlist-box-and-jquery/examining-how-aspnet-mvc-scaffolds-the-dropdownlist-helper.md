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
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78498361"
---
# <a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a><span data-ttu-id="98651-102">Examinar cómo ASP.NET MVC agrega un scaffold al asistente DropDownList</span><span class="sxs-lookup"><span data-stu-id="98651-102">Examining  how  ASP.NET MVC scaffolds the DropDownList Helper</span></span>

<span data-ttu-id="98651-103">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="98651-103">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="98651-104">En **Explorador de soluciones**, haga clic con el botón secundario en la carpeta *Controllers* y seleccione **Agregar controlador**.</span><span class="sxs-lookup"><span data-stu-id="98651-104">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span> <span data-ttu-id="98651-105">Asigne al controlador el nombre **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="98651-105">Name the controller **StoreManagerController**.</span></span> <span data-ttu-id="98651-106">Establezca las opciones del cuadro de diálogo **Agregar controlador** , tal como se muestra en la imagen siguiente.</span><span class="sxs-lookup"><span data-stu-id="98651-106">Set the options for the **Add Controller** dialog as shown in the image below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

<span data-ttu-id="98651-107">Edite la vista *StoreManager\Index.cshtml* y quite `AlbumArtUrl`.</span><span class="sxs-lookup"><span data-stu-id="98651-107">Edit the *StoreManager\Index.cshtml* view and remove `AlbumArtUrl`.</span></span> <span data-ttu-id="98651-108">Quitar `AlbumArtUrl` hará que la presentación sea más legible.</span><span class="sxs-lookup"><span data-stu-id="98651-108">Removing `AlbumArtUrl` will make the presentation more readable.</span></span> <span data-ttu-id="98651-109">A continuación se muestra el código completado.</span><span class="sxs-lookup"><span data-stu-id="98651-109">The completed code is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

<span data-ttu-id="98651-110">Abra el archivo *Controllers\StoreManagerController.CS* y busque el método `Index`.</span><span class="sxs-lookup"><span data-stu-id="98651-110">Open the *Controllers\StoreManagerController.cs* file and find the `Index` method.</span></span> <span data-ttu-id="98651-111">Agregue la cláusula `OrderBy` para que los álbumes se ordenen por precio.</span><span class="sxs-lookup"><span data-stu-id="98651-111">Add the `OrderBy` clause so the albums will be sorted by price.</span></span> <span data-ttu-id="98651-112">A continuación se muestra el código completo.</span><span class="sxs-lookup"><span data-stu-id="98651-112">The complete code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

<span data-ttu-id="98651-113">La ordenación por precio facilitará la prueba de los cambios en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="98651-113">Sorting by price will make it easier to test changes to the database.</span></span> <span data-ttu-id="98651-114">Cuando se prueban los métodos Edit y Create, se puede usar un precio bajo, por lo que los datos guardados aparecerán en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="98651-114">When you are testing the edit and create methods, you can use a low price so the saved data will appear first.</span></span>

<span data-ttu-id="98651-115">Abra el archivo *StoreManager\Edit.cshtml* .</span><span class="sxs-lookup"><span data-stu-id="98651-115">Open the *StoreManager\Edit.cshtml* file.</span></span> <span data-ttu-id="98651-116">Agregue la siguiente línea justo después de la etiqueta de leyenda.</span><span class="sxs-lookup"><span data-stu-id="98651-116">Add the following line just after the legend tag.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

<span data-ttu-id="98651-117">En el código siguiente se muestra el contexto de este cambio:</span><span class="sxs-lookup"><span data-stu-id="98651-117">The following code shows the context of this change:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

<span data-ttu-id="98651-118">El `AlbumId` es necesario para realizar cambios en un registro del álbum.</span><span class="sxs-lookup"><span data-stu-id="98651-118">The `AlbumId` is required to make changes to an album record.</span></span>

<span data-ttu-id="98651-119">Presione CTRL+F5 para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="98651-119">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="98651-120">Seleccione el vínculo **admin (Administrador** ) y, a continuación, seleccione el vínculo **crear nuevo** para crear un nuevo álbum.</span><span class="sxs-lookup"><span data-stu-id="98651-120">Select to the **Admin** link, then select the **Create New** link to create a new album.</span></span> <span data-ttu-id="98651-121">Compruebe que se ha guardado la información del álbum.</span><span class="sxs-lookup"><span data-stu-id="98651-121">Verify the album information was saved.</span></span> <span data-ttu-id="98651-122">Edite un álbum y compruebe que los cambios realizados son persistentes.</span><span class="sxs-lookup"><span data-stu-id="98651-122">Edit an album and verify the changes you made are persisted.</span></span>

### <a name="the-album-schema"></a><span data-ttu-id="98651-123">El esquema del álbum</span><span class="sxs-lookup"><span data-stu-id="98651-123">The Album Schema</span></span>

<span data-ttu-id="98651-124">El controlador de `StoreManager` creado por el mecanismo de scaffolding de MVC permite el acceso CRUD (crear, leer, actualizar, eliminar) a los álbumes en la base de datos del almacén de música.</span><span class="sxs-lookup"><span data-stu-id="98651-124">The `StoreManager` controller created by the MVC scaffolding mechanism allows CRUD (Create, Read, Update, Delete) access to the albums in the music store database.</span></span> <span data-ttu-id="98651-125">A continuación se muestra el esquema para la información del álbum:</span><span class="sxs-lookup"><span data-stu-id="98651-125">The schema for album information is shown below:</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

<span data-ttu-id="98651-126">La tabla `Albums` no almacena el género y la descripción del álbum, sino que almacena una clave externa en la tabla `Genres`.</span><span class="sxs-lookup"><span data-stu-id="98651-126">The `Albums` table does not store the album genre and description, it stores a foreign key to the `Genres` table.</span></span> <span data-ttu-id="98651-127">La tabla `Genres` contiene el nombre y la descripción del género.</span><span class="sxs-lookup"><span data-stu-id="98651-127">The `Genres` table contains the genre name and description.</span></span> <span data-ttu-id="98651-128">Del mismo modo, la tabla `Albums` no contiene el nombre de los intérpretes del álbum, sino una clave externa para la tabla `Artists`.</span><span class="sxs-lookup"><span data-stu-id="98651-128">Likewise, the `Albums` table doesn't contain the album artists name, but a foreign key to the `Artists` table.</span></span> <span data-ttu-id="98651-129">La tabla `Artists` contiene el nombre del artista.</span><span class="sxs-lookup"><span data-stu-id="98651-129">The `Artists` table contains the artist's name.</span></span> <span data-ttu-id="98651-130">Si examina los datos de la tabla de `Albums`, puede ver que cada fila contiene una clave externa para la tabla de `Genres` y una clave externa para la tabla de `Artists`.</span><span class="sxs-lookup"><span data-stu-id="98651-130">If you examine the data in the `Albums` table, you can see each row contains a foreign key to the `Genres` table and a foreign key to the `Artists` table.</span></span> <span data-ttu-id="98651-131">En la imagen siguiente se muestran algunos datos de tabla de la tabla `Albums`.</span><span class="sxs-lookup"><span data-stu-id="98651-131">The image below show some table data from the `Albums` table.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a><span data-ttu-id="98651-132">Etiqueta de selección HTML</span><span class="sxs-lookup"><span data-stu-id="98651-132">The HTML Select Tag</span></span>

<span data-ttu-id="98651-133">El elemento `<select>` HTML (creado por la aplicación auxiliar de [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) de HTML) se usa para mostrar una lista completa de valores (como la lista de géneros).</span><span class="sxs-lookup"><span data-stu-id="98651-133">The HTML `<select>` element (created by the HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper) is used to display a complete list of values (such as the list of genres).</span></span> <span data-ttu-id="98651-134">Para editar formularios, cuando se conoce el valor actual, la lista de selección puede mostrar el valor actual.</span><span class="sxs-lookup"><span data-stu-id="98651-134">For edit forms, when the current value is known, the select list can display the current value.</span></span> <span data-ttu-id="98651-135">Vimos esto anteriormente al establecer el valor seleccionado en **comedia**.</span><span class="sxs-lookup"><span data-stu-id="98651-135">We saw this previously when we set the selected value to **Comedy**.</span></span> <span data-ttu-id="98651-136">La lista de selección es ideal para Mostrar datos de categoría o de clave externa.</span><span class="sxs-lookup"><span data-stu-id="98651-136">The select list is ideal for displaying category or foreign key data.</span></span> <span data-ttu-id="98651-137">El elemento `<select>` de la clave externa Genre muestra la lista de posibles nombres de género, pero al guardar el formulario la propiedad Genre se actualiza con el valor de clave externa Genre, no con el nombre de género mostrado.</span><span class="sxs-lookup"><span data-stu-id="98651-137">The `<select>` element for the Genre foreign key displays the list of possible genre names, but when you save the form the Genre property is updated with the Genre foreign key value, not the displayed genre name.</span></span> <span data-ttu-id="98651-138">En la imagen siguiente, el género seleccionado es **disco** y el artista es **Donna verano**.</span><span class="sxs-lookup"><span data-stu-id="98651-138">In the image below, the genre selected is **Disco** and the artist is **Donna Summer**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a><span data-ttu-id="98651-139">Examinar el código con scaffolding de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="98651-139">Examining the ASP.NET MVC Scaffolded Code</span></span>

<span data-ttu-id="98651-140">Abra el archivo *Controllers\StoreManagerController.CS* y busque el método `HTTP GET Create`.</span><span class="sxs-lookup"><span data-stu-id="98651-140">Open the *Controllers\StoreManagerController.cs* file and find the `HTTP GET Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

<span data-ttu-id="98651-141">El método `Create` agrega dos objetos [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) al `ViewBag`, uno para contener la información del género y otro para contener la información del intérprete.</span><span class="sxs-lookup"><span data-stu-id="98651-141">The `Create` method adds two [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objects to the `ViewBag`, one to contain the genre information, and one to contain the artist information.</span></span> <span data-ttu-id="98651-142">La sobrecarga del constructor [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) utilizada anteriormente toma tres argumentos:</span><span class="sxs-lookup"><span data-stu-id="98651-142">The [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) constructor overload used above takes three arguments:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. <span data-ttu-id="98651-143">*elementos*: objeto [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) que contiene los elementos de la lista.</span><span class="sxs-lookup"><span data-stu-id="98651-143">*items*: An [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) containing the items in the list.</span></span> <span data-ttu-id="98651-144">En el ejemplo anterior, la lista de géneros devuelta por `db.Genres`.</span><span class="sxs-lookup"><span data-stu-id="98651-144">In the example above, the list of genres returned by `db.Genres`.</span></span>
2. <span data-ttu-id="98651-145">*dataValueField*: el nombre de la propiedad en la lista **IEnumerable** que contiene el valor de clave.</span><span class="sxs-lookup"><span data-stu-id="98651-145">*dataValueField*: The name of the property in the **IEnumerable** list that contains the key value.</span></span> <span data-ttu-id="98651-146">En el ejemplo anterior, `GenreId` y `ArtistId`.</span><span class="sxs-lookup"><span data-stu-id="98651-146">In the example above, `GenreId` and `ArtistId`.</span></span>
3. <span data-ttu-id="98651-147">*DataTextField*: el nombre de la propiedad en la lista **IEnumerable** que contiene la información que se va a mostrar.</span><span class="sxs-lookup"><span data-stu-id="98651-147">*dataTextField*: The name of the property in the **IEnumerable** list that contains the information to display.</span></span> <span data-ttu-id="98651-148">En la tabla artistas y género, se utiliza el campo `name`.</span><span class="sxs-lookup"><span data-stu-id="98651-148">In both the artists and genre table, the `name` field is used.</span></span>

<span data-ttu-id="98651-149">Abra el archivo *Views\StoreManager\Create.cshtml* y examine el marcado de la aplicación auxiliar `Html.DropDownList` para el campo Genre.</span><span class="sxs-lookup"><span data-stu-id="98651-149">Open the *Views\StoreManager\Create.cshtml* file and examine the `Html.DropDownList` helper markup for the genre field.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

<span data-ttu-id="98651-150">La primera línea muestra que la vista de creación toma un modelo de `Album`.</span><span class="sxs-lookup"><span data-stu-id="98651-150">The first line shows that the create view takes an `Album` model.</span></span> <span data-ttu-id="98651-151">En el método `Create` mostrado anteriormente, no se pasó ningún modelo, por lo que la vista obtiene un modelo de `Album` **null** .</span><span class="sxs-lookup"><span data-stu-id="98651-151">In the `Create` method shown above, no model was passed, so the view gets a **null** `Album` model.</span></span> <span data-ttu-id="98651-152">En este momento, vamos a crear un nuevo álbum para que no tengamos ningún `Album` datos para él.</span><span class="sxs-lookup"><span data-stu-id="98651-152">At this point we are creating a new album so we don't have any `Album` data for it.</span></span>

<span data-ttu-id="98651-153">La sobrecarga [HTML. DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) mostrada anteriormente toma el nombre del campo que se va a enlazar al modelo.</span><span class="sxs-lookup"><span data-stu-id="98651-153">The [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) overload shown above takes the name of the field to bind to the model.</span></span> <span data-ttu-id="98651-154">También usa este nombre para buscar un objeto **ViewBag** que contenga un objeto [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) .</span><span class="sxs-lookup"><span data-stu-id="98651-154">It also uses this name to look for a **ViewBag** object containing a [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) object.</span></span> <span data-ttu-id="98651-155">Con esta sobrecarga, es necesario asignar un nombre al objeto **ViewBag SelectList** `GenreId`.</span><span class="sxs-lookup"><span data-stu-id="98651-155">Using this overload, you are required to name the **ViewBag SelectList** object `GenreId`.</span></span> <span data-ttu-id="98651-156">El segundo parámetro (`String.Empty`) es el texto que se muestra cuando no se selecciona ningún elemento.</span><span class="sxs-lookup"><span data-stu-id="98651-156">The second parameter (`String.Empty`) is the text to display when no item is selected.</span></span> <span data-ttu-id="98651-157">Esto es exactamente lo que queremos al crear un nuevo álbum.</span><span class="sxs-lookup"><span data-stu-id="98651-157">This is exactly what we want when creating a new album.</span></span> <span data-ttu-id="98651-158">Si quitó el segundo parámetro y utilizó el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="98651-158">If you removed the second parameter and used the following code:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

<span data-ttu-id="98651-159">La lista de selección tendría como valor predeterminado el primer elemento o rock en nuestro ejemplo.</span><span class="sxs-lookup"><span data-stu-id="98651-159">The select list would default to the first element, or Rock in our sample.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

<span data-ttu-id="98651-160">Examinando el método de `HTTP POST Create`.</span><span class="sxs-lookup"><span data-stu-id="98651-160">Examining the `HTTP POST Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

<span data-ttu-id="98651-161">Esta sobrecarga del método `Create` toma un objeto `album`, creado por el sistema de enlace del modelo MVC de ASP.NET a partir de los valores de formulario enviados.</span><span class="sxs-lookup"><span data-stu-id="98651-161">This overload of the `Create` method takes an `album` object, created by the ASP.NET MVC model binding system from the form values posted.</span></span> <span data-ttu-id="98651-162">Cuando se envía un nuevo álbum, si el estado del modelo es válido y no hay ningún error de base de datos, se agrega la base de datos al nuevo álbum.</span><span class="sxs-lookup"><span data-stu-id="98651-162">When you submit a new album, if model state is valid and there are no database errors, the new album is added the database.</span></span> <span data-ttu-id="98651-163">En la imagen siguiente se muestra la creación de un nuevo álbum.</span><span class="sxs-lookup"><span data-stu-id="98651-163">The following image shows the creation of a new album.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

<span data-ttu-id="98651-164">Puede usar la [herramienta Fiddler](http://www.fiddler2.com/fiddler2/) para examinar los valores de formulario publicados que usa el enlace de modelos MVC de ASP.net para crear el objeto Album.</span><span class="sxs-lookup"><span data-stu-id="98651-164">You can use the [fiddler tool](http://www.fiddler2.com/fiddler2/) to examine the posted form values that ASP.NET MVC model binding uses to create the album object.</span></span>

<span data-ttu-id="98651-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png)Operador</span><span class="sxs-lookup"><span data-stu-id="98651-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span></span>

### <a name="refactoring-the-viewbag-selectlist-creation"></a><span data-ttu-id="98651-166">Refactorización de la creación de ViewBag SelectList</span><span class="sxs-lookup"><span data-stu-id="98651-166">Refactoring the ViewBag SelectList Creation</span></span>

<span data-ttu-id="98651-167">Tanto los métodos `Edit` como el método `HTTP POST Create` tienen código idéntico para configurar **SelectList** en **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="98651-167">Both the `Edit` methods and the `HTTP POST Create` method have identical code to set up the **SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="98651-168">En el espíritu de [seco](http://en.wikipedia.org/wiki/Don't_repeat_yourself), se refactoriza este código.</span><span class="sxs-lookup"><span data-stu-id="98651-168">In the spirit of [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), we will refactor this code.</span></span> <span data-ttu-id="98651-169">Más adelante usaremos este código refactorizado.</span><span class="sxs-lookup"><span data-stu-id="98651-169">We'll make use of this refactored code later.</span></span>

<span data-ttu-id="98651-170">Cree un nuevo método para agregar un género y un artista **SelectList** a **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="98651-170">Create a new method to add a genre and artist **SelectList** to the **ViewBag**.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

<span data-ttu-id="98651-171">Reemplace las dos líneas estableciendo el `ViewBag` en cada uno de los métodos `Create` y `Edit` con una llamada al método `SetGenreArtistViewBag`.</span><span class="sxs-lookup"><span data-stu-id="98651-171">Replace the two lines setting the `ViewBag` in each of the `Create` and `Edit` methods with a call to the `SetGenreArtistViewBag` method.</span></span> <span data-ttu-id="98651-172">A continuación se muestra el código completado.</span><span class="sxs-lookup"><span data-stu-id="98651-172">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

<span data-ttu-id="98651-173">Cree un nuevo álbum y edite un álbum para comprobar que los cambios funcionan.</span><span class="sxs-lookup"><span data-stu-id="98651-173">Create a new album and edit an album to verify the changes work.</span></span>

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a><span data-ttu-id="98651-174">Pasar explícitamente SelectList a DropDownList</span><span class="sxs-lookup"><span data-stu-id="98651-174">Explicitly Passing the SelectList to the DropDownList</span></span>

<span data-ttu-id="98651-175">Las vistas de creación y edición creadas por el scaffolding de ASP.NET MVC usan la siguiente sobrecarga de **DropDownList** :</span><span class="sxs-lookup"><span data-stu-id="98651-175">The create and edit views created by the ASP.NET MVC scaffolding use the following **DropDownList** overload:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

<span data-ttu-id="98651-176">A continuación se muestra el marcado de `DropDownList` para la vista crear.</span><span class="sxs-lookup"><span data-stu-id="98651-176">The `DropDownList` markup for the create view is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

<span data-ttu-id="98651-177">Dado que la propiedad `ViewBag` del `SelectList` se llama `GenreId`, la aplicación auxiliar de **DropDownList** usará el `GenreId`**SelectList** en **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="98651-177">Because the `ViewBag` property for the `SelectList` is named `GenreId`, the **DropDownList** helper will use the `GenreId`**SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="98651-178">En la siguiente sobrecarga de **DropDownList** , el `SelectList` se pasa explícitamente.</span><span class="sxs-lookup"><span data-stu-id="98651-178">In the following **DropDownList** overload, the `SelectList` is explicitly passed in.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

<span data-ttu-id="98651-179">Abra el archivo *Views\StoreManager\Edit.cshtml* y cambie la llamada de **DropDownList** para pasar explícitamente el **SelectList**con la sobrecarga anterior.</span><span class="sxs-lookup"><span data-stu-id="98651-179">Open the *Views\StoreManager\Edit.cshtml* file, and change the **DropDownList** call to explicitly pass in the **SelectList**, using the overload above.</span></span> <span data-ttu-id="98651-180">Haga esto para la categoría género.</span><span class="sxs-lookup"><span data-stu-id="98651-180">Do this for the Genre category.</span></span> <span data-ttu-id="98651-181">A continuación se muestra el código completado:</span><span class="sxs-lookup"><span data-stu-id="98651-181">The completed code is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

<span data-ttu-id="98651-182">Ejecute la aplicación y haga clic en el vínculo **admin** , desplácese a un álbum de jazz y seleccione el vínculo **Editar** .</span><span class="sxs-lookup"><span data-stu-id="98651-182">Run the application and click the **Admin** link, then navigate to a Jazz album and select the **Edit** link.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

<span data-ttu-id="98651-183">En lugar de mostrar jazz como el género seleccionado actualmente, se muestra Rock.</span><span class="sxs-lookup"><span data-stu-id="98651-183">Instead of showing Jazz as the currently selected genre, Rock is displayed.</span></span> <span data-ttu-id="98651-184">Cuando el argumento de cadena (la propiedad que se va a enlazar) y el objeto **SelectList** tienen el mismo nombre, no se utiliza el valor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="98651-184">When the string argument (the property to bind) and the **SelectList** object have the same name, the selected value is not used.</span></span> <span data-ttu-id="98651-185">Cuando no se proporciona ningún valor seleccionado, los exploradores tienen como valor predeterminado el primer elemento del **SelectList**(que es **Rock** en el ejemplo anterior).</span><span class="sxs-lookup"><span data-stu-id="98651-185">When there is no selected value provided, browsers default to the first element in the **SelectList**(which is **Rock** in the example above).</span></span> <span data-ttu-id="98651-186">Se trata de una limitación conocida de la aplicación auxiliar de **DropDownList** .</span><span class="sxs-lookup"><span data-stu-id="98651-186">This is a known limitation of the **DropDownList** helper.</span></span>

<span data-ttu-id="98651-187">Abra el archivo *Controllers\StoreManagerController.CS* y cambie los nombres de objeto **SelectList** a `Genres` y `Artists`.</span><span class="sxs-lookup"><span data-stu-id="98651-187">Open the *Controllers\StoreManagerController.cs* file and change the **SelectList** object names to `Genres` and `Artists`.</span></span> <span data-ttu-id="98651-188">A continuación se muestra el código completado:</span><span class="sxs-lookup"><span data-stu-id="98651-188">The completed code is shown below:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

<span data-ttu-id="98651-189">Los nombres géneros y artistas son mejores nombres para las categorías, ya que contienen más que el identificador de cada categoría.</span><span class="sxs-lookup"><span data-stu-id="98651-189">The names Genres and Artists are better names for the categories, as they contain more than just the ID of each category.</span></span> <span data-ttu-id="98651-190">La refactorización que hicimos anteriormente se pagó.</span><span class="sxs-lookup"><span data-stu-id="98651-190">The refactoring we did earlier paid off.</span></span> <span data-ttu-id="98651-191">En lugar de cambiar **ViewBag** en cuatro métodos, los cambios se aislaron en el método `SetGenreArtistViewBag`.</span><span class="sxs-lookup"><span data-stu-id="98651-191">Instead of changing the **ViewBag** in four methods, our changes were isolated to the `SetGenreArtistViewBag` method.</span></span>

<span data-ttu-id="98651-192">Cambie la llamada de **DropDownList** en las vistas de creación y edición para usar los nuevos nombres de **SelectList** .</span><span class="sxs-lookup"><span data-stu-id="98651-192">Change the **DropDownList** call in the create and edit views to use the new **SelectList** names.</span></span> <span data-ttu-id="98651-193">A continuación se muestra el nuevo marcado para la vista de edición:</span><span class="sxs-lookup"><span data-stu-id="98651-193">The new markup for the edit view is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

<span data-ttu-id="98651-194">La vista Create requiere una cadena vacía para evitar que se muestre el primer elemento del SelectList.</span><span class="sxs-lookup"><span data-stu-id="98651-194">The Create view requires an empty string to prevent the first item in the SelectList from being displayed.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

<span data-ttu-id="98651-195">Cree un nuevo álbum y edite un álbum para comprobar que los cambios funcionan.</span><span class="sxs-lookup"><span data-stu-id="98651-195">Create a new album and edit an album to verify the changes work.</span></span> <span data-ttu-id="98651-196">Pruebe el código de edición seleccionando un álbum con un género distinto de rock.</span><span class="sxs-lookup"><span data-stu-id="98651-196">Test the edit code by selecting an album with a genre other than Rock.</span></span>

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a><span data-ttu-id="98651-197">Usar un modelo de vista con la aplicación auxiliar de DropDownList</span><span class="sxs-lookup"><span data-stu-id="98651-197">Using a View Model with the DropDownList Helper</span></span>

<span data-ttu-id="98651-198">Cree una nueva clase en la carpeta ViewModels denominada `AlbumSelectListViewModel`.</span><span class="sxs-lookup"><span data-stu-id="98651-198">Create a new class in the ViewModels folder named `AlbumSelectListViewModel`.</span></span> <span data-ttu-id="98651-199">Reemplace el código de la clase `AlbumSelectListViewModel` por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="98651-199">Replace the code in the `AlbumSelectListViewModel` class with the following:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

<span data-ttu-id="98651-200">El constructor de `AlbumSelectListViewModel` toma un álbum, una lista de artistas y géneros, y crea un objeto que contiene el álbum y una `SelectList` para los géneros y artistas.</span><span class="sxs-lookup"><span data-stu-id="98651-200">The `AlbumSelectListViewModel` constructor takes an album, a list of artists and genres and creates an object containing the album and a `SelectList` for genres and artists.</span></span>

<span data-ttu-id="98651-201">Compile el proyecto para que el `AlbumSelectListViewModel` esté disponible al crear una vista en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="98651-201">Build the project so the `AlbumSelectListViewModel` is available when we create a view in the next step.</span></span>

<span data-ttu-id="98651-202">Agregue un método `EditVM` a la `StoreManagerController`.</span><span class="sxs-lookup"><span data-stu-id="98651-202">Add an `EditVM` method to the `StoreManagerController`.</span></span> <span data-ttu-id="98651-203">A continuación se muestra el código completado.</span><span class="sxs-lookup"><span data-stu-id="98651-203">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

<span data-ttu-id="98651-204">Haga clic con el botón derecho en `AlbumSelectListViewModel`, seleccione **resolver**y, a continuación, **use MvcMusicStore. ViewModels;** .</span><span class="sxs-lookup"><span data-stu-id="98651-204">Right click `AlbumSelectListViewModel`, select **Resolve**, then **using MvcMusicStore.ViewModels;**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

<span data-ttu-id="98651-205">Como alternativa, puede Agregar la siguiente instrucción using:</span><span class="sxs-lookup"><span data-stu-id="98651-205">Alternatively, you can add the following using statement:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

<span data-ttu-id="98651-206">Haga clic con el botón derecho en `EditVM` y seleccione **Agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="98651-206">Right click `EditVM` and select **Add View**.</span></span> <span data-ttu-id="98651-207">Use las opciones que se muestran a continuación.</span><span class="sxs-lookup"><span data-stu-id="98651-207">Use the options shown below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

<span data-ttu-id="98651-208">Seleccione **Agregar**y, a continuación, reemplace el contenido del archivo *Views\StoreManager\EditVM.cshtml* por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="98651-208">Select **Add**, then replace the contents of the *Views\StoreManager\EditVM.cshtml* file with the following:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

<span data-ttu-id="98651-209">El marcado `EditVM` es muy similar al marcado de `Edit` original con las excepciones siguientes.</span><span class="sxs-lookup"><span data-stu-id="98651-209">The `EditVM` markup is very similar to the original `Edit` markup with the following exceptions.</span></span>

- <span data-ttu-id="98651-210">Las propiedades del modelo en la vista `Edit` tienen el formato `model.property`(por ejemplo, `model.Title`).</span><span class="sxs-lookup"><span data-stu-id="98651-210">Model properties in the `Edit` view are of the form `model.property`(for example, `model.Title` ).</span></span> <span data-ttu-id="98651-211">Las propiedades del modelo en la vista `EditVm` tienen el formato `model.Album.property`(por ejemplo, `model.Album.Title`).</span><span class="sxs-lookup"><span data-stu-id="98651-211">Model properties in the `EditVm` view are of the form `model.Album.property`(for example, `model.Album.Title`).</span></span> <span data-ttu-id="98651-212">Esto se debe a que la vista de `EditVM` se pasa a un contenedor para un `Album`, no a un `Album` como en la vista de `Edit`.</span><span class="sxs-lookup"><span data-stu-id="98651-212">That's because the `EditVM` view is passed a container for an `Album`, not an `Album` as in the `Edit` view.</span></span>
- <span data-ttu-id="98651-213">El segundo parámetro de **DropDownList** procede del modelo de vista, no de **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="98651-213">The **DropDownList** second parameter comes from the view model, not the **ViewBag**.</span></span>
- <span data-ttu-id="98651-214">La aplicación auxiliar **BeginForm** en la `EditVM` vista se devuelve explícitamente al método de acción `Edit`.</span><span class="sxs-lookup"><span data-stu-id="98651-214">The **BeginForm** helper in the `EditVM` view explicitly posts back to the `Edit` action method.</span></span> <span data-ttu-id="98651-215">Al volver a la acción de `Edit`, no es necesario escribir una acción de `HTTP POST EditVM` y puede reutilizar la acción `Edit` `HTTP POST`.</span><span class="sxs-lookup"><span data-stu-id="98651-215">By posting back to the `Edit` action, we don't have to write an `HTTP POST EditVM` action and can reuse the `HTTP POST` `Edit` action.</span></span>

<span data-ttu-id="98651-216">Ejecute la aplicación y edite un álbum.</span><span class="sxs-lookup"><span data-stu-id="98651-216">Run the application and edit an album.</span></span> <span data-ttu-id="98651-217">Cambie la dirección URL para utilizar `EditVM`.</span><span class="sxs-lookup"><span data-stu-id="98651-217">Change the URL to use `EditVM`.</span></span> <span data-ttu-id="98651-218">Cambie un campo y presione el botón **Guardar** para comprobar que el código funciona.</span><span class="sxs-lookup"><span data-stu-id="98651-218">Change a field and hit the **Save** button to verify the code is working.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a><span data-ttu-id="98651-219">¿Qué enfoque debería usar?</span><span class="sxs-lookup"><span data-stu-id="98651-219">Which Approach Should You Use?</span></span>

<span data-ttu-id="98651-220">Los tres enfoques mostrados son aceptables.</span><span class="sxs-lookup"><span data-stu-id="98651-220">All three approaches shown are acceptable.</span></span> <span data-ttu-id="98651-221">Muchos desarrolladores prefieren pasar explícitamente el `SelectList` al `DropDownList` mediante el `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="98651-221">Many developers prefer to explicitly pass the `SelectList` to the `DropDownList` using the `ViewBag`.</span></span> <span data-ttu-id="98651-222">Este enfoque tiene la ventaja adicional de ofrecerle la flexibilidad de usar un nombre más adecuado para la colección.</span><span class="sxs-lookup"><span data-stu-id="98651-222">This approach has the added advantage of giving you the flexibility of using a more appropriate name for the collection.</span></span> <span data-ttu-id="98651-223">Una advertencia es que no puede asignar al objeto `ViewBag SelectList` el mismo nombre que la propiedad del modelo.</span><span class="sxs-lookup"><span data-stu-id="98651-223">The one caveat is you cannot name the `ViewBag SelectList` object the same name as the model property.</span></span>

<span data-ttu-id="98651-224">Algunos desarrolladores prefieren el enfoque de ViewModel.</span><span class="sxs-lookup"><span data-stu-id="98651-224">Some developers prefer the ViewModel approach.</span></span> <span data-ttu-id="98651-225">Otros consideran el marcado más detallado y el HTML generado del enfoque de ViewModel como una desventaja.</span><span class="sxs-lookup"><span data-stu-id="98651-225">Others consider the more verbose markup and generated HTML of the ViewModel approach a disadvantage.</span></span>

<span data-ttu-id="98651-226">En esta sección hemos aprendido tres enfoques para usar el **DropDownList** con datos de categoría.</span><span class="sxs-lookup"><span data-stu-id="98651-226">In this section we have learned three approaches to using the **DropDownList** with category data.</span></span> <span data-ttu-id="98651-227">En la siguiente sección, mostraremos cómo agregar una nueva categoría.</span><span class="sxs-lookup"><span data-stu-id="98651-227">In the next section, we'll show how to add a new category.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="98651-228">[Anterior](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [Siguiente](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="98651-228">[Previous](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[Next](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span></span>
