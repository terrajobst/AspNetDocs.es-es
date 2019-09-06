---
uid: web-forms/overview/data-access/caching-data/caching-data-at-application-startup-cs
title: Almacenar datos en caché al iniciarC#la aplicación () | Microsoft Docs
author: rick-anderson
description: En cualquier aplicación Web, algunos datos se utilizarán con frecuencia y algunos datos se usarán con poca frecuencia. Podemos mejorar el rendimiento de nuestra aplicación ASP.NET b...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 22ca8efa-7cd1-45a7-b9ce-ce6eb3b3ff95
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-at-application-startup-cs
msc.type: authoredcontent
ms.openlocfilehash: a0b55b0df1b7843120de284891e16178df23fabe
ms.sourcegitcommit: fe5c7512383a9b0a05d321ff10d3cca1611556f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2019
ms.locfileid: "70386568"
---
# <a name="caching-data-at-application-startup-c"></a>Almacenar datos en caché al iniciar la aplicación (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar PDF](caching-data-at-application-startup-cs/_static/datatutorial60cs1.pdf)

> En cualquier aplicación Web, algunos datos se utilizarán con frecuencia y algunos datos se usarán con poca frecuencia. Podemos mejorar el rendimiento de nuestra aplicación ASP.NET cargando de antemano los datos usados con frecuencia, una técnica conocida como almacenamiento en caché. En este tutorial se muestra un enfoque para la carga proactiva, que consiste en cargar datos en la memoria caché al iniciarse la aplicación.

## <a name="introduction"></a>Introducción

En los dos tutoriales anteriores se vio el almacenamiento en caché de los datos en los niveles de presentación y almacenamiento en caché. En el [almacenamiento de datos en caché con ObjectDataSource](caching-data-with-the-objectdatasource-cs.md), analizamos el uso de las características de almacenamiento en caché de ObjectDataSource para almacenar los datos en la capa de presentación. [Almacenar en caché los datos de la arquitectura](caching-data-in-the-architecture-cs.md) examina Caching en un nuevo nivel de almacenamiento en caché independiente. Ambos tutoriales usaban la *carga reactiva* al trabajar con la memoria caché de datos. Con la carga reactiva, cada vez que se solicitan los datos, el sistema comprueba primero si está en la memoria caché. Si no es así, obtiene los datos del origen de origen, como la base de datos y, a continuación, los almacena en la memoria caché. La principal ventaja de la carga reactiva es su facilidad de implementación. Una de sus desventajas es el rendimiento desigual en todas las solicitudes. Imagine una página que utiliza la capa de almacenamiento en caché del tutorial anterior para mostrar la información del producto. Cuando esta página se visita por primera vez, o se visita por primera vez después de que los datos en caché se hayan expulsado debido a restricciones de memoria o hasta que se haya alcanzado la expiración especificada, se deben recuperar los datos de la base de datos. Por lo tanto, estas solicitudes de usuarios tardarán más tiempo que las solicitudes de los usuarios a las que se puede atender desde la memoria caché.

La *carga proactiva* proporciona una estrategia alternativa de administración de la memoria caché que suaviza el rendimiento en todas las solicitudes mediante la carga de los datos almacenados en caché antes de que se necesiten. Normalmente, la carga proactiva utiliza algún proceso que comprueba periódicamente o se le notifica cuando se ha producido una actualización de los datos subyacentes. Este proceso actualiza la memoria caché para mantenerla actualizada. La carga proactiva es especialmente útil si los datos subyacentes proceden de una conexión de base de datos lenta, un servicio web o algún otro origen de datos especialmente lento. Pero este enfoque para la carga proactiva es más difícil de implementar, ya que requiere crear, administrar e implementar un proceso para comprobar los cambios y actualizar la memoria caché.

Otro tipo de carga proactiva y el tipo que exploraremos en este tutorial es cargando datos en la memoria caché al iniciarse la aplicación. Este enfoque es especialmente útil para almacenar en caché datos estáticos, como los registros de las tablas de búsqueda de bases de datos.

