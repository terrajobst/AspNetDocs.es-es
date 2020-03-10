---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Self-host ASP.NET Web API 1 (C#)-ASP.net 4. x
author: MikeWasson
description: Tutorial con código muestra cómo hospedar una API Web en una aplicación de consola.
ms.author: riande
ms.date: 01/26/2012
ms.custom: seoapril2019
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: bae1737ba5b16bc67fa0ed0474ff04df0add1b3a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78421375"
---
# <a name="self-host-aspnet-web-api-1-c"></a>Self-host ASP.NET Web API 1 (C#)

por [Mike Wasson](https://github.com/MikeWasson)

> En este tutorial se muestra cómo hospedar una API Web en una aplicación de consola. ASP.NET Web API no requiere IIS. Puede hospedar una API Web en su propio proceso de host. 
> 
> **Las nuevas aplicaciones deben usar OWIN para autohospedar la API Web.** Consulte [uso de OWIN para Autohospedar ASP.net web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - API web 1
> - Visual Studio 2012

## <a name="create-the-console-application-project"></a>Crear el proyecto de aplicación de consola

Inicie Visual Studio y seleccione **nuevo proyecto** en la página de **Inicio** . O bien, en el menú **archivo** , seleccione **nuevo** y luego **proyecto**.

En el panel **plantillas** , seleccione **plantillas instaladas** y expanda el nodo **Visual C#**  . En **Visual C#** , seleccione **Windows**. En la lista de plantillas de proyecto, seleccione **aplicación de consola**. Asigne al proyecto el nombre &quot;SelfHost&quot; y haga clic en **Aceptar**.

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a>Establecer la plataforma de destino (Visual Studio 2010)

Si usa Visual Studio 2010, cambie la versión de .NET Framework de destino a .NET Framework 4,0. (De forma predeterminada, la plantilla de proyecto tiene como destino [.NET Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile)).

En Explorador de soluciones, haga clic con el botón secundario en el proyecto y seleccione **propiedades**. En la lista desplegable **plataforma de destino** , cambie la versión de .NET Framework de destino a .NET Framework 4,0. Cuando se le pida que aplique el cambio, haga clic en **sí**.

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a>Instalar el administrador de paquetes NuGet

El administrador de paquetes NuGet es la forma más fácil de agregar los ensamblados de la API Web a un proyecto de non-ASP.NET.

Para comprobar si el administrador de paquetes NuGet está instalado, haga clic en el menú **herramientas** de Visual Studio. Si ve un elemento de menú denominado **Administrador de paquetes Nuget**, tiene el administrador de paquetes Nuget.

Para instalar el administrador de paquetes NuGet:

1. Inicie Visual Studio.
2. En el menú **Herramientas**, seleccione **Extensiones y actualizaciones**.
3. En el cuadro de diálogo **extensiones y actualizaciones** , seleccione **en línea**.
4. Si no ve "Administrador de paquetes NuGet", escriba "Administrador de paquetes Nuget" en el cuadro de búsqueda.
5. Seleccione el administrador de paquetes NuGet y haga clic en **Descargar**.
6. Una vez finalizada la descarga, se le pedirá que instale.
7. Una vez finalizada la instalación, es posible que se le pida que reinicie Visual Studio.

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a>Adición del paquete NuGet de Web API

Una vez instalado el administrador de paquetes NuGet, agregue el paquete de autohospedaje de la API Web al proyecto.

1. En el menú **herramientas** , seleccione **Administrador de paquetes NuGet**. *Nota*: Si no ve este elemento de menú, asegúrese de que el administrador de paquetes NuGet esté instalado correctamente.
2. Seleccione **administrar paquetes NuGet para la solución**
3. En el cuadro de diálogo **administrar paquetes** de **Internet, seleccione en línea**.
4. En el cuadro de búsqueda, escriba &quot;Microsoft. AspNet. WebApi. SelfHost&quot;.
5. Seleccione el paquete de host propio ASP.NET Web API y haga clic en **instalar**.
6. Una vez instalado el paquete, haga clic en **cerrar** para cerrar el cuadro de diálogo.

> [!NOTE]
> Asegúrese de instalar el paquete denominado Microsoft. AspNet. WebApi. SelfHost, no AspNetWebApi. SelfHost.

![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a>Crear el modelo y el controlador

En este tutorial se usan las mismas clases de modelo y controlador que en el tutorial de [Introducción](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) .

Agregue una clase pública denominada `Product`.

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

Agregue una clase pública denominada `ProductsController`. Derive esta clase de **System. Web. http. ApiController**.

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

Para obtener más información sobre el código de este controlador, vea el tutorial de [Introducción](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) . Este controlador define tres acciones GET:

| Identificador URI | Description |
| --- | --- |
| /api/products | Obtiene una lista de todos los productos. |
| *identificador* de/API/Products/ | Obtener un producto por identificador. |
| /API/Products/? categoría =*categoría* | Obtiene una lista de productos por categoría. |

## <a name="host-the-web-api"></a>Hospedar la API Web

Abra el archivo Program.cs y agregue las siguientes instrucciones Using:

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

Agregue el código siguiente a la clase **Program** .

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a>Opta Agregar una reserva de espacio de nombres de URL HTTP

Esta aplicación escucha en `http://localhost:8080/`. De forma predeterminada, la escucha en una dirección HTTP determinada requiere privilegios de administrador. Al ejecutar el tutorial, por lo tanto, puede obtener este error: "HTTP no pudo registrar la dirección URL http://+:8080/" hay dos maneras de evitar este error:

- Ejecutar Visual Studio con permisos de administrador elevados, o
- Use netsh. exe para conceder permisos de cuenta para reservar la dirección URL.

Para usar netsh. exe, abra un símbolo del sistema con privilegios de administrador y escriba el siguiente comando:

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

donde *equipo\nombredeusuario* es su cuenta de usuario.

Cuando haya terminado el autohospedado, asegúrese de eliminar la reserva:

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a>Llamar a la API Web desde una aplicación clienteC#()

Vamos a escribir una aplicación de consola simple que llama a la API Web.

Agregue un nuevo proyecto de aplicación de consola a la solución:

- En Explorador de soluciones, haga clic con el botón derecho en la solución y seleccione **Agregar nuevo proyecto**.
- Cree una nueva aplicación de consola denominada &quot;ClientApp&quot;.

![](self-host-a-web-api/_static/image5.png)

Use el administrador de paquetes NuGet para agregar el paquete de bibliotecas de ASP.NET Web API Core:

- En el menú herramientas, seleccione **Administrador de paquetes NuGet**.
- Seleccione **administrar paquetes NuGet para la solución**
- En el cuadro de diálogo **administrar paquetes NuGet** , seleccione **en línea**.
- En el cuadro de búsqueda, escriba &quot;Microsoft. AspNet. WebApi. Client&quot;.
- Seleccione el paquete de bibliotecas de cliente de API Web de Microsoft ASP.NET y haga clic en **instalar**.

Agregue una referencia en ClientApp al proyecto SelfHost:

- En Explorador de soluciones, haga clic con el botón derecho en el proyecto ClientApp.
- Seleccione **Agregar referencia**.
- En el cuadro de diálogo **Administrador de referencias** , en **solución**, seleccione **proyectos**.
- Seleccione el proyecto SelfHost.
- Haga clic en **Aceptar**.

![](self-host-a-web-api/_static/image6.png)

Abra el archivo Client/Program. cs. Agregue la siguiente instrucción **using** :

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

Agregue una instancia de **HttpClient** estática:

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

Agregue los siguientes métodos para enumerar todos los productos, mostrar un producto por identificador y mostrar los productos por categoría.

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

Cada uno de estos métodos sigue el mismo patrón:

1. Llame a **HttpClient. GetAsync** para enviar una solicitud GET al URI adecuado.
2. Llame a **HttpResponseMessage. EnsureSuccessStatusCode**. Este método produce una excepción si el estado de la respuesta HTTP es un código de error.
3. Llame a **ReadAsAsync&lt;t&gt;** para deserializar un tipo de CLR a partir de la respuesta http. Este método es un método de extensión, que se define en **System .net. http. HttpContentExtensions**.

Los métodos **GetAsync** y **ReadAsAsync** son asincrónicos. Devuelven objetos de **tarea** que representan la operación asincrónica. Al obtener la propiedad **resultado** , el subproceso se bloquea hasta que se completa la operación.

Para obtener más información sobre el uso de HttpClient, incluido cómo realizar llamadas sin bloqueo, consulte [llamar a una API Web desde un cliente .net](../advanced/calling-a-web-api-from-a-net-client.md).

Antes de llamar a estos métodos, establezca la propiedad BaseAddress en la instancia HttpClient en "`http://localhost:8080`". Por ejemplo:

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

Esto debería generar la siguiente información. (Recuerde ejecutar primero la aplicación SelfHost).

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
