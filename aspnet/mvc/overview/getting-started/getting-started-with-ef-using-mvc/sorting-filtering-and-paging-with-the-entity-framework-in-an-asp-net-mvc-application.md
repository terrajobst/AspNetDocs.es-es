---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutorial: agregar ordenación, filtrado y paginación con el Entity Framework en una aplicación ASP.NET MVC | Microsoft Docs'
author: tdykstra
description: En este tutorial, agregará funcionalidad de ordenación, filtrado y paginación a la página de índice de **Students** . También creará una página de agrupación simple.
ms.author: riande
ms.date: 01/14/2019
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: d9fadc12aa83a8095f364cf39e5376243a7d0670
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78499333"
---
# <a name="tutorial-add-sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Tutorial: agregar ordenación, filtrado y paginación con el Entity Framework en una aplicación ASP.NET MVC

En el [tutorial anterior](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md), ha implementado un conjunto de páginas web para las operaciones CRUD básicas para las entidades `Student`. En este tutorial, agregará funcionalidad de ordenación, filtrado y paginación a la página de índice de **Students** . También creará una página de agrupación simple.

En la imagen siguiente se muestra el aspecto que tendrá la página cuando haya terminado. Los encabezados de columna son vínculos en los que el usuario puede hacer clic para ordenar las columnas correspondientes. Si se hace clic de forma consecutiva en el encabezado de una columna, el criterio de ordenación alterna entre ascendente y descendente.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

En este tutorial va a:

> [!div class="checklist"]
> * Agrega vínculos de ordenación de columnas
> * Agrega un cuadro de búsqueda
> * Adición de paginación
> * Crea una página About

## <a name="prerequisites"></a>Requisitos previos

* [Implementar la funcionalidad CRUD básica](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)

## <a name="add-column-sort-links"></a>Agrega vínculos de ordenación de columnas

Para agregar la ordenación a la página de índice de estudiante, cambiará el método `Index` del controlador de `Student` y agregará código a la vista de índice `Student`.

### <a name="add-sorting-functionality-to-the-index-method"></a>Agregar funcionalidad de ordenación al método index

- En *Controllers\StudentController.CS*, reemplace el método `Index` por el código siguiente:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Este código recibe un parámetro `sortOrder` de la cadena de consulta en la dirección URL. ASP.NET MVC proporciona el valor de la cadena de consulta como un parámetro al método de acción. El parámetro es una cadena que es "Name" o "date", seguido opcionalmente por un carácter de subrayado y la cadena "DESC" para especificar el orden descendente. El criterio de ordenación predeterminado es el ascendente.

La primera vez que se solicita la página de índice, no hay ninguna cadena de consulta. Los alumnos se muestran en orden ascendente por `LastName`, que es el valor predeterminado establecido por el caso de paso a través en la instrucción `switch`. Cuando el usuario hace clic en un hipervínculo de encabezado de columna, se proporciona el valor `sortOrder` correspondiente en la cadena de consulta.

Las dos variables `ViewBag` se usan para que la vista pueda configurar los hipervínculos del encabezado de columna con los valores de cadena de consulta adecuados:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Estas son las instrucciones ternarias. La primera especifica que si el parámetro `sortOrder` es null o está vacío, `ViewBag.NameSortParm` debe establecerse en "Name\_DESC"; de lo contrario, debe establecerse en una cadena vacía. Estas dos instrucciones habilitan la vista para establecer los hipervínculos de encabezado de columna de la forma siguiente:

| Criterio de ordenación actual | Hipervínculo de apellido | Hipervínculo de fecha |
| --- | --- | --- |
| Apellido: ascendente | descending | ascending |
| Apellido: descendente | ascending | ascending |
| Fecha: ascendente | ascending | descending |
| Fecha: descendente | ascending | ascending |

El método utiliza [LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities) para especificar la columna por la que se va a ordenar. El código crea una variable <xref:System.Linq.IQueryable%601> antes de la instrucción `switch`, la modifica en la instrucción `switch` y llama al método `ToList` después de la instrucción `switch`. Al crear y modificar variables `IQueryable`, no se envía ninguna consulta a la base de datos. La consulta no se ejecuta hasta que se convierte el objeto de `IQueryable` en una colección mediante una llamada a un método como `ToList`. Por lo tanto, este código produce una única consulta que no se ejecuta hasta la instrucción `return View`.

