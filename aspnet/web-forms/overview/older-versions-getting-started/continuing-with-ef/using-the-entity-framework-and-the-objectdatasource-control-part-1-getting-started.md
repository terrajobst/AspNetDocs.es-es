---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
title: 'Usar el Entity Framework 4,0 y el control ObjectDataSource, parte 1: Introducción | Microsoft Docs'
author: tdykstra
description: Esta serie de tutoriales se basa en la aplicación web contoso University que crea el Introducción con la serie de tutoriales Entity Framework. Si yo...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 244278c1-fec8-4255-8a8a-13bde491c4f5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
msc.type: authoredcontent
ms.openlocfilehash: 2f14707eb058d438495dd2bc4c17b976c471fc97
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78440407"
---
# <a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-1-getting-started"></a>Usar el Entity Framework 4,0 y el control ObjectDataSource, parte 1: Introducción

por [Tom Dykstra](https://github.com/tdykstra)

> Esta serie de tutoriales se basa en la aplicación web contoso University que crea el [Introducción con la](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) serie de tutoriales Entity Framework 4,0. Si no completó los tutoriales anteriores, como punto de partida para este tutorial, puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que habría creado. También puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) que se crea en la serie completa de tutoriales.
> 
> En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET mediante el Entity Framework 4,0 y Visual Studio 2010. La aplicación de ejemplo es un sitio web de una Universidad ficticia de contoso. Incluye funciones como la admisión de estudiantes, la creación de cursos y asignaciones de instructores.
> 
> En el tutorial se muestran C#ejemplos de. El [ejemplo descargable](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) contiene código en C# y Visual Basic.
> 
> ## <a name="database-first"></a>Database First
> 
> Hay tres maneras de trabajar con datos en el Entity Framework: *Database First*, *Model First*y *code First*. Este tutorial es para Database First. Para obtener información sobre las diferencias entre estos flujos de trabajo e instrucciones sobre cómo elegir el mejor para su escenario, consulte [Entity Framework flujos de trabajo de desarrollo](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>formularios Web Forms
> 
> Al igual que la serie Introducción, en esta serie de tutoriales se usa el modelo de formularios Web Forms de ASP.NET y se supone que sabe cómo trabajar con formularios Web Forms de ASP.NET en Visual Studio. Si no lo hace, vea [Introducción con formularios Web Forms de ASP.NET 4,5](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Si prefiere trabajar con el marco de ASP.NET MVC, consulte [Introducción con el Entity Framework mediante ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Versiones de software
> 
> | **Se muestra en el tutorial** | **También funciona con** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express para Web. El tutorial no se ha probado con versiones posteriores de Visual Studio. Hay muchas diferencias en las selecciones de menú, los cuadros de diálogo y las plantillas. |
> | .NET 4 | .NET 4,5 es compatible con versiones anteriores de .NET 4, pero el tutorial no se ha probado con .NET 4,5. |
> | Entity Framework 4 | El tutorial no se ha probado con versiones posteriores de Entity Framework. A partir de Entity Framework 5, EF utiliza de forma predeterminada el `DbContext API` que se presentó con EF 4,1. El control EntityDataSource se diseñó para usar la API de `ObjectContext`. Para obtener información sobre cómo usar el control EntityDataSource con la API de `DbContext`, consulte [esta entrada de blog](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Preguntas
> 
> Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [Entity Framework de ASP.net](https://forums.asp.net/1227.aspx), en el [Foro de Entity Framework y LINQ to Entities](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), o en [stackoverflow.com](http://stackoverflow.com/).

El control `EntityDataSource` permite crear una aplicación muy rápidamente, pero normalmente requiere que se mantenga una cantidad significativa de lógica de negocios y lógica de acceso a datos en las páginas *. aspx* . Si espera que la aplicación crezca en complejidad y requiera mantenimiento en curso, puede invertir más tiempo de desarrollo por adelantado para crear una estructura de aplicación de *n niveles* o en *capas* que sea más fácil de mantener. Para implementar esta arquitectura, separe la capa de presentación de la capa de lógica de negocios (BLL) y la capa de acceso a datos (DAL). Una manera de implementar esta estructura es usar el control `ObjectDataSource` en lugar del control `EntityDataSource`. Cuando se usa el control `ObjectDataSource`, se implementa su propio código de acceso a datos y, a continuación, se invoca en páginas *. aspx* con un control que tiene muchas de las mismas características que otros controles de origen de datos. Esto le permite combinar las ventajas de un enfoque de n niveles con las ventajas de usar un control de formularios Web Forms para el acceso a datos.

El control `ObjectDataSource` ofrece más flexibilidad también en otras maneras. Dado que escribe su propio código de acceso a datos, es más fácil hacer algo más que simplemente leer, insertar, actualizar o eliminar un tipo de entidad concreto, que son las tareas que el control `EntityDataSource` está diseñado para realizar. Por ejemplo, puede realizar el registro cada vez que se actualiza una entidad, archivar datos cada vez que se elimina una entidad o comprobar y actualizar automáticamente los datos relacionados según sea necesario al insertar una fila con un valor de clave externa.

## <a name="business-logic-and-repository-classes"></a>Clases de repositorio y lógica de negocios

Un control de `ObjectDataSource` funciona mediante la invocación de una clase que se crea. La clase incluye métodos que recuperan y actualizan datos, y proporciona los nombres de esos métodos al control `ObjectDataSource` en el marcado. Durante la representación o el procesamiento de postback, el `ObjectDataSource` llama a los métodos que ha especificado.

Además de las operaciones CRUD básicas, la clase que se crea para utilizar con el control `ObjectDataSource` puede necesitar ejecutar la lógica de negocios cuando el `ObjectDataSource` lee o actualiza los datos. Por ejemplo, al actualizar un departamento, es posible que tenga que validar que ningún otro departamento tenga el mismo administrador porque una persona no puede ser administrador de más de un departamento.

En algunos `ObjectDataSource` documentación, como la [información general de la clase ObjectDataSource](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.aspx), el control llama a una clase denominada *objeto comercial* que incluye lógica de negocios y lógica de acceso a datos. En este tutorial, creará clases independientes para la lógica de negocios y para la lógica de acceso a datos. La clase que encapsula la lógica de acceso a datos se denomina *repositorio*. La clase de lógica de negocios incluye métodos de lógica empresarial y métodos de acceso a datos, pero los métodos de acceso a datos llaman al repositorio para realizar tareas de acceso a datos.

También creará una capa de abstracción entre la capa BLL y la capa DAL que facilita las pruebas unitarias automatizadas de la capa BLL. Esta capa de abstracción se implementa mediante la creación de una interfaz y el uso de la interfaz cuando se crea una instancia del repositorio en la clase de lógica de negocios. Esto permite proporcionar la clase de lógica de negocios con una referencia a cualquier objeto que implemente la interfaz del repositorio. Para un funcionamiento normal, se proporciona un objeto de repositorio que funciona con el Entity Framework. Para las pruebas, debe proporcionar un objeto de repositorio que funcione con los datos almacenados de una forma que pueda manipular fácilmente, como las variables de clase definidas como colecciones.

En la ilustración siguiente se muestra la diferencia entre una clase de lógica empresarial que incluye la lógica de acceso a datos sin un repositorio y otra que usa un repositorio.

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image1.png)

Comenzará creando páginas web en las que el control de `ObjectDataSource` se enlaza directamente a un repositorio porque solo realiza tareas básicas de acceso a datos. En el tutorial siguiente, creará una clase de lógica de negocios con la lógica de validación y enlazará el control de `ObjectDataSource` a esa clase en lugar de a la clase de repositorio. También creará pruebas unitarias para la lógica de validación. En el tercer tutorial de esta serie, agregará funcionalidad de ordenación y filtrado a la aplicación.

Las páginas que crea en este tutorial funcionan con el conjunto de entidades `Departments` del modelo de datos creado en la [serie de tutoriales de introducción](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md).

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image3.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image5.png)

## <a name="updating-the-database-and-the-data-model"></a>Actualizar la base de datos y el modelo de datos

Para comenzar este tutorial, debe realizar dos cambios en la base de datos, y ambos requieren los cambios correspondientes en el modelo de datos que ha creado en el [Introducción con los tutoriales de Entity Framework y formularios Web Forms](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) . En uno de esos tutoriales, realizó cambios en el diseñador manualmente para sincronizar el modelo de datos con la base de datos después de un cambio en la base de datos. En este tutorial, usará la herramienta **Actualizar modelo de la base de** datos del diseñador para actualizar el modelo de datos automáticamente.

### <a name="adding-a-relationship-to-the-database"></a>Agregar una relación a la base de datos

En Visual Studio, abra la aplicación web contoso University que creó en el Introducción con la serie de tutoriales de [Entity Framework y Web Forms](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) y, a continuación, abra el diagrama de base de datos `SchoolDiagram`.

Si observa la tabla `Department` del diagrama de base de datos, verá que tiene una columna `Administrator`. Esta columna es una clave externa de la tabla de `Person`, pero no se ha definido ninguna relación de clave externa en la base de datos. Debe crear la relación y actualizar el modelo de datos para que el Entity Framework pueda controlar automáticamente esta relación.

En el diagrama de base de datos, haga clic con el botón secundario en la tabla `Department` y seleccione **relaciones**.

[![Image80](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image7.png)

En el cuadro **relaciones de clave externa** , haga clic en **Agregar**y, a continuación, haga clic en los puntos suspensivos para **especificación de tablas y columnas**.

[![Image81](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image9.png)

En el cuadro de diálogo **tablas y columnas** , establezca la tabla y el campo de la clave principal en `Person` y `PersonID`, y establezca la tabla y el campo de la clave externa en `Department` y `Administrator`. (Al hacerlo, el nombre de la relación cambiará de `FK_Department_Department` a `FK_Department_Person`).

[![Image82](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image11.png)

Haga clic en **Aceptar** en el cuadro **tablas y columnas** , haga clic en **cerrar** en el cuadro **relaciones de clave externa** y guarde los cambios. Si se le pregunta si desea guardar las tablas `Person` y `Department`, haga clic en **sí**.

> [!NOTE]
> Si ha eliminado `Person` filas que se corresponden con los datos que ya están en la columna `Administrator`, no podrá guardar este cambio. En ese caso, use el editor de tablas de **Explorador de servidores** para asegurarse de que el valor de `Administrator` de cada fila de `Department` contiene el identificador de un registro que realmente existe en la tabla de `Person`.
> 
> Después de guardar el cambio, no podrá eliminar una fila de la tabla `Person` si esa persona es un administrador de departamento. En una aplicación de producción, se proporciona un mensaje de error específico cuando una restricción de base de datos impide una eliminación o se especifica una eliminación en cascada. Para obtener un ejemplo de cómo especificar una eliminación en cascada, vea [el Entity Framework y ASP.net – introducción parte 2](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2.md).

### <a name="adding-a-view-to-the-database"></a>Agregar una vista a la base de datos

En la página New *departments. aspx* que va a crear, desea proporcionar una lista desplegable de instructores, con nombres en el formato "último, primero" para que los usuarios puedan seleccionar administradores de departamento. Para facilitar la tarea, se creará una vista en la base de datos. La vista se compondrá solo de los datos necesarios para la lista desplegable: el nombre completo (con el formato correcto) y la clave de registro.

En **Explorador de servidores**, expanda *School. MDF*, haga clic con el botón secundario en la carpeta **views** y seleccione **Agregar nueva vista**.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image13.png)

Haga clic en **cerrar** cuando aparezca el cuadro de diálogo **Agregar tabla** y pegue la siguiente instrucción SQL en el panel SQL:

[!code-sql[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample1.sql)]

Guarde la vista como `vInstructorName`.

### <a name="updating-the-data-model"></a>Actualizar el modelo de datos

En la carpeta *Dal* , abra el archivo *SchoolModel. edmx* , haga clic con el botón secundario en la superficie de diseño y seleccione **Actualizar modelo desde base de datos**.

[![Image07](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image15.png)

En el cuadro de diálogo **Elija los objetos de base de datos** , seleccione la pestaña **Agregar** y seleccione la vista que acaba de crear.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image17.png)

Haga clic en **Finalizar**.

En el diseñador, verá que la herramienta creó una entidad `vInstructorName` y una nueva asociación entre las entidades `Department` y `Person`.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image19.png)

> [!NOTE]
> En las ventanas **salida** y **lista de errores** , es posible que vea un mensaje de advertencia que le informa de que la herramienta creó automáticamente una clave principal para la nueva vista `vInstructorName`. Este es el comportamiento normal.

Cuando se hace referencia a la nueva entidad `vInstructorName` en el código, no se desea usar la Convención de base de datos de prefijo "v" en minúsculas. Por lo tanto, cambiará el nombre de la entidad y del conjunto de entidades en el modelo.

Abra el **Explorador de modelos**. Verá `vInstructorName` enumerada como un tipo de entidad y una vista.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image21.png)

En **SchoolModel** (no en **SchoolModel. Store**), haga clic con el botón secundario en **vInstructorName** y seleccione **propiedades**. En la ventana **propiedades** , cambie la propiedad **nombre** a "InstructorName" y cambie la propiedad nombre del **conjunto de entidades** por "InstructorNames".

[![Image15](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image24.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image23.png)

Guarde y cierre el modelo de datos y, a continuación, recompile el proyecto.

## <a name="using-a-repository-class-and-an-objectdatasource-control"></a>Usar una clase de repositorio y un control ObjectDataSource

Cree un nuevo archivo de clase en la carpeta *Dal* , asígnele el nombre *SchoolRepository.CS*y reemplace el código existente por el código siguiente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample2.cs)]

Este código proporciona un único método de `GetDepartments` que devuelve todas las entidades del conjunto de entidades `Departments`. Como sabe que va a tener acceso a la propiedad de navegación `Person` para cada fila devuelta, especifique la carga diligente para esa propiedad mediante el método `Include`. La clase también implementa la interfaz `IDisposable` para asegurarse de que la conexión a la base de datos se libera cuando se desecha el objeto.

> [!NOTE]
> Una práctica común es crear una clase de repositorio para cada tipo de entidad. En este tutorial, se usa una clase de repositorio para varios tipos de entidad. Para obtener más información sobre el patrón de repositorio, consulte las publicaciones en [el blog del equipo de Entity Framework](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) y el [blog de Julia Lerman](http://thedatafarm.com/blog/data-access/agile-ef4-repository-part-3-fine-tuning-the-repository/).

El método `GetDepartments` devuelve un objeto `IEnumerable` en lugar de un objeto `IQueryable` para asegurarse de que la colección devuelta se puede usar incluso después de que se elimine el propio objeto de repositorio. Un objeto `IQueryable` puede producir el acceso a la base de datos cada vez que se tiene acceso a él, pero el objeto de repositorio podría desecharse en el momento en que un control enlazado a datos intenta representar los datos. Podría devolver otro tipo de colección, como un objeto `IList` en lugar de un objeto `IEnumerable`. Sin embargo, la devolución de un objeto `IEnumerable` garantiza que puede realizar tareas habituales de procesamiento de lista de solo lectura como bucles de `foreach` y consultas LINQ, pero no puede Agregar o quitar elementos de la colección, lo que puede implicar que tales cambios se conserven en la base de datos.

Cree una página *departments. aspx* que use la página maestra *site. Master* y agregue el marcado siguiente en el control `Content` denominado `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample3.aspx)]

Este marcado crea un control `ObjectDataSource` que utiliza la clase de repositorio que acaba de crear y un control `GridView` para mostrar los datos. El control `GridView` especifica comandos de **edición** y **eliminación** , pero no ha agregado código para admitirlos todavía.

Varias columnas usan controles de `DynamicField` para que pueda aprovechar las ventajas de la funcionalidad de validación y formato de datos automática. Para que funcionen, deberá llamar al método `EnableDynamicData` en el controlador de eventos `Page_Init`. (`DynamicControl` los controles no se usan en el campo `Administrator` porque no funcionan con las propiedades de navegación).

Los atributos de `Vertical-Align="Top"` se convertirán en importantes más adelante cuando agregue una columna que tenga un control de `GridView` anidado a la cuadrícula.

Abra el archivo *departments.aspx.CS* y agregue la siguiente instrucción `using`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample4.cs)]

A continuación, agregue el siguiente controlador para el evento de `Init` de la página:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample5.cs)]

En la carpeta *Dal* , cree un nuevo archivo de clase denominado *Department.CS* y reemplace el código existente por el código siguiente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample6.cs)]

Este código agrega metadatos al modelo de datos. Especifica que la propiedad `Budget` de la entidad `Department` realmente representa la moneda, aunque su tipo de datos sea `Decimal`y especifica que el valor debe estar comprendido entre 0 y $1.000.000,00. También especifica que la propiedad `StartDate` debe tener el formato de fecha en formato mm/dd/aaaa.

Ejecute la página *departments. aspx* .

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image26.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image25.png)

Tenga en cuenta que, aunque no especificó una cadena de formato en el marcado de página *departments. aspx* para las columnas **presupuesto** o **fecha de inicio** , el formato de fecha y moneda predeterminado se les ha aplicado mediante los controles `DynamicField`, usando los metadatos que proporcionó en el archivo *Department.CS* .

## <a name="adding-insert-and-delete-functionality"></a>Agregar funcionalidad de inserción y eliminación

Abra *SchoolRepository.CS*y agregue el código siguiente para crear un método de `Insert` y un método `Delete`. El código también incluye un método denominado `GenerateDepartmentID` que calcula el siguiente valor de clave de registro disponible para su uso por el método `Insert`. Esto es necesario porque la base de datos no está configurada para calcular esto automáticamente para la tabla `Department`.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample7.cs)]

### <a name="the-attach-method"></a>Método Attach

El método `DeleteDepartment` llama al método `Attach` para volver a establecer el vínculo que se mantiene en el administrador de estado de objetos del contexto del objeto entre la entidad en la memoria y la fila de base de datos que representa. Esto debe ocurrir antes de que el método llame al método `SaveChanges`.

El término *contexto del objeto* hace referencia a la clase Entity Framework que se deriva de la clase `ObjectContext` que se usa para tener acceso a las entidades y los conjuntos de entidades. En el código de este proyecto, la clase se denomina `SchoolEntities`y una instancia de se denomina siempre `context`. El *Administrador de estado de objetos* del contexto del objeto es una clase que se deriva de la clase `ObjectStateManager`. El contacto de objeto utiliza el administrador de estado de objetos para almacenar objetos de entidad y para realizar un seguimiento de si cada uno está sincronizado con su fila o filas de tabla correspondientes en la base de datos.

Cuando se lee una entidad, el contexto del objeto la almacena en el administrador de estado de objetos y realiza un seguimiento de si esa representación del objeto está sincronizada con la base de datos. Por ejemplo, si cambia un valor de propiedad, se establece una marca para indicar que la propiedad modificada ya no está sincronizada con la base de datos. A continuación, al llamar al método `SaveChanges`, el contexto del objeto sabe qué hacer en la base de datos porque el administrador de estado de objetos sabe exactamente cuáles son las diferencias entre el estado actual de la entidad y el estado de la base de datos.

Sin embargo, este proceso no funciona normalmente en una aplicación Web, porque la instancia del contexto del objeto que lee una entidad, junto con todo en el administrador de estado de objetos, se elimina después de representar una página. La instancia del contexto del objeto que debe aplicar los cambios es una nueva instancia de con la que se crean instancias para el procesamiento de postback. En el caso del método `DeleteDepartment`, el control `ObjectDataSource` vuelve a crear la versión original de la entidad a partir de los valores del estado de vista, pero esta entidad que se ha vuelto a crear `Department` no existe en el administrador de estado de objetos. Si llamó al método `DeleteObject` en esta entidad que se ha vuelto a crear, se produciría un error en la llamada porque el contexto del objeto no sabe si la entidad está sincronizada con la base de datos. Sin embargo, al llamar al método `Attach` se vuelve a establecer el mismo seguimiento entre la entidad que se ha vuelto a crear y los valores de la base de datos que se realizaron originalmente automáticamente cuando la entidad se leyó en una instancia anterior del contexto del objeto.

Hay ocasiones en las que no desea que el contexto del objeto realice el seguimiento de las entidades en el administrador de estado de objetos, y puede establecer marcas para evitar que lo haga. En los tutoriales posteriores de esta serie se muestran ejemplos de esto.

### <a name="the-savechanges-method"></a>El método SaveChanges

Esta clase de repositorio simple muestra los principios básicos de cómo realizar operaciones CRUD. En este ejemplo, se llama al método `SaveChanges` inmediatamente después de cada actualización. En una aplicación de producción podría querer llamar al método `SaveChanges` desde un método independiente para proporcionar más control sobre cuándo se actualiza la base de datos. (Al final del tutorial siguiente encontrará un vínculo a una nota del producto que describe el patrón de unidad de trabajo, que es un enfoque para coordinar las actualizaciones relacionadas). Observe también que en el ejemplo, el método `DeleteDepartment` no incluye el código para controlar los conflictos de simultaneidad. código que se va a agregar en un tutorial posterior de esta serie.

### <a name="retrieving-instructor-names-to-select-when-inserting"></a>Recuperando nombres de instructor para seleccionar al insertar

Los usuarios deben poder seleccionar un administrador de una lista de instructores en una lista desplegable al crear nuevos departamentos. Por consiguiente, agregue el código siguiente a *SchoolRepository.CS* para crear un método para recuperar la lista de instructores mediante la vista que creó anteriormente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample8.cs)]

### <a name="creating-a-page-for-inserting-departments"></a>Crear una página para insertar departamentos

Cree una página *DepartmentsAdd. aspx* que use la página *site. Master* y agregue el marcado siguiente en el control `Content` denominado `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample9.aspx)]

Este marcado crea dos controles `ObjectDataSource`, uno para insertar nuevas entidades `Department` y otro para recuperar los nombres de los instructores para el control `DropDownList` que se usa para seleccionar administradores de departamento. El marcado crea un control de `DetailsView` para la entrada de nuevos departamentos y especifica un controlador para el evento de `ItemInserting` del control, de modo que pueda establecer el valor de clave externa de `Administrator`. Al final hay un control de `ValidationSummary` para mostrar los mensajes de error.

Abra *DepartmentsAdd.aspx.CS* y agregue la siguiente instrucción `using`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample10.cs)]

