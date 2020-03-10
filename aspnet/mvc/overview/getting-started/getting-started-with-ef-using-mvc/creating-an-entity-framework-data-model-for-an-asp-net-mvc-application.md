---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: 'Tutorial: Introducción a Entity Framework 6 Code First con MVC 5 | Microsoft Docs'
description: En esta serie de tutoriales, aprenderá a crear una aplicación ASP.NET MVC 5 que usa Entity Framework 6 para el acceso a datos.
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: a00a5a3aa295c2584b90abbf931c258c9e1b58ca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78499411"
---
# <a name="tutorial-get-started-with-entity-framework-6-code-first-using-mvc-5"></a>Tutorial: Introducción a Entity Framework 6 Code First con MVC 5

> [!NOTE]
> En el desarrollo nuevo, se recomienda [ASP.NET Core Razor pages](/aspnet/core/razor-pages) a través de los controladores y vistas de MVC de ASP.net. Para obtener una serie de tutoriales similar a esta con Razor Pages, consulte [Tutorial: Introducción a la Razor pages en ASP.net Core](/aspnet/core/tutorials/razor-pages/razor-pages-start). El nuevo tutorial:
> * Es más fácil de seguir.
> * Proporciona más procedimientos recomendados de EF Core.
> * Usa consultas más eficaces.
> * Es más actual en relación con la API más reciente.
> * Abarca más características.
> * Es el método preferido para el desarrollo de nuevas aplicaciones.

En esta serie de tutoriales, aprenderá a crear una aplicación ASP.NET MVC 5 que usa Entity Framework 6 para el acceso a datos. En este tutorial se usa el flujo de trabajo Code First. Para obtener información sobre cómo elegir entre Code First, Database First y Model First, vea [crear un modelo](/ef/ef6/modeling/).

