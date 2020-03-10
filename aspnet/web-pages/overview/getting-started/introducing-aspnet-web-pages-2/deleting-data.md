---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: Presentación de ASP.NET Web Pages-eliminación de datos de base de datos | Microsoft Docs
author: Rick-Anderson
description: En este tutorial se muestra cómo eliminar una entrada de base de datos individual. Se supone que ha completado la serie a través de la actualización de los datos de la base de datos en ASP.NET Web PA...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: c8620fc1abc61d514bdc039c66f7a84e67e89abe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78510463"
---
# <a name="introducing-aspnet-web-pages---deleting-database-data"></a><span data-ttu-id="d3345-104">Introducción a la eliminación de datos de la base de datos de ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="d3345-104">Introducing ASP.NET Web Pages - Deleting Database Data</span></span>

<span data-ttu-id="d3345-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d3345-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d3345-106">En este tutorial se muestra cómo eliminar una entrada de base de datos individual.</span><span class="sxs-lookup"><span data-stu-id="d3345-106">This tutorial shows you how to delete an individual database entry.</span></span> <span data-ttu-id="d3345-107">Se supone que ha completado las series a través [de la actualización de los datos de la base de datos en ASP.NET Web pages](updating-data.md).</span><span class="sxs-lookup"><span data-stu-id="d3345-107">It assumes you have completed the series through [Updating Database Data in ASP.NET Web Pages](updating-data.md).</span></span>
> 
> <span data-ttu-id="d3345-108">Temas que se abordarán:</span><span class="sxs-lookup"><span data-stu-id="d3345-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="d3345-109">Cómo seleccionar un registro individual de una lista de registros.</span><span class="sxs-lookup"><span data-stu-id="d3345-109">How to select an individual record from a listing of records.</span></span>
> - <span data-ttu-id="d3345-110">Cómo eliminar un solo registro de una base de datos.</span><span class="sxs-lookup"><span data-stu-id="d3345-110">How to delete a single record from a database.</span></span>
> - <span data-ttu-id="d3345-111">Cómo comprobar que se hizo clic en un botón específico en un formulario.</span><span class="sxs-lookup"><span data-stu-id="d3345-111">How to check that a specific button was clicked in a form.</span></span>
>   
> 
> <span data-ttu-id="d3345-112">Características y tecnologías descritas:</span><span class="sxs-lookup"><span data-stu-id="d3345-112">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="d3345-113">Aplicación auxiliar de `WebGrid`.</span><span class="sxs-lookup"><span data-stu-id="d3345-113">The `WebGrid` helper.</span></span>
> - <span data-ttu-id="d3345-114">Comando de SQL `Delete`.</span><span class="sxs-lookup"><span data-stu-id="d3345-114">The SQL `Delete` command.</span></span>
> - <span data-ttu-id="d3345-115">El método `Database.Execute` para ejecutar un comando de `Delete` SQL.</span><span class="sxs-lookup"><span data-stu-id="d3345-115">The `Database.Execute` method to run a SQL `Delete` command.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="d3345-116">Lo que creará</span><span class="sxs-lookup"><span data-stu-id="d3345-116">What You'll Build</span></span>

<span data-ttu-id="d3345-117">En el tutorial anterior, aprendió a actualizar un registro de base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="d3345-117">In the previous tutorial, you learned how to update an existing database record.</span></span> <span data-ttu-id="d3345-118">Este tutorial es similar, salvo que en lugar de actualizar el registro, lo eliminará.</span><span class="sxs-lookup"><span data-stu-id="d3345-118">This tutorial is similar, except that instead of updating the record, you'll delete it.</span></span> <span data-ttu-id="d3345-119">Los procesos son muy parecidos, salvo que la eliminación es más simple, por lo que este tutorial será breve.</span><span class="sxs-lookup"><span data-stu-id="d3345-119">The processes are much the same, except that deleting is simpler, so this tutorial will be short.</span></span>

<span data-ttu-id="d3345-120">En la página *películas* , actualizará la aplicación auxiliar de `WebGrid` para que muestre un vínculo **eliminar** junto a cada película que acompañe al vínculo de **edición** que agregó anteriormente.</span><span class="sxs-lookup"><span data-stu-id="d3345-120">In the *Movies* page, you'll update the `WebGrid` helper so that it displays a **Delete** link next to each movie to accompany the **Edit** link you added earlier.</span></span>

