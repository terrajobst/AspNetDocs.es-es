---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
title: Convenciones de enrutamiento en ASP.NET Web API 2 OData-ASP.NET 4. x
author: MikeWasson
description: Describe las convenciones de enrutamiento que Web API 2 de ASP.NET 4. x usa para los puntos de conexión de OData.
ms.author: riande
ms.date: 07/31/2013
ms.custom: seoapril2019
ms.assetid: adbc175a-14eb-4ab2-a441-d056ffa8266f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
msc.type: authoredcontent
ms.openlocfilehash: 63df4a82cd8df92631485b2544117844cfd0ca56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78498199"
---
# <a name="routing-conventions-in-aspnet-web-api-2-odata"></a>Convenciones de enrutamiento en ASP.NET Web API 2 OData

por [Mike Wasson](https://github.com/MikeWasson)

> En este artículo se describen las convenciones de enrutamiento que Web API 2 de ASP.NET 4. x usa para los puntos de conexión de OData.

Cuando Web API obtiene una solicitud de OData, asigna la solicitud a un nombre de controlador y un nombre de acción. La asignación se basa en el método HTTP y el URI. Por ejemplo, `GET /odata/Products(1)` se asigna a `ProductsController.GetProduct`.

En la primera parte de este artículo, se describen las convenciones de enrutamiento de OData integradas. Estas convenciones están diseñadas específicamente para los puntos de conexión de OData y reemplazan el sistema de enrutamiento de API Web predeterminado. (El reemplazo se produce cuando se llama a **MapODataRoute**).

En la parte 2, se muestra cómo agregar convenciones de enrutamiento personalizadas. Actualmente, las convenciones integradas no cubren todo el intervalo de identificadores URI de OData, pero puede ampliarlos para administrar casos adicionales.

- [Convenciones de enrutamiento integradas](#conventions)
- [Convenciones de enrutamiento personalizadas](#custom)

<a id="conventions"></a>
## <a name="built-in-routing-conventions"></a>Convenciones de enrutamiento integradas

Antes de describir las convenciones de enrutamiento de OData en Web API, resulta útil comprender los URI de OData. Un [URI de oData](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/) consta de:

- La raíz del servicio
- La ruta de acceso del recurso
- Opciones de consulta

![](odata-routing-conventions/_static/image1.png)

Para el enrutamiento, la parte importante es la ruta de acceso al recurso. La ruta de acceso al recurso se divide en segmentos. Por ejemplo, `/Products(1)/Supplier` tiene tres segmentos:

- `Products` hace referencia a un conjunto de entidades denominado "Products".
- `1` es una clave de entidad, seleccionando una sola entidad del conjunto.
- `Supplier` es una propiedad de navegación que selecciona una entidad relacionada.

Por lo tanto, este trazado toma el proveedor del producto 1.

> [!NOTE]
> Los segmentos de ruta de acceso de OData no siempre se corresponden con segmentos de URI. Por ejemplo, "1" se considera un segmento de ruta de acceso.

**Nombres de controlador.** El nombre del controlador siempre se deriva del conjunto de entidades en la raíz de la ruta de acceso del recurso. Por ejemplo, si la ruta de acceso a recursos es `/Products(1)/Supplier`, Web API busca un controlador denominado `ProductsController`.

**Nombres de acción.** Los nombres de acción se derivan de los segmentos de ruta de acceso más el Entity Data Model (EDM), como se muestra en las tablas siguientes. En algunos casos, tiene dos opciones para el nombre de la acción. Por ejemplo, "Get" o &quot;GetProducts&quot;.

**Consultar entidades**

| Request | URI de ejemplo | Nombre de la acción | Acción de ejemplo |
| --- | --- | --- | --- |
| OBTENER/EntitySet | /Products | GetEntitySet o get | GetProducts |
| OBTENER/EntitySet (clave) | /Products (1) | GetEntityType o get | GetProduct |
| GET/EntitySet (Key)/CAST | /Products (1)/Models.Book | GetEntityType o get | GetBook |

Para obtener más información, vea [crear un punto de conexión de oData de solo lectura](odata-v3/creating-an-odata-endpoint.md).

**Crear, actualizar y eliminar entidades**

| Request | URI de ejemplo | Nombre de la acción | Acción de ejemplo |
| --- | --- | --- | --- |
| POST/EntitySet | /Products | PostEntityType o post | Postproducto |
| PUT/EntitySet (clave) | /Products (1) | PutEntityType o Put | PutProduct |
| PUT/EntitySet (clave)/CAST | /Products (1)/Models.Book | PutEntityType o Put | PutBook |
| REVISIÓN/EntitySet (clave) | /Products (1) | PatchEntityType o patch | PatchProduct |
| PATCH/EntitySet (Key)/CAST | /Products (1)/Models.Book | PatchEntityType o patch | PatchBook |
| ELIMINAR/EntitySet (clave) | /Products (1) | DeleteEntityType o DELETE | DeleteProduct |
| ELIMINe/EntitySet (Key)/CAST | /Products (1)/Models.Book | DeleteEntityType o DELETE | DeleteBook |

**Consultar una propiedad de navegación**

| Request | URI de ejemplo | Nombre de la acción | Acción de ejemplo |
| --- | --- | --- | --- |
| GET/EntitySet (Key)/Navigation | /Products (1)/supplier | GetNavigationFromEntityType o GetNavigation | GetSupplierFromProduct |
| GET/EntitySet (Key)/CAST/Navigation | /Products (1)/Models.Book/Author | GetNavigationFromEntityType o GetNavigation | GetAuthorFromBook |

Para obtener más información, vea [trabajar con relaciones de entidad](odata-v3/working-with-entity-relations.md).

**Crear y eliminar vínculos**

| Request | URI de ejemplo | Nombre de la acción |
| --- | --- | --- |
| POST /entityset(key)/$links/navigation | /Products (1)/$links/supplier | CreateLink |
| PUT/EntitySet (Key)/$links/Navigation | /Products (1)/$links/supplier | CreateLink |
| DELETE/EntitySet (Key)/$links/Navigation | /Products (1)/$links/supplier | DeleteLink |
| DELETE /entityset(key)/$links/navigation(relatedKey) | /Products/(1)/$links/Suppliers (1) | DeleteLink |

Para obtener más información, vea [trabajar con relaciones de entidad](odata-v3/working-with-entity-relations.md).

**Propiedades**

*Requiere Web API 2*

| Request | URI de ejemplo | Nombre de la acción | Acción de ejemplo |
| --- | --- | --- | --- |
| GET/EntitySet (Key)/Property | /Products (1)/Name | GetPropertyFromEntityType o GetProperty | GetNameFromProduct |
| GET/EntitySet (Key)/CAST/Property | /Products (1)/Models.Book/Author | GetPropertyFromEntityType o GetProperty | GetTitleFromBook |

**Acciones**

| Request | URI de ejemplo | Nombre de la acción | Acción de ejemplo |
| --- | --- | --- | --- |
| POST/EntitySet (Key)/Action | /Products (1)/Rate | ActionNameOnEntityType o ActionName | RateOnProduct |
| POST/EntitySet (Key)/CAST/Action | /Products (1)/Models.Book/CheckOut | ActionNameOnEntityType o ActionName | CheckOutOnBook |

Para obtener más información, vea [acciones de oData](odata-v3/odata-actions.md).

**Firmas de método**

Estas son algunas reglas para las signaturas de método:

- Si la ruta de acceso contiene una clave, la acción debe tener un parámetro denominado *key*.
- Si la ruta de acceso contiene una clave en una propiedad de navegación, la acción debe tener un parámetro denominado *relatedKey*.
- Decorar los parámetros *key* y *relatedKey* con el parámetro **[FromODataUri]** .
- Las solicitudes POST y PUT toman un parámetro del tipo de entidad.
- Las solicitudes PATCH toman un parámetro de tipo **Delta&lt;t&gt;** , donde *t* es el tipo de entidad.

Como referencia, este es un ejemplo en el que se muestran firmas de método para cada Convención de enrutamiento de OData integrada.

[!code-csharp[Main](odata-routing-conventions/samples/sample1.cs)]

<a id="custom"></a>
## <a name="custom-routing-conventions"></a>Convenciones de enrutamiento personalizadas

Actualmente, las convenciones integradas no cubren todos los posibles URI de OData. Puede agregar nuevas convenciones implementando la interfaz **IODataRoutingConvention** . Esta interfaz tiene dos métodos:

[!code-csharp[Main](odata-routing-conventions/samples/sample2.cs)]

- **SelectController** devuelve el nombre del controlador.
- **SelectAction** devuelve el nombre de la acción.

En ambos métodos, si la Convención no se aplica a esa solicitud, el método debe devolver null.

El parámetro **ODataPath** representa la ruta de acceso del recurso OData analizada. Contiene una lista de instancias de **[ODataPathSegment](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatapathsegment.aspx)** , una para cada segmento de la ruta de acceso del recurso. **ODataPathSegment** es una clase abstracta; cada tipo de segmento se representa mediante una clase que se deriva de **ODataPathSegment**.

La propiedad **ODataPath. TemplatePath** es una cadena que representa la concatenación de todos los segmentos de la ruta de acceso. Por ejemplo, si el URI es `/Products(1)/Supplier`, la plantilla de ruta de acceso es &quot;~/EntitySet/Key/Navigation&quot;. Tenga en cuenta que los segmentos no se corresponden directamente con los segmentos de URI. Por ejemplo, la clave de entidad (1) se representa como su propio **ODataPathSegment**.

Normalmente, una implementación de **IODataRoutingConvention** hace lo siguiente:

1. Compare la plantilla de ruta de acceso para ver si esta Convención se aplica a la solicitud actual. Si no se aplica, devuelve NULL.
2. Si se aplica la Convención, use las propiedades de las instancias de **ODataPathSegment** para derivar los nombres de los controladores y las acciones.
3. En el caso de las acciones, agregue cualquier valor al Diccionario de rutas que deba enlazarse a los parámetros de acción (normalmente, las claves de entidad).

Echemos un vistazo a un ejemplo específico. Las convenciones de enrutamiento integradas no admiten la indización en una colección de navegación. En otras palabras, no hay ninguna Convención para URI como la siguiente:

[!code-javascript[Main](odata-routing-conventions/samples/sample3.js)]

Esta es una Convención de enrutamiento personalizada para controlar este tipo de consulta.

[!code-csharp[Main](odata-routing-conventions/samples/sample4.cs)]

Notas:

1. Se deriva de **EntitySetRoutingConvention**, porque el método **SelectController** de esa clase es adecuado para esta nueva Convención de enrutamiento. Esto significa que no es necesario volver a implementar **SelectController**.
2. La Convención solo se aplica a las solicitudes GET y solo cuando la plantilla de ruta de acceso está &quot;~/EntitySet/Key/Navigation/Key&quot;.
3. El nombre de la acción es &quot;Get {EntityType}&quot;, donde *{EntityType}* es el tipo de la colección de navegación. Por ejemplo, &quot;GetSupplier&quot;. Puede usar cualquier Convención de nomenclatura que le guste &#8212; , solo tiene que asegurarse de que las acciones del controlador coinciden.
4. La acción toma dos parámetros denominados *key* y *relatedKey*. (Para obtener una lista de algunos nombres de parámetros predefinidos, vea [ODataRouteConstants](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatarouteconstants.aspx)).

El siguiente paso es agregar la nueva Convención a la lista de convenciones de enrutamiento. Esto sucede durante la configuración, como se muestra en el código siguiente:

[!code-csharp[Main](odata-routing-conventions/samples/sample5.cs)]

A continuación se muestran otras convenciones de enrutamiento de ejemplo que son útiles para el estudio:

- [CompositeKeyRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataCompositeKeySample/ODataCompositeKeySample/Extensions/CompositeKeyRoutingConvention.cs)
- [CustomNavigationRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataServiceSample/ODataService/Extensions/CustomNavigationRoutingConvention.cs)
- [NonBindableActionRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataActionsSample/ODataActionsSample/NonBindableActionRoutingConvention.cs)
- [ODataVersionRouteConstraint](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataVersioningSample/ODataVersioningSample/Extensions/ODataVersionRouteConstraint.cs)

Y, por supuesto, la propia API Web es de código abierto, por lo que puede ver el [código fuente](http://aspnetwebstack.codeplex.com/) de las convenciones de enrutamiento integradas. Se definen en el espacio de nombres **System. Web. http. OData. Routing. Conventions** .