Agregue la siguiente variable y métodos de clase:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample11.cs)]

El método `Page_Init` habilita la funcionalidad de datos dinámicos. El controlador del evento de `Init` del control `DropDownList` guarda una referencia al control y el controlador del evento `Inserting` del control `DetailsView` utiliza esa referencia para obtener el valor `PersonID` del instructor seleccionado y actualizar la propiedad `Administrator` clave externa de la entidad `Department`.

Ejecute la página, agregue información para un nuevo departamento y, a continuación, haga clic en el vínculo **Insertar** .

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image28.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image27.png)

Escriba los valores de otro departamento nuevo. Escriba un número mayor que 1.000.000,00 en el campo **presupuesto** y la pestaña en el campo siguiente. Aparece un asterisco en el campo y, si mantiene el puntero del mouse sobre él, puede ver el mensaje de error que especificó en los metadatos de ese campo.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image30.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image29.png)

Haga clic en **Insertar**y verá el mensaje de error que muestra el control `ValidationSummary` en la parte inferior de la página.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image32.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image31.png)

A continuación, cierre el explorador y abra la página *departments. aspx* . Agregue la funcionalidad de eliminación a la página *departments. aspx* agregando un `DeleteMethod` atributo al control `ObjectDataSource` y un atributo `DataKeyNames` al control `GridView`. Ahora, las etiquetas de apertura de estos controles son similares a las del ejemplo siguiente:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample12.aspx)]