![Página de películas que muestra un vínculo de eliminación para cada película](deleting-data/_static/image1.png)

<span data-ttu-id="d3345-122">Al igual que con la edición, al hacer clic en el vínculo **eliminar** , se le remite a una página diferente, donde la información de la película ya está en un formulario:</span><span class="sxs-lookup"><span data-stu-id="d3345-122">As with editing, when you click the **Delete** link, it takes you to a different page, where the movie information is already in a form:</span></span>

![Página eliminar película con una película mostrada](deleting-data/_static/image2.png)

<span data-ttu-id="d3345-124">Después, puede hacer clic en el botón para eliminar el registro de forma permanente.</span><span class="sxs-lookup"><span data-stu-id="d3345-124">You can then click the button to delete the record permanently.</span></span>

## <a name="adding-a-delete-link-to-the-movie-listing"></a><span data-ttu-id="d3345-125">Agregar un vínculo de eliminación a la lista de películas</span><span class="sxs-lookup"><span data-stu-id="d3345-125">Adding a Delete Link to the Movie Listing</span></span>

<span data-ttu-id="d3345-126">Empezará agregando un vínculo de **eliminación** a la aplicación auxiliar de `WebGrid`.</span><span class="sxs-lookup"><span data-stu-id="d3345-126">You'll start by adding a **Delete** link to the `WebGrid` helper.</span></span> <span data-ttu-id="d3345-127">Este vínculo es similar al vínculo de **edición** que ha agregado en un tutorial anterior.</span><span class="sxs-lookup"><span data-stu-id="d3345-127">This link is similar to the **Edit** link you added in a previous tutorial.</span></span>

<span data-ttu-id="d3345-128">Abra el archivo *movies. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="d3345-128">Open the *Movies.cshtml* file.</span></span>

<span data-ttu-id="d3345-129">Cambie el marcado de `WebGrid` en el cuerpo de la página agregando una columna.</span><span class="sxs-lookup"><span data-stu-id="d3345-129">Change the `WebGrid` markup in the body of the page by adding a column.</span></span> <span data-ttu-id="d3345-130">Este es el marcado modificado:</span><span class="sxs-lookup"><span data-stu-id="d3345-130">Here's the modified markup:</span></span>

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

<span data-ttu-id="d3345-131">La nueva columna es esta:</span><span class="sxs-lookup"><span data-stu-id="d3345-131">The new column is this one:</span></span>

[!code-html[Main](deleting-data/samples/sample2.html)]

<span data-ttu-id="d3345-132">La forma en que se configura la cuadrícula, la columna **Editar** está más a la izquierda en la cuadrícula y la columna **eliminar** está en la derecha.</span><span class="sxs-lookup"><span data-stu-id="d3345-132">The way the grid is configured, the **Edit** column is leftmost in the grid and the **Delete** column is rightmost.</span></span> <span data-ttu-id="d3345-133">(Ahora hay una coma después de la columna de `Year`, en caso de que no lo advierta). No hay nada especial sobre el lugar en el que se colocan estas columnas de vínculo y puede colocarlas fácilmente una junto a la otra.</span><span class="sxs-lookup"><span data-stu-id="d3345-133">(There's a comma after the `Year` column now, in case you didn't notice that.) There's nothing special about where these link columns go, and you could as easily put them next to each other.</span></span> <span data-ttu-id="d3345-134">En este caso, son independientes para que sean más difíciles de combinar.</span><span class="sxs-lookup"><span data-stu-id="d3345-134">In this case, they're separate to make them harder to get mixed up.</span></span>

![Página de películas con vínculos de edición y detalles marcados para mostrar que no están junto a otros](deleting-data/_static/image3.png)

<span data-ttu-id="d3345-136">La nueva columna muestra un vínculo (elemento`<a>`) cuyo texto dice "eliminar".</span><span class="sxs-lookup"><span data-stu-id="d3345-136">The new column shows a link (`<a>` element) whose text says "Delete".</span></span> <span data-ttu-id="d3345-137">El destino del vínculo (su `href` atributo) es un código que se resuelve en última instancia en algo como esta dirección URL, con el valor `id` distinto para cada película:</span><span class="sxs-lookup"><span data-stu-id="d3345-137">The target of the link (its `href` attribute) is code that ultimately resolves to something like this URL, with the `id` value different for each movie:</span></span>

