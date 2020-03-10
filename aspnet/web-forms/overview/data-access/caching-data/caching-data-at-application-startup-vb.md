---
uid: web-forms/overview/data-access/caching-data/caching-data-at-application-startup-vb
title: Almacenar datos en caché al iniciar la aplicación (VB) | Microsoft Docs
author: rick-anderson
description: En cualquier aplicación Web, algunos datos se utilizarán con frecuencia y algunos datos se usarán con poca frecuencia. Podemos mejorar el rendimiento de nuestra aplicación ASP.NET b...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 84afe4ac-cc53-4f2e-a867-27eaf692c2df
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-at-application-startup-vb
msc.type: authoredcontent
ms.openlocfilehash: 6c07b565329ab17496d2436f4c35bc4507694ed8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78465745"
---
# <a name="caching-data-at-application-startup-vb"></a>Almacenar datos en caché al iniciar la aplicación (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar PDF](caching-data-at-application-startup-vb/_static/datatutorial60vb1.pdf)

> En cualquier aplicación Web, algunos datos se utilizarán con frecuencia y algunos datos se usarán con poca frecuencia. Podemos mejorar el rendimiento de nuestra aplicación ASP.NET cargando de antemano los datos usados con frecuencia, una técnica conocida como. En este tutorial se muestra un enfoque para la carga proactiva, que consiste en cargar datos en la memoria caché al iniciarse la aplicación.

## <a name="introduction"></a>Introducción

En los dos tutoriales anteriores se vio el almacenamiento en caché de los datos en los niveles de presentación y almacenamiento en caché. En el [almacenamiento de datos en caché con ObjectDataSource](caching-data-with-the-objectdatasource-vb.md), analizamos el uso de las características de almacenamiento en caché de ObjectDataSource para almacenar en caché los datos en el nivel de presentación. [Almacenar en caché los datos de la arquitectura](caching-data-in-the-architecture-vb.md) examina Caching en un nuevo nivel de almacenamiento en caché independiente. Ambos tutoriales usaban la *carga reactiva* al trabajar con la memoria caché de datos. Con la carga reactiva, cada vez que se solicitan los datos, el sistema comprueba primero si están en la memoria caché. Si no es así, obtiene los datos del origen de origen, como la base de datos y, a continuación, los almacena en la memoria caché. La principal ventaja de la carga reactiva es su facilidad de implementación. Una de sus desventajas es el rendimiento desigual en todas las solicitudes. Imagine una página que utiliza la capa de almacenamiento en caché del tutorial anterior para mostrar la información del producto. Cuando esta página se visita por primera vez, o se visita por primera vez después de que los datos en caché se hayan expulsado debido a restricciones de memoria o hasta que se haya alcanzado la expiración especificada, se deben recuperar los datos de la base de datos. Por lo tanto, estas solicitudes de usuarios tardarán más tiempo que las solicitudes de los usuarios a las que se puede atender desde la memoria caché.

La *carga proactiva* proporciona una estrategia alternativa de administración de la memoria caché que suaviza el rendimiento en todas las solicitudes cargando los datos almacenados en caché antes de que se necesiten. Normalmente, la carga proactiva utiliza algún proceso que comprueba periódicamente o se le notifica cuando se ha producido una actualización de los datos subyacentes. Este proceso actualiza la memoria caché para mantenerla actualizada. La carga proactiva es especialmente útil si los datos subyacentes proceden de una conexión de base de datos lenta, un servicio web o algún otro origen de datos especialmente lento. Pero este enfoque para la carga proactiva es más difícil de implementar, ya que requiere crear, administrar e implementar un proceso para comprobar los cambios y actualizar la memoria caché.

Otro tipo de carga proactiva y el tipo que exploraremos en este tutorial es cargando datos en la memoria caché al iniciarse la aplicación. Este enfoque es especialmente útil para almacenar en caché datos estáticos, como los registros de las tablas de búsqueda de bases de datos.