> [!NOTE]
> Para obtener información más detallada sobre las diferencias entre la carga proactiva y reactiva, así como las listas de ventajas, desventajas y recomendaciones de implementación, consulte la sección [Administración del contenido de una caché](https://msdn.microsoft.com/library/ms978503.aspx) de la [Guía de arquitectura de almacenamiento en caché para .net Aplicaciones de Framework](https://msdn.microsoft.com/library/ms978498.aspx).

## <a name="step-1-determining-what-data-to-cache-at-application-startup"></a>Paso 1: Determinar qué datos se van a almacenar en caché al iniciar la aplicación

Los ejemplos de almacenamiento en caché que usan la carga reactiva que hemos examinado en los dos tutoriales anteriores funcionan bien con los datos que pueden cambiar periódicamente y no tardan mucho tiempo en generarse. Pero si los datos almacenados en caché no cambian nunca, la expiración usada por la carga reactiva es superflua. Del mismo modo, si los datos que se almacenan en caché tardan demasiado tiempo en generarse, los usuarios cuyas solicitudes busquen la caché vacía tendrán que superar una espera prolongada mientras se recuperan los datos subyacentes. Considere la posibilidad de almacenar en memoria caché datos y datos estáticos que tardan mucho tiempo en generarse en el inicio de la aplicación.

Mientras que las bases de datos tienen muchos valores dinámicos que cambian con frecuencia, la mayoría también tienen una cantidad equitativa de datos estáticos. Por ejemplo, prácticamente todos los modelos de datos tienen una o más columnas que contienen un valor determinado de un conjunto fijo de opciones. Una `Patients` tabla de base de datos `PrimaryLanguage` puede tener una columna, cuyo conjunto de valores puede ser inglés, español, Francés, Ruso, Japonés, etc. A menudo, estos tipos de columnas se implementan mediante *tablas de búsqueda*. En lugar de almacenar la cadena inglés o francés en `Patients` la tabla, se crea una segunda tabla con, normalmente, dos columnas: un identificador único y una descripción de cadena, con un registro para cada valor posible. La `PrimaryLanguage` columna de la `Patients` tabla almacena el identificador único correspondiente en la tabla de búsqueda. En la figura 1, el idioma principal del paciente John Doe es el inglés, mientras que Ed Johnson es ruso.

![La tabla de idiomas es una tabla de búsqueda utilizada por la tabla patients](caching-data-at-application-startup-cs/_static/image1.png)

**Figura 1**: La `Languages` tabla es una tabla de búsqueda utilizada por `Patients` la tabla

La interfaz de usuario para editar o crear un nuevo paciente incluirá una lista desplegable de idiomas permitidos rellenados por los registros `Languages` de la tabla. Sin almacenamiento en caché, cada vez que se visita esta interfaz, el `Languages` sistema debe consultar la tabla. Esto es excesivo e innecesario, ya que los valores de la tabla de búsqueda cambian con poca frecuencia, si es que se producen.

Podríamos almacenar en caché `Languages` los datos con las mismas técnicas de carga reactiva examinadas en los tutoriales anteriores. Sin embargo, la carga reactiva utiliza una expiración basada en el tiempo, que no es necesaria para los datos de la tabla de búsqueda estática. Aunque el almacenamiento en caché con la carga reactiva sería mejor que no almacenar en caché, el mejor enfoque sería cargar proactivamente los datos de la tabla de búsqueda en la memoria caché al iniciarse la aplicación.

En este tutorial, veremos cómo almacenar en caché los datos de la tabla de búsqueda y otra información estática.

## <a name="step-2-examining-the-different-ways-to-cache-data"></a>Paso 2: Examinar las distintas formas de almacenar datos en caché

La información se puede almacenar en caché mediante programación en una aplicación de ASP.NET mediante diversos enfoques. Ya vimos cómo usar la memoria caché de datos en los tutoriales anteriores. Como alternativa, los objetos se pueden almacenar en caché mediante programación con *miembros estáticos* o el estado de la *aplicación*.

Al trabajar con una clase, normalmente se debe crear una instancia de la clase antes de que se pueda tener acceso a sus miembros. Por ejemplo, para invocar un método de una de las clases de nuestra capa de lógica de negocios, primero debemos crear una instancia de la clase:

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample1.cs)]

Antes de poder invocar *SomeMethod* o trabajar con *SomeProperty*, primero debemos crear una instancia de la clase con la `new` palabra clave. *SomeMethod* y *SomeProperty* se asocian a una instancia determinada. La duración de estos miembros está ligada a la duración de su objeto asociado. Los *miembros estáticos*, por otro lado, son variables, propiedades y métodos que se comparten entre *todas* las instancias de la clase y, por consiguiente, tienen una duración siempre que la clase. Los miembros estáticos se indican mediante `static`la palabra clave.

