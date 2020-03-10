---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
title: Enlace de enlaces a Accordion (C#) | Microsoft Docs
author: wenz
description: El control Accordion en el kit de herramientas de control de AJAX proporciona varios paneles y permite al usuario mostrar uno de ellos a la vez. Normalmente, los paneles se declaran...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 9c8f0054-e319-46f8-80c0-35b606d2fbd4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
msc.type: authoredcontent
ms.openlocfilehash: 6c28cc958a1de9844627ae16175a5aed153993a8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78498061"
---
# <a name="databinding-to-an-accordion-c"></a>Enlace de datos a Accordion (C#)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)

> El control Accordion en el kit de herramientas de control de AJAX proporciona varios paneles y permite al usuario mostrar uno de ellos a la vez. Los paneles se declaran normalmente dentro de la propia página, pero el enlace a un origen de datos ofrece más flexibilidad.

## <a name="overview"></a>Información general

El control Accordion en el kit de herramientas de control de AJAX proporciona varios paneles y permite al usuario mostrar uno de ellos a la vez. Los paneles se declaran normalmente dentro de la propia página, pero el enlace a un origen de datos ofrece más flexibilidad.

## <a name="steps"></a>Pasos

En primer lugar, se necesita un origen de datos. Este ejemplo utiliza la base de datos AdventureWorks y el Microsoft SQL Server 2005 Express Edition. La base de datos es una parte opcional de una instalación de Visual Studio (incluida Express Edition) y también está disponible como descarga independiente en [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). La base de datos AdventureWorks forma parte de los ejemplos de SQL Server 2005 y de las bases de datos de ejemplo (descarga en [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). La forma más sencilla de configurar la base de datos es usar el Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) y adjuntar el archivo de base de datos `AdventureWorks.mdf`.

En este ejemplo, se supone que la instancia del SQL Server 2005 Express Edition se denomina `SQLEXPRESS` y reside en el mismo equipo que el servidor Web. Esta es también la configuración predeterminada. Si el programa de instalación es diferente, tendrá que adaptar la información de conexión de la base de datos.

Para activar la funcionalidad de ASP.NET AJAX y control Toolkit, el control `ScriptManager` debe colocarse en cualquier parte de la página (pero dentro del elemento `<form>`):

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample1.aspx)]

A continuación, agregue un origen de datos a la página. Para usar una cantidad limitada de datos, solo se seleccionan las cinco primeras entradas de la tabla Vendor de la base de datos AdventureWorks. Si usa el Asistente de Visual Studio para crear el origen de datos, tenga en cuenta que un error en la versión actual no antepone el nombre de tabla (`Vendor`) con `Purchasing`. En el marcado siguiente se muestra la sintaxis correcta:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample2.aspx)]

Recuerde el nombre (ID.) del origen de datos. Esta identificación se debe usar en la `DataSourceID` propiedad del control Accordion:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample3.aspx)]

Dentro del control Accordion, puede proporcionar plantillas para varias partes del control, incluido el encabezado (`<HeaderTemplate>`) y el contenido (`<ContentTemplate>`). Dentro de estos elementos, basta con que se generen los datos del origen de datos mediante el método `DataBinder.Eval()`:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample4.aspx)]

Cuando se carga la página, el origen de datos debe estar enlazado a la Accordion con este código del servidor:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample5.aspx)]

Para concluir este ejemplo, debe definir las dos clases CSS a las que se hace referencia en el control Accordion (en sus propiedades `HeaderCssClass` y `ContentCssClass`). Coloque el marcado siguiente en la sección `<head>` de la página:

[!code-css[Main](databinding-to-an-accordion-cs/samples/sample6.css)]

[![los datos de la Accordion proceden directamente del origen de datos](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)

Los datos de la Accordion proceden directamente del origen de datos ([haga clic para ver la imagen de tamaño completo](databinding-to-an-accordion-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Siguiente](dynamically-adding-an-accordion-pane-cs.md)
