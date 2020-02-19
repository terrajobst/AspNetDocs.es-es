---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Agregar un nuevo campo a la tabla y al modelo de películas | Microsoft Docs
author: Rick-Anderson
description: 'Nota: hay disponible una versión actualizada de este tutorial que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro, mucho más fácil de seguir y demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: d966b95163f64b20a17d2327a12c5d6c44a4a66b
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457705"
---
# <a name="adding-a-new-field-to-the-movie-model-and-table"></a>Agregar un nuevo campo a la tabla y modelo de películas

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > [Hay disponible una](../../getting-started/introduction/getting-started.md) versión actualizada de este tutorial que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro, mucho más fácil de seguir y demuestra más características.

En esta sección, usará Migraciones de Entity Framework Code First para migrar algunos cambios a las clases del modelo para que el cambio se aplique en la base de datos.

De forma predeterminada, cuando se utiliza Entity Framework Code First para crear automáticamente una base de datos, como hizo anteriormente en este tutorial, Code First agrega una tabla a la base de datos para realizar un seguimiento de si el esquema de la base de datos está sincronizado con las clases de modelo desde las que se generó. Si no están sincronizados, el Entity Framework produce un error. Esto facilita el seguimiento de los problemas en el tiempo de desarrollo que, de otra forma, podría encontrar (mediante errores ocultos) en tiempo de ejecución.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Configuración de Migraciones de Code First para los cambios de modelo

Si usa Visual Studio 2012, haga doble clic en el archivo *movies. MDF* desde explorador de soluciones para abrir la herramienta de base de datos. Visual Studio Express para web mostrará Explorador de bases de datos, Visual Studio 2012 mostrará Explorador de servidores. Si usa Visual Studio 2010, use Explorador de objetos de SQL Server.

En la herramienta de base de datos (Explorador de bases de datos, Explorador de servidores o Explorador de objetos de SQL Server), haga clic con el botón derecho en `MovieDBContext` y seleccione **eliminar** para quitar la base de datos de películas.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

Vuelva a Explorador de soluciones. Haga clic con el botón derecho en el archivo *movies. MDF* y seleccione **eliminar** para quitar la base de datos de películas.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

Compile la aplicación para asegurarse de que no hay ningún error.

En el menú **Tools** (Herramientas), haga clic en **Administrador de paquetes NuGet** y luego en **Consola del Administrador de paquetes**.