En esta serie de tutoriales se explica cómo crear la aplicación de ejemplo contoso University. La aplicación de ejemplo es un sitio web sencillo de la Universidad. Con ella, puede ver y actualizar la información de estudiantes, cursos e instructores. A continuación se muestran dos de las pantallas que se crean:

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![Editar estudiante](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

En este tutorial va a:

> [!div class="checklist"]
> * Creación de una aplicación Web de MVC
> * Configurar el estilo del sitio
> * Instalación de Entity Framework 6
> * Crear el modelo de datos
> * Crear el contexto de base de datos
> * Inicializa la base de datos con datos de prueba
> * Configurar EF 6 para usar LocalDB
> * Crea un controlador y vistas
> * Consulta la base de datos

## <a name="prerequisites"></a>Requisitos previos

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

## <a name="create-an-mvc-web-app"></a>Creación de una aplicación Web de MVC

1. Abra Visual Studio y cree un C# proyecto web con la plantilla **aplicación Web de ASP.net (.NET Framework)** . Asigne al proyecto el nombre *ContosoUniversity* y seleccione **Aceptar**.

   ![Cuadro de diálogo nuevo proyecto en Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-project-dialog.png)

1. En **nueva aplicación Web de ASP.net-ContosoUniversity**, seleccione **MVC**.

   ![Cuadro de diálogo Nueva aplicación web en Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-web-app-dialog.png)

    > [!NOTE]
    > De forma predeterminada, la opción de **autenticación** está establecida en **sin autenticación**. En este tutorial, la aplicación web no requiere que los usuarios inicien sesión. Además, no restringe el acceso en función de quién haya iniciado sesión.

1. Seleccione **Aceptar** para crear el proyecto.

## <a name="set-up-the-site-style"></a>Configurar el estilo del sitio

Con algunos cambios sencillos se configura el menú del sitio, el diseño y la página principal.

1. Abra *Views\Shared\\_Layout. cshtml*y realice los cambios siguientes:

   - Cambie todas las apariciones de "My ASP.NET Application" y "Application name" por "Contoso University".
   - Agregue entradas de menú para estudiantes, cursos, instructores y departamentos, y elimine la entrada de contacto.

   Los cambios se resaltan en el siguiente fragmento de código:

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

2. En *Views\Home\Index.cshtml*, reemplace el contenido del archivo por el código siguiente para reemplazar el texto sobre ASP.net y MVC por texto sobre esta aplicación:

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

3. Presione Ctrl + F5 para ejecutar el sitio Web. Verá la Página principal con el menú principal.

## <a name="install-entity-framework-6"></a>Instalación de Entity Framework 6

1. En el menú **herramientas** , elija **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**.

2. En la ventana **Consola del Administrador de paquetas** , escriba el siguiente comando:

   ```text
   Install-Package EntityFramework
   ```

Este paso es uno de los pasos que este tutorial tiene manualmente, pero esto podría haber sido realizado automáticamente por la característica de scaffolding de ASP.NET MVC. Los está haciendo manualmente para que pueda ver los pasos necesarios para usar Entity Framework (EF). Usará la técnica scaffolding más adelante para crear el controlador y las vistas de MVC. Una alternativa consiste en permitir que la técnica scaffolding instale automáticamente el paquete de NuGet de EF, cree la clase de contexto de base de datos y cree la cadena de conexión. Cuando esté listo para hacerlo, lo único que tiene que hacer es omitir esos pasos y aplicar la técnica scaffolding al controlador MVC después de crear las clases de entidad.

## <a name="create-the-data-model"></a>Crear el modelo de datos

A continuación podrá crear las clases de entidad para la aplicación Contoso University. Empezará con las tres entidades siguientes:

**Curso** <-> **inscripción** <-> **estudiante**

| Entidades | Relación |
| -------- | ------------ |
| Curso para la inscripción | Uno a varios |
| Estudiante para inscribirse | Uno a varios |

Hay una relación uno a varios entre las entidades `Student` y `Enrollment`, y también entre las entidades `Course` y `Enrollment`. En otras palabras, un estudiante se puede inscribir en cualquier número de cursos y un curso puede tener cualquier número de alumnos inscritos.

En las secciones siguientes, creará una clase para cada una de estas entidades.

> [!NOTE]
> Si intenta compilar el proyecto antes de terminar de crear todas estas clases de entidad, obtendrá errores del compilador.

### <a name="the-student-entity"></a>La entidad Student

- En la carpeta *Models* , cree un archivo de clase denominado *Student.CS* ; para ello, haga clic con el botón derecho en la carpeta en **Explorador de soluciones** y elija **Agregar** > **clase**. Reemplace el código de plantilla por el código siguiente:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

La propiedad `ID` se convertirá en la columna de clave principal de la tabla de base de datos que corresponde a esta clase. De forma predeterminada, Entity Framework interpreta una propiedad denominada `ID` o *classname* `ID` como clave principal.

La propiedad `Enrollments` es una *propiedad de navegación*. Las propiedades de navegación contienen otras entidades relacionadas con esta entidad. En este caso, la propiedad `Enrollments` de una entidad `Student` contendrá todas las entidades `Enrollment` que están relacionadas con esa entidad de `Student`. En otras palabras, si una fila de `Student` determinada de la base de datos tiene dos filas de `Enrollment` relacionadas (filas que contienen el valor de clave principal de ese estudiante en la columna de clave externa de `StudentID`), la propiedad de navegación `Enrollments` de la entidad `Student` contendrá las dos entidades de `Enrollment`.

Las propiedades de navegación se definen normalmente como `virtual` para que puedan beneficiarse de ciertas funcionalidades de Entity Framework, como la *carga diferida*. (La carga diferida se explicará más adelante, en el tutorial [leer datos relacionados](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) más adelante en esta serie).

Si una propiedad de navegación puede contener varias entidades (como en las relaciones de varios a varios o uno a varios), su tipo debe ser una lista a la que se puedan agregar las entradas, eliminarlas y actualizarlas, como `ICollection`.

### <a name="the-enrollment-entity"></a>La entidad Enrollment

- En la carpeta *Models*, cree *Enrollment.cs* y reemplace el código existente con el código siguiente:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

La propiedad `EnrollmentID` será la clave principal; esta entidad usa el patrón *classname* `ID` en lugar de `ID` por sí solo como se vio en la entidad `Student`. Normalmente debería elegir un patrón y usarlo en todo el modelo de datos. En este caso, la variación muestra que se puede usar cualquiera de los patrones. En un tutorial posterior, verá cómo usar `ID` sin `classname` facilita la implementación de la herencia en el modelo de datos.

La propiedad `Grade` es una [enumeración](/ef/ef6/modeling/code-first/data-types/enums). El signo de interrogación después de la declaración de tipo `Grade` indica que la propiedad `Grade`[acepta valores NULL](/dotnet/csharp/programming-guide/nullable-types/using-nullable-types). Una calificación que es NULL es diferente de un grado cero: null significa que no se conoce una calificación o que todavía no se ha asignado.

La propiedad `StudentID` es una clave externa y la propiedad de navegación correspondiente es `Student`. Una entidad `Enrollment` está asociada con una entidad `Student`, por lo que la propiedad solo puede contener un única entidad `Student` (a diferencia de la propiedad de navegación `Student.Enrollments` que se vio anteriormente, que puede contener varias entidades `Enrollment`).

La propiedad `CourseID` es una clave externa y la propiedad de navegación correspondiente es `Course`. Una entidad `Enrollment` está asociada con una entidad `Course`.

Entity Framework interpreta una propiedad como una propiedad de clave externa si se denomina *&lt;nombre de propiedad de navegación&gt;&lt;nombre de propiedad de clave principal&gt;* (por ejemplo, `StudentID` para la propiedad de navegación `Student` porque la clave principal de la entidad `Student` es `ID`). Las propiedades de clave externa también se pueden denominar simplemente *&lt;nombre de propiedad de clave principal&gt;* (por ejemplo, `CourseID` porque la clave principal de la entidad de `Course` es `CourseID`).

### <a name="the-course-entity"></a>La entidad Course

- En la carpeta *Models* , cree *Course.CS*y reemplace el código de plantilla por el código siguiente:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

La propiedad `Enrollments` es una propiedad de navegación. Una entidad `Course` puede estar relacionada con cualquier número de entidades `Enrollment`.

En un tutorial posterior de esta serie, hablaremos sobre el atributo <xref:System.ComponentModel.DataAnnotations.Schema.DatabaseGeneratedAttribute>. Básicamente, este atributo permite escribir la clave principal para el curso en lugar de hacer que la base de datos lo genere.

## <a name="create-the-database-context"></a>Crear el contexto de base de datos

La clase principal que coordina Entity Framework funcionalidad para un modelo de datos determinado es la clase de *contexto de base* de datos. Cree esta clase derivando de la clase [System. Data. Entity. DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.113).aspx) . En el código, especifique qué entidades se incluyen en el modelo de datos. También se puede personalizar determinado comportamiento de Entity Framework. En este proyecto, la clase se denomina `SchoolContext`.

