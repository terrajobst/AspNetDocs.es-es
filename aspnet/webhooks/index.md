---
uid: webhooks/index
title: Información general sobre webhooks de ASP.NET | Microsoft Docs
author: rick-anderson
description: Una introducción a webhooks de ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.openlocfilehash: 1e21c92e950893c0ff87c63f03f4710a158441fd
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2020
ms.locfileid: "77075091"
---
# <a name="aspnet-webhooks-overview"></a>Información general de webhooks de ASP.NET

Webhooks es un patrón de HTTP ligero que proporciona un modelo pub/sub sencillo para el cableado de las API Web y los servicios SaaS. Cuando se produce un evento en un servicio, se envía una notificación en forma de una solicitud HTTP POST a los suscriptores registrados. La solicitud POST contiene información sobre el evento, lo que hace posible que el receptor actúe en consecuencia.

Debido a su simplicidad, los webhooks ya están expuestos por un gran número de servicios, como [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [Bitbucket](https://bitbucket.org/), [Mailchimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), el [margen de demora](http://www.slack.com), la [franja](http://www.stripe.com), [Trello](http://www.trello.com/)y muchos otros. Por ejemplo, un webhook puede indicar que un archivo ha cambiado en [Dropbox](http://dropbox.com/)o que se ha confirmado un cambio de código en Github o que se ha iniciado un pago en [PayPal](http://www.paypal.com/)o que se ha creado una tarjeta en [Trello](http://www.trello.com/). Las posibilidades son infinitas.

Microsoft ASP.NET webhooks facilita el envío y la recepción de webhooks como parte de la aplicación ASP.NET:

* En el lado receptor, proporciona un modelo común para recibir y procesar webhooks de cualquier número de proveedores de webhook. Viene de la caja con la compatibilidad con [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [Bitbucket](https://bitbucket.org/), [Mailchimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [inserciones](http://www.pusher.com), [Salesforce](http://www.salesforce.com), la [demora](http://www.slack.com), la [franja](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) y [zendesk](https://www.zendesk.com/) , pero es fácil agregar compatibilidad para más.

* En el lado de envío, proporciona compatibilidad para administrar y almacenar suscripciones, así como para enviar notificaciones de eventos al conjunto de suscriptores correcto. Esto le permite definir su propio conjunto de eventos a los que los suscriptores pueden suscribirse y notificarles cuándo sucede todo.

Las dos partes pueden usarse juntas o separadas en función de su escenario. Si solo necesita recibir webhooks de otros servicios, puede usar solo la parte receptora; Si solo desea exponer webhooks para que los usen otros, puede hacerlo.

El código tiene como destino ASP.NET Web API 2 y ASP.NET MVC 5 y está disponible como [OSS en github](https://github.com/aspnet/WebHooks).

## <a name="webhooks-overview"></a>Información general sobre webhooks

Los webhooks son un patrón, lo que significa que varía su uso de servicio a servicio, pero la idea básica es la misma. Los webhooks se pueden considerar como un modelo pub/sub sencillo en el que un usuario puede suscribirse a eventos que se producen en otro lugar. Las notificaciones de eventos se propagan como solicitudes HTTP POST que contienen información sobre el propio evento.

Normalmente, la solicitud HTTP POST contiene un objeto JSON o datos de formulario HTML determinados por el remitente de webhook, incluida información sobre el evento que provoca la activación del webhook. Por ejemplo, un cuerpo de solicitud POST de webhook de [GitHub](https://www.github.com/) tiene el siguiente aspecto como resultado de la apertura de un nuevo problema en un repositorio determinado:

```json
{
  "action": "opened",
  "issue": {
      "url": "https://api.github.com/repos/octocat/Hello-World/issues/1347",
      "number": 1347,
      ...
  },
  "repository": {
      "id": 1296269,
      "full_name": "octocat/Hello-World",
      "owner": {
          "login": "octocat",
          "id": 1
          ...
      },
      ...
  },
  "sender": {
      "login": "octocat",
      "id": 1,
      ...
  }
}
```

Para asegurarse de que el webhook es realmente del remitente deseado, la solicitud POST se protege de algún modo y, a continuación, el receptor lo comprueba. Por ejemplo, los [Webhooks de github](https://developer.github.com/webhooks/) incluyen un encabezado HTTP *X-Hub-Signature* con un hash del cuerpo de la solicitud que la implementación del receptor comprueba, por lo que no tiene que preocuparse por él.

El flujo de webhook suele ser algo parecido a esto:

* El remitente de webhook expone eventos a los que un cliente puede suscribirse. Los eventos describen los cambios observables en el sistema, por ejemplo, que se ha insertado un nuevo elemento de datos, que se ha completado un proceso o algo más.

* El receptor de webhook se suscribe registrando un webhook que consta de cuatro cosas:

     1. Un URI para el lugar en el que se debe exponer la notificación de eventos en forma de solicitud HTTP POST;

     2. Un conjunto de filtros que describen los eventos concretos para los que se debe activar el webhook;

     3. Una clave secreta que se usa para firmar la solicitud HTTP POST;

     4. Datos adicionales que se van a incluir en la solicitud HTTP POST. Por ejemplo, puede tratarse de propiedades o campos de encabezado HTTP adicionales incluidos en el cuerpo de la solicitud HTTP POST.

* Una vez que se produce un evento, se encuentran los registros de webhook coincidentes y se envían las solicitudes HTTP POST. Normalmente, la generación de solicitudes HTTP POST se vuelve a intentar varias veces si por alguna razón el destinatario no responde o la solicitud HTTP POST produce una respuesta de error.

## <a name="webhooks-processing-pipeline"></a>Canalización de procesamiento de webhooks

La canalización de procesamiento de webhooks de Microsoft ASP.NET para los webhooks entrantes tiene el siguiente aspecto:

![Canalización de procesamiento de webhooks de ASP.NET](_static/WebHookReceivers.png)

Los dos conceptos clave aquí son los *receptores* y los *Controladores*:

* Los *receptores* son responsables de administrar el tipo concreto de webhook de un remitente determinado y de aplicar las comprobaciones de seguridad para asegurarse de que la solicitud de webhook es realmente del remitente deseado.

* Los *Controladores* suelen ser donde el código de usuario ejecuta el procesamiento de un webhook determinado.

En los siguientes nodos, estos conceptos se describen con más detalle.
