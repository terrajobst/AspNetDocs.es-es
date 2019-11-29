---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Creación de un modelo de datos de Entity Framework para una aplicación ASP.NET MVC (1 de 10) | Microsoft Docs
author: tdykstra
description: Hay disponible una versión más reciente de esta serie de tutoriales, por Visual Studio 2013, Entity Framework 6 y MVC 5. La aplicación Web de ejemplo contoso University de...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 4ba029b6-ee7c-4e45-a0e7-b703c37e5d9a
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 8ee5aa22b6b2329b01d41437f30508e28a2288b2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595969"
---
# <a name="creating-an-entity-framework-data-model-for-an-aspnet-mvc-application-1-of-10"></a>Creación de un modelo de datos de Entity Framework para una aplicación ASP.NET MVC (1 de 10)

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto completado](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> > [!NOTE] 
> > 
> > Hay disponible una [versión más reciente de esta serie de tutoriales](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) , por Visual Studio 2013, Entity Framework 6 y MVC 5.
> 
> 
> En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones ASP.NET MVC 4 mediante el Entity Framework 5 y Visual Studio 2012. La aplicación de ejemplo es un sitio web de una universidad ficticia, Contoso University. Incluye funciones como la admisión de estudiantes, la creación de cursos y asignaciones de instructores. En esta serie de tutoriales se explica cómo crear la aplicación de ejemplo contoso University. Puede [descargar la aplicación completada](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8).
> 
> ## <a name="code-first"></a>Code First
> 
> Hay tres maneras de trabajar con datos en el Entity Framework: *Database First*, *Model First*y *code First*. Este tutorial es para Code First. Para obtener información sobre las diferencias entre estos flujos de trabajo e instrucciones sobre cómo elegir el mejor para su escenario, consulte [Entity Framework flujos de trabajo de desarrollo](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="mvc"></a>MVC
> 
> La aplicación de ejemplo se basa en [ASP.NET MVC](../../../index.md). Si prefiere trabajar con el modelo de formularios Web Forms de ASP.NET, vea la serie de tutoriales de [enlace de modelos y formularios Web Forms](../../../../web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data.md) y la [asignación de contenido de ASP.net Data Access](../../../../whitepapers/aspnet-data-access-content-map.md).
> 
> ## <a name="software-versions"></a>Versiones de software
> 
> | **Se muestra en el tutorial** | **También funciona con** |
> | --- | --- |
> | Windows 8 | Windows 7 |
> | Visual Studio 2012 | Visual Studio 2012 Express para Web. Lo instala automáticamente el SDK de Windows Azure si aún no tiene VS 2012 o VS 2012 Express para Web. Visual Studio 2013 debería funcionar, pero el tutorial no se ha probado con él y algunas opciones de menú y cuadros de diálogo son diferentes. La [versión VS 2013 del SDK de Windows Azure](https://go.microsoft.com/fwlink/p/?linkid=323510) es necesaria para la implementación de Windows Azure. |
> | .NET 4.5 | La mayoría de las características que se muestran funcionarán en .NET 4, pero algunas no. Por ejemplo, la compatibilidad con enumeración en EF requiere .NET 4,5. |
> | Entity Framework 5 |  |
> | [SDK de Windows Azure 2,1](https://go.microsoft.com/fwlink/p/?linkid=323511) | Si omite los pasos de implementación de Windows Azure, no necesita el SDK. Cuando se lance una nueva versión del SDK, el vínculo instalará la versión más reciente. En ese caso, es posible que tenga que adaptar algunas de las instrucciones a la nueva interfaz de usuario y las características. |
> 
> ## <a name="questions"></a>Preguntas
> 
> Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [Entity Framework de ASP.net](https://forums.asp.net/1227.aspx), en el [Foro de Entity Framework y LINQ to Entities](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), o en [stackoverflow.com](http://stackoverflow.com/).
> 
> ## <a name="acknowledgments"></a>Agradecimientos
> 
> Vea el último tutorial de la serie para obtener [confirmaciones y una nota sobre VB](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#acknowledgments).
> 
> ## <a name="original-version-of-the-tutorial"></a>Versión original del tutorial
> 
> La versión original del tutorial está disponible en el [libro electrónico EF 4,1/MVC 3](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC).

## <a name="the-contoso-university-web-application"></a>La aplicación web contoso University

La aplicación que se va a compilar en estos tutoriales es un sitio web sencillo de una universidad.

Los usuarios pueden ver y actualizar la información de estudiantes, cursos e instructores. A continuación se muestran algunas de las pantallas que se van a crear.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

El estilo de la interfaz de usuario de este sitio se ha mantenido fiel a lo que generan las plantillas integradas, para que el tutorial se pueda centrar principalmente en cómo usar Entity Framework.

## <a name="prerequisites"></a>Requisitos previos

Las instrucciones y las capturas de pantalla de este tutorial suponen que está usando [visual studio 2012](https://www.microsoft.com/visualstudio/eng/downloads) o [Visual Studio 2012 Express para web](https://go.microsoft.com/fwlink/?LinkID=275131), con la actualización más reciente y el SDK de Azure para .net instalado a partir de la versión de julio de 2013. Puede obtener todo esto con el siguiente vínculo:

[SDK de Azure para .NET (Visual Studio 2012)](https://go.microsoft.com/fwlink/?LinkId=254364)

Si tiene instalado Visual Studio, el vínculo anterior instalará los componentes que falten. Si no tiene Visual Studio, el vínculo instalará Visual Studio 2012 Express para Web. Puede usar Visual Studio 2013, pero algunos de los procedimientos y pantallas necesarios serán diferentes.

## <a name="create-an-mvc-web-application"></a>Creación de una aplicación web MVC

Abra Visual Studio y cree un nuevo C# proyecto denominado "ContosoUniversity" mediante la plantilla de **aplicación Web ASP.NET MVC 4** . Asegúrese de que tiene como destino **.NET Framework 4,5** (se usarán [`enum` propiedades](https://msdn.microsoft.com/data/hh859576.aspx)y que requiera .net 4,5).

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

En el cuadro de diálogo **nuevo proyecto de ASP.NET MVC 4** , seleccione la plantilla **aplicación de Internet** .

Deje seleccionado el motor de vistas de **Razor** y deje desactivada la casilla **crear un proyecto de prueba unitaria** .

Haga clic en **Aceptar**.

![Project_template_options](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

## <a name="set-up-the-site-style"></a>Configuración del estilo de sitio

Con algunos cambios sencillos se configura el menú del sitio, el diseño y la página principal.

Abra *Views\Shared\\_Layout. cshtml*y reemplace el contenido del archivo por el código siguiente. Los cambios aparecen resaltados.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=5,15,25-28,43)]

Este código realiza los cambios siguientes:

- Reemplaza las instancias de plantilla de "mi aplicación de ASP.NET MVC" y "su logotipo aquí" por "Contoso University".
- Agrega varios vínculos de acción que se utilizarán más adelante en el tutorial.

En *Views\Home\Index.cshtml*, reemplace el contenido del archivo por el código siguiente para eliminar los párrafos de plantilla sobre ASP.net y MVC:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

En *Controllers\HomeController.CS*, cambie el valor de `ViewBag.Message` en el método de acción `Index` a "Bienvenido a contoso University!", tal como se muestra en el ejemplo siguiente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=3)]

Presione CTRL + F5 para ejecutar el sitio. Verá la Página principal con el menú principal.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

## <a name="create-the-data-model"></a>Crear el modelo de datos

A continuación podrá crear las clases de entidad para la aplicación Contoso University. Empezará con las tres entidades siguientes:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Hay una relación uno a varios entre las entidades `Student` y `Enrollment`, y también entre las entidades `Course` y `Enrollment`. En otras palabras, un estudiante se puede inscribir en cualquier número de cursos y un curso puede tener cualquier número de alumnos inscritos.

En las secciones siguientes creará una clase para cada una de estas entidades.

> [!NOTE]
> Si intenta compilar el proyecto antes de terminar de crear todas estas clases de entidad, obtendrá errores del compilador.

### <a name="the-student-entity"></a>La entidad Student

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

En la carpeta *Models* , cree *Student.CS* y reemplace el código existente por el código siguiente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

La propiedad `StudentID` se convertirá en la columna de clave principal de la tabla de base de datos que corresponde a esta clase. De forma predeterminada, la Entity Framework interpreta una propiedad denominada `ID` o *classname* `ID` como clave principal.

La propiedad `Enrollments` es una *propiedad de navegación*. Las propiedades de navegación contienen otras entidades relacionadas con esta entidad. En este caso, la propiedad `Enrollments` de una entidad `Student` contendrá todas las entidades `Enrollment` que están relacionadas con esa entidad de `Student`. En otras palabras, si una fila de `Student` determinada de la base de datos tiene dos filas de `Enrollment` relacionadas (filas que contienen el valor de clave principal de ese estudiante en la columna de clave externa de `StudentID`), la propiedad de navegación `Enrollments` de la entidad `Student` contendrá las dos entidades de `Enrollment`.

Las propiedades de navegación se definen normalmente como `virtual` para que puedan beneficiarse de ciertas funcionalidades de Entity Framework, como la *carga diferida*. (La carga diferida se explicará más adelante, en el tutorial [leer datos relacionados](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) más adelante en esta serie.

Si una propiedad de navegación puede contener varias entidades (como en las relaciones de varios a varios o uno a varios), su tipo debe ser una lista a la que se puedan agregar las entradas, eliminarlas y actualizarlas, como `ICollection`.

### <a name="the-enrollment-entity"></a>La entidad de inscripción

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

En la carpeta *Models*, cree *Enrollment.cs* y reemplace el código existente con el código siguiente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

La propiedad grade es una [enumeración](https://msdn.microsoft.com/data/hh859576.aspx). El signo de interrogación después de la declaración de tipo `Grade` indica que la propiedad `Grade` [acepta valores NULL](https://msdn.microsoft.com/library/2cf62fcy.aspx). Una calificación que es NULL es diferente de un grado cero: null significa que no se conoce una calificación o que todavía no se ha asignado.

La propiedad `StudentID` es una clave externa y la propiedad de navegación correspondiente es `Student`. Una entidad `Enrollment` está asociada con una entidad `Student`, por lo que la propiedad solo puede contener un única entidad `Student` (a diferencia de la propiedad de navegación `Student.Enrollments` que se vio anteriormente, que puede contener varias entidades `Enrollment`).

La propiedad `CourseID` es una clave externa y la propiedad de navegación correspondiente es `Course`. Una entidad `Enrollment` está asociada con una entidad `Course`.

### <a name="the-course-entity"></a>La entidad Course

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

En la carpeta *Models* , cree *Course.CS*y reemplace el código existente por el código siguiente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

La propiedad `Enrollments` es una propiedad de navegación. Una entidad `Course` puede estar relacionada con cualquier número de entidades `Enrollment`.

Nos referiremos más sobre el [[DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)([DatabaseGeneratedOption](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.95).aspx). None)] en el siguiente tutorial. Básicamente, este atributo permite escribir la clave principal para el curso en lugar de hacer que la base de datos lo genere.

## <a name="create-the-database-context"></a>Crear el contexto de base de datos

La clase principal que coordina Entity Framework funcionalidad para un modelo de datos determinado es la clase de *contexto de base* de datos. Cree esta clase derivando de la clase [System. Data. Entity. DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) . En el código se especifica qué entidades se incluyen en el modelo de datos. También se puede personalizar determinado comportamiento de Entity Framework. En este proyecto, la clase se denomina `SchoolContext`.

Cree una carpeta denominada *Dal* (para la capa de acceso a datos). En esa carpeta, cree un nuevo archivo de clase denominado *SchoolContext.CS*y reemplace el código existente por el código siguiente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Este código crea una propiedad [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) para cada conjunto de entidades. En Entity Framework terminología, un *conjunto de entidades* se corresponde normalmente con una tabla de base de datos y una *entidad* corresponde a una fila de la tabla.

La instrucción `modelBuilder.Conventions.Remove` en el método [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) impide que los nombres de tabla se confirmen. Si no lo hizo, las tablas generadas se denominarán `Students`, `Courses`y `Enrollments`. En su lugar, los nombres de tabla se `Student`, `Course`y `Enrollment`. Los desarrolladores están en desacuerdo sobre si los nombres de tabla deben ser plurales o no. En este tutorial se usa la forma singular, pero lo importante es que puede seleccionar cualquier forma que prefiera incluyendo u omitiendo esta línea de código.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) es una versión ligera de la SQL Server Express motor de base de datos que se inicia a petición y se ejecuta en modo de usuario. LocalDB se ejecuta en un modo de ejecución especial de SQL Server Express que le permite trabajar con bases de datos como archivos *. MDF* . Normalmente, los archivos de base de datos LocalDB se guardan en la carpeta *App\_Data* de un proyecto Web. La característica de instancia de usuario de SQL Server Express también le permite trabajar con archivos *. MDF* , pero la característica de instancia de usuario está en desuso. por lo tanto, se recomienda LocalDB para trabajar con archivos *. MDF* .

Normalmente SQL Server Express no se utiliza para las aplicaciones Web de producción. En concreto, LocalDB no se recomienda para su uso en producción con una aplicación web porque no está diseñado para funcionar con IIS.

En Visual Studio 2012 y versiones posteriores, LocalDB se instala de forma predeterminada con Visual Studio. En Visual Studio 2010 y versiones anteriores, SQL Server Express (sin LocalDB) se instala de forma predeterminada con Visual Studio. tendrá que instalarlo manualmente si utiliza Visual Studio 2010.

En este tutorial, trabajará con LocalDB para que la base de datos pueda almacenarse en la carpeta *App\_Data* como archivo *. MDF* . Abra el archivo raíz *Web. config* y agregue una nueva cadena de conexión a la colección de `connectionStrings`, como se muestra en el ejemplo siguiente. (Asegúrese de actualizar el archivo *Web. config* en la carpeta raíz del proyecto. También hay un archivo *Web. config* en la subcarpeta *views* que no necesita actualizar).

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.xml)]

De forma predeterminada, la Entity Framework busca una cadena de conexión denominada igual que la clase `DbContext` (`SchoolContext` para este proyecto). La cadena de conexión que ha agregado especifica una base de datos LocalDB denominada *ContosoUniversity. MDF* ubicada en la carpeta *App\_Data* . Para obtener más información, vea [SQL Server las cadenas de conexión para las aplicaciones Web de ASP.net](https://msdn.microsoft.com/library/jj653752.aspx).

En realidad, no es necesario especificar la cadena de conexión. Si no proporciona una cadena de conexión, Entity Framework creará una automáticamente; sin embargo, es posible que la base de datos no esté en la carpeta *app\_Data* de la aplicación. Para obtener información sobre dónde se creará la base de datos, vea [code First en una nueva base de datos](https://msdn.microsoft.com/data/jj193542).

La colección de `connectionStrings` también tiene una cadena de conexión denominada `DefaultConnection` que se utiliza para la base de datos de pertenencia. No utilizará la base de datos de pertenencia en este tutorial. La única diferencia entre las dos cadenas de conexión es el nombre de la base de datos y el valor del atributo name.

## <a name="set-up-and-execute-a-code-first-migration"></a>Configuración y ejecución de una migración de Code First

Al empezar a desarrollar una aplicación por primera vez, el modelo de datos cambia con frecuencia y, cada vez que el modelo cambia, no se sincroniza con la base de datos. Puede configurar el Entity Framework para quitar y volver a crear la base de datos automáticamente cada vez que cambie el modelo de datos. Esto no es un problema temprano en el desarrollo porque los datos de prueba se vuelven a crear fácilmente, pero después de haber implementado en producción, normalmente desea actualizar el esquema de la base de datos sin quitar la base de datos. La característica migraciones habilita Code First para actualizar la base de datos sin quitarla y volver a crearla. Al principio del ciclo de desarrollo de un proyecto nuevo podría querer usar [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=vs.103).aspx) para quitarlo, volver a crear y reinicializar la base de datos cada vez que cambie el modelo. Una vez que esté listo para implementar la aplicación, puede convertir al método de migración. En este tutorial solo usará migraciones. Para obtener más información, vea la [serie de presentaciones](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx)en pantalla de [migraciones de Code First](https://msdn.microsoft.com/data/jj591621) y migraciones.

### <a name="enable-code-first-migrations"></a>Habilitar Migraciones de Code First

1. En el menú **herramientas** , haga clic en **Administrador de paquetes NuGet** y luego en **consola del administrador de paquetes**.

    ![Selecting_Package_Manager_Console](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)
2. En el símbolo del sistema `PM>` escriba el siguiente comando:

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.ps1)]

    ![comando enable-Migrations](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

    Este comando crea una carpeta *Migrations* en el proyecto ContosoUniversity y coloca en esa carpeta un archivo *Configuration.CS* que se puede editar para configurar las migraciones.

    ![Carpeta migraciones](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)

    La clase `Configuration` incluye un método de `Seed` al que se llama cuando se crea la base de datos y cada vez que se actualiza después de un cambio en el modelo de datos.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

    El propósito de este método `Seed` es permitirle insertar datos de prueba en la base de datos después de que Code First lo cree o lo actualice.

### <a name="set-up-the-seed-method"></a>Configurar el método de inicialización

El método de [inicialización](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) se ejecuta cuando migraciones de Code First crea la base de datos y cada vez que actualiza la base de datos a la migración más reciente. El propósito del método de inicialización es permitirle insertar datos en las tablas antes de que la aplicación tenga acceso a la base de datos por primera vez.

En versiones anteriores de Code First, antes de que se lanzaran las migraciones, era habitual que los métodos de `Seed` insertaran datos de prueba, porque con cada cambio de modelo durante el desarrollo, la base de datos debía eliminarse completamente y volver a crearse desde el principio. Con Migraciones de Code First, los datos de prueba se conservan después de los cambios en la base de datos, por lo que normalmente no es necesario incluir datos de prueba en el método de [inicialización](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) . De hecho, no desea que el método `Seed` Inserte los datos de prueba si va a usar las migraciones para implementar la base de datos en producción, ya que el método `Seed` se ejecutará en producción. En ese caso, desea que el método `Seed` se inserte en la base de datos solo de los datos que desea insertar en producción. Por ejemplo, puede que desee que la base de datos incluya nombres de Departamento reales en la tabla `Department` cuando la aplicación esté disponible en producción.

En este tutorial, va a usar las migraciones para la implementación, pero el método de `Seed` insertará los datos de prueba para que sea más fácil ver cómo funciona la funcionalidad de la aplicación sin tener que insertar manualmente una gran cantidad de datos.

1. Reemplace el contenido del archivo *Configuration.CS* por el código siguiente, que cargará los datos de prueba en la nueva base de datos. 

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    El método de [inicialización](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) toma el objeto de contexto de base de datos como parámetro de entrada y el código del método usa ese objeto para agregar nuevas entidades a la base de datos. Para cada tipo de entidad, el código crea una colección de nuevas entidades, las agrega a la propiedad [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) adecuada y, a continuación, guarda los cambios en la base de datos. No es necesario llamar al método [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) después de cada grupo de entidades, como se hace aquí, pero hacerlo le ayuda a localizar el origen de un problema si se produce una excepción mientras el código está escribiendo en la base de datos.

    Algunas de las instrucciones que insertan datos usan el método [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) para realizar una operación "Upsert". Dado que el método de `Seed` se ejecuta con cada migración, no se pueden insertar datos, ya que las filas que está intentando agregar ya estarán allí después de la primera migración que crea la base de datos. La operación "Upsert" evita errores que podrían producirse si se intenta insertar una fila que ya existe, pero ***reemplaza*** cualquier cambio en los datos que haya realizado al probar la aplicación. En el caso de los datos de prueba de algunas tablas, es posible que no desee que suceda: en algunos casos, cuando cambie los datos mientras realiza las pruebas desea que los cambios permanezcan después de las actualizaciones de la base de datos. En ese caso, desea realizar una operación de inserción condicional: Inserte una fila solo si aún no existe. El método de inicialización utiliza ambos enfoques.

    El primer parámetro que se pasa al método [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) especifica la propiedad que se va a usar para comprobar si ya existe una fila. En el caso de los datos de estudiante de prueba que se proporcionan, se puede usar la propiedad `LastName` para este propósito, ya que cada apellido de la lista es único:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    Este código supone que los apellidos son únicos. Si agrega manualmente un estudiante con un apellido duplicado, recibirá la siguiente excepción la próxima vez que realice una migración.

    La secuencia contiene más de un elemento

    Para obtener más información sobre el método `AddOrUpdate`, consulte el método de encargarse [con EF 4,3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) en el blog de Julia Lerman.

    El código que agrega entidades `Enrollment` no usa el método `AddOrUpdate`. Comprueba si ya existe una entidad e inserta la entidad si no existe. Este enfoque conservará los cambios que realice en un grado de inscripción cuando se ejecuten las migraciones. El código recorre en bucle cada miembro de la [lista](https://msdn.microsoft.com/library/6sh2ey19.aspx) de `Enrollment`y, si no se encuentra la inscripción en la base de datos, agrega la inscripción a la base de datos. La primera vez que actualice la base de datos, la base de datos estará vacía, por lo que agregará cada inscripción.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

    Para obtener información sobre cómo depurar el método de `Seed` y cómo controlar datos redundantes, como dos estudiantes denominados "Alexander Carson", consulte las bases de datos de [propagación y Depuración Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) en el blog de Rick Anderson.
2. Generar el proyecto.

### <a name="create-and-execute-the-first-migration"></a>Crear y ejecutar la primera migración

1. En la ventana de la consola del administrador de paquetes, escriba los siguientes comandos: 

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample14.ps1)]

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    El comando `add-migration` agrega a la carpeta Migrations un archivo *[fecha]\_InitialCreate.CS* que contiene el código que crea la base de datos. El primer parámetro (`InitialCreate)` se usa para el nombre de archivo y puede ser el que desee; normalmente, se elige una palabra o frase que resume lo que se hace en la migración. Por ejemplo, puede asignar un nombre a una migración posterior &quot;AddDepartmentTable&quot;.

    ![Carpeta de migraciones con migración inicial](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    El método `Up` de la clase `InitialCreate` crea las tablas de base de datos que corresponden a los conjuntos de entidades del modelo de datos y el método `Down` las elimina. Las migraciones llaman al método `Up` para implementar los cambios del modelo de datos para una migración. Cuando se escribe un comando para revertir la actualización, las migraciones llaman al método `Down`. En el código siguiente se muestra el contenido del archivo `InitialCreate`:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

    El comando `update-database` ejecuta el método `Up` para crear la base de datos y, a continuación, ejecuta el método `Seed` para rellenar la base de datos.

Ahora se ha creado una base de datos de SQL Server para el modelo de datos. El nombre de la base de datos es *ContosoUniversity*y el archivo *. MDF* está en la carpeta de la aplicación del proyecto *\_Data* , ya que es lo que especificó en la cadena de conexión.

Puede usar **Explorador de servidores** o **Explorador de objetos de SQL Server** (SSOX) para ver la base de datos en Visual Studio. En este tutorial, usará **Explorador de servidores**. En Visual Studio Express 2012 para Web, se llama a **Explorador de servidores** **Explorador de bases de datos**.

1. En el menú **Ver** , haga clic en **Explorador de servidores**.
2. Haga clic en el icono **Agregar conexión** .

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)
3. Si se le solicita el cuadro de diálogo **elegir origen de datos** , haga clic en **Microsoft SQL Server**y, a continuación, haga clic en **continuar**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
4. En el cuadro de diálogo **Agregar conexión** , escriba **(LocalDB) \V11.0** para el **nombre del servidor**. En **Seleccione o escriba un nombre de base de datos**, seleccione **ContosoUniversity.**  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
5. Haga clic en **Aceptar.**
6. Expanda **SchoolContext** y, a continuación, expanda **tablas**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image18.png)
7. Haga clic con el botón secundario en la tabla **Student** y haga clic en **Mostrar datos de tabla** para ver las columnas que se crearon y las filas que se insertaron en la tabla.

    ![Tabla Student](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image19.png)

## <a name="creating-a-student-controller-and-views"></a>Crear un controlador y vistas de estudiante

El siguiente paso consiste en crear un controlador ASP.NET MVC y vistas en la aplicación que puedan funcionar con una de estas tablas.

1. Para crear un controlador de `Student`, haga clic con el botón secundario en la carpeta **Controllers** en **Explorador de soluciones**, seleccione **Agregar**y, a continuación, haga clic en **controlador**. En el cuadro de diálogo **Agregar controlador** , realice las siguientes selecciones y, a continuación, haga clic en **Agregar**: 

   - Nombre del controlador: **StudentController**.
   - Plantilla: **controlador de MVC con acciones de lectura/escritura y vistas, mediante Entity Framework**.
   - Clase de modelo: **Student (ContosoUniversity. Models)** . (Si no ve esta opción en la lista desplegable, compile el proyecto y vuelva a intentarlo).
   - Clase de contexto de datos: **SchoolContext (ContosoUniversity. Models)** .
   - Vistas: **Razor (CSHTML)** . (El valor predeterminado).

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image20.png)
2. Visual Studio abre el archivo *Controllers\StudentController.CS* . Verá que se ha creado una variable de clase que crea instancias de un objeto de contexto de base de datos:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

     El método de acción `Index` obtiene una lista de estudiantes del conjunto de entidades *Students* mediante la lectura de la propiedad `Students` de la instancia de contexto de base de datos:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]

     La vista *Student\Index.cshtml* muestra esta lista en una tabla:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample18.cshtml)]
