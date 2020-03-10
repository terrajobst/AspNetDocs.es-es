---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Enrutamiento y selección de acciones en ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 12/14/2018
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 62114e56fb29e80c93b82dcb78ce2bc2a123a83b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78446917"
---
# <a name="routing-and-action-selection-in-aspnet-web-api"></a>Enrutamiento y selección de acciones en ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

En este artículo se describe cómo ASP.NET Web API enruta una solicitud HTTP a una acción determinada en un controlador.

> [!NOTE]
> Para obtener información general de alto nivel sobre el enrutamiento, consulte [enrutamiento en ASP.net web API](routing-in-aspnet-web-api.md).

En este artículo se examinan los detalles del proceso de enrutamiento. Si crea un proyecto de API Web y observa que algunas solicitudes no se enrutan de la manera esperada, esperamos que este artículo le ayude.

El enrutamiento tiene tres fases principales:

1. Coincidencia del URI con una plantilla de ruta.
2. Seleccionar un controlador.
3. Seleccionar una acción.

Puede reemplazar algunas partes del proceso por sus propios comportamientos personalizados. En este artículo, se describe el comportamiento predeterminado. Al final, se indican los lugares en los que puede personalizar el comportamiento.

## <a name="route-templates"></a>Plantillas de ruta

Una plantilla de ruta es similar a una ruta de acceso de URI, pero puede tener valores de marcador de posición, indicados con llaves:

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

Al crear una ruta, puede proporcionar valores predeterminados para algunos o todos los marcadores de posición:

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

También puede proporcionar restricciones, que restringen el modo en que un segmento URI puede coincidir con un marcador de posición:

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

El marco de trabajo intenta hacer coincidir los segmentos de la ruta de acceso del URI con la plantilla. Los literales de la plantilla deben coincidir exactamente. Un marcador de posición coincide con cualquier valor, a menos que se especifiquen restricciones. El marco de trabajo no coincide con otras partes del URI, como el nombre de host o los parámetros de consulta. El marco de trabajo selecciona la primera ruta de la tabla de rutas que coincide con el URI.

Hay dos marcadores de posición especiales: "{Controller}" y "{Action}".

- "{Controller}" proporciona el nombre del controlador.
- "{Action}" proporciona el nombre de la acción. En la API Web, la Convención habitual es omitir "{Action}".

### <a name="defaults"></a>Valores predeterminados

Si proporciona valores predeterminados, la ruta coincidirá con un URI al que les faltan esos segmentos. Por ejemplo:

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

Los URI `http://localhost/api/products/all` y `http://localhost/api/products` coinciden con la ruta anterior. En el último URI, el segmento de `{category}` que falta tiene asignado el valor predeterminado `all`.

### <a name="route-dictionary"></a>Diccionario de ruta

Si el marco de trabajo encuentra una coincidencia para un URI, crea un diccionario que contiene el valor de cada marcador de posición. Las claves son los nombres de marcador de posición, sin incluir las llaves. Los valores se toman de la ruta de acceso del URI o de los valores predeterminados. El diccionario se almacena en el objeto **IHttpRouteData** .

Durante esta fase de coincidencia de ruta, los marcadores de posición especiales "{Controller}" y "{Action}" se tratan igual que los demás marcadores de posición. Simplemente se almacenan en el diccionario con los demás valores.

Un valor predeterminado puede tener el valor especial **RouteParameter. Optional**. Si se asigna este valor a un marcador de posición, el valor no se agrega al Diccionario de ruta. Por ejemplo:

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

Para la ruta de acceso de URI "API/Products", el Diccionario de ruta contendrá:

- controlador: "Products"
- Categoría: "All"

En el caso de "API/Products/Toys/123", sin embargo, el Diccionario de ruta contendrá:

- controlador: "Products"
- Categoría: "juguetes"
- ID.: "123"

Los valores predeterminados también pueden incluir un valor que no aparece en ninguna parte de la plantilla de ruta. Si la ruta coincide, ese valor se almacena en el diccionario. Por ejemplo:

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

Si la ruta de acceso del URI es "API/root/8", el Diccionario contendrá dos valores:

- controlador: "Customers"
- ID.: "8"

## <a name="selecting-a-controller"></a>Selección de un controlador

El método **IHttpControllerSelector. SelectController** controla la selección del controlador. Este método toma una instancia de **HttpRequestMessage** y devuelve un **HttpControllerDescriptor**. La implementación predeterminada se proporciona mediante la clase **DefaultHttpControllerSelector** . Esta clase usa un algoritmo sencillo:

1. Busque la clave "Controller" en el Diccionario de ruta.
2. Tome el valor de esta clave y anexe la cadena "Controller" para obtener el nombre del tipo de controlador.
3. Busque un controlador de API Web con este nombre de tipo.

Por ejemplo, si el Diccionario de rutas contiene el par clave-valor "Controller" = "Products", el tipo de controlador es "ProductsController". Si no hay ningún tipo coincidente, o si hay varias coincidencias, el marco de trabajo devuelve un error al cliente.

En el paso 3, **DefaultHttpControllerSelector** usa la interfaz **IHttpControllerTypeResolver** para obtener la lista de tipos de controlador de API Web. La implementación predeterminada de **IHttpControllerTypeResolver** devuelve todas las clases públicas que (a) implementan **IHttpController**, (b) no son abstractas y (c) tienen un nombre que termina en "Controller".

## <a name="action-selection"></a>Selección de acción

Después de seleccionar el controlador, el marco de trabajo selecciona la acción llamando al método **IHttpActionSelector. SelectAction** . Este método toma un **HttpControllerContext** y devuelve un **HttpActionDescriptor**.

La implementación predeterminada se proporciona mediante la clase **ApiControllerActionSelector** . Para seleccionar una acción, tiene un aspecto similar al siguiente:

- Método HTTP de la solicitud.
- El marcador de posición "{Action}" en la plantilla de ruta, si está presente.
- Parámetros de las acciones en el controlador.

Antes de examinar el algoritmo de selección, necesitamos comprender algunas cosas sobre las acciones de controlador.

**¿Qué métodos del controlador se consideran "acciones"?** Al seleccionar una acción, el marco solo busca métodos de instancia públicos en el controlador. Además, excluye los métodos ["Special name"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) (constructores, eventos, sobrecargas de operador, etc.) y los métodos heredados de la clase **ApiController** .

**Métodos HTTP.** El marco de trabajo solo elige las acciones que coinciden con el método HTTP de la solicitud, que se determina de la manera siguiente:

1. Puede especificar el método HTTP con un atributo: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**o **HttpPut**.
2. De lo contrario, si el nombre del método de controlador comienza por "Get", "post", "put", "Delete", "Head", "Options" o "patch", entonces por Convención la acción admite ese método HTTP.
3. Si ninguno de los anteriores, el método admite POST.

**Enlaces de parámetros.** Un enlace de parámetros es cómo Web API crea un valor para un parámetro. Esta es la regla predeterminada para el enlace de parámetros:

- Los tipos simples se toman del URI.
- Los tipos complejos se toman del cuerpo de la solicitud.