Ejecute la página.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image34.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image33.png)

Elimine el Departamento que agregó al ejecutar la página *DepartmentsAdd. aspx* .

## <a name="adding-update-functionality"></a>Agregar funcionalidad de actualización

Abra *SchoolRepository.CS* y agregue el siguiente método de `Update`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample13.cs)]

Al hacer clic en **Actualizar** en la página *departments. aspx* , el control `ObjectDataSource` crea dos entidades `Department` para pasar al método `UpdateDepartment`. Uno contiene los valores originales que se han almacenado en el estado de vista y el otro contiene los nuevos valores que se especificaron en el control `GridView`. El código del método `UpdateDepartment` pasa la entidad `Department` que tiene los valores originales al método `Attach` para establecer el seguimiento entre la entidad y lo que hay en la base de datos. A continuación, el código pasa la entidad `Department` que tiene los valores nuevos al método `ApplyCurrentValues`. El contexto del objeto compara los valores antiguos y nuevos. Si un valor nuevo es diferente de un valor anterior, el contexto del objeto cambia el valor de la propiedad. A continuación, el método `SaveChanges` actualiza solo las columnas modificadas en la base de datos. (Sin embargo, si la función de actualización de esta entidad se asignó a un procedimiento almacenado, toda la fila se actualizaría independientemente de las columnas que se cambiaran).