> [!NOTE]
> Para obtener información más detallada sobre las diferencias entre la carga proactiva y reactiva, así como las listas de ventajas, desventajas y recomendaciones de implementación, consulte la sección [Administración del contenido de una caché](https://msdn.microsoft.com/library/ms978503.aspx) de la [Guía de arquitectura de almacenamiento en caché para .NET Framework aplicaciones](https://msdn.microsoft.com/library/ms978498.aspx).

## <a name="step-1-determining-what-data-to-cache-at-application-startup"></a>Paso 1: determinar qué datos se van a almacenar en caché al iniciar la aplicación

Los ejemplos de almacenamiento en caché que usan la carga reactiva que hemos examinado en los dos tutoriales anteriores funcionan bien con los datos que pueden cambiar periódicamente y no tardan mucho tiempo en generarse. Pero si los datos almacenados en caché no cambian nunca, la expiración usada por la carga reactiva es superflua. Del mismo modo, si los datos que se almacenan en caché tardan demasiado tiempo en generarse, los usuarios cuyas solicitudes busquen la caché vacía tendrán que superar una espera prolongada mientras se recuperan los datos subyacentes. Considere la posibilidad de almacenar en memoria caché datos y datos estáticos que tardan mucho tiempo en generarse en el inicio de la aplicación.

Mientras que las bases de datos tienen muchos valores dinámicos que cambian con frecuencia, la mayoría también tienen una cantidad equitativa de datos estáticos. Por ejemplo, prácticamente todos los modelos de datos tienen una o más columnas que contienen un valor determinado de un conjunto fijo de opciones. Una tabla de base de datos de `Patients` puede tener una columna `PrimaryLanguage`, cuyo conjunto de valores puede ser inglés, español, Francés, Ruso, Japonés, etc. A menudo, estos tipos de columnas se implementan mediante *tablas de búsqueda*. En lugar de almacenar la cadena inglés o francés en el `Patients` tabla, se crea una segunda tabla con, normalmente, dos columnas: un identificador único y una descripción de cadena, con un registro para cada valor posible. La columna `PrimaryLanguage` de la tabla `Patients` almacena el identificador único correspondiente en la tabla de búsqueda. En la figura 1, el idioma principal del paciente John Doe es el inglés, mientras que Ed Johnson s es ruso.

![La tabla de idiomas es una tabla de búsqueda utilizada por la tabla patients](caching-data-at-application-startup-vb/_static/image1.png)

**Figura 1**: la tabla `Languages` es una tabla de búsqueda utilizada por la tabla `Patients`

La interfaz de usuario para editar o crear un nuevo paciente incluirá una lista desplegable de idiomas permitidos rellenados por los registros de la tabla `Languages`. Sin almacenamiento en caché, cada vez que se visita esta interfaz, el sistema debe consultar la tabla `Languages`. Esto es excesivo e innecesario, ya que los valores de la tabla de búsqueda cambian con poca frecuencia, si es que se producen.

Podríamos almacenar en caché los datos de `Languages` con las mismas técnicas de carga reactiva examinadas en los tutoriales anteriores. Sin embargo, la carga reactiva utiliza una expiración basada en el tiempo, que no es necesaria para los datos de la tabla de búsqueda estática. Aunque el almacenamiento en caché con la carga reactiva sería mejor que no almacenar en caché, el mejor enfoque sería cargar proactivamente los datos de la tabla de búsqueda en la memoria caché al iniciarse la aplicación.

En este tutorial, veremos cómo almacenar en caché los datos de la tabla de búsqueda y otra información estática.

## <a name="step-2-examining-the-different-ways-to-cache-data"></a>Paso 2: examinar las distintas formas de almacenar datos en caché

La información se puede almacenar en caché mediante programación en una aplicación de ASP.NET mediante diversos enfoques. Ya vimos cómo usar la memoria caché de datos en los tutoriales anteriores. Como alternativa, los objetos se pueden almacenar en caché mediante programación con *miembros estáticos* o el estado de la *aplicación*.

Al trabajar con una clase, normalmente se debe crear una instancia de la clase antes de que se pueda tener acceso a sus miembros. Por ejemplo, para invocar un método de una de las clases de nuestra capa de lógica de negocios, primero debemos crear una instancia de la clase:

[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample1.vb)]

Antes de poder invocar *SomeMethod* o trabajar con *SomeProperty*, primero debemos crear una instancia de la clase mediante la palabra clave `New`. *SomeMethod* y *SomeProperty* se asocian a una instancia determinada. La duración de estos miembros está ligada a la duración de su objeto asociado. Los *miembros estáticos*, por otro lado, son variables, propiedades y métodos que se comparten entre *todas* las instancias de la clase y, por consiguiente, tienen una duración siempre que la clase. Los miembros estáticos se indican mediante la palabra clave `Shared`.

