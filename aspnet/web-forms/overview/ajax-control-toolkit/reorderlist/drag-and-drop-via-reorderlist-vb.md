---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
title: Arrastrar y colocar mediante ReorderList (VB) | Microsoft Docs
author: wenz
description: /data-access/tutorials/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 848e6bcf-4c3f-4d14-974d-e45b9444ab79
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 3f7c5749053d8bf587467fb1939fca05ce2872a4
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598636"
---
# <a name="drag-and-drop-via-reorderlist-vb"></a><span data-ttu-id="17a0e-103">Arrastrar y colocar mediante ReorderList (VB)</span><span class="sxs-lookup"><span data-stu-id="17a0e-103">Drag and Drop via ReorderList (VB)</span></span>

<span data-ttu-id="17a0e-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="17a0e-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="17a0e-105">[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="17a0e-105">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span></span>

> <span data-ttu-id="17a0e-106">El control ReorderList en el kit de herramientas de control de AJAX proporciona una lista que el usuario puede reordenar mediante arrastrar y colocar.</span><span class="sxs-lookup"><span data-stu-id="17a0e-106">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="17a0e-107">El orden actual de la lista se conservará en el servidor.</span><span class="sxs-lookup"><span data-stu-id="17a0e-107">The current order of the list shall be persisted on the server.</span></span>

## <a name="overview"></a><span data-ttu-id="17a0e-108">Información general del</span><span class="sxs-lookup"><span data-stu-id="17a0e-108">Overview</span></span>

<span data-ttu-id="17a0e-109">El control `ReorderList` en el kit de herramientas de control de AJAX proporciona una lista que el usuario puede reordenar mediante arrastrar y colocar.</span><span class="sxs-lookup"><span data-stu-id="17a0e-109">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="17a0e-110">El orden actual de la lista se conservará en el servidor.</span><span class="sxs-lookup"><span data-stu-id="17a0e-110">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="17a0e-111">Pasos</span><span class="sxs-lookup"><span data-stu-id="17a0e-111">Steps</span></span>

<span data-ttu-id="17a0e-112">El control `ReorderList` admite el enlace de datos de una base de datos a la lista.</span><span class="sxs-lookup"><span data-stu-id="17a0e-112">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="17a0e-113">Lo mejor de todo es que también admite la escritura de cambios en el orden del elemento de lista de nuevo en el almacén de datos.</span><span class="sxs-lookup"><span data-stu-id="17a0e-113">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="17a0e-114">En este ejemplo se utiliza Microsoft SQL Server 2005 Express Edition como almacén de datos.</span><span class="sxs-lookup"><span data-stu-id="17a0e-114">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="17a0e-115">La base de datos es una parte opcional (y gratuita) de una instalación de Visual Studio, incluida la edición Express.</span><span class="sxs-lookup"><span data-stu-id="17a0e-115">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="17a0e-116">También está disponible como descarga independiente en [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="17a0e-116">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="17a0e-117">En este ejemplo, se supone que la instancia del SQL Server 2005 Express Edition se denomina `SQLEXPRESS` y reside en el mismo equipo que el servidor Web. Esta es también la configuración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="17a0e-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="17a0e-118">Si el programa de instalación es diferente, tendrá que adaptar la información de conexión de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="17a0e-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="17a0e-119">La forma más sencilla de configurar la base de datos es usar el Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span><span class="sxs-lookup"><span data-stu-id="17a0e-119">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="17a0e-120">Conéctese al servidor, haga doble clic en `Databases` y cree una nueva base de datos (haga clic con el botón derecho y seleccione `New Database`) denominada `Tutorials`.</span><span class="sxs-lookup"><span data-stu-id="17a0e-120">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="17a0e-121">En esta base de datos, cree una nueva tabla llamada `AJAX` con las cuatro columnas siguientes:</span><span class="sxs-lookup"><span data-stu-id="17a0e-121">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- <span data-ttu-id="17a0e-122">`id` (clave principal, entero, identidad, no NULL)</span><span class="sxs-lookup"><span data-stu-id="17a0e-122">`id` (primary key, integer, identity, not NULL)</span></span>
- <span data-ttu-id="17a0e-123">`char` (Char (1), NULL)</span><span class="sxs-lookup"><span data-stu-id="17a0e-123">`char` (char(1), NULL)</span></span>
- <span data-ttu-id="17a0e-124">`description` (varchar (50), NULL)</span><span class="sxs-lookup"><span data-stu-id="17a0e-124">`description` (varchar(50), NULL)</span></span>
- <span data-ttu-id="17a0e-125">`position` (int, NULL)</span><span class="sxs-lookup"><span data-stu-id="17a0e-125">`position` (int, NULL)</span></span>

<span data-ttu-id="17a0e-126">[![del diseño de la tabla de AJAX](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="17a0e-126">[![The layout of the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="17a0e-127">El diseño de la tabla de AJAX ([haga clic para ver la imagen de tamaño completo](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="17a0e-127">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span></span>

<span data-ttu-id="17a0e-128">A continuación, rellene la tabla con un par de valores.</span><span class="sxs-lookup"><span data-stu-id="17a0e-128">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="17a0e-129">Tenga en cuenta que la columna `position` contiene el criterio de ordenación de los elementos.</span><span class="sxs-lookup"><span data-stu-id="17a0e-129">Note that the `position` column holds the sort order of the elements.</span></span>

<span data-ttu-id="17a0e-130">[![los datos iniciales de la tabla de AJAX](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="17a0e-130">[![The initial data in the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span></span>

<span data-ttu-id="17a0e-131">Los datos iniciales de la tabla de AJAX ([haga clic para ver la imagen de tamaño completo](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="17a0e-131">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span></span>

<span data-ttu-id="17a0e-132">El paso siguiente requiere que se genere un control `SqlDataSource` para comunicarse con la nueva base de datos y su tabla.</span><span class="sxs-lookup"><span data-stu-id="17a0e-132">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="17a0e-133">El origen de datos debe admitir los comandos SQL `SELECT` y `UPDATE`.</span><span class="sxs-lookup"><span data-stu-id="17a0e-133">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="17a0e-134">Cuando se cambia el orden de los elementos de la lista, el control `ReorderList` envía automáticamente dos valores al comando `Update` del origen de datos: la nueva posición y el identificador del elemento.</span><span class="sxs-lookup"><span data-stu-id="17a0e-134">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="17a0e-135">Por lo tanto, el origen de datos necesita una sección `<UpdateParameters>` para estos dos valores:</span><span class="sxs-lookup"><span data-stu-id="17a0e-135">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="17a0e-136">El control `ReorderList` debe establecer los siguientes atributos:</span><span class="sxs-lookup"><span data-stu-id="17a0e-136">The `ReorderList` control needs to set the following attributes:</span></span>

- <span data-ttu-id="17a0e-137">`AllowReorder`: si se pueden reorganizar los elementos de la lista</span><span class="sxs-lookup"><span data-stu-id="17a0e-137">`AllowReorder`: Whether the list items may be rearranged</span></span>
- <span data-ttu-id="17a0e-138">`DataSourceID`: el identificador del origen de datos</span><span class="sxs-lookup"><span data-stu-id="17a0e-138">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="17a0e-139">`DataKeyField`: el nombre de la columna de clave principal del origen de datos</span><span class="sxs-lookup"><span data-stu-id="17a0e-139">`DataKeyField`: The name of the primary key column in the data source</span></span>
- <span data-ttu-id="17a0e-140">`SortOrderField`: la columna de origen de datos que proporciona el criterio de ordenación para los elementos de lista</span><span class="sxs-lookup"><span data-stu-id="17a0e-140">`SortOrderField`: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="17a0e-141">En las secciones `<DragHandleTemplate>` y `<ItemTemplate>`, se puede ajustar el diseño de la lista.</span><span class="sxs-lookup"><span data-stu-id="17a0e-141">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="17a0e-142">Además, el enlace de los DataBindings es posible mediante el método `Eval()`, como se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="17a0e-142">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="17a0e-143">La siguiente información de estilo CSS (a la que se hace referencia en la sección `<DragHandleTemplate>` del control de `ReorderList`) garantiza que el puntero del mouse cambie correctamente cuando se mantenga el puntero sobre el controlador de arrastre:</span><span class="sxs-lookup"><span data-stu-id="17a0e-143">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

<span data-ttu-id="17a0e-144">Por último, un control `ScriptManager` inicializa ASP.NET AJAX para la página:</span><span class="sxs-lookup"><span data-stu-id="17a0e-144">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="17a0e-145">Ejecute este ejemplo en el explorador y reorganice los elementos de la lista un poco.</span><span class="sxs-lookup"><span data-stu-id="17a0e-145">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="17a0e-146">A continuación, vuelva a cargar la página o examine la base de datos.</span><span class="sxs-lookup"><span data-stu-id="17a0e-146">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="17a0e-147">Las posiciones modificadas se han mantenido y también se reflejan en los valores de la columna `position` en la base de datos y sin ningún código, solo con el marcado.</span><span class="sxs-lookup"><span data-stu-id="17a0e-147">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>

<span data-ttu-id="17a0e-148">[![los datos de la base de datos cambian según el nuevo orden de los elementos de la lista](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="17a0e-148">[![The data in the database changes according to the new list item order](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span></span>

<span data-ttu-id="17a0e-149">Los datos de la base de datos cambian según el nuevo orden de los elementos de lista ([haga clic para ver la imagen de tamaño completo](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="17a0e-149">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="17a0e-150">Anterior</span><span class="sxs-lookup"><span data-stu-id="17a0e-150">Previous</span></span>](using-postbacks-with-reorderlist-vb.md)