Abra el archivo *departments. aspx* y agregue los siguientes atributos al control `DepartmentsObjectDataSource`:

- `UpdateMethod="UpdateDepartment"`
- `ConflictDetection="CompareAllValues"`   
 Esto hace que los valores anteriores se almacenen en el estado de vista para que se puedan comparar con los nuevos valores del método `Update`.
- `OldValuesParameterFormatString="orig{0}"`   
 Esto informa al control de que el nombre del parámetro de valores originales es `origDepartment`.

El marcado de la etiqueta de apertura del control `ObjectDataSource` ahora es similar al ejemplo siguiente:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample14.aspx)]

Agregue un atributo `OnRowUpdating="DepartmentsGridView_RowUpdating"` al control `GridView`. Lo usará para establecer el valor de la propiedad `Administrator` en función de la fila que el usuario seleccione en una lista desplegable. La etiqueta de apertura de `GridView` ahora es similar al ejemplo siguiente:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample15.aspx)]

Agregue un control de `EditItemTemplate` para la columna `Administrator` al control `GridView`, inmediatamente después del control `ItemTemplate` para esa columna:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample16.aspx)]

Este control `EditItemTemplate` es similar al control `InsertItemTemplate` en la página *DepartmentsAdd. aspx* . La diferencia es que el valor inicial del control se establece mediante el atributo `SelectedValue`.