3. Presione CTRL+F5 para ejecutar el proyecto.

     Haga clic en la pestaña **Students** para ver los datos de prueba que insertó el método `Seed`.

     ![Página de índice de estudiante](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image21.png)

## <a name="conventions"></a>Convenciones

La cantidad de código que tenía que escribir para que el Entity Framework pueda crear una base de datos completa es mínima debido al uso de *convenciones*o suposiciones que realiza el Entity Framework. Algunos de ellos ya se han anotado:

- Las formas en plural de los nombres de clase de entidad se usan como nombres de tabla.
- Los nombres de propiedad de entidad se usan para los nombres de columna.
- Las propiedades de entidad que se denominan `ID` o *classname* `ID` se reconocen como propiedades de clave principal.

Ha visto que las convenciones se pueden invalidar (por ejemplo, si especificó que los nombres de tabla no se han plural) y aprenderá más acerca de las convenciones y cómo invalidarlas en el tutorial [creación de un modelo de datos más complejo](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) más adelante en esta serie. Para obtener más información, vea [convenciones de Code First](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Resumen

Ahora ha creado una aplicación sencilla que usa el Entity Framework y SQL Server Express para almacenar y Mostrar datos. En el siguiente tutorial aprenderá a realizar operaciones CRUD (crear, leer, actualizar y eliminar) básicas. Puede dejar comentarios en la parte inferior de esta página. Háganoslo saber cómo le gustó esta parte del tutorial y cómo podríamos mejorarlo.

Los vínculos a otros recursos de Entity Framework pueden encontrarse en la [asignación de contenido de ASP.net Data Access](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Siguiente](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