- Para crear una carpeta en el proyecto ContosoUniversity, haga clic con el botón secundario en el proyecto en **Explorador de soluciones** , haga clic en **Agregar**y, a continuación, haga clic en **nueva carpeta**. Asigne a la nueva carpeta el nombre *Dal* (para capa de acceso a datos). En esa carpeta, cree un nuevo archivo de clase denominado *SchoolContext.CS*y reemplace el código de plantilla por el código siguiente:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specify-entity-sets"></a>Especificar conjuntos de entidades

Este código crea una propiedad [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.113).aspx) para cada conjunto de entidades. En Entity Framework terminología, un *conjunto de entidades* se corresponde normalmente con una tabla de base de datos y una *entidad* corresponde a una fila de la tabla.

> [!NOTE]
>
> Puede omitir las instrucciones `DbSet<Enrollment>` y `DbSet<Course>` y funcionaría igual. Entity Framework incluiría implícitamente porque la entidad `Student` hace referencia a la entidad `Enrollment` y la entidad `Enrollment` hace referencia a la entidad `Course`.

### <a name="specify-the-connection-string"></a>Especificar la cadena de conexión

El nombre de la cadena de conexión (que agregará al archivo Web. config más adelante) se pasa al constructor.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

También puede pasar la cadena de conexión en lugar del nombre de una que esté almacenada en el archivo Web. config. Para obtener más información sobre las opciones para especificar la base de datos que se va a usar, consulte [cadenas y modelos de conexión](/ef/ef6/fundamentals/configuring/connection-strings).

Si no especifica una cadena de conexión o el nombre de una explícitamente, Entity Framework supone que el nombre de la cadena de conexión es el mismo que el nombre de la clase. En este ejemplo, el nombre de la cadena de conexión predeterminada se `SchoolContext`, igual que el que se especifica explícitamente.

### <a name="specify-singular-table-names"></a>Especificar nombres de tabla en singular