Además de los miembros estáticos, los datos se pueden almacenar en caché con el estado de la aplicación. Cada aplicación de ASP.NET mantiene una colección de nombres y valores que se comparte entre todos los usuarios y páginas de la aplicación. Se puede tener acceso a esta colección mediante la [ `Application` propiedad](https://msdn.microsoft.com/library/system.web.httpcontext.application.aspx)de la [ `HttpContext` clase](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)y usarse desde la clase de código subyacente de una página de ASP.net como la siguiente:

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample2.cs)]

La memoria caché de datos proporciona una API mucho más rica para almacenar datos en caché, proporcionando mecanismos para expiraciones basadas en tiempo y dependencias, prioridades de elementos de caché, etc. Con los miembros estáticos y el estado de la aplicación, el desarrollador de páginas debe agregar manualmente dichas características. Sin embargo, cuando se almacenan en caché los datos durante el inicio de la aplicación durante el período de duración de la aplicación, se mootn las ventajas de la memoria caché de datos. En este tutorial veremos el código que usa las tres técnicas para almacenar en caché los datos estáticos.

## <a name="step-3-caching-thesupplierstable-data"></a>Paso 3: Almacenar en`Suppliers`caché los datos de la tabla

Las tablas de base de datos Northwind que hemos implementado hasta la fecha no incluyen ninguna tabla de búsqueda tradicional. Las cuatro DataTables implementadas en las tablas de modelo DAL All cuyos valores no son estáticos. En lugar de dedicar tiempo a agregar un nuevo DataTable a la capa Dal y, después, a una nueva clase y a los métodos de la capa BLL, en `Suppliers` este tutorial vamos a fingir que los datos de la tabla son estáticos. Por lo tanto, podríamos almacenar en caché estos datos en el inicio de la aplicación.

Para empezar, cree una nueva clase denominada `StaticCache.cs` en la `CL` carpeta.

![Crear la clase StaticCache.cs en la carpeta CL](caching-data-at-application-startup-cs/_static/image2.png)

**Figura 2**: Crear la `StaticCache.cs` clase en la `CL` carpeta

Necesitamos agregar un método que cargue los datos en el inicio en el almacén de caché adecuado, así como los métodos que devuelven datos de esta caché.

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample3.cs)]

En el código anterior se usa una variable miembro `suppliers`estática,, para almacenar los resultados `SuppliersBLL` del `GetSuppliers()` método de la clase, al que se `LoadStaticCache()` llama desde el método. El `LoadStaticCache()` método está pensado para ser llamado durante el inicio de la aplicación. Una vez que estos datos se han cargado durante el inicio de la aplicación, cualquier página que necesite trabajar con los `StaticCache` datos de `GetSuppliers()` proveedor puede llamar al método de la clase. Por lo tanto, la llamada a la base de datos para obtener los proveedores solo se produce una vez, en el inicio de la aplicación.

En lugar de usar una variable miembro estática como almacén de caché, podríamos haber usado el estado de la aplicación o la memoria caché de datos. En el código siguiente se muestra la clase retools para usar el estado de la aplicación:

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample4.cs)]

En `LoadStaticCache()`, la información del proveedor se almacena en la *clave*de variable de la aplicación. Se devuelve como el tipo adecuado (`Northwind.SuppliersDataTable`) de. `GetSuppliers()` Aunque se puede obtener acceso al estado de la aplicación en las clases de código subyacente de `Application["key"]`las páginas ASP.net con, en la `HttpContext.Current.Application["key"]` arquitectura se debe usar para obtener `HttpContext`el objeto actual.

Del mismo modo, la memoria caché de datos se puede usar como almacén de caché, como se muestra en el código siguiente:

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample5.cs)]

Para agregar un elemento a la memoria caché de datos sin expiración basada en el tiempo, `System.Web.Caching.Cache.NoAbsoluteExpiration` Utilice `System.Web.Caching.Cache.NoSlidingExpiration` los valores y como parámetros de entrada. Esta sobrecarga concreta del método de `Insert` la caché de datos se ha seleccionado para que se pueda especificar la *prioridad* del elemento de la memoria caché. La prioridad se usa para determinar qué elementos se borrarán de la memoria caché cuando la memoria disponible se ejecute bajo. Aquí usamos la prioridad `NotRemovable`, lo que garantiza que este elemento de caché no se borrará.

> [!NOTE]
> La descarga de este tutorial implementa la `StaticCache` clase mediante el enfoque de variable de miembro estático. El código de las técnicas de estado de la aplicación y caché de datos está disponible en los comentarios del archivo de clase.

## <a name="step-4-executing-code-at-application-startup"></a>Paso 4: Ejecutar código al iniciar la aplicación

