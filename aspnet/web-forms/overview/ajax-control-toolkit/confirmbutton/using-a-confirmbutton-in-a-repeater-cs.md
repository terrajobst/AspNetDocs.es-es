---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
title: Usar un ConfirmButton en un repetidor (C#) | Microsoft Docs
author: wenz
description: El extensor ConfirmButton en el kit de herramientas de control de AJAX crea una ventana emergente sí/no cuando el usuario hace clic en un botón (incluido el control LinkButton). Solo si sí es...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a973ed3e-400c-4925-ace2-0b086b479301
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: 468a830f01c48dc39b22bc5d826f80533df65c1a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74574393"
---
# <a name="using-a-confirmbutton-in-a-repeater-c"></a>Usar un ConfirmButton en un control Repeater (C#)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1CS.pdf)

> El extensor ConfirmButton en el kit de herramientas de control de AJAX crea una ventana emergente sí/no cuando el usuario hace clic en un botón (incluido el control LinkButton). Solo si se hace clic en sí, se ejecuta la acción del botón; de lo contrario, se cancela. Esto también es posible en un repetidor.

## <a name="overview"></a>Información general del

El extensor ConfirmButton en el kit de herramientas de control de AJAX crea una ventana emergente sí/no cuando el usuario hace clic en un botón (incluido el control LinkButton). Solo si se hace clic en sí, se ejecuta la acción del botón; de lo contrario, se cancela. Esto también es posible en un repetidor.

## <a name="steps"></a>Pasos

En primer lugar, se necesita un origen de datos. Este ejemplo utiliza la base de datos AdventureWorks y el Microsoft SQL Server 2005 Express Edition. La base de datos es una parte opcional de una instalación de Visual Studio (incluida Express Edition) y también está disponible como descarga independiente en [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). La base de datos AdventureWorks forma parte de los ejemplos de SQL Server 2005 y de las bases de datos de ejemplo (descarga en [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). La forma más sencilla de configurar la base de datos es usar el Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) y adjuntar el archivo de base de datos `AdventureWorks.mdf`.

En este ejemplo, se supone que la instancia del SQL Server 2005 Express Edition se denomina `SQLEXPRESS` y reside en el mismo equipo que el servidor Web. Esta es también la configuración predeterminada. Si el programa de instalación es diferente, tendrá que adaptar la información de conexión de la base de datos.

Para activar la funcionalidad de ASP.NET AJAX y control Toolkit, el control `ScriptManager` debe colocarse en cualquier parte de la página (pero dentro del elemento `<form>`):

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample1.aspx)]

A continuación, se necesita un origen de datos. Por motivos de simplicidad, solo se recuperan las cinco primeras entradas de la tabla de proveedores de AdventureWorks. Tenga en cuenta que cuando se usa el Asistente de Visual Studio para crear el origen de datos, el nombre de la tabla (`Vendors`) no tiene correctamente el prefijo `Purchasing`. El siguiente marcado es el correcto:

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample2.aspx)]

Este origen de datos se puede usar en un repetidor. Como de costumbre, el método `DataBinder.Eval()` recupera datos del origen de datos. Después, el control `ConfirmButtonExtender` debe colocarse dentro de la sección `<ItemTemplate>` del repetidor para que aparezca para cada entrada del origen de datos.

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample3.aspx)]

[![aparece el botón confirmar junto a cada entrada del origen de datos](using-a-confirmbutton-in-a-repeater-cs/_static/image2.png)](using-a-confirmbutton-in-a-repeater-cs/_static/image1.png)

El botón confirmar aparece junto a cada entrada del origen de datos ([haga clic para ver la imagen de tamaño completo](using-a-confirmbutton-in-a-repeater-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Siguiente](using-a-confirmbutton-in-a-repeater-vb.md)
