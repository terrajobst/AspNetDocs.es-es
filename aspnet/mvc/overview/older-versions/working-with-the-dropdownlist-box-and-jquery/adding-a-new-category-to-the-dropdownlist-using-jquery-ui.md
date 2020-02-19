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
# <a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a>Agregar una nueva categoría al control DropDownList mediante jQuery UI

por [Rick Anderson](https://twitter.com/RickAndMSFT)

La etiqueta de `Select` HTML es ideal para presentar una lista de datos de categoría fija, pero a menudo es necesario agregar una nueva categoría. Supongamos que queremos agregar el género "opera" a las categorías de nuestra base de datos. En esta sección, usaremos la interfaz de usuario de jQuery para agregar un cuadro de diálogo que se puede usar para agregar una nueva categoría. En la imagen siguiente se muestra cómo se presentará la interfaz de usuario en el explorador.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

Cuando un usuario selecciona el vínculo **Agregar nuevo género** , un cuadro de diálogo emergente solicita al usuario un nuevo nombre de género (y, opcionalmente, una descripción). En la imagen siguiente se muestra el cuadro de diálogo emergente **Agregar género** .

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

Cuando se escribe un nuevo nombre de género y se inserta el botón **Guardar** , ocurre lo siguiente:

1. Una llamada AJAX envía los datos al método Create del controlador Genre, que guarda el nuevo género en la base de datos y devuelve la nueva información del género (nombre y identificador del género) como JSON.
2. JavaScript agrega los nuevos datos de género a la lista de selección.
3. JavaScript convierte el nuevo género en el elemento seleccionado.

   En la imagen siguiente, **opera** se agregó a la base de datos y se seleccionó en la lista desplegable **género** . 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

Abra el archivo *Views\StoreManager\Create.cshtml* y reemplace el marcado de género con el siguiente código:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

La vista parcial de `_ChooseGenre` contendrá toda la lógica para enlazar el código JavaScript y jQuery que se usa para implementar la característica agregar nuevo género. Una vez completado el código, será fácil hacer lo mismo con la interfaz de usuario del intérprete.

En Explorador de soluciones, haga clic con el botón secundario en la carpeta *Views\StoreManager* , seleccione **Agregar**y, a continuación, **Ver**. En la entrada **nombre de vista** , escriba `_ChooseGenre`, a continuación, seleccione **Agregar**. Reemplace el marcado en el archivo *Views\StoreManager\\_ChooseGenre. cshtml* con lo siguiente:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

La primera línea declara que pasamos un `Album` como modelo, exactamente la misma instrucción del modelo que se encuentra en la vista crear. Las siguientes líneas son el marcado de la aplicación auxiliar de **etiqueta** . La siguiente línea es la llamada de la aplicación auxiliar de **DropDownList** , exactamente igual que en la vista de creación original. La línea siguiente agrega un vínculo con el nombre `Add New Genre`y lo estilos como un botón. La línea que contiene `ValidationMessageFor` se copia directamente desde la vista crear. Las siguientes líneas:

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

crea un div oculto, con el identificador de `genreDialog`. Usaremos jQuery para enlazar el cuadro de diálogo **Agregar género** con el identificador `genreDialog` en este div. Las dos últimas etiquetas de script contienen vínculos a los archivos de JavaScript que se utilizarán para implementar la característica agregar nuevo género. El archivo */scripts/chooseGenre.js* se proporciona en el proyecto. lo examinaremos más adelante en el tutorial.

Ejecute la aplicación y haga clic en el botón **Agregar nuevo género** . En el cuadro de diálogo **Agregar género** , escriba **opera** en el cuadro de entrada **nombre** .

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

Haga clic en el botón **Save** (Guardar). Una llamada AJAX crea la categoría opera y, a continuación, rellena la lista desplegable con opera y establece opera como el género seleccionado.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

Escriba un intérprete, un título y un precio y, a continuación, seleccione el botón **crear** . Si especifica un precio inferior a $8,99, el nuevo álbum aparecerá en la parte superior de la vista de índice. Compruebe que la nueva entrada de álbum se guardó en la base de datos.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

Intente crear un nuevo género con una sola letra. El siguiente código del archivo *Models\Genre.CS* establece la longitud mínima y máxima del nombre del género.

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

Informes de validación del lado cliente debe especificar una cadena de entre 2 y 20 caracteres.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a>Examinar cómo se agrega un nuevo género a la base de datos y la lista de selección.

Abra el archivo *Scripts\chooseGenre.js* y examine el código.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

La segunda línea usa el identificador `genreDialog` para crear un cuadro de diálogo en la etiqueta div en el archivo *Views\StoreManager\\_ChooseGenre. cshtml* . La mayoría de los parámetros con nombre son autoexplicativos. El parámetro `autoOpen` está establecido en false, al seleccionar el botón **crear género** se abrirá el cuadro de diálogo explícitamente (esto se describe aquí). El cuadro de diálogo tiene dos botones: **Guardar** y **Cancelar**. El botón **Cancelar** cierra el cuadro de diálogo. En el código siguiente se muestra la función del botón **Guardar** .

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

El `var createGenreForm` se selecciona en el ID. de `createGenreForm`. El identificador de `createGenreForm` se estableció en el siguiente código que se encuentra en el archivo *Views\Genre\\_CreateGenre. cshtml* .

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

La sobrecarga de la aplicación auxiliar [HTML. BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) utilizada en el archivo *Views\Genre\\_CreateGenre. cshtml* genera HTML con un atributo de acción que contiene la dirección URL para enviar el formulario. Puede verlo mostrando la página crear álbum en un explorador y seleccionando Mostrar origen en el explorador. El marcado siguiente muestra el HTML generado que contiene la etiqueta de formulario.

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

La línea de `$.post` de jQuery realiza una llamada AJAX al atributo Action (`/StoreManager/Create`) y pasa los datos del cuadro de diálogo **Create Genre** . Los datos se componen del nombre del nuevo género y una descripción opcional. Si la llamada AJAX se realiza correctamente, el nuevo nombre y valor de género se agregan al marcado de selección y el nuevo género se establece en el valor seleccionado. Dado que se trata de un marcado generado dinámicamente, no se puede ver la nueva opción seleccionar viendo el origen en el explorador. Puede ver el nuevo HTML con las herramientas de desarrollo F12 de IE 9. Para ver la nueva opción seleccionar, en Internet Explorer 9, presione la tecla F12 para iniciar las herramientas de desarrollo F12. Navegue hasta la página crear y agregue un nuevo género para que el nuevo género esté seleccionado en la lista de selección de género. En las herramientas de desarrollo F12:

1. Seleccione la pestaña HTML.
2. Presione el icono de actualización.  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. En el cuadro de búsqueda, escriba GenreID.
4. Con el siguiente icono,   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   vaya a la siguiente etiqueta Select:

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. Expanda el último valor de opción.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

El siguiente código del archivo *Scripts\chooseGenre.js* muestra cómo se conecta el botón **Agregar nuevo género** al evento click y cómo se crea el cuadro de diálogo **Agregar nuevo género** .

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

La primera línea crea una función de clic asociada al botón **Agregar nuevo género** . El siguiente marcado del archivo Views\StoreManager\\_ChooseGenre. cshtml muestra cómo se crea el botón **Agregar nuevo género** :

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

El método Load crea y abre el cuadro de diálogo Agregar género y llama al método jQuery `parse`, por lo que la validación del cliente se produce en los datos especificados en el cuadro de diálogo.

En esta sección, ha aprendido a crear un cuadro de diálogo que se puede usar para agregar nuevos datos de categoría a una lista de selección. Puede seguir el mismo procedimiento para crear una interfaz de usuario para agregar un nuevo artista a la lista de selección de intérprete. En este tutorial se ha proporcionado información general sobre cómo trabajar con el **DropDownList**de la aplicación auxiliar HTML MVC de ASP.net. Para obtener información adicional sobre cómo trabajar con **DropDownList**, vea la sección referencias adicionales. Háganoslo saber si este tutorial ha sido útil.

Rick.Anderson[at]Microsoft.com

### <a name="additional-references"></a>Referencias adicionales

- [ASP.NET MVC: listas desplegables en cascada tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) de [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- [Elegido](https://harvesthq.github.com/chosen/) Un complemento de JavaScript que admite la selección múltiple y el filtrado.

### <a name="contributors"></a>Colaboradores

- [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- Jean-Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)

### <a name="reviewers"></a>Revisores

- Jean-Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)
- Mike Pope
- Tom Dykstra

> [!div class="step-by-step"]
> [Anterior](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
