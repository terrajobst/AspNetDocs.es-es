---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Recuperar y Mostrar datos con el enlace de modelos y formularios Web Forms | Microsoft Docs
author: Rick-Anderson
description: En esta serie de tutoriales se muestran los aspectos básicos del uso del enlace de modelos con un proyecto de formularios Web Forms ASP.NET. El enlace de modelos hace que la interacción de datos sea más recta-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: 81cca22cb4752d071d2a68986ae9ac2bed737594
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74633172"
---
# <a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>Recuperar y Mostrar datos con el enlace de modelos y formularios Web Forms

> En esta serie de tutoriales se muestran los aspectos básicos del uso del enlace de modelos con un proyecto de formularios Web Forms ASP.NET. El enlace de modelos hace que la interacción de datos sea más directa que tratar con objetos de origen de datos (como ObjectDataSource o SqlDataSource). Esta serie comienza con material introductorio y se traslada a conceptos más avanzados en los tutoriales posteriores.
> 
> El patrón de enlace de modelos funciona con cualquier tecnología de acceso a datos. En este tutorial, usará Entity Framework, pero puede usar la tecnología de acceso a datos que le resulte más familiar. En un control de servidor enlazado a datos, como un control GridView, ListView, DetailsView o FormView, se especifican los nombres de los métodos que se van a usar para seleccionar, actualizar, eliminar y crear datos. En este tutorial, va a especificar un valor para SelectMethod. 
> 
> Dentro de ese método, se proporciona la lógica para recuperar los datos. En el siguiente tutorial, establecerá los valores de UpdateMethod, DeleteMethod y InsertMethod.
>
> Puede [Descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o Visual Basic. El código descargable funciona con Visual Studio 2012 y versiones posteriores. Usa la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2017 que se muestra en este tutorial.
> 
> En el tutorial, ejecute la aplicación en Visual Studio. También puede implementar la aplicación en un proveedor de hospedaje y hacer que esté disponible a través de Internet. Microsoft ofrece hospedaje web gratuito para hasta 10 sitios web en un  
> [cuenta de evaluación gratuita de Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604). Para obtener información sobre cómo implementar un proyecto Web de Visual Studio en Azure App Service Web Apps, consulte [implementación web de ASP.net con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) series. En este tutorial también se muestra cómo usar Migraciones de Entity Framework Code First para implementar la base de datos SQL Server en Azure SQL Database.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> - Microsoft Visual Studio 2017 o Microsoft Visual Studio Community 2017
>   
> Este tutorial también funciona con Visual Studio 2012 y Visual Studio 2013, pero hay algunas diferencias en la interfaz de usuario y la plantilla de proyecto.

## <a name="what-youll-build"></a>Lo que va a compilar

En este tutorial, hará lo siguiente:

* Cree objetos de datos que reflejen una Universidad con estudiantes inscritos en cursos
* Crear tablas de base de datos a partir de objetos
* Rellenar la base de datos con datos de prueba
* Mostrar datos en un formulario Web Forms

## <a name="create-the-project"></a>Crear el proyecto

1. En Visual Studio 2017, cree un proyecto de **aplicación Web de ASP.net (.NET Framework)** denominado **ContosoUniversityModelBinding**.

   ![crear proyecto](retrieving-data/_static/image19.png)

2. Seleccione **Aceptar**. Aparece el cuadro de diálogo para seleccionar una plantilla.

   ![seleccionar formularios Web Forms](retrieving-data/_static/image3.png)

3. Seleccione la plantilla de **formularios Web Forms** . 

4. Si es necesario, cambie la autenticación a **cuentas de usuario individuales**. 

5. Seleccione **Aceptar** para crear el proyecto.

## <a name="modify-site-appearance"></a>Modificar la apariencia del sitio

   Realice algunos cambios para personalizar la apariencia del sitio. 
   
   1. Abra el archivo site. Master.
   
   2. Cambie el título para que muestre **contoso University** y no **la aplicación ASP.net**.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

   3. Cambie el texto del encabezado del nombre de la **aplicación** a **contoso University**.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

   4. Cambie los vínculos del encabezado de navegación a los apropiados para el sitio. 
   
      Quite los vínculos de **acerca** de y **póngase en contacto** con y, en su lugar, vincule a una página de **estudiantes** , que creará.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample3.aspx)]

   5. Guarde site. Master.

