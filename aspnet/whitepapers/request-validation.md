---
uid: whitepapers/request-validation
title: 'Validación de solicitudes: prevención de ataques de scripts | Microsoft Docs'
author: rick-anderson
description: En este documento se describe la característica de validación de solicitudes de ASP.NET en la que, de forma predeterminada, se evita que la aplicación procese el envío de contenido HTML sin codificar...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 807cccd6fe1acdd6359b014387abd3878840d4cd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78520585"
---
# <a name="request-validation---preventing-script-attacks"></a><span data-ttu-id="a897b-103">Validación de solicitudes - Prevenir ataques de scripts</span><span class="sxs-lookup"><span data-stu-id="a897b-103">Request Validation - Preventing Script Attacks</span></span>

> <span data-ttu-id="a897b-104">En este documento se describe la característica de validación de solicitudes de ASP.NET en la que, de forma predeterminada, se evita que la aplicación procese el contenido HTML sin codificar que se envía al servidor.</span><span class="sxs-lookup"><span data-stu-id="a897b-104">This paper describes the request validation feature of ASP.NET where, by default, the application is prevented from processing unencoded HTML content submitted to the server.</span></span> <span data-ttu-id="a897b-105">Esta característica de validación de solicitudes se puede deshabilitar cuando la aplicación se ha diseñado para procesar datos HTML de forma segura.</span><span class="sxs-lookup"><span data-stu-id="a897b-105">This request validation feature can be disabled when the application has been designed to safely process HTML data.</span></span>
> 
> <span data-ttu-id="a897b-106">Se aplica a ASP.NET 1,1 y ASP.NET 2,0.</span><span class="sxs-lookup"><span data-stu-id="a897b-106">Applies to ASP.NET 1.1 and ASP.NET 2.0.</span></span>

<span data-ttu-id="a897b-107">La validación de solicitudes, una característica de ASP.NET desde la versión 1.1, impide que el servidor acepte contenido con HTML sin codificar.</span><span class="sxs-lookup"><span data-stu-id="a897b-107">Request validation, a feature of ASP.NET since version 1.1, prevents the server from accepting content containing un-encoded HTML.</span></span> <span data-ttu-id="a897b-108">Esta característica está diseñada para ayudar a evitar algunos ataques de inserción de script mediante los cuales el código de script del cliente o HTML puede enviarse sin que se sepa a un servidor, almacenarse y presentarse a otros usuarios.</span><span class="sxs-lookup"><span data-stu-id="a897b-108">This feature is designed to help prevent some script-injection attacks whereby client script code or HTML can be unknowingly submitted to a server, stored, and then presented to other users.</span></span> <span data-ttu-id="a897b-109">Se recomienda encarecidamente validar todos los datos de entrada y codificar el código HTML cuando corresponda.</span><span class="sxs-lookup"><span data-stu-id="a897b-109">We still strongly recommend that you validate all input data and HTML encode it when appropriate.</span></span>

<span data-ttu-id="a897b-110">Por ejemplo, crea una página web que solicita la dirección de correo electrónico de un usuario y, a continuación, almacena esa dirección de correo electrónico en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="a897b-110">For example, you create a Web page that requests a user's email address and then stores that email address in a database.</span></span> <span data-ttu-id="a897b-111">Si el usuario escribe &lt;SCRIPT&gt;alerta ("Hello from script")&lt;/SCRIPT&gt; en lugar de una dirección de correo electrónico válida, cuando se presenten los datos, se puede ejecutar este script si el contenido no se ha codificado correctamente.</span><span class="sxs-lookup"><span data-stu-id="a897b-111">If the user enters &lt;SCRIPT&gt;alert("hello from script")&lt;/SCRIPT&gt; instead of a valid email address, when that data is presented, this script can be executed if the content was not properly encoded.</span></span> <span data-ttu-id="a897b-112">La característica de validación de solicitudes de ASP.NET impide que esto suceda.</span><span class="sxs-lookup"><span data-stu-id="a897b-112">The request validation feature of ASP.NET prevents this from happening.</span></span>

## <a name="why-this-feature-is-useful"></a><span data-ttu-id="a897b-113">¿Por qué esta característica es útil?</span><span class="sxs-lookup"><span data-stu-id="a897b-113">Why this feature is useful</span></span>

<span data-ttu-id="a897b-114">Muchos sitios no saben que están abiertos a ataques de inyección de script simple.</span><span class="sxs-lookup"><span data-stu-id="a897b-114">Many sites are not aware that they are open to simple script injection attacks.</span></span> <span data-ttu-id="a897b-115">Si el propósito de estos ataques es desorientar el sitio al mostrar HTML o ejecutar el script de cliente para redirigir al usuario al sitio de un hacker, los ataques de inyección de script son un problema con el que los desarrolladores web deben competir.</span><span class="sxs-lookup"><span data-stu-id="a897b-115">Whether the purpose of these attacks is to deface the site by displaying HTML, or to potentially execute client script to redirect the user to a hacker's site, script injection attacks are a problem that Web developers must contend with.</span></span>