Además de los miembros estáticos, los datos se pueden almacenar en caché con el estado de la aplicación. Cada aplicación de ASP.NET mantiene una colección de nombres y valores que se comparte entre todos los usuarios y páginas de la aplicación. Se puede tener acceso a esta colección mediante la propiedad [`HttpContext` Class](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) s [`Application`](https://msdn.microsoft.com/library/system.web.httpcontext.application.aspx)y usarse desde una clase de código subyacente de la página ASP.net, como la siguiente:

[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample2.vb)]

La memoria caché de datos proporciona una API mucho más rica para almacenar datos en caché, proporcionando mecanismos para expiraciones basadas en tiempo y dependencias, prioridades de elementos de caché, etc. Con los miembros estáticos y el estado de la aplicación, el desarrollador de páginas debe agregar manualmente dichas características. Sin embargo, cuando se almacenan en caché los datos en el inicio de la aplicación durante el período de duración de la aplicación, se mootn las ventajas de la memoria caché de datos. En este tutorial veremos el código que usa las tres técnicas para almacenar en caché los datos estáticos.

## <a name="step-3-caching-thesupplierstable-data"></a>Paso 3: almacenamiento en caché de los datos de la tabla`Suppliers`

Las tablas de base de datos Northwind que hemos implementado hasta la fecha no incluyen ninguna tabla de búsqueda tradicional. Las cuatro DataTables implementadas en las tablas de modelo DAL All cuyos valores no son estáticos. En lugar de dedicar tiempo a agregar un nuevo DataTable a la capa DAL y, después, a una nueva clase y a los métodos de la capa BLL, en este tutorial vamos a subastar que los datos de la `Suppliers` tabla s sean estáticos. Por lo tanto, podríamos almacenar en caché estos datos en el inicio de la aplicación.

Para empezar, cree una nueva clase denominada `StaticCache.cs` en la carpeta `CL`.

![Crear la clase StaticCache. VB en la carpeta CL](caching-data-at-application-startup-vb/_static/image2.png)

**Figura 2**: creación de la clase `StaticCache.vb` en la carpeta `CL`

Necesitamos agregar un método que cargue los datos en el inicio en el almacén de caché adecuado, así como los métodos que devuelven datos de esta caché.

[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample3.vb)]

En el código anterior se utiliza una variable miembro estática, `suppliers`, para almacenar los resultados del método `GetSuppliers()` de `SuppliersBLL` Class, al que se llama desde el método `LoadStaticCache()`. El método `LoadStaticCache()` está diseñado para ser llamado durante el inicio de la aplicación. Una vez que estos datos se han cargado durante el inicio de la aplicación, cualquier página que necesite trabajar con los datos de proveedor puede llamar al método `StaticCache` Class s `GetSuppliers()`. Por lo tanto, la llamada a la base de datos para obtener los proveedores solo se produce una vez, en el inicio de la aplicación.

En lugar de usar una variable miembro estática como almacén de caché, podríamos haber usado el estado de la aplicación o la memoria caché de datos. En el código siguiente se muestra la clase retools para usar el estado de la aplicación:

[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample4.vb)]

En `LoadStaticCache()`, la información del proveedor se almacena en la *clave*variable de la aplicación. Se devuelve como el tipo adecuado (`Northwind.SuppliersDataTable`) de `GetSuppliers()`. Aunque se puede tener acceso al estado de la aplicación en las clases de código subyacente de las páginas ASP.NET con `Application("key")`, en la arquitectura se debe usar `HttpContext.Current.Application("key")` para obtener el `HttpContext`actual.

Del mismo modo, la memoria caché de datos se puede usar como almacén de caché, como se muestra en el código siguiente:

[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample5.vb)]

Para agregar un elemento a la memoria caché de datos sin expiración basada en el tiempo, utilice los valores `System.Web.Caching.Cache.NoAbsoluteExpiration` y `System.Web.Caching.Cache.NoSlidingExpiration` como parámetros de entrada. Esta sobrecarga concreta del método `Insert` de la caché de datos se ha seleccionado para que podamos especificar la *prioridad* del elemento de caché. La prioridad se usa para determinar qué elementos se borrarán de la memoria caché cuando la memoria disponible se ejecute bajo. Aquí usamos el `NotRemovable`de prioridad, que garantiza que este elemento de caché no se borrará.

> [!NOTE]
> Esta descarga de tutoriales implementa la clase `StaticCache` mediante el enfoque de variable de miembro estático. El código de las técnicas de estado de la aplicación y caché de datos está disponible en los comentarios del archivo de clase.

## <a name="step-4-executing-code-at-application-startup"></a>Paso 4: ejecutar código al iniciar la aplicación