Como alternativa a escribir instrucciones LINQ diferentes para cada criterio de ordenación, puede crear dinámicamente una instrucción LINQ. Para obtener información sobre LINQ dinámico, vea [LINQ dinámico](https://go.microsoft.com/fwlink/?LinkID=323957).

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Agregar hipervínculos de encabezado de columna a la vista de índice de Student

1. En *Views\Student\Index.cshtml*, reemplace los elementos `<tr>` y `<th>` de la fila de encabezado por el código resaltado:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

   Este código usa la información de las propiedades de `ViewBag` para configurar hipervínculos con los valores de cadena de consulta adecuados.

2. Ejecute la página y haga clic en los encabezados de columna **Last Name** y **Enrollment Date** para comprobar que funciona la ordenación.

   Después de hacer clic en el encabezado **Last Name** , los estudiantes se muestran en orden descendente de apellidos.

## <a name="add-a-search-box"></a>Agrega un cuadro de búsqueda

Para agregar filtrado a la página de índice de Students, agregará un cuadro de texto y un botón de envío a la vista y realizará los cambios correspondientes en el método `Index`. El cuadro de texto permite especificar una cadena que se va a buscar en los campos nombre y apellidos.

### <a name="add-filtering-functionality-to-the-index-method"></a>Agregar la funcionalidad de filtrado al método Index

- En *Controllers\StudentController.CS*, reemplace el método `Index` por el código siguiente (los cambios se resaltan):

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

El código agrega un parámetro `searchString` al método `Index`. El valor de la cadena de búsqueda se recibe desde un cuadro de texto que agregará a la vista de índice. También agrega una cláusula `where` a la instrucción LINQ que selecciona solo los alumnos cuyo nombre o apellido contienen la cadena de búsqueda. La instrucción que agrega la cláusula <xref:System.Linq.Queryable.Where%2A> solo se ejecuta si hay un valor que se va a buscar.

> [!NOTE]
> En muchos casos, puede llamar al mismo método en un conjunto de entidades Entity Framework o como un método de extensión en una colección en memoria. Los resultados suelen ser los mismos pero, en algunos casos, pueden ser diferentes.
>
> Por ejemplo, la implementación de .NET Framework del método `Contains` devuelve todas las filas cuando se le pasa una cadena vacía, pero el proveedor de Entity Framework para SQL Server Compact 4,0 devuelve cero filas para las cadenas vacías. Por lo tanto, el código del ejemplo (que coloca la instrucción `Where` dentro de una instrucción `if`) garantiza que obtiene los mismos resultados para todas las versiones de SQL Server. Además, la implementación de .NET Framework del método `Contains` realiza una comparación con distinción entre mayúsculas y minúsculas de forma predeterminada, pero Entity Framework proveedores de SQL Server realizan comparaciones sin distinción entre mayúsculas y minúsculas de forma predeterminada. Por lo tanto, al llamar al método `ToUpper` para hacer que la prueba no distinga explícitamente mayúsculas de minúsculas, se garantiza que los resultados no cambian cuando se cambia el código más adelante para utilizar un repositorio, que devolverá una colección `IEnumerable` en lugar de un objeto `IQueryable`. (Al hacer una llamada al método `Contains` en una colección `IEnumerable`, obtendrá la implementación de .NET Framework; al hacer una llamada a un objeto `IQueryable`, obtendrá la implementación del proveedor de base de datos).
>
> El control de valores null también puede ser diferente para los diferentes proveedores de bases de datos o cuando se usa un objeto de `IQueryable` con respecto al uso de una colección `IEnumerable`. Por ejemplo, en algunos escenarios es posible que una condición `Where` como `table.Column != 0` no devuelva columnas que tengan `null` como valor. De forma predeterminada, EF genera operadores SQL adicionales para que la igualdad entre valores NULL funcione en la base de datos, como funciona en memoria, pero puede establecer la marca [UseDatabaseNullSemantics](https://docs.microsoft.com/dotnet/api/system.data.entity.infrastructure.dbcontextconfiguration.usedatabasenullsemantics) en EF6 o llamar al método [UseRelationalNulls](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.infrastructure.relationaldbcontextoptionsbuilder-2.userelationalnulls) en EF Core para configurar este comportamiento.

### <a name="add-a-search-box-to-the-student-index-view"></a>Agregar un cuadro de búsqueda a la vista de índice de Student

1. En *Views\Student\Index.cshtml*, agregue el código resaltado inmediatamente antes de la etiqueta de apertura `table` para crear un título, un cuadro de texto y un botón de **búsqueda** .

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=4-11)]

