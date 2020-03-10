---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: Qué no hacer en ASP.NET y qué hacer en su lugar | Microsoft Docs
author: Rick-Anderson
description: En este tema se describen varios errores comunes que los usuarios realizan en los proyectos Web de ASP.NET. Proporciona recomendaciones sobre lo que debe hacer para evitar estas comunicaciones...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 980d3544df70643043391e6573803ce21b3a824f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78500137"
---
# <a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a><span data-ttu-id="d8966-104">Qué no se debe hacer en ASP.NET y qué hacer en su lugar</span><span class="sxs-lookup"><span data-stu-id="d8966-104">What not to do in ASP.NET, and what to do instead</span></span>

> <span data-ttu-id="d8966-105">En este tema se describen varios errores comunes que los usuarios realizan en los proyectos Web de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d8966-105">This topic describes several common mistakes people make within ASP.NET web projects.</span></span> <span data-ttu-id="d8966-106">Proporciona recomendaciones sobre lo que debe hacer para evitar estos errores comunes.</span><span class="sxs-lookup"><span data-stu-id="d8966-106">It provides recommendations for what you should do to avoid these common mistakes.</span></span> <span data-ttu-id="d8966-107">Se basa en una [presentación](http://vimeo.com/68390507) de **Damian Edwards** en la Conferencia de desarrolladores de noruego.</span><span class="sxs-lookup"><span data-stu-id="d8966-107">It is based on a [presentation](http://vimeo.com/68390507) by **Damian Edwards** at Norwegian Developers Conference.</span></span>

## <a name="disclaimer"></a><span data-ttu-id="d8966-108">Declinación de responsabilidades</span><span class="sxs-lookup"><span data-stu-id="d8966-108">Disclaimer</span></span>

<span data-ttu-id="d8966-109">Este tema no pretende ser una guía completa para asegurarse de que la aplicación sea segura y eficaz.</span><span class="sxs-lookup"><span data-stu-id="d8966-109">This topic is not intended as a complete guide to ensure your application is secure and efficient.</span></span> <span data-ttu-id="d8966-110">Todavía debe seguir los procedimientos recomendados de seguridad y rendimiento que no se describen en este tema.</span><span class="sxs-lookup"><span data-stu-id="d8966-110">You still need to follow best practices for security and performance that are not outlined in this topic.</span></span> <span data-ttu-id="d8966-111">Solo sugiere cómo evitar errores comunes relacionados con las clases y los procesos de .NET.</span><span class="sxs-lookup"><span data-stu-id="d8966-111">It only suggests how to avoid common mistakes related to .NET classes and processes.</span></span>

## <a name="overview"></a><span data-ttu-id="d8966-112">Información general</span><span class="sxs-lookup"><span data-stu-id="d8966-112">Overview</span></span>

<span data-ttu-id="d8966-113">Este tema contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="d8966-113">This topic contains the following sections:</span></span>

