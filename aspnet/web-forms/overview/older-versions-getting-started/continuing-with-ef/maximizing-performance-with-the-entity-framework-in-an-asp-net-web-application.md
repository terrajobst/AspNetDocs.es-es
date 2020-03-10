---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
title: Maximizar el rendimiento con el Entity Framework 4,0 en una aplicación Web de ASP.NET 4 | Microsoft Docs
author: tdykstra
description: Esta serie de tutoriales se basa en la aplicación web contoso University que crea el Introducción con la serie de tutoriales Entity Framework 4,0. I...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 4e43455e-dfa1-42db-83cb-c987703f04b5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 5630200a1ad1d30f6d89b38e15179f15b699fa9f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78439267"
---
# <a name="maximizing-performance-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Maximizar el rendimiento con el Entity Framework 4,0 en una aplicación Web de ASP.NET 4

por [Tom Dykstra](https://github.com/tdykstra)

> Esta serie de tutoriales se basa en la aplicación web contoso University que crea el [Introducción con la](https://asp.net/entity-framework/tutorials#Getting%20Started) serie de tutoriales Entity Framework 4,0. Si no completó los tutoriales anteriores, como punto de partida para este tutorial, puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que habría creado. También puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) que se crea en la serie completa de tutoriales. Si tiene alguna pregunta sobre los tutoriales, puede publicarlos en el [Foro de Entity Framework de ASP.net](https://forums.asp.net/1227.aspx).

En el tutorial anterior, vio cómo controlar los conflictos de simultaneidad. En este tutorial se muestran las opciones para mejorar el rendimiento de una aplicación Web ASP.NET que usa el Entity Framework. Conocerá varios métodos para maximizar el rendimiento o para diagnosticar problemas de rendimiento.

La información que se presenta en las secciones siguientes es probable que sea útil en una amplia variedad de escenarios:

- Cargar datos relacionados de forma eficaz.
- Administrar el estado de vista.

La información que se presenta en las secciones siguientes puede ser útil si tiene consultas individuales que presentan problemas de rendimiento:

- Use la opción de fusión mediante combinación de `NoTracking`.
- Compile previamente las consultas LINQ.
- Examine los comandos de consulta enviados a la base de datos.

La información que se presenta en la sección siguiente es muy útil para las aplicaciones que tienen modelos de datos extremadamente grandes:

- Generar previamente las vistas.

> [!NOTE]
> El rendimiento de las aplicaciones web se ve afectado por muchos factores, como el tamaño de los datos de solicitud y respuesta, la velocidad de las consultas de base de datos, el número de solicitudes que el servidor puede poner en cola y la rapidez con la que pueden atenderla, e incluso la eficacia de cualquier bibliotecas de scripts de cliente que puede usar. Si el rendimiento es crítico en la aplicación, o si las pruebas o la experiencia muestran que el rendimiento de la aplicación no es satisfactorio, debe seguir el protocolo normal para el ajuste del rendimiento. Mida para determinar dónde se producen los cuellos de botella de rendimiento y, a continuación, solucione las áreas que tendrán el mayor impacto en el rendimiento general de la aplicación.
> 
> Este tema se centra principalmente en las maneras en las que puede mejorar el rendimiento específicamente del Entity Framework en ASP.NET. Estas sugerencias son útiles si determina que el acceso a datos es uno de los cuellos de botella de rendimiento de la aplicación. A menos que se indique lo contrario, los métodos que se explican aquí no se deben tener en cuenta &quot;prácticas recomendadas&quot; en general; muchos de ellos solo son adecuados en situaciones excepcionales o para abordar tipos muy específicos de cuellos de botella de rendimiento.

Para iniciar el tutorial, inicie Visual Studio y abra la aplicación web contoso University con la que estaba trabajando en el tutorial anterior.

## <a name="efficiently-loading-related-data"></a>Cargar datos relacionados de forma eficaz

Los Entity Framework pueden cargar datos relacionados en las propiedades de navegación de una entidad de varias maneras:

- *Carga diferida*. Cuando la entidad se lee por primera vez, no se recuperan datos relacionados. Pero la primera vez que intente obtener acceso a una propiedad de navegación, se recuperan automáticamente los datos necesarios para esa propiedad de navegación. Esto da como resultado que se envíen varias consultas a la base de datos, una para la propia entidad y otra cada vez que se deben recuperar los datos relacionados de la entidad. 

    [![Image05](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

*Carga diligente*. Cuando se lee la entidad, junto a ella se recuperan datos relacionados. Esto normalmente da como resultado una única consulta de combinación en la que se recuperan todos los datos que se necesitan. La carga diligente se especifica mediante el método `Include`, como ya se ha visto en estos tutoriales.

[![Image07](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

- *Carga explícita*. Esto es similar a la carga diferida, salvo que recupera explícitamente los datos relacionados en el código; no se produce automáticamente cuando se obtiene acceso a una propiedad de navegación. Los datos relacionados se cargan manualmente mediante el método `Load` de la propiedad de navegación para las colecciones, o se usa el método `Load` de la propiedad Reference para las propiedades que contienen un solo objeto. (Por ejemplo, se llama al método `PersonReference.Load` para cargar la propiedad de navegación `Person` de una entidad `Department`).

    [![Image06](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Dado que no recuperan los valores de propiedad de inmediato, la carga diferida y la carga explícita también se conocen como *carga diferida*.

La carga diferida es el comportamiento predeterminado de un contexto del objeto generado por el diseñador. Si abre el archivo *SchoolModel.Designer.CS* que define la clase de contexto del objeto, encontrará tres métodos de constructor, y cada uno de ellos incluirá la siguiente instrucción:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

En general, si sabe que necesita datos relacionados para cada entidad recuperada, la carga diligente ofrece el mejor rendimiento, ya que una sola consulta que se envía a la base de datos suele ser más eficaz que las consultas independientes para cada entidad recuperada. Por otro lado, si necesita tener acceso a las propiedades de navegación de una entidad solo con poca frecuencia o solo para un pequeño conjunto de entidades, la carga diferida o la carga explícita pueden ser más eficaces, porque la carga diligente recuperaría más datos de los que necesita.

En una aplicación Web, la carga diferida puede tener un valor relativamente bajo, ya que las acciones del usuario que afectan a la necesidad de datos relacionados tienen lugar en el explorador, que no tiene conexión con el contexto del objeto que representó la página. Por otro lado, al enlazar un control de datos, normalmente sabe qué datos necesita y, por lo general, es mejor elegir la carga diligente o la carga aplazada en función de lo que sea adecuado en cada escenario.

Además, un control DataBound podría usar un objeto entidad después de desechar el contexto del objeto. En ese caso, se producirá un error al intentar cargar de una propiedad de navegación. El mensaje de error que recibe es claro: &quot;`The ObjectContext instance has been disposed and can no longer be used for operations that require a connection.`&quot;

El control `EntityDataSource` deshabilita la carga diferida de forma predeterminada. En el caso del control de `ObjectDataSource` que está usando para el tutorial actual (o si tiene acceso al contexto del objeto desde el código de la página), hay varias maneras de hacer que la carga diferida esté deshabilitada de forma predeterminada. Puede deshabilitarlo al crear una instancia de un contexto del objeto. Por ejemplo, puede Agregar la siguiente línea al método constructor de la clase `SchoolRepository`:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Para la aplicación contoso University, hará que el contexto del objeto deshabilite automáticamente la carga diferida para que no sea necesario establecer esta propiedad cada vez que se cree una instancia de un contexto.

Abra el modelo de datos *SchoolModel. edmx* , haga clic en la superficie de diseño y, a continuación, en el panel Propiedades, establezca la propiedad **carga diferida habilitada** en `False`. Guarde y cierre el modelo de datos.

[![Image04](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

## <a name="managing-view-state"></a>Administrar el estado de vista

Para proporcionar la funcionalidad de actualización, una página web de ASP.NET debe almacenar los valores de propiedad originales de una entidad cuando se representa una página. Durante el procesamiento de los postback, el control puede volver a crear el estado original de la entidad y llamar al método `Attach` de la entidad antes de aplicar los cambios y llamar al método `SaveChanges`. De forma predeterminada, los controles de datos de formularios Web Forms de ASP.NET usan el estado de vista para almacenar los valores originales. Sin embargo, el estado de vista puede afectar al rendimiento, porque se almacena en campos ocultos que pueden aumentar considerablemente el tamaño de la página que se envía a y desde el explorador.

Las técnicas para administrar el estado de vista, o alternativas como el estado de sesión, no son únicas en el Entity Framework, por lo que este tutorial no entra en este tema en detalle. Para obtener más información, consulte los vínculos al final del tutorial.

Sin embargo, la versión 4 de ASP.NET proporciona una nueva forma de trabajar con el estado de vista que todos los desarrolladores de ASP.NET de aplicaciones de formularios Web Forms deben tener en cuenta: la propiedad `ViewStateMode`. Esta nueva propiedad se puede establecer en el nivel de página o de control y le permite deshabilitar el estado de vista de forma predeterminada en una página y habilitarla solo para los controles que la necesiten.

En el caso de las aplicaciones en las que el rendimiento es crítico, se recomienda deshabilitar siempre el estado de vista en el nivel de página y habilitarlo solo para los controles que lo requieran. Este método no disminuirá sustancialmente el tamaño del estado de vista en las páginas de Contoso University, pero para ver cómo funciona, lo hará en la página *instructors. aspx* . Esa página contiene muchos controles, incluido un control `Label` que tiene deshabilitado el estado de vista. Ninguno de los controles de esta página realmente necesita tener habilitado el estado de vista. (La propiedad `DataKeyNames` del control `GridView` especifica el estado que se debe mantener entre los postbacks, pero estos valores se mantienen en el estado del control, lo que no se ve afectado por la propiedad `ViewStateMode`).

La Directiva de `Page` y el marcado de control de `Label` actualmente son similares al ejemplo siguiente:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

Se han realizado los siguientes cambios:

- Agregue `ViewStateMode="Disabled"` a la Directiva `Page`.
- Quite `ViewStateMode="Disabled"` del control `Label`.

El marcado ahora es similar al ejemplo siguiente:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

El estado de vista ahora está deshabilitado para todos los controles. Si posteriormente agrega un control que necesita usar el estado de vista, lo único que debe hacer es incluir el `ViewStateMode="Enabled"` atributo para ese control.

## <a name="using-the-notracking-merge-option"></a>Usar la opción de combinación NoTracking

Cuando un contexto de objeto recupera filas de base de datos y crea objetos de entidad que las representan, de forma predeterminada también realiza el seguimiento de los objetos de entidad mediante su administrador de estado de objetos. Estos datos de seguimiento actúan como una memoria caché y se usan al actualizar una entidad. Dado que una aplicación web tiene normalmente instancias de contexto de objeto de corta duración, las consultas a menudo devuelven datos de los que no es necesario realizar un seguimiento, ya que el contexto del objeto que los Lee se eliminará antes de que cualquiera de las entidades que lee se vuelva a usar o se actualice.

En el Entity Framework, puede especificar si el contexto del objeto realiza un seguimiento de los objetos de entidad mediante la configuración de una *opción de combinación*. Puede establecer la opción de fusión mediante combinación para consultas individuales o para conjuntos de entidades. Si lo establece para un conjunto de entidades, significa que está estableciendo la opción de fusión mediante combinación predeterminada para todas las consultas que se crean para ese conjunto de entidades.

En el caso de la aplicación contoso University, no es necesario realizar el seguimiento de ninguno de los conjuntos de entidades a los que se tiene acceso desde el repositorio, por lo que puede establecer la opción de fusión mediante combinación en `NoTracking` para esos conjuntos de entidades cuando cree una instancia del contexto del objeto en la clase repository. (Tenga en cuenta que en este tutorial, el establecimiento de la opción de fusión mediante combinación no tendrá ningún efecto apreciable en el rendimiento de la aplicación. La opción `NoTracking` es probable que solo haga una mejora del rendimiento observable en ciertos escenarios de gran volumen de datos).

En la carpeta DAL, abra el archivo *SchoolRepository.CS* y agregue un método de constructor que establezca la opción de fusión mediante combinación para los conjuntos de entidades a los que el repositorio tiene acceso:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

## <a name="pre-compiling-linq-queries"></a>Compilar previamente consultas LINQ

La primera vez que el Entity Framework ejecuta una consulta de Entity SQL dentro de la vida de una determinada instancia de `ObjectContext`, se tarda algún tiempo en compilar la consulta. El resultado de la compilación se almacena en caché, lo que significa que las ejecuciones posteriores de la consulta son mucho más rápidas. Las consultas LINQ siguen un patrón similar, salvo que parte del trabajo necesario para compilar la consulta se realiza cada vez que se ejecuta la consulta. En otras palabras, para las consultas LINQ, de forma predeterminada, no todos los resultados de la compilación se almacenan en caché.

Si tiene una consulta LINQ que espera ejecutar repetidamente en la vida de un contexto del objeto, puede escribir código que haga que todos los resultados de la compilación se almacenen en la memoria caché la primera vez que se ejecute la consulta LINQ.

Como ejemplo, lo hará para dos métodos `Get` en la clase `SchoolRepository`, uno de los cuales no toma ningún parámetro (el método `GetInstructorNames`) y otro que requiere un parámetro (el método `GetDepartmentsByAdministrator`). Ya no es necesario compilar estos métodos, ya que no es necesario compilarlos porque no son consultas LINQ:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

Sin embargo, para que pueda probar las consultas compiladas, continuará como si se hubiesen escrito como las siguientes consultas LINQ:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Puede cambiar el código de estos métodos a lo que se muestra anteriormente y ejecutar la aplicación para comprobar que funciona antes de continuar. Sin embargo, las instrucciones siguientes pasan directamente a la creación de versiones compiladas previamente.

Cree un archivo de clase en la carpeta *Dal* , asígnele el nombre *SchoolEntities.CS*y reemplace el código existente por el código siguiente:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Este código crea una clase parcial que extiende la clase de contexto del objeto generado automáticamente. La clase parcial incluye dos consultas LINQ compiladas mediante el método `Compile` de la clase `CompiledQuery`. También crea métodos que puede usar para llamar a las consultas. Guarde y cierre este archivo.

A continuación, en *SchoolRepository.CS*, cambie los métodos `GetInstructorNames` y `GetDepartmentsByAdministrator` existentes en la clase de repositorio para que llamen a las consultas compiladas:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

Ejecute la página *departments. aspx* para comprobar que funciona como antes. Se llama al método `GetInstructorNames` para rellenar la lista desplegable administrador y se llama al método `GetDepartmentsByAdministrator` al hacer clic en **Actualizar** para comprobar que ningún instructor sea un administrador de más de un departamento.

[![Image03](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

Ha compilado previamente las consultas en la aplicación contoso University solo para ver cómo hacerlo, no porque mejoraría el rendimiento. La precompilación de consultas LINQ agrega un nivel de complejidad al código, por lo que debe asegurarse de hacerlo solo para las consultas que realmente representan cuellos de botella de rendimiento en la aplicación.

## <a name="examining-queries-sent-to-the-database"></a>Examinar consultas enviadas a la base de datos

Cuando está investigando problemas de rendimiento, a veces es útil conocer los comandos SQL exactos que el Entity Framework envía a la base de datos. Si está trabajando con un objeto `IQueryable`, una manera de hacerlo es usar el método `ToTraceString`.

En *SchoolRepository.CS*, cambie el código del método `GetDepartmentsByName` para que coincida con el ejemplo siguiente:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.cs)]

La variable `departments` se debe convertir en un tipo `ObjectQuery` solo porque el método `Where` al final de la línea anterior crea un objeto `IQueryable`. sin el método `Where`, la conversión no sería necesaria.

Establezca un punto de interrupción en la línea de `return` y, a continuación, ejecute la página *departments. aspx* en el depurador. Cuando alcance el punto de interrupción, examine la variable `commandText` en la ventana **variables locales** y use el visualizador de texto (la lupa en la columna **valor** ) para mostrar su valor en la ventana del **visualizador de texto** . Puede ver el comando SQL completo que es el resultado de este código:

[![Image08](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Como alternativa, la característica IntelliTrace de Visual Studio Ultimate proporciona una manera de ver los comandos SQL generados por el Entity Framework que no requieren que se cambie el código ni siquiera se establezca un punto de interrupción.

> [!NOTE]
> Solo puede realizar los siguientes procedimientos si tiene Visual Studio Ultimate.

Restaure el código original en el método `GetDepartmentsByName` y, a continuación, ejecute la página *departments. aspx* en el depurador.

En Visual Studio, seleccione el menú **depurar** , **IntelliTrace**y **eventos de IntelliTrace**.

[![Image11](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

En la ventana **IntelliTrace** , haga clic en **interrumpir todo**.

[![Image12](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

La ventana **IntelliTrace** muestra una lista de eventos recientes:

[![Image09](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Haga clic en la línea **ADO.net** . Se expande para mostrar el texto del comando:

[![Image10](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Puede copiar la cadena de texto de comando completa en el Portapapeles desde la ventana **variables locales** .

Supongamos que estaba trabajando con una base de datos con más tablas, relaciones y columnas que la base de datos de `School` simple. Es posible que una consulta que recopile toda la información que necesita en una sola instrucción `Select` que contenga varias cláusulas de `Join` sea demasiado compleja para que funcione de forma eficaz. En ese caso, puede cambiar de la carga diligente a la carga explícita para simplificar la consulta.

Por ejemplo, intente cambiar el código en el método `GetDepartmentsByName` de *SchoolRepository.CS*. Actualmente, en ese método tiene una consulta de objeto que tiene métodos `Include` para las propiedades de navegación `Person` y `Courses`. Reemplace la instrucción `return` por el código que realiza la carga explícita, como se muestra en el ejemplo siguiente:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Ejecute la página *departments. aspx* en el depurador y vuelva a comprobar la ventana **IntelliTrace** como hizo antes. Ahora, cuando haya una sola consulta antes, verá una larga secuencia de ellos.

[![Image13](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Haga clic en la primera línea de **ADO.net** para ver lo que ha ocurrido en la consulta compleja que vio anteriormente.

[![Image14](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

La consulta de los departamentos se ha convertido en una consulta de `Select` simple sin ninguna cláusula de `Join`, pero está seguida de consultas independientes que recuperan los cursos relacionados y un administrador, mediante un conjunto de dos consultas para cada departamento devuelto por la consulta original.

> [!NOTE]
> Si deja habilitada la carga diferida, el patrón que se ve aquí, con la misma consulta repetida muchas veces, puede deberse a la carga diferida. Un patrón que normalmente desea evitar es la carga diferida de datos relativos a cada fila de la tabla principal. A menos que haya comprobado que una única consulta de combinación es demasiado compleja para ser eficaz, normalmente podría mejorar el rendimiento en tales casos cambiando la consulta principal para usar la carga diligente.

## <a name="pre-generating-views"></a>Generar vistas previas

Cuando un objeto de `ObjectContext` se crea por primera vez en un nuevo dominio de aplicación, el Entity Framework genera un conjunto de clases que usa para tener acceso a la base de datos. Estas clases se denominan *vistas*y, si tiene un modelo de datos muy grande, la generación de estas vistas puede retrasar la respuesta del sitio web a la primera solicitud de una página después de inicializar un nuevo dominio de aplicación. Puede reducir este retraso de primera solicitud creando las vistas en tiempo de compilación en lugar de en tiempo de ejecución.

> [!NOTE]
> Si la aplicación no tiene un modelo de datos extremadamente grande, o si tiene un modelo de datos de gran tamaño, pero no le preocupa un problema de rendimiento que afecte únicamente a la primera solicitud de página después de que se recicle IIS, puede omitir esta sección. La creación de vistas no se produce cada vez que se crea una instancia de un objeto `ObjectContext`, porque las vistas se almacenan en la memoria caché del dominio de aplicación. Por lo tanto, a menos que se esté reciclando con frecuencia la aplicación en IIS, muy pocas solicitudes de página se beneficiarían de las vistas generadas previamente.

Puede generar previamente vistas mediante la herramienta de línea de comandos *EdmGen. exe* o mediante una plantilla de *texto de transformación de plantillas de texto* (T4). En este tutorial usará una plantilla T4.

En la carpeta *Dal* , agregue un archivo mediante la plantilla de **plantilla de texto** (se encuentra en el nodo **General** de la lista **plantillas instaladas** ) y asígnele el nombre *SchoolModel.views.TT*. Reemplace el código existente en el archivo por el código siguiente:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

Este código genera vistas para un archivo *. edmx* que se encuentra en la misma carpeta que la plantilla y que tiene el mismo nombre que el archivo de plantilla. Por ejemplo, si el archivo de plantilla se denomina *SchoolModel.views.TT*, buscará un archivo de modelo de datos denominado *SchoolModel. edmx*.

Guarde el archivo, haga clic con el botón derecho en el archivo en **Explorador de soluciones** y seleccione **Ejecutar herramienta personalizada**.

[![Image02](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Visual Studio genera un archivo de código que crea las vistas, que se denomina *SchoolModel.views.CS* basándose en la plantilla. (Es posible que haya observado que el archivo de código se genera incluso antes de seleccionar **Ejecutar herramienta personalizada**, tan pronto como se guarda el archivo de plantilla).

[![Image01](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Ahora puede ejecutar la aplicación y comprobar que funciona como antes.

Para obtener más información sobre las vistas generadas previamente, vea los siguientes recursos:

- [Cómo: generar previamente vistas para mejorar el rendimiento](https://msdn.microsoft.com/library/bb896240.aspx) de las consultas en el sitio web de MSDN. Explica cómo usar la herramienta de línea de comandos `EdmGen.exe` para generar previamente las vistas.
- [Aislar el rendimiento con vistas precompiladas o generadas previamente en el Entity Framework 4](https://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/isolating-performance-with-precompiled-pre-generated-views-in-the-entity-framework-4.aspx) en el blog del equipo de asesoramiento al cliente de Windows Server AppFabric.

Esto completa la introducción a la mejora del rendimiento en una aplicación Web ASP.NET que utiliza el Entity Framework. Para obtener más información, vea los siguientes recursos:

- [Consideraciones de rendimiento (Entity Framework)](https://msdn.microsoft.com/library/cc853327.aspx) en el sitio web de MSDN.
- [Publicaciones relacionadas con el rendimiento en el blog del equipo de Entity Framework](https://blogs.msdn.com/b/adonet/archive/tags/performance/).
- [Opciones de combinación de EF y consultas compiladas](https://blogs.msdn.com/b/dsimmons/archive/2010/01/12/ef-merge-options-and-compiled-queries.aspx). Entrada de blog en la que se explican los comportamientos inesperados de las consultas compiladas y las opciones de combinación como `NoTracking`. Si planea usar las consultas compiladas o manipular la configuración de las opciones de combinación en la aplicación, lea esto primero.
- [En el blog del equipo de asesoría de clientes de datos y modelado. Entity Framework](https://blogs.msdn.com/b/dmcat/archive/tags/entity+framework/) Incluye publicaciones en las consultas compiladas y el uso del generador de perfiles de Visual Studio 2010 para detectar problemas de rendimiento.
- [Entity Framework subproceso del foro con consejos sobre cómo mejorar el rendimiento de las consultas muy complejas](https://social.msdn.microsoft.com/Forums/adodotnetentityframework/thread/ffe8b2ab-c5b5-4331-8988-33a872d0b5f6).
- [Recomendaciones de administración de estado de ASP.net](https://msdn.microsoft.com/library/z1hkazw7.aspx).
- [Usar el Entity Framework y la paginación ObjectDataSource: Custom](http://geekswithblogs.net/Frez/articles/using-the-entity-framework-and-the-objectdatasource-custom-paging.aspx). Entrada de blog que se basa en la aplicación ContosoUniversity creada en estos tutoriales para explicar cómo implementar la paginación en la página *departments. aspx* .

En el siguiente tutorial se revisan algunas de las mejoras importantes en las Entity Framework que son nuevas en la versión 4.

> [!div class="step-by-step"]
> [Anterior](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
> [Siguiente](what-s-new-in-the-entity-framework-4.md)