## <a name="add-a-web-form-to-display-student-data"></a>Agregar un formulario web para mostrar los datos de los estudiantes

   1. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto, seleccione **Agregar** y, a continuación, **nuevo elemento**. 
   
   2. En el cuadro de diálogo **Agregar nuevo elemento** , seleccione el **formulario web con la plantilla de página maestra** y asígnele el nombre **Students. aspx**.

      ![Crear página](retrieving-data/_static/image5.png)

   3. Seleccione **Agregar**.
   
   4. En la página maestra del formulario web, seleccione **site. Master**.
   
   5. Seleccione **Aceptar**.

## <a name="add-the-data-model"></a>Agregar el modelo de datos

En la carpeta **Models** , agregue una clase denominada **UniversityModels.CS**.

   1. Haga clic con el botón derecho en **modelos**, seleccione **Agregar**y, a continuación, **nuevo elemento**. Aparecerá el cuadro de diálogo **Agregar nuevo elemento**.

   2. En el menú de navegación izquierdo, seleccione **código**y, a continuación, **clase**.

      ![crear clase de modelo](retrieving-data/_static/image20.png)

   3. Asigne a la clase el nombre **UniversityModels.CS** y seleccione **Agregar**.

      En este archivo, defina las clases `SchoolContext`, `Student`, `Enrollment`y `Course` de la siguiente manera:

      [!code-csharp[Main](retrieving-data/samples/sample4.cs)]

      La clase `SchoolContext` deriva de `DbContext`, que administra la conexión a la base de datos y los cambios en los datos.

      En la clase `Student`, observe los atributos que se aplican a las propiedades `FirstName`, `LastName`y `Year`. En este tutorial se usan estos atributos para la validación de datos. Para simplificar el código, solo estas propiedades se marcan con los atributos de validación de datos. En un proyecto real, aplicaría atributos de validación a todas las propiedades que necesitan validación.

   4. Guarde UniversityModels.cs.

## <a name="set-up-the-database-based-on-classes"></a>Configurar la base de datos basada en clases

