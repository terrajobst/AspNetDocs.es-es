---
uid: web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-vb
title: Almacenar datos en caché en la arquitectura (VB) | Microsoft Docs
author: rick-anderson
description: En el tutorial anterior, aprendió a aplicar el almacenamiento en caché en el nivel de presentación. En este tutorial, aprenderá cómo sacar partido de nuestro architectu con capas...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 5e189dd7-f4f9-4f28-9b3a-6cb7d392e9c7
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-vb
msc.type: authoredcontent
ms.openlocfilehash: dc991a205fa7e61f604bc0f26e9b24b3faefd3d3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78443593"
---
# <a name="caching-data-in-the-architecture-vb"></a>Almacenar datos en caché en la arquitectura (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_59_VB.exe) de la aplicación de ejemplo o [descarga de PDF](caching-data-in-the-architecture-vb/_static/datatutorial59vb1.pdf)

> En el tutorial anterior, aprendió a aplicar el almacenamiento en caché en el nivel de presentación. En este tutorial, aprenderá a aprovechar nuestra arquitectura en capas para almacenar en caché los datos en el nivel de lógica de negocios. Para ello, se extiende la arquitectura para incluir una capa de almacenamiento en caché.

## <a name="introduction"></a>Introducción

Como vimos en el tutorial anterior, el almacenamiento en caché de los datos de ObjectDataSource es tan sencillo como establecer un par de propiedades. Desafortunadamente, ObjectDataSource aplica el almacenamiento en caché en el nivel de presentación, que acopla estrechamente las directivas de almacenamiento en caché con la página de ASP.NET. Uno de los motivos para crear una arquitectura en capas es permitir que tales acoplamientos se interrumpan. La capa de lógica de negocios, por ejemplo, desacopla la lógica de negocios de las páginas de ASP.NET, mientras que la capa de acceso a datos desacopla los detalles del acceso a datos. En parte, se prefiere este desacoplamiento de la lógica de negocios y los detalles del acceso a los datos, ya que hace que el sistema sea más legible, más fácil de mantener y más flexible para cambiar. También permite el conocimiento del dominio y la división del trabajo que un desarrollador que trabaja en el nivel de presentación no necesita estar familiarizado con los detalles de la base de datos para poder realizar su trabajo. Desacoplar la Directiva de almacenamiento en caché del nivel de presentación ofrece ventajas similares.

En este tutorial aumentaremos nuestra arquitectura para incluir una *capa de almacenamiento en caché* (o cl para abreviar) que emplea nuestra directiva de almacenamiento en caché. La capa de almacenamiento en caché incluirá una `ProductsCL` clase que proporciona acceso a la información del producto con métodos como `GetProducts()`, `GetProductsByCategoryID(categoryID)`, etc., que, cuando se invoca, intentará recuperar primero los datos de la memoria caché. Si la memoria caché está vacía, estos métodos invocarán el método `ProductsBLL` adecuado en la capa BLL, que a su vez obtendrá los datos de la capa DAL. Los métodos `ProductsCL` almacenan en caché los datos recuperados de la capa BLL antes de devolverlos.

Como se muestra en la figura 1, el CL se encuentra entre las capas de presentación y de lógica de negocios.

![La capa de almacenamiento en caché (CL) es otra capa de nuestra arquitectura](caching-data-in-the-architecture-vb/_static/image1.png)

**Figura 1**: la capa de almacenamiento en caché (cl) es otra capa de nuestra arquitectura

## <a name="step-1-creating-the-caching-layer-classes"></a>Paso 1: creación de las clases de nivel de almacenamiento en caché

En este tutorial, se creará un CL muy sencillo con una sola clase `ProductsCL` que tiene solo unos cuantos métodos. La creación de una capa de almacenamiento en caché completa para toda la aplicación requeriría la creación de `CategoriesCL`, `EmployeesCL`y `SuppliersCL` clases, y proporcionar un método en estas clases de nivel de almacenamiento en caché para cada método de acceso a datos o modificación de la capa BLL. Al igual que con BLL y DAL, la capa de almacenamiento en caché debería implementarse idealmente como un proyecto de biblioteca de clases independiente. sin embargo, se implementará como una clase en la carpeta `App_Code`.