La instrucción `modelBuilder.Conventions.Remove` en el método [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) impide que los nombres de tabla se confirmen. Si no lo hizo, las tablas generadas en la base de datos se denominarán `Students`, `Courses`y `Enrollments`. En su lugar, los nombres de tabla se `Student`, `Course`y `Enrollment`. Los desarrolladores están en desacuerdo sobre si los nombres de tabla deben ser plurales o no. En este tutorial se usa la forma singular, pero lo importante es que puede seleccionar cualquier forma que prefiera incluyendo u omitiendo esta línea de código.

## <a name="initialize-db-with-test-data"></a>Inicializa la base de datos con datos de prueba

Entity Framework puede crear (o quitar y volver a crear) automáticamente una base de datos cuando se ejecuta la aplicación. Puede especificar que esto se realice cada vez que se ejecute la aplicación o solo cuando el modelo no esté sincronizado con la base de datos existente. También puede escribir un método `Seed` que Entity Framework llama automáticamente después de crear la base de datos con el fin de rellenarla con datos de prueba.

El comportamiento predeterminado es crear una base de datos solo si no existe (y producir una excepción si el modelo ha cambiado y la base de datos ya existe). En esta sección, especificará que la base de datos debe quitarse y volver a crearse cada vez que cambie el modelo. Al quitar la base de datos, se pierden todos los datos. Por lo general, es correcto durante el desarrollo, porque el método de `Seed` se ejecutará cuando se vuelva a crear la base de datos y volverá a crear los datos de prueba. Pero en producción por lo general no desea perder todos los datos cada vez que necesite cambiar el esquema de la base de datos. Más adelante, verá cómo controlar los cambios del modelo mediante Migraciones de Code First para cambiar el esquema de la base de datos en lugar de quitar y volver a crear la base de datos.

1. En la carpeta DAL, cree un nuevo archivo de clase denominado *SchoolInitializer.CS* y reemplace el código de plantilla por el código siguiente, que hace que se cree una base de datos cuando sea necesario y carga los datos de prueba en la nueva base de datos.

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

   El método `Seed` toma el objeto de contexto de base de datos como parámetro de entrada y el código del método usa ese objeto para agregar nuevas entidades a la base de datos. Para cada tipo de entidad, el código crea una colección de nuevas entidades, las agrega a la propiedad `DbSet` adecuada y, a continuación, guarda los cambios en la base de datos. No es necesario llamar al método `SaveChanges` después de cada grupo de entidades, como se hace aquí, pero hacerlo le ayuda a localizar el origen de un problema si se produce una excepción mientras el código está escribiendo en la base de datos.

2. Para indicar a Entity Framework que use la clase de inicializador, agregue un elemento al elemento `entityFramework` en el archivo *Web. config* de la aplicación (el de la carpeta raíz del proyecto), tal como se muestra en el ejemplo siguiente:

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

   El `context type` especifica el nombre completo de la clase de contexto y el ensamblado en el que se encuentra, y el `databaseinitializer type` especifica el nombre completo de la clase de inicializador y el ensamblado en el que se encuentra. (Si no desea que EF use el inicializador, puede establecer un atributo en el elemento `context`: `disableDatabaseInitialization="true"`). Para obtener más información, consulte Configuración del [archivo de configuración](/ef/ef6/fundamentals/configuring/config-file).

   Una alternativa al establecimiento del inicializador en el archivo *Web. config* es hacerlo en el código agregando una instrucción `Database.SetInitializer` al método `Application_Start` en el archivo *global.asax.CS* . Para obtener más información, vea [Descripción de los inicializadores de base de datos en Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm).

Ahora la aplicación está configurada de modo que, al tener acceso a la base de datos por primera vez en una ejecución determinada de la aplicación, Entity Framework Compare la base de datos con el modelo (las clases de entidad y `SchoolContext`). Si hay una diferencia, la aplicación quita y vuelve a crear la base de datos.

> [!NOTE]
> Al implementar una aplicación en un servidor Web de producción, debe quitar o deshabilitar el código que quita y vuelve a crear la base de datos. Lo hará en un tutorial posterior de esta serie.

## <a name="set-up-ef-6-to-use-localdb"></a>Configurar EF 6 para usar LocalDB

[LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb?view=sql-server-2017) es una versión ligera del motor de base de datos de SQL Server Express. Es fácil de instalar y configurar, inicia a petición y se ejecuta en modo de usuario. LocalDB se ejecuta en un modo de ejecución especial de SQL Server Express que le permite trabajar con bases de datos como archivos *. MDF* . Puede colocar los archivos de base de datos de LocalDB en la carpeta *App\_Data* de un proyecto web si desea poder copiar la base de datos con el proyecto. La característica de instancia de usuario de SQL Server Express también le permite trabajar con archivos *. MDF* , pero la característica de instancia de usuario está en desuso. por lo tanto, se recomienda LocalDB para trabajar con archivos *. MDF* . LocalDB se instala de forma predeterminada con Visual Studio.

Normalmente, SQL Server Express no se utiliza para las aplicaciones Web de producción. En concreto, LocalDB no se recomienda para su uso en producción con una aplicación web porque no está diseñado para funcionar con IIS.

- En este tutorial, trabajará con LocalDB. Abra el archivo *Web. config* de la aplicación y agregue un elemento `connectionStrings` que preceda al elemento `appSettings`, tal y como se muestra en el ejemplo siguiente. (Asegúrese de actualizar el archivo *Web. config* en la carpeta raíz del proyecto. También hay un archivo *Web. config* en la subcarpeta *views* que no necesita actualizar).

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

La cadena de conexión que ha agregado especifica que Entity Framework usará una base de datos LocalDB denominada *ContosoUniversity1. MDF*. (La base de datos no existe todavía, pero EF la creará). Si desea crear la base de datos en la *aplicación\_* carpeta de datos, puede Agregar `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` a la cadena de conexión. Para obtener más información sobre las cadenas de conexión, vea [SQL Server cadenas de conexión para las aplicaciones Web de ASP.net](/previous-versions/aspnet/jj653752(v=vs.110)).

En realidad, no se necesita una cadena de conexión en el archivo *Web. config* . Si no proporciona una cadena de conexión, Entity Framework utiliza una cadena de conexión predeterminada basada en la clase de contexto. Para obtener más información, vea [code First en una nueva base de datos](/ef/ef6/modeling/code-first/workflows/new-database).

## <a name="create-controller-and-views"></a>Crea un controlador y vistas

Ahora creará una página web para mostrar los datos. El proceso de solicitar los datos desencadena automáticamente la creación de la base de datos. Comenzará creando un nuevo controlador. Pero antes de hacerlo, compile el proyecto para que el modelo y las clases de contexto estén disponibles para el scaffolding del controlador de MVC.

1. Haga clic con el botón secundario en la carpeta **Controllers** en **Explorador de soluciones**, seleccione **Agregar**y, a continuación, haga clic en **nuevo elemento con scaffolding**.
2. En el cuadro de diálogo **Agregar scaffold** , seleccione **controlador de MVC 5 con vistas, usando Entity Framework**y, a continuación, elija **Agregar**.

     ![Cuadro de diálogo Agregar scaffolding en Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/add-scaffold.png)

3. En el cuadro de diálogo **Agregar controlador** , realice las siguientes selecciones y, a continuación, elija **Agregar**:

   - Clase de modelo: **Student (ContosoUniversity. Models)** . (Si no ve esta opción en la lista desplegable, compile el proyecto y vuelva a intentarlo).
   - Clase de contexto de datos: **SchoolContext (ContosoUniversity. Dal)** .
   - Nombre del controlador: **StudentController** (no StudentsController).
   - Deje los valores predeterminados para los demás campos.

     Al hacer clic en **Agregar**, el scaffolding crea un archivo *StudentController.CS* y un conjunto de vistas (archivos *. cshtml* ) que funcionan con el controlador. En el futuro, cuando cree proyectos que utilicen Entity Framework, también puede aprovechar algunas funciones adicionales del scaffolding: crear su primera clase de modelo, no crear una cadena de conexión y, a continuación, en el cuadro **Agregar controlador** especifique el **nuevo contexto de datos** seleccionando el botón **+** junto a clase de **contexto de datos**. El scaffolding creará la clase de `DbContext` y la cadena de conexión, así como el controlador y las vistas.
4. Visual Studio abre el archivo *Controllers\StudentController.CS* . Verá que se ha creado una variable de clase que crea instancias de un objeto de contexto de base de datos:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

     El método de acción `Index` obtiene una lista de estudiantes del conjunto de entidades *Students* mediante la lectura de la propiedad `Students` de la instancia de contexto de base de datos:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

     La vista *Student\Index.cshtml* muestra esta lista en una tabla:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