<span data-ttu-id="a897b-116">Los ataques por inyección de script son un problema de todos los desarrolladores web, ya estén usando ASP.NET, ASP u otras tecnologías de desarrollo web.</span><span class="sxs-lookup"><span data-stu-id="a897b-116">Script injection attacks are a concern of all web developers, whether they are using ASP.NET, ASP, or other web development technologies.</span></span>

<span data-ttu-id="a897b-117">La característica de validación de solicitudes de ASP.NET impide proactivamente estos ataques, ya que no permite que el servidor procese el contenido HTML sin codificar a menos que el desarrollador decida permitir dicho contenido.</span><span class="sxs-lookup"><span data-stu-id="a897b-117">The ASP.NET request validation feature proactively prevents these attacks by not allowing unencoded HTML content to be processed by the server unless the developer decides to allow that content.</span></span>

## <a name="what-to-expect-error-page"></a><span data-ttu-id="a897b-118">Qué esperar: Página de error</span><span class="sxs-lookup"><span data-stu-id="a897b-118">What to expect: Error Page</span></span>

<span data-ttu-id="a897b-119">En la captura de pantalla siguiente se muestra un código de ejemplo de ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="a897b-119">The screen shot below shows some sample ASP.NET code:</span></span>

![](request-validation/_static/image1.png)

<span data-ttu-id="a897b-120">La ejecución de este código da como resultado una página simple que le permite escribir texto en el cuadro de texto, hacer clic en el botón y mostrar el texto en el control etiqueta:</span><span class="sxs-lookup"><span data-stu-id="a897b-120">Running this code results in a simple page that allows you to enter some text in the textbox, click the button, and display the text in the label control:</span></span>

![](request-validation/_static/image2.png)

<span data-ttu-id="a897b-121">Sin embargo, con JavaScript, como `<script>alert("hello!")</script>` que se va a escribir y enviar, se obtendría una excepción:</span><span class="sxs-lookup"><span data-stu-id="a897b-121">However, were JavaScript, such as `<script>alert("hello!")</script>` to be entered and submitted we would get an exception:</span></span>

![](request-validation/_static/image3.png)

<span data-ttu-id="a897b-122">El mensaje de error indica que se detectó un valor de "solicitud potencialmente peligrosa. valor de formulario" y proporciona más detalles en la descripción en lo que se produjo exactamente y cómo cambiar el comportamiento.</span><span class="sxs-lookup"><span data-stu-id="a897b-122">The error message states that a 'potentially dangerous Request.Form value was detected' and provides more details in the description as to exactly what occurred and how to change the behavior.</span></span> <span data-ttu-id="a897b-123">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="a897b-123">For example:</span></span>

<span data-ttu-id="a897b-124">La validación de solicitudes ha detectado un valor de entrada de cliente potencialmente peligroso y se ha anulado el procesamiento de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="a897b-124">Request validation has detected a potentially dangerous client input value, and processing of the request has been aborted.</span></span> <span data-ttu-id="a897b-125">Este valor puede indicar un intento de poner en peligro la seguridad de la aplicación, como un ataque de scripts entre sitios.</span><span class="sxs-lookup"><span data-stu-id="a897b-125">This value may indicate an attempt to compromise the security of your application, such as a cross-site scripting attack.</span></span> <span data-ttu-id="a897b-126">Puede deshabilitar la validación de solicitudes estableciendo `validateRequest=false` en la Directiva de página o en la sección de configuración.</span><span class="sxs-lookup"><span data-stu-id="a897b-126">You can disable request validation by setting `validateRequest=false` in the Page directive or in the configuration section.</span></span> <span data-ttu-id="a897b-127">Sin embargo, se recomienda encarecidamente que la aplicación Compruebe explícitamente todas las entradas en este caso.</span><span class="sxs-lookup"><span data-stu-id="a897b-127">However, it is strongly recommended that your application explicitly check all inputs in this case.</span></span>

## <a name="disabling-request-validation-on-a-page"></a><span data-ttu-id="a897b-128">Deshabilitar la validación de solicitudes en una página</span><span class="sxs-lookup"><span data-stu-id="a897b-128">Disabling request validation on a page</span></span>

<span data-ttu-id="a897b-129">Para deshabilitar la validación de solicitudes en una página, debe establecer el atributo `validateRequest` de la Directiva de página en `false`:</span><span class="sxs-lookup"><span data-stu-id="a897b-129">To disable request validation on a page you must set the `validateRequest` attribute of the Page directive to `false`:</span></span>

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> <span data-ttu-id="a897b-130">Cuando la validación de solicitudes está deshabilitada, el contenido se puede enviar a una página. es responsabilidad del desarrollador de páginas asegurarse de que el contenido se codifica o procesa correctamente.</span><span class="sxs-lookup"><span data-stu-id="a897b-130">When request validation is disabled, content can be submitted to a page; it is the responsibility of the page developer to ensure that content is properly encoded or processed.</span></span>

## <a name="disabling-request-validation-for-your-application"></a><span data-ttu-id="a897b-131">Deshabilitación de la validación de solicitudes para la aplicación</span><span class="sxs-lookup"><span data-stu-id="a897b-131">Disabling request validation for your application</span></span>