Para separar más claramente las clases de CL de las clases DAL y BLL, cree una nueva subcarpeta en la carpeta `App_Code`. Haga clic con el botón derecho en la carpeta `App_Code` del Explorador de soluciones, elija nueva carpeta y asigne a la nueva carpeta el nombre `CL`. Después de crear esta carpeta, agregue una nueva clase denominada `ProductsCL.vb`.

![Agregue una nueva carpeta denominada CL y una clase denominada ProductsCL. VB.](caching-data-in-the-architecture-vb/_static/image2.png)

**Figura 2**: agregue una nueva carpeta denominada `CL` y una clase denominada `ProductsCL.vb`

La clase `ProductsCL` debe incluir el mismo conjunto de métodos de acceso a datos y de modificación que se encuentra en su clase de capa de lógica de negocios correspondiente (`ProductsBLL`). En lugar de crear todos estos métodos, vamos a crear un par aquí para hacerse una idea de los patrones usados por CL. En concreto, vamos a agregar los métodos `GetProducts()` y `GetProductsByCategoryID(categoryID)` en el paso 3 y una sobrecarga `UpdateProduct` en el paso 4. Puede Agregar los demás métodos de `ProductsCL` y `CategoriesCL`, `EmployeesCL`y las clases de `SuppliersCL` en el momento de su ocio.

## <a name="step-2-reading-and-writing-to-the-data-cache"></a>Paso 2: lectura y escritura en la memoria caché de datos

La característica de almacenamiento en caché de ObjectDataSource explorada en el tutorial anterior usa internamente la caché de datos ASP.NET para almacenar los datos recuperados de la capa BLL. También se puede tener acceso a la memoria caché de datos mediante programación desde las páginas de ASP.NET de código subyacente o desde las clases de la arquitectura de la aplicación Web. Para leer y escribir en la memoria caché de datos desde una clase de código subyacente de la página ASP.NET, use el siguiente patrón:

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample1.vb)]

El método [`Cache` Class](https://msdn.microsoft.com/library/system.web.caching.cache.aspx) s [`Insert`](https://msdn.microsoft.com/library/system.web.caching.cache.insert.aspx) tiene varias sobrecargas. `Cache("key") = value` y `Cache.Insert(key, value)` son sinónimos y ambos agregan un elemento a la memoria caché utilizando la clave especificada sin una expiración definida. Normalmente, queremos especificar una expiración al agregar un elemento a la memoria caché, ya sea como una dependencia, una expiración basada en el tiempo o ambos. Use una de las otras sobrecargas del método `Insert` para proporcionar información de expiración basada en la dependencia o en el tiempo.

Los métodos de la capa de almacenamiento en caché deben comprobar primero si los datos solicitados están en la memoria caché y, en caso afirmativo, devolverlos desde allí. Si los datos solicitados no están en la memoria caché, es necesario invocar el método BLL adecuado. Su valor devuelto debe almacenarse en caché y devolverse, como se muestra en el siguiente diagrama de secuencia.

![Los métodos de nivel de almacenamiento en caché devuelven datos de la memoria caché si están disponibles](caching-data-in-the-architecture-vb/_static/image3.png)

**Figura 3**: los métodos de la capa de almacenamiento en caché devuelven datos de la memoria caché si están disponibles

La secuencia que se muestra en la figura 3 se realiza en las clases de CL con el siguiente patrón:

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample2.vb)]

Aquí, *tipo* es el tipo de datos que se almacenan en la memoria caché `Northwind.ProductsDataTable`, por ejemplo, mientras que la *clave* es la clave que identifica de forma única el elemento de la memoria caché. Si el elemento con la *clave* especificada no está en la memoria caché, se `Nothing`rá la *instancia* y los datos se recuperarán del método BLL adecuado y se agregarán a la memoria caché. En el momento en que se alcanza `Return instance`, la *instancia* contiene una referencia a los datos, ya sea desde la memoria caché o extraída de la capa BLL.

