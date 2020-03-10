---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: Implementar la paginación eficaz de los datos | Microsoft Docs
author: microsoft
description: En el paso 8 se muestra cómo agregar compatibilidad con la paginación a nuestra dirección URL de/Dinners para que, en lugar de mostrar miles de cenas a la vez, solo se muestren las 10 cenas próximas en...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: 2d9a0dba381b71755ac626f76d52bc5bcb434447
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486487"
---
# <a name="implement-efficient-data-paging"></a>Implementar la paginación de datos eficaz

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 8 de un [tutorial de aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito en el que se explica cómo crear una aplicación web pequeña, pero completa, con ASP.NET MVC 1.
> 
> En el paso 8 se muestra cómo agregar compatibilidad de paginación a nuestra dirección URL de/Dinners para que, en lugar de mostrar miles de cenas a la vez, solo se muestren 10 próximas cenas a la vez, y que los usuarios finales puedan avanzar y retroceder por toda la lista de una manera fácil de SEO.
> 
> Si usa ASP.NET MVC 3, se recomienda seguir los tutoriales [de la introducción con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o MVC [Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-8-paging-support"></a>NerdDinner paso 8: compatibilidad con la paginación

Si el sitio se realiza correctamente, tendrá miles de cenas. Tenemos que asegurarnos de que la interfaz de usuario se escala para controlar todas estas cenas y permite que los usuarios las examinen. Para habilitar esta opción, agregaremos compatibilidad con la paginación a nuestra dirección URL de */Dinners* para que en lugar de mostrar miles de cenas a la vez, solo mostraremos las 10 cenas a la vez y permitiremos a los usuarios finales avanzar y retroceder por toda la lista de una manera fácil de SEO.

### <a name="index-action-method-recap"></a>Resumen del método de acción index ()

El método de acción index () dentro de nuestra clase DinnersController tiene actualmente el siguiente aspecto:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

Cuando se realiza una solicitud a la dirección URL de */Dinners* , se recupera una lista de todas las cenas futuras y, a continuación, se presenta una lista de todas ellas:

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iqueryablelttgt"></a>Descripción de IQueryable&lt;T&gt;

*IQueryable&lt;t&gt;* es una interfaz que se presentó con LINQ como parte de .net 3,5. Permite escenarios eficaces de "ejecución diferida" que podemos aprovechar para implementar la compatibilidad con la paginación.

En nuestro DinnerRepository, se devuelve una secuencia de&gt; de IQueryable&lt;cena desde nuestro método FindUpcomingDinners ():

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

El objeto IQueryable&lt;cena&gt; devuelto por nuestro método FindUpcomingDinners () encapsula una consulta para recuperar objetos cena de nuestra base de datos mediante LINQ to SQL. Lo importante es que no ejecute la consulta en la base de datos hasta que se intente acceder a los datos de la consulta o iterarlos en ellos, o hasta que se llame al método ToList () en él. El código que llama al método FindUpcomingDinners () puede optar entre agregar operaciones o filtros adicionales "encadenados" al objeto IQueryable&lt;cena&gt; antes de ejecutar la consulta. LINQ to SQL es lo suficientemente inteligente como para ejecutar la consulta combinada en la base de datos cuando se solicitan los datos.

Para implementar la lógica de paginación, podemos actualizar el método de acción de índice () de DinnersController para que aplique operadores "SKIP" y "Take" adicionales a la secuencia IQueryable&lt;cena&gt; antes de llamar a ToList () en ella:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

El código anterior omite las 10 primeras cenas en la base de datos y, a continuación, devuelve 20 cenas. LINQ to SQL es lo suficientemente inteligente como para construir una consulta SQL optimizada que lleve a cabo esta lógica de omisión en la base de datos SQL, y no en el servidor Web. Esto significa que, aunque tengamos millones de cenas en la base de datos, solo se recuperarán los 10 que queremos como parte de esta solicitud (lo que hace que sea eficaz y escalable).

### <a name="adding-a-page-value-to-the-url"></a>Agregar un valor de "página" a la dirección URL

En lugar de codificar de forma rígida un intervalo de páginas específico, queremos que nuestras direcciones URL incluyan un parámetro "página" que indique el intervalo de cena que solicita un usuario.

#### <a name="using-a-querystring-value"></a>Usar un valor QueryString

En el código siguiente se muestra cómo se puede actualizar el método de acción index () para admitir un parámetro QueryString y habilitar direcciones URL como */Dinners? Page = 2*:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

El método de acción index () anterior tiene un parámetro denominado "Page". El parámetro se declara como un entero que acepta valores NULL (es decir, int? indica). Esto significa que la dirección URL de */Dinners? Page = 2* hará que se pase un valor de "2" como valor de parámetro. La dirección URL de */Dinners* (sin un valor QueryString) hará que se pase un valor null.

Estamos multiplicando el valor de la página por el tamaño de la página (en este caso, 10 filas) para determinar cuántas cenas debe omitir. Estamos usando el [ C# operador "Coalesce" (??) null,](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) que es útil cuando se trabaja con tipos que aceptan valores NULL. El código anterior asigna el valor 0 a la página si el parámetro de página es NULL.

#### <a name="using-embedded-url-values"></a>Usar valores de URL insertados

Una alternativa al uso de un valor QueryString sería incrustar el parámetro Page dentro de la propia dirección URL real. Por ejemplo: */Dinners/Page/2* o */Dinners/2*. ASP.NET MVC incluye un potente motor de enrutamiento de direcciones URL que facilita la compatibilidad con escenarios como este.

Podemos registrar reglas de enrutamiento personalizadas que asignen cualquier dirección URL o formato de dirección URL entrante a cualquier método de acción o clase de controlador que desee. Lo único que debemos hacer es abrir el archivo global. asax dentro de nuestro proyecto:

![](implement-efficient-data-paging/_static/image2.png)

Y, a continuación, registre una nueva regla de asignación con el método auxiliar MapRoute () como la primera llamada a las rutas. MapRoute () a continuación:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

Por encima, registramos una nueva regla de enrutamiento denominada "UpcomingDinners". Estamos indicando que tiene el formato de dirección URL "cenas/Page/{Page}", donde {Page} es un valor de parámetro incrustado en la dirección URL. El tercer parámetro al método MapRoute () indica que se deben asignar las direcciones URL que coinciden con este formato al método de acción index () en la clase DinnersController.

Podemos usar el mismo código de índice () exacto que teníamos antes con el escenario de cadena de cadenas, excepto que ahora nuestro parámetro de "página" proviene de la dirección URL y no de la cadena de QueryString:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

Y ahora, cuando ejecutemos la aplicación y escribamos en */Dinners* , veremos las 10 primeras cenas:

![](implement-efficient-data-paging/_static/image3.png)

Y cuando escribamos en */Dinners/Page/1* , veremos la siguiente página de cenas:

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>Agregar interfaz de usuario de navegación de páginas

El último paso para completar el escenario de paginación es implementar la interfaz de usuario de navegación "siguiente" y "anterior" en nuestra plantilla de vista para que los usuarios puedan omitir fácilmente los datos de la cena.

Para implementarlo correctamente, es necesario conocer el número total de cenas en la base de datos, así como cuántas páginas de datos se traducen en. A continuación, tendremos que calcular si el valor de "página" solicitado actualmente está al principio o al final de los datos, y mostrar u ocultar la interfaz de usuario "anterior" y "siguiente" en consecuencia. Podríamos implementar esta lógica en nuestro método de acción index (). Como alternativa, podemos agregar una clase auxiliar a nuestro proyecto que encapsula esta lógica de forma más reutilizable.

A continuación se muestra una sencilla clase auxiliar "PaginatedList" que deriva de la lista&lt;T&gt; clase de colección integrada en el .NET Framework. Implementa una clase de colección reutilizable que se puede utilizar para paginar cualquier secuencia de datos IQueryable. En nuestra aplicación de NerdDinner, trabajaremos en IQueryable&lt;cena&gt; resultados, pero se podría usar fácilmente con IQueryable&lt;Product&gt; o IQueryable&lt;Customer&gt; resultados en otros escenarios de aplicación:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

Observe lo anterior cómo calcula y expone propiedades como "PageIndex", "PageSize", "TotalCount" y "TotalPages". También expone dos propiedades auxiliares "HasPreviousPage" y "HasNextPage" que indican si la página de datos de la colección está al principio o al final de la secuencia original. El código anterior hará que se ejecuten dos consultas SQL: la primera para recuperar el recuento del número total de objetos de cenas (esto no devuelve los objetos, sino que realiza una instrucción "SELECT COUNT" que devuelve un entero) y el segundo para recuperar solo las filas de datos que necesitamos de nuestra base de datos para la página de datos actual.

A continuación, podemos actualizar el método de la aplicación auxiliar DinnersController. index () para crear un PaginatedList&lt;cena&gt; desde el resultado de DinnerRepository. FindUpcomingDinners () y pasarlo a nuestra plantilla de vista:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

A continuación, podemos actualizar la plantilla de vista \Views\Dinners\Index.aspx para que herede de ViewPage&lt;NerdDinner. helpers. PaginatedList&lt;cena&gt;&gt; en lugar de ViewPage&lt;IEnumerable&lt;cena&gt;&gt;y, a continuación, agregue el código siguiente al final de la plantilla de vista para mostrar u ocultar la interfaz de usuario de navegación siguiente y anterior:

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

Observe cómo se usa el método auxiliar HTML. RouteLink () para generar los hipervínculos. Este método es similar al método auxiliar HTML. ActionLink () que hemos usado previamente. La diferencia es que la dirección URL se genera mediante la regla de enrutamiento "UpcomingDinners" que se configura en el archivo global. asax. Esto garantiza que se generen direcciones URL en el método de acción index () que tenga el formato: */Dinners/Page/{Page}* , donde el valor de {Page} es una variable que se proporciona anteriormente basándose en el PageIndex actual.

Y ahora, cuando ejecutemos de nuevo la aplicación, veremos 10 cenas a la vez en nuestro explorador:

![](implement-efficient-data-paging/_static/image5.png)

También tenemos &lt;&lt;&lt; y &gt;&gt;la interfaz de usuario de navegación en la parte inferior de la página, lo que nos permite omitir la redirección hacia delante y hacia atrás por los datos mediante las direcciones URL accesibles del motor de búsqueda:&gt;

![](implement-efficient-data-paging/_static/image6.png)

| **Tema del lado: Descripción de las implicaciones de IQueryable&lt;T&gt;** |
| --- |
| IQueryable&lt;T&gt; es una característica muy eficaz que permite una variedad de escenarios de ejecución aplazados interesantes (como las consultas basadas en la paginación y la composición). Al igual que con todas las características eficaces, se recomienda tener cuidado con la forma de usarla y asegurarse de que no se abusa de él. Es importante reconocer que devolver un IQueryable&lt;T&gt; resultado del repositorio permite que el código de llamada se anexe a él en métodos de operador encadenados, por lo que debe participar en la última ejecución de la consulta. Si no desea proporcionar código de llamada a esta capacidad, debe devolver IList&lt;T&gt; o IEnumerable&lt;T&gt; resultados, que contienen los resultados de una consulta que ya se ha ejecutado. En los escenarios de paginación, esto requeriría que inserte la lógica de paginación de datos real en el método de repositorio al que se llama. En este escenario, podríamos actualizar nuestro método Finder de FindUpcomingDinners () para que tenga una firma que devolviera un PaginatedList: PaginatedList&lt; cena&gt; FindUpcomingDinners (int pageIndex, int PageSize) {} o Return a IList&lt;cena&gt;y use un parámetro out "totalCount" para devolver el recuento total de cenas: IList&lt;cena&gt; FindUpcomingDinners (int pageIndex, int PageSize, out int totalCount) {} |

### <a name="next-step"></a>Paso siguiente

Ahora veamos cómo podemos agregar compatibilidad con la autenticación y la autorización a nuestra aplicación.

> [!div class="step-by-step"]
> [Anterior](re-use-ui-using-master-pages-and-partials.md)
> [Siguiente](secure-applications-using-authentication-and-authorization.md)
