---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Instalación de una aplicación auxiliar en un sitio de ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: En este artículo se describe cómo instalar una aplicación auxiliar en un sitio web de ASP.NET Web Pages (Razor). Una aplicación auxiliar es un componente reutilizable que incluye código y marcado a por...
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 41e33c04a53a6ad257c3937cdadcec767e9217c8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78518647"
---
# <a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a>Instalación de una aplicación auxiliar en un sitio de ASP.NET Web Pages (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se describe cómo instalar una aplicación auxiliar en un sitio web de ASP.NET Web Pages (Razor). Una *aplicación auxiliar* es un componente reutilizable que incluye código y marcado para realizar una tarea que podría ser tediosa o compleja.
> 
> Temas que se abordarán:
> 
> - Cómo instalar una aplicación auxiliar en un sitio web creado mediante WebMatrix 3.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - WebMatrix 3

## <a name="overview-of-helpers"></a>Información general de las aplicaciones auxiliares

Algunas tareas que los usuarios suelen querer realizar en páginas Web requieren mucho código o requieren conocimientos adicionales. Entre los ejemplos se incluye la presentación de un gráfico para los datos; colocar un botón "seguir" de Twitter en una página; enviar correo electrónico desde su sitio web; recortar o cambiar el tamaño de las imágenes; usar PayPal para su sitio. Para facilitar la creación de estos tipos de cosas, ASP.NET Web Pages le permite usar *aplicaciones auxiliares*. Las aplicaciones auxiliares son componentes que se instalan para un sitio y que permiten realizar tareas típicas usando solo una línea o dos de código Razor.

ASP.NET Web Pages tiene algunas aplicaciones auxiliares integradas. Sin embargo, hay muchas aplicaciones auxiliares disponibles en paquetes (complementos) que se proporcionan mediante el administrador de paquetes NuGet. NuGet le permite seleccionar un paquete para instalarlo y, a continuación, se ocupa de todos los detalles de la instalación.

## <a name="installing-a-helper-in-webmatrix-3"></a>Instalación de una aplicación auxiliar en WebMatrix 3

1. En WebMatrix 3, haga clic en el botón **NuGet** .

    ![Cuadro de diálogo Galería de NuGet en WebMatrix](installing-helpers/_static/image1.png)
2. Se inicia el administrador de paquetes NuGet y se muestran los paquetes disponibles. En el cuadro de búsqueda, escriba una palabra clave para el ayudante que desea instalar.

    ![Cuadro de diálogo Galería de NuGet en WebMatrix](installing-helpers/_static/image2.png)
3. Seleccione el paquete y, a continuación, haga clic en **instalar**. Haga clic en **sí** cuando se le pregunte si desea instalar el paquete e indicar que acepta los términos.

     Si es la primera vez que instala una aplicación auxiliar, NuGet crea carpetas en el sitio web para el código que constituye el ayudante.
4. Para desinstalar una aplicación auxiliar, haga clic en el botón **Galería** , haga clic en la pestaña **instalado** y seleccione el paquete que desea desinstalar.

## <a name="installing-the-twitter-helper"></a>Instalación de la aplicación auxiliar de Twitter

La versión más reciente de la API de Twitter no es compatible con la aplicación auxiliar de Twitter que se instala a través de NuGet. En su lugar, consulte el tema [aplicación auxiliar de Twitter con WebMatrix](twitter-helper.md) para obtener información sobre cómo configurar la aplicación auxiliar de Twitter en el proyecto.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales

[Introducción a ASP.NET Web Pages 2: aspectos básicos de la programación](../getting-started/introducing-razor-syntax-c.md)

[Aplicación auxiliar de Twitter con WebMatrix](twitter-helper.md)