Para ejecutar código cuando se inicia una aplicación web por primera vez, es necesario crear un archivo `Global.asax`especial denominado. Este archivo puede contener controladores de eventos para eventos de nivel de aplicación, sesión y solicitud, y aquí es donde podemos agregar código que se ejecutará cada vez que se inicie la aplicación.

Agregue el `Global.asax` archivo al directorio raíz de la aplicación web; para ello, haga clic con el botón derecho en el nombre del proyecto de sitio web en el explorador de soluciones de Visual Studio y elija Agregar nuevo elemento. En el cuadro de diálogo Agregar nuevo elemento, seleccione el tipo de elemento clase de aplicación global y, a continuación, haga clic en el botón Agregar.

> [!NOTE]
> Si ya tiene un `Global.asax` archivo en el proyecto, el tipo de elemento de clase de aplicación global no se mostrará en el cuadro de diálogo Agregar nuevo elemento.

[![Agregar el archivo global. asax al directorio raíz de la aplicación Web](caching-data-at-application-startup-cs/_static/image4.png)](caching-data-at-application-startup-cs/_static/image3.png)

**Figura 3**: Agregue el `Global.asax` archivo al directorio raíz de la aplicación web ([haga clic para ver la imagen de tamaño completo](caching-data-at-application-startup-cs/_static/image5.png))

La plantilla `Global.asax` de archivo predeterminada incluye cinco métodos en una etiqueta del `<script>` lado servidor:

- **`Application_Start`** se ejecuta cuando se inicia la aplicación web por primera vez.
- **`Application_End`** se ejecuta cuando se está cerrando la aplicación
- **`Application_Error`** se ejecuta siempre que una excepción no controlada llega a la aplicación.
- **`Session_Start`** se ejecuta cuando se crea una nueva sesión
- **`Session_End`** se ejecuta cuando una sesión expira o se abandona

