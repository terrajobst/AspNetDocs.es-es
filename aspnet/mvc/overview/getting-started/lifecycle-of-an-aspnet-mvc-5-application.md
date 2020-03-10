---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: Ciclo de vida de una aplicación ASP.NET MVC 5 | Microsoft Docs
author: cephalin
description: Descargue un documento PDF que gráfico del ciclo de vida de una aplicación ASP.NET MVC 5. Este documento del ciclo de vida proporciona una vista de alto nivel del ciclo de vida de MVC.
ms.author: riande
ms.date: 02/28/2014
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: f4a9b3fb61552b070db11fba617b5627fcd71cd5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78470329"
---
# <a name="lifecycle-of-an-aspnet-mvc-5-application"></a>Ciclo de vida de una aplicación de ASP.NET MVC 5

por [Cephas](https://github.com/cephalin)

[Descargar documento PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

Aquí puede descargar un documento PDF que realiza un gráfico del ciclo de vida de cada aplicación ASP.NET MVC 5, desde la recepción de la solicitud HTTP al envío de la respuesta HTTP al cliente. Está diseñada como herramienta educativa para quienes no están familiarizados con ASP.NET MVC y también como una referencia para aquellos que necesitan profundizar en aspectos específicos de la aplicación. El documento PDF tiene las siguientes características:

- Fases de [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) relevantes para ayudarle a entender dónde se integra MVC en el ciclo de vida de la [aplicación ASP.net](https://msdn.microsoft.com/library/bb470252.aspx).
- Una vista de alto nivel del ciclo de vida de la aplicación MVC, donde puede comprender las fases principales que pasa cada aplicación MVC en la canalización de procesamiento de solicitudes.  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- Una vista de detalle que muestra los detalles de la canalización de procesamiento de solicitudes. Puede comparar la vista de alto nivel y la vista de detalle para ver cómo se recopilan los detalles de los ciclos de vida en las distintas fases. [Descargue PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) para ver una vista más grande.
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- Ubicación y propósito de todos los métodos reemplazables en el objeto de [controlador](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) en la canalización de procesamiento de solicitudes. Es posible que tenga o no la necesidad de invalidar cualquier método, pero es importante que comprenda su rol en el ciclo de vida de la aplicación para que pueda escribir código en la fase del ciclo de vida adecuado para el efecto que desea.
- Diagramas de fundido que muestran cómo se invoca cada uno de los tipos de filtro (autenticación, autorización, acción y resultado).
- Vínculo a un artículo o blog útil de cada punto de interés en la vista de detalle.

## <a name="next-steps"></a>Pasos siguientes

¿Este documento satisface sus necesidades? Agradecemos sus comentarios. Si tiene alguna pregunta sobre el ciclo de vida de ASP.NET MVC en su [aplicación,](http://stackoverflow.com/help) los [foros de ASP.net mvc](https://forums.asp.net/1146.aspx) y de son excelentes. Síganos en Twitter para que pueda obtener [actualizaciones en los](https://twitter.com/Cephas_MSFT) tutoriales más recientes.