Para ejecutar código cuando se inicia una aplicación web por primera vez, es necesario crear un archivo especial denominado `Global.asax`. Este archivo puede contener controladores de eventos para eventos de nivel de aplicación, sesión y solicitud, y aquí es donde podemos agregar código que se ejecutará cada vez que se inicie la aplicación.

Agregue el archivo de `Global.asax` al directorio raíz de la aplicación Web. para ello, haga clic con el botón derecho en el nombre del proyecto de sitio web en Visual Studio s Explorador de soluciones y elija Agregar nuevo elemento. En el cuadro de diálogo Agregar nuevo elemento, seleccione el tipo de elemento clase de aplicación global y, a continuación, haga clic en el botón Agregar.

> [!NOTE]
> Si ya tiene un archivo `Global.asax` en el proyecto, el tipo de elemento de clase de aplicación global no se mostrará en el cuadro de diálogo Agregar nuevo elemento.

[![agregar el archivo global. asax al directorio raíz de la aplicación Web](caching-data-at-application-startup-vb/_static/image4.png)](caching-data-at-application-startup-vb/_static/image3.png)

**Figura 3**: agregar el archivo de `Global.asax` al directorio raíz de la aplicación web ([haga clic para ver la imagen de tamaño completo](caching-data-at-application-startup-vb/_static/image5.png))

La plantilla de archivo `Global.asax` predeterminada incluye cinco métodos en una etiqueta de `<script>` del lado servidor:

- **`Application_Start`** se ejecuta cuando la aplicación web se inicia por primera vez
- **`Application_End`** se ejecuta cuando se está cerrando la aplicación
- **`Application_Error`** se ejecuta siempre que una excepción no controlada llega a la aplicación
- **`Session_Start`** se ejecuta cuando se crea una nueva sesión
- **`Session_End`** se ejecuta cuando una sesión ha expirado o abandonado

