---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: 'Tutorial: Más información sobre los escenarios de EF avanzados para una aplicación web MVC 5'
description: En este tutorial se presentan varios temas que es útil tener en cuenta cuando va más allá de los aspectos básicos del desarrollo de aplicaciones Web ASP.NET que usan Code First de Entity Framework.
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: f35a9b0c-49ef-4cde-b06d-19d1543feb0b
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: d7cc83a5b78a60f575f5c3065079679189296a0c
ms.sourcegitcommit: f774732a3960fca079438a88a5472c37cf7be08a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/19/2019
ms.locfileid: "58425280"
---
# <a name="tutorial-learn-about-advanced-ef-scenarios-for-an-mvc-5-web-app"></a>Tutorial: Más información sobre los escenarios de EF avanzados para una aplicación web MVC 5

En el tutorial anterior, ha implementado la herencia de tabla por jerarquía. En este tutorial se presentan varios temas que es útil tener en cuenta cuando va más allá de los aspectos básicos del desarrollo de aplicaciones Web ASP.NET que usan Code First de Entity Framework. Las primeras secciones tienen instrucciones paso a paso que le guiarán a través del código y el uso de Visual Studio para completar las tareas. en las secciones siguientes se presentan varios temas con breves introducciones seguidos de vínculos a recursos para obtener más información.

Para la mayoría de estos temas, trabajará con las páginas que ya ha creado. Para usar SQL sin procesar para realizar actualizaciones masivas, creará una nueva página que actualiza el número de créditos de todos los cursos en la base de datos:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

En este tutorial ha:

> [!div class="checklist"]
> * Realiza consultas SQL sin formato
> * Realizar consultas sin seguimiento
> * Examinar consultas SQL enviadas a la base de datos

También aprenderá lo siguiente:

> [!div class="checklist"]
> * Crear una capa de abstracción
> * Clases de proxy
> * Detección de cambios automática
> * Validación automática
> * Herramientas avanzadas de Entity Framework
> * Código fuente de Entity Framework

## <a name="prerequisite"></a>Requisito previo

* [Implementar la herencia](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="perform-raw-sql-queries"></a>Realiza consultas SQL sin formato

La API de Entity Framework Code First incluye métodos que le permiten pasar comandos SQL directamente a la base de datos. Tiene las siguientes opciones:

- Use el método [DbSet. SqlQuery](https://msdn.microsoft.com/library/system.data.entity.dbset.sqlquery.aspx) para las consultas que devuelven tipos de entidad. Los objetos devueltos deben ser del tipo esperado por `DbSet` el objeto y el contexto de la base de datos realiza un seguimiento de ellos automáticamente a menos que se desactive el seguimiento. (Vea la siguiente sección sobre el método [AsNoTracking](https://msdn.microsoft.com/library/system.data.entity.dbextensions.asnotracking.aspx) ).
- Use el método [Database. SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery.aspx) para las consultas que devuelven tipos que no son entidades. No se realiza un seguimiento de los datos devueltos por el contexto de la base de datos, incluso si usa este método para recuperar tipos de entidad.
- Use la [base de datos. ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456.aspx) para los comandos que no son de consulta.

Una de las ventajas del uso de Entity Framework es que evita enlazar el código demasiado estrechamente a un método concreto de almacenamiento de datos. Lo consigue mediante la generación de consultas SQL y comandos, lo que también le evita tener que escribirlos usted mismo. Pero hay escenarios excepcionales en los que es necesario ejecutar consultas SQL específicas que se han creado manualmente, y estos métodos permiten controlar dichas excepciones.

Como siempre es true cuando ejecuta comandos SQL en una aplicación web, debe tomar precauciones para proteger su sitio contra los ataques por inyección de código SQL. Una manera de hacerlo es mediante consultas parametrizadas para asegurarse de que las cadenas enviadas por una página web no se pueden interpretar como comandos SQL. En este tutorial usará las consultas con parámetros al integrar la entrada de usuario en una consulta.

### <a name="calling-a-query-that-returns-entities"></a>Llamar a una consulta que devuelve entidades

La [clase&lt;DbSet de&gt; la carpa](https://msdn.microsoft.com/library/gg696460.aspx) proporciona un método que puede usar para ejecutar una consulta que devuelve una entidad de tipo `TEntity`. Para ver cómo funciona esto, cambiará el código en el `Details` método `Department` del controlador.

En *DepartmentController.CS*, en el `Details` método, reemplace la `db.Departments.FindAsync` llamada al método con `db.Departments.SqlQuery` una llamada al método, como se muestra en el siguiente código resaltado:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs?highlight=8-14)]

Para comprobar que el nuevo código funciona correctamente, seleccione la pestaña **Departments** y, después, **Details** para uno de los departamentos. Asegúrese de que todos los datos se muestran como se esperaba.

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Llamar a una consulta que devuelve otros tipos de objetos

Anteriormente creó una cuadrícula de estadísticas de alumno de la página About que mostraba el número de alumnos para cada fecha de inscripción. El código que lo hace en *HomeController.CS* usa LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

Supongamos que desea escribir el código que recupera estos datos directamente en SQL en lugar de usar LINQ. Para ello, debe ejecutar una consulta que devuelva un valor que no sea objetos entidad, lo que significa que debe usar el método [Database. SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery(v=VS.103).aspx) .

En *HomeController.CS*, reemplace la instrucción LINQ en el `About` método por una instrucción SQL, como se muestra en el siguiente código resaltado:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs?highlight=3-18)]

