---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Agregando un nuevo campo | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: d79655bfadff83095bf4cb84445f5efaf44d6a89
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519081"
---
# <a name="adding-a-new-field"></a>Adición de un nuevo campo

por [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](index.md)]

En esta sección, usará Migraciones de Entity Framework Code First para migrar algunos cambios a las clases del modelo para que el cambio se aplique en la base de datos.

De forma predeterminada, cuando se utiliza Entity Framework Code First para crear automáticamente una base de datos, como hizo anteriormente en este tutorial, Code First agrega una tabla a la base de datos para realizar un seguimiento de si el esquema de la base de datos está sincronizado con las clases de modelo desde las que se generó. Si no están sincronizados, el Entity Framework produce un error. Esto facilita el seguimiento de los problemas en el tiempo de desarrollo que, de otra forma, podría encontrar (mediante errores ocultos) en tiempo de ejecución.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Configuración de Migraciones de Code First para los cambios de modelo

Vaya a Explorador de soluciones. Haga clic con el botón derecho en el archivo *movies. MDF* y seleccione **eliminar** para quitar la base de datos de películas. Si no ve el archivo *movies. MDF* , haga clic en el icono **Mostrar todos los archivos** que se muestra a continuación en el contorno rojo.

![](adding-a-new-field/_static/image1.png)

Compile la aplicación para asegurarse de que no hay ningún error.

En el menú **Tools** (Herramientas), haga clic en **Administrador de paquetes NuGet** y luego en **Consola del Administrador de paquetes**.

![Agregar el paquete Man](adding-a-new-field/_static/image2.png)

En la ventana de la **consola del administrador de paquetes** , en el símbolo del sistema `PM>` escriba

Enable-Migrations-ContextTypeName MvcMovie. Models. MovieDBContext

![](adding-a-new-field/_static/image3.png)

El comando **enable-Migrations** (mostrado anteriormente) crea un archivo *Configuration.CS* en una nueva carpeta *Migrations* .

![](adding-a-new-field/_static/image4.png)

Visual Studio abre el archivo *Configuration.CS* . Reemplace el método `Seed` del archivo *Configuration.CS* por el código siguiente:

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

Mantenga el mouse sobre la línea ondulada roja en `Movie`, haga clic en `Show Potential Fixes` y, a continuación, haga clic en **usar** **MvcMovie. Models;**

![](adding-a-new-field/_static/image5.png)

Al hacerlo, se agrega la siguiente instrucción using:

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> Migraciones de Code First llama al método `Seed` después de cada migración (es decir, al llamar a **Update-Database** en la consola del administrador de paquetes) y este método actualiza las filas que ya se han insertado o las inserta si aún no existen.
> 
> El método [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) del código siguiente realiza una operación "Upsert":
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> Dado que el método de [inicialización](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) se ejecuta con cada migración, no se pueden insertar datos, ya que las filas que está intentando agregar ya estarán allí después de la primera migración que crea la base de datos. La operación "[Upsert](http://en.wikipedia.org/wiki/Upsert)" evita errores que podrían producirse si se intenta insertar una fila que ya existe, pero reemplaza cualquier cambio en los datos que haya realizado al probar la aplicación. En el caso de los datos de prueba de algunas tablas, es posible que no desee que suceda: en algunos casos, cuando cambie los datos mientras realiza las pruebas desea que los cambios permanezcan después de las actualizaciones de la base de datos. En ese caso, desea realizar una operación de inserción condicional: Inserte una fila solo si aún no existe.   
> 
> El primer parámetro que se pasa al método [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) especifica la propiedad que se va a usar para comprobar si ya existe una fila. En el caso de los datos de película de prueba que se proporcionan, se puede usar la propiedad `Title` para este propósito, ya que cada título de la lista es único:
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> Este código supone que los títulos son únicos. Si agrega manualmente un título duplicado, recibirá la siguiente excepción la próxima vez que realice una migración.   
> 
> *La secuencia contiene más de un elemento*  
> 
> Para obtener más información acerca del método [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) , consulte el método de encargarse [con EF 4,3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/).

**Presione Ctrl-Mayús-B para compilar el proyecto.** (Se producirá un error en los pasos siguientes si no se compila en este momento).

