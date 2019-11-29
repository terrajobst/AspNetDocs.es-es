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
# <a name="drag-and-drop-via-reorderlist-vb"></a>Arrastrar y colocar mediante ReorderList (VB)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)

> El control ReorderList en el kit de herramientas de control de AJAX proporciona una lista que el usuario puede reordenar mediante arrastrar y colocar. El orden actual de la lista se conservará en el servidor.

## <a name="overview"></a>Información general del

El control `ReorderList` en el kit de herramientas de control de AJAX proporciona una lista que el usuario puede reordenar mediante arrastrar y colocar. El orden actual de la lista se conservará en el servidor.

## <a name="steps"></a>Pasos

El control `ReorderList` admite el enlace de datos de una base de datos a la lista. Lo mejor de todo es que también admite la escritura de cambios en el orden del elemento de lista de nuevo en el almacén de datos.

En este ejemplo se utiliza Microsoft SQL Server 2005 Express Edition como almacén de datos. La base de datos es una parte opcional (y gratuita) de una instalación de Visual Studio, incluida la edición Express. También está disponible como descarga independiente en [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). En este ejemplo, se supone que la instancia del SQL Server 2005 Express Edition se denomina `SQLEXPRESS` y reside en el mismo equipo que el servidor Web. Esta es también la configuración predeterminada. Si el programa de instalación es diferente, tendrá que adaptar la información de conexión de la base de datos.

La forma más sencilla de configurar la base de datos es usar el Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ). Conéctese al servidor, haga doble clic en `Databases` y cree una nueva base de datos (haga clic con el botón derecho y seleccione `New Database`) denominada `Tutorials`.

En esta base de datos, cree una nueva tabla llamada `AJAX` con las cuatro columnas siguientes:

- `id` (clave principal, entero, identidad, no NULL)
- `char` (Char (1), NULL)
- `description` (varchar (50), NULL)
- `position` (int, NULL)

[![del diseño de la tabla de AJAX](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)

El diseño de la tabla de AJAX ([haga clic para ver la imagen de tamaño completo](drag-and-drop-via-reorderlist-vb/_static/image3.png))

A continuación, rellene la tabla con un par de valores. Tenga en cuenta que la columna `position` contiene el criterio de ordenación de los elementos.

[![los datos iniciales de la tabla de AJAX](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)

Los datos iniciales de la tabla de AJAX ([haga clic para ver la imagen de tamaño completo](drag-and-drop-via-reorderlist-vb/_static/image6.png))

El paso siguiente requiere que se genere un control `SqlDataSource` para comunicarse con la nueva base de datos y su tabla. El origen de datos debe admitir los comandos SQL `SELECT` y `UPDATE`. Cuando se cambia el orden de los elementos de la lista, el control `ReorderList` envía automáticamente dos valores al comando `Update` del origen de datos: la nueva posición y el identificador del elemento. Por lo tanto, el origen de datos necesita una sección `<UpdateParameters>` para estos dos valores:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

El control `ReorderList` debe establecer los siguientes atributos:

- `AllowReorder`: si se pueden reorganizar los elementos de la lista
- `DataSourceID`: el identificador del origen de datos
- `DataKeyField`: el nombre de la columna de clave principal del origen de datos
- `SortOrderField`: la columna de origen de datos que proporciona el criterio de ordenación para los elementos de lista

En las secciones `<DragHandleTemplate>` y `<ItemTemplate>`, se puede ajustar el diseño de la lista. Además, el enlace de los DataBindings es posible mediante el método `Eval()`, como se muestra aquí:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

La siguiente información de estilo CSS (a la que se hace referencia en la sección `<DragHandleTemplate>` del control de `ReorderList`) garantiza que el puntero del mouse cambie correctamente cuando se mantenga el puntero sobre el controlador de arrastre:

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

Por último, un control `ScriptManager` inicializa ASP.NET AJAX para la página:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

Ejecute este ejemplo en el explorador y reorganice los elementos de la lista un poco. A continuación, vuelva a cargar la página o examine la base de datos. Las posiciones modificadas se han mantenido y también se reflejan en los valores de la columna `position` en la base de datos y sin ningún código, solo con el marcado.

[![los datos de la base de datos cambian según el nuevo orden de los elementos de la lista](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)

Los datos de la base de datos cambian según el nuevo orden de los elementos de lista ([haga clic para ver la imagen de tamaño completo](drag-and-drop-via-reorderlist-vb/_static/image9.png))

> [!div class="step-by-step"]
> [Anterior](using-postbacks-with-reorderlist-vb.md)
