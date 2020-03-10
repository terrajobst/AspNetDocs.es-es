---
uid: web-api/overview/advanced/http-cookies
title: Cookies HTTP en ASP.NET Web API ASP.NET 4. x
author: MikeWasson
description: Describe cómo enviar y recibir cookies HTTP en Web API para ASP.NET 4. x.
ms.author: riande
ms.date: 09/17/2012
ms.custom: seoapril2019
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 8ca26ff6776daa13bc4f8b06c2eba61afcfefba2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449317"
---
# <a name="http-cookies-in-aspnet-web-api"></a><span data-ttu-id="c68d7-103">Cookies HTTP en ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="c68d7-103">HTTP Cookies in ASP.NET Web API</span></span>

<span data-ttu-id="c68d7-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c68d7-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="c68d7-105">En este tema se describe cómo enviar y recibir cookies HTTP en Web API.</span><span class="sxs-lookup"><span data-stu-id="c68d7-105">This topic describes how to send and receive HTTP cookies in Web API.</span></span>

## <a name="background-on-http-cookies"></a><span data-ttu-id="c68d7-106">Información general sobre cookies HTTP</span><span class="sxs-lookup"><span data-stu-id="c68d7-106">Background on HTTP Cookies</span></span>

<span data-ttu-id="c68d7-107">En esta sección se proporciona una breve descripción de cómo se implementan las cookies en el nivel HTTP.</span><span class="sxs-lookup"><span data-stu-id="c68d7-107">This section gives a brief overview of how cookies are implemented at the HTTP level.</span></span> <span data-ttu-id="c68d7-108">Para obtener más información, consulte [RFC 6265](http://tools.ietf.org/html/rfc6265).</span><span class="sxs-lookup"><span data-stu-id="c68d7-108">For details, consult [RFC 6265](http://tools.ietf.org/html/rfc6265).</span></span>

<span data-ttu-id="c68d7-109">Una cookie es un fragmento de datos que un servidor envía en la respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="c68d7-109">A cookie is a piece of data that a server sends in the HTTP response.</span></span> <span data-ttu-id="c68d7-110">El cliente (opcionalmente) almacena la cookie y la devuelve en solicitudes posteriores.</span><span class="sxs-lookup"><span data-stu-id="c68d7-110">The client (optionally) stores the cookie and returns it on subsequent requests.</span></span> <span data-ttu-id="c68d7-111">Esto permite que el cliente y el servidor compartan el estado.</span><span class="sxs-lookup"><span data-stu-id="c68d7-111">This allows the client and server to share state.</span></span> <span data-ttu-id="c68d7-112">Para establecer una cookie, el servidor incluye un encabezado Set-Cookie en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="c68d7-112">To set a cookie, the server includes a Set-Cookie header in the response.</span></span> <span data-ttu-id="c68d7-113">El formato de una cookie es un par nombre-valor, con atributos opcionales.</span><span class="sxs-lookup"><span data-stu-id="c68d7-113">The format of a cookie is a name-value pair, with optional attributes.</span></span> <span data-ttu-id="c68d7-114">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c68d7-114">For example:</span></span>

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

<span data-ttu-id="c68d7-115">A continuación se muestra un ejemplo con atributos:</span><span class="sxs-lookup"><span data-stu-id="c68d7-115">Here is an example with attributes:</span></span>

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

<span data-ttu-id="c68d7-116">Para devolver una cookie al servidor, el cliente incluye un encabezado de cookie en las solicitudes posteriores.</span><span class="sxs-lookup"><span data-stu-id="c68d7-116">To return a cookie to the server, the client includes a Cookie header in later requests.</span></span>

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

<span data-ttu-id="c68d7-117">Una respuesta HTTP puede incluir varios encabezados Set-Cookie.</span><span class="sxs-lookup"><span data-stu-id="c68d7-117">An HTTP response can include multiple Set-Cookie headers.</span></span>

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

<span data-ttu-id="c68d7-118">El cliente devuelve varias cookies mediante un encabezado de cookie único.</span><span class="sxs-lookup"><span data-stu-id="c68d7-118">The client returns multiple cookies using a single Cookie header.</span></span>

[!code-console[Main](http-cookies/samples/sample5.cmd)]

<span data-ttu-id="c68d7-119">El ámbito y la duración de una cookie se controlan mediante los siguientes atributos del encabezado Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="c68d7-119">The scope and duration of a cookie are controlled by following attributes in the Set-Cookie header:</span></span>

- <span data-ttu-id="c68d7-120">**Dominio**: indica al cliente qué dominio debe recibir la cookie.</span><span class="sxs-lookup"><span data-stu-id="c68d7-120">**Domain**: Tells the client which domain should receive the cookie.</span></span> <span data-ttu-id="c68d7-121">Por ejemplo, si el dominio es "example.com", el cliente devuelve la cookie a todos los subdominios de example.com.</span><span class="sxs-lookup"><span data-stu-id="c68d7-121">For example, if the domain is "example.com", the client returns the cookie to every subdomain of example.com.</span></span> <span data-ttu-id="c68d7-122">Si no se especifica, el dominio es el servidor de origen.</span><span class="sxs-lookup"><span data-stu-id="c68d7-122">If not specified, the domain is the origin server.</span></span>
- <span data-ttu-id="c68d7-123">**Ruta de acceso**: restringe la cookie a la ruta de acceso especificada dentro del dominio.</span><span class="sxs-lookup"><span data-stu-id="c68d7-123">**Path**: Restricts the cookie to the specified path within the domain.</span></span> <span data-ttu-id="c68d7-124">Si no se especifica, se usa la ruta de acceso del URI de solicitud.</span><span class="sxs-lookup"><span data-stu-id="c68d7-124">If not specified, the path of the request URI is used.</span></span>
- <span data-ttu-id="c68d7-125">**Expires**: establece una fecha de expiración para la cookie.</span><span class="sxs-lookup"><span data-stu-id="c68d7-125">**Expires**: Sets an expiration date for the cookie.</span></span> <span data-ttu-id="c68d7-126">El cliente elimina la cookie cuando expira.</span><span class="sxs-lookup"><span data-stu-id="c68d7-126">The client deletes the cookie when it expires.</span></span>
- <span data-ttu-id="c68d7-127">**Max-Age**: establece la antigüedad máxima de la cookie.</span><span class="sxs-lookup"><span data-stu-id="c68d7-127">**Max-Age**: Sets the maximum age for the cookie.</span></span> <span data-ttu-id="c68d7-128">El cliente elimina la cookie cuando llega a la antigüedad máxima.</span><span class="sxs-lookup"><span data-stu-id="c68d7-128">The client deletes the cookie when it reaches the maximum age.</span></span>

<span data-ttu-id="c68d7-129">Si se establecen `Expires` y `Max-Age`, `Max-Age` tiene prioridad.</span><span class="sxs-lookup"><span data-stu-id="c68d7-129">If both `Expires` and `Max-Age` are set, `Max-Age` takes precedence.</span></span> <span data-ttu-id="c68d7-130">Si no se establece ninguno, el cliente elimina la cookie cuando finaliza la sesión actual.</span><span class="sxs-lookup"><span data-stu-id="c68d7-130">If neither is set, the client deletes the cookie when the current session ends.</span></span> <span data-ttu-id="c68d7-131">(El significado exacto de "session" viene determinado por el agente de usuario).</span><span class="sxs-lookup"><span data-stu-id="c68d7-131">(The exact meaning of "session" is determined by the user-agent.)</span></span>

<span data-ttu-id="c68d7-132">Sin embargo, tenga en cuenta que los clientes pueden omitir las cookies.</span><span class="sxs-lookup"><span data-stu-id="c68d7-132">However, be aware that clients may ignore cookies.</span></span> <span data-ttu-id="c68d7-133">Por ejemplo, un usuario podría deshabilitar las cookies por motivos de privacidad.</span><span class="sxs-lookup"><span data-stu-id="c68d7-133">For example, a user might disable cookies for privacy reasons.</span></span> <span data-ttu-id="c68d7-134">Los clientes pueden eliminar cookies antes de que expiren, o bien limitar el número de cookies almacenadas.</span><span class="sxs-lookup"><span data-stu-id="c68d7-134">Clients may delete cookies before they expire, or limit the number of cookies stored.</span></span> <span data-ttu-id="c68d7-135">Por motivos de privacidad, los clientes a menudo rechazan las cookies de "terceros", donde el dominio no coincide con el servidor de origen.</span><span class="sxs-lookup"><span data-stu-id="c68d7-135">For privacy reasons, clients often reject "third party" cookies, where the domain does not match the origin server.</span></span> <span data-ttu-id="c68d7-136">En Resumen, el servidor no debe confiar en obtener las cookies que establece.</span><span class="sxs-lookup"><span data-stu-id="c68d7-136">In short, the server should not rely on getting back the cookies that it sets.</span></span>

## <a name="cookies-in-web-api"></a><span data-ttu-id="c68d7-137">Cookies en Web API</span><span class="sxs-lookup"><span data-stu-id="c68d7-137">Cookies in Web API</span></span>

<span data-ttu-id="c68d7-138">Para agregar una cookie a una respuesta HTTP, cree una instancia de **CookieHeaderValue** que represente la cookie.</span><span class="sxs-lookup"><span data-stu-id="c68d7-138">To add a cookie to an HTTP response, create a **CookieHeaderValue** instance that represents the cookie.</span></span> <span data-ttu-id="c68d7-139">A continuación, llame al método de extensión **AddCookies** , que se define en **System .net. http. Clase HttpResponseHeadersExtensions** , para agregar la cookie.</span><span class="sxs-lookup"><span data-stu-id="c68d7-139">Then call the **AddCookies** extension method, which is defined in the **System.Net.Http. HttpResponseHeadersExtensions** class, to add the cookie.</span></span>

<span data-ttu-id="c68d7-140">Por ejemplo, el código siguiente agrega una cookie dentro de una acción del controlador:</span><span class="sxs-lookup"><span data-stu-id="c68d7-140">For example, the following code adds a cookie within a controller action:</span></span>

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

<span data-ttu-id="c68d7-141">Tenga en cuenta que **AddCookies** toma una matriz de instancias de **CookieHeaderValue** .</span><span class="sxs-lookup"><span data-stu-id="c68d7-141">Notice that **AddCookies** takes an array of **CookieHeaderValue** instances.</span></span>

<span data-ttu-id="c68d7-142">Para extraer las cookies de una solicitud de cliente, llame al método **GetCookies** :</span><span class="sxs-lookup"><span data-stu-id="c68d7-142">To extract the cookies from a client request, call the **GetCookies** method:</span></span>

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

<span data-ttu-id="c68d7-143">Un **CookieHeaderValue** contiene una colección de instancias de **CookieState** .</span><span class="sxs-lookup"><span data-stu-id="c68d7-143">A **CookieHeaderValue** contains a collection of **CookieState** instances.</span></span> <span data-ttu-id="c68d7-144">Cada **CookieState** representa una cookie.</span><span class="sxs-lookup"><span data-stu-id="c68d7-144">Each **CookieState** represents one cookie.</span></span> <span data-ttu-id="c68d7-145">Use el método de indexador para obtener un **CookieState** por nombre, como se muestra.</span><span class="sxs-lookup"><span data-stu-id="c68d7-145">Use the indexer method to get a **CookieState** by name, as shown.</span></span>

## <a name="structured-cookie-data"></a><span data-ttu-id="c68d7-146">Datos de cookies estructurados</span><span class="sxs-lookup"><span data-stu-id="c68d7-146">Structured Cookie Data</span></span>

<span data-ttu-id="c68d7-147">Muchos exploradores limitan cuántas cookies almacenarán&#8212;tanto el número total como el número por dominio.</span><span class="sxs-lookup"><span data-stu-id="c68d7-147">Many browsers limit how many cookies they will store&#8212;both the total number, and the number per domain.</span></span> <span data-ttu-id="c68d7-148">Por lo tanto, puede ser útil poner los datos estructurados en una cookie única, en lugar de establecer varias cookies.</span><span class="sxs-lookup"><span data-stu-id="c68d7-148">Therefore, it can be useful to put structured data into a single cookie, instead of setting multiple cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="c68d7-149">RFC 6265 no define la estructura de los datos de cookies.</span><span class="sxs-lookup"><span data-stu-id="c68d7-149">RFC 6265 does not define the structure of cookie data.</span></span>

<span data-ttu-id="c68d7-150">Con la clase **CookieHeaderValue** , puede pasar una lista de pares nombre-valor para los datos de cookies.</span><span class="sxs-lookup"><span data-stu-id="c68d7-150">Using the **CookieHeaderValue** class, you can pass a list of name-value pairs for the cookie data.</span></span> <span data-ttu-id="c68d7-151">Estos pares de nombre y valor se codifican como datos de formulario con codificación URL en el encabezado Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="c68d7-151">These name-value pairs are encoded as URL-encoded form data in the Set-Cookie header:</span></span>

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

<span data-ttu-id="c68d7-152">El código anterior genera el siguiente encabezado Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="c68d7-152">The previous code produces the following Set-Cookie header:</span></span>

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

<span data-ttu-id="c68d7-153">La clase **CookieState** proporciona un método de indexador para leer los subvalores de una cookie en el mensaje de solicitud:</span><span class="sxs-lookup"><span data-stu-id="c68d7-153">The **CookieState** class provides an indexer method to read the sub-values from a cookie in the request message:</span></span>

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a><span data-ttu-id="c68d7-154">Ejemplo: establecimiento y recuperación de cookies en un controlador de mensajes</span><span class="sxs-lookup"><span data-stu-id="c68d7-154">Example: Set and Retrieve Cookies in a Message Handler</span></span>

<span data-ttu-id="c68d7-155">En los ejemplos anteriores se mostró cómo usar las cookies desde un controlador de API Web.</span><span class="sxs-lookup"><span data-stu-id="c68d7-155">The previous examples showed how to use cookies from within a Web API controller.</span></span> <span data-ttu-id="c68d7-156">Otra opción consiste en usar [controladores de mensajes](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="c68d7-156">Another option is to use [message handlers](http-message-handlers.md).</span></span> <span data-ttu-id="c68d7-157">Los controladores de mensajes se invocan anteriormente en la canalización que los controladores.</span><span class="sxs-lookup"><span data-stu-id="c68d7-157">Message handlers are invoked earlier in the pipeline than controllers.</span></span> <span data-ttu-id="c68d7-158">Un controlador de mensajes puede leer cookies de la solicitud antes de que la solicitud llegue al controlador o agregar cookies a la respuesta después de que el controlador genere la respuesta.</span><span class="sxs-lookup"><span data-stu-id="c68d7-158">A message handler can read cookies from the request before the request reaches the controller, or add cookies to the response after the controller generates the response.</span></span>

![](http-cookies/_static/image2.png)

<span data-ttu-id="c68d7-159">En el código siguiente se muestra un controlador de mensajes para crear identificadores de sesión.</span><span class="sxs-lookup"><span data-stu-id="c68d7-159">The following code shows a message handler for creating session IDs.</span></span> <span data-ttu-id="c68d7-160">El ID. de sesión se almacena en una cookie.</span><span class="sxs-lookup"><span data-stu-id="c68d7-160">The session ID is stored in a cookie.</span></span> <span data-ttu-id="c68d7-161">El controlador comprueba la solicitud de la cookie de sesión.</span><span class="sxs-lookup"><span data-stu-id="c68d7-161">The handler checks the request for the session cookie.</span></span> <span data-ttu-id="c68d7-162">Si la solicitud no incluye la cookie, el controlador genera un nuevo identificador de sesión.</span><span class="sxs-lookup"><span data-stu-id="c68d7-162">If the request does not include the cookie, the handler generates a new session ID.</span></span> <span data-ttu-id="c68d7-163">En cualquier caso, el controlador almacena el identificador de sesión en el contenedor de propiedades **HttpRequestMessage. Properties** .</span><span class="sxs-lookup"><span data-stu-id="c68d7-163">In either case, the handler stores the session ID in the **HttpRequestMessage.Properties** property bag.</span></span> <span data-ttu-id="c68d7-164">También agrega la cookie de sesión a la respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="c68d7-164">It also adds the session cookie to the HTTP response.</span></span>

<span data-ttu-id="c68d7-165">Esta implementación no valida que el servidor haya emitido realmente el identificador de sesión del cliente.</span><span class="sxs-lookup"><span data-stu-id="c68d7-165">This implementation does not validate that the session ID from the client was actually issued by the server.</span></span> <span data-ttu-id="c68d7-166">No lo use como forma de autenticación.</span><span class="sxs-lookup"><span data-stu-id="c68d7-166">Don't use it as a form of authentication!</span></span> <span data-ttu-id="c68d7-167">El punto del ejemplo es mostrar la administración de cookies HTTP.</span><span class="sxs-lookup"><span data-stu-id="c68d7-167">The point of the example is to show HTTP cookie management.</span></span>

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

<span data-ttu-id="c68d7-168">Un controlador puede obtener el identificador de sesión del contenedor de propiedades **HttpRequestMessage. Properties** .</span><span class="sxs-lookup"><span data-stu-id="c68d7-168">A controller can get the session ID from the **HttpRequestMessage.Properties** property bag.</span></span>

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
