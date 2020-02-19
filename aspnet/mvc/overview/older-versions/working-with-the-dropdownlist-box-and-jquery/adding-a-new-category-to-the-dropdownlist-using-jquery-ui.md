---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Agregar una nueva categoría a DropDownList con la interfaz de usuario de jQuery | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 3207079ee468232e5f75b081421241c232936baf
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77455729"
---
# <a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a><span data-ttu-id="57102-102">Agregar una nueva categoría al control DropDownList mediante jQuery UI</span><span class="sxs-lookup"><span data-stu-id="57102-102">Adding a New Category to the DropDownList using jQuery UI</span></span>

<span data-ttu-id="57102-103">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="57102-103">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="57102-104">La etiqueta de `Select` HTML es ideal para presentar una lista de datos de categoría fija, pero a menudo es necesario agregar una nueva categoría.</span><span class="sxs-lookup"><span data-stu-id="57102-104">The HTML `Select` tag is ideal for presenting a list of fixed category data, but often times you need to add a new category.</span></span> <span data-ttu-id="57102-105">Supongamos que queremos agregar el género "opera" a las categorías de nuestra base de datos.</span><span class="sxs-lookup"><span data-stu-id="57102-105">Suppose we want to add the genre "Opera" to the categories in our database?</span></span> <span data-ttu-id="57102-106">En esta sección, usaremos la interfaz de usuario de jQuery para agregar un cuadro de diálogo que se puede usar para agregar una nueva categoría.</span><span class="sxs-lookup"><span data-stu-id="57102-106">In this section, we will use jQuery UI to add a dialog box we can use to add a new category.</span></span> <span data-ttu-id="57102-107">En la imagen siguiente se muestra cómo se presentará la interfaz de usuario en el explorador.</span><span class="sxs-lookup"><span data-stu-id="57102-107">The image below shows how the UI will present in the browser.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

<span data-ttu-id="57102-108">Cuando un usuario selecciona el vínculo **Agregar nuevo género** , un cuadro de diálogo emergente solicita al usuario un nuevo nombre de género (y, opcionalmente, una descripción).</span><span class="sxs-lookup"><span data-stu-id="57102-108">When a user selects the **Add New Genre** link, a pop-up dialog box prompts the user for a new genre name (and optionally a description).</span></span> <span data-ttu-id="57102-109">En la imagen siguiente se muestra el cuadro de diálogo emergente **Agregar género** .</span><span class="sxs-lookup"><span data-stu-id="57102-109">The image below show the **Add Genre** pop-up dialog.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

<span data-ttu-id="57102-110">Cuando se escribe un nuevo nombre de género y se inserta el botón **Guardar** , ocurre lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="57102-110">When a new genre name is entered and the **Save** button is pushed, the following happens:</span></span>

1. <span data-ttu-id="57102-111">Una llamada AJAX envía los datos al método Create del controlador Genre, que guarda el nuevo género en la base de datos y devuelve la nueva información del género (nombre y identificador del género) como JSON.</span><span class="sxs-lookup"><span data-stu-id="57102-111">An AJAX call posts the data to the Create method of the Genre Controller, which saves the new genre to the database and returns the new genre information (genre name and ID) as JSON.</span></span>
2. <span data-ttu-id="57102-112">JavaScript agrega los nuevos datos de género a la lista de selección.</span><span class="sxs-lookup"><span data-stu-id="57102-112">JavaScript adds the new genre data to the select list.</span></span>
3. <span data-ttu-id="57102-113">JavaScript convierte el nuevo género en el elemento seleccionado.</span><span class="sxs-lookup"><span data-stu-id="57102-113">JavaScript makes the new genre the selected item.</span></span>

   <span data-ttu-id="57102-114">En la imagen siguiente, **opera** se agregó a la base de datos y se seleccionó en la lista desplegable **género** .</span><span class="sxs-lookup"><span data-stu-id="57102-114">In the image below, **Opera** was added to the database and selected in the **Genre** drop down list.</span></span> 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