En este tutorial se usa [migraciones de Code First](https://docs.microsoft.com/ef/ef6/modeling/code-first/migrations/) para crear objetos y tablas de base de datos. Estas tablas almacenan información acerca de los estudiantes y sus cursos.

   1. Seleccione **herramientas** > **Administrador de paquetes NuGet** > **consola del administrador de paquetes**.

   2. En la **consola del administrador de paquetes**, ejecute este comando:  
      `enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

      Si el comando se completa correctamente, aparece un mensaje que indica que se han habilitado las migraciones.

      ![habilitar migraciones](retrieving-data/_static/image8.png)

      Observe que se ha creado un archivo denominado *Configuration.CS* . La clase `Configuration` tiene un método `Seed`, que puede rellenar previamente las tablas de base de datos con datos de prueba.

## <a name="pre-populate-the-database"></a>Rellenar previamente la base de datos

   1. Abra Configuration.cs.
   
   2. Agregue el código siguiente al método `Seed` . Además, agregue una instrucción `using` para el espacio de nombres `ContosoUniversityModelBinding. Models`.

      [!code-csharp[Main](retrieving-data/samples/sample5.cs)]

   3. Guarde Configuration.cs.

   4. En la consola del administrador de paquetes, ejecute el comando **Add-Migration Initial**.

   5. Ejecute el comando **Update-Database**.

      Si recibe una excepción al ejecutar este comando, los valores `StudentID` y `CourseID` pueden ser diferentes de los valores del método `Seed`. Abra esas tablas de base de datos y busque los valores existentes para `StudentID` y `CourseID`. Agregue esos valores al código para inicializar la tabla `Enrollments`.

## <a name="add-a-gridview-control"></a>Agregar un control GridView

Con datos de base de datos rellenados, ahora está listo para recuperar los datos y mostrarlos. 

1. Abra Students. aspx.

2. Busque el marcador de posición `MainContent`. Dentro de ese marcador de posición, agregue un control **GridView** que incluya este código.

   [!code-aspx-csharp[Main](retrieving-data/samples/sample6.aspx)]

   Aspectos que se deben tener en cuenta:
   * Observe el valor establecido para la propiedad `SelectMethod` en el elemento GridView. Este valor especifica el método usado para recuperar los datos de GridView, que se crean en el paso siguiente. 
   
   * La propiedad `ItemType` se establece en la clase `Student` creada anteriormente. Esta configuración le permite hacer referencia a las propiedades de clase en el marcado. Por ejemplo, la clase `Student` tiene una colección denominada `Enrollments`. Puede usar `Item.Enrollments` para recuperar esa colección y, a continuación, utilizar la [Sintaxis de LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) para recuperar la suma de los créditos inscritos de cada estudiante.
   
3. Guarde Students. aspx.

## <a name="add-code-to-retrieve-data"></a>Agregar código para recuperar datos

   En el archivo de código subyacente Students. aspx, agregue el método especificado para el valor `SelectMethod`. 
   
   1. Abra Students.aspx.cs.
   
   2. Agregue `using` instrucciones para los espacios de nombres `ContosoUniversityModelBinding. Models` y `System.Data.Entity`.

      [!code-csharp[Main](retrieving-data/samples/sample7.cs)]

   3. Agregue el método especificado para `SelectMethod`:

      [!code-csharp[Main](retrieving-data/samples/sample8.cs)]

      La cláusula `Include` mejora el rendimiento de las consultas, pero no es necesario. Sin la cláusula `Include`, los datos se recuperan mediante la [*carga diferida*](https://en.wikipedia.org/wiki/Lazy_loading), lo que implica el envío de una consulta independiente a la base de datos cada vez que se recuperan los datos relacionados. Con la cláusula `Include`, los datos se recuperan mediante la *carga diligente*, lo que significa que una consulta de base de datos única recupera todos los datos relacionados. Si no se usan datos relacionados, la carga diligente es menos eficaz porque se recuperan más datos. Sin embargo, en este caso, la carga diligente le ofrece el mejor rendimiento, ya que los datos relacionados se muestran para cada registro.

      Para obtener más información sobre las consideraciones de rendimiento al cargar datos relacionados, consulte la sección sobre la **carga diferida, diligente y explícita de datos relacionados** en el artículo sobre la [lectura de datos relacionados con el Entity Framework en una aplicación ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) .

      De forma predeterminada, los datos se ordenan por los valores de la propiedad marcada como la clave. Puede Agregar una cláusula `OrderBy` para especificar un valor de ordenación diferente. En este ejemplo, la propiedad `StudentID` predeterminada se usa para la ordenación. En el artículo [ordenación, paginación y filtrado de datos](sorting-paging-and-filtering-data.md) , el usuario está habilitado para seleccionar una columna para la ordenación.
 
   4. Guarde Students.aspx.cs.

## <a name="run-your-application"></a>Ejecutar la aplicación 

Ejecute la aplicación web (**F5**) y vaya a la página **Students** , que muestra lo siguiente:

   ![Mostrar datos](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>Generación automática de métodos de enlace de modelos

Al trabajar en esta serie de tutoriales, simplemente puede copiar el código del tutorial al proyecto. Sin embargo, una desventaja de este enfoque es que es posible que no sea consciente de la característica proporcionada por Visual Studio para generar automáticamente código para los métodos de enlace de modelos. Cuando trabaje en sus propios proyectos, la generación automática de código puede ahorrarle tiempo y ayudarle a tener una idea de cómo implementar una operación. En esta sección se describe la característica de generación automática de código. Esta sección solo es informativa y no contiene ningún código que deba implementar en el proyecto. 

Al establecer un valor para las propiedades `SelectMethod`, `UpdateMethod`, `InsertMethod`o `DeleteMethod` en el código de marcado, puede seleccionar la opción **crear nuevo método** .

![crear un método](retrieving-data/_static/image18.png)

Visual Studio no solo crea un método en el código subyacente con la firma correcta, sino que también genera código de implementación para realizar la operación. Si primero establece la propiedad `ItemType` antes de utilizar la característica de generación automática de código, el código generado utiliza ese tipo para las operaciones. Por ejemplo, al establecer la propiedad `UpdateMethod`, se genera automáticamente el código siguiente:

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

De nuevo, no es necesario agregar este código al proyecto. En el siguiente tutorial, implementará métodos para actualizar, eliminar y agregar nuevos datos.

## <a name="summary"></a>Resumen

En este tutorial, ha creado clases de modelo de datos y ha generado una base de datos a partir de esas clases. Ha rellenado las tablas de base de datos con datos de prueba. Usó el enlace de modelos para recuperar datos de la base de datos y, a continuación, muestra los datos en un control GridView.

En el siguiente [tutorial](updating-deleting-and-creating-data.md) de esta serie, habilitará la actualización, la eliminación y la creación de datos.

> [!div class="step-by-step"]
> [Siguiente](updating-deleting-and-creating-data.md)