2. Ejecute la página, escriba una cadena de búsqueda y haga clic en **Buscar** para comprobar que funciona el filtrado.

   Observe que la dirección URL no contiene la cadena de búsqueda "an", lo que significa que, si marca esta página, no obtendrá la lista filtrada al usar el marcador. Esto se aplica también a los vínculos de ordenación de columnas, ya que ordenarán toda la lista. Cambiará el botón **Buscar** para usar cadenas de consulta para los criterios de filtro más adelante en el tutorial.

## <a name="add-paging"></a>Adición de paginación

Para agregar paginación a la página de índice de Students, debe empezar por instalar el paquete NuGet **PagedList. Mvc** . A continuación, realizará cambios adicionales en el método `Index` y agregará vínculos de paginación a la vista `Index`. **PagedList. Mvc** es uno de los muchos paquetes correctos de paginación y ordenación para ASP.NET MVC, y su uso aquí solo está pensado como ejemplo, no como una recomendación para él en otras opciones.

### <a name="install-the-pagedlistmvc-nuget-package"></a>Instalación del paquete NuGet de PagedList. MVC

El paquete NuGet **PagedList. Mvc** instala automáticamente el paquete **PagedList** como una dependencia. El paquete **PagedList** instala un tipo de colección `PagedList` y métodos de extensión para las colecciones `IQueryable` y `IEnumerable`. Los métodos de extensión crean una sola página de datos en una colección de `PagedList` fuera del `IQueryable` o `IEnumerable`, y la colección de `PagedList` proporciona varias propiedades y métodos que facilitan la paginación. El paquete **PagedList. Mvc** instala una aplicación auxiliar de paginación que muestra los botones de paginación.

1. En el menú **herramientas** , seleccione **Administrador de paquetes NuGet** y **consola del administrador de paquetes**.

2. En la ventana de la **consola del administrador de paquetes** , asegúrese de que el origen del **paquete** es **Nuget.org** y el **proyecto predeterminado** es **ContosoUniversity**y, a continuación, escriba el siguiente comando:

   ```text
   Install-Package PagedList.Mvc
   ```

3. Compile el proyecto.

### <a name="add-paging-functionality-to-the-index-method"></a>Agregar la funcionalidad de paginación al método Index

1. En *Controllers\StudentController.CS*, agregue una instrucción `using` para el espacio de nombres `PagedList`:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

2. Reemplace el método `Index` con el código siguiente:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

   Este código agrega un parámetro `page`, un parámetro de criterio de ordenación actual y un parámetro de filtro actual a la firma del método:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

   La primera vez que se muestra la página o si el usuario no ha hecho clic en un vínculo de paginación o de ordenación, todos los parámetros son NULL. Si se hace clic en un vínculo de paginación, la variable de `page` contiene el número de página que se va a mostrar.

   Una propiedad `ViewBag` proporciona la vista con el criterio de ordenación actual, ya que debe incluirse en los vínculos de paginación para mantener el criterio de ordenación igual durante la paginación:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

   Otra propiedad, `ViewBag.CurrentFilter`, proporciona la vista con la cadena de filtro actual. Este valor debe incluirse en los vínculos de paginación para mantener la configuración de filtrado durante la paginación y debe restaurarse en el cuadro de texto cuando se vuelve a mostrar la página. Si se cambia la cadena de búsqueda durante la paginación, la página debe restablecerse a 1, porque el nuevo filtro puede hacer que se muestren diferentes datos. La cadena de búsqueda cambia cuando se escribe un valor en el cuadro de texto y se presiona el botón Enviar. En ese caso, el parámetro `searchString` no es NULL.

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

   Al final del método, el método de extensión `ToPagedList` del objeto Students `IQueryable` convierte la consulta Student en una sola página de Students en un tipo de colección que admite la paginación. A continuación, esa página única de estudiantes se pasa a la vista:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

   El método `ToPagedList` toma un número de página. Los dos signos de interrogación representan el [operador de uso combinado de NULL](/dotnet/csharp/language-reference/operators/null-coalescing-operator). El operador de uso combinado de NULL define un valor predeterminado para un tipo que acepta valores NULL; la expresión `(page ?? 1)` devuelve el valor de `page` si tiene algún valor o devuelve 1 si `page` es NULL.

### <a name="add-paging-links-to-the-student-index-view"></a>Agregar vínculos de paginación a la vista de índice de Student

