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
# <a name="tracing-in-aspnet-web-api-2"></a><span data-ttu-id="0f3cc-103">Seguimiento en ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="0f3cc-103">Tracing in ASP.NET Web API 2</span></span>

<span data-ttu-id="0f3cc-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0f3cc-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="0f3cc-105">Cuando se intenta depurar una aplicación basada en Web, no hay ningún sustituto para un buen conjunto de registros de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-105">When you are trying to debug a web-based application, there is no substitute for a good set of trace logs.</span></span> <span data-ttu-id="0f3cc-106">En este tutorial se muestra cómo habilitar el seguimiento en ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-106">This tutorial shows how to enable tracing in ASP.NET Web API.</span></span> <span data-ttu-id="0f3cc-107">Puede usar esta característica para realizar un seguimiento de lo que hace el marco de la API Web antes y después de invocar el controlador.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-107">You can use this feature to trace what the Web API framework does before and after it invokes your controller.</span></span> <span data-ttu-id="0f3cc-108">También puede usarla para realizar un seguimiento de su propio código.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-108">You can also use it to trace your own code.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="0f3cc-109">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="0f3cc-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="0f3cc-110">[Visual studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) (también funciona con visual Studio 2015)</span><span class="sxs-lookup"><span data-stu-id="0f3cc-110">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) (also works with Visual Studio 2015)</span></span>
> - <span data-ttu-id="0f3cc-111">API Web 2</span><span class="sxs-lookup"><span data-stu-id="0f3cc-111">Web API 2</span></span>
> - [<span data-ttu-id="0f3cc-112">Microsoft. AspNet. WebApi. Tracing</span><span class="sxs-lookup"><span data-stu-id="0f3cc-112">Microsoft.AspNet.WebApi.Tracing</span></span>](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)

## <a name="enable-systemdiagnostics-tracing-in-web-api"></a><span data-ttu-id="0f3cc-113">Habilitación del seguimiento de System. Diagnostics en Web API</span><span class="sxs-lookup"><span data-stu-id="0f3cc-113">Enable System.Diagnostics Tracing in Web API</span></span>

<span data-ttu-id="0f3cc-114">En primer lugar, vamos a crear un nuevo proyecto de aplicación Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-114">First, we'll create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="0f3cc-115">En Visual Studio, en el menú **archivo** , seleccione **nuevo** **proyecto**de > .</span><span class="sxs-lookup"><span data-stu-id="0f3cc-115">In Visual Studio, from the **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="0f3cc-116">En **plantillas**, **Web**, seleccione **aplicación Web ASP.net**.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-116">Under **Templates**, **Web**, select **ASP.NET Web Application**.</span></span>

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="0f3cc-117">Elija la plantilla de proyecto de API Web.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-117">Choose the Web API project template.</span></span>

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

<span data-ttu-id="0f3cc-118">En el menú **herramientas** , seleccione **Administrador de paquetes NuGet**y, a continuación, **paquete administrar consola**.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-118">From the **Tools** menu, select **NuGet Package Manager**, then **Package Manage Console**.</span></span>

<span data-ttu-id="0f3cc-119">En la ventana de la consola del administrador de paquetes, escriba los siguientes comandos.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-119">In the Package Manager Console window, type the following commands.</span></span>

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

<span data-ttu-id="0f3cc-120">El primer comando instala el paquete de seguimiento de API Web más reciente.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-120">The first command installs the latest Web API tracing package.</span></span> <span data-ttu-id="0f3cc-121">También se actualizan los paquetes principales de la API Web.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-121">It also updates the core Web API packages.</span></span> <span data-ttu-id="0f3cc-122">El segundo comando actualiza el paquete WebApi. webhost a la versión más reciente.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-122">The second command updates the WebApi.WebHost package to the latest version.</span></span>

> [!NOTE]
> <span data-ttu-id="0f3cc-123">Si desea establecer como destino una versión específica de Web API, use la marca-version al instalar el paquete de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-123">If you want to target a specific version of Web API, use the -Version flag when you install the tracing package.</span></span>

<span data-ttu-id="0f3cc-124">Abra el archivo WebApiConfig.cs en la carpeta de inicio del\_de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-124">Open the file WebApiConfig.cs in the App\_Start folder.</span></span> <span data-ttu-id="0f3cc-125">Agregue el código siguiente al método **Register** .</span><span class="sxs-lookup"><span data-stu-id="0f3cc-125">Add the following code to the **Register** method.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