[!code-css[Main](deleting-data/samples/sample3.css)]

<span data-ttu-id="d3345-138">Este vínculo invocará una página denominada *DeleteMovie* y la pasará al identificador de la película que ha seleccionado.</span><span class="sxs-lookup"><span data-stu-id="d3345-138">This link will invoke a page named *DeleteMovie* and pass it the ID of the movie you've selected.</span></span>

<span data-ttu-id="d3345-139">En este tutorial no se detallará cómo se construye este vínculo, ya que es casi idéntico al vínculo de **edición** del tutorial anterior ([actualización de datos de base de datos en ASP.NET Web pages](updating-data.md)).</span><span class="sxs-lookup"><span data-stu-id="d3345-139">This tutorial won't go into detail about how this link is constructed, because it's almost identical to the **Edit** link from the previous tutorial ([Updating Database Data in ASP.NET Web Pages](updating-data.md)).</span></span>

## <a name="creating-the-delete-page"></a><span data-ttu-id="d3345-140">Creación de la página eliminar</span><span class="sxs-lookup"><span data-stu-id="d3345-140">Creating the Delete Page</span></span>

<span data-ttu-id="d3345-141">Ahora puede crear la página que será el destino del vínculo de **eliminación** en la cuadrícula.</span><span class="sxs-lookup"><span data-stu-id="d3345-141">Now you can create the page that will be the target for the **Delete** link in the grid.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="d3345-142">**Importante** La técnica de seleccionar primero un registro que se va a eliminar y, a continuación, usar una página y un botón independientes para confirmar el proceso es extremadamente importante para la seguridad.</span><span class="sxs-lookup"><span data-stu-id="d3345-142">**Important** The technique of first selecting a record to delete and then using a separate page and button to confirm the process is extremely important for security.</span></span> <span data-ttu-id="d3345-143">Como ha leído en los tutoriales anteriores, realizar *cualquier* tipo de cambio en el sitio Web *siempre* debe realizarse mediante un formulario &mdash; es decir, mediante una operación http post.</span><span class="sxs-lookup"><span data-stu-id="d3345-143">As you've read in previous tutorials, making *any* sort of change to your website should *always* be done using a form &mdash; that is, using an HTTP POST operation.</span></span> <span data-ttu-id="d3345-144">Si ha hecho posible cambiar el sitio simplemente haciendo clic en un vínculo (es decir, mediante una operación GET), los usuarios podrían realizar solicitudes simples al sitio y eliminar los datos.</span><span class="sxs-lookup"><span data-stu-id="d3345-144">If you made it possible to change the site just by clicking a link (that is, using a GET operation), people could make simple requests to your site and delete your data.</span></span> <span data-ttu-id="d3345-145">Incluso un rastreador del motor de búsqueda que está indizando su sitio podría eliminar accidentalmente los datos simplemente mediante los vínculos siguientes.</span><span class="sxs-lookup"><span data-stu-id="d3345-145">Even a search-engine crawler that's indexing your site could inadvertently delete data just by following links.</span></span>
> 
> <span data-ttu-id="d3345-146">Cuando la aplicación permite a los usuarios cambiar un registro, tiene que presentar el registro al usuario para su edición.</span><span class="sxs-lookup"><span data-stu-id="d3345-146">When your app lets people change a record, you have to present the record to the user for editing anyway.</span></span> <span data-ttu-id="d3345-147">Pero podría verse tentado a omitir este paso para eliminar un registro.</span><span class="sxs-lookup"><span data-stu-id="d3345-147">But you might be tempted to skip this step for deleting a record.</span></span> <span data-ttu-id="d3345-148">Sin embargo, no omita este paso.</span><span class="sxs-lookup"><span data-stu-id="d3345-148">Don't skip that step, though.</span></span> <span data-ttu-id="d3345-149">(También resulta útil para los usuarios ver el registro y confirmar que están eliminando el registro que pretendía).</span><span class="sxs-lookup"><span data-stu-id="d3345-149">(It's also helpful for users to see the record and confirm that they're deleting the record that they intended.)</span></span>
> 
> <span data-ttu-id="d3345-150">En un conjunto de tutoriales posterior, verá cómo agregar la funcionalidad de inicio de sesión para que un usuario tenga que iniciar sesión antes de eliminar un registro.</span><span class="sxs-lookup"><span data-stu-id="d3345-150">In a subsequent tutorial set, you'll see how to add login functionality so a user would have to log in before deleting a record.</span></span>

