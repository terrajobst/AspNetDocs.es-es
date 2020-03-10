---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Controladores de pruebas unitarias en ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: En este tema se describen algunas técnicas específicas para los controladores de pruebas unitarias en Web API 2. Antes de leer este tema, puede que desee leer la unidad del tutorial...
ms.author: riande
ms.date: 06/11/2014
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: cdb1700537021e276669de1a9e0330a62659746c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447007"
---
# <a name="unit-testing-controllers-in-aspnet-web-api-2"></a>Controladores de pruebas unitarias en ASP.NET Web API 2

por [Mike Wasson](https://github.com/MikeWasson)

> En este tema se describen algunas técnicas específicas para los controladores de pruebas unitarias en Web API 2. Antes de leer este tema, puede que desee leer las [pruebas unitarias del tutorial ASP.net web API 2](unit-testing-with-aspnet-web-api.md), que muestra cómo agregar un proyecto de prueba unitaria a la solución.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - API Web 2
> - [MOQ](https://github.com/Moq) 4.5.30

> [!NOTE]
> He usado MOQ, pero la misma idea se aplica a cualquier marco ficticio. MOQ 4.5.30 (y versiones posteriores) es compatible con Visual Studio 2017, Roslyn y .NET 4,5 y versiones posteriores.

Un patrón común en las pruebas unitarias es &quot;Arrange-Act-Assert&quot;:

- Organizar: Configure los requisitos previos para que se ejecute la prueba.
- Act: realice la prueba.
- Assert: Compruebe que la prueba se ha realizado correctamente.

En el paso de organización, a menudo usará objetos ficticios o de código auxiliar. Esto minimiza el número de dependencias, por lo que la prueba se centra en probar una cosa.

Estas son algunas de las cosas que debe realizar en la prueba unitaria en los controladores de la API Web:

- La acción devuelve el tipo de respuesta correcto.
- Los parámetros no válidos devuelven la respuesta de error correcta.
- La acción llama al método correcto en el repositorio o el nivel de servicio.
- Si la respuesta incluye un modelo de dominio, compruebe el tipo de modelo.

Estos son algunos de los aspectos generales para probar, pero los detalles dependen de la implementación del controlador. En concreto, hace una gran diferencia si las acciones del controlador devuelven **HttpResponseMessage** o **IHttpActionResult**. Para obtener más información sobre estos tipos de resultado, consulte [resultados de la acción en Web API 2](../getting-started-with-aspnet-web-api/action-results.md).

## <a name="testing-actions-that-return-httpresponsemessage"></a>Acciones de prueba que devuelven HttpResponseMessage

A continuación se muestra un ejemplo de un controlador cuyas acciones devuelven **HttpResponseMessage**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

Observe que el controlador utiliza la inserción de dependencias para insertar un `IProductRepository`. Esto hace que el controlador sea más comprobable, ya que puede insertar un repositorio ficticio. La prueba unitaria siguiente comprueba que el método `Get` escribe un `Product` en el cuerpo de la respuesta. Supongamos que `repository` es un `IProductRepository`ficticio.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

Es importante establecer la **solicitud** y la **configuración** en el controlador. De lo contrario, se producirá un error en la prueba con una **excepción ArgumentNullException** o **InvalidOperationException**.

## <a name="testing-link-generation"></a>Probar la generación de vínculos

El método `Post` llama a **UrlHelper. Link** para crear vínculos en la respuesta. Esto requiere un poco más de configuración en la prueba unitaria:

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

La clase **UrlHelper** necesita la dirección URL de la solicitud y los datos de ruta, por lo que la prueba tiene que establecer valores para ellos. Otra opción es Mock o stub **UrlHelper**. Con este enfoque, reemplazará el valor predeterminado de [ApiController. URL](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) por una versión de código auxiliar o ficticio que devuelva un valor fijo.

Vamos a escribir la prueba con el marco [MOQ](https://github.com/Moq) . Instale el paquete NuGet de `Moq` en el proyecto de prueba.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

En esta versión, no es necesario configurar ningún dato de ruta, porque el **UrlHelper** ficticio devuelve una cadena de constante.

## <a name="testing-actions-that-return-ihttpactionresult"></a>Acciones de prueba que devuelven IHttpActionResult

En Web API 2, una acción de controlador puede devolver **IHttpActionResult**, que es análogo a **ACTIONRESULT** en ASP.NET MVC. La interfaz **IHttpActionResult** define un patrón de comando para crear respuestas http. En lugar de crear la respuesta directamente, el controlador devuelve un **IHttpActionResult**. Más adelante, la canalización invoca el **IHttpActionResult** para crear la respuesta. Este enfoque facilita la escritura de pruebas unitarias, ya que se puede omitir una gran cantidad de configuración que se necesita para **HttpResponseMessage**.

A continuación se muestra un controlador de ejemplo cuyas acciones devuelven **IHttpActionResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

En este ejemplo se muestran algunos patrones comunes con **IHttpActionResult**. Veamos cómo hacer pruebas unitarias.

### <a name="action-returns-200-ok-with-a-response-body"></a>La acción devuelve 200 (correcto) con un cuerpo de respuesta

El método `Get` llama a `Ok(product)` si se encuentra el producto. En la prueba unitaria, asegúrese de que el tipo de valor devuelto es **OkNegotiatedContentResult** y que el producto devuelto tiene el identificador correcto.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

Tenga en cuenta que la prueba unitaria no ejecuta el resultado de la acción. Puede suponer que el resultado de la acción crea la respuesta HTTP correctamente. (Este es el motivo por el que el marco de trabajo de la API Web tiene sus propias pruebas unitarias).

### <a name="action-returns-404-not-found"></a>La acción devuelve 404 (no encontrado)

El método `Get` llama a `NotFound()` si no se encuentra el producto. En este caso, la prueba unitaria solo comprueba si el tipo de valor devuelto es **NotFoundResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a>La acción devuelve 200 (correcto) sin cuerpo de respuesta

El método `Delete` llama a `Ok()` para devolver una respuesta HTTP 200 vacía. Al igual que en el ejemplo anterior, la prueba unitaria comprueba el tipo de valor devuelto, en este caso **OkResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a>La acción devuelve 201 (creado) con un encabezado de ubicación

El método `Post` llama a `CreatedAtRoute` para devolver una respuesta HTTP 201 con un URI en el encabezado Location. En la prueba unitaria, compruebe que la acción establece los valores de enrutamiento correctos.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a>La acción devuelve otro 2xx con un cuerpo de respuesta

El método `Put` llama a `Content` para devolver una respuesta HTTP 202 (aceptado) con un cuerpo de respuesta. Este caso es similar a devolver 200 (correcto), pero la prueba unitaria también debe comprobar el código de estado.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a>Recursos adicionales

- [Entity Framework ficticios cuando las pruebas unitarias ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- [Escritura de pruebas para un servicio de ASP.net web API](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (entrada de blog de Youssef Moussaoui).
- [Depurar ASP.NET Web API con Route Debugger](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