<span data-ttu-id="a897b-132">Para deshabilitar la validación de solicitudes de la aplicación, debe modificar o crear un archivo Web. config para la aplicación y establecer el atributo validateRequest de la sección `<pages />` en `false`:</span><span class="sxs-lookup"><span data-stu-id="a897b-132">To disable request validation for your application, you must modify or create a Web.config file for your application and set the validateRequest attribute of the `<pages />` section to `false`:</span></span>

[!code-xml[Main](request-validation/samples/sample2.xml)]

<span data-ttu-id="a897b-133">Si desea deshabilitar la validación de solicitudes para todas las aplicaciones del servidor, puede realizar esta modificación en el archivo Machine. config.</span><span class="sxs-lookup"><span data-stu-id="a897b-133">If you wish to disable request validation for all applications on your server, you can make this modification to your Machine.config file.</span></span>

> [!CAUTION]
> <span data-ttu-id="a897b-134">Cuando la validación de solicitudes está deshabilitada, el contenido se puede enviar a la aplicación; es responsabilidad del desarrollador de aplicaciones asegurarse de que el contenido se codifica o procesa correctamente.</span><span class="sxs-lookup"><span data-stu-id="a897b-134">When request validation is disabled, content can be submitted to your application; it is the responsibility of the application developer to ensure that content is properly encoded or processed.</span></span>

<span data-ttu-id="a897b-135">El código siguiente se modifica para desactivar la validación de solicitudes:</span><span class="sxs-lookup"><span data-stu-id="a897b-135">The code below is modified to turn off request validation:</span></span>

![](request-validation/_static/image4.png)

<span data-ttu-id="a897b-136">Ahora, si se especificó el siguiente código JavaScript en el cuadro de texto `<script>alert("hello!")</script>` el resultado sería:</span><span class="sxs-lookup"><span data-stu-id="a897b-136">Now if the following JavaScript was entered into the textbox `<script>alert("hello!")</script>` the result would be:</span></span>

![](request-validation/_static/image5.png)

<span data-ttu-id="a897b-137">Para evitar que esto suceda, con la validación de solicitudes desactivada, es necesario codificar en HTML el contenido.</span><span class="sxs-lookup"><span data-stu-id="a897b-137">To prevent this from happening, with request validation turned off, we need to HTML encode the content.</span></span>

## <a name="how-to-html-encode-content"></a><span data-ttu-id="a897b-138">Cómo codificar contenido en HTML</span><span class="sxs-lookup"><span data-stu-id="a897b-138">How to HTML encode content</span></span>

<span data-ttu-id="a897b-139">Si ha deshabilitado la validación de solicitudes, se recomienda codificar en HTML el contenido que se almacenará para su uso futuro.</span><span class="sxs-lookup"><span data-stu-id="a897b-139">If you have disabled request validation, it is good practice to HTML-encode content that will be stored for future use.</span></span> <span data-ttu-id="a897b-140">La codificación HTML reemplazará automáticamente cualquier '&lt;' o '&gt;' (junto con otros símbolos) por su representación codificada en HTML correspondiente.</span><span class="sxs-lookup"><span data-stu-id="a897b-140">HTML encoding will automatically replace any ‘&lt;' or ‘&gt;' (together with several other symbols) with their corresponding HTML encoded representation.</span></span> <span data-ttu-id="a897b-141">Por ejemplo, '&lt;' se reemplaza por '&amp;lt; ' y '&gt;' se reemplaza por '&amp;gt; '.</span><span class="sxs-lookup"><span data-stu-id="a897b-141">For example, ‘&lt;' is replaced by ‘&amp;lt;' and ‘&gt;' is replaced by ‘&amp;gt;'.</span></span> <span data-ttu-id="a897b-142">Los exploradores usan estos códigos especiales para mostrar el '&lt;' o '&gt;' en el explorador.</span><span class="sxs-lookup"><span data-stu-id="a897b-142">Browsers use these special codes to display the ‘&lt;' or ‘&gt;' in the browser.</span></span>

<span data-ttu-id="a897b-143">El contenido se puede codificar en HTML con facilidad en el servidor mediante la API de `Server.HtmlEncode(string)`.</span><span class="sxs-lookup"><span data-stu-id="a897b-143">Content can be easily HTML-encoded on the server using the `Server.HtmlEncode(string)` API.</span></span> <span data-ttu-id="a897b-144">El contenido también se puede descodificar con facilidad HTML, es decir, volver al código HTML estándar con el método `Server.HtmlDecode(string)`.</span><span class="sxs-lookup"><span data-stu-id="a897b-144">Content can also be easily HTML-decoded, that is, reverted back to standard HTML using the `Server.HtmlDecode(string)` method.</span></span>

![](request-validation/_static/image6.png)

<span data-ttu-id="a897b-145">Resultado de:</span><span class="sxs-lookup"><span data-stu-id="a897b-145">Resulting in:</span></span>

![](request-validation/_static/image7.png)
