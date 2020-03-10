---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Validación del modelo en ASP.NET Web API ASP.NET 4. x
author: MikeWasson
description: Información general sobre la validación de modelos en ASP.NET Web API para ASP.NET 4. x.
ms.author: riande
ms.date: 07/20/2012
ms.custom: seoapril2019
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 531a66b7ab642bd012663517640f2766f1917f25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448933"
---
# <a name="model-validation-in-aspnet-web-api"></a>Validación del modelo en ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

En este artículo se muestra cómo anotar los modelos, usar las anotaciones para la validación de datos y controlar los errores de validación en la API Web. Cuando un cliente envía datos a la API Web, a menudo desea validar los datos antes de realizar cualquier procesamiento. 

## <a name="data-annotations"></a>Anotaciones de datos

En ASP.NET Web API, puede usar atributos del espacio de nombres [System. ComponentModel. DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) para establecer reglas de validación para las propiedades del modelo. Considere el modelo siguiente:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

Si ha usado la validación de modelos en ASP.NET MVC, debería resultar familiar. El atributo **required** indica que la propiedad `Name` no debe ser null. El atributo de **intervalo** indica que `Weight` debe estar entre cero y 999.

Supongamos que un cliente envía una solicitud POST con la siguiente representación JSON:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

Puede ver que el cliente no incluyó la propiedad `Name`, que está marcada como requerida. Cuando Web API convierte el archivo JSON en una instancia de `Product`, valida el `Product` con los atributos de validación. En la acción del controlador, puede comprobar si el modelo es válido:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

La validación del modelo no garantiza que los datos del cliente sean seguros. Podría ser necesaria una validación adicional en otras capas de la aplicación. (Por ejemplo, el nivel de datos puede exigir restricciones Foreign Key). El tutorial [uso de Web API con Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explora algunos de estos problemas.

**"Superexponer"** : la subexposición se produce cuando el cliente abandona algunas propiedades. Por ejemplo, supongamos que el cliente envía lo siguiente:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

Aquí, el cliente no especificó valores para `Price` o `Weight`. El formateador JSON asigna un valor predeterminado de cero a las propiedades que faltan.

![](model-validation-in-aspnet-web-api/_static/image1.png)

El estado del modelo es válido, porque cero es un valor válido para estas propiedades. El hecho de que se trata de un problema depende del escenario. Por ejemplo, en una operación de actualización, es posible que desee distinguir entre "cero" y "sin establecer". Para obligar a los clientes a establecer un valor, haga que la propiedad acepte valores NULL y establezca el atributo **required** :

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

**"Sobreexposición"** : un cliente también puede enviar *más* datos de los esperados. Por ejemplo:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

Aquí, el archivo JSON incluye una propiedad ("color") que no existe en el modelo de `Product`. En este caso, el formateador JSON simplemente omite este valor. (El formateador XML hace lo mismo). La publicación en exceso produce problemas si el modelo tiene propiedades que desea que sean de solo lectura. Por ejemplo:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

No quiere que los usuarios actualicen la propiedad `IsAdmin` y se eleven a los administradores. La estrategia más segura es usar una clase de modelo que coincida exactamente con la que el cliente puede enviar:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> La entrada de blog de Brad Wilson "[validación de entrada frente a validación de modelo en ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" tiene una buena explicación de la publicación en exceso y la publicación en exceso. Aunque la publicación es sobre ASP.NET MVC 2, los problemas siguen siendo pertinentes para la API Web.

## <a name="handling-validation-errors"></a>Control de errores de validación

La API Web no devuelve automáticamente un error al cliente cuando se produce un error de validación. Depende de la acción del controlador comprobar el estado del modelo y responder de forma adecuada.

También puede crear un filtro de acción para comprobar el estado del modelo antes de que se invoque la acción del controlador. El código siguiente muestra un ejemplo:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

Si se produce un error en la validación del modelo, este filtro devuelve una respuesta HTTP que contiene los errores de validación. En ese caso, no se invoca la acción del controlador.

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

Para aplicar este filtro a todos los controladores de API Web, agregue una instancia del filtro a la colección **HttpConfiguration. filters** durante la configuración:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

Otra opción consiste en establecer el filtro como un atributo en los controladores o acciones de controlador individuales:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