<span data-ttu-id="0f3cc-126">Este código agrega la clase [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) a la canalización de la API Web.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-126">This code adds the [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) class to the Web API pipeline.</span></span> <span data-ttu-id="0f3cc-127">La clase **SystemDiagnosticsTraceWriter** escribe seguimientos en [System. Diagnostics. Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).</span><span class="sxs-lookup"><span data-stu-id="0f3cc-127">The **SystemDiagnosticsTraceWriter** class writes traces to [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).</span></span>

<span data-ttu-id="0f3cc-128">Para ver los seguimientos, ejecute la aplicación en el depurador.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-128">To see the traces, run the application in the debugger.</span></span> <span data-ttu-id="0f3cc-129">En el explorador, vaya a `/api/values`.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-129">In the browser, navigate to `/api/values`.</span></span>

![](tracing-in-aspnet-web-api/_static/image5.png)

<span data-ttu-id="0f3cc-130">Las instrucciones de seguimiento se escriben en la ventana de salida de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-130">The trace statements are written to the Output window in Visual Studio.</span></span> <span data-ttu-id="0f3cc-131">(En el menú **Ver** , seleccione **salida**).</span><span class="sxs-lookup"><span data-stu-id="0f3cc-131">(From the **View** menu, select **Output**).</span></span>

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