Ejecute la página about. Compruebe que muestra los mismos datos que antes.

### <a name="calling-an-update-query"></a>Llamar a una consulta Update

Supongamos que los administradores de Contoso University quieren realizar cambios masivos en la base de datos, como cambiar el número de créditos para cada curso. Si la universidad tiene un gran número de cursos, sería poco eficaz recuperarlos todos como entidades y cambiarlos de forma individual. En esta sección, implementará una página web que permite al usuario especificar un factor por el que se va a cambiar el número de créditos para todos los cursos y realizará el cambio mediante la ejecución de `UPDATE` una instrucción SQL. 

En *CourseController.CS*, agregue `UpdateCourseCredits` métodos para `HttpGet` y `HttpPost`:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Cuando el controlador procesa una `HttpGet` solicitud, no se devuelve nada en `ViewBag.RowsAffected` la variable y la vista muestra un cuadro de texto vacío y un botón de envío.

Cuando se hace clic en el botón **Actualizar** , `HttpPost` se llama al método y `multiplier` se especifica el valor en el cuadro de texto. A continuación, el código ejecuta el SQL que actualiza los cursos y devuelve el número de filas afectadas a la vista `ViewBag.RowsAffected` en la variable. Cuando la vista obtiene un valor en esa variable, muestra el número de filas actualizadas en lugar del cuadro de texto y el botón de envío.

En *CourseController.CS*, haga clic con el botón secundario `UpdateCourseCredits` en uno de los métodos y, a continuación, haga clic en **Agregar vista**. Aparece el cuadro de diálogo **Agregar vista** . Deje los valores predeterminados y seleccione **Agregar**.

En *Views\Course\UpdateCourseCredits.cshtml*, reemplace el código de plantilla por el código siguiente:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cshtml)]

Ejecute el método `UpdateCourseCredits` seleccionando la pestaña **Courses**, después, agregue "/UpdateCourseCredits" al final de la dirección URL en la barra de direcciones del explorador (por ejemplo: `http://localhost:50205/Course/UpdateCourseCredits`). Escriba un número en el cuadro de texto:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

Haga clic en **Actualizar**. Verá el número de filas afectadas.

Haga clic en **Volver a la lista** para ver la lista de cursos con el número de créditos revisado.