El siguiente paso consiste en crear una clase `DbMigration` para la migración inicial. Esta migración crea una nueva base de datos, por eso se ha eliminado el archivo *Movie. MDF* en un paso anterior.

En la ventana de la **consola del administrador de paquetes** , escriba el comando `add-migration Initial` para crear la migración inicial. El nombre "Initial" es arbitrario y se utiliza para asignar un nombre al archivo de migración creado.

![](adding-a-new-field/_static/image6.png)

Migraciones de Code First crea otro archivo de clase en la carpeta *Migrations* (con el nombre *{fecha}\_Initial.CS* ) y esta clase contiene código que crea el esquema de la base de datos. El nombre de archivo de la migración se ha fijado previamente con una marca de tiempo para ayudar con la ordenación. Examine el archivo *{fecha}\_Initial.CS* , que contiene las instrucciones para crear la tabla de `Movies` de la base de la película. Al actualizar la base de datos en las instrucciones siguientes, se ejecutará este archivo *{fecha}\_Initial.CS* y se creará el esquema de la base de datos. A continuación, el método de **inicialización** se ejecutará para rellenar la base de datos con datos de prueba.

En la **consola del administrador de paquetes**, escriba el comando `update-database` para crear la base de datos y ejecutar el método `Seed`.

![](adding-a-new-field/_static/image7.png)

Si recibe un error que indica que una tabla ya existe y no se puede crear, es probable que haya ejecutado la aplicación después de eliminar la base de datos y antes de ejecutar `update-database`. En ese caso, elimine el archivo *movies. MDF* de nuevo y vuelva a intentar el comando `update-database`. Si sigue apareciendo un error, elimine la carpeta migraciones y el contenido y, a continuación, comience con las instrucciones que se indican en la parte superior de esta página (es decir, elimine el archivo *movies. MDF* y continúe con habilitar-migraciones). Si sigue apareciendo un error, abra Explorador de objetos de SQL Server y quite la base de datos de la lista.

Ejecute la aplicación y vaya a la dirección URL de */Movies* . Se muestran los datos de inicialización.

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Adición de una propiedad de clasificación al modelo Movie

Comience agregando una nueva propiedad `Rating` a la clase `Movie` existente. Abra el archivo *Models\Movie.CS* y agregue la propiedad `Rating` como esta:

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

La clase `Movie` completa ahora es similar al código siguiente:

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

Compile la aplicación (Ctrl + Mayús + B).

Dado que ha agregado un nuevo campo a la `Movie` (clase), también debe actualizar el enlace *lista de permitidos* por lo que se incluirá esta nueva propiedad. Actualice el atributo `bind` de `Create` y `Edit` métodos de acción para incluir la propiedad `Rating`:

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

También debe actualizar las plantillas de vista para mostrar, crear y editar la nueva propiedad `Rating` en la vista del explorador.

Abra el archivo *\Views\Movies\Index.cshtml* y agregue un encabezado de columna `<th>Rating</th>` justo después de la columna **Price** . A continuación, agregue una `<td>` columna cerca del final de la plantilla para representar el valor `@item.Rating`. A continuación se muestra el aspecto de la plantilla de vista *index. cshtml* actualizada:

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

A continuación, abra el archivo *\Views\Movies\Create.cshtml* y agregue el campo `Rating` con el siguiente marcado resaltado. Esto representa un cuadro de texto para que pueda especificar una clasificación cuando se crea una nueva película.

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

Ahora ha actualizado el código de aplicación para admitir la nueva propiedad `Rating`.

Ejecute la aplicación y vaya a la dirección URL de */Movies* . Sin embargo, cuando lo haga, verá uno de los siguientes errores:

![](adding-a-new-field/_static/image9.png)  
  