1. En *Views\Student\Index.cshtml*, reemplace el código existente por el código siguiente. Los cambios aparecen resaltados.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

   La instrucción `@model` de la parte superior de la página especifica que ahora la vista obtiene un objeto `PagedList` en lugar de un objeto `List`.

   La instrucción `using` para `PagedList.Mvc` proporciona acceso a la aplicación auxiliar de MVC para los botones de paginación.

   El código usa una sobrecarga de [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) que le permite especificar [formmethod. Get](/previous-versions/aspnet/dd460179(v=vs.100)).

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

   El valor de [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) predeterminado envía datos de formulario con post, lo que significa que los parámetros se pasan en el cuerpo del mensaje http y no en la dirección URL como cadenas de consulta. Al especificar HTTP GET, los datos de formulario se pasan en la dirección URL como cadenas de consulta, lo que permite que los usuarios marquen la dirección URL. Las [directrices del W3C para el uso de http Get](http://www.w3.org/2001/tag/doc/whenToUseGet.html) recomiendan usar Get cuando la acción no produce una actualización.

   El cuadro de texto se inicializa con la cadena de búsqueda actual, por lo que al hacer clic en una página nueva puede ver la cadena de búsqueda actual.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

   Los vínculos del encabezado de la columna usan la cadena de consulta para pasar la cadena de búsqueda actual al controlador, de modo que el usuario pueda ordenar los resultados del filtro:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

   Se muestran la página actual y el número total de páginas.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

   Si no hay ninguna página para mostrar, se muestra "página 0 de 0". (En ese caso, el número de página es mayor que el recuento de páginas porque `Model.PageNumber` es 1 y `Model.PageCount` es 0).

   La aplicación auxiliar `PagedListPager` muestra los botones de paginación:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

   La aplicación auxiliar de `PagedListPager` proporciona varias opciones que puede personalizar, incluidas las direcciones URL y los estilos. Para obtener más información, consulte [TroyGoode/PagedList](https://github.com/TroyGoode/PagedList) en el sitio de github.

2. Ejecute la página.

   Haga clic en los vínculos de paginación en distintos criterios de ordenación para comprobar que la paginación funciona correctamente. A continuación, escriba una cadena de búsqueda e intente llevar a cabo la paginación de nuevo, para comprobar que la paginación también funciona correctamente con filtrado y ordenación.

## <a name="create-an-about-page"></a>Crea una página About

En la página acerca del sitio web de Contoso University, mostrará Cuántos alumnos se han inscrito para cada fecha de inscripción. Esto requiere realizar agrupaciones y cálculos sencillos en los grupos. Para conseguirlo, haga lo siguiente:

- Cree una clase de modelo de vista para los datos que necesita pasar a la vista.
- Modifique el método `About` en el controlador de `Home`.
- Modifique la vista de `About`.

### <a name="create-the-view-model"></a>Crear el modelo de vista

Cree una carpeta *ViewModels* en la carpeta del proyecto. En esa carpeta, agregue un archivo de clase *EnrollmentDateGroup.CS* y reemplace el código de plantilla por el código siguiente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Modificación del controlador Home

1. En *HomeController.CS*, agregue las siguientes instrucciones `using` en la parte superior del archivo:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

2. Agregue una variable de clase para el contexto de la base de datos inmediatamente después de la llave de apertura de la clase:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

3. Reemplace el método `About` con el código siguiente:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

   La instrucción LINQ agrupa las entidades de alumnos por fecha de inscripción, calcula la cantidad de entidades que se incluyen en cada grupo y almacena los resultados en una colección de objetos de modelo de la vista `EnrollmentDateGroup`.

4. Agregue un método `Dispose`:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Modificación de la vista About

1. Reemplace el código del archivo *Views\Home\About.cshtml* por el código siguiente:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

2. Ejecute la aplicación y haga clic en el vínculo **About (acerca de** ).

   El número de alumnos para cada fecha de inscripción se muestra en una tabla.

   ![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="get-the-code"></a>Obtención del código

[Descargar el proyecto completado](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Recursos adicionales

Los vínculos a otros recursos de Entity Framework pueden encontrarse en [el acceso a datos ASP.net: recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial va a:

> [!div class="checklist"]
> * Agrega vínculos de ordenación de columnas
> * Agrega un cuadro de búsqueda
> * Adición de paginación
> * Crea una página About

Avance al siguiente artículo para aprender a usar la resistencia de conexión y la interceptación de comandos.
> [!div class="nextstepaction"]
> [Resistencia de la conexión e intercepción de comandos](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)