<span data-ttu-id="57102-115">Abra el archivo *Views\StoreManager\Create.cshtml* y reemplace el marcado de género con el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="57102-115">Open the *Views\StoreManager\Create.cshtml* file and replace the genre markup with the following the following code:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

<span data-ttu-id="57102-116">La vista parcial de `_ChooseGenre` contendrá toda la lógica para enlazar el código JavaScript y jQuery que se usa para implementar la característica agregar nuevo género.</span><span class="sxs-lookup"><span data-stu-id="57102-116">The `_ChooseGenre` partial view will contain all the logic to hook up the JavaScript and jQuery used to implement the add new genre feature.</span></span> <span data-ttu-id="57102-117">Una vez completado el código, será fácil hacer lo mismo con la interfaz de usuario del intérprete.</span><span class="sxs-lookup"><span data-stu-id="57102-117">Once we have completed the code it will be simple to do the same with the artist UI.</span></span>

<span data-ttu-id="57102-118">En Explorador de soluciones, haga clic con el botón secundario en la carpeta *Views\StoreManager* , seleccione **Agregar**y, a continuación, **Ver**.</span><span class="sxs-lookup"><span data-stu-id="57102-118">In Solution Explorer, right click the *Views\StoreManager* folder and select **Add**, then **View**.</span></span> <span data-ttu-id="57102-119">En la entrada **nombre de vista** , escriba `_ChooseGenre`, a continuación, seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="57102-119">In the **View name** input, enter `_ChooseGenre` then select **Add**.</span></span> <span data-ttu-id="57102-120">Reemplace el marcado en el archivo *Views\StoreManager\\_ChooseGenre. cshtml* con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="57102-120">Replace the markup in the *Views\StoreManager\\_ChooseGenre.cshtml* file with the following:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

<span data-ttu-id="57102-121">La primera línea declara que pasamos un `Album` como modelo, exactamente la misma instrucción del modelo que se encuentra en la vista crear.</span><span class="sxs-lookup"><span data-stu-id="57102-121">The first line declares that we are passing in an `Album` as our model, exactly the same model statement found in the Create view.</span></span> <span data-ttu-id="57102-122">Las siguientes líneas son el marcado de la aplicación auxiliar de **etiqueta** .</span><span class="sxs-lookup"><span data-stu-id="57102-122">The next few lines are the **Label** helper markup.</span></span> <span data-ttu-id="57102-123">La siguiente línea es la llamada de la aplicación auxiliar de **DropDownList** , exactamente igual que en la vista de creación original.</span><span class="sxs-lookup"><span data-stu-id="57102-123">The next line is the **DropDownList** helper call, exactly the same as in the original Create view.</span></span> <span data-ttu-id="57102-124">La línea siguiente agrega un vínculo con el nombre `Add New Genre`y lo estilos como un botón.</span><span class="sxs-lookup"><span data-stu-id="57102-124">The next line adds a link with the name `Add New Genre`, and styles it like a button.</span></span> <span data-ttu-id="57102-125">La línea que contiene `ValidationMessageFor` se copia directamente desde la vista crear.</span><span class="sxs-lookup"><span data-stu-id="57102-125">The line containing `ValidationMessageFor` is copied directly from the Create view.</span></span> <span data-ttu-id="57102-126">Las siguientes líneas:</span><span class="sxs-lookup"><span data-stu-id="57102-126">The following lines:</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