Asegúrese de usar el patrón anterior al obtener acceso a los datos de la memoria caché. El siguiente patrón, que, a primera vista, parece equivalente, contiene una diferencia sutil que presenta una condición de carrera. Las condiciones de carrera son difíciles de depurar porque se revelan a sí mismas esporádicamente y son difíciles de reproducir.

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample3.vb)]

La diferencia en este segundo, fragmento de código incorrecto es que, en lugar de almacenar una referencia al elemento almacenado en caché en una variable local, se tiene acceso a la memoria caché de datos directamente en la instrucción condicional *y* en el `Return`. Imagine que cuando se alcanza este código, `Cache("key")` no se `Nothing`, pero antes de que se alcance la instrucción `Return`, la *clave* del sistema expulsa de la memoria caché. En este caso excepcional, el código devolverá `Nothing` en lugar de un objeto del tipo esperado.

> [!NOTE]
> La caché de datos es segura para subprocesos, por lo que no es necesario sincronizar el acceso a los subprocesos para lecturas o escrituras simples. Sin embargo, si necesita realizar varias operaciones en los datos de la memoria caché que deben ser atómicos, es responsable de implementar un bloqueo o algún otro mecanismo para garantizar la seguridad para subprocesos. Vea [sincronizar el acceso a la memoria caché de ASP.net](http://www.ddj.com/184406369) para obtener más información.

Un elemento se puede desalojar mediante programación de la caché de datos mediante el [método`Remove`](https://msdn.microsoft.com/library/system.web.caching.cache.remove.aspx) como el siguiente:

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample4.vb)]

## <a name="step-3-returning-product-information-from-theproductsclclass"></a>Paso 3: devolver información del producto de la clase`ProductsCL`

En este tutorial, se permite a s implementar dos métodos para devolver información del producto de la clase `ProductsCL`: `GetProducts()` y `GetProductsByCategoryID(categoryID)`. Al igual que con la clase `ProductsBL` en el nivel de lógica de negocios, el método `GetProducts()` de CL devuelve información sobre todos los productos como un objeto `Northwind.ProductsDataTable`, mientras que `GetProductsByCategoryID(categoryID)` devuelve todos los productos de una categoría especificada.

En el código siguiente se muestra una parte de los métodos de la clase `ProductsCL`:

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample5.vb)]

En primer lugar, tenga en cuenta los atributos `DataObject` y `DataObjectMethodAttribute` aplicados a la clase y los métodos. Estos atributos proporcionan información al asistente de ObjectDataSource s, que indica qué clases y métodos deben aparecer en los pasos del asistente. Dado que se tendrá acceso a las clases y métodos de CL desde un ObjectDataSource en el nivel de presentación, agregué estos atributos para mejorar la experiencia en tiempo de diseño. Consulte el tutorial [creación de una capa de lógica de negocios](../introduction/creating-a-business-logic-layer-vb.md) para obtener una descripción más detallada de estos atributos y sus efectos.

En los métodos `GetProducts()` y `GetProductsByCategoryID(categoryID)`, los datos devueltos del método `GetCacheItem(key)` se asignan a una variable local. El método `GetCacheItem(key)`, que examinaremos en breve, devolverá un elemento determinado de la memoria caché en función de la *clave*especificada. Si no se encuentran estos datos en la memoria caché, se recuperan del método de clase `ProductsBLL` correspondiente y, a continuación, se agregan a la memoria caché mediante el método `AddCacheItem(key, value)`.

Los métodos `GetCacheItem(key)` y `AddCacheItem(key, value)` interfaz con la memoria caché de datos, los valores de lectura y escritura, respectivamente. El método `GetCacheItem(key)` es el más simple de los dos. Simplemente devuelve el valor de la clase cache mediante la *clave*pasada:

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample6.vb)]

