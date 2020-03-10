---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Enrutamiento de atributos en ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 7da1805d8a7066e82743dc9bd7e024cc9813ee89
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447001"
---
# <a name="attribute-routing-in-aspnet-web-api-2"></a>Enrutamiento de atributos en ASP.NET Web API 2

por [Mike Wasson](https://github.com/MikeWasson)

El *enrutamiento* es el modo en que la API Web coincide con un URI de una acción. Web API 2 admite un nuevo tipo de enrutamiento, denominado *enrutamiento de atributos*. Como implica el nombre, el enrutamiento de atributos utiliza atributos para definir rutas. El enrutamiento de atributos le proporciona más control sobre los URI de la API Web. Por ejemplo, puede crear fácilmente URI que describen jerarquías de recursos.

El estilo anterior de enrutamiento, denominado enrutamiento basado en convenciones, sigue siendo totalmente compatible. De hecho, puede combinar ambas técnicas en el mismo proyecto.

En este tema se muestra cómo habilitar el enrutamiento de atributos y se describen las distintas opciones para el enrutamiento de atributos. Para ver un tutorial de un extremo a otro que usa el enrutamiento de atributos, consulte [creación de una API de REST con enrutamiento de atributos en Web API 2](create-a-rest-api-with-attribute-routing.md).

## <a name="prerequisites"></a>Requisitos previos

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional o Enterprise Edition

También puede usar el administrador de paquetes NuGet para instalar los paquetes necesarios. En el menú **herramientas** de Visual Studio, seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**. Escriba el siguiente comando en la ventana de la consola del administrador de paquetes:

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a>¿Por qué el enrutamiento de atributos?

La primera versión de la API Web usó el enrutamiento *basado en convenciones* . En ese tipo de enrutamiento, se definen una o varias plantillas de ruta, que básicamente son cadenas con parámetros. Cuando el marco de trabajo recibe una solicitud, coincide con el URI en la plantilla de ruta. (Para obtener más información sobre el enrutamiento basado en convenciones, consulte [enrutamiento en ASP.net web API](routing-in-aspnet-web-api.md).

Una ventaja del enrutamiento basado en convenciones es que las plantillas se definen en un único lugar y las reglas de enrutamiento se aplican de forma coherente en todos los controladores. Desafortunadamente, el enrutamiento basado en convenciones dificulta la compatibilidad con determinados patrones de URI que son comunes en las API de RESTful. Por ejemplo, los recursos suelen contener recursos secundarios: los clientes tienen pedidos, las películas tienen actores, los libros tienen autores, etc. Es natural crear identificadores URI que reflejen estas relaciones:

`/customers/1/orders`

Este tipo de URI es difícil de crear mediante el enrutamiento basado en convenciones. Aunque se puede hacer, los resultados no se escalan bien si tiene muchos controladores o tipos de recursos.

Con el enrutamiento de atributos, es trivial definir una ruta para este URI. Simplemente agregue un atributo a la acción del controlador:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

Estos son otros patrones que facilita el enrutamiento de atributos.

**Control de versiones de API**

En este ejemplo, "/API/v1/Products" se enrutaría a un controlador diferente que "/API/V2/Products".

`/api/v1/products`
`/api/v2/products`

**Segmentos de URI sobrecargados**

En este ejemplo, "1" es un número de pedido, pero "pendiente" se asigna a una colección.

`/orders/1`
`/orders/pending`

**Varios tipos de parámetro**

En este ejemplo, "1" es un número de pedido, pero "2013/06/16" especifica una fecha.

`/orders/1`
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a>Habilitar el enrutamiento de atributos

Para habilitar el enrutamiento de atributos, llame a **MapHttpAttributeRoutes** durante la configuración. Este método de extensión se define en la clase **System. Web. http. HttpConfigurationExtensions** .

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

El enrutamiento de atributos se puede combinar con [el enrutamiento basado en convenciones](routing-in-aspnet-web-api.md) . Para definir las rutas basadas en convenciones, llame al método **MapHttpRoute** .

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

Para obtener más información sobre la configuración de la API Web, consulte [configuración de ASP.net web API 2](../advanced/configuring-aspnet-web-api.md).

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a>Nota: migración desde web API 1

Antes de la API Web 2, las plantillas de proyecto de la API Web generaban código similar al siguiente:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

Si el enrutamiento de atributos está habilitado, este código producirá una excepción. Si actualiza un proyecto de API Web existente para usar el enrutamiento de atributos, asegúrese de actualizar este código de configuración a lo siguiente:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> Para obtener más información, consulte Configuración de la [API Web con hospedaje de ASP.net](../advanced/configuring-aspnet-web-api.md#webhost).

<a id="add-routes"></a>
## <a name="adding-route-attributes"></a>Agregar atributos de ruta

Este es un ejemplo de una ruta definida mediante un atributo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

La cadena &quot;customers/{customerId}/Orders&quot; es la plantilla URI de la ruta. Web API intenta hacer coincidir el URI de solicitud con la plantilla. En este ejemplo, "Customers" y "Orders" son segmentos literales y "{customerId}" es un parámetro de variable. Los siguientes URI coincidirían con esta plantilla:

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

Puede restringir la coincidencia mediante [restricciones](#constraints), descritas más adelante en este tema.

Observe que el parámetro &quot;{customerId}&quot; de la plantilla de ruta coincide con el nombre del parámetro *customerId* en el método. Cuando la API Web invoca la acción del controlador, intenta enlazar los parámetros de ruta. Por ejemplo, si el URI es `http://example.com/customers/1/orders`, Web API intenta enlazar el valor "1" al parámetro *customerId* en la acción.

Una plantilla de URI puede tener varios parámetros:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

Los métodos de controlador que no tienen un atributo de ruta usan el enrutamiento basado en convenciones. De este modo, puede combinar ambos tipos de enrutamiento en el mismo proyecto.

## <a name="http-methods"></a>HTTP Methods

Web API también selecciona acciones basadas en el método HTTP de la solicitud (GET, POST, etc.). De forma predeterminada, Web API busca una coincidencia sin distinción entre mayúsculas y minúsculas con el inicio del nombre del método de controlador. Por ejemplo, un método de controlador denominado `PutCustomers` coincide con una solicitud PUT de HTTP.

Puede invalidar esta Convención decorando el método con cualquiera de los siguientes atributos:

- **HttpDelete**
- **[HttpGet]**
- **[HttpHead]**
- **[HttpOptions]**
- **[HttpPatch]**
- **HttpPost**
- **HttpPut**

En el ejemplo siguiente se asigna el método CreateBook a las solicitudes HTTP POST.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

Para todos los demás métodos HTTP, incluidos los métodos no estándar, use el atributo **AcceptVerbs** , que toma una lista de métodos http.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a>Prefijos de ruta

A menudo, todas las rutas de un controlador se inician con el mismo prefijo. Por ejemplo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

Puede establecer un prefijo común para todo un controlador mediante el atributo **[RoutePrefix]** :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

Use una tilde (~) en el atributo del método para invalidar el prefijo de ruta:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

El prefijo de ruta puede incluir parámetros:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a>Restricciones de ruta

Las restricciones de ruta permiten restringir el modo en que se buscan coincidencias con los parámetros de la plantilla de ruta. La sintaxis general es &quot;{parámetro: Constraint}&quot;. Por ejemplo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

Aquí, la primera ruta solo se seleccionará si el identificador de &quot;&quot; segmento del URI es un entero. De lo contrario, se elegirá la segunda ruta.

En la tabla siguiente se enumeran las restricciones que se admiten.

| Restricción | Description | Ejemplo |
| --- | --- | --- |
| alpha | Coincide con caracteres del alfabeto latino en mayúsculas o minúsculas (a-z, A-Z) | {x:alpha} |
| bool | Coincide con un valor booleano. | {x:bool} |
| datetime | Coincide con un valor **DateTime** . | {x:datetime} |
| decimal | Coincide con un valor decimal. | {x:decimal} |
| double | Coincide con un valor de punto flotante de 64 bits. | {x:double} |
| float | Coincide con un valor de punto flotante de 32 bits. | {x:float} |
| guid | Coincide con un valor de GUID. | {x:guid} |
| int | Coincide con un valor entero de 32 bits. | {x:int} |
| longitud | Coincide con una cadena con la longitud especificada o dentro de un intervalo de longitud especificado. | {x:length (6)} {x:length (1, 20)} |
| long | Coincide con un valor entero de 64 bits. | {x:long} |
| max | Coincide con un entero con un valor máximo. | {x:max(10)} |
| longitud | Coincide con una cadena con una longitud máxima. | {x:maxlength(10)} |
| min | Coincide con un entero con un valor mínimo. | {x:min(10)} |
| MinLength | Coincide con una cadena con una longitud mínima. | {x:MinLength (10)} |
| range | Coincide con un entero dentro de un intervalo de valores. | {x:Range (10, 50)} |
| regex | Coincide con una expresión regular. | {x:regex (^ \d{3}-\d{3}-\d{4}$)} |

Observe que algunas de las restricciones, como &quot;min&quot;, toman argumentos entre paréntesis. Puede aplicar varias restricciones a un parámetro, separadas por dos puntos.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a>Restricciones de ruta personalizadas

Puede crear restricciones de ruta personalizadas implementando la interfaz **IHttpRouteConstraint** . Por ejemplo, la restricción siguiente restringe un parámetro a un valor entero distinto de cero.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

En el código siguiente se muestra cómo registrar la restricción:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

Ahora puede aplicar la restricción en las rutas:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

También puede reemplazar toda la clase **DefaultInlineConstraintResolver** implementando la interfaz **IInlineConstraintResolver** . Al hacerlo, se reemplazarán todas las restricciones integradas, a menos que la implementación de **IInlineConstraintResolver** las agregue específicamente.

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a>Parámetros de URI opcionales y valores predeterminados

Puede crear un parámetro de URI opcional si agrega un signo de interrogación al parámetro Route. Si un parámetro de ruta es opcional, debe definir un valor predeterminado para el parámetro de método.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

En este ejemplo, `/api/books/locale/1033` y `/api/books/locale` devuelven el mismo recurso.

También puede especificar un valor predeterminado dentro de la plantilla de ruta, como se indica a continuación:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

Esto es prácticamente igual que el ejemplo anterior, pero hay una pequeña diferencia de comportamiento cuando se aplica el valor predeterminado.

- En el primer ejemplo ("{LCID: int?}"), el valor predeterminado de 1033 se asigna directamente al parámetro del método, por lo que el parámetro tendrá este valor exacto.
- En el segundo ejemplo ("{LCID: int = 1033}"), el valor predeterminado de "1033" pasa por el proceso de enlace de modelos. El enlazador de modelos predeterminado convertirá "1033" en el valor numérico 1033. Sin embargo, podría conectar un enlazador de modelos personalizado, que podría hacer algo diferente.

(En la mayoría de los casos, a menos que tenga enlazadores de modelos personalizados en la canalización, las dos formas serán equivalentes).

<a id="route-names"></a>
## <a name="route-names"></a>Nombres de ruta

En Web API, cada ruta tiene un nombre. Los nombres de ruta son útiles para generar vínculos, por lo que puede incluir un vínculo en una respuesta HTTP.

Para especificar el nombre de la ruta, establezca la propiedad **nombre** en el atributo. En el ejemplo siguiente se muestra cómo establecer el nombre de la ruta y cómo usar el nombre de la ruta al generar un vínculo.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a>Orden de ruta

Cuando el marco intenta hacer coincidir un URI con una ruta, evalúa las rutas en un orden determinado. Para especificar el orden, establezca la propiedad **Order** en el atributo Route. Primero se evalúan los valores inferiores. El valor de orden predeterminado es cero.

A continuación se indica cómo se determina el orden total:

1. Compare la propiedad **Order** del atributo Route.
2. Examine cada segmento del URI en la plantilla de ruta. Para cada segmento, orden de la manera siguiente:

    1. Segmentos literales.
    2. Parámetros de ruta con restricciones.
    3. Parámetros de ruta sin restricciones.
    4. Segmentos de parámetros comodín con restricciones.
    5. Segmentos de parámetros comodín sin restricciones.
3. En el caso de una empate, las rutas se ordenan por una comparación de cadenas ordinales sin distinción entre mayúsculas y minúsculas ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) de la plantilla de ruta.

A continuación se muestra un ejemplo. Supongamos que define el siguiente controlador:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

Estas rutas se ordenan de la manera siguiente.

1. pedidos y detalles
2. orders/{id}
3. orders/{customerName}
4. Orders/{\*Date}
5. pedidos/pendientes

Observe que "details" es un segmento literal y aparece antes que "{ID}", pero "Pending" aparece en último lugar porque la propiedad **Order** es 1. (En este ejemplo se da por supuesto que no hay clientes con el nombre "details" o "Pending". En general, intente evitar rutas ambiguas. En este ejemplo, una plantilla de ruta mejor para `GetByCustomer` es "Customers/{customerName}")