<span data-ttu-id="57102-127">crea un div oculto, con el identificador de `genreDialog`.</span><span class="sxs-lookup"><span data-stu-id="57102-127">creates a hidden div, with the ID of `genreDialog`.</span></span> <span data-ttu-id="57102-128">Usaremos jQuery para enlazar el cuadro de diálogo **Agregar género** con el identificador `genreDialog` en este div.</span><span class="sxs-lookup"><span data-stu-id="57102-128">We will use jQuery to hook up our **Add Genre** dialog box with the ID `genreDialog` in this div.</span></span> <span data-ttu-id="57102-129">Las dos últimas etiquetas de script contienen vínculos a los archivos de JavaScript que se utilizarán para implementar la característica agregar nuevo género.</span><span class="sxs-lookup"><span data-stu-id="57102-129">The last two script tags contain links to the JavaScript files we will use to implement the add new genre feature.</span></span> <span data-ttu-id="57102-130">El archivo */scripts/chooseGenre.js* se proporciona en el proyecto. lo examinaremos más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="57102-130">The */Scripts/chooseGenre.js* file is provided for you in the project, we will examine it later in the tutorial.</span></span>

<span data-ttu-id="57102-131">Ejecute la aplicación y haga clic en el botón **Agregar nuevo género** .</span><span class="sxs-lookup"><span data-stu-id="57102-131">Run the application and click on the **Add New Genre** button.</span></span> <span data-ttu-id="57102-132">En el cuadro de diálogo **Agregar género** , escriba **opera** en el cuadro de entrada **nombre** .</span><span class="sxs-lookup"><span data-stu-id="57102-132">In the **Add Genre** dialog box, enter **Opera** in the **Name** input box.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

<span data-ttu-id="57102-133">Haga clic en el botón **Save** (Guardar).</span><span class="sxs-lookup"><span data-stu-id="57102-133">Click the **Save** button.</span></span> <span data-ttu-id="57102-134">Una llamada AJAX crea la categoría opera y, a continuación, rellena la lista desplegable con opera y establece opera como el género seleccionado.</span><span class="sxs-lookup"><span data-stu-id="57102-134">An AJAX call creates the Opera category and then populates the dropdown list with Opera, and sets Opera as the selected genre.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

<span data-ttu-id="57102-135">Escriba un intérprete, un título y un precio y, a continuación, seleccione el botón **crear** .</span><span class="sxs-lookup"><span data-stu-id="57102-135">Enter an artist, title and price, then select the **Create** button.</span></span> <span data-ttu-id="57102-136">Si especifica un precio inferior a $8,99, el nuevo álbum aparecerá en la parte superior de la vista de índice.</span><span class="sxs-lookup"><span data-stu-id="57102-136">If you enter a price less than $8.99, the new album will appear at the top of the Index view.</span></span> <span data-ttu-id="57102-137">Compruebe que la nueva entrada de álbum se guardó en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="57102-137">Verify the new album entry was saved in the database.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

<span data-ttu-id="57102-138">Intente crear un nuevo género con una sola letra.</span><span class="sxs-lookup"><span data-stu-id="57102-138">Try creating a new genre with only one letter.</span></span> <span data-ttu-id="57102-139">El siguiente código del archivo *Models\Genre.CS* establece la longitud mínima y máxima del nombre del género.</span><span class="sxs-lookup"><span data-stu-id="57102-139">The following code in the *Models\Genre.cs* file sets the minimum and maximum length of the genre name.</span></span>

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

<span data-ttu-id="57102-140">Informes de validación del lado cliente debe especificar una cadena de entre 2 y 20 caracteres.</span><span class="sxs-lookup"><span data-stu-id="57102-140">Client side validation reports you must enter a string between 2 and 20 characters.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a><span data-ttu-id="57102-141">Examinar cómo se agrega un nuevo género a la base de datos y la lista de selección.</span><span class="sxs-lookup"><span data-stu-id="57102-141">Examining How a New Genre is Added to the Database and the Select List.</span></span>

<span data-ttu-id="57102-142">Abra el archivo *Scripts\chooseGenre.js* y examine el código.</span><span class="sxs-lookup"><span data-stu-id="57102-142">Open the *Scripts\chooseGenre.js* file and examine the code.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