<span data-ttu-id="0f3cc-132">Dado que **SystemDiagnosticsTraceWriter** escribe seguimientos en **System. Diagnostics. Trace**, puede registrar agentes de escucha de seguimiento adicionales; por ejemplo, para escribir seguimientos en un archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-132">Because **SystemDiagnosticsTraceWriter** writes traces to **System.Diagnostics.Trace**, you can register additional trace listeners; for example, to write traces to a log file.</span></span> <span data-ttu-id="0f3cc-133">Para obtener más información acerca de los escritores de seguimiento, vea el tema [agentes de escucha de seguimiento](https://msdn.microsoft.com/library/4y5y10s7.aspx) en MSDN.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-133">For more information about trace writers, see the [Trace Listeners](https://msdn.microsoft.com/library/4y5y10s7.aspx) topic on MSDN.</span></span>

### <a name="configuring-systemdiagnosticstracewriter"></a><span data-ttu-id="0f3cc-134">Configuración de SystemDiagnosticsTraceWriter</span><span class="sxs-lookup"><span data-stu-id="0f3cc-134">Configuring SystemDiagnosticsTraceWriter</span></span>

<span data-ttu-id="0f3cc-135">En el código siguiente se muestra cómo configurar el escritor de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-135">The following code shows how to configure the trace writer.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="0f3cc-136">Hay dos opciones de configuración que puede controlar:</span><span class="sxs-lookup"><span data-stu-id="0f3cc-136">There are two settings that you can control:</span></span>

- <span data-ttu-id="0f3cc-137">IsVerbose: si es false, cada seguimiento contiene información mínima.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-137">IsVerbose: If false, each trace contains minimal information.</span></span> <span data-ttu-id="0f3cc-138">Si es true, los seguimientos incluyen más información.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-138">If true, traces include more information.</span></span>
- <span data-ttu-id="0f3cc-139">MinimumLevel: establece el nivel de seguimiento mínimo.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-139">MinimumLevel: Sets the minimum trace level.</span></span> <span data-ttu-id="0f3cc-140">Los niveles de seguimiento, en orden, son debug, info, WARN, error y fatal.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-140">Trace levels, in order, are Debug, Info, Warn, Error, and Fatal.</span></span>

## <a name="adding-traces-to-your-web-api-application"></a><span data-ttu-id="0f3cc-141">Agregar seguimientos a la aplicación de API Web</span><span class="sxs-lookup"><span data-stu-id="0f3cc-141">Adding Traces to Your Web API Application</span></span>

<span data-ttu-id="0f3cc-142">Agregar un escritor de seguimiento proporciona acceso inmediato a los seguimientos creados por la canalización de API Web.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-142">Adding a trace writer gives you immediate access to the traces created by the Web API pipeline.</span></span> <span data-ttu-id="0f3cc-143">También puede usar el escritor de seguimiento para realizar un seguimiento de su propio código:</span><span class="sxs-lookup"><span data-stu-id="0f3cc-143">You can also use the trace writer to trace your own code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="0f3cc-144">Para obtener el escritor de seguimiento, llame a **HttpConfiguration. Services. GetTraceWriter**.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-144">To get the trace writer, call **HttpConfiguration.Services.GetTraceWriter**.</span></span> <span data-ttu-id="0f3cc-145">Desde un controlador, este método es accesible a través de la propiedad **ApiController. Configuration** .</span><span class="sxs-lookup"><span data-stu-id="0f3cc-145">From a controller, this method is accessible through the **ApiController.Configuration** property.</span></span>

<span data-ttu-id="0f3cc-146">Para escribir un seguimiento, puede llamar al método **ITraceWriter. Trace** directamente, pero la clase [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) define algunos métodos de extensión que son más fáciles de ver.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-146">To write a trace, you can call the **ITraceWriter.Trace** method directly, but the [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) class defines some extension methods that are more friendly.</span></span> <span data-ttu-id="0f3cc-147">Por ejemplo, el método **info** mostrado anteriormente crea un seguimiento con **información**de nivel de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-147">For example, the **Info** method shown above creates a trace with trace level **Info**.</span></span>

## <a name="web-api-tracing-infrastructure"></a><span data-ttu-id="0f3cc-148">Infraestructura de seguimiento de API Web</span><span class="sxs-lookup"><span data-stu-id="0f3cc-148">Web API Tracing Infrastructure</span></span>

<span data-ttu-id="0f3cc-149">En esta sección se describe cómo escribir un escritor de seguimiento personalizado para la API Web.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-149">This section describes how to write a custom trace writer for Web API.</span></span>

<span data-ttu-id="0f3cc-150">El paquete Microsoft. AspNet. WebApi. Tracing se basa en una infraestructura de seguimiento más general en Web API.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-150">The Microsoft.AspNet.WebApi.Tracing package is built on top of a more general tracing infrastructure in Web API.</span></span> <span data-ttu-id="0f3cc-151">En lugar de usar Microsoft. AspNet. WebApi. Tracing, también puede conectar otra biblioteca de seguimiento/registro, como [NLog](http://nlog-project.org/) o [log4net](http://logging.apache.org/log4net/).</span><span class="sxs-lookup"><span data-stu-id="0f3cc-151">Instead of using Microsoft.AspNet.WebApi.Tracing, you can also plug in some other tracing/logging library, such as [NLog](http://nlog-project.org/) or [log4net](http://logging.apache.org/log4net/).</span></span>

<span data-ttu-id="0f3cc-152">Para recopilar seguimientos, implemente la interfaz **ITraceWriter** .</span><span class="sxs-lookup"><span data-stu-id="0f3cc-152">To collect traces, implement the **ITraceWriter** interface.</span></span> <span data-ttu-id="0f3cc-153">A continuación se incluye un ejemplo sencillo:</span><span class="sxs-lookup"><span data-stu-id="0f3cc-153">Here is a simple example:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="0f3cc-154">El método **ITraceWriter. Trace** crea un seguimiento.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-154">The **ITraceWriter.Trace** method creates a trace.</span></span> <span data-ttu-id="0f3cc-155">El autor de la llamada especifica una categoría y un nivel de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-155">The caller specifies a category and trace level.</span></span> <span data-ttu-id="0f3cc-156">La categoría puede ser cualquier cadena definida por el usuario.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-156">The category can be any user-defined string.</span></span> <span data-ttu-id="0f3cc-157">La implementación de **seguimiento** debe hacer lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="0f3cc-157">Your implementation of **Trace** should do the following:</span></span>

1. <span data-ttu-id="0f3cc-158">Cree un nuevo **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-158">Create a new **TraceRecord**.</span></span> <span data-ttu-id="0f3cc-159">Inicialícelo con la solicitud, la categoría y el nivel de seguimiento, como se muestra.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-159">Initialize it with the request, category, and trace level, as shown.</span></span> <span data-ttu-id="0f3cc-160">El autor de la llamada proporciona estos valores.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-160">These values are provided by the caller.</span></span>
2. <span data-ttu-id="0f3cc-161">Invoque el delegado de *traceAction* .</span><span class="sxs-lookup"><span data-stu-id="0f3cc-161">Invoke the *traceAction* delegate.</span></span> <span data-ttu-id="0f3cc-162">Dentro de este delegado, se espera que el llamador rellene el resto de **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-162">Inside this delegate, the caller is expected to fill in the rest of the **TraceRecord**.</span></span>
3. <span data-ttu-id="0f3cc-163">Escriba **TraceRecord**con cualquier técnica de registro que desee.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-163">Write the **TraceRecord**, using any logging technique that you like.</span></span> <span data-ttu-id="0f3cc-164">El ejemplo que se muestra aquí simplemente llama a **System. Diagnostics. Trace**.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-164">The example shown here simply calls into **System.Diagnostics.Trace**.</span></span>

## <a name="setting-the-trace-writer"></a><span data-ttu-id="0f3cc-165">Establecimiento del escritor de seguimiento</span><span class="sxs-lookup"><span data-stu-id="0f3cc-165">Setting the Trace Writer</span></span>

<span data-ttu-id="0f3cc-166">Para habilitar el seguimiento, debe configurar la API Web para usar la implementación de **ITraceWriter** .</span><span class="sxs-lookup"><span data-stu-id="0f3cc-166">To enable tracing, you must configure Web API to use your **ITraceWriter** implementation.</span></span> <span data-ttu-id="0f3cc-167">Esto se hace a través del objeto **HttpConfiguration** , como se muestra en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0f3cc-167">You do this through the **HttpConfiguration** object, as shown in the following code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="0f3cc-168">Solo un escritor de seguimiento puede estar activo.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-168">Only one trace writer can be active.</span></span> <span data-ttu-id="0f3cc-169">De forma predeterminada, Web API establece un &quot;&quot; no operativo que no hace nada.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-169">By default, Web API sets a &quot;no-op&quot; tracer that does nothing.</span></span> <span data-ttu-id="0f3cc-170">(El &quot;control de&quot; no operativo existe para que el código de seguimiento no tenga que comprobar si el escritor de seguimiento es **null** antes de escribir un seguimiento).</span><span class="sxs-lookup"><span data-stu-id="0f3cc-170">(The &quot;no-op&quot; tracer exists so that tracing code does not have to check whether the trace writer is **null** before writing a trace.)</span></span>

## <a name="how-web-api-tracing-works"></a><span data-ttu-id="0f3cc-171">Cómo funciona el seguimiento de API Web</span><span class="sxs-lookup"><span data-stu-id="0f3cc-171">How Web API Tracing Works</span></span>

<span data-ttu-id="0f3cc-172">El seguimiento en Web API usa un patrón de *fachada* : cuando el seguimiento está habilitado, la API Web encapsula varias partes de la canalización de solicitudes con clases que realizan llamadas de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-172">Tracing in Web API uses a *facade* pattern: When tracing is enabled, Web API wraps various parts of the request pipeline with classes that perform trace calls.</span></span>

<span data-ttu-id="0f3cc-173">Por ejemplo, al seleccionar un controlador, la canalización usa la interfaz **IHttpControllerSelector** .</span><span class="sxs-lookup"><span data-stu-id="0f3cc-173">For example, when selecting a controller, the pipeline uses the **IHttpControllerSelector** interface.</span></span> <span data-ttu-id="0f3cc-174">Con el seguimiento habilitado, la canalización inserta una clase que implementa **IHttpControllerSelector** pero llama a a través de la implementación real:</span><span class="sxs-lookup"><span data-stu-id="0f3cc-174">With tracing enabled, the pipeline inserts a class that implements **IHttpControllerSelector** but calls through to the real implementation:</span></span>

![El seguimiento de la API Web usa el patrón de fachada.](tracing-in-aspnet-web-api/_static/image8.png)

<span data-ttu-id="0f3cc-176">Las ventajas de este diseño incluyen:</span><span class="sxs-lookup"><span data-stu-id="0f3cc-176">The benefits of this design include:</span></span>

- <span data-ttu-id="0f3cc-177">Si no agrega un escritor de seguimiento, no se crean instancias de los componentes de seguimiento y no tienen ningún impacto en el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-177">If you do not add a trace writer, the tracing components are not instantiated and have no performance impact.</span></span>
- <span data-ttu-id="0f3cc-178">Si reemplaza los servicios predeterminados, como **IHttpControllerSelector** , por su propia implementación personalizada, el seguimiento no se verá afectado, ya que el seguimiento lo realiza el objeto contenedor.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-178">If you replace default services such as **IHttpControllerSelector** with your own custom implementation, tracing is not affected, because tracing is done by the wrapper object.</span></span>

<span data-ttu-id="0f3cc-179">También puede reemplazar el marco de seguimiento de la API Web completo por su propio marco de trabajo personalizado, reemplazando el servicio **ITraceManager** predeterminado:</span><span class="sxs-lookup"><span data-stu-id="0f3cc-179">You can also replace the entire Web API trace framework with your own custom framework, by replacing the default **ITraceManager** service:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="0f3cc-180">Implemente **ITraceManager. Initialize** para inicializar el sistema de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-180">Implement **ITraceManager.Initialize** to initialize your tracing system.</span></span> <span data-ttu-id="0f3cc-181">Tenga en cuenta que esto reemplaza *todo* el marco de seguimiento, incluido todo el código de seguimiento que está integrado en Web API.</span><span class="sxs-lookup"><span data-stu-id="0f3cc-181">Be aware that this replaces the *entire* trace framework, including all of the tracing code that is built into Web API.</span></span>