Se llama al controlador de eventos `Application_Start` solo una vez durante el ciclo de vida de una aplicación. La aplicación se inicia la primera vez que se solicita un recurso ASP.NET desde la aplicación y continúa ejecutándose hasta que se reinicia la aplicación, lo que puede ocurrir si se modifica el contenido de la carpeta `/Bin`, se modifica `Global.asax`, se modifica el contenido de la carpeta `App_Code` o se modifica el archivo de `Web.config`, entre otras causas. Consulte información general sobre el ciclo de vida de la [aplicación ASP.net](https://msdn.microsoft.com/library/ms178473.aspx) para obtener una explicación más detallada sobre el ciclo de vida de la aplicación.

Para estos tutoriales, solo necesitamos agregar código al método `Application_Start`, así que no dude en quitar los demás. En `Application_Start`, solo tiene que llamar al método `StaticCache` de clase s `LoadStaticCache()`, que cargará y almacenará en caché la información del proveedor:

[!code-aspx[Main](caching-data-at-application-startup-vb/samples/sample6.aspx)]

Eso es todo lo que tiene que hacer. Durante el inicio de la aplicación, el método `LoadStaticCache()` tomará la información del proveedor de la capa BLL y la almacenará en una variable miembro estática (o en cualquier almacén de caché que haya terminado usando en la clase `StaticCache`). Para comprobar este comportamiento, establezca un punto de interrupción en el método `Application_Start` y ejecute la aplicación. Tenga en cuenta que el punto de interrupción se alcanza en el inicio de la aplicación. Sin embargo, las solicitudes posteriores no hacen que el método `Application_Start` se ejecute.

[![utilizar un punto de interrupción para comprobar que se está ejecutando el controlador de eventos Application_Start](caching-data-at-application-startup-vb/_static/image7.png)](caching-data-at-application-startup-vb/_static/image6.png)

**Figura 4**: uso de un punto de interrupción para comprobar que se está ejecutando el controlador de eventos `Application_Start` ([haga clic para ver la imagen de tamaño completo](caching-data-at-application-startup-vb/_static/image8.png))

> [!NOTE]
> Si no alcanza el punto de interrupción de `Application_Start` cuando inicia la depuración por primera vez, se debe a que ya se ha iniciado la aplicación. Forzar el reinicio de la aplicación modificando los archivos de `Global.asax` o `Web.config` e inténtelo de nuevo. Simplemente puede Agregar (o quitar) una línea en blanco al final de uno de estos archivos para reiniciar rápidamente la aplicación.

## <a name="step-5-displaying-the-cached-data"></a>Paso 5: mostrar los datos almacenados en caché

En este momento, la clase `StaticCache` tiene una versión de los datos de proveedor almacenados en caché al iniciar la aplicación a la que se puede tener acceso a través de su método `GetSuppliers()`. Para trabajar con estos datos desde la capa de presentación, se puede usar una clase ObjectDataSource o invocar mediante programación el método `StaticCache` Class s `GetSuppliers()` desde una clase de código subyacente de la página ASP.NET. Echemos un vistazo al uso de los controles ObjectDataSource y GridView para mostrar la información de proveedor almacenada en caché.

Para empezar, abra la página `AtApplicationStartup.aspx` de la carpeta `Caching`. Arrastre un control GridView desde el cuadro de herramientas hasta el diseñador y establezca su propiedad `ID` en `Suppliers`. A continuación, desde la etiqueta inteligente de GridView s, elija crear un nuevo ObjectDataSource denominado `SuppliersCachedDataSource`. Configure ObjectDataSource para usar el método de `GetSuppliers()` de `StaticCache` Class s.

[![configurar ObjectDataSource para usar la clase StaticCache](caching-data-at-application-startup-vb/_static/image10.png)](caching-data-at-application-startup-vb/_static/image9.png)

**Figura 5**: configuración de ObjectDataSource para usar la clase `StaticCache` ([haga clic para ver la imagen de tamaño completo](caching-data-at-application-startup-vb/_static/image11.png))

[![usar el método GetSuppliers () para recuperar los datos de proveedor almacenados en caché](caching-data-at-application-startup-vb/_static/image13.png)](caching-data-at-application-startup-vb/_static/image12.png)

**Figura 6**: uso del método `GetSuppliers()` para recuperar los datos de proveedor almacenados en caché ([haga clic para ver la imagen de tamaño completo](caching-data-at-application-startup-vb/_static/image14.png))

Después de completar el asistente, Visual Studio agregará automáticamente BoundFields para cada uno de los campos de datos de `SuppliersDataTable`. El marcado declarativo de GridView y ObjectDataSource s debería tener un aspecto similar al siguiente:

[!code-aspx[Main](caching-data-at-application-startup-vb/samples/sample7.aspx)]

En la ilustración 7 se muestra la página cuando se ve a través de un explorador. La salida es la misma en la que se extrajeron los datos de la clase `SuppliersBLL` BLL, pero al usar la clase `StaticCache` se devuelven los datos del proveedor como almacenamiento en caché al iniciar la aplicación. Puede establecer puntos de interrupción en el método `StaticCache` Class s `GetSuppliers()` para comprobar este comportamiento.

[![los datos de proveedor almacenados en caché se muestran en un control GridView](caching-data-at-application-startup-vb/_static/image16.png)](caching-data-at-application-startup-vb/_static/image15.png)

**Figura 7**: los datos de proveedor almacenados en caché se muestran en un control GridView ([haga clic para ver la imagen de tamaño completo](caching-data-at-application-startup-vb/_static/image17.png))

## <a name="summary"></a>Resumen

La mayoría de los modelos de datos contienen una cantidad equitativa de datos estáticos, que normalmente se implementan en forma de tablas de búsqueda. Puesto que esta información es estática, no hay ninguna razón para tener acceso continuamente a la base de datos cada vez que es necesario mostrar esta información. Además, debido a su naturaleza estática, al almacenar en caché los datos, no es necesario que expiren. En este tutorial vimos cómo tomar estos datos y almacenarlos en caché en la memoria caché de datos, en el estado de la aplicación y a través de una variable miembro estática. Esta información se almacena en la memoria caché durante el inicio de la aplicación y permanece en la memoria caché a lo largo de la duración de la aplicación.

En este tutorial y en los dos últimos, hemos examinado los datos de almacenamiento en caché para la duración de la duración de la aplicación, así como con las expiraciones basadas en el tiempo. Sin embargo, al almacenar en caché los datos de la base de datos, una expiración basada en el tiempo puede ser menor que la ideal. En lugar de vaciar periódicamente la memoria caché, sería óptimo desalojar solo el elemento almacenado en caché cuando se modifican los datos de la base de datos subyacente. Este ideal es posible mediante el uso de dependencias de caché de SQL, que examinaremos en el siguiente tutorial.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial eran Teresa Murphy y Zack Jones. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](caching-data-in-the-architecture-vb.md)
> [Siguiente](using-sql-cache-dependencies-vb.md)