<span data-ttu-id="57102-143">La segunda línea usa el identificador `genreDialog` para crear un cuadro de diálogo en la etiqueta div en el archivo *Views\StoreManager\\_ChooseGenre. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="57102-143">The second line uses the ID `genreDialog` to create a dialog box on the div tag in the *Views\StoreManager\\_ChooseGenre.cshtml* file.</span></span> <span data-ttu-id="57102-144">La mayoría de los parámetros con nombre son autoexplicativos.</span><span class="sxs-lookup"><span data-stu-id="57102-144">Most of the named parameters are self explanatory.</span></span> <span data-ttu-id="57102-145">El parámetro `autoOpen` está establecido en false, al seleccionar el botón **crear género** se abrirá el cuadro de diálogo explícitamente (esto se describe aquí).</span><span class="sxs-lookup"><span data-stu-id="57102-145">The `autoOpen` parameter is set to false, selecting the **Create Genre** button will open the dialogue explicitly (this is described latter on).</span></span> <span data-ttu-id="57102-146">El cuadro de diálogo tiene dos botones: **Guardar** y **Cancelar**.</span><span class="sxs-lookup"><span data-stu-id="57102-146">The dialog has two buttons, **Save** and **Cancel**.</span></span> <span data-ttu-id="57102-147">El botón **Cancelar** cierra el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="57102-147">The **Cancel** button closes the dialog.</span></span> <span data-ttu-id="57102-148">En el código siguiente se muestra la función del botón **Guardar** .</span><span class="sxs-lookup"><span data-stu-id="57102-148">The following code shows the **Save** button function.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

<span data-ttu-id="57102-149">El `var createGenreForm` se selecciona en el ID. de `createGenreForm`.</span><span class="sxs-lookup"><span data-stu-id="57102-149">The `var createGenreForm` is selected from the `createGenreForm` ID.</span></span> <span data-ttu-id="57102-150">El identificador de `createGenreForm` se estableció en el siguiente código que se encuentra en el archivo *Views\Genre\\_CreateGenre. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="57102-150">The `createGenreForm` ID was set in the following code found in the *Views\Genre\\_CreateGenre.cshtml* file.</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

<span data-ttu-id="57102-151">La sobrecarga de la aplicación auxiliar [HTML. BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) utilizada en el archivo *Views\Genre\\_CreateGenre. cshtml* genera HTML con un atributo de acción que contiene la dirección URL para enviar el formulario.</span><span class="sxs-lookup"><span data-stu-id="57102-151">The [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) helper overload used in the *Views\Genre\\_CreateGenre.cshtml* file generates HTML with an action attribute containing the URL to submit the form.</span></span> <span data-ttu-id="57102-152">Puede verlo mostrando la página crear álbum en un explorador y seleccionando Mostrar origen en el explorador.</span><span class="sxs-lookup"><span data-stu-id="57102-152">You can see this by displaying the create album page in a browser and selecting show source in the browser.</span></span> <span data-ttu-id="57102-153">El marcado siguiente muestra el HTML generado que contiene la etiqueta de formulario.</span><span class="sxs-lookup"><span data-stu-id="57102-153">The following markup shows the generated HTML containing the form tag.</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

