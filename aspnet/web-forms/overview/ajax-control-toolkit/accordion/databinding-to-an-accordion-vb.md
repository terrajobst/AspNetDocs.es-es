---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
title: Enlace de enlaces a Accordion (VB) | Microsoft Docs
author: wenz
description: El control Accordion en el kit de herramientas de control de AJAX proporciona varios paneles y permite al usuario mostrar uno de ellos a la vez. Normalmente, los paneles se declaran...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b19f0875-7d3e-4ecf-baa1-a0c693c765b3
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
msc.type: authoredcontent
ms.openlocfilehash: bfb31940c0395c7ed1d5d471fb8fb686b66c59ad
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78498031"
---
# <a name="databinding-to-an-accordion-vb"></a><span data-ttu-id="77b16-104">Enlace de datos a Accordion (VB)</span><span class="sxs-lookup"><span data-stu-id="77b16-104">Databinding to an Accordion (VB)</span></span>

<span data-ttu-id="77b16-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="77b16-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="77b16-106">[Descargar código](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="77b16-106">[Download Code](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1VB.pdf)</span></span>

> <span data-ttu-id="77b16-107">El control Accordion en el kit de herramientas de control de AJAX proporciona varios paneles y permite al usuario mostrar uno de ellos a la vez.</span><span class="sxs-lookup"><span data-stu-id="77b16-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="77b16-108">Los paneles se declaran normalmente dentro de la propia página, pero el enlace a un origen de datos ofrece más flexibilidad.</span><span class="sxs-lookup"><span data-stu-id="77b16-108">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>

## <a name="overview"></a><span data-ttu-id="77b16-109">Información general</span><span class="sxs-lookup"><span data-stu-id="77b16-109">Overview</span></span>

<span data-ttu-id="77b16-110">El control Accordion en el kit de herramientas de control de AJAX proporciona varios paneles y permite al usuario mostrar uno de ellos a la vez.</span><span class="sxs-lookup"><span data-stu-id="77b16-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="77b16-111">Los paneles se declaran normalmente dentro de la propia página, pero el enlace a un origen de datos ofrece más flexibilidad.</span><span class="sxs-lookup"><span data-stu-id="77b16-111">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="77b16-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="77b16-112">Steps</span></span>

<span data-ttu-id="77b16-113">En primer lugar, se necesita un origen de datos.</span><span class="sxs-lookup"><span data-stu-id="77b16-113">First of all, a data source is required.</span></span> <span data-ttu-id="77b16-114">Este ejemplo utiliza la base de datos AdventureWorks y el Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="77b16-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="77b16-115">La base de datos es una parte opcional de una instalación de Visual Studio (incluida Express Edition) y también está disponible como descarga independiente en [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="77b16-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="77b16-116">La base de datos AdventureWorks forma parte de los ejemplos de SQL Server 2005 y de las bases de datos de ejemplo (descarga en [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="77b16-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="77b16-117">La forma más sencilla de configurar la base de datos es usar el Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) y adjuntar el archivo de base de datos `AdventureWorks.mdf`.</span><span class="sxs-lookup"><span data-stu-id="77b16-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="77b16-118">En este ejemplo, se supone que la instancia del SQL Server 2005 Express Edition se denomina `SQLEXPRESS` y reside en el mismo equipo que el servidor Web. Esta es también la configuración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="77b16-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="77b16-119">Si el programa de instalación es diferente, tendrá que adaptar la información de conexión de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="77b16-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="77b16-120">Para activar la funcionalidad de ASP.NET AJAX y control Toolkit, el control `ScriptManager` debe colocarse en cualquier parte de la página (pero dentro del elemento `<form>`):</span><span class="sxs-lookup"><span data-stu-id="77b16-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample1.aspx)]

<span data-ttu-id="77b16-121">A continuación, agregue un origen de datos a la página.</span><span class="sxs-lookup"><span data-stu-id="77b16-121">Then, add a data source to the page.</span></span> <span data-ttu-id="77b16-122">Para usar una cantidad limitada de datos, solo se seleccionan las cinco primeras entradas de la tabla Vendor de la base de datos AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="77b16-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="77b16-123">Si usa el Asistente de Visual Studio para crear el origen de datos, tenga en cuenta que un error en la versión actual no antepone el nombre de tabla (`Vendor`) con `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="77b16-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="77b16-124">En el marcado siguiente se muestra la sintaxis correcta:</span><span class="sxs-lookup"><span data-stu-id="77b16-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample2.aspx)]

<span data-ttu-id="77b16-125">Recuerde el nombre (ID.) del origen de datos.</span><span class="sxs-lookup"><span data-stu-id="77b16-125">Remember the name (ID) of the data source.</span></span> <span data-ttu-id="77b16-126">Esta identificación se debe usar en la `DataSourceID` propiedad del control Accordion:</span><span class="sxs-lookup"><span data-stu-id="77b16-126">This very identification must then be used in the `DataSourceID` property of the Accordion control:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample3.aspx)]

<span data-ttu-id="77b16-127">Dentro del control Accordion, puede proporcionar plantillas para varias partes del control, incluido el encabezado (`<HeaderTemplate>`) y el contenido (`<ContentTemplate>`).</span><span class="sxs-lookup"><span data-stu-id="77b16-127">Within the Accordion control, you can provide templates for various parts of the control, including the header (`<HeaderTemplate>`) and the content (`<ContentTemplate>`).</span></span> <span data-ttu-id="77b16-128">Dentro de estos elementos, basta con que se generen los datos del origen de datos mediante el método `DataBinder.Eval()`:</span><span class="sxs-lookup"><span data-stu-id="77b16-128">Within these elements, just output the data from the data source, using the `DataBinder.Eval()` method:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample4.aspx)]