<span data-ttu-id="d3345-151">Cree una página denominada *DeleteMovie. cshtml* y reemplace lo que hay en el archivo con el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="d3345-151">Create a page named *DeleteMovie.cshtml* and replace what's in the file with the following markup:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

<span data-ttu-id="d3345-152">Este marcado es como las páginas *EditMovie* , salvo que en lugar de usar cuadros de texto (`<input type="text">`), el marcado incluye elementos `<span>`.</span><span class="sxs-lookup"><span data-stu-id="d3345-152">This markup is like the *EditMovie* pages, except that instead of using text boxes (`<input type="text">`), the markup includes `<span>` elements.</span></span> <span data-ttu-id="d3345-153">No hay nada que editar.</span><span class="sxs-lookup"><span data-stu-id="d3345-153">There's nothing here to edit.</span></span> <span data-ttu-id="d3345-154">Lo único que tiene que hacer es mostrar los detalles de la película para que los usuarios puedan asegurarse de que están eliminando la película adecuada.</span><span class="sxs-lookup"><span data-stu-id="d3345-154">All you have to do is display the movie details so that users can make sure that they're deleting the right movie.</span></span>

<span data-ttu-id="d3345-155">El marcado ya contiene un vínculo que permite al usuario volver a la página de lista de películas.</span><span class="sxs-lookup"><span data-stu-id="d3345-155">The markup already contains a link that lets the user return to the movie listing page.</span></span>

