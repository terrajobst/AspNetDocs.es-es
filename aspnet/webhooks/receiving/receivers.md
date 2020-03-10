---
uid: webhooks/receiving/receivers
title: Receptores de webhooks de ASP.NET | Microsoft Docs
author: rick-anderson
description: Receptores de webhooks de ASP.NET
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.openlocfilehash: d771a588b23abcd7b1b33e694af17b219683fc48
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78463465"
---
# <a name="aspnet-webhooks-receivers"></a><span data-ttu-id="5d5a9-103">Receptores de webhooks de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5d5a9-103">ASP.NET WebHooks receivers</span></span>

<span data-ttu-id="5d5a9-104">La recepción de webhooks depende de quién sea el remitente.</span><span class="sxs-lookup"><span data-stu-id="5d5a9-104">Receiving WebHooks depends on who the sender is.</span></span> <span data-ttu-id="5d5a9-105">A veces, hay pasos adicionales que registran un webhook para comprobar que el suscriptor está escuchando realmente.</span><span class="sxs-lookup"><span data-stu-id="5d5a9-105">Sometimes there are additional steps registering a WebHook verifying that the subscriber is really listening.</span></span> <span data-ttu-id="5d5a9-106">Algunos webhooks proporcionan un modelo de inserción y extracción en el que la solicitud HTTP POST solo contiene una referencia a la información de eventos que se recuperará de forma independiente.</span><span class="sxs-lookup"><span data-stu-id="5d5a9-106">Some WebHooks provide a push-to-pull model where the HTTP POST request only contains a reference to the event information which is then to be retrieved independently.</span></span> <span data-ttu-id="5d5a9-107">A menudo, el modelo de seguridad varía bastante.</span><span class="sxs-lookup"><span data-stu-id="5d5a9-107">Often the security model varies quite a bit.</span></span>

<span data-ttu-id="5d5a9-108">El propósito de Microsoft ASP.NET webhooks es que sea más sencillo y más coherente para conectar la API sin dedicar mucho tiempo a averiguar cómo tratar cualquier variante determinada de webhooks.</span><span class="sxs-lookup"><span data-stu-id="5d5a9-108">The purpose of Microsoft ASP.NET WebHooks is to make it both simpler and more consistent to wire up your API without spending a lot of time figuring out how to handle any particular variant of WebHooks.</span></span>

<span data-ttu-id="5d5a9-109">Un receptor de webhook es responsable de aceptar y comprobar los webhooks de un remitente determinado.</span><span class="sxs-lookup"><span data-stu-id="5d5a9-109">A WebHook receiver is responsible for accepting and verifying WebHooks from a particular sender.</span></span> <span data-ttu-id="5d5a9-110">Un receptor de webhook puede admitir cualquier número de webhooks, cada uno con su propia configuración.</span><span class="sxs-lookup"><span data-stu-id="5d5a9-110">A WebHook receiver can support any number of WebHooks, each with their own configuration.</span></span> <span data-ttu-id="5d5a9-111">Por ejemplo, el receptor de webhook de GitHub puede aceptar webhooks de cualquier número de repositorios de GitHub.</span><span class="sxs-lookup"><span data-stu-id="5d5a9-111">For example, the GitHub WebHook receiver can accept WebHooks from any number of GitHub repositories.</span></span>

## <a name="webhook-receiver-uris"></a><span data-ttu-id="5d5a9-112">URI del receptor de webhook</span><span class="sxs-lookup"><span data-stu-id="5d5a9-112">WebHook Receiver URIs</span></span>

<span data-ttu-id="5d5a9-113">Mediante la instalación de Microsoft ASP.NET webhooks obtiene un controlador de webhook general que acepta solicitudes de webhook de un número de servicios abierto.</span><span class="sxs-lookup"><span data-stu-id="5d5a9-113">By installing Microsoft ASP.NET WebHooks you get a general WebHook controller which accepts WebHook requests from an open-ended number of services.</span></span> <span data-ttu-id="5d5a9-114">Cuando llega una solicitud, elige el receptor adecuado que ha instalado para controlar un remitente de webhook determinado.</span><span class="sxs-lookup"><span data-stu-id="5d5a9-114">When a request arrives, it picks the appropriate receiver that you have installed for handling a particular WebHook sender.</span></span>

<span data-ttu-id="5d5a9-115">El URI de este controlador es el URI de webhook que se registra con el servicio y tiene el formato:</span><span class="sxs-lookup"><span data-stu-id="5d5a9-115">The URI of this controller is the WebHook URI that you register with the service and is of the form:</span></span>

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

<span data-ttu-id="5d5a9-116">Por motivos de seguridad, muchos receptores de webhook requieren que el URI sea un URI *https* y, en algunos casos, también debe contener un parámetro de consulta adicional que se utiliza para exigir que solo la entidad deseada pueda enviar webhooks al URI anterior.</span><span class="sxs-lookup"><span data-stu-id="5d5a9-116">For security reasons, many WebHook receivers require that the URI is an *https* URI and in some cases it must also contain an additional query parameter which is used to enforce that only the intended party can send WebHooks to the URI above.</span></span>

<span data-ttu-id="5d5a9-117">El componente `<receiver>` es el nombre del receptor, por ejemplo `github` o `slack`.</span><span class="sxs-lookup"><span data-stu-id="5d5a9-117">The `<receiver>` component is the name of the receiver, for example `github` or `slack`.</span></span>