<span data-ttu-id="57102-154">La línea de `$.post` de jQuery realiza una llamada AJAX al atributo Action (`/StoreManager/Create`) y pasa los datos del cuadro de diálogo **Create Genre** .</span><span class="sxs-lookup"><span data-stu-id="57102-154">The jQuery `$.post` line makes an AJAX call to the action attribute (`/StoreManager/Create`) and passes in the data from the **Create Genre** dialog box.</span></span> <span data-ttu-id="57102-155">Los datos se componen del nombre del nuevo género y una descripción opcional.</span><span class="sxs-lookup"><span data-stu-id="57102-155">The data consists of the name for the new genre and an optional description.</span></span> <span data-ttu-id="57102-156">Si la llamada AJAX se realiza correctamente, el nuevo nombre y valor de género se agregan al marcado de selección y el nuevo género se establece en el valor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="57102-156">If the AJAX call is successful, the new genre name and value are added to the Select markup, and the new genre is set to the selected value.</span></span> <span data-ttu-id="57102-157">Dado que se trata de un marcado generado dinámicamente, no se puede ver la nueva opción seleccionar viendo el origen en el explorador.</span><span class="sxs-lookup"><span data-stu-id="57102-157">Because this is dynamically generated markup, you can't see the new select option by viewing the source in the browser.</span></span> <span data-ttu-id="57102-158">Puede ver el nuevo HTML con las herramientas de desarrollo F12 de IE 9.</span><span class="sxs-lookup"><span data-stu-id="57102-158">You can see the new HTML with the IE 9 F12 developer tools.</span></span> <span data-ttu-id="57102-159">Para ver la nueva opción seleccionar, en Internet Explorer 9, presione la tecla F12 para iniciar las herramientas de desarrollo F12.</span><span class="sxs-lookup"><span data-stu-id="57102-159">To view the new select option, in Internet Explorer 9, hit the F12 key to start the F12 developer tools.</span></span> <span data-ttu-id="57102-160">Navegue hasta la página crear y agregue un nuevo género para que el nuevo género esté seleccionado en la lista de selección de género.</span><span class="sxs-lookup"><span data-stu-id="57102-160">Navigate to the Create page and add a new genre so the new genre is selected in the genre select list.</span></span> <span data-ttu-id="57102-161">En las herramientas de desarrollo F12:</span><span class="sxs-lookup"><span data-stu-id="57102-161">In the F12 developer tools:</span></span>

1. <span data-ttu-id="57102-162">Seleccione la pestaña HTML.</span><span class="sxs-lookup"><span data-stu-id="57102-162">Select the HTML tab.</span></span>
2. <span data-ttu-id="57102-163">Presione el icono de actualización.</span><span class="sxs-lookup"><span data-stu-id="57102-163">Hit the refresh icon.</span></span>  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. <span data-ttu-id="57102-164">En el cuadro de búsqueda, escriba GenreID.</span><span class="sxs-lookup"><span data-stu-id="57102-164">In the search box, enter GenreID.</span></span>
4. <span data-ttu-id="57102-165">Con el siguiente icono,</span><span class="sxs-lookup"><span data-stu-id="57102-165">Using the next icon,</span></span>   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   <span data-ttu-id="57102-166">vaya a la siguiente etiqueta Select:</span><span class="sxs-lookup"><span data-stu-id="57102-166">navigate to the following select tag:</span></span>

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. <span data-ttu-id="57102-167">Expanda el último valor de opción.</span><span class="sxs-lookup"><span data-stu-id="57102-167">Expand the last option value.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

<span data-ttu-id="57102-168">El siguiente código del archivo *Scripts\chooseGenre.js* muestra cómo se conecta el botón **Agregar nuevo género** al evento click y cómo se crea el cuadro de diálogo **Agregar nuevo género** .</span><span class="sxs-lookup"><span data-stu-id="57102-168">The following code in the *Scripts\chooseGenre.js* file shows the how the **Add New Genre** button gets connected to the click event, and how the **Add New Genre** dialog box is created.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

<span data-ttu-id="57102-169">La primera línea crea una función de clic asociada al botón **Agregar nuevo género** .</span><span class="sxs-lookup"><span data-stu-id="57102-169">The first line creates a click function attached to the **Add New Genre** button.</span></span> <span data-ttu-id="57102-170">El siguiente marcado del archivo Views\StoreManager\\_ChooseGenre. cshtml muestra cómo se crea el botón **Agregar nuevo género** :</span><span class="sxs-lookup"><span data-stu-id="57102-170">The following markup from the Views\StoreManager\\_ChooseGenre.cshtml file shows how the **Add New Genre** button is created:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

<span data-ttu-id="57102-171">El método Load crea y abre el cuadro de diálogo Agregar género y llama al método jQuery `parse`, por lo que la validación del cliente se produce en los datos especificados en el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="57102-171">The load method creates and opens the Add Genre dialog and calls the jQuery `parse` method so client validation occurs on data entered in the dialog.</span></span>

