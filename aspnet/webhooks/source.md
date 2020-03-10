---
uid: webhooks/source
title: Código fuente de webhooks de ASP.NET y paquetes NuGet | Microsoft Docs
author: rick-anderson
description: Vínculos a código fuente de webhooks de ASP.NET y paquetes NuGet
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.openlocfilehash: 8d07848754d9efda9c893b8ba54ac6d0c0214a53
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78513913"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>Código fuente de webhooks de ASP.NET y paquetes NuGet

Microsoft ASP.NET webhooks forma parte de la familia de Microsoft ASP.NET de módulos y se hospeda como un [proyecto de código abierto en github](https://github.com/aspnet/WebHooks). Esto significa que se aceptan contribuciones, pero consulte las directrices de [contribución](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) antes de enviar una solicitud de incorporación de cambios.

Esta documentación en línea que está leyendo ahora también se hospeda como [código abierto en github](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) y también acepta contribuciones.

## <a name="nuget-packages"></a>Paquetes NuGet

Los [paquetes NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) se dividen en tres partes:

* [Común](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): un paquete común que se comparte entre remitentes y receptores.

* [Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): un conjunto de paquetes que permiten enviar sus propios webhooks a otros usuarios. La funcionalidad para enviar webhooks se describe con más detalle en [envío de Webhooks](sending/senders.md).

* [Receptores](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): un conjunto de paquetes que admiten la recepción de webhooks de otros usuarios. La funcionalidad para recibir webhooks se describe con más detalle en [recepción de Webhooks](receiving/index.md).
