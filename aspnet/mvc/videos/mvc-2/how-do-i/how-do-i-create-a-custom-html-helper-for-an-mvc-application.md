---
uid: mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
title: 'Cómo: crear una aplicación auxiliar HTML personalizada para una aplicación MVC | Microsoft Docs'
author: rick-anderson
description: En este vídeo, Chris Pels muestra cómo crear una HtmlHelper personalizada que no está disponible en el conjunto estándar en una aplicación MVC. En primer lugar, un ejemplo de aplicaciones MVC...
ms.author: riande
ms.date: 12/11/2009
ms.assetid: 58b5eb15-4160-4ce2-ae70-6ba94262ea73
msc.legacyurl: /mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
msc.type: video
ms.openlocfilehash: 60953243d3038667e4f729b1394e68f0c9d7c178
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78450481"
---
# <a name="how-do-i-create-a-custom-html-helper-for-an-mvc-application"></a>Cómo: crear una aplicación auxiliar HTML personalizada para una aplicación MVC

por [Chris Pels](https://twitter.com/chrispels)

En este vídeo, Chris Pels muestra cómo crear una HtmlHelper personalizada que no está disponible en el conjunto estándar en una aplicación MVC. En primer lugar, se crea una aplicación MVC de ejemplo con un controlador de demostración y una vista para probar el HtmlHelper personalizado. A continuación, se crea un módulo con una función pública que es un método de extensión que representa la implementación de HtmlHelper personalizado. El ayudante personalizado es para crear `<img>` etiquetas en una página y recibe varios parámetros de entrada, como el identificador, la dirección URL y el texto alternativo de la etiqueta de imagen. A continuación, se agrega la lógica a la función para devolver la etiqueta `<img>` completada con la información especificada. A continuación, se usa el HtmlHelper personalizado en la página de demostración para mostrar una imagen. Por último, el HtmlHelper personalizado se expande para incluir varias invalidaciones de constructor que proporcionan flexibilidad para crear más fácilmente etiquetas de `<img>` diferentes.

[&#9654;Ver vídeo (18 minutos)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-create-a-custom-html-helper-for-an-mvc-application)

> [!div class="step-by-step"]
> [Anterior](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
> [Siguiente](how-do-i-work-with-model-binders-in-an-mvc-application.md)