<span data-ttu-id="57102-172">En esta sección, ha aprendido a crear un cuadro de diálogo que se puede usar para agregar nuevos datos de categoría a una lista de selección.</span><span class="sxs-lookup"><span data-stu-id="57102-172">In this section you have learned how to create a dialog that can be used to add new category data to a select list.</span></span> <span data-ttu-id="57102-173">Puede seguir el mismo procedimiento para crear una interfaz de usuario para agregar un nuevo artista a la lista de selección de intérprete.</span><span class="sxs-lookup"><span data-stu-id="57102-173">You can follow the same procedure to create UI to add a new artist to the artist select list.</span></span> <span data-ttu-id="57102-174">En este tutorial se ha proporcionado información general sobre cómo trabajar con el **DropDownList**de la aplicación auxiliar HTML MVC de ASP.net.</span><span class="sxs-lookup"><span data-stu-id="57102-174">This tutorial has given an overview of working with the ASP.NET MVC HTML helper **DropDownList**.</span></span> <span data-ttu-id="57102-175">Para obtener información adicional sobre cómo trabajar con **DropDownList**, vea la sección referencias adicionales.</span><span class="sxs-lookup"><span data-stu-id="57102-175">For additional information on working with the **DropDownList**, see the addition references section below.</span></span> <span data-ttu-id="57102-176">Háganoslo saber si este tutorial ha sido útil.</span><span class="sxs-lookup"><span data-stu-id="57102-176">Please let us know if this tutorial has been helpful.</span></span>

<span data-ttu-id="57102-177">Rick.Anderson[at]Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="57102-177">Rick.Anderson[at]Microsoft.com</span></span>

### <a name="additional-references"></a><span data-ttu-id="57102-178">Referencias adicionales</span><span class="sxs-lookup"><span data-stu-id="57102-178">Additional References</span></span>

- <span data-ttu-id="57102-179">[ASP.NET MVC: listas desplegables en cascada tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) de [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span><span class="sxs-lookup"><span data-stu-id="57102-179">[ASP.NET MVC–Cascading Dropdown Lists Tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) by [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span></span>
- <span data-ttu-id="57102-180">[Elegido](https://harvesthq.github.com/chosen/) Un complemento de JavaScript que admite la selección múltiple y el filtrado.</span><span class="sxs-lookup"><span data-stu-id="57102-180">[Chosen](https://harvesthq.github.com/chosen/) A JavaScript plugin that support multi-select and filtering.</span></span>

### <a name="contributors"></a><span data-ttu-id="57102-181">Colaboradores</span><span class="sxs-lookup"><span data-stu-id="57102-181">Contributors</span></span>

- [<span data-ttu-id="57102-182">Radu Enuca</span><span class="sxs-lookup"><span data-stu-id="57102-182">Radu Enuca</span></span>](https://weblogs.asp.net/raduenuca/default.aspx)
- <span data-ttu-id="57102-183">Jean-Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="57102-183">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="57102-184">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="57102-184">Brad Wilson</span></span>](http://bradwilson.typepad.com/)

### <a name="reviewers"></a><span data-ttu-id="57102-185">Revisores</span><span class="sxs-lookup"><span data-stu-id="57102-185">Reviewers</span></span>

- <span data-ttu-id="57102-186">Jean-Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="57102-186">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="57102-187">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="57102-187">Brad Wilson</span></span>](http://bradwilson.typepad.com/)
- <span data-ttu-id="57102-188">Mike Pope</span><span class="sxs-lookup"><span data-stu-id="57102-188">Mike Pope</span></span>
- <span data-ttu-id="57102-189">Tom Dykstra</span><span class="sxs-lookup"><span data-stu-id="57102-189">Tom Dykstra</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="57102-190">Anterior</span><span class="sxs-lookup"><span data-stu-id="57102-190">Previous</span></span>](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
