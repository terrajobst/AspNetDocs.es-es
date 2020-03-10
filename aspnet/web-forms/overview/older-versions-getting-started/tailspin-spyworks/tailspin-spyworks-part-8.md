---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Parte 8: páginas finales, control de excepciones y conclusión | Microsoft Docs'
author: JoeStagner
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks. La parte 8 agrega una página de contacto, acerca de la página y la excepción...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 707dc9d87ae324a7897c971a451e40bc54c96cb3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78474343"
---
# <a name="part-8-final-pages-exception-handling-and-conclusion"></a>Parte 8: páginas finales, control de excepciones y conclusión

por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks muestra cómo es extraordinariamente simple la creación de aplicaciones eficaces y escalables para la plataforma .NET. Muestra cómo usar las excelentes características nuevas de ASP.NET 4 para crear una tienda en línea, como compras, desprotección y administración.
> 
> En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks. La parte 8 agrega una página de contacto, about Page y control de excepciones. Esta es la conclusión de la serie.

## <a id="_Toc260221680"></a>Página de contacto (envío de correo electrónico desde ASP.NET)

Cree una nueva página denominada contactus. aspx

Con el diseñador, cree el siguiente formulario tomando nota especial para incluir ToolkitScriptManager y el control de editor de AjaxControlToolkit. .

![](tailspin-spyworks-part-8/_static/image1.jpg)

Haga doble clic en el botón "enviar" para generar un controlador de eventos de clic en el archivo de código subyacente e implementar un método para enviar la información de contacto como un correo electrónico.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

Este código requiere que el archivo Web. config contenga una entrada en la sección de configuración que especifique el servidor SMTP que se va a usar para enviar correo.

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>Página acerca de

Cree una página denominada aboutUs. aspx y agregue el contenido que desee.

## <a id="_Toc260221682"></a>Controlador de excepciones global

Por último, a lo largo de la aplicación hemos producido excepciones y existen circunstancias imprevistas que también causan excepciones no controladas en nuestra aplicación Web.

Nunca queremos que se muestre una excepción no controlada en el visitante de un sitio Web.

![](tailspin-spyworks-part-8/_static/image2.jpg)

Además de ser una experiencia de usuario muy terrible, las excepciones no controladas también pueden ser un problema de seguridad.

Para solucionar este problema, implementaremos un controlador de excepciones global.

Para ello, abra el archivo global. asax y observe el siguiente controlador de eventos generado previamente.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

Agregue código para implementar el controlador de errores de\_de la aplicación como se indica a continuación.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

Después, agregue una página denominada error. aspx a la solución y agregue este fragmento de código de marcado.

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

Ahora, en la página\_controlador de eventos de carga Extraiga los mensajes de error del objeto de solicitud.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>Celebre

Hemos visto que los WebForms de ASP.NET facilitan la creación de un sitio web sofisticado con acceso a bases de datos, pertenencia, AJAX, etc. con bastante rapidez.

Espero que este tutorial le haya proporcionado las herramientas que necesita para empezar a crear sus propias aplicaciones de WebForms de ASP.NET.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-7.md)
