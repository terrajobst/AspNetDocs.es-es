---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
title: Usar un ConfirmButton en un repetidor (VB) | Microsoft Docs
author: wenz
description: El extensor ConfirmButton en el kit de herramientas de control de AJAX crea una ventana emergente sí/no cuando el usuario hace clic en un botón (incluido el control LinkButton). Solo si sí es...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 18c31709-3f9d-4d93-8b01-f1356bf610b4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: 001233d866d8a731d93d6900f714cd2060f3d08c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78497545"
---
# <a name="using-a-confirmbutton-in-a-repeater-vb"></a>Usar un ConfirmButton en un control Repeater (VB)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)

> El extensor ConfirmButton en el kit de herramientas de control de AJAX crea una ventana emergente sí/no cuando el usuario hace clic en un botón (incluido el control LinkButton). Solo si se hace clic en sí, se ejecuta la acción del botón; de lo contrario, se cancela. Esto también es posible en un repetidor.

## <a name="overview"></a>Información general

El extensor ConfirmButton en el kit de herramientas de control de AJAX crea una ventana emergente sí/no cuando el usuario hace clic en un botón (incluido el control LinkButton). Solo si se hace clic en sí, se ejecuta la acción del botón; de lo contrario, se cancela. Esto también es posible en un repetidor.

## <a name="steps"></a>Pasos

En primer lugar, se necesita un origen de datos. Este ejemplo utiliza la base de datos AdventureWorks y el Microsoft SQL Server 2005 Express Edition. La base de datos es una parte opcional de una instalación de Visual Studio (incluida Express Edition) y también está disponible como descarga independiente en [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). La base de datos AdventureWorks forma parte de los ejemplos de SQL Server 2005 y de las bases de datos de ejemplo (descarga en [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). La forma más sencilla de configurar la base de datos es usar el Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) y adjuntar el archivo de base de datos `AdventureWorks.mdf`.

En este ejemplo, se supone que la instancia del SQL Server 2005 Express Edition se denomina `SQLEXPRESS` y reside en el mismo equipo que el servidor Web. Esta es también la configuración predeterminada. Si el programa de instalación es diferente, tendrá que adaptar la información de conexión de la base de datos.

Para activar la funcionalidad de ASP.NET AJAX y control Toolkit, el control `ScriptManager` debe colocarse en cualquier parte de la página (pero dentro del elemento `<form>`):

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample1.aspx)]

A continuación, se necesita un origen de datos. Por motivos de simplicidad, solo se recuperan las cinco primeras entradas de la tabla de proveedores de AdventureWorks. Tenga en cuenta que cuando se usa el Asistente de Visual Studio para crear el origen de datos, el nombre de la tabla (`Vendors`) no tiene correctamente el prefijo `Purchasing`. El siguiente marcado es el correcto:

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample2.aspx)]

Este origen de datos se puede usar en un repetidor. Como de costumbre, el método `DataBinder.Eval()` recupera datos del origen de datos. Después, el control `ConfirmButtonExtender` debe colocarse dentro de la sección `<ItemTemplate>` del repetidor para que aparezca para cada entrada del origen de datos.

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample3.aspx)]

[![aparece el botón confirmar junto a cada entrada del origen de datos](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)

El botón confirmar aparece junto a cada entrada del origen de datos ([haga clic para ver la imagen de tamaño completo](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-a-confirmbutton-in-a-repeater-cs.md)