`GetCacheItem(key)` no usa el valor de *clave* proporcionado, sino que llama al método `GetCacheKey(key)`, que devuelve la *clave* antepuesto con ProductsCache-. El método `AddCacheItem(key, value)` también utiliza el `MasterCacheKeyArray`, que contiene la cadena ProductsCache, como veremos en breve.

En una clase de código subyacente de la página ASP.NET, se puede tener acceso a la memoria caché de datos mediante la propiedad `Page` Class s [`Cache`](https://msdn.microsoft.com/library/system.web.ui.page.cache.aspx)y permite una sintaxis como `Cache("key") = value`, como se describe en el paso 2. Desde una clase dentro de la arquitectura, se puede tener acceso a la memoria caché de datos utilizando `HttpRuntime.Cache` o `HttpContext.Current.Cache`. En la entrada de blog de [Peter Johnson](https://weblogs.asp.net/pjohnson/default.aspx) [httpRuntime. cache frente a HttpContext. Current. cache](https://weblogs.asp.net/pjohnson/httpruntime-cache-vs-httpcontext-current-cache) se trata de una ligera ventaja de rendimiento al usar `HttpRuntime` en lugar de `HttpContext.Current`; por consiguiente, `ProductsCL` usa `HttpRuntime`.

> [!NOTE]
> Si la arquitectura se implementa mediante proyectos de biblioteca de clases, tendrá que agregar una referencia al ensamblado `System.Web` para poder usar las clases [`HttpRuntime`](https://msdn.microsoft.com/library/system.web.httpruntime.aspx) y [`HttpContext`](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) .

Si el elemento no se encuentra en la memoria caché, los métodos de la clase `ProductsCL`n obtienen los datos de la capa BLL y lo agregan a la memoria caché mediante el método `AddCacheItem(key, value)`. Para agregar *valor* a la memoria caché, podríamos usar el código siguiente, que usa una expiración de 60 segundos:

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample7.vb)]

`DateTime.Now.AddSeconds(CacheDuration)` especifica la expiración basada en tiempo 60 segundos en el futuro, mientras [`System.Web.Caching.Cache.NoSlidingExpiration`](https://msdn.microsoft.com/library/system.web.caching.cache.noslidingexpiration(vs.80).aspx) indica que no hay ninguna expiración variable. Aunque esta `Insert` sobrecarga del método tiene parámetros de entrada para una expiración absoluta y variable, solo puede proporcionar uno de los dos. Si intenta especificar tanto una hora absoluta como un intervalo de tiempo, el método `Insert` producirá una excepción `ArgumentException`.

> [!NOTE]
> Actualmente, esta implementación del método `AddCacheItem(key, value)` tiene algunas deficiencias. Solucionaremos estos problemas en el paso 4.

## <a name="step-4-invalidating-the-cache-when-the-data-is-modified-through-the-architecture"></a>Paso 4: invalidar la memoria caché cuando se modifican los datos a través de la arquitectura

Junto con los métodos de recuperación de datos, la capa de almacenamiento en caché debe proporcionar los mismos métodos que el BLL para insertar, actualizar y eliminar datos. Los métodos de modificación de datos de CL s no modifican los datos en caché, sino que llaman al método de modificación de datos correspondiente a BLL s y, a continuación, invalidan la caché. Como vimos en el tutorial anterior, este es el mismo comportamiento que ObjectDataSource aplica cuando se habilitan sus características de almacenamiento en caché y se invocan los métodos `Insert`, `Update`o `Delete`.

La siguiente sobrecarga de `UpdateProduct` muestra cómo implementar los métodos de modificación de datos en CL:

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample8.vb)]

