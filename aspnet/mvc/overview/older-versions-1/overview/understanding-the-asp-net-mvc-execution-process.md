---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Descripción del proceso de ejecución de ASP.NET MVC | Microsoft Docs
author: microsoft
description: Obtenga información sobre cómo el marco ASP.NET MVC procesa una solicitud de explorador paso a paso.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 28940947253e0af43886cf1231f8aaf4615526cc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78435457"
---
# <a name="understanding-the-aspnet-mvc-execution-process"></a>Descripción del proceso de ejecución de ASP.NET MVC

por [Microsoft](https://github.com/microsoft)

> Obtenga información sobre cómo el marco ASP.NET MVC procesa una solicitud de explorador paso a paso.

Las solicitudes a una aplicación web basada en MVC de ASP.NET primero pasan a través del objeto **UrlRoutingModule** , que es un módulo http. Este módulo analiza la solicitud y realiza la selección de la ruta. El objeto **UrlRoutingModule** selecciona el primer objeto de ruta que coincide con la solicitud actual. (Un objeto de ruta es una clase que implementa **RouteBase**y normalmente es una instancia de la clase **Route** ). Si no coinciden las rutas, el objeto **UrlRoutingModule** no realiza ninguna acción y permite a la solicitud revertir al procesamiento de solicitudes ASP.net o IIS normal.

En el objeto **Route** seleccionado, el objeto **UrlRoutingModule** obtiene el objeto **IRouteHandler** asociado al objeto **Route** . Normalmente, en una aplicación MVC, se trata de una instancia de **MvcRouteHandler**. La instancia de **IRouteHandler** crea un objeto **IHttpHandler** y lo pasa al objeto **IHttpContext** . De forma predeterminada, la instancia de **IHttpHandler** para MVC es el objeto **mvchandler** . A continuación, el objeto **mvchandler** selecciona el controlador que, en última instancia, administrará la solicitud.

> [!NOTE]
> Cuando una aplicación web ASP.NET MVC se ejecuta en IIS 7.0, no se requiere una extensión de nombre de archivo para los proyectos de MVC. Sin embargo, en IIS 6.0, el controlador requiere que asigne la extensión de nombre de archivo .mvc a la DLL de ISAPI de ASP.NET.

El módulo y el controlador son la entrada que apunta al marco de MVC de ASP.NET. Realizan las siguientes acciones:

- Seleccionar el controlador adecuado en una aplicación web MVC.
- Obtener una instancia del controlador concreta.
- Llame al método **Execute** del controlador.

A continuación se enumeran las fases de ejecución de un proyecto Web de MVC:

- Recibir la primera solicitud para la aplicación 

    - En el archivo global. asax, los objetos de **ruta** se agregan al objeto **RouteTable** .
- Realizar el enrutamiento 

    - El **módulo UrlRoutingModule** usa el primer objeto de **ruta** coincidente en la colección **RouteTable** para crear el objeto **RouteData** , que utiliza a continuación para crear un objeto **RequestContext** (**IHttpContext**).
- Crear el controlador de solicitudes de MVC 

    - El objeto **MvcRouteHandler** crea una instancia de la clase **mvchandler** y la pasa a la instancia **RequestContext** .
- Crear el controlador 

    - El objeto **mvchandler** usa la instancia **RequestContext** para identificar el objeto **IControllerFactory** (normalmente una instancia de la clase **DefaultControllerFactory** ) con el que crear la instancia del controlador.
- Ejecutar controlador: la instancia de **mvchandler** llama al método de **ejecución** del controlador. |
- Invocar la acción 

    - La mayoría de los controladores heredan de la clase base del **controlador** . En el caso de los controladores que lo hacen, el objeto **ControllerActionInvoker** asociado al controlador determina qué método de acción de la clase de controlador debe llamar y, a continuación, llama a ese método.
- Ejecutar el resultado 

    - Un método de acción típico podría recibir la entrada del usuario, preparar los datos de respuesta adecuados y, a continuación, ejecutar el resultado devolviendo un tipo de resultado. Los tipos de resultado integrados que se pueden ejecutar son los siguientes: **ViewResult** (que representa una vista y es el tipo de resultado utilizado con mayor frecuencia), **RedirectToRouteResult**, **RedirectResult**, **ContentResult**, **JsonResult**y **EmptyResult**.