<span data-ttu-id="77b16-129">Cuando se carga la página, el origen de datos debe estar enlazado a la Accordion con este código del servidor:</span><span class="sxs-lookup"><span data-stu-id="77b16-129">When the page is loaded, the data source must be bound to the accordion with this server-side code:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample5.aspx)]

<span data-ttu-id="77b16-130">Para concluir este ejemplo, debe definir las dos clases CSS a las que se hace referencia en el control Accordion (en sus propiedades `HeaderCssClass` y `ContentCssClass`).</span><span class="sxs-lookup"><span data-stu-id="77b16-130">To conclude this sample, you need to define the two CSS classes that are referenced in the Accordion control (in its properties `HeaderCssClass` and `ContentCssClass`).</span></span> <span data-ttu-id="77b16-131">Coloque el marcado siguiente en la sección `<head>` de la página:</span><span class="sxs-lookup"><span data-stu-id="77b16-131">Put the following markup in the `<head>` section of the page:</span></span>

[!code-css[Main](databinding-to-an-accordion-vb/samples/sample6.css)]

<span data-ttu-id="77b16-132">[![los datos de la Accordion proceden directamente del origen de datos](databinding-to-an-accordion-vb/_static/image2.png)](databinding-to-an-accordion-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="77b16-132">[![The data in the accordion comes directly from the data source](databinding-to-an-accordion-vb/_static/image2.png)](databinding-to-an-accordion-vb/_static/image1.png)</span></span>

<span data-ttu-id="77b16-133">Los datos de la Accordion proceden directamente del origen de datos ([haga clic para ver la imagen de tamaño completo](databinding-to-an-accordion-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="77b16-133">The data in the accordion comes directly from the data source ([Click to view full-size image](databinding-to-an-accordion-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="77b16-134">[Anterior](dynamically-adding-an-accordion-pane-cs.md)
> [Siguiente](dynamically-adding-an-accordion-pane-vb.md)</span><span class="sxs-lookup"><span data-stu-id="77b16-134">[Previous](dynamically-adding-an-accordion-pane-cs.md)
[Next](dynamically-adding-an-accordion-pane-vb.md)</span></span>