Solo `Application_Start` se llama a este controlador de eventos una vez durante el ciclo de vida de una aplicación. La aplicación se inicia la primera vez que se solicita un recurso de ASP.net de la aplicación y continúa ejecutándose hasta que se reinicia la aplicación, lo que puede ocurrir si se modifica el contenido `/Bin` de la carpeta, `Global.asax`se modifica, se modifica el contenido de la `App_Code` carpeta o modificación del `Web.config` archivo, entre otras causas. Consulte información general sobre el ciclo de vida de la [aplicación ASP.net](https://msdn.microsoft.com/library/ms178473.aspx) para obtener una explicación más detallada sobre el ciclo de vida de la aplicación.

Para estos tutoriales, solo necesitamos agregar código al `Application_Start` método, así que no dude en quitar los demás. En `Application_Start`, basta con llamar `StaticCache` al método `LoadStaticCache()` de la clase, que cargará y almacenará en caché la información del proveedor:

[!code-aspx[Main](caching-data-at-application-startup-cs/samples/sample6.aspx)]

Así de simple. Al iniciarse la aplicación `LoadStaticCache()` , el método tomará la información del proveedor de la capa BLL y la almacenará en una variable miembro estática (o en el almacén de caché `StaticCache` que haya terminado usando en la clase). Para comprobar este comportamiento, establezca un punto de interrupción `Application_Start` en el método y ejecute la aplicación. Tenga en cuenta que el punto de interrupción se alcanza en el inicio de la aplicación. Sin embargo, las solicitudes posteriores no hacen que `Application_Start` el método se ejecute.

[![Usar un punto de interrupción para comprobar que se está ejecutando el controlador de eventos Application_Start](caching-data-at-application-startup-cs/_static/image7.png)](caching-data-at-application-startup-cs/_static/image6.png)

**Figura 4**: Use un punto de interrupción para comprobar `Application_Start` que el controlador de eventos se está ejecutando ([haga clic para ver la imagen de tamaño completo](caching-data-at-application-startup-cs/_static/image8.png))

> [!NOTE]
> Si no alcanza el punto de `Application_Start` interrupción la primera vez que inicia la depuración, se debe a que ya se ha iniciado la aplicación. Fuerce la aplicación para que se reinicie mediante `Global.asax` la `Web.config` modificación de los archivos o y, a continuación, vuelva a intentarlo. Simplemente puede Agregar (o quitar) una línea en blanco al final de uno de estos archivos para reiniciar rápidamente la aplicación.

## <a name="step-5-displaying-the-cached-data"></a>Paso 5: Mostrar los datos almacenados en caché

En este momento, `StaticCache` la clase tiene una versión de los datos de proveedor almacenados en caché al iniciarse la aplicación a la `GetSuppliers()` que se puede tener acceso a través de su método. Para trabajar con estos datos desde la capa de presentación, podemos usar un ObjectDataSource o invocar mediante programación `StaticCache` el método `GetSuppliers()` de la clase desde la clase de código subyacente de una página de ASP.net. Echemos un vistazo al uso de los controles ObjectDataSource y GridView para mostrar la información de proveedor almacenada en caché.

Para empezar, Abra `AtApplicationStartup.aspx` la página en `Caching` la carpeta. Arrastre un control GridView desde el cuadro de herramientas hasta el diseñador `ID` y establezca `Suppliers`su propiedad en. A continuación, desde la etiqueta inteligente de GridView, elija crear un nuevo ObjectDataSource `SuppliersCachedDataSource`denominado. Configure ObjectDataSource para usar el `StaticCache` método de `GetSuppliers()` la clase.

[![Configurar ObjectDataSource para usar la clase StaticCache](caching-data-at-application-startup-cs/_static/image10.png)](caching-data-at-application-startup-cs/_static/image9.png)

**Figura 5**: Configurar ObjectDataSource para usar la clase `StaticCache` ([haga clic para ver la imagen de tamaño completo](caching-data-at-application-startup-cs/_static/image11.png))

[![Usar el método GetSuppliers () para recuperar los datos de proveedor almacenados en caché](caching-data-at-application-startup-cs/_static/image13.png)](caching-data-at-application-startup-cs/_static/image12.png)

**Ilustración 6**: Use el `GetSuppliers()` método para recuperar los datos de proveedor almacenados en caché ([haga clic para ver la imagen de tamaño completo](caching-data-at-application-startup-cs/_static/image14.png))

Después de completar el asistente, Visual Studio agregará automáticamente BoundFields para cada uno de los campos `SuppliersDataTable`de datos de. El marcado declarativo de GridView y ObjectDataSource debe ser similar al siguiente:

[!code-aspx[Main](caching-data-at-application-startup-cs/samples/sample7.aspx)]

En la ilustración 7 se muestra la página cuando se ve a través de un explorador. La salida es la misma en la que se extrajeron los datos `SuppliersBLL` de la clase de la `StaticCache` capa BLL, pero el uso de la clase devuelve los datos del proveedor como almacenamiento en caché al iniciar la aplicación. Puede establecer puntos de interrupción en el `StaticCache` método de `GetSuppliers()` la clase para comprobar este comportamiento.

[![Los datos de proveedor almacenados en caché se muestran en un control GridView](caching-data-at-application-startup-cs/_static/image16.png)](caching-data-at-application-startup-cs/_static/image15.png)

**Figura 7**: Los datos de proveedor almacenados en caché se muestran en un control GridView ([haga clic para ver la imagen de tamaño completo](caching-data-at-application-startup-cs/_static/image17.png))

## <a name="summary"></a>Resumen

La mayoría de los modelos de datos contienen una cantidad equitativa de datos estáticos, que normalmente se implementan en forma de tablas de búsqueda. Dado que esta información es estática, no hay ningún motivo para tener acceso continuamente a la base de datos cada vez que es necesario mostrar esta información. Además, debido a su naturaleza estática, al almacenar en caché los datos, no es necesario que expiren. En este tutorial vimos cómo tomar estos datos y almacenarlos en caché en la memoria caché de datos, en el estado de la aplicación y a través de una variable miembro estática. Esta información se almacena en la memoria caché durante el inicio de la aplicación y permanece en la memoria caché a lo largo de la duración de la aplicación.

En este tutorial y en los dos últimos, hemos examinado los datos de almacenamiento en caché durante la duración de la aplicación, así como el uso de expiraciones basadas en el tiempo. Sin embargo, al almacenar en caché los datos de la base de datos, una expiración basada en el tiempo puede ser menor que la ideal. En lugar de vaciar periódicamente la memoria caché, sería óptimo desalojar solo el elemento almacenado en caché cuando se modifican los datos de la base de datos subyacente. Este ideal es posible mediante el uso de dependencias de caché de SQL, que examinaremos en el siguiente tutorial.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial eran Teresa Murphy y Zack Jones. ¿Está interesado en revisar los próximos artículos de MSDN? Si es así, suéltelo en una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](caching-data-in-the-architecture-cs.md)
> [Siguiente](using-sql-cache-dependencies-cs.md)