Antes del control `GridView`, agregue un control `ValidationSummary` como hizo en la página *DepartmentsAdd. aspx* .

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample17.aspx)]

Abra *departments.aspx.CS* y inmediatamente después de la declaración de clase parcial, agregue el código siguiente para crear un campo privado para hacer referencia al control `DropDownList`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample18.cs)]

A continuación, agregue Controladores para el evento `Init` del control `DropDownList` y el evento `RowUpdating` del control `GridView`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample19.cs)]

El controlador del evento `Init` guarda una referencia al control `DropDownList` en el campo de clase. El controlador del evento `RowUpdating` usa la referencia para obtener el valor que el usuario ha escrito y aplicarlo a la propiedad `Administrator` de la entidad `Department`.

Use la página *DepartmentsAdd. aspx* para agregar un nuevo departamento y, a continuación, ejecute la página *departments. aspx* y haga clic en **Editar** en la fila que ha agregado.

> [!NOTE]
> No podrá modificar filas que no haya agregado (es decir, que ya estaban en la base de datos), debido a que los datos no son válidos en la base de datos. los administradores de las filas que se crearon con la base de datos son estudiantes. Si intenta editar uno de ellos, aparecerá una página de error que informa de un error como `'InstructorsDropDownList' has a SelectedValue which is invalid because it does not exist in the list of items.`

[![Image10](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image36.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image35.png)

Si escribe una cantidad de **presupuesto** no válida y, a continuación, hace clic en **Actualizar**, verá el mismo asterisco y el mismo mensaje de error que vio en la página *departments. aspx* .

Cambie un valor de campo o seleccione un administrador diferente y haga clic en **Actualizar**. Se muestra el cambio.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image38.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image37.png)

Esto completa la introducción al uso del control `ObjectDataSource` para las operaciones CRUD (creación, lectura, actualización y eliminación) básicas con la Entity Framework. Ha creado una aplicación de n niveles simple, pero la capa de lógica de negocios todavía está estrechamente acoplada a la capa de acceso a datos, lo que complica las pruebas unitarias automatizadas. En el tutorial siguiente, verá cómo implementar el patrón de repositorio para facilitar las pruebas unitarias.

> [!div class="step-by-step"]
> [Siguiente](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