![Agregar el paquete Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

En la ventana de la **consola del administrador de paquetes** , en el símbolo del sistema de `PM>`, escriba "Enable-Migrations-ContextTypeName MvcMovie. Models. MovieDBContext".

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

El comando **enable-Migrations** (mostrado anteriormente) crea un archivo *Configuration.CS* en una nueva carpeta *Migrations* .

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

Visual Studio abre el archivo *Configuration.CS* . Reemplace el método `Seed` del archivo *Configuration.CS* por el código siguiente:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

Haga clic con el botón derecho en la línea ondulada roja en `Movie` y seleccione **resolver** y, a continuación, **use** **MvcMovie. Models;**

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

Al hacerlo, se agrega la siguiente instrucción using:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> Migraciones de Code First llama al método `Seed` después de cada migración (es decir, al llamar a **Update-Database** en la consola del administrador de paquetes) y este método actualiza las filas que ya se han insertado o las inserta si aún no existen.

**Presione Ctrl-Mayús-B para compilar el proyecto.** (Se producirá un error en los pasos siguientes si no se compila en este momento).

El siguiente paso consiste en crear una clase `DbMigration` para la migración inicial. Esta migración a crea una nueva base de datos, por eso se ha eliminado el archivo *Movie. MDF* en un paso anterior.

En la ventana de la **consola del administrador de paquetes** , escriba el comando "agregar-migración inicial" para crear la migración inicial. El nombre "Initial" es arbitrario y se utiliza para asignar un nombre al archivo de migración creado.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

Migraciones de Code First crea otro archivo de clase en la carpeta *Migrations* (con el nombre *{fecha}\_Initial.CS* ) y esta clase contiene código que crea el esquema de la base de datos. El nombre de archivo de la migración se ha fijado previamente con una marca de tiempo para ayudar con la ordenación. Examine el archivo *{fecha}\_Initial.CS* , que contiene las instrucciones para crear la tabla de películas para la base de la película. Al actualizar la base de datos en las instrucciones siguientes, se ejecutará este archivo *{fecha}\_Initial.CS* y se creará el esquema de la base de datos. A continuación, el método de **inicialización** se ejecutará para rellenar la base de datos con datos de prueba.

En la **consola del administrador de paquetes**, escriba el comando "Update-Database" para crear la base de datos y ejecutar el método de **inicialización** .

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

Si recibe un error que indica que una tabla ya existe y no se puede crear, es probable que haya ejecutado la aplicación después de eliminar la base de datos y antes de ejecutar `update-database`. En ese caso, elimine el archivo *movies. MDF* de nuevo y vuelva a intentar el comando `update-database`. Si sigue apareciendo un error, elimine la carpeta migraciones y el contenido y, a continuación, comience con las instrucciones que se indican en la parte superior de esta página (es decir, elimine el archivo *movies. MDF* y continúe con habilitar-migraciones).

Ejecute la aplicación y vaya a la dirección URL de */Movies* . Se muestran los datos de inicialización.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Adición de una propiedad de clasificación al modelo Movie

Comience agregando una nueva propiedad `Rating` a la clase `Movie` existente. Abra el archivo *Models\Movie.CS* y agregue la propiedad `Rating` como esta:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

La clase `Movie` completa ahora es similar al código siguiente:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

Compile la aplicación con el comando de menú **Compilar** &gt;**compilar película** o presionando Ctrl-Mayús-B.

Ahora que ha actualizado la clase `Model`, también debe actualizar las plantillas de vista *\Views\Movies\Index.cshtml* y *\Views\Movies\Create.cshtml* para mostrar la nueva propiedad `Rating` en la vista de explorador.

Abra el archivo<em>\Views\Movies\Index.cshtml</em> y agregue un encabezado de columna `<th>Rating</th>` justo después de la columna <strong>Price</strong> . A continuación, agregue una `<td>` columna cerca del final de la plantilla para representar el valor `@item.Rating`. A continuación se muestra el aspecto de la plantilla de vista <em>index. cshtml</em> actualizada:

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

A continuación, abra el archivo *\Views\Movies\Create.cshtml* y agregue el siguiente marcado cerca del final del formulario. Esto representa un cuadro de texto para que pueda especificar una clasificación cuando se crea una nueva película.

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

Ahora ha actualizado el código de aplicación para admitir la nueva propiedad `Rating`.

Ahora ejecute la aplicación y vaya a la dirección URL de */Movies* . Sin embargo, cuando lo haga, verá uno de los siguientes errores:

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

Está viendo este error porque la clase del modelo de `Movie` actualizada en la aplicación es ahora diferente del esquema de la tabla de `Movie` de la base de datos existente. (No hay ninguna columna `Rating` en la tabla de la base de datos).

Este error se puede resolver de varias maneras:

1. Haga que Entity Framework quite de forma automática la base de datos y la vuelva a crear basándose en el nuevo esquema de la clase del modelo. Este enfoque es muy práctico cuando se realiza el desarrollo activo en una base de datos de prueba; permite desarrollar rápidamente el modelo y el esquema de la base de datos juntos. Sin embargo, el inconveniente es que se pierden los datos existentes en la base de datos, por lo que *no* se desea usar este enfoque en una base de datos de producción. El uso de un inicializador para inicializar automáticamente una base de datos con datos de prueba suele ser una forma productiva de desarrollar una aplicación. Para obtener más información sobre Entity Framework inicializadores de base de datos, vea el [tutorial de ASP.NET MVC/Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)de Tom Dykstra.
2. Modifique explícitamente el esquema de la base de datos existente para que coincida con las clases del modelo. La ventaja de este enfoque es que se conservan los datos. Puede realizar este cambio de forma manual o mediante la creación de un script de cambio de base de datos.
3. Use Migraciones de Code First para actualizar el esquema de la base de datos.

Para este tutorial se usa Migraciones de Code First.

Actualice el método de inicialización para que proporcione un valor para la nueva columna. Abra el archivo Migrations\Configuration.cs y agregue un campo rating a cada objeto Movie.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

Compile la solución y, a continuación, abra la ventana de la **consola del administrador de paquetes** y escriba el siguiente comando:

`add-migration AddRatingMig`

El comando `add-migration` indica al marco de migración que examine el modelo de película actual con el esquema de la base de la película actual y cree el código necesario para migrar la base de BD al nuevo modelo. AddRatingMig es arbitrario y se utiliza para asignar un nombre al archivo de migración. Resulta útil usar un nombre descriptivo para el paso de migración.

Cuando finaliza este comando, Visual Studio abre el archivo de clase que define el nuevo `DbMigration` clase derivada y, en el método `Up`, puede ver el código que crea la nueva columna.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

Compile la solución y, a continuación, escriba el comando "Update-Database" en la ventana de la **consola del administrador de paquetes** .

En la imagen siguiente se muestra el resultado en la ventana de la **consola del administrador de paquetes** (la marca de fecha que preAddRatingMig es diferente).

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

Vuelva a ejecutar la aplicación y vaya a la dirección URL de/movies. Puede ver el nuevo campo de clasificación.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

Haga clic en el vínculo **crear nuevo** para agregar una nueva película. Tenga en cuenta que puede Agregar una clasificación.

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

Haga clic en **Crear**. La nueva película, incluida la clasificación, se muestra ahora en la lista de películas:

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

También debe agregar el campo `Rating` a las plantillas de vista Edit, details y SearchIndex.

Puede volver a escribir el comando "Update-Database" en la ventana de la **consola del administrador de paquetes** y no se realizarán cambios, ya que el esquema coincide con el modelo.

En esta sección vio cómo puede modificar objetos de modelo y mantener la base de datos sincronizada con los cambios. También aprendió una manera de rellenar una base de datos recién creada con datos de ejemplo para que pueda probar escenarios. A continuación, echemos un vistazo a cómo puede Agregar una lógica de validación más enriquecida a las clases de modelo y permitir que se apliquen algunas reglas de negocios.

> [!div class="step-by-step"]
> [Anterior](examining-the-edit-methods-and-edit-view.md)
> [Siguiente](adding-validation-to-the-model.md)
