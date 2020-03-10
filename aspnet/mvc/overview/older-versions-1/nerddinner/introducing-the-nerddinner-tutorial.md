---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Introducción al tutorial de NerdDinner | Microsoft Docs
author: shanselman
description: La mejor manera de aprender un nuevo marco es crear algo con él. En este tutorial se explica cómo crear una aplicación pequeña, pero completa, con ASP.NE...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 154cfe6694cf723c0a1f8e33bfdb42c97594518f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78468937"
---
# <a name="introducing-the-nerddinner-tutorial"></a>Introducción al tutorial de NerdDinner

por [Scott Hanselman](https://github.com/shanselman)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> La mejor manera de aprender un nuevo marco es crear algo con él. En este tutorial se explica cómo crear una aplicación pequeña, pero completa, que use ASP.NET MVC 1, y se presentan algunos de los conceptos principales que hay detrás.
> 
> Si usa ASP.NET MVC 3, se recomienda seguir los tutoriales [de la introducción con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o MVC [Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-tutorial"></a>Tutorial de NerdDinner

La mejor manera de aprender un nuevo marco es crear algo con él. En este tutorial se explica cómo crear una aplicación pequeña, pero completa, con ASP.NET MVC, y se presentan algunos de los conceptos principales que hay detrás.

La aplicación que se va a compilar se denomina "NerdDinner". NerdDinner proporciona a los usuarios una manera fácil de buscar y organizar las cenas en línea:

![](introducing-the-nerddinner-tutorial/_static/image1.png)

NerdDinner permite a los usuarios registrados crear, editar y eliminar cenas. Aplica un conjunto coherente de reglas de validación y de negocios a través de la aplicación:

![](introducing-the-nerddinner-tutorial/_static/image2.png)

Los visitantes pueden usar un mapa basado en AJAX para buscar las siguientes cenas que se mantienen cerca de ellas:

![](introducing-the-nerddinner-tutorial/_static/image3.png)

Al hacer clic en una cena, se le llevará a una página de detalles donde podrá obtener más información al respecto:

![](introducing-the-nerddinner-tutorial/_static/image4.png)

Si están interesados en asistir a la cena, pueden iniciar sesión o registrarse en el sitio:

![](introducing-the-nerddinner-tutorial/_static/image5.png)

Después, pueden hacer clic en un vínculo RSVP basado en AJAX para asistir al evento:

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a>Implementación de NerdDinner

Vamos a comenzar nuestra aplicación NerdDinner con el comando archivo-&gt;nuevo proyecto dentro de Visual Studio para crear un nuevo proyecto de ASP.NET MVC de marca. A continuación, agregaremos funcionalidades y características de forma incremental. A lo largo del proceso, trataremos:

1. [Cómo crear un nuevo proyecto de ASP.NET MVC](create-a-new-aspnet-mvc-project.md)
2. [Cómo crear una base de datos](create-a-database.md)
3. [Cómo crear un modelo con validaciones de reglas de negocios](build-a-model-with-business-rule-validations.md)
4. [Cómo usar controladores y vistas para implementar una interfaz de usuario de lista/detalles](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
5. [Cómo proporcionar compatibilidad con la entrada de formulario de datos CRUD (creación, lectura, actualización y eliminación)](provide-crud-create-read-update-delete-data-form-entry-support.md)
6. [Cómo usar ViewData e implementar clases ViewModel](use-viewdata-and-implement-viewmodel-classes.md)
7. [Cómo volver a usar la interfaz de usuario con páginas maestras y parciales](re-use-ui-using-master-pages-and-partials.md)
8. [Cómo implementar la paginación de datos eficaz](implement-efficient-data-paging.md)
9. [Cómo proteger las aplicaciones mediante la autenticación y la autorización](secure-applications-using-authentication-and-authorization.md)
10. [Cómo usar AJAX para ofrecer actualizaciones dinámicas](use-ajax-to-deliver-dynamic-updates.md)
11. [Cómo usar AJAX para implementar escenarios de asignación](use-ajax-to-implement-mapping-scenarios.md)
12. [Cómo habilitar las pruebas unitarias automatizadas](enable-automated-unit-testing.md)

Puede crear su propia copia de NerdDinner desde cero completando cada paso que se tutorial en este capítulo. Como alternativa, puede descargar una versión completada del código fuente aquí: [NerdDinner en github](https://github.com/AspNetMVPSamples/NerdDinner). También puede descargar opcionalmente [una versión en PDF gratuita de este tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) si desea leer el tutorial sin conexión.

Puede usar Visual Studio 2008 o Visual Web Developer 2008 Express gratuito para compilar la aplicación. Puede usar SQL Server o el SQL Server Express gratuito para la base de datos.

Puede instalar ASP.NET MVC, Visual Web Developer 2008 Express y SQL Server Express (todo gratis) mediante la versión 2 de la [instalador de plataforma web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)

### <a name="now-lets-get-started"></a>Ahora vamos a empezar....

Ahora que hemos analizado qué es NerdDinner, vamos a acumular nuestras mangas y a escribir código.

Comenzaremos usando el archivo&gt;nuevo proyecto dentro de Visual Studio para crear la aplicación NerdDinner.

> [!div class="step-by-step"]
> [Siguiente](create-a-new-aspnet-mvc-project.md)
