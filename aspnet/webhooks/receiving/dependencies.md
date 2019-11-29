---
uid: webhooks/receiving/dependencies
title: Dependencias del receptor de webhooks de ASP.NET | Microsoft Docs
author: rick-anderson
description: Dependencias del receptor y la inserción de dependencias en webhooks de ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: 477b8828209d0da1d485ef883b0f99b4e1b9b5bf
ms.sourcegitcommit: 6f0e10e4ca61a1e5534b09c655fd35cdc6886c8a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2019
ms.locfileid: "74564869"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a>Dependencias del receptor de webhooks de ASP.NET

Microsoft ASP.NET webhooks está diseñado pensando en la inserción de dependencias. La mayoría de las dependencias del sistema se pueden reemplazar por implementaciones alternativas mediante un motor de inserción de dependencias.

Consulte [DependencyScopeExtensions](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) para obtener una lista de dependencias del receptor. Si no se ha registrado ninguna dependencia, se usa una implementación predeterminada. Consulte [ReceiverServices](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) para obtener una lista de las implementaciones predeterminadas.
