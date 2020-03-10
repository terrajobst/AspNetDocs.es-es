---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Vistas dinámicas frente a las Vistas fuertemente tipadas | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/27/2011
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 3e81c6381b1e280e3b74cb7eb6ea6e6c3224e655
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78432577"
---
# <a name="dynamic-v-strongly-typed-views"></a>Vistas dinámicas frente a las vistas fuertemente tipadas

por [Rick Anderson](https://twitter.com/RickAndMSFT)

Hay tres maneras de pasar información de un controlador a una vista en ASP.NET MVC 3:

1. Como un objeto de modelo fuertemente tipado.
2. Como tipo dinámico (mediante @model dinámico)
3. Usar ViewBag

He escrito una aplicación sencilla de blog de MVC 3 para comparar y contrastar las vistas dinámicas y fuertemente tipadas. El controlador empieza con una sencilla lista de blogs:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

Haga clic con el botón derecho en el método IndexNotStonglyTyped () y agregue una vista de Razor.

[![8475. NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)

Asegúrese de que la casilla **crear una vista fuertemente tipada** no esté activada. La vista resultante no contiene muchos:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

Como estamos usando una vista dinámica y no fuertemente tipada, IntelliSense no nos ayuda. A continuación se muestra el código completado:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

[![6646. NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)

Ahora vamos a agregar una vista fuertemente tipada. Agregue el código siguiente al controlador:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]

Observe que es exactamente la misma vista de devolución (blog). Llame a como la vista sin establecimiento inflexible de tipos. Haga clic con el botón derecho dentro de *StonglyTypedIndex ()* y seleccione **Agregar vista**. Esta vez, seleccione la clase de modelo de **blog** y seleccione **lista** como plantilla scaffolding.

[![5658. StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)

Dentro de la nueva plantilla de vista obtenemos compatibilidad con IntelliSense.

[![7002. IntelliSense [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)

El proyecto de c# se puede descargar [aquí](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).
