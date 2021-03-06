---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-vb
title: Usar ModalPopup con un control Repeater (VB) | Microsoft Docs
author: wenz
description: El control ModalPopup en el kit de herramientas de control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios del lado cliente. También es posible usar esta contr...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0c8e74f1-b3ba-4ca9-a1c5-f5c4831a359a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 0966770f0218ca91ba7d25e7bf703bf7b005738e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78496843"
---
# <a name="using-modalpopup-with-a-repeater-control-vb"></a>Usar ModalPopup con un control Repeater (VB)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2VB.pdf)

> El control ModalPopup en el kit de herramientas de control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios del lado cliente. También es posible usar este control dentro de un repetidor.

## <a name="overview"></a>Información general

El control ModalPopup en el kit de herramientas de control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios del lado cliente. También es posible usar este control dentro de un repetidor.

## <a name="steps"></a>Pasos

En primer lugar, se necesita un origen de datos. Este ejemplo utiliza la base de datos AdventureWorks y el Microsoft SQL Server 2005 Express Edition. La base de datos es una parte opcional de una instalación de Visual Studio (incluida Express Edition) y también está disponible como descarga independiente en [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). La base de datos AdventureWorks forma parte de los ejemplos de SQL Server 2005 y de las bases de datos de ejemplo (descarga en [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). La forma más sencilla de configurar la base de datos es usar el Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) y adjuntar el archivo de base de datos `AdventureWorks.mdf`. En este ejemplo, se supone que la instancia del SQL Server 2005 Express Edition se denomina `SQLEXPRESS` y reside en el mismo equipo que el servidor Web. Esta es también la configuración predeterminada. Si el programa de instalación es diferente, tendrá que adaptar la información de conexión de la base de datos. Para activar la funcionalidad de ASP.NET AJAX y control Toolkit, el control `ScriptManager` debe colocarse en cualquier parte de la página (pero dentro del elemento `<form>`):

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample1.aspx)]

A continuación, agregue un origen de datos a la página. Para usar una cantidad limitada de datos, solo se seleccionan las cinco primeras entradas de la tabla Vendor de la base de datos AdventureWorks. Si usa el Asistente de Visual Studio para crear el origen de datos, tenga en cuenta que un error en la versión actual no antepone el nombre de tabla (`Vendor`) con `Purchasing`. En el marcado siguiente se muestra la sintaxis correcta:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample2.aspx)]

A continuación, agregue un panel que actúe como elemento emergente modal. Contiene un control `Button` para cerrar de nuevo el elemento emergente:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample3.aspx)]

Para que el elemento emergente funcione dentro del repetidor, el control `ModalPopupExtender` debe colocarse dentro de la sección `<ItemTemplate>` del Repeater. Por lo tanto, el panel está fuera del repetidor, pero el extensor está dentro. Este es el marcado del Repeater:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample4.aspx)]

Después, cada elemento del origen de datos se muestra con un botón junto al mismo que desencadena el elemento emergente modal.

[![se puede desencadenar el elemento emergente modal para cada entrada de origen de datos](using-modalpopup-with-a-repeater-control-vb/_static/image2.png)](using-modalpopup-with-a-repeater-control-vb/_static/image1.png)

La ventana emergente modal se puede desencadenar para cada entrada de origen de datos ([haga clic para ver la imagen de tamaño completo](using-modalpopup-with-a-repeater-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](launching-a-modal-popup-window-from-server-code-vb.md)
> [Siguiente](handling-postbacks-from-a-modalpopup-vb.md)