- [<span data-ttu-id="d8966-114">Cumplimiento de estándares</span><span class="sxs-lookup"><span data-stu-id="d8966-114">Standards Compliance</span></span>](#standards)

    - [<span data-ttu-id="d8966-115">Adaptadores de control</span><span class="sxs-lookup"><span data-stu-id="d8966-115">Control Adapters</span></span>](#adapters)
    - [<span data-ttu-id="d8966-116">Propiedades de estilo en controles</span><span class="sxs-lookup"><span data-stu-id="d8966-116">Style Properties on Controls</span></span>](#styleprop)
    - [<span data-ttu-id="d8966-117">Devoluciones de llamada de páginas y controles</span><span class="sxs-lookup"><span data-stu-id="d8966-117">Page and Control Callbacks</span></span>](#callback)
    - [<span data-ttu-id="d8966-118">Detección de capacidad del explorador</span><span class="sxs-lookup"><span data-stu-id="d8966-118">Browser Capability Detection</span></span>](#browsercap)
- [<span data-ttu-id="d8966-119">Seguridad</span><span class="sxs-lookup"><span data-stu-id="d8966-119">Security</span></span>](#security)

    - [<span data-ttu-id="d8966-120">Validación de solicitudes</span><span class="sxs-lookup"><span data-stu-id="d8966-120">Request Validation</span></span>](#validation)
    - [<span data-ttu-id="d8966-121">Autenticación y sesión de formularios sin cookies</span><span class="sxs-lookup"><span data-stu-id="d8966-121">Cookieless Forms Authentication and Session</span></span>](#cookieless)
    - [<span data-ttu-id="d8966-122">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="d8966-122">EnableViewStateMac</span></span>](#viewstatemac)
    - [<span data-ttu-id="d8966-123">Confianza media</span><span class="sxs-lookup"><span data-stu-id="d8966-123">Medium Trust</span></span>](#medium)
    - [<span data-ttu-id="d8966-124">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="d8966-124">&lt;appSettings&gt;</span></span>](#appsettings)
    - [<span data-ttu-id="d8966-125">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="d8966-125">UrlPathEncode</span></span>](#urlpathencode)
- [<span data-ttu-id="d8966-126">Confiabilidad y rendimiento</span><span class="sxs-lookup"><span data-stu-id="d8966-126">Reliability and Performance</span></span>](#performance)

    - [<span data-ttu-id="d8966-127">PreSendRequestHeaders y PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="d8966-127">PreSendRequestHeaders and PreSendRequestContent</span></span>](#presend)
    - [<span data-ttu-id="d8966-128">Eventos de página asincrónica con formularios Web Forms</span><span class="sxs-lookup"><span data-stu-id="d8966-128">Asynchronous Page Events with Web Forms</span></span>](#asyncevents)
    - [<span data-ttu-id="d8966-129">Trabajo de incendio y olvidar</span><span class="sxs-lookup"><span data-stu-id="d8966-129">Fire-and-Forget Work</span></span>](#fire)
    - [<span data-ttu-id="d8966-130">Cuerpo de la entidad de solicitud</span><span class="sxs-lookup"><span data-stu-id="d8966-130">Request Entity Body</span></span>](#requestentity)
    - [<span data-ttu-id="d8966-131">Response. Redirect y Response. end</span><span class="sxs-lookup"><span data-stu-id="d8966-131">Response.Redirect and Response.End</span></span>](#redirect)
    - [<span data-ttu-id="d8966-132">EnableViewState y ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="d8966-132">EnableViewState and ViewStateMode</span></span>](#viewstatemode)
    - [<span data-ttu-id="d8966-133">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="d8966-133">SqlMembershipProvider</span></span>](#sqlprovider)
    - [<span data-ttu-id="d8966-134">Solicitudes de ejecución prolongada (> 110 segundos)</span><span class="sxs-lookup"><span data-stu-id="d8966-134">Long Running Requests (>110 seconds)</span></span>](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a><span data-ttu-id="d8966-135">Cumplimiento de estándares</span><span class="sxs-lookup"><span data-stu-id="d8966-135">Standards Compliance</span></span>

<a id="adapters"></a>

### <a name="control-adapters"></a><span data-ttu-id="d8966-136">Adaptadores de control</span><span class="sxs-lookup"><span data-stu-id="d8966-136">Control adapters</span></span>

<span data-ttu-id="d8966-137">Recomendación: dejar de usar adaptadores de control para la representación adaptable y, en su lugar, usar las consultas multimedia de CSS y HTML compatible con los estándares.</span><span class="sxs-lookup"><span data-stu-id="d8966-137">Recommendation: Stop using control adapters for adaptive rendering, and instead use CSS media queries and standards-compliant HTML.</span></span>

<span data-ttu-id="d8966-138">Los adaptadores de controles se introdujeron en .NET 2,0 para representar el código de presentación que se personalizó para diferentes dispositivos y entornos.</span><span class="sxs-lookup"><span data-stu-id="d8966-138">Controls Adapters were introduced in .NET 2.0 to render presentation code that was customized for different devices and environments.</span></span> <span data-ttu-id="d8966-139">Ahora, esta representación adaptable se puede realizar con CSS y HTML.</span><span class="sxs-lookup"><span data-stu-id="d8966-139">Now, this adaptive rendering can be accomplished with CSS and HTML.</span></span> <span data-ttu-id="d8966-140">Debe dejar de usar los adaptadores de control y convertir los adaptadores existentes en CSS y HTML.</span><span class="sxs-lookup"><span data-stu-id="d8966-140">You should stop using Control Adapters and convert any existing adapters to CSS and HTML.</span></span>

<span data-ttu-id="d8966-141">Para obtener más información, vea [consultas multimedia](http://www.w3.org/TR/css3-mediaqueries/) y [Cómo: agregar páginas móviles a la aplicación web FORMs/MVC de ASP.net](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="d8966-141">For more information, see [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) and [How To: Add Mobile Pages to Your ASP.NET Web Forms / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span></span>

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a><span data-ttu-id="d8966-142">Propiedades de estilo en controles</span><span class="sxs-lookup"><span data-stu-id="d8966-142">Style properties on controls</span></span>

<span data-ttu-id="d8966-143">Recomendación: detenga el establecimiento de valores de estilo en el marcado de control y, en su lugar, establezca valores de formato en hojas de estilos CSS.</span><span class="sxs-lookup"><span data-stu-id="d8966-143">Recommendation: Stop setting style values in the control markup, and instead set formatting values in CSS stylesheets.</span></span>

<span data-ttu-id="d8966-144">Los controles de servidor web contienen docenas de propiedades que se pueden usar para establecer las propiedades de estilo en línea.</span><span class="sxs-lookup"><span data-stu-id="d8966-144">Web server controls contain dozens of properties which can be used to set in-line style properties.</span></span> <span data-ttu-id="d8966-145">Por ejemplo, la propiedad ForeColor establece el color del texto de un control.</span><span class="sxs-lookup"><span data-stu-id="d8966-145">For example, the ForeColor property sets the color of the text for a control.</span></span> <span data-ttu-id="d8966-146">Puede lograr este mismo efecto más eficazmente a través de hojas de estilo CSS.</span><span class="sxs-lookup"><span data-stu-id="d8966-146">You can accomplish this same effect more efficiently through CSS stylesheets.</span></span> <span data-ttu-id="d8966-147">Las hojas de estilos permiten centralizar los valores de estilo y evitar establecer estos valores en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d8966-147">Stylesheets enable you to centralize style values and avoid setting these values throughout your application.</span></span>

<span data-ttu-id="d8966-148">En el ejemplo siguiente se muestra una clase CSS que establece el texto en rojo.</span><span class="sxs-lookup"><span data-stu-id="d8966-148">The following example shows a CSS class the sets text to red.</span></span>

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

<span data-ttu-id="d8966-149">En el ejemplo siguiente se muestra cómo aplicar dinámicamente la clase CSS.</span><span class="sxs-lookup"><span data-stu-id="d8966-149">The next example shows how to dynamically apply the CSS class.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a><span data-ttu-id="d8966-150">Devoluciones de llamada de páginas y controles</span><span class="sxs-lookup"><span data-stu-id="d8966-150">Page and control callbacks</span></span>

<span data-ttu-id="d8966-151">Recomendación: deje de usar devoluciones de llamada de página y de control y, en su lugar, use cualquiera de los siguientes: AJAX, UpdatePanel, métodos de acción de MVC, API Web o Signalr.</span><span class="sxs-lookup"><span data-stu-id="d8966-151">Recommendation: Stop using page and control callbacks, and instead use any of the following: AJAX, UpdatePanel, MVC action methods, Web API, or SignalR.</span></span>

<span data-ttu-id="d8966-152">En versiones anteriores de ASP.NET, los métodos de devolución de llamada de página y control permitían actualizar parte de la página web sin actualizar una página completa.</span><span class="sxs-lookup"><span data-stu-id="d8966-152">In earlier versions of ASP.NET, Page and Control callback methods enabled you to update part of the web page without refreshing an entire page.</span></span> <span data-ttu-id="d8966-153">Ahora puede realizar actualizaciones parciales de página a través de [Ajax](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [API Web](../../../web-api/index.md) o [signalr](../../../signalr/index.md).</span><span class="sxs-lookup"><span data-stu-id="d8966-153">You can now accomplish partial-page updates through [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) or [SignalR](../../../signalr/index.md).</span></span> <span data-ttu-id="d8966-154">Debe dejar de usar los métodos de devolución de llamada porque pueden causar problemas con direcciones URL y enrutamiento descriptivos.</span><span class="sxs-lookup"><span data-stu-id="d8966-154">You should stop using callback methods because they can cause issues with friendly URLs and routing.</span></span> <span data-ttu-id="d8966-155">De forma predeterminada, los controles no habilitan los métodos de devolución de llamada, pero si ha habilitado esta característica en un control, debe deshabilitarla.</span><span class="sxs-lookup"><span data-stu-id="d8966-155">By default, controls do not enable callback methods, but if you enabled this feature in a control, you should disable it.</span></span>

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a><span data-ttu-id="d8966-156">Detección de capacidad del explorador</span><span class="sxs-lookup"><span data-stu-id="d8966-156">Browser capability detection</span></span>

<span data-ttu-id="d8966-157">Recomendación: deje de usar la detección de capacidad del explorador estática y, en su lugar, use la detección de características dinámicas.</span><span class="sxs-lookup"><span data-stu-id="d8966-157">Recommendation: Stop using static browser capability detection, and instead use dynamic feature detection.</span></span>

<span data-ttu-id="d8966-158">En versiones anteriores de ASP.NET, las características admitidas para cada explorador se almacenaban en un archivo XML.</span><span class="sxs-lookup"><span data-stu-id="d8966-158">In earlier versions of ASP.NET, the supported features for each browser were stored in an XML file.</span></span> <span data-ttu-id="d8966-159">La detección de la compatibilidad de características a través de una búsqueda estática no es el mejor enfoque.</span><span class="sxs-lookup"><span data-stu-id="d8966-159">Detecting feature support through a static lookup is not the best approach.</span></span> <span data-ttu-id="d8966-160">Ahora, puede detectar dinámicamente las características admitidas de un explorador mediante el uso de un marco de detección de características, como [modernizr](http://modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="d8966-160">Now, you can dynamically detect a browser's supported features by using a feature detection framework, such as [Modernizr](http://modernizr.com/).</span></span> <span data-ttu-id="d8966-161">La detección de características determina la compatibilidad intentando usar un método o una propiedad y, a continuación, comprobando si el explorador generó el resultado deseado.</span><span class="sxs-lookup"><span data-stu-id="d8966-161">Feature detection determines support by attempting to use a method or property and then checking to see if the browser produced the desired result.</span></span> <span data-ttu-id="d8966-162">De forma predeterminada, Modernizr se incluye en las plantillas de aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="d8966-162">By default, Modernizr is included in the Web application templates.</span></span>

<a id="security"></a>

## <a name="security"></a><span data-ttu-id="d8966-163">Seguridad</span><span class="sxs-lookup"><span data-stu-id="d8966-163">Security</span></span>

<a id="validation"></a>

### <a name="request-validation"></a><span data-ttu-id="d8966-164">Validación de solicitudes</span><span class="sxs-lookup"><span data-stu-id="d8966-164">Request validation</span></span>

<span data-ttu-id="d8966-165">Recomendación: validar la entrada del usuario y codificar la salida de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="d8966-165">Recommendation: Validate user input, and encode output from users.</span></span>

<span data-ttu-id="d8966-166">La validación de solicitudes es una característica de ASP.NET que inspecciona cada solicitud y detiene la solicitud si se encuentra una amenaza percibida.</span><span class="sxs-lookup"><span data-stu-id="d8966-166">Request validation is a feature of ASP.NET that inspects each request and stops the request if a perceived threat is found.</span></span> <span data-ttu-id="d8966-167">No dependa de la validación de solicitudes para proteger su aplicación contra ataques de scripts entre sitios.</span><span class="sxs-lookup"><span data-stu-id="d8966-167">Do not depend on request validation for securing your application against cross-site scripting attacks.</span></span> <span data-ttu-id="d8966-168">En su lugar, valide toda la entrada de los usuarios y codifique la salida.</span><span class="sxs-lookup"><span data-stu-id="d8966-168">Instead, validate all input from users and encode the output.</span></span> <span data-ttu-id="d8966-169">En algunos casos limitados, puede usar expresiones regulares para validar la entrada, pero en casos más complicados debe validar la entrada del usuario mediante el uso de clases de .NET que determinan si el valor coincide con los valores permitidos.</span><span class="sxs-lookup"><span data-stu-id="d8966-169">In some limited cases, you can use regular expressions to validate the input, but in more complicated cases you should validate user input by using .NET classes that determine if the value matches allowed values.</span></span>

<span data-ttu-id="d8966-170">En el ejemplo siguiente se muestra cómo usar un método estático en la clase URI para determinar si el URI proporcionado por un usuario es válido.</span><span class="sxs-lookup"><span data-stu-id="d8966-170">The following example shows how to use a static method in the Uri class to determine whether the Uri provided by a user is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

<span data-ttu-id="d8966-171">Sin embargo, para comprobar suficientemente el URI, debe comprobar también para asegurarse de que especifica `http` o `https`.</span><span class="sxs-lookup"><span data-stu-id="d8966-171">However, to sufficiently verify the Uri, you should also check to make sure it specifies `http` or `https`.</span></span> <span data-ttu-id="d8966-172">En el ejemplo siguiente se usan métodos de instancia para comprobar que el URI es válido.</span><span class="sxs-lookup"><span data-stu-id="d8966-172">The following example uses instance methods to verify that the Uri is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

<span data-ttu-id="d8966-173">Antes de representar la entrada del usuario como HTML o incluir la entrada del usuario en una consulta SQL, codifique los valores para asegurarse de que no se incluye código malintencionado.</span><span class="sxs-lookup"><span data-stu-id="d8966-173">Before rendering user input as HTML or including user input in a SQL query, encode the values to ensure malicious code is not included.</span></span>

<span data-ttu-id="d8966-174">Puede codificar en HTML el valor en el marcado con la sintaxis &lt;%:%&gt;, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="d8966-174">You can HTML encode the value in markup with the &lt;%: %&gt; syntax, as shown below.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

<span data-ttu-id="d8966-175">O bien, en sintaxis Razor, puede codificar HTML con @, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="d8966-175">Or, in Razor syntax, you can HTML encode with @, as shown below.</span></span>

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="d8966-176">En el ejemplo siguiente se muestra cómo codificar en HTML un valor en el código subyacente.</span><span class="sxs-lookup"><span data-stu-id="d8966-176">The next example shows how to HTML encode a value in code-behind.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

<span data-ttu-id="d8966-177">Para codificar un valor de forma segura para los comandos SQL, use parámetros de comando como [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span><span class="sxs-lookup"><span data-stu-id="d8966-177">To safely encode a value for SQL commands, use command parameters such as the [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span></span> <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a><span data-ttu-id="d8966-178">Autenticación y sesión de formularios sin cookies</span><span class="sxs-lookup"><span data-stu-id="d8966-178">Cookieless forms authentication and session</span></span>

<span data-ttu-id="d8966-179">Recomendación: requerir cookies.</span><span class="sxs-lookup"><span data-stu-id="d8966-179">Recommendation: Require cookies.</span></span>

<span data-ttu-id="d8966-180">No es seguro pasar la información de autenticación en la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="d8966-180">Passing authentication information in the query string is not secure.</span></span> <span data-ttu-id="d8966-181">Por lo tanto, se requieren cookies cuando la aplicación incluye autenticación.</span><span class="sxs-lookup"><span data-stu-id="d8966-181">Therefore, require cookies when your application includes authentication.</span></span> <span data-ttu-id="d8966-182">Si la cookie almacena información confidencial, considere la posibilidad de requerir SSL para la cookie.</span><span class="sxs-lookup"><span data-stu-id="d8966-182">If your cookie stores sensitive information, consider requiring SSL for the cookie.</span></span>

<span data-ttu-id="d8966-183">En el ejemplo siguiente se muestra cómo especificar en el archivo Web. config que la autenticación de formularios requiere una cookie que se transmite a través de SSL.</span><span class="sxs-lookup"><span data-stu-id="d8966-183">The following example shows how to specify in the Web.config file that Forms Authentication requires a cookie that is transmitted over SSL.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a><span data-ttu-id="d8966-184">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="d8966-184">EnableViewStateMac</span></span>

<span data-ttu-id="d8966-185">Recomendación: no establezca nunca en false.</span><span class="sxs-lookup"><span data-stu-id="d8966-185">Recommendation: Never set to false.</span></span>

<span data-ttu-id="d8966-186">De forma predeterminada, EnableViewStateMac está establecido en true.</span><span class="sxs-lookup"><span data-stu-id="d8966-186">By default, EnableViewStateMac is set to true.</span></span> <span data-ttu-id="d8966-187">Incluso si la aplicación no usa el estado de vista, no establezca EnableViewStateMac en false.</span><span class="sxs-lookup"><span data-stu-id="d8966-187">Even if your application is not using view state, do not set EnableViewStateMac to false.</span></span> <span data-ttu-id="d8966-188">Si se establece este valor en false, la aplicación será vulnerable a la creación de scripts entre sitios.</span><span class="sxs-lookup"><span data-stu-id="d8966-188">Setting this value to false will make your application vulnerable to cross-site scripting.</span></span>

<span data-ttu-id="d8966-189">A partir de ASP.NET 4.5.2, el tiempo de ejecución exige **EnableViewStateMac = true**.</span><span class="sxs-lookup"><span data-stu-id="d8966-189">Starting with ASP.NET 4.5.2, the runtime enforces **EnableViewStateMac=true**.</span></span> <span data-ttu-id="d8966-190">Incluso si se establece en false, el tiempo de ejecución omite este valor y continúa con el valor establecido en true.</span><span class="sxs-lookup"><span data-stu-id="d8966-190">Even if you set it to false, the runtime ignores this value and proceeds with the value set to true.</span></span> <span data-ttu-id="d8966-191">Para obtener más información, consulte [ASP.net 4.5.2 y EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span><span class="sxs-lookup"><span data-stu-id="d8966-191">For more information, see [ASP.NET 4.5.2 and EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span></span>

<span data-ttu-id="d8966-192">En el ejemplo siguiente se muestra cómo establecer EnableViewStateMac en true.</span><span class="sxs-lookup"><span data-stu-id="d8966-192">The following example shows how to set EnableViewStateMac to true.</span></span> <span data-ttu-id="d8966-193">En realidad, no es necesario establecer este valor en true porque es true de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="d8966-193">You do not need to actually set this value to true because it is true by default.</span></span> <span data-ttu-id="d8966-194">Sin embargo, si se ha establecido en false en cualquier página de la aplicación, debe corregir este valor inmediatamente.</span><span class="sxs-lookup"><span data-stu-id="d8966-194">However, if you have set it to false on any page in your application, you must immediately correct this value.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a><span data-ttu-id="d8966-195">Confianza media</span><span class="sxs-lookup"><span data-stu-id="d8966-195">Medium trust</span></span>

<span data-ttu-id="d8966-196">Recomendación: no dependa de la confianza media (o cualquier otro nivel de confianza) como límite de seguridad.</span><span class="sxs-lookup"><span data-stu-id="d8966-196">Recommendation: Do not depend on Medium Trust (or any other trust level) as a security boundary.</span></span>

<span data-ttu-id="d8966-197">La confianza parcial no protege la aplicación de forma adecuada y no debe usarse.</span><span class="sxs-lookup"><span data-stu-id="d8966-197">Partial trust does not adequately protect your application and should not be used.</span></span> <span data-ttu-id="d8966-198">En su lugar, utilice plena confianza y aísle las aplicaciones que no son de confianza en grupos de aplicaciones independientes.</span><span class="sxs-lookup"><span data-stu-id="d8966-198">Instead, use Full Trust, and isolate untrusted applications in separate application pools.</span></span> <span data-ttu-id="d8966-199">Además, ejecute cada grupo de aplicaciones con una identidad única.</span><span class="sxs-lookup"><span data-stu-id="d8966-199">Also, run each application pool under a unique identity.</span></span> <span data-ttu-id="d8966-200">Para obtener más información, consulte [confianza parcial de ASP.net no garantiza el aislamiento de aplicaciones](https://support.microsoft.com/kb/2698981).</span><span class="sxs-lookup"><span data-stu-id="d8966-200">For more information, see [ASP.NET Partial Trust does not guarantee application isolation](https://support.microsoft.com/kb/2698981).</span></span>

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a><span data-ttu-id="d8966-201">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="d8966-201">&lt;appSettings&gt;</span></span>

<span data-ttu-id="d8966-202">Recomendación: no deshabilite la configuración de seguridad en &lt;elemento&gt; appSettings.</span><span class="sxs-lookup"><span data-stu-id="d8966-202">Recommendation: Do not disable security settings in &lt;appSettings&gt; element.</span></span>

<span data-ttu-id="d8966-203">El elemento appSettings contiene muchos valores que son necesarios para las actualizaciones de seguridad.</span><span class="sxs-lookup"><span data-stu-id="d8966-203">The appSettings element contains many values which are required for security updates.</span></span> <span data-ttu-id="d8966-204">No debe cambiar o deshabilitar estos valores.</span><span class="sxs-lookup"><span data-stu-id="d8966-204">You should not change or disable these values.</span></span> <span data-ttu-id="d8966-205">Si debe deshabilitar estos valores al implementar una actualización, vuelva a habilitar inmediatamente después de completar la implementación.</span><span class="sxs-lookup"><span data-stu-id="d8966-205">If you must disable these values when deploying an update, immediately re-enable after completing deployment.</span></span>

<span data-ttu-id="d8966-206">Para obtener más información, consulte [elemento ASP.net appSettings](https://msdn.microsoft.com/library/hh975440.aspx).</span><span class="sxs-lookup"><span data-stu-id="d8966-206">For details, see [ASP.NET appSettings Element](https://msdn.microsoft.com/library/hh975440.aspx).</span></span>

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a><span data-ttu-id="d8966-207">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="d8966-207">UrlPathEncode</span></span>

<span data-ttu-id="d8966-208">Recomendación: use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) en su lugar.</span><span class="sxs-lookup"><span data-stu-id="d8966-208">Recommendation: Use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) instead.</span></span>

<span data-ttu-id="d8966-209">El método UrlPathEncode se ha agregado al .NET Framework para resolver un problema de compatibilidad de explorador muy específico.</span><span class="sxs-lookup"><span data-stu-id="d8966-209">The UrlPathEncode method was added to the .NET Framework to resolve a very specific browser compatibility problem.</span></span> <span data-ttu-id="d8966-210">No codifica adecuadamente una dirección URL y no protege la aplicación de scripting entre sitios.</span><span class="sxs-lookup"><span data-stu-id="d8966-210">It does not adequately encode a URL, and does not protect your application from cross-site scripting.</span></span> <span data-ttu-id="d8966-211">Nunca debe usarlo en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d8966-211">You should never use it in your application.</span></span> <span data-ttu-id="d8966-212">En su lugar, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span><span class="sxs-lookup"><span data-stu-id="d8966-212">Instead, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span></span>

<span data-ttu-id="d8966-213">En el ejemplo siguiente se muestra cómo pasar una dirección URL codificada como un parámetro de cadena de consulta para un control de hipervínculo.</span><span class="sxs-lookup"><span data-stu-id="d8966-213">The following example shows how to pass an encoded URL as a query string parameter for a hyperlink control.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a><span data-ttu-id="d8966-214">Confiabilidad y rendimiento</span><span class="sxs-lookup"><span data-stu-id="d8966-214">Reliability and performance</span></span>

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a><span data-ttu-id="d8966-215">PreSendRequestHeaders y PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="d8966-215">PreSendRequestHeaders and PreSendRequestContent</span></span>

<span data-ttu-id="d8966-216">Recomendación: no use estos eventos con módulos administrados.</span><span class="sxs-lookup"><span data-stu-id="d8966-216">Recommendation: Do not use these events with managed modules.</span></span> <span data-ttu-id="d8966-217">En su lugar, escriba un módulo de IIS nativo para realizar la tarea necesaria.</span><span class="sxs-lookup"><span data-stu-id="d8966-217">Instead, write a native IIS module to perform the required task.</span></span> <span data-ttu-id="d8966-218">Vea [crear módulos http de código nativo](https://msdn.microsoft.com/library/ms693629.aspx).</span><span class="sxs-lookup"><span data-stu-id="d8966-218">See [Creating Native-Code HTTP Modules](https://msdn.microsoft.com/library/ms693629.aspx).</span></span>

<span data-ttu-id="d8966-219">Puede usar los eventos [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) y [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) con los módulos nativos de IIS.</span><span class="sxs-lookup"><span data-stu-id="d8966-219">You can use the [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) and [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) events with native IIS modules.</span></span>
> [!WARNING]
> <span data-ttu-id="d8966-220">No utilice `PreSendRequestHeaders` y `PreSendRequestContent` con módulos administrados que implementan `IHttpModule`.</span><span class="sxs-lookup"><span data-stu-id="d8966-220">Do not use `PreSendRequestHeaders` and `PreSendRequestContent` with managed modules that implement `IHttpModule`.</span></span> <span data-ttu-id="d8966-221">Al establecer estas propiedades, pueden producirse problemas con las solicitudes asincrónicas.</span><span class="sxs-lookup"><span data-stu-id="d8966-221">Setting these properties can cause issues with asynchronous requests.</span></span> <span data-ttu-id="d8966-222">La combinación de enrutamiento solicitado de aplicación (ARR) y WebSockets podría provocar excepciones de infracción de acceso que pueden hacer que w3wp se bloquee.</span><span class="sxs-lookup"><span data-stu-id="d8966-222">The combination of Application Requested Routing (ARR) and websockets might lead to access violation exceptions that can cause w3wp to crash.</span></span> <span data-ttu-id="d8966-223">Por ejemplo, iiscore! W3_CONTEXT_BASE:: GetIsLastNotification + 68 en iiscore. dll ha provocado una excepción de infracción de acceso (0xC0000005).</span><span class="sxs-lookup"><span data-stu-id="d8966-223">For example, iiscore!W3_CONTEXT_BASE::GetIsLastNotification+68 in iiscore.dll has caused an access violation exception (0xC0000005).</span></span>

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a><span data-ttu-id="d8966-224">Eventos de página asincrónica con formularios Web Forms</span><span class="sxs-lookup"><span data-stu-id="d8966-224">Asynchronous page events with web forms</span></span>

<span data-ttu-id="d8966-225">Recomendación: en formularios Web Forms, evite escribir métodos void void para eventos del ciclo de vida de la página y, en su lugar, use [Page. RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) para el código asincrónico.</span><span class="sxs-lookup"><span data-stu-id="d8966-225">Recommendation: In Web Forms, avoid writing async void methods for Page lifecycle events, and instead use [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) for asynchronous code.</span></span>

<span data-ttu-id="d8966-226">Cuando se marca un evento de página con **Async** y **void**, no se puede determinar cuándo ha finalizado el código asincrónico.</span><span class="sxs-lookup"><span data-stu-id="d8966-226">When you mark a page event with **async** and **void**, you cannot determine when the asynchronous code has finished.</span></span> <span data-ttu-id="d8966-227">En su lugar, use Page. RegisterAsyncTask para ejecutar el código asincrónico de una forma que le permita realizar un seguimiento de su finalización.</span><span class="sxs-lookup"><span data-stu-id="d8966-227">Instead, use Page.RegisterAsyncTask to run the asynchronous code in a way that enables you to track its completion.</span></span>

<span data-ttu-id="d8966-228">En el ejemplo siguiente se muestra un controlador de clic de botón que contiene código asincrónico.</span><span class="sxs-lookup"><span data-stu-id="d8966-228">The following example shows a button click handler that contains asynchronous code.</span></span> <span data-ttu-id="d8966-229">En este ejemplo se incluye la lectura asincrónica de un valor de cadena, que se proporciona solo como un ejemplo simplificado de una tarea asincrónica y no como práctica recomendada.</span><span class="sxs-lookup"><span data-stu-id="d8966-229">This example includes reading a string value asynchronously, which is provided only as a simplified example of an asynchronous task and not as a recommended practice.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

<span data-ttu-id="d8966-230">Si usa tareas asincrónicas, establezca la plataforma de destino de tiempo de ejecución http en 4,5 (o posterior) en el archivo Web. config.</span><span class="sxs-lookup"><span data-stu-id="d8966-230">If you are using asynchronous Tasks, set the Http runtime target framework to 4.5 (or later) in the Web.config file.</span></span> <span data-ttu-id="d8966-231">Al establecer la versión de .NET Framework de destino en 4,5, se activa el nuevo contexto de sincronización que se agregó en .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="d8966-231">Setting the target framework to 4.5 turns on the new synchronization context that was added in .NET 4.5.</span></span> <span data-ttu-id="d8966-232">Este valor se establece de forma predeterminada en los nuevos proyectos de Visual Studio, pero no se establece si se trabaja con un proyecto existente.</span><span class="sxs-lookup"><span data-stu-id="d8966-232">This value is set by default in new projects in Visual Studio, but is not be set if you are working with an existing project.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a><span data-ttu-id="d8966-233">Trabajo de incendio y olvidar</span><span class="sxs-lookup"><span data-stu-id="d8966-233">Fire-and-forget work</span></span>

<span data-ttu-id="d8966-234">Recomendación: al controlar una solicitud en ASP.NET, evite iniciar el trabajo de desencadenamiento y olvidar (llamando al método ThreadPool. QueueUserWorkItem o creando un temporizador que llame repetidamente a un delegado).</span><span class="sxs-lookup"><span data-stu-id="d8966-234">Recommendation: When handling a request within ASP.NET, avoid launching fire-and-forget work (such calling the ThreadPool.QueueUserWorkItem method or creating a timer that repeatedly calls a delegate).</span></span>

<span data-ttu-id="d8966-235">Si la aplicación tiene un trabajo de desencadenamiento y olvidar que se ejecuta dentro de ASP.NET, la aplicación puede dejar de estar sincronizada. En cualquier momento, el dominio de la aplicación se puede destruir, lo que significa que el proceso en curso puede dejar de coincidir con el estado actual de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d8966-235">If your application has fire-and-forget work that runs within ASP.NET, your application can get out of sync. At any time, the app domain can be destroyed which means your ongoing process may no longer match the current state of the application.</span></span>

<span data-ttu-id="d8966-236">Debe trasladar este tipo de trabajo fuera de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d8966-236">You should move this type of work outside of ASP.NET.</span></span> <span data-ttu-id="d8966-237">Puede usar trabajos Web, un servicio de Windows o un rol de trabajo en Azure para realizar el trabajo en curso y ejecutar ese código desde otro proceso.</span><span class="sxs-lookup"><span data-stu-id="d8966-237">You can use a Web Jobs, Windows Service or a Worker role in Azure to perform ongoing work, and run that code from another process.</span></span>

<span data-ttu-id="d8966-238">Si debe realizar este trabajo en ASP.NET, puede Agregar el paquete Nuget llamado [Webbackgrounder](http://www.nuget.org/packages/webbackgrounder) para ejecutar el código.</span><span class="sxs-lookup"><span data-stu-id="d8966-238">If you must perform this work within ASP.NET, you can add the Nuget package called [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) to run the code.</span></span>

<a id="requestentity"></a>

### <a name="request-entity-body"></a><span data-ttu-id="d8966-239">Cuerpo de la entidad de solicitud</span><span class="sxs-lookup"><span data-stu-id="d8966-239">Request entity body</span></span>

<span data-ttu-id="d8966-240">Recomendación: Evite leer request. Form o request. InputStream antes del evento Execute del controlador.</span><span class="sxs-lookup"><span data-stu-id="d8966-240">Recommendation: Avoid reading Request.Form or Request.InputStream before the handler's execute event.</span></span>

<span data-ttu-id="d8966-241">Lo más antiguo que debe leer de request. Form o request. InputStream es durante el evento Execute del controlador.</span><span class="sxs-lookup"><span data-stu-id="d8966-241">The earliest you should read from Request.Form or Request.InputStream is during the handler's execute event.</span></span> <span data-ttu-id="d8966-242">En MVC, el controlador es el controlador y el evento Execute es cuando se ejecuta el método de acción.</span><span class="sxs-lookup"><span data-stu-id="d8966-242">In MVC, the Controller is the handler and the execute event is when the action method runs.</span></span> <span data-ttu-id="d8966-243">En los formularios Web Forms, la página es el controlador y el evento Execute es cuando se desencadena el evento Page. init.</span><span class="sxs-lookup"><span data-stu-id="d8966-243">In Web Forms, the Page is the handler and the execute event is when the Page.Init event fires.</span></span> <span data-ttu-id="d8966-244">Si lee el cuerpo de la entidad de solicitud antes que el evento Execute, interferirá con el procesamiento de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="d8966-244">If you read the request entity body earlier than the execute event, you interfere with the processing of the request.</span></span>

<span data-ttu-id="d8966-245">Si necesita leer el cuerpo de la entidad de solicitud antes del evento Execute, use [request. GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) o [request. GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span><span class="sxs-lookup"><span data-stu-id="d8966-245">If you need to read the request entity body before the execute event, use either [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) or [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span></span> <span data-ttu-id="d8966-246">Cuando se usa GetBufferlessInputStream, se obtiene el flujo sin formato de la solicitud y se asume la responsabilidad de procesar la solicitud completa.</span><span class="sxs-lookup"><span data-stu-id="d8966-246">When you use GetBufferlessInputStream, you get the raw stream from the request, and assume responsibility for processing the entire request.</span></span> <span data-ttu-id="d8966-247">Después de llamar a GetBufferlessInputStream, Request. Form y request. InputStream no están disponibles porque no se han rellenado con ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d8966-247">After calling GetBufferlessInputStream, Request.Form and Request.InputStream are not available because they have not been populated by ASP.NET.</span></span> <span data-ttu-id="d8966-248">Cuando se usa GetBufferedInputStream, se obtiene una copia de la secuencia de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="d8966-248">When you use GetBufferedInputStream, you get a copy of the stream from the request.</span></span> <span data-ttu-id="d8966-249">Request. Form y request. InputStream siguen estando disponibles posteriormente en la solicitud porque ASP.NET rellena la otra copia.</span><span class="sxs-lookup"><span data-stu-id="d8966-249">Request.Form and Request.InputStream are still available later in the request because ASP.NET populates the other copy.</span></span>

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a><span data-ttu-id="d8966-250">Response. Redirect y Response. end</span><span class="sxs-lookup"><span data-stu-id="d8966-250">Response.Redirect and Response.End</span></span>

<span data-ttu-id="d8966-251">Recomendación: tenga en cuenta las diferencias en la forma en que se controla el subproceso después de llamar a [Response. Redirect (cadena)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span><span class="sxs-lookup"><span data-stu-id="d8966-251">Recommendation: Be aware of differences in how thread is handled after calling [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span></span>

<span data-ttu-id="d8966-252">El método [Response. Redirect (String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) llama al método Response. end.</span><span class="sxs-lookup"><span data-stu-id="d8966-252">The [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) method calls the Response.End method.</span></span> <span data-ttu-id="d8966-253">En un proceso sincrónico, al llamar a request. Redirect, el subproceso actual se anula inmediatamente.</span><span class="sxs-lookup"><span data-stu-id="d8966-253">In a synchronous process, calling Request.Redirect causes the current thread to immediately abort.</span></span> <span data-ttu-id="d8966-254">Sin embargo, en un proceso asincrónico, al llamar a Response. Redirect no se anula el subproceso actual, por lo que la ejecución del código continúa para la solicitud.</span><span class="sxs-lookup"><span data-stu-id="d8966-254">However, in an asynchronous process, calling Response.Redirect does not abort the current thread, so code execution continues for the request.</span></span> <span data-ttu-id="d8966-255">En un proceso asincrónico, debe devolver la tarea desde el método para detener la ejecución del código.</span><span class="sxs-lookup"><span data-stu-id="d8966-255">In an asynchronous process, you must return the Task from the method to stop the code execution.</span></span>

<span data-ttu-id="d8966-256">En un proyecto de MVC, no debe llamar a Response. Redirect.</span><span class="sxs-lookup"><span data-stu-id="d8966-256">In an MVC project, you should not call Response.Redirect.</span></span> <span data-ttu-id="d8966-257">En su lugar, devuelva un RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="d8966-257">Instead, return a RedirectResult.</span></span>

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a><span data-ttu-id="d8966-258">EnableViewState y ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="d8966-258">EnableViewState and ViewStateMode</span></span>

<span data-ttu-id="d8966-259">Recomendación: Use ViewStateMode, en lugar de EnableViewState, para proporcionar un control granular sobre qué controles usan el estado de vista.</span><span class="sxs-lookup"><span data-stu-id="d8966-259">Recommendation: Use ViewStateMode, instead of EnableViewState, to provide granular control over which controls use view state.</span></span>

<span data-ttu-id="d8966-260">Cuando se establece EnableViewState en false en la Directiva de página, el estado de vista está deshabilitado para todos los controles de la página y no se puede habilitar.</span><span class="sxs-lookup"><span data-stu-id="d8966-260">When you set EnableViewState to false in the Page directive, view state is disabled for all controls within the page and cannot be enabled.</span></span> <span data-ttu-id="d8966-261">Si desea habilitar el estado de vista solo para determinados controles de la página, establezca ViewStateMode en deshabilitado para la página.</span><span class="sxs-lookup"><span data-stu-id="d8966-261">If you want to enable view state for only certain controls in your page, set ViewStateMode to Disabled for the Page.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

<span data-ttu-id="d8966-262">A continuación, establezca ViewStateMode en habilitado solo en los controles que realmente necesiten el estado de vista.</span><span class="sxs-lookup"><span data-stu-id="d8966-262">Then, set ViewStateMode to Enabled on only the controls that actually need view state.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

<span data-ttu-id="d8966-263">Al habilitar el estado de vista solo para los controles que lo necesiten, puede reducir el tamaño del estado de vista de las páginas Web.</span><span class="sxs-lookup"><span data-stu-id="d8966-263">By enabling view state for only the controls that need it, you can shrink the size of the view state for your web pages.</span></span>

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a><span data-ttu-id="d8966-264">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="d8966-264">SqlMembershipProvider</span></span>

<span data-ttu-id="d8966-265">Recomendación: Use Proveedores universales.</span><span class="sxs-lookup"><span data-stu-id="d8966-265">Recommendation: Use Universal Providers.</span></span>

<span data-ttu-id="d8966-266">En las plantillas de proyecto actuales, SqlMembershipProvider se ha reemplazado por [proveedores universales de ASP.net](http://www.nuget.org/packages/Microsoft.AspNet.Providers), que está disponible como un paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="d8966-266">In the current project templates, SqlMembershipProvider has been replaced by [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), which is available as a NuGet package.</span></span> <span data-ttu-id="d8966-267">Si usa SqlMembershipProvider en un proyecto que se compiló con una versión anterior de las plantillas, debe cambiar a Proveedores universales.</span><span class="sxs-lookup"><span data-stu-id="d8966-267">If you are using SqlMembershipProvider in a project that was built with an earlier version of the templates, you should switch to Universal Providers.</span></span> <span data-ttu-id="d8966-268">El Proveedores universales trabajar con todas las bases de datos que son compatibles con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d8966-268">The Universal Providers work with all databases that are supported by Entity Framework.</span></span>

<span data-ttu-id="d8966-269">Para obtener más información, consulte [introducing proveedores universales de ASP.net](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span><span class="sxs-lookup"><span data-stu-id="d8966-269">For more information, see [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span></span>

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a><span data-ttu-id="d8966-270">Solicitudes de ejecución prolongada (> 110 segundos)</span><span class="sxs-lookup"><span data-stu-id="d8966-270">Long-running requests (>110 seconds)</span></span>

<span data-ttu-id="d8966-271">Recomendación: use [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) o [signalr](../../../signalr/index.md) para clientes conectados y use operaciones de e/s asincrónicas.</span><span class="sxs-lookup"><span data-stu-id="d8966-271">Recommendation: Use [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) or [SignalR](../../../signalr/index.md) for connected clients, and use asynchronous I/O operations.</span></span>

<span data-ttu-id="d8966-272">Las solicitudes de ejecución prolongada pueden producir resultados imprevisibles y un rendimiento deficiente en la aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="d8966-272">Long-running requests can cause unpredictable results and poor performance in your web application.</span></span> <span data-ttu-id="d8966-273">La configuración de tiempo de espera predeterminada para una solicitud es de 110 segundos.</span><span class="sxs-lookup"><span data-stu-id="d8966-273">The default timeout setting for a request is 110 seconds.</span></span> <span data-ttu-id="d8966-274">Si usa el estado de sesión con una solicitud de ejecución prolongada, ASP.NET liberará el bloqueo en el objeto de sesión después de 110 segundos.</span><span class="sxs-lookup"><span data-stu-id="d8966-274">If you are using session state with a long-running request, ASP.NET will release the lock on the Session object after 110 seconds.</span></span> <span data-ttu-id="d8966-275">Sin embargo, la aplicación puede estar en medio de una operación en el objeto de sesión cuando se libera el bloqueo y la operación podría no completarse correctamente.</span><span class="sxs-lookup"><span data-stu-id="d8966-275">However, your application might be in the middle of an operation on the Session object when the lock is released, and the operation might not complete successfully.</span></span> <span data-ttu-id="d8966-276">Si se bloquea una segunda solicitud del usuario mientras se ejecuta la primera solicitud, la segunda solicitud podría tener acceso al objeto de sesión en un estado incoherente.</span><span class="sxs-lookup"><span data-stu-id="d8966-276">If a second request from the user is blocked while the first request is running, the second request might access the Session object in an inconsistent state.</span></span>

<span data-ttu-id="d8966-277">Si la aplicación incluye operaciones de e/s de bloqueo (o sincrónicas), la aplicación no responderá.</span><span class="sxs-lookup"><span data-stu-id="d8966-277">If your application includes blocking (or synchronous) I/O operations, the application will be unresponsive.</span></span>

<span data-ttu-id="d8966-278">Para mejorar el rendimiento, use las operaciones de e/s asincrónicas en el .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d8966-278">To improve performance, use the asynchronous I/O operations in the .NET Framework.</span></span> <span data-ttu-id="d8966-279">Además, utilice WebSockets o Signalr para conectar clientes al servidor.</span><span class="sxs-lookup"><span data-stu-id="d8966-279">Also, use WebSockets or SignalR for connecting clients to the server.</span></span> <span data-ttu-id="d8966-280">Estas características están diseñadas para controlar de forma eficaz las solicitudes de ejecución prolongada.</span><span class="sxs-lookup"><span data-stu-id="d8966-280">These features are designed to efficiently handle long-running requests.</span></span>
