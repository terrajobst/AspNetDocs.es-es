---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Llamada a una API Web desde un cliente .NETC#()-ASP.net 4. x
author: MikeWasson
description: En este tutorial se muestra cómo llamar a una API Web desde una aplicación .NET 4. x.
ms.author: riande
ms.date: 11/24/2017
ms.custom: seoapril2019
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: ab3ba71839123e848dffaa59871f9dac8c1a88d0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504961"
---
# <a name="call-a-web-api-from-a-net-client-c"></a>Llamada a una API Web desde un cliente .NETC#()

por [Mike Wasson](https://github.com/MikeWasson) y [Rick Anderson](https://twitter.com/RickAndMSFT)

[Descargar el proyecto completado](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample). [Instrucciones de descarga](/aspnet/core/tutorials/#how-to-download-a-sample). 

En este tutorial se muestra cómo llamar a una API Web desde una aplicación .NET mediante [System .net. http. HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)

En este tutorial, se escribe una aplicación cliente que usa la siguiente API Web:

| Acción | Método HTTP | URI relativo |
| --- | --- | --- |
| Obtener un producto por identificador | GET | *identificador* de/API/Products/ |
| Crear un nuevo producto | EXPONER | /api/products |
| Actualizar un producto | PUT | *identificador* de/API/Products/ |
| Eliminar un producto | SUPRIMIR | *identificador* de/API/Products/ |

Para obtener información sobre cómo implementar esta API con ASP.NET Web API, consulte [creación de una API Web que admita operaciones CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).

Para simplificar, la aplicación cliente de este tutorial es una aplicación de consola de Windows. **HttpClient** también se admite para las aplicaciones de Windows Phone y de la tienda Windows. Para obtener más información, consulte [escritura de código de cliente de API Web para varias plataformas mediante bibliotecas portátiles](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx) .

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a>Crear la aplicación de consola

En Visual Studio, cree una nueva aplicación de consola de Windows denominada **HttpClientSample** y péguela en el código siguiente:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

El código anterior es la aplicación cliente completa.

`RunAsync` ejecuta y se bloquea hasta que se completa. La mayoría de los métodos **HttpClient** son asincrónicos, ya que realizan operaciones de e/s de red. Todas las tareas asincrónicas se realizan dentro de `RunAsync`. Normalmente, una aplicación no bloquea el subproceso principal, pero esta aplicación no permite ninguna interacción.

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a>Instalación de las bibliotecas de cliente de Web API

Use el administrador de paquetes NuGet para instalar el paquete de bibliotecas de cliente de API Web.

En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**. En la consola del administrador de paquetes (PMC), escriba el siguiente comando:

`Install-Package Microsoft.AspNet.WebApi.Client`

El comando anterior agrega los siguientes paquetes de NuGet al proyecto:

* Microsoft.AspNet.WebApi.Client
* Newtonsoft.Json

Netwonsoft. JSON (también conocido como Json.NET) es un marco JSON de alto rendimiento muy conocido para .NET.

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a>Agregar una clase de modelo

Examine la clase `Product`:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

Esta clase coincide con el modelo de datos que usa la API Web. Una aplicación puede usar **HttpClient** para leer una instancia de `Product` de una respuesta http. La aplicación no tiene que escribir ningún código de deserialización.

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a>Creación e inicialización de HttpClient

Examine la propiedad **HttpClient** estática:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

**HttpClient** se ha diseñado para que se cree una instancia de una vez y se reutilice a lo largo de la vida de una aplicación. Las siguientes condiciones pueden dar lugar a errores de **SocketException** :

* Creación de una nueva instancia de **HttpClient** por solicitud.
* Servidor con mucha carga.

La creación de una nueva instancia de **HttpClient** por solicitud puede agotar los sockets disponibles.

El código siguiente inicializa la instancia **HttpClient** :

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

El código anterior:

* Establece el URI base para las solicitudes HTTP. Cambie el número de puerto al puerto usado en la aplicación de servidor. La aplicación no funcionará a menos que se use el puerto para la aplicación de servidor.
* Establece el encabezado Accept en "Application/JSON". Al establecer este encabezado, se indica al servidor que envíe datos en formato JSON.

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a>Enviar una solicitud GET para recuperar un recurso

El código siguiente envía una solicitud GET para un producto:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

El método **GetAsync** envía la solicitud HTTP GET. Cuando el método se completa, devuelve un **HttpResponseMessage** que contiene la respuesta http. Si el código de estado de la respuesta es un código de éxito, el cuerpo de respuesta contiene la representación JSON de un producto. Llame a **ReadAsAsync** para deserializar la carga de JSON en una instancia de `Product`. El método **ReadAsAsync** es asincrónico porque el cuerpo de la respuesta puede ser arbitrariamente grande.

**HttpClient** no inicia una excepción cuando la respuesta http contiene un código de error. En su lugar, la propiedad **IsSuccessStatusCode** es **false** si el estado es un código de error. Si prefiere tratar los códigos de error HTTP como excepciones, llame a [HttpResponseMessage. EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) en el objeto de respuesta. `EnsureSuccessStatusCode` produce una excepción si el código de estado está fuera del intervalo 200&ndash;299. Tenga en cuenta que **HttpClient** puede producir excepciones por otras razones &mdash; por ejemplo, si se agota el tiempo de espera de la solicitud.

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a>Formateadores de tipo de medio para deserializar

Cuando se llama a **ReadAsAsync** sin parámetros, usa un conjunto predeterminado de *formateadores multimedia* para leer el cuerpo de la respuesta. Los formateadores predeterminados admiten datos codificados con JSON, XML y la dirección URL de formulario.

En lugar de usar los formateadores predeterminados, puede proporcionar una lista de formateadores al método **ReadAsAsync** .  El uso de una lista de formateadores es útil si tiene un formateador de tipo multimedia personalizado:

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

Para obtener más información, consulte [formateadores de medios en ASP.net web API 2](../formats-and-model-binding/media-formatters.md) .

## <a name="sending-a-post-request-to-create-a-resource"></a>Envío de una solicitud POST para crear un recurso

El código siguiente envía una solicitud POST que contiene una instancia de `Product` en formato JSON:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

El método **PostAsJsonAsync** :

* Serializa un objeto a JSON.
* Envía la carga de JSON en una solicitud POST.

Si la solicitud se realiza correctamente:

* Debe devolver una respuesta 201 (creada).
* La respuesta debe incluir la dirección URL de los recursos creados en el encabezado de ubicación.

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a>Envío de una solicitud PUT para actualizar un recurso

El código siguiente envía una solicitud PUT para actualizar un producto:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

El método **PutAsJsonAsync** funciona como **PostAsJsonAsync**, salvo que envía una solicitud put en lugar de post.

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a>Enviar una solicitud de eliminación para eliminar un recurso

El código siguiente envía una solicitud de eliminación para eliminar un producto:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

Como GET, una solicitud DELETE no tiene un cuerpo de solicitud. No es necesario especificar el formato JSON o XML con DELETE.

## <a name="test-the-sample"></a>Prueba del ejemplo

Para probar la aplicación cliente:

1. [Descargue](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) y ejecute la aplicación de servidor. [Instrucciones de descarga](/aspnet/core/#how-to-download-a-sample). Compruebe que la aplicación de servidor funciona. Por ejemplo, `http://localhost:64195/api/products` debe devolver una lista de productos.
2. Establezca el URI base para las solicitudes HTTP. Cambie el número de puerto al puerto usado en la aplicación de servidor.
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. Ejecute la aplicación cliente. Se produce el siguiente resultado:

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