<span data-ttu-id="d3345-156">Como en la página *EditMovie* , el identificador de la película seleccionada se almacena en un campo oculto.</span><span class="sxs-lookup"><span data-stu-id="d3345-156">As in the *EditMovie* page, the ID of the selected movie is stored in a hidden field.</span></span> <span data-ttu-id="d3345-157">(Se pasa a la página en primer lugar como valor de cadena de consulta). Hay una llamada `Html.ValidationSummary` que mostrará los errores de validación.</span><span class="sxs-lookup"><span data-stu-id="d3345-157">(It's passed into the page in the first place as a query string value.) There's an `Html.ValidationSummary` call that will display validation errors.</span></span> <span data-ttu-id="d3345-158">En este caso, el error puede ser que no se ha pasado ningún ID. de película a la página o que el ID. de la película no es válido.</span><span class="sxs-lookup"><span data-stu-id="d3345-158">In this case, the error might be that no movie ID was passed to the page or that the movie ID is invalid.</span></span> <span data-ttu-id="d3345-159">Esta situación puede producirse si alguien ejecutó esta página sin seleccionar primero una película en la página *películas* .</span><span class="sxs-lookup"><span data-stu-id="d3345-159">This situation could occur if someone ran this page without first selecting a movie in the *Movies* page.</span></span>

<span data-ttu-id="d3345-160">El título del botón es **eliminar película**y su atributo de nombre está establecido en `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="d3345-160">The button caption is **Delete Movie**, and its name attribute is set to `buttonDelete`.</span></span> <span data-ttu-id="d3345-161">El atributo `name` se usará en el código para identificar el botón que envió el formulario.</span><span class="sxs-lookup"><span data-stu-id="d3345-161">The `name` attribute will be used in the code to identify the button that submitted the form.</span></span>

<span data-ttu-id="d3345-162">Tendrá que escribir código en 1) leer los detalles de la película cuando se muestre la página por primera vez y 2) eliminar realmente la película cuando el usuario haga clic en el botón.</span><span class="sxs-lookup"><span data-stu-id="d3345-162">You'll have to write code to 1) read the movie details when the page is first displayed and 2) actually delete the movie when the user clicks the button.</span></span>

## <a name="adding-code-to-read-a-single-movie"></a><span data-ttu-id="d3345-163">Agregar código para leer una sola película</span><span class="sxs-lookup"><span data-stu-id="d3345-163">Adding Code to Read a Single Movie</span></span>

<span data-ttu-id="d3345-164">En la parte superior de la página *DeleteMovie. cshtml* , agregue el siguiente bloque de código:</span><span class="sxs-lookup"><span data-stu-id="d3345-164">At the top of the *DeleteMovie.cshtml* page, add the following code block:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

<span data-ttu-id="d3345-165">Este marcado es el mismo que el código correspondiente en la página *EditMovie* .</span><span class="sxs-lookup"><span data-stu-id="d3345-165">This markup is the same as the corresponding code in the *EditMovie* page.</span></span> <span data-ttu-id="d3345-166">Obtiene el ID. de la película de la cadena de consulta y usa el identificador para leer un registro de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d3345-166">It gets the movie ID out of the query string and uses the ID to read a record from the database.</span></span> <span data-ttu-id="d3345-167">El código incluye la prueba de validación (`IsInt()` y `row != null`) para asegurarse de que el ID. de película que se pasa a la página es válido.</span><span class="sxs-lookup"><span data-stu-id="d3345-167">The code includes the validation test (`IsInt()` and `row != null`) to make sure that the movie ID being passed to the page is valid.</span></span>

<span data-ttu-id="d3345-168">Recuerde que este código solo debe ejecutarse la primera vez que se ejecute la página.</span><span class="sxs-lookup"><span data-stu-id="d3345-168">Remember that this code should only run the first time the page runs.</span></span> <span data-ttu-id="d3345-169">No desea volver a leer el registro de la película en la base de datos cuando el usuario hace clic en el botón **eliminar película** .</span><span class="sxs-lookup"><span data-stu-id="d3345-169">You don't want to re-read the movie record from the database when the user clicks the **Delete Movie** button.</span></span> <span data-ttu-id="d3345-170">Por lo tanto, el código para leer la película se encuentra dentro de una prueba que indica `if(!IsPost)` &mdash; es decir, *si la solicitud no es una operación post (envío de formulario)* .</span><span class="sxs-lookup"><span data-stu-id="d3345-170">Therefore, code to read the movie is inside a test that says `if(!IsPost)` &mdash; that is, *if the request is not a post operation (form submission)*.</span></span>

## <a name="adding-code-to-delete-the-selected-movie"></a><span data-ttu-id="d3345-171">Agregar código para eliminar la película seleccionada</span><span class="sxs-lookup"><span data-stu-id="d3345-171">Adding Code to Delete the Selected Movie</span></span>

<span data-ttu-id="d3345-172">Para eliminar la película cuando el usuario haga clic en el botón, agregue el código siguiente justo dentro de la llave de cierre del bloque `@`:</span><span class="sxs-lookup"><span data-stu-id="d3345-172">To delete the movie when the user clicks the button, add the following code just inside the closing brace of the `@` block:</span></span>

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

<span data-ttu-id="d3345-173">Este código es similar al código para actualizar un registro existente, pero más sencillo.</span><span class="sxs-lookup"><span data-stu-id="d3345-173">This code is similar to the code for updating an existing record, but simpler.</span></span> <span data-ttu-id="d3345-174">Básicamente, el código ejecuta una instrucción de `Delete` de SQL.</span><span class="sxs-lookup"><span data-stu-id="d3345-174">The code basically runs a SQL `Delete` statement.</span></span>

 <span data-ttu-id="d3345-175">Como en la página *EditMovie* , el código se encuentra en un bloque `if(IsPost)`.</span><span class="sxs-lookup"><span data-stu-id="d3345-175">As in the *EditMovie* page, the code is in an `if(IsPost)` block.</span></span> <span data-ttu-id="d3345-176">Esta vez, la condición `if()` es un poco más complicada:</span><span class="sxs-lookup"><span data-stu-id="d3345-176">This time, the `if()` condition is a little more complicated:</span></span> 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

<span data-ttu-id="d3345-177">Aquí hay dos condiciones.</span><span class="sxs-lookup"><span data-stu-id="d3345-177">There are two conditions here.</span></span> <span data-ttu-id="d3345-178">La primera es que se envía la página, como ha visto antes &mdash; `if(IsPost)`.</span><span class="sxs-lookup"><span data-stu-id="d3345-178">The first is that the page is being submitted, as you've seen before &mdash; `if(IsPost)`.</span></span>

<span data-ttu-id="d3345-179">La segunda condición es `!Request["buttonDelete"].IsEmpty()`, lo que significa que la solicitud tiene un objeto denominado `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="d3345-179">The second condition is `!Request["buttonDelete"].IsEmpty()`, meaning that the request has an object named `buttonDelete`.</span></span> <span data-ttu-id="d3345-180">De forma indirecta, es un modo indirecto de probar qué botón envió el formulario.</span><span class="sxs-lookup"><span data-stu-id="d3345-180">Admittedly, it's an indirect way of testing which button submitted the form.</span></span> <span data-ttu-id="d3345-181">Si un formulario contiene varios botones de envío, solo aparece en la solicitud el nombre del botón en el que se hizo clic.</span><span class="sxs-lookup"><span data-stu-id="d3345-181">If a form contains multiple submit buttons, only the name of the button that was clicked appears in the request.</span></span> <span data-ttu-id="d3345-182">Por lo tanto, lógicamente, si el nombre de un botón determinado aparece en la solicitud &mdash; o como se indica en el código, si ese botón no está vacío &mdash; es el botón que envió el formulario.</span><span class="sxs-lookup"><span data-stu-id="d3345-182">Therefore, logically, if the name of a particular button appears in the request &mdash; or as stated in the code, if that button isn't empty &mdash; that's the button that submitted the form.</span></span>

<span data-ttu-id="d3345-183">El operador `&&` significa "and" (AND lógico).</span><span class="sxs-lookup"><span data-stu-id="d3345-183">The `&&` operator means "and" (logical AND).</span></span> <span data-ttu-id="d3345-184">Por lo tanto, toda la condición de `if` es...</span><span class="sxs-lookup"><span data-stu-id="d3345-184">Therefore the entire `if` condition is ...</span></span>

<span data-ttu-id="d3345-185">*Esta solicitud es post (no es una solicitud por primera vez)*</span><span class="sxs-lookup"><span data-stu-id="d3345-185">*This request is a post (not a first-time request)*</span></span>  
  
 <span data-ttu-id="d3345-186">AND</span><span class="sxs-lookup"><span data-stu-id="d3345-186">AND</span></span>  
  
<span data-ttu-id="d3345-187">*El* *botón `buttonDelete`era el botón que envió el formulario.*</span><span class="sxs-lookup"><span data-stu-id="d3345-187">*The* `buttonDelete`*button was the button that submitted the form.*</span></span>

<span data-ttu-id="d3345-188">Este formulario (de hecho, esta página) contiene un solo botón, por lo que técnicamente no es necesaria la prueba adicional para `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="d3345-188">This form (in fact, this page) contains only one button, so the additional test for `buttonDelete` is technically not required.</span></span> <span data-ttu-id="d3345-189">Todavía está a punto de realizar una operación que quitará los datos permanentemente.</span><span class="sxs-lookup"><span data-stu-id="d3345-189">Still, you're about to perform an operation that will permanently remove data.</span></span> <span data-ttu-id="d3345-190">Por lo tanto, es posible que desee asegurarse de que está realizando la operación solo cuando el usuario la ha solicitado explícitamente.</span><span class="sxs-lookup"><span data-stu-id="d3345-190">So you want to be as sure as possible that you're performing the operation only when the user has explicitly requested it.</span></span> <span data-ttu-id="d3345-191">Por ejemplo, supongamos que ha expandido esta página más adelante y le ha agregado otros botones.</span><span class="sxs-lookup"><span data-stu-id="d3345-191">For example, suppose that you expanded this page later and added other buttons to it.</span></span> <span data-ttu-id="d3345-192">Incluso después, el código que elimina la película solo se ejecutará si se hizo clic en el botón `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="d3345-192">Even then, the code that deletes the movie will run only if the `buttonDelete` button was clicked.</span></span>

<span data-ttu-id="d3345-193">Como en la página *EditMovie* , obtiene el identificador del campo oculto y, a continuación, ejecuta el comando SQL.</span><span class="sxs-lookup"><span data-stu-id="d3345-193">As in the *EditMovie* page, you get the ID from the hidden field and then run the SQL command.</span></span> <span data-ttu-id="d3345-194">La sintaxis de la instrucción `Delete` es:</span><span class="sxs-lookup"><span data-stu-id="d3345-194">The syntax for the `Delete` statement is:</span></span>

`DELETE FROM table WHERE ID = value`

<span data-ttu-id="d3345-195">Es fundamental incluir la cláusula `WHERE` y el identificador.</span><span class="sxs-lookup"><span data-stu-id="d3345-195">It's vital to include the `WHERE` clause and the ID.</span></span> <span data-ttu-id="d3345-196">Si omite la cláusula WHERE, se *eliminarán todos los registros de la tabla*.</span><span class="sxs-lookup"><span data-stu-id="d3345-196">If you leave out the WHERE clause, *all the records in the table will be deleted*.</span></span> <span data-ttu-id="d3345-197">Como ha aprendido, puede pasar el valor del identificador al comando SQL mediante un marcador de posición.</span><span class="sxs-lookup"><span data-stu-id="d3345-197">As you have seen, you pass the ID value to the SQL command by using a placeholder.</span></span>

## <a name="testing-the-movie-delete-process"></a><span data-ttu-id="d3345-198">Probar el proceso de eliminación de películas</span><span class="sxs-lookup"><span data-stu-id="d3345-198">Testing the Movie Delete Process</span></span>

<span data-ttu-id="d3345-199">Ahora puede probar.</span><span class="sxs-lookup"><span data-stu-id="d3345-199">Now you can test.</span></span> <span data-ttu-id="d3345-200">Ejecute la página *películas* y haga clic en **eliminar** junto a una película.</span><span class="sxs-lookup"><span data-stu-id="d3345-200">Run the *Movies* page, and click **Delete** next to a movie.</span></span> <span data-ttu-id="d3345-201">Cuando aparezca la página *DeleteMovie* , haga clic en **eliminar película**.</span><span class="sxs-lookup"><span data-stu-id="d3345-201">When the *DeleteMovie* page appears, click **Delete Movie**.</span></span>

![Página eliminar película con el botón Eliminar película resaltado](deleting-data/_static/image4.png)

<span data-ttu-id="d3345-203">Al hacer clic en el botón, el código elimina las películas y vuelve a la lista de películas.</span><span class="sxs-lookup"><span data-stu-id="d3345-203">When you click the button, the code deletes the movies and returns to the movie listing.</span></span> <span data-ttu-id="d3345-204">Allí puede buscar la película eliminada y confirmar que se ha eliminado.</span><span class="sxs-lookup"><span data-stu-id="d3345-204">There you can search for the deleted movie and confirm that it's been deleted.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="d3345-205">Próxima</span><span class="sxs-lookup"><span data-stu-id="d3345-205">Coming Up Next</span></span>

<span data-ttu-id="d3345-206">En el siguiente tutorial se muestra cómo dar a todas las páginas del sitio una apariencia y un diseño comunes.</span><span class="sxs-lookup"><span data-stu-id="d3345-206">The next tutorial shows you how to give all the pages on your site a common look and layout.</span></span>

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a><span data-ttu-id="d3345-207">Lista completa de la página de película (actualizada con vínculos de eliminación)</span><span class="sxs-lookup"><span data-stu-id="d3345-207">Complete Listing for Movie Page (Updated with Delete Links)</span></span>

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a><span data-ttu-id="d3345-208">Lista completa de la página de DeleteMovie</span><span class="sxs-lookup"><span data-stu-id="d3345-208">Complete Listing for DeleteMovie Page</span></span>

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="d3345-209">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="d3345-209">Additional Resources</span></span>

- [<span data-ttu-id="d3345-210">Introducción a la programación web de ASP.NET mediante la sintaxis Razor</span><span class="sxs-lookup"><span data-stu-id="d3345-210">Introduction to ASP.NET Web Programming by Using the Razor Syntax</span></span>](../introducing-razor-syntax-c.md)
- <span data-ttu-id="d3345-211">[Instrucción SQL Delete](http://www.w3schools.com/sql/sql_delete.asp) en el sitio w3schools</span><span class="sxs-lookup"><span data-stu-id="d3345-211">[SQL DELETE Statement](http://www.w3schools.com/sql/sql_delete.asp) on the W3Schools site</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d3345-212">[Anterior](updating-data.md)
> [Siguiente](layouts.md)</span><span class="sxs-lookup"><span data-stu-id="d3345-212">[Previous](updating-data.md)
[Next](layouts.md)</span></span>
