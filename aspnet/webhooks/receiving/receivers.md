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
# <a name="aspnet-webhooks-receivers"></a>Receptores de webhooks de ASP.NET

La recepción de webhooks depende de quién sea el remitente. A veces, hay pasos adicionales que registran un webhook para comprobar que el suscriptor está escuchando realmente. Algunos webhooks proporcionan un modelo de inserción y extracción en el que la solicitud HTTP POST solo contiene una referencia a la información de eventos que se recuperará de forma independiente. A menudo, el modelo de seguridad varía bastante.

El propósito de Microsoft ASP.NET webhooks es que sea más sencillo y más coherente para conectar la API sin dedicar mucho tiempo a averiguar cómo tratar cualquier variante determinada de webhooks.

Un receptor de webhook es responsable de aceptar y comprobar los webhooks de un remitente determinado. Un receptor de webhook puede admitir cualquier número de webhooks, cada uno con su propia configuración. Por ejemplo, el receptor de webhook de GitHub puede aceptar webhooks de cualquier número de repositorios de GitHub.

## <a name="webhook-receiver-uris"></a>URI del receptor de webhook

Mediante la instalación de Microsoft ASP.NET webhooks obtiene un controlador de webhook general que acepta solicitudes de webhook de un número de servicios abierto. Cuando llega una solicitud, elige el receptor adecuado que ha instalado para controlar un remitente de webhook determinado.

El URI de este controlador es el URI de webhook que se registra con el servicio y tiene el formato:

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

Por motivos de seguridad, muchos receptores de webhook requieren que el URI sea un URI *https* y, en algunos casos, también debe contener un parámetro de consulta adicional que se utiliza para exigir que solo la entidad deseada pueda enviar webhooks al URI anterior.

El componente `<receiver>` es el nombre del receptor, por ejemplo `github` o `slack`.

*{ID}* es un identificador opcional que se puede usar para identificar una configuración determinada del receptor de webhook. Se puede usar para registrar N webhooks con un receptor determinado. Por ejemplo, se pueden usar los tres URI siguientes para registrarse en tres webhooks independientes:

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>Instalación de un receptor de webhook

Para recibir webhooks con Microsoft ASP.NET webhooks, primero debe instalar el paquete de Nuget para el proveedor o proveedores de webhook de los que desea recibir webhooks. Los paquetes Nuget se denominan [Microsoft. Aspnet. Webhooks. receptores. *](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) donde la última parte indica el servicio admitido. Por ejemplo

[Microsoft. Aspnet. Webhooks. receptores. github](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) proporciona compatibilidad para la recepción de webhooks de Github y [Microsoft. Aspnet. webhooks](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) . el servicio personalizado proporciona compatibilidad para recibir webhooks generados por webhooks de ASP.net.

De forma remota, puede encontrar compatibilidad con Dropbox, GitHub, MailChimp, PayPal, inserciones, Salesforce, el margen de demora, la franja, Trello y WordPress, pero es posible admitir cualquier número de proveedores.

## <a name="configuring-a-webhook-receiver"></a>Configuración de un receptor de webhook

Los receptores de webhook se configuran a través de la interfaz [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) y las implementaciones concretas de esa interfaz se pueden registrar con cualquier modelo de inyección de dependencia. La implementación predeterminada usa la configuración de la aplicación que se puede establecer en el archivo Web. config, o bien, si se usa Azure Web Apps, puede establecerse a través de [Azure portal](https://portal.azure.com/).

![Configuración de aplicaciones web](_static/AzureAppSettings.png)

El formato de las claves de configuración de la aplicación es el siguiente:

```
MS_WebHookReceiverSecret_<receiver>
```

El valor es una lista separada por comas de valores que coinciden con los valores *{ID}* para los que se han registrado webhooks, por ejemplo:

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>Inicialización de un receptor de webhook

Los receptores de webhook se inicializan mediante su registro, normalmente en la clase estática *WebApiConfig* , por ejemplo:

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
