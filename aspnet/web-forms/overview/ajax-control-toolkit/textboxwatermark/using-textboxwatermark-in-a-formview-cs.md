---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-cs
title: Usar TextBoxWatermark en FormView (C#) | Microsoft Docs
author: wenz
description: El control TextBoxWatermark en el kit de herramientas de control de AJAX extiende un cuadro de texto para que se muestre un texto en el cuadro. Cuando un usuario hace clic en el cuadro,...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e6ee90bf-32a5-4987-a384-15cc7dd30c8a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-cs
msc.type: authoredcontent
ms.openlocfilehash: 13ac0da5ca53756aa7c660cdc47c96f0c865b006
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78508843"
---
# <a name="using-textboxwatermark-in-a-formview-c"></a>Usar TextBoxWatermark en un FormView (C#)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1CS.pdf)

> El control TextBoxWatermark en el kit de herramientas de control de AJAX extiende un cuadro de texto para que se muestre un texto en el cuadro. Cuando un usuario hace clic en el cuadro, se vacía. Si el usuario deja el cuadro sin escribir texto, vuelve a aparecer el texto rellenado. Esto también es posible dentro de un control FormView.

## <a name="overview"></a>Información general

El control `TextBoxWatermark` en el kit de herramientas de control de AJAX extiende un cuadro de texto para que se muestre un texto en el cuadro. Cuando un usuario hace clic en el cuadro, se vacía. Si el usuario deja el cuadro sin escribir texto, vuelve a aparecer el texto rellenado. Esto también es posible dentro de un control de `FormView`.

## <a name="steps"></a>Pasos

En primer lugar, se necesita un origen de datos. Este ejemplo utiliza la base de datos AdventureWorks y el Microsoft SQL Server 2005 Express Edition. La base de datos es una parte opcional de una instalación de Visual Studio (incluida Express Edition) y también está disponible como descarga independiente en [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). La base de datos AdventureWorks forma parte de los ejemplos de SQL Server 2005 y de las bases de datos de ejemplo (descarga en [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). La forma más sencilla de configurar la base de datos es usar el Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) y adjuntar el archivo de base de datos `AdventureWorks.mdf`.

En este ejemplo, se supone que la instancia del SQL Server 2005 Express Edition se denomina `SQLEXPRESS` y reside en el mismo equipo que el servidor Web. Esta es también la configuración predeterminada. Si el programa de instalación es diferente, tendrá que adaptar la información de conexión de la base de datos.

Para activar la funcionalidad de ASP.NET AJAX y control Toolkit, el control `ScriptManager` debe colocarse en cualquier parte de la página (pero dentro del elemento `<form>`):

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample1.aspx)]

A continuación, agregue un origen de datos a la página que admita las instrucciones SQL `DELETE`, `INSERT` y `UPDATE`. Si usa el Asistente de Visual Studio para crear el origen de datos, tenga en cuenta que un error en la versión actual no antepone el nombre de tabla (`Vendor`) con `Purchasing`. En el marcado siguiente se muestra la sintaxis correcta:

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample2.aspx)]

Recuerde el nombre (`ID`) del origen de datos, ya que se usará en la propiedad `DataSourceID` del control `FormView`. La sección `<InsertItemTemplate>` del `FormView` contiene un cuadro de texto que se extiende mediante el control `TextBoxWatermarkExtender`. Asegúrese de que el extensor se encuentra dentro de la plantilla y no fuera de él.

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample3.aspx)]

Ahora, cuando el usuario cambia al modo de inserción del control `FormView`, el campo de texto para el nuevo proveedor se rellena previamente gracias al control `TextBoxWatermarkExtender`. Un clic dentro del cuadro de texto permite que el texto del relleno desaparezca.

[![la marca de agua del campo proviene del extensor](using-textboxwatermark-in-a-formview-cs/_static/image2.png)](using-textboxwatermark-in-a-formview-cs/_static/image1.png)

La marca de agua del campo proviene del dispositivo extender ([haga clic para ver la imagen de tamaño completo](using-textboxwatermark-in-a-formview-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Siguiente](using-textboxwatermark-with-validation-controls-cs.md)
