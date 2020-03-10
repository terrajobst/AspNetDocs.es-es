---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: Seguimiento en ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Muestra cómo habilitar el seguimiento en ASP.NET Web API.
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a01acb649556d06ab9828ceab0fcbdf363bbc0d1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484345"
---
# <a name="tracing-in-aspnet-web-api-2"></a>Seguimiento en ASP.NET Web API 2

por [Mike Wasson](https://github.com/MikeWasson)

> Cuando se intenta depurar una aplicación basada en Web, no hay ningún sustituto para un buen conjunto de registros de seguimiento. En este tutorial se muestra cómo habilitar el seguimiento en ASP.NET Web API. Puede usar esta característica para realizar un seguimiento de lo que hace el marco de la API Web antes y después de invocar el controlador. También puede usarla para realizar un seguimiento de su propio código.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
>
> - [Visual studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) (también funciona con visual Studio 2015)
> - API Web 2
> - [Microsoft. AspNet. WebApi. Tracing](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)

## <a name="enable-systemdiagnostics-tracing-in-web-api"></a>Habilitación del seguimiento de System. Diagnostics en Web API

En primer lugar, vamos a crear un nuevo proyecto de aplicación Web ASP.NET. En Visual Studio, en el menú **archivo** , seleccione **nuevo** **proyecto**de > . En **plantillas**, **Web**, seleccione **aplicación Web ASP.net**.

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

Elija la plantilla de proyecto de API Web.

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

En el menú **herramientas** , seleccione **Administrador de paquetes NuGet**y, a continuación, **paquete administrar consola**.

En la ventana de la consola del administrador de paquetes, escriba los siguientes comandos.

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

El primer comando instala el paquete de seguimiento de API Web más reciente. También se actualizan los paquetes principales de la API Web. El segundo comando actualiza el paquete WebApi. webhost a la versión más reciente.

> [!NOTE]
> Si desea establecer como destino una versión específica de Web API, use la marca-version al instalar el paquete de seguimiento.

Abra el archivo WebApiConfig.cs en la carpeta de inicio del\_de la aplicación. Agregue el código siguiente al método **Register** .

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

Este código agrega la clase [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) a la canalización de la API Web. La clase **SystemDiagnosticsTraceWriter** escribe seguimientos en [System. Diagnostics. Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).

Para ver los seguimientos, ejecute la aplicación en el depurador. En el explorador, vaya a `/api/values`.

![](tracing-in-aspnet-web-api/_static/image5.png)

Las instrucciones de seguimiento se escriben en la ventana de salida de Visual Studio. (En el menú **Ver** , seleccione **salida**).

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

Dado que **SystemDiagnosticsTraceWriter** escribe seguimientos en **System. Diagnostics. Trace**, puede registrar agentes de escucha de seguimiento adicionales; por ejemplo, para escribir seguimientos en un archivo de registro. Para obtener más información acerca de los escritores de seguimiento, vea el tema [agentes de escucha de seguimiento](https://msdn.microsoft.com/library/4y5y10s7.aspx) en MSDN.

### <a name="configuring-systemdiagnosticstracewriter"></a>Configuración de SystemDiagnosticsTraceWriter

En el código siguiente se muestra cómo configurar el escritor de seguimiento.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

Hay dos opciones de configuración que puede controlar:

- IsVerbose: si es false, cada seguimiento contiene información mínima. Si es true, los seguimientos incluyen más información.
- MinimumLevel: establece el nivel de seguimiento mínimo. Los niveles de seguimiento, en orden, son debug, info, WARN, error y fatal.

## <a name="adding-traces-to-your-web-api-application"></a>Agregar seguimientos a la aplicación de API Web

Agregar un escritor de seguimiento proporciona acceso inmediato a los seguimientos creados por la canalización de API Web. También puede usar el escritor de seguimiento para realizar un seguimiento de su propio código:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

Para obtener el escritor de seguimiento, llame a **HttpConfiguration. Services. GetTraceWriter**. Desde un controlador, este método es accesible a través de la propiedad **ApiController. Configuration** .

Para escribir un seguimiento, puede llamar al método **ITraceWriter. Trace** directamente, pero la clase [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) define algunos métodos de extensión que son más fáciles de ver. Por ejemplo, el método **info** mostrado anteriormente crea un seguimiento con **información**de nivel de seguimiento.

## <a name="web-api-tracing-infrastructure"></a>Infraestructura de seguimiento de API Web

En esta sección se describe cómo escribir un escritor de seguimiento personalizado para la API Web.

El paquete Microsoft. AspNet. WebApi. Tracing se basa en una infraestructura de seguimiento más general en Web API. En lugar de usar Microsoft. AspNet. WebApi. Tracing, también puede conectar otra biblioteca de seguimiento/registro, como [NLog](http://nlog-project.org/) o [log4net](http://logging.apache.org/log4net/).

Para recopilar seguimientos, implemente la interfaz **ITraceWriter** . A continuación se incluye un ejemplo sencillo:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

El método **ITraceWriter. Trace** crea un seguimiento. El autor de la llamada especifica una categoría y un nivel de seguimiento. La categoría puede ser cualquier cadena definida por el usuario. La implementación de **seguimiento** debe hacer lo siguiente:

1. Cree un nuevo **TraceRecord**. Inicialícelo con la solicitud, la categoría y el nivel de seguimiento, como se muestra. El autor de la llamada proporciona estos valores.
2. Invoque el delegado de *traceAction* . Dentro de este delegado, se espera que el llamador rellene el resto de **TraceRecord**.
3. Escriba **TraceRecord**con cualquier técnica de registro que desee. El ejemplo que se muestra aquí simplemente llama a **System. Diagnostics. Trace**.

## <a name="setting-the-trace-writer"></a>Establecimiento del escritor de seguimiento

Para habilitar el seguimiento, debe configurar la API Web para usar la implementación de **ITraceWriter** . Esto se hace a través del objeto **HttpConfiguration** , como se muestra en el código siguiente:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

Solo un escritor de seguimiento puede estar activo. De forma predeterminada, Web API establece un &quot;&quot; no operativo que no hace nada. (El &quot;control de&quot; no operativo existe para que el código de seguimiento no tenga que comprobar si el escritor de seguimiento es **null** antes de escribir un seguimiento).

## <a name="how-web-api-tracing-works"></a>Cómo funciona el seguimiento de API Web

El seguimiento en Web API usa un patrón de *fachada* : cuando el seguimiento está habilitado, la API Web encapsula varias partes de la canalización de solicitudes con clases que realizan llamadas de seguimiento.

Por ejemplo, al seleccionar un controlador, la canalización usa la interfaz **IHttpControllerSelector** . Con el seguimiento habilitado, la canalización inserta una clase que implementa **IHttpControllerSelector** pero llama a a través de la implementación real:

![El seguimiento de la API Web usa el patrón de fachada.](tracing-in-aspnet-web-api/_static/image8.png)

Las ventajas de este diseño incluyen:

- Si no agrega un escritor de seguimiento, no se crean instancias de los componentes de seguimiento y no tienen ningún impacto en el rendimiento.
- Si reemplaza los servicios predeterminados, como **IHttpControllerSelector** , por su propia implementación personalizada, el seguimiento no se verá afectado, ya que el seguimiento lo realiza el objeto contenedor.

También puede reemplazar el marco de seguimiento de la API Web completo por su propio marco de trabajo personalizado, reemplazando el servicio **ITraceManager** predeterminado:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

Implemente **ITraceManager. Initialize** para inicializar el sistema de seguimiento. Tenga en cuenta que esto reemplaza *todo* el marco de seguimiento, incluido todo el código de seguimiento que está integrado en Web API.