<span data-ttu-id="5d5a9-118">*{ID}* es un identificador opcional que se puede usar para identificar una configuración determinada del receptor de webhook.</span><span class="sxs-lookup"><span data-stu-id="5d5a9-118">The *{id}* is an optional identifier which can be used to identify a particular WebHook receiver configuration.</span></span> <span data-ttu-id="5d5a9-119">Se puede usar para registrar N webhooks con un receptor determinado.</span><span class="sxs-lookup"><span data-stu-id="5d5a9-119">This can be used to register N WebHooks with a particular receiver.</span></span> <span data-ttu-id="5d5a9-120">Por ejemplo, se pueden usar los tres URI siguientes para registrarse en tres webhooks independientes:</span><span class="sxs-lookup"><span data-stu-id="5d5a9-120">For example, the following three URIs can be used to register for three independent WebHooks:</span></span>

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a><span data-ttu-id="5d5a9-121">Instalación de un receptor de webhook</span><span class="sxs-lookup"><span data-stu-id="5d5a9-121">Installing a WebHook Receiver</span></span>

<span data-ttu-id="5d5a9-122">Para recibir webhooks con Microsoft ASP.NET webhooks, primero debe instalar el paquete de Nuget para el proveedor o proveedores de webhook de los que desea recibir webhooks.</span><span class="sxs-lookup"><span data-stu-id="5d5a9-122">To receive WebHooks using Microsoft ASP.NET WebHooks, you first install the Nuget package for the WebHook provider or providers you want to receive WebHooks from.</span></span> <span data-ttu-id="5d5a9-123">Los paquetes Nuget se denominan [Microsoft. Aspnet. Webhooks. receptores. \*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) donde la última parte indica el servicio admitido.</span><span class="sxs-lookup"><span data-stu-id="5d5a9-123">The Nuget packages are named [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) where the last part indicates the service supported.</span></span> <span data-ttu-id="5d5a9-124">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="5d5a9-124">For example</span></span>

<span data-ttu-id="5d5a9-125">[Microsoft. Aspnet. Webhooks. receptores. github](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) proporciona compatibilidad para la recepción de webhooks de Github y [Microsoft. Aspnet. webhooks](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) . el servicio personalizado proporciona compatibilidad para recibir webhooks generados por webhooks de ASP.net.</span><span class="sxs-lookup"><span data-stu-id="5d5a9-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) provides support for receiving WebHooks from GitHub and [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) provides support for receiving WebHooks generated by ASP.NET WebHooks.</span></span>

<span data-ttu-id="5d5a9-126">De forma remota, puede encontrar compatibilidad con Dropbox, GitHub, MailChimp, PayPal, inserciones, Salesforce, el margen de demora, la franja, Trello y WordPress, pero es posible admitir cualquier número de proveedores.</span><span class="sxs-lookup"><span data-stu-id="5d5a9-126">Out of the box you can find support for Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, and WordPress but it is possible to support any number of other providers.</span></span>

## <a name="configuring-a-webhook-receiver"></a><span data-ttu-id="5d5a9-127">Configuración de un receptor de webhook</span><span class="sxs-lookup"><span data-stu-id="5d5a9-127">Configuring a WebHook Receiver</span></span>

<span data-ttu-id="5d5a9-128">Los receptores de webhook se configuran a través de la interfaz [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) y las implementaciones concretas de esa interfaz se pueden registrar con cualquier modelo de inyección de dependencia.</span><span class="sxs-lookup"><span data-stu-id="5d5a9-128">WebHook Receivers are configured through the [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) inteface and particular implementations of that interface can be registered using any dependency injection model.</span></span> <span data-ttu-id="5d5a9-129">La implementación predeterminada usa la configuración de la aplicación que se puede establecer en el archivo Web. config, o bien, si se usa Azure Web Apps, puede establecerse a través de [Azure portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="5d5a9-129">The default implementation uses Application Settings which can either be set in the Web.config file, or, if using Azure Web Apps, can be set through the [Azure Portal](https://portal.azure.com/).</span></span>

![Configuración de aplicaciones web](_static/AzureAppSettings.png)

<span data-ttu-id="5d5a9-131">El formato de las claves de configuración de la aplicación es el siguiente:</span><span class="sxs-lookup"><span data-stu-id="5d5a9-131">The format for Application Setting keys is as follows:</span></span>

```
MS_WebHookReceiverSecret_<receiver>
```

<span data-ttu-id="5d5a9-132">El valor es una lista separada por comas de valores que coinciden con los valores *{ID}* para los que se han registrado webhooks, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="5d5a9-132">The value is a comma-separated list of values matching the *{id}* values for which WebHooks have been registered, for example:</span></span>

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a><span data-ttu-id="5d5a9-133">Inicialización de un receptor de webhook</span><span class="sxs-lookup"><span data-stu-id="5d5a9-133">Initializing a WebHook Receiver</span></span>

<span data-ttu-id="5d5a9-134">Los receptores de webhook se inicializan mediante su registro, normalmente en la clase estática *WebApiConfig* , por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="5d5a9-134">WebHook Receivers are initialized by registering them, typically in the *WebApiConfig* static class, for example:</span></span>

```csharp
namespace WebHookReceivers
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            // Load receivers
            config.InitializeReceiveGitHubWebHooks();
        }
    }
}
```