Se invoca el método de capa de lógica de negocios de modificación de datos adecuado, pero antes de que se devuelva su respuesta, es necesario invalidar la memoria caché. Desafortunadamente, la invalidación de la memoria caché no es sencilla, ya que los métodos `ProductsCL` Class s `GetProducts()` y `GetProductsByCategoryID(categoryID)` cada uno agrega elementos a la memoria caché con claves diferentes y el método `GetProductsByCategoryID(categoryID)` agrega un elemento de caché diferente para cada *CategoryID*único.

Al invalidar la memoria caché, es necesario quitar *todos* los elementos que puede haber agregado la clase `ProductsCL`. Esto puede lograrse asociando una *dependencia de caché* con cada elemento agregado a la memoria caché en el método `AddCacheItem(key, value)`. En general, una dependencia de caché puede ser otro elemento de la memoria caché, un archivo del sistema de archivos o datos de una base de datos de Microsoft SQL Server. Cuando la dependencia cambia o se quita de la memoria caché, los elementos de la memoria caché a los que está asociado se desalojan automáticamente de la memoria caché. En este tutorial, queremos crear un elemento adicional en la memoria caché que actúe como una dependencia de caché para todos los elementos agregados a través de la clase `ProductsCL`. De este modo, todos estos elementos se pueden quitar de la memoria caché simplemente quitando la dependencia de caché.

Permita actualizar el método de `AddCacheItem(key, value)` para que cada elemento agregado a la memoria caché a través de este método esté asociado a una única dependencia de caché:

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample9.vb)]

`MasterCacheKeyArray` es una matriz de cadenas que contiene un solo valor, ProductsCache. En primer lugar, se agrega un elemento de caché a la memoria caché y se le asigna la fecha y hora actuales. Si el elemento de la memoria caché ya existe, se actualiza. A continuación, se crea una dependencia de caché. La [`CacheDependency`](https://msdn.microsoft.com/library/system.web.caching.cachedependency(VS.80).aspx) constructor de clase s tiene varias sobrecargas, pero la que se usa en este caso espera dos entradas de matriz `String`. El primero especifica el conjunto de archivos que se usarán como dependencias. Dado que no deseamos usar ninguna dependencia basada en archivos, se usa un valor de `Nothing` para el primer parámetro de entrada. El segundo parámetro de entrada especifica el conjunto de claves de caché que se va a usar como dependencias. Aquí se especifica nuestra dependencia única `MasterCacheKeyArray`. A continuación, el `CacheDependency` se pasa al método `Insert`.

Con esta modificación en `AddCacheItem(key, value)`, la invalidación de la memoria caché es tan sencilla como quitar la dependencia.

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample10.vb)]

## <a name="step-5-calling-the-caching-layer-from-the-presentation-layer"></a>Paso 5: llamar a la capa de almacenamiento en caché desde el nivel de presentación

Las clases y los métodos de la capa de almacenamiento en caché se pueden usar para trabajar con datos mediante las técnicas que se examinan en estos tutoriales. Para ilustrar el trabajo con datos almacenados en caché, guarde los cambios en la clase `ProductsCL` y, a continuación, abra la página `FromTheArchitecture.aspx` en la carpeta `Caching` y agregue un control GridView. En la etiqueta inteligente de GridView s, cree un nuevo ObjectDataSource. En el primer paso del asistente, debería ver la clase `ProductsCL` como una de las opciones de la lista desplegable.

[![la clase ProductsCL se incluye en la lista desplegable objeto comercial](caching-data-in-the-architecture-vb/_static/image5.png)](caching-data-in-the-architecture-vb/_static/image4.png)

**Figura 4**: la clase `ProductsCL` se incluye en la lista desplegable objeto comercial ([haga clic para ver la imagen de tamaño completo](caching-data-in-the-architecture-vb/_static/image6.png))

Después de seleccionar `ProductsCL`, haga clic en siguiente. La lista desplegable de la pestaña seleccionar tiene dos elementos: `GetProducts()` y `GetProductsByCategoryID(categoryID)` y la pestaña actualizar tiene la única sobrecarga `UpdateProduct`. Elija el método de `GetProducts()` en la pestaña seleccionar y el método `UpdateProducts` en la pestaña actualizar y haga clic en finalizar.

