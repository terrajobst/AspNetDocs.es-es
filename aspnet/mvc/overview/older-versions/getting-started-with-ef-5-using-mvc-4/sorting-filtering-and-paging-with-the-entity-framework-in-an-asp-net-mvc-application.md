---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Ordenación, filtrado y paginación con el Entity Framework en una aplicación ASP.NET MVC (3 de 10) | Microsoft Docs
author: tdykstra
description: En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones ASP.NET MVC 4 con el Code First de Entity Framework 5 y Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 8af630e0-fffa-4110-9eca-c96e201b2724
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: b1ddb70805dcb07fb60eea895ff572c054bde5c6
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595232"
---
# <a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application-3-of-10"></a>Ordenación, filtrado y paginación con el Entity Framework en una aplicación ASP.NET MVC (3 de 10)

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto completado](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones ASP.NET MVC 4 con el Code First de Entity Framework 5 y Visual Studio 2012. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Puede iniciar la serie de tutoriales desde el principio o [descargar un proyecto de inicio para este capítulo](building-the-ef5-mvc4-chapter-downloads.md) y empezar aquí.
> 
> > [!NOTE] 
> > 
> > Si experimenta un problema que no puede resolver, [Descargue el capítulo completado](building-the-ef5-mvc4-chapter-downloads.md) e intente reproducir el problema. Por lo general, puede encontrar la solución al problema comparando el código con el código completado. Para algunos errores comunes y cómo resolverlos, consulte [errores y soluciones alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

En el tutorial anterior, implementó un conjunto de páginas web para las operaciones CRUD básicas para las entidades `Student`. En este tutorial, agregará funcionalidad de ordenación, filtrado y paginación a la página de índice de **Students** . También crearemos una página que realice agrupaciones sencillas.

En la siguiente ilustración se muestra el aspecto que tendrá la página cuando haya terminado. Los encabezados de columna son vínculos en los que el usuario puede hacer clic para ordenar las columnas correspondientes. Si se hace clic de forma consecutiva en el encabezado de una columna, el criterio de ordenación alterna entre ascendente y descendente.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Agregar vínculos de ordenación de columnas en la página de índice de Students

Para agregar la ordenación a la página de índice de estudiante, cambiará el método `Index` del controlador de `Student` y agregará código a la vista de índice `Student`.

### <a name="add-sorting-functionality-to-the-index-method"></a>Agregar funcionalidad de ordenación al método index

En *Controllers\StudentController.CS*, reemplace el método `Index` por el código siguiente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Este código recibe un parámetro `sortOrder` de la cadena de consulta en la dirección URL. ASP.NET MVC proporciona el valor de la cadena de consulta como un parámetro al método de acción. El parámetro es una cadena que puede ser "Name" o "Date", seguido (opcionalmente) por un guión bajo y la cadena "desc" para especificar el orden descendente. El criterio de ordenación predeterminado es el ascendente.

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

El método utiliza [LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx) para especificar la columna por la que se va a ordenar. El código crea una variable [IQueryable](https://msdn.microsoft.com/library/bb351562.aspx) antes de la instrucción `switch`, la modifica en la instrucción `switch` y llama al método `ToList` después de la instrucción `switch`. Al crear y modificar variables `IQueryable`, no se envía ninguna consulta a la base de datos. La consulta no se ejecuta hasta que se convierte el objeto de `IQueryable` en una colección mediante una llamada a un método como `ToList`. Por lo tanto, este código produce una única consulta que no se ejecuta hasta la instrucción `return View`.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Agregar hipervínculos de encabezado de columna a la vista de índice de Student

En *Views\Student\Index.cshtml*, reemplace los elementos `<tr>` y `<th>` de la fila de encabezado por el código resaltado:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

Este código usa la información de las propiedades de `ViewBag` para configurar hipervínculos con los valores de cadena de consulta adecuados.

Ejecute la página y haga clic en los encabezados de columna **Last Name** y **Enrollment Date** para comprobar que funciona la ordenación.

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Después de hacer clic en el encabezado **Last Name** , los estudiantes se muestran en orden descendente de apellidos.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Agregar un cuadro de búsqueda a la página de índice de Students

Para agregar filtrado a la página de índice de Students, agregue un cuadro de texto y un botón de envío a la vista y haga los cambios correspondientes en el método `Index`. El cuadro de texto le permite escribir la cadena que quiera buscar en los campos de nombre y apellido.

### <a name="add-filtering-functionality-to-the-index-method"></a>Agregar funcionalidad de filtrado al método index

En *Controllers\StudentController.CS*, reemplace el método `Index` por el código siguiente (los cambios se resaltan):

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Ha agregado un parámetro `searchString` al método `Index`. También ha agregado a la instrucción LINQ una cláusula `where` que selecciona solo los alumnos cuyo nombre o apellido contienen la cadena de búsqueda. El valor de la cadena de búsqueda se recibe de un cuadro de texto que se agregará a la vista de índice. La instrucción que agrega la cláusula [Where](https://msdn.microsoft.com/library/bb535040.aspx) solo se ejecuta si hay un valor que se va a buscar.

> [!NOTE]
> En muchos casos, puede llamar al mismo método en un conjunto de entidades Entity Framework o como un método de extensión en una colección en memoria. Los resultados suelen ser los mismos pero, en algunos casos, pueden ser diferentes. Por ejemplo, la implementación de .NET Framework del método `Contains` devuelve todas las filas cuando se le pasa una cadena vacía, pero el proveedor de Entity Framework para SQL Server Compact 4,0 devuelve cero filas para las cadenas vacías. Por lo tanto, el código del ejemplo (que coloca la instrucción `Where` dentro de una instrucción `if`) garantiza que obtiene los mismos resultados para todas las versiones de SQL Server. Además, la implementación de .NET Framework del método `Contains` realiza una comparación con distinción entre mayúsculas y minúsculas de forma predeterminada, pero Entity Framework proveedores de SQL Server realizan comparaciones sin distinción entre mayúsculas y minúsculas de forma predeterminada. Por lo tanto, al llamar al método `ToUpper` para hacer que la prueba no distinga explícitamente mayúsculas de minúsculas, se garantiza que los resultados no cambian cuando se cambia el código más adelante para utilizar un repositorio, que devolverá una colección `IEnumerable` en lugar de un objeto `IQueryable`. (Al hacer una llamada al método `Contains` en una colección `IEnumerable`, obtendrá la implementación de .NET Framework; al hacer una llamada a un objeto `IQueryable`, obtendrá la implementación del proveedor de base de datos).

### <a name="add-a-search-box-to-the-student-index-view"></a>Agregar un cuadro de búsqueda a la vista de índice de Student

En *Views\Student\Index.cshtml*, agregue el código resaltado inmediatamente antes de la etiqueta de apertura `table` para crear un título, un cuadro de texto y un botón de **búsqueda** .

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Ejecute la página, escriba una cadena de búsqueda y haga clic en **Buscar** para comprobar que funciona el filtrado.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Observe que la dirección URL no contiene la cadena de búsqueda "an", lo que significa que, si marca esta página, no obtendrá la lista filtrada al usar el marcador. Cambiará el botón **Buscar** para usar cadenas de consulta para los criterios de filtro más adelante en el tutorial.

## <a name="add-paging-to-the-students-index-page"></a>Agregar paginación a la página de índice de Students

Para agregar paginación a la página de índice de Students, debe empezar por instalar el paquete NuGet **PagedList. Mvc** . A continuación, realizará cambios adicionales en el método `Index` y agregará vínculos de paginación a la vista `Index`. **PagedList. Mvc** es uno de los muchos paquetes correctos de paginación y ordenación para ASP.NET MVC, y su uso aquí solo está pensado como ejemplo, no como una recomendación para él en otras opciones. En la ilustración siguiente se muestran los vínculos de paginación.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Instalación del paquete NuGet de PagedList. MVC

El paquete NuGet **PagedList. Mvc** instala automáticamente el paquete **PagedList** como una dependencia. El paquete **PagedList** instala un tipo de colección `PagedList` y métodos de extensión para las colecciones `IQueryable` y `IEnumerable`. Los métodos de extensión crean una sola página de datos en una colección de `PagedList` fuera del `IQueryable` o `IEnumerable`, y la colección de `PagedList` proporciona varias propiedades y métodos que facilitan la paginación. El paquete **PagedList. Mvc** instala una aplicación auxiliar de paginación que muestra los botones de paginación.

En el menú **herramientas** , seleccione **Administrador de paquetes Nuget** y, a continuación, **administrar paquetes Nuget para la solución**.

En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en la pestaña en **línea** de la izquierda y, a continuación, escriba "paginada" en el cuadro de búsqueda. Cuando vea el paquete **PagedList. Mvc** , haga clic en **instalar**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

En el cuadro **seleccionar proyectos** , haga clic en **Aceptar**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="add-paging-functionality-to-the-index-method"></a>Agregar funcionalidad de paginación al método index

En *Controllers\StudentController.CS*, agregue una instrucción `using` para el espacio de nombres `PagedList`:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Reemplace el método `Index` con el código siguiente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Este código agrega un parámetro `page`, un parámetro de criterio de ordenación actual y un parámetro de filtro actual a la firma del método, como se muestra aquí:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

La primera vez que se muestra la página, o si el usuario no ha hecho clic en un vínculo de ordenación o paginación, todos los parámetros son nulos. Si se hace clic en un vínculo de paginación, la variable de `page` contendrá el número de página que se va a mostrar.

`A ViewBag` propiedad proporciona la vista con el criterio de ordenación actual, ya que debe incluirse en los vínculos de paginación para mantener el criterio de ordenación igual durante la paginación:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Otra propiedad, `ViewBag.CurrentFilter`, proporciona la vista con la cadena de filtro actual. Este valor debe incluirse en los vínculos de paginación para mantener la configuración de filtrado durante la paginación y debe restaurarse en el cuadro de texto cuando se vuelve a mostrar la página. Si se cambia la cadena de búsqueda durante la paginación, la página debe restablecerse a 1, porque el nuevo filtro puede hacer que se muestren diferentes datos. La cadena de búsqueda cambia cuando se escribe un valor en el cuadro de texto y se presiona el botón Enviar. En ese caso, el parámetro `searchString` no es NULL.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Al final del método, el método de extensión `ToPagedList` del objeto Students `IQueryable` convierte la consulta Student en una sola página de Students en un tipo de colección que admite la paginación. A continuación, esa página única de estudiantes se pasa a la vista:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

El método `ToPagedList` toma un número de página. Los dos signos de interrogación representan el [operador de uso combinado de NULL](https://msdn.microsoft.com/library/ms173224.aspx). El operador de uso combinado de NULL define un valor predeterminado para un tipo que acepta valores NULL; la expresión `(page ?? 1)` devuelve el valor de `page` si tiene algún valor o devuelve 1 si `page` es NULL.

### <a name="add-paging-links-to-the-student-index-view"></a>Agregar vínculos de paginación a la vista de índice de Student

En *Views\Student\Index.cshtml*, reemplace el código existente por el código siguiente:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=6,9,14-20,56-58)]

La instrucción `@model` de la parte superior de la página especifica que ahora la vista obtiene un objeto `PagedList` en lugar de un objeto `List`.

La instrucción `using` para `PagedList.Mvc` proporciona acceso a la aplicación auxiliar de MVC para los botones de paginación.

El código usa una sobrecarga de [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) que le permite especificar [formmethod. Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

El valor de [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) predeterminado envía datos de formulario con post, lo que significa que los parámetros se pasan en el cuerpo del mensaje http y no en la dirección URL como cadenas de consulta. Al especificar HTTP GET, los datos de formulario se pasan en la dirección URL como cadenas de consulta, lo que permite que los usuarios marquen la dirección URL. Las [instrucciones del W3C para el uso de http Get](http://www.w3.org/2001/tag/doc/whenToUseGet.html) especifican que se debe utilizar Get cuando la acción no produce una actualización.

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

Ejecute la página.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Haga clic en los vínculos de paginación en distintos criterios de ordenación para comprobar que la paginación funciona correctamente. A continuación, escriba una cadena de búsqueda e intente llevar a cabo la paginación de nuevo, para comprobar que la paginación también funciona correctamente con filtrado y ordenación.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Crear una página about que muestre las estadísticas de los estudiantes

En la página acerca del sitio web de Contoso University, mostrará Cuántos alumnos se han inscrito para cada fecha de inscripción. Esto requiere realizar agrupaciones y cálculos sencillos en los grupos. Para conseguirlo, haga lo siguiente:

- Cree una clase de modelo de vista para los datos que necesita pasar a la vista.
- Modifique el método `About` en el controlador de `Home`.
- Modifique la vista de `About`.

### <a name="create-the-view-model"></a>Crear el modelo de vista

Cree una carpeta *ViewModels* . En esa carpeta, agregue un archivo de clase *EnrollmentDateGroup.CS* y reemplace el código existente por el código siguiente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Modificación del controlador Home

En *HomeController.CS*, agregue las siguientes instrucciones `using` en la parte superior del archivo:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Agregue una variable de clase para el contexto de la base de datos inmediatamente después de la llave de apertura de la clase:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

Reemplace el método `About` con el código siguiente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

La instrucción LINQ agrupa las entidades de alumnos por fecha de inscripción, calcula la cantidad de entidades que se incluyen en cada grupo y almacena los resultados en una colección de objetos de modelo de la vista `EnrollmentDateGroup`.

Agregue un método `Dispose`:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Modificación de la vista About

Reemplace el código del archivo *Views\Home\About.cshtml* por el código siguiente:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

Ejecute la aplicación y haga clic en el vínculo **About (acerca de** ). En una tabla se muestra el número de alumnos para cada fecha de inscripción.

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

## <a name="optional-deploy-the-app-to-windows-azure"></a>Opcional: implementar la aplicación en Windows Azure

Hasta ahora, la aplicación se ejecuta localmente en IIS Express en el equipo de desarrollo. Para que esté disponible para que otras personas puedan utilizarla a través de Internet, debe implementarla en un proveedor de hospedaje Web. En esta sección opcional del tutorial, la implementará en un sitio web de Windows Azure.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Usar Migraciones de Code First para implementar la base de datos

Para implementar la base de datos, usará Migraciones de Code First. Al crear el perfil de publicación que se usa para configurar las opciones de implementación desde Visual Studio, se activará una casilla de verificación con la etiqueta **ejecutar migraciones de Code First (se ejecuta al iniciar la aplicación)** . Esta configuración hace que el proceso de implementación configure automáticamente el archivo *Web. config* de la aplicación en el servidor de destino para que Code First utilice la clase de inicializador `MigrateDatabaseToLatestVersion`.

Visual Studio no realiza ninguna acción con la base de datos durante el proceso de implementación. Cuando la aplicación implementada tiene acceso a la base de datos por primera vez después de la implementación, Code First crea automáticamente la base de datos o actualiza el esquema de la base de datos a la versión más reciente. Si la aplicación implementa un método de `Seed` migraciones, el método se ejecuta después de que se cree la base de datos o se actualice el esquema.

El método Migrations `Seed` inserta datos de prueba. Si estuviera implementando en un entorno de producción, tendría que cambiar el método de `Seed` para que solo inserte los datos que desea insertar en la base de datos de producción. Por ejemplo, en el modelo de datos actual podría querer tener cursos reales pero estudiantes ficticios en la base de datos de desarrollo. Puede escribir un método `Seed` para cargarlo tanto en el entorno de desarrollo como para, a continuación, comentar los estudiantes ficticios antes de realizar la implementación en producción. O bien, puede escribir un método `Seed` para cargar solo los cursos, y especificar los estudiantes ficticios en la base de datos de prueba manualmente mediante la interfaz de usuario de la aplicación.

### <a name="get-a-windows-azure-account"></a>Obtener una cuenta de Windows Azure

Necesitará una cuenta de Windows Azure. Si aún no tiene una, puede crear una cuenta de evaluación gratuita en un par de minutos. Para obtener más información, consulte [evaluación gratuita de Windows Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

### <a name="create-a-web-site-and-a-sql-database-in-windows-azure"></a>Crear un sitio web y una base de datos SQL en Windows Azure

El sitio web de Windows Azure se ejecutará en un entorno de hospedaje compartido, lo que significa que se ejecuta en máquinas virtuales (VM) que se comparten con otros clientes de Windows Azure. Un entorno de hospedaje compartido es un método de bajo costo para empezar a trabajar en la nube. Más adelante, si el tráfico web aumenta, la aplicación se puede escalar para satisfacer la necesidad de ejecutar en máquinas virtuales dedicadas. Si necesita una arquitectura más compleja, puede migrar a un servicio en la nube de Windows Azure. Los servicios en la nube se ejecutan en máquinas virtuales dedicadas que se pueden configurar según sus necesidades.

Windows Azure SQL Database es un servicio de base de datos relacional basado en la nube que se basa en SQL Server Technologies. Las herramientas y aplicaciones que funcionan con SQL Server también funcionan con SQL Database.

1. En el [portal de administración de Windows Azure](https://manage.windowsazure.com/), haga clic en **sitios web** en la pestaña de la izquierda y, a continuación, haga clic en **nuevo**.

    ![Botón nuevo en Portal de administración](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)
2. Haga clic en **creación personalizada**.

    ![Crear con vínculo de base de datos en Portal de administración](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

   Se abre el Asistente para **crear nuevo sitio web** .
3. En el paso **nuevo sitio web** del asistente, escriba una cadena en el cuadro **dirección URL** que se usará como dirección URL única para la aplicación. La dirección URL completa consistirá en lo que escriba aquí y el sufijo que aparece junto al cuadro de texto. En la ilustración se muestra "ConU", pero es probable que esta dirección URL se tome, por lo que tendrá que elegir una diferente.

    ![Crear con vínculo de base de datos en Portal de administración](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)
4. En la lista desplegable **región** , elija una región cercana. Esta configuración especifica en qué centro de datos se ejecutará el sitio Web.
5. En la lista desplegable **base de datos** , elija **crear una base de datos SQL gratuita de 20 MB**.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)
6. En el **nombre**de la cadena de conexión de base de BD, escriba *SchoolContext*.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)
7. Haga clic en la flecha que apunta a la derecha en la parte inferior del cuadro. El asistente avanza al paso de configuración de la **base de datos** .
8. En el cuadro **nombre** , escriba *ContosoUniversityDB*.
9. En el cuadro **servidor** , seleccione **nuevo SQL Database servidor**. Como alternativa, si creó anteriormente un servidor, puede seleccionar ese servidor en la lista desplegable.
10. Escriba un nombre y una **contraseña**de **Inicio de sesión** de administrador. Si seleccionó **nuevo servidor de SQL Database** no está escribiendo un nombre y una contraseña existentes aquí, escriba un nombre y una contraseña nuevos que se van a definir para usarlos más adelante cuando se tenga acceso a la base de datos. Si ha seleccionado un servidor que ha creado anteriormente, escribirá las credenciales para ese servidor. En este tutorial, no activará la casilla ***Opciones avanzadas*** . Las opciones ***avanzadas*** permiten establecer la [Intercalación](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx)de base de datos.
11. Elija la misma **región** que eligió para el sitio Web.
12. Haga clic en la marca de verificación situada en la parte inferior derecha del cuadro para indicar que ha terminado.   
  
    ![Paso de configuración de base de datos del nuevo sitio web: Asistente crear con base de datos](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image16.png)  

    En la imagen siguiente se muestra el uso de un SQL Server e inicio de sesión existente.   
  
    ![Paso de configuración de base de datos del nuevo sitio web: Asistente crear con base de datos](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image17.png)  
  
    La Portal de administración vuelve a la página sitios web y la columna **Estado** muestra que se está creando el sitio. Después de un tiempo (normalmente menos de un minuto), la columna **Estado** muestra que el sitio se ha creado correctamente. En la barra de navegación de la izquierda, el número de sitios que tiene en su cuenta aparece junto al icono **sitios web** y el número de bases de datos aparece al lado del icono **bases de datos SQL** .

## <a name="deploy-the-application-to-windows-azure"></a>Implementar la aplicación en Windows Azure

1. En Visual Studio, haga clic con el botón derecho en el proyecto en **Explorador de soluciones** y seleccione **publicar** en el menú contextual.  
  
    ![Publicar en el menú contextual del proyecto](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image18.png)
2. En la pestaña **perfil** del Asistente para **publicación web** , haga clic en **importar**.  
  
    ![Importar configuración de publicación](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image19.png)
3. Si no ha agregado previamente su suscripción de Windows Azure en Visual Studio, realice los pasos siguientes. En estos pasos, agregará su suscripción para que la lista desplegable que se encuentra bajo **Importar desde un sitio web de Windows Azure** incluya el sitio Web.

    a. En el cuadro de diálogo **importar Perfil de publicación** , haga clic en **Importar desde un sitio web de Windows Azure**y, a continuación, haga clic en **Agregar suscripción de Windows Azure**.

    ![Agregar suscripción de Windows Azure](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image20.png)

    b. En el cuadro de diálogo **importar suscripciones de Windows Azure** , haga clic en **Descargar archivo de suscripción**.

    ![Descargar archivo de suscripción](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image21.png)

    c. En la ventana del explorador, guarde el archivo *. publishsettings* .

    ![descarga del archivo. publishsettings](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image22.png)

    > [!WARNING]
    > Seguridad: el archivo *publishsettings* contiene las credenciales (sin codificar) que se usan para administrar sus servicios y suscripciones de Windows Azure. El procedimiento recomendado de seguridad para este archivo consiste en almacenarlo temporalmente fuera de los directorios de origen (por ejemplo, en la carpeta *bibliotecas\documentos* ) y, a continuación, eliminarlo una vez completada la importación. Un usuario malintencionado que obtenga acceso al archivo `.publishsettings` puede editar, crear y eliminar sus servicios de Windows Azure.

    d. En el cuadro de diálogo **importar suscripciones de Windows Azure** , haga clic en **examinar** y navegue hasta el archivo *. publishsettings* .

    ![descargar sub](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image23.png)

    e. Haga clic en **importar**.

    ![importación](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image24.png)
4. En el cuadro de diálogo **importar Perfil de publicación** , seleccione **Importar desde un sitio web de Windows Azure**, seleccione el sitio web en la lista desplegable y, a continuación, haga clic en **Aceptar**.  
  
    ![Importar Perfil de publicación](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image25.png)
5. En la pestaña **conexión** , haga clic en **validar conexión** para asegurarse de que la configuración sea correcta.  
  
    ![Validar conexión](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)
6. Cuando se ha validado la conexión, se muestra una marca de verificación verde junto al botón **validar conexión** . Haga clic en **Siguiente**.  
  
    ![La conexión se validó correctamente](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image27.png)
7. Abra la lista desplegable **cadena de conexión remota** en **SchoolContext** y seleccione la cadena de conexión para la base de datos que creó.
8. Seleccione **ejecutar migraciones de Code First (se ejecuta al iniciar la aplicación)** .
9. Desactive **usar esta cadena de conexión en tiempo de ejecución** para el **UserContext (DefaultConnection)** , ya que esta aplicación no está usando la base de datos de pertenencia.   
  
    ![Pestaña Configuración](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image28.png)
10. Haga clic en **Siguiente**.
11. En la pestaña **vista previa** , haga clic en **iniciar vista previa**.  
  
    ![Botón StartPreview en la pestaña vista previa](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image29.png)  
  
    En la pestaña se muestra una lista de los archivos que se copiarán en el servidor. No es necesario mostrar la vista previa para publicar la aplicación, pero es una función útil que se debe tener en cuenta. En este caso, no es necesario hacer nada con la lista de archivos que se muestra. La próxima vez que implemente esta aplicación, solo los archivos que han cambiado estarán en esta lista.  
  
    ![Salida del archivo StartPreview](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image30.png)
12. Haga clic en **Publicar**.  
    Visual Studio comienza el proceso de copia de los archivos en el servidor de Windows Azure.
13. La ventana **salida** muestra las acciones de implementación que se realizaron e informa de la finalización correcta de la implementación.  
  
    ![Ventana de salida que informa de una implementación correcta](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image31.png)
14. Tras una implementación correcta, el explorador predeterminado se abre automáticamente en la dirección URL del sitio Web implementado.  
    La aplicación que ha creado se ejecuta ahora en la nube. Haga clic en la pestaña Students.  
  
    ![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image32.png)

En este momento, la base de datos de *SchoolContext* se ha creado en la Azure SQL Database de Windows porque seleccionó **Ejecutar migraciones de Code First (se ejecuta al iniciar la aplicación)** . El archivo *Web. config* del sitio Web implementado se ha cambiado para que el inicializador [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) se ejecutara la primera vez que el código lea o escriba datos en la base de datos (que se produjera al seleccionar la pestaña **Students** ):

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image33.png)

El proceso de implementación también ha creado una nueva cadena de conexión *(SchoolContext\_DatabasePublish*) para que migraciones de Code First la pueda usar para actualizar el esquema de la base de datos y para inicializar la base de datos.

![Database_Publish cadena de conexión](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image34.png)

La cadena de conexión *DefaultConnection* es para la base de datos de pertenencia (que no se usa en este tutorial). La cadena de conexión *SchoolContext* es para la base de datos ContosoUniversity.

Puede encontrar la versión implementada del archivo Web. config en su propio equipo en *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Puede tener acceso al propio archivo *Web. config* implementado mediante FTP. Para obtener instrucciones, consulte [ASP.net web Deployment Using Visual Studio: Deploying a Code Update](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Siga las instrucciones que comienzan con "para usar una herramienta de FTP, necesita tres cosas: la dirección URL de FTP, el nombre de usuario y la contraseña".

> [!NOTE]
> La aplicación web no implementa la seguridad, por lo que cualquiera que encuentre la dirección URL puede cambiar los datos. Para obtener instrucciones sobre cómo proteger el sitio web, vea [implementación de una aplicación de ASP.NET MVC segura con pertenencia, OAuth y SQL Database en un sitio web de Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Puede impedir que otras personas usen el sitio mediante el Portal de administración de Windows Azure o **Explorador de servidores** en Visual Studio para detener el sitio.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image35.png)

## <a name="code-first-initializers"></a>Inicializadores de Code First

En la sección implementación vio el inicializador [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) que se estaba usando. Code First también proporciona otros inicializadores que puede usar, como [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (valor predeterminado), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) y [DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). El inicializador de `DropCreateAlways` puede ser útil para configurar las condiciones de las pruebas unitarias. También puede escribir sus propios inicializadores, y puede llamar a un inicializador explícitamente si no desea esperar hasta que la aplicación lea o escriba en la base de datos. Para obtener una explicación detallada de los inicializadores, vea el capítulo 6 del libro [programming Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) by Julie Lerman y Rowan Miller.

## <a name="summary"></a>Resumen

En este tutorial, ha visto cómo crear un modelo de datos e implementar la funcionalidad básica CRUD, ordenación, filtrado, paginación y agrupación. En el siguiente tutorial, comenzará a examinar temas más avanzados expandiendo el modelo de datos.

Los vínculos a otros recursos de Entity Framework pueden encontrarse en la [asignación de contenido de ASP.net Data Access](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [Siguiente](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
