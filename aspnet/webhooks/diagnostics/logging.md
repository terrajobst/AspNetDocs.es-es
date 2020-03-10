---
uid: webhooks/diagnostics/logging
title: Registro de webhooks de ASP.NET | Microsoft Docs
author: rick-anderson
description: Cómo realizar el registro en webhooks de ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.openlocfilehash: a05b32c4a8f9577bcf6170bd19a9e440b1aeb75b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78440911"
---
# <a name="aspnet-webhooks-logging"></a>Registro de webhooks de ASP.NET

Microsoft ASP.NET webhooks usa el registro como una manera de informar de problemas y problemas. De forma predeterminada, los registros se escriben con [System. Diagnostics. Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) , donde se pueden llevar a cabo mediante [agentes de escucha de seguimiento](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) como cualquier otro flujo de registro.

Al implementar la aplicación web como una aplicación Web de Azure, los registros se seleccionan automáticamente y se pueden administrar junto con cualquier otro registro [System. Diagnostics. Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) . Para obtener más información, consulte [habilitación del registro de diagnóstico para aplicaciones web en Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)

Además, los registros se pueden obtener directamente desde Visual Studio, tal y como se describe en [solución de problemas de una aplicación web en Azure App Service con Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="redirecting-logs"></a>Redirigir registros

En lugar de escribir registros en [System. Diagnostics. Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), es posible proporcionar una implementación de registro alternativa que pueda registrarse directamente en un administrador de registros como [log4net](http://logging.apache.org/log4net/) y [NLog](http://nlog-project.org/). Simplemente proporcione una implementación de [ILogger](https://github.com/aspnet/AspNetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) y regístrela con un motor de inserción de dependencias de su elección y lo recogerán Microsoft ASP.net webhooks. Para más información, consulte la [inserción de dependencias en ASP.net web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) .