[![los métodos de la clase ProductsCL se muestran en las listas desplegables](caching-data-in-the-architecture-vb/_static/image8.png)](caching-data-in-the-architecture-vb/_static/image7.png)

**Figura 5**: los métodos de la clase `ProductsCL` se enumeran en las listas desplegables ([haga clic para ver la imagen a tamaño completo](caching-data-in-the-architecture-vb/_static/image9.png))

Después de completar el asistente, Visual Studio establecerá la propiedad ObjectDataSource s `OldValuesParameterFormatString` en `original_{0}` y agregará los campos correspondientes a GridView. Vuelva a cambiar la propiedad `OldValuesParameterFormatString` a su valor predeterminado, `{0}`y configure GridView para que admita la paginación, la ordenación y la edición. Dado que la sobrecarga de `UploadProducts` utilizada por CL solo acepta el nombre y el precio del producto editado, limite GridView para que solo se puedan editar estos campos.

En el tutorial anterior, hemos definido un control GridView para incluir campos para los campos `ProductName`, `CategoryName`y `UnitPrice`. No dude en replicar este formato y estructura, en cuyo caso el marcado declarativo de GridView y ObjectDataSource s debería tener un aspecto similar al siguiente:

[!code-aspx[Main](caching-data-in-the-architecture-vb/samples/sample11.aspx)]

En este momento, tenemos una página que utiliza la capa de almacenamiento en caché. Para ver la memoria caché en acción, establezca puntos de interrupción en los métodos `GetProducts()` y `UpdateProduct` de la clase `ProductsCL`. Visite la página en un explorador y recorra el código al ordenar y paginar para ver los datos extraídos de la memoria caché. A continuación, actualice un registro y tenga en cuenta que la memoria caché se invalida y, por consiguiente, se recupera de la capa BLL cuando los datos se reenlazan a GridView.

> [!NOTE]
> La capa de almacenamiento en caché proporcionada en la descarga que acompaña a este artículo no está completa. Solo contiene una clase, `ProductsCL`, que solo Sports unos cuantos métodos. Además, en una sola página de ASP.NET se usa el CL (`~/Caching/FromTheArchitecture.aspx`) que todos los demás todavía hacen referencia directamente a la capa BLL. Si planea usar un CL en la aplicación, todas las llamadas desde el nivel de presentación deben ir a CL, lo que requeriría que las clases y los métodos de CL se encontraran en las clases y los métodos de la capa BLL utilizada actualmente por el nivel de presentación.

## <a name="summary"></a>Resumen

Aunque el almacenamiento en caché se puede aplicar en el nivel de presentación con los controles ASP.NET 2,0 s SqlDataSource y ObjectDataSource, idealmente, las responsabilidades del almacenamiento en caché se delegarían a una capa independiente en la arquitectura. En este tutorial, creamos una capa de almacenamiento en caché que reside entre el nivel de presentación y la capa de lógica de negocios. La capa de almacenamiento en caché debe proporcionar el mismo conjunto de clases y métodos que existen en la capa BLL y a los que se llama desde el nivel de presentación.

Los ejemplos de nivel de almacenamiento en caché que se exploran en este y en los tutoriales anteriores exhibeban la *carga reactiva*. Con la carga reactiva, los datos se cargan en la memoria caché solo cuando se realiza una solicitud de datos y faltan datos en la memoria caché. Los datos también se pueden *cargar proactivamente* en la memoria caché, una técnica que carga los datos en la memoria caché antes de que se necesiten realmente. En el siguiente tutorial, veremos un ejemplo de carga proactiva cuando veremos cómo almacenar valores estáticos en la memoria caché al iniciarse la aplicación.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor responsable de este tutorial era Teresa Murphy. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](caching-data-with-the-objectdatasource-vb.md)
> [Siguiente](caching-data-at-application-startup-vb.md)