5. Presione Ctrl+F5 para ejecutar el proyecto. (Si obtiene el error "no se puede crear la instantánea", cierre el explorador e inténtelo de nuevo).

     Haga clic en la pestaña **Students** para ver los datos de prueba que insertó el método `Seed`. En función de la restricción de la ventana del explorador, verá el vínculo de la pestaña Student en la barra de direcciones superior o tendrá que hacer clic en la esquina superior derecha para ver el vínculo.

     ![Botón de menú](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

## <a name="view-the-database"></a>Consulta la base de datos

Cuando ejecutó la página Students y la aplicación intentó tener acceso a la base de datos, EF detectó que no había ninguna base de datos y creó una. EF ejecutó el método de inicialización para rellenar la base de datos con datos.

Puede usar **Explorador de servidores** o **Explorador de objetos de SQL Server** (SSOX) para ver la base de datos en Visual Studio. En este tutorial, usará **Explorador de servidores**.

1. Cierre el explorador.
2. En **Explorador de servidores**, expanda **conexiones de datos** (es posible que deba seleccionar primero el botón actualizar), expanda **contexto de escuela (ContosoUniversity)** y, a continuación, expanda **tablas** para ver las tablas de la base de datos nueva.

3. Haga clic con el botón secundario en la tabla **Student** y haga clic en **Mostrar datos de tabla** para ver las columnas que se crearon y las filas que se insertaron en la tabla.

4. Cierre la conexión **Explorador de servidores** .

Los archivos de base de datos *ContosoUniversity1. MDF* y *. ldf* se encuentran en la carpeta *% userprofile%* .

Dado que está usando el inicializador de `DropCreateDatabaseIfModelChanges`, ahora puede realizar un cambio en la clase `Student`, ejecutar de nuevo la aplicación y la base de datos se volverá a crear automáticamente para que coincida con el cambio. Por ejemplo, si agrega una propiedad `EmailAddress` a la clase `Student`, vuelve a ejecutar la página Students y, a continuación, vuelve a examinar la tabla, verá una nueva columna `EmailAddress`.

## <a name="conventions"></a>Convenciones

La cantidad de código que tenía que escribir para que Entity Framework pueda crear una base de datos completa es mínima debido a las *convenciones*o suposiciones que realiza Entity Framework. Algunos de ellos ya se han anotado o se usaron sin ser consciente de ellos:

- Las formas en plural de los nombres de clase de entidad se usan como nombres de tabla.
- Los nombres de propiedad de entidad se usan para los nombres de columna.
- Las propiedades de entidad que se denominan `ID` o *classname* `ID` se reconocen como propiedades de clave principal.
- Una propiedad se interpreta como una propiedad de clave externa si se denomina *&lt;nombre de propiedad de navegación&gt;&lt;nombre de la propiedad de clave principal&gt;* (por ejemplo, `StudentID` para la propiedad de navegación `Student` porque la clave principal de la entidad `Student` es `ID`). Las propiedades de clave externa también se pueden denominar simplemente &lt;nombre de propiedad de clave principal&gt; (por ejemplo, `EnrollmentID` porque la clave principal de la entidad de `Enrollment` es `EnrollmentID`).

Ha visto que las convenciones se pueden invalidar. Por ejemplo, si especificó que los nombres de tabla no se deben pluralar, verá más adelante cómo marcar explícitamente una propiedad como una propiedad de clave externa.

## <a name="get-the-code"></a>Obtención del código

[Descargar proyecto completado](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Recursos adicionales

Para obtener más información sobre EF 6, consulte estos artículos:

* [Acceso a los datos de ASP.NET: Recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md)

* [Convenciones de Code First](/ef/ef6/modeling/code-first/conventions/built-in)

* [Crear un modelo de datos más complejo](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial va a:

> [!div class="checklist"]
> * Se creó una aplicación Web de MVC
> * Configurar el estilo del sitio
> * Instalado Entity Framework 6
> * Creado el modelo de datos
> * Creado el contexto de la base de datos
> * Inicializado la base de datos con datos de prueba
> * Configurar EF 6 para usar LocalDB
> * Creado un controlador y vistas
> * Consultado la base de datos

Avance al siguiente artículo para aprender a revisar y personalizar el código de creación, lectura, actualización y eliminación (CRUD) en los controladores y las vistas.
> [!div class="nextstepaction"]
> [Implementación de la funcionalidad CRUD básica](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)