Para obtener más información acerca de las consultas SQL sin formato, vea [consultas SQL sin formato](https://msdn.microsoft.com/data/jj592907) en MSDN.

## <a name="no-tracking-queries"></a>Consultas de no seguimiento

Cuando un contexto de base de datos recupera las filas de tabla y crea objetos de entidad que las representa, de forma predeterminada realiza el seguimiento de si las entidades en memoria están sincronizadas con el contenido de la base de datos. Los datos en memoria actúan como una caché y se usan cuando se actualiza una entidad. Este almacenamiento en caché suele ser necesario en una aplicación web porque las instancias de contexto normalmente son de corta duración (para cada solicitud se crea una y se elimina) y el contexto que lee una entidad normalmente se elimina antes de volver a usar esa entidad.

Puede deshabilitar el seguimiento de objetos entidad en la memoria mediante el método [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) . Los siguientes son escenarios típicos en los que es posible que quiera hacer esto:

- Una consulta recupera un gran volumen de datos que la desactivación del seguimiento puede mejorar notablemente el rendimiento.
- Quiere adjuntar una entidad para actualizarla, pero antes recuperó la misma entidad para un propósito diferente. Como el contexto de base de datos ya está realizando el seguimiento de la entidad, no se puede adjuntar la entidad que se quiere cambiar. Una manera de controlar esta situación es usar la `AsNoTracking` opción con la consulta anterior.

Para obtener un ejemplo en el que se muestra cómo usar el método [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) , vea [la versión anterior de este tutorial](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). En esta versión del tutorial no se establece la marca Modified en una entidad creada por el enlazador de modelos en el método Edit, por `AsNoTracking`lo que no es necesario.

## <a name="examine-sql-sent-to-database"></a>Examinar SQL enviado a la base de datos

A veces resulta útil poder ver las consultas SQL reales que se envían a la base de datos. En un tutorial anterior vio cómo hacerlo en el código del interceptor; Ahora verá algunas maneras de hacerlo sin escribir código de interceptor. Para probarlo, verá una consulta simple y, a continuación, examinará lo que le sucede a medida que agrega opciones como la carga diligente, el filtrado y la ordenación.

En *Controllers/CourseController*, reemplace `Index` el método por el código siguiente para detener temporalmente la carga diligente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

Ahora, establezca un punto de `return` interrupción en la instrucción (F9 con el cursor en esa línea). Presione **F5** para ejecutar el proyecto en modo de depuración y seleccione la página índice del curso. Cuando el código alcance el punto de interrupción, `sql` examine la variable. Verá la consulta que se envía a SQL Server. Se trata de una `Select` instrucción sencilla.

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.json)]

Haga clic en la lupa para ver la consulta en el **visualizador de texto**.

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Ahora agregará una lista desplegable a la página de índice de cursos para que los usuarios puedan filtrar por un departamento determinado. Ordenará los cursos por título y especificará la carga diligente para la `Department` propiedad de navegación.

En *CourseController.CS*, reemplace el `Index` método por el código siguiente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Restaure el punto de `return` interrupción en la instrucción.

El método recibe el valor seleccionado de la lista desplegable en el `SelectedDepartment` parámetro. Si no se selecciona nada, este parámetro será null.

Una `SelectList` colección que contiene todos los departamentos se pasa a la vista de la lista desplegable. Los parámetros que se pasan `SelectList` al constructor especifican el nombre del campo de valor, el nombre del campo de texto y el elemento seleccionado.

En el caso del `Course` `Department` método del repositorio, el código especifica una expresión de filtro, un criterio de ordenación y la carga diligente para la propiedad de navegación. `Get` La expresión de filtro siempre `true` devuelve si no hay nada seleccionado en la lista desplegable (es decir `SelectedDepartment` , es null).

En *Views\Course\Index.cshtml*, inmediatamente antes de la `table` etiqueta de apertura, agregue el código siguiente para crear la lista desplegable y un botón de envío:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Con el punto de interrupción todavía establecido, ejecute la página de índice del curso. Continúe a la primera vez que el código llegue a un punto de interrupción, de modo que la página se muestre en el explorador. Seleccione un departamento en la lista desplegable y haga clic en **filtro**.

Esta vez, el primer punto de interrupción será para la consulta de departamentos de la lista desplegable. Omitir y ver la `query` variable la próxima vez que el código alcance el punto de interrupción para ver el `Course` aspecto de la consulta. Verá algo parecido a lo siguiente:

[!code-sql[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.sql)]

Puede ver que la `JOIN` consulta es ahora una consulta que carga `Department` datos junto con los `Course` datos y que incluye una `WHERE` cláusula.

Quite la `var sql = courses.ToString()` línea.

## <a name="create-an-abstraction-layer"></a>Crea una capa de abstracción

Muchos desarrolladores escriben código para implementar el repositorio y una unidad de patrones de trabajo como un contenedor en torno al código que funciona con Entity Framework. Estos patrones están previstos para crear una capa de abstracción entre la capa de acceso de datos y la capa de lógica de negocios de una aplicación. Implementar estos patrones puede ayudar a aislar la aplicación de cambios en el almacén de datos y puede facilitar la realización de pruebas unitarias automatizadas o el desarrollo controlado por pruebas (TDD). Sin embargo, escribir código adicional para implementar estos patrones no es siempre la mejor opción para las aplicaciones que usan EF, por varias razones:

- La propia clase de contexto de EF aísla el código del código específico del almacén de datos.
- La clase de contexto de EF puede actuar como una clase de unidad de trabajo para las actualizaciones de base de datos que hace con EF.
- Las características introducidas en Entity Framework 6 facilitan la implementación de TDD sin escribir código de repositorio.

Para obtener más información sobre cómo implementar los patrones de repositorio y unidad de trabajo, consulte [la versión Entity Framework 5 de esta serie de tutoriales](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md). Para obtener información sobre las formas de implementar TDD en Entity Framework 6, consulte los siguientes recursos:

- [Cómo EF6 permite simular DbSets con más facilidad](http://thedatafarm.com/data-access/how-ef6-enables-mocking-dbsets-more-easily/)
- [Probar con un marco ficticio](https://msdn.microsoft.com/data/dn314429)
- [Pruebas con sus propias dobles de pruebas](https://msdn.microsoft.com/data/dn314431)

<a id="proxies"></a>

## <a name="proxy-classes"></a>Clases de proxy

Cuando el Entity Framework crea instancias de entidad (por ejemplo, al ejecutar una consulta), suele crearlas como instancias de un tipo derivado generado dinámicamente que actúa como un proxy para la entidad. Por ejemplo, vea las dos imágenes del depurador siguientes. En la primera imagen, verá que la `student` variable es el tipo esperado `Student` inmediatamente después de crear una instancia de la entidad. En la segunda imagen, después de que se haya usado EF para leer una entidad Student de la base de datos, verá la clase de proxy.

![Antes de la clase de proxy](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

![Después de la clase proxy](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Esta clase de proxy invalida algunas propiedades virtuales de la entidad para insertar enlaces para realizar acciones automáticamente cuando se tiene acceso a la propiedad. Una función para la que se utiliza este mecanismo es la carga diferida.

La mayoría de las veces no es necesario tener en cuenta este uso de servidores proxy, pero hay excepciones:

- En algunos escenarios podría querer evitar que el Entity Framework Cree instancias de proxy. Por ejemplo, al serializar entidades, normalmente desea las clases POCO, no las clases de proxy. Una manera de evitar problemas de serialización es serializar los objetos de transferencia de datos (DTO) en lugar de los objetos entidad, como se muestra en el tutorial uso de la [API Web con Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md) . Otra manera es deshabilitar la [creación del proxy](https://msdn.microsoft.com/data/jj592886.aspx).
- Cuando se crean instancias de una clase de entidad `new` mediante el operador, no se obtiene una instancia de proxy. Esto significa que no obtendrá funcionalidad como la carga diferida y el seguimiento automático de cambios. Normalmente está bien. por lo general, no se necesita la carga diferida, ya que se está creando una nueva entidad que no está en la base de datos y, por lo general, no `Added`se necesita seguimiento de cambios si se marca explícitamente la entidad como. Sin embargo, si necesita una carga diferida y necesita seguimiento de cambios, puede crear nuevas instancias de entidad con servidores proxy mediante el método [Create](https://msdn.microsoft.com/library/gg679504.aspx) de `DbSet` la clase.
- Es posible que desee obtener un tipo de entidad real a partir de un tipo de proxy. Puede usar el método [GetObjectType](https://msdn.microsoft.com/library/system.data.objects.objectcontext.getobjecttype.aspx) de la `ObjectContext` clase para obtener el tipo de entidad real de una instancia de tipo de proxy.

Para obtener más información, vea [trabajar con servidores proxy](https://msdn.microsoft.com/data/JJ592886.aspx) en MSDN.

## <a name="automatic-change-detection"></a>Detección de cambios automática

Entity Framework determina cómo ha cambiado una entidad (y, por tanto, las actualizaciones que hay que enviar a la base de datos) comparando los valores actuales de una entidad con los valores originales. Cuando se consulta o se adjunta la entidad, se almacenan los valores originales. Algunos de los métodos que provocan la detección de cambios automática son los siguientes:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Si está realizando el seguimiento de un gran número de entidades y llama a uno de estos métodos muchas veces en un bucle, podría obtener mejoras de rendimiento significativas desactivando temporalmente la detección de cambios automática mediante la propiedad [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx) . Para obtener más información, vea [detección automática de cambios](https://msdn.microsoft.com/data/jj556205) en MSDN.

## <a name="automatic-validation"></a>Validación automática

Cuando se llama al `SaveChanges` método, de forma predeterminada, el Entity Framework valida los datos de todas las propiedades de todas las entidades modificadas antes de actualizar la base de datos. Si ha actualizado un gran número de entidades y ya ha validado los datos, este trabajo no es necesario y puede hacer que el proceso de guardar los cambios tarde menos tiempo en desactivar la validación de forma temporal. Puede hacerlo mediante la propiedad [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx) . Para obtener más información, vea [validación](https://msdn.microsoft.com/data/gg193959) en MSDN.

## <a name="entity-framework-power-tools"></a>Herramientas avanzadas de Entity Framework

[Entity Framework Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) es un complemento de Visual Studio que se usó para crear los diagramas de modelo de datos que se muestran en estos tutoriales. Las herramientas también pueden realizar otras funciones, como generar clases de entidad basadas en las tablas de una base de datos existente para que pueda usar la base de datos con Code First. Después de instalar las herramientas, aparecen algunas opciones adicionales en los menús contextuales. Por ejemplo, al hacer clic con el botón secundario en la clase de contexto en **Explorador de soluciones**, verá y **Entity Framework** opción. Esto le ofrece la posibilidad de generar un diagrama. Cuando se usa Code First no se puede cambiar el modelo de datos en el diagrama, pero se pueden mover elementos para que sea más fácil de entender.

![Diagrama EF](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image15.png)

## <a name="entity-framework-source-code"></a>Código fuente de Entity Framework

El código fuente de Entity Framework 6 está disponible en [GitHub](https://github.com/aspnet/EntityFramework6). Puede archivar errores y puede aportar sus propias mejoras al código fuente de EF.

Aunque el código fuente está abierto, Entity Framework es totalmente compatible como producto de Microsoft. El equipo de Microsoft Entity Framework mantiene el control sobre qué contribuciones se aceptan y comprueba todos los cambios de código para garantizar la calidad de cada versión.

## <a name="acknowledgments"></a>Agradecimientos

- Tom Dykstra escribió la versión original de este tutorial, creó conjuntamente la actualización EF 5 y escribió la actualización EF 6. Tom es redactor de programación Senior en el equipo de contenido de herramientas y plataforma web de Microsoft.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (Twitter [@RickAndMSFT](http://twitter.com/RickAndMSFT)) ha realizado la mayor parte del trabajo al actualizar el tutorial de EF 5 y MVC 4 y coautor de la actualización de EF 6. Rick es un redactor de programación Senior para Microsoft que se centra en Azure y MVC.
- [Rowan Miller](http://www.romiller.com) y otros miembros del equipo de Entity Framework asistieron a las revisiones de código y ayudó a depurar muchos problemas con las migraciones que surgieron mientras actualizamos el tutorial para EF 5 y EF 6.

## <a name="troubleshoot-common-errors"></a>Solución de errores comunes

### <a name="cannot-createshadow-copy"></a>No se puede crear o copiar instantáneas

Mensaje de error:

> No se puede crear ni instantánea&lt;'&gt;nombredearchivo ' cuando el archivo ya existe.

Solución

Espere unos segundos y actualice la página.

### <a name="update-database-not-recognized"></a>Update: no se reconoce la base de datos

Mensaje de error (desde `Update-Database` el comando en la PMC):

> El término ' Update-Database ' no se reconoce como nombre de un cmdlet, función, archivo de script o programa ejecutable. Compruebe la ortografía del nombre, o bien, si se incluyó una ruta de acceso, compruebe que la ruta de acceso es correcta y vuelva a intentarlo.

Solución

Salga de Visual Studio. Vuelva a abrir el proyecto e inténtelo de nuevo.

### <a name="validation-failed"></a>Error de validación

Mensaje de error (desde `Update-Database` el comando en la PMC):

> Error de validación de una o más entidades. Vea la propiedad ' EntityValidationErrors ' para obtener más detalles.

Solución

Una causa de este problema son los errores de validación `Seed` cuando se ejecuta el método. Vea [bases de los Entity Framework de propagación y depuración (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) para obtener sugerencias `Seed` sobre cómo depurar el método.

### <a name="http-50019-error"></a>Error HTTP 500,19

Mensaje de error:

> Error HTTP 500,19-error interno del servidor no se puede tener acceso a la página solicitada porque los datos de configuración relacionados para la página no son válidos.

Solución

Una manera de obtener este error es que se pueden obtener varias copias de la solución, cada una de ellas con el mismo número de puerto. Normalmente puede resolver este problema saliendo de todas las instancias de Visual Studio y, a continuación, reinicie el proyecto en el que está trabajando. Si eso no funciona, pruebe a cambiar el número de puerto. Haga clic con el botón derecho en el archivo de proyecto y después haga clic en propiedades. Seleccione la pestaña **Web** y, a continuación, cambie el número de puerto en el cuadro de texto **dirección URL del proyecto** .

### <a name="error-locating-sql-server-instance"></a>Error al buscar la instancia de SQL Server

Mensaje de error:

> Error relacionado con la red o específico de la instancia mientras se establecía una conexión con el servidor SQL Server. No se encontró el servidor o éste no estaba accesible. Compruebe que el nombre de la instancia es correcto y que SQL Server está configurado para admitir conexiones remotas. (proveedor: Interfaces de red SQL, error: 26: error al buscar el servidor o la instancia especificados)

Solución

Compruebe la cadena de conexión. Si ha eliminado manualmente la base de datos, cambie el nombre de la base de datos en la cadena de construcción.

## <a name="get-the-code"></a>Obtención del código

[Descargar proyecto completado](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Recursos adicionales

 Para obtener más información sobre cómo trabajar con datos mediante el Entity Framework, consulte la [Página de documentación de EF en MSDN](https://msdn.microsoft.com/data/ee712907) y [ASP.net Data Access-Recommended](../../../../whitepapers/aspnet-data-access-content-map.md)Resources.

Para obtener más información sobre cómo implementar la aplicación web después de haberla creado, vea [ASP.net web Deployment-Recommended](../../../../whitepapers/aspnet-web-deployment-content-map.md) Resources en MSDN Library.

Para obtener información sobre otros temas relacionados con MVC, como la autenticación y la autorización, vea los [recursos de ASP.NET MVC recomendados](../recommended-resources-for-mvc.md).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha:

> [!div class="checklist"]
> * Realizado consultas SQL sin formato
> * Se realizaron consultas sin seguimiento
> * Consultas SQL examinadas enviadas a la base de datos

También ha aprendido lo siguiente:

> [!div class="checklist"]
> * Crear una capa de abstracción
> * Clases de proxy
> * Detección de cambios automática
> * Validación automática
> * Herramientas avanzadas de Entity Framework
> * Código fuente de Entity Framework

Con esto se completa esta serie de tutoriales sobre el uso de la Entity Framework en una aplicación ASP.NET MVC. Si desea obtener más información sobre EF Database First, consulte la serie del primer tutorial de la base de BD.
> [!div class="nextstepaction"]
> [Entity Framework Database First](../database-first-development/setting-up-database.md)