Entre los tipos simples se incluyen todos los [.NET Framework tipos primitivos](https://msdn.microsoft.com/library/system.type.isprimitive), además de **DateTime**, **decimal**, **GUID**, **String**y **TimeSpan**. Para cada acción, a lo sumo un parámetro puede leer el cuerpo de la solicitud.

> [!NOTE]
> Es posible invalidar las reglas de enlace predeterminadas. Consulte [enlace de parámetros de WebAPI en el capó](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).

Con ese fondo, este es el algoritmo de selección de acción.

1. Cree una lista de todas las acciones del controlador que coincidan con el método de solicitud HTTP.
2. Si el Diccionario de rutas tiene una entrada de "acción", quite las acciones cuyo nombre no coincida con este valor.
3. Intente hacer coincidir los parámetros de acción con el URI, como se indica a continuación: 

    1. Para cada acción, obtenga una lista de los parámetros que son de tipo simple, donde el enlace obtiene el parámetro del URI. Excluya los parámetros opcionales.
    2. En esta lista, intente encontrar una coincidencia para cada nombre de parámetro, ya sea en el Diccionario de ruta o en la cadena de consulta de URI. Las coincidencias no distinguen mayúsculas de minúsculas y no dependen del orden de los parámetros.
    3. Seleccione una acción en la que todos los parámetros de la lista tengan una coincidencia en el URI.
    4. Si hay más de una acción que cumpla estos criterios, seleccione la que tenga más coincidencias de parámetros.
4. Omitir acciones con el atributo **[no Action]** .

El paso #3 es probablemente el más confuso. La idea básica es que un parámetro puede obtener su valor desde el URI, desde el cuerpo de la solicitud o desde un enlace personalizado. En el caso de los parámetros que proceden del URI, queremos asegurarnos de que el URI contiene realmente un valor para ese parámetro, ya sea en la ruta de acceso (a través del Diccionario de rutas) o en la cadena de consulta.

Por ejemplo, considere la siguiente acción:

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

El parámetro *ID* se enlaza al URI. Por lo tanto, esta acción solo puede coincidir con un URI que contenga un valor para "ID", ya sea en el Diccionario de ruta o en la cadena de consulta.

Los parámetros opcionales son una excepción, porque son opcionales. Para un parámetro opcional, es correcto si el enlace no puede obtener el valor del URI.

Los tipos complejos son una excepción por una razón diferente. Un tipo complejo solo puede enlazar con el URI a través de un enlace personalizado. Pero en ese caso, el marco no puede saber de antemano si el parámetro se enlaza a un URI determinado. Para averiguarlo, debe invocar el enlace. El objetivo del algoritmo de selección es seleccionar una acción de la descripción estática antes de invocar cualquier enlace. Por lo tanto, los tipos complejos se excluyen del algoritmo de coincidencia.

Una vez seleccionada la acción, se invocan todos los enlaces de parámetros.

Resumen:

- La acción debe coincidir con el método HTTP de la solicitud.
- El nombre de la acción debe coincidir con la entrada "acción" del Diccionario de rutas, si está presente.
- Para cada parámetro de la acción, si el parámetro se toma del URI, el nombre del parámetro debe encontrarse en el Diccionario de ruta o en la cadena de consulta de URI. (Se excluyen los parámetros y parámetros opcionales con tipos complejos).
- Intente buscar coincidencias con el mayor número de parámetros. La mejor coincidencia podría ser un método sin parámetros.

## <a name="extended-example"></a>Ejemplo extendido

Route

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

Controlador:

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

Solicitud HTTP:

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a>Coincidencia de rutas

El URI coincide con la ruta denominada "DefaultApi". El Diccionario de rutas contiene las siguientes entradas:

- controlador: "Products"
- ID.: "1"

El Diccionario de rutas no contiene los parámetros de la cadena de consulta, "version" y "details", pero todavía se tendrán en cuenta durante la selección de la acción.

### <a name="controller-selection"></a>Selección de controlador

Desde la entrada "Controller" en el Diccionario de ruta, el tipo de controlador es `ProductsController`.

### <a name="action-selection"></a>Selección de acción

La solicitud HTTP es una solicitud GET. Las acciones de controlador que admiten GET son `GetAll`, `GetById`y `FindProductsByName`. El Diccionario de rutas no contiene una entrada para "Action", por lo que no es necesario que coincida con el nombre de la acción.

A continuación, intentamos hacer coincidir los nombres de parámetro para las acciones, solo en las acciones GET.

| Acción | Parámetros que deben coincidir |
| --- | --- |
| `GetAll` | ninguna |
| `GetById` | "id" |
| `FindProductsByName` | Name |

Observe que no se tiene en cuenta el parámetro *version* de `GetById`, porque es un parámetro opcional.

El método `GetAll` coincide trivialmente. El método `GetById` también coincide, porque el Diccionario de rutas contiene "ID". El método `FindProductsByName` no coincide.

El método `GetById` gana, porque coincide con un parámetro, frente a ningún parámetro de `GetAll`. El método se invoca con los siguientes valores de parámetro:

- *ID.* = 1
- *versión* = 1,5

Tenga en cuenta que aunque la *versión* no se utilizó en el algoritmo de selección, el valor del parámetro procede de la cadena de consulta de URI.

## <a name="extension-points"></a>Puntos de extensión

La API Web proporciona puntos de extensión para algunas partes del proceso de enrutamiento.

| Interfaz | Description |
| --- | --- |
| **IHttpControllerSelector** | Selecciona el controlador. |
| **IHttpControllerTypeResolver** | Obtiene la lista de tipos de controlador. **DefaultHttpControllerSelector** elige el tipo de controlador de esta lista. |
| **IAssembliesResolver** | Obtiene la lista de ensamblados del proyecto. La interfaz **IHttpControllerTypeResolver** usa esta lista para encontrar los tipos de controlador. |
| **IHttpControllerActivator** | Crea nuevas instancias de controlador. |
| **IHttpActionSelector** | Selecciona la acción. |
| **IHttpActionInvoker** | Invoca la acción. |

Para proporcionar su propia implementación para cualquiera de estas interfaces, use la colección de **servicios** en el objeto **HttpConfiguration** :

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