El modelo de respaldo del contexto ' MovieDBContext ' ha cambiado desde que se creó la base de datos. Considere la posibilidad de usar Migraciones de Code First para actualizar la base de datos (https://go.microsoft.com/fwlink/?LinkId=238269).

![](adding-a-new-field/_static/image10.png)

Está viendo este error porque la clase del modelo de `Movie` actualizada en la aplicación es ahora diferente del esquema de la tabla de `Movie` de la base de datos existente. (No hay ninguna columna `Rating` en la tabla de la base de datos).

Este error se puede resolver de varias maneras:

1. Haga que Entity Framework quite de forma automática la base de datos y la vuelva a crear basándose en el nuevo esquema de la clase del modelo. Este enfoque resulta muy conveniente al principio del ciclo de desarrollo cuando se está realizando el desarrollo activo en una base de datos de prueba; permite desarrollar rápidamente el esquema del modelo y la base de datos juntos. Sin embargo, el inconveniente es que se pierden los datos existentes en la base de datos, por lo que *no* se desea usar este enfoque en una base de datos de producción. Usar un inicializador para inicializar automáticamente una base de datos con datos de prueba suele ser una manera productiva de desarrollar una aplicación. Para obtener más información sobre Entity Framework inicializadores de base de datos, vea el [tutorial de ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Modifique explícitamente el esquema de la base de datos existente para que coincida con las clases del modelo. La ventaja de este enfoque es que se conservan los datos. Puede realizar este cambio de forma manual o mediante la creación de un script de cambio de base de datos.
3. Use Migraciones de Code First para actualizar el esquema de la base de datos.

Para este tutorial se usa Migraciones de Code First.

Actualice el método de inicialización para que proporcione un valor para la nueva columna. Abra el archivo Migrations\Configuration.cs y agregue un campo rating a cada objeto Movie.

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

Compile la solución y, a continuación, abra la ventana de la **consola del administrador de paquetes** y escriba el siguiente comando:

`add-migration Rating`

El comando `add-migration` indica al marco de migración que examine el modelo de película actual con el esquema de la base de la película actual y cree el código necesario para migrar la base de BD al nuevo modelo. La *clasificación* del nombre es arbitraria y se usa para asignar un nombre al archivo de migración. Resulta útil usar un nombre descriptivo para el paso de migración.

Cuando finaliza este comando, Visual Studio abre el archivo de clase que define el nuevo `DbMigration` clase derivada y, en el método `Up`, puede ver el código que crea la nueva columna.

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

Compile la solución y, a continuación, escriba el comando `update-database` en la ventana de la **consola del administrador de paquetes** .

La siguiente imagen muestra la salida en la ventana de la **consola del administrador de paquetes** (la *clasificación* de fecha pendiente de la marca de fecha será diferente).

![](adding-a-new-field/_static/image11.png)

Vuelva a ejecutar la aplicación y vaya a la dirección URL de/movies. Puede ver el nuevo campo de clasificación.

![](adding-a-new-field/_static/image12.png)

Haga clic en el vínculo **crear nuevo** para agregar una nueva película. Tenga en cuenta que puede Agregar una clasificación.

![7_CreateRioII](adding-a-new-field/_static/image13.png)

Haga clic en **Crear**. La nueva película, incluida la clasificación, se muestra ahora en la lista de películas:

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

Ahora que el proyecto está usando migraciones, no tendrá que quitar la base de datos al agregar un nuevo campo o actualizar el esquema. En la siguiente sección, realizaremos más cambios en el esquema y usaremos migraciones para actualizar la base de datos.

También debe agregar el campo `Rating` a las plantillas de vista Edit, details y DELETE.

Podría escribir de nuevo el comando "Update-Database" en la ventana de la **consola del administrador de paquetes** y no se ejecutaría ningún código de migración, porque el esquema coincide con el modelo. Sin embargo, la ejecución de "Update-Database" volverá a ejecutar el método `Seed` y, si ha cambiado alguno de los datos de inicialización, se perderán los cambios porque el método de `Seed` upserts datos. Puede leer más sobre el método `Seed` en el [tutorial popular ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)de Tom Dykstra.

En esta sección vio cómo puede modificar objetos de modelo y mantener la base de datos sincronizada con los cambios. También aprendió una manera de rellenar una base de datos recién creada con datos de ejemplo para que pueda probar escenarios. Esta es solo una introducción rápida a Code First, vea [creación de un modelo de datos Entity Framework para una aplicación ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) para obtener un tutorial más completo sobre el tema. A continuación, echemos un vistazo a cómo puede Agregar una lógica de validación más enriquecida a las clases de modelo y permitir que se apliquen algunas reglas de negocios.

> [!div class="step-by-step"]
> [Anterior](adding-search.md)
> [Siguiente](adding-validation.md)
