---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
title: Introducción a AJAX control Toolkit (VB) | Microsoft Docs
author: microsoft
description: Aprenda todo lo que necesita saber para comenzar a usar el kit de herramientas de control de AJAX.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 9f8fa166-49a2-402c-b236-20caef0c658f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
msc.type: authoredcontent
ms.openlocfilehash: ff614308555b31710b11f408e12e9a6fadbf98d0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78497143"
---
# <a name="get-started-with-the-ajax-control-toolkit-vb"></a>Introducción a AJAX Control Toolkit (VB)

por [Microsoft](https://github.com/microsoft)

> Aprenda todo lo que necesita saber para comenzar a usar el kit de herramientas de control de AJAX.

El kit de herramientas de control de AJAX contiene más de 30 controles gratuitos que puede usar en las aplicaciones de ASP.NET. En este tutorial, obtendrá información sobre cómo descargar el kit de herramientas de control de AJAX y agregar los controles del kit de herramientas al cuadro de herramientas de Visual Studio o Visual Web Developer Express.

## <a name="downloading-the-ajax-control-toolkit"></a>Descargar el kit de herramientas de control de AJAX

[Ajax control Toolkit](http://devexpress.com/act) es un proyecto de código abierto desarrollado por los miembros de la comunidad asp.net y el equipo ASP.net.

[![descargar el kit de herramientas de control de AJAX](get-started-with-the-ajax-control-toolkit-vb/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image1.png)

**Figura 01**: descarga de Ajax control Toolkit ([haga clic para ver la imagen de tamaño completo](get-started-with-the-ajax-control-toolkit-vb/_static/image2.png))

Después de descargar el archivo, debe desbloquear el archivo. Haga clic con el botón derecho en el archivo, seleccione Propiedades y haga clic en el botón **desbloquear** (consulte la figura 2).

[![desbloquear el archivo ZIP del kit de herramientas de control de AJAX](get-started-with-the-ajax-control-toolkit-vb/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image3.png)

**Figura 02**: desbloqueo del archivo zip del kit de herramientas de control de Ajax ([haga clic para ver la imagen de tamaño completo](get-started-with-the-ajax-control-toolkit-vb/_static/image4.png))

Después de desbloquear el archivo, puede descomprimir el archivo: haga clic con el botón derecho en el archivo y seleccione la opción de menú **extraer todo** . Ahora, estamos preparados para agregar el kit de herramientas al cuadro de herramientas de Visual Studio o Visual Web Developer.

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>Agregar el kit de herramientas de control de AJAX al cuadro de herramientas

La forma más sencilla de usar AJAX control Toolkit es agregar el kit de herramientas al cuadro de herramientas de Visual Studio o Visual Web Developer (consulte la figura 3). De este modo, simplemente puede arrastrar un control de kit de herramientas a una página cuando desee usarlo.

[![AJAX control Toolkit aparece en el cuadro de herramientas](get-started-with-the-ajax-control-toolkit-vb/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image5.png)

**Figura 03**: el kit de herramientas de control de Ajax aparece en el cuadro de herramientas ([haga clic para ver la imagen de tamaño completo](get-started-with-the-ajax-control-toolkit-vb/_static/image6.png))

En primer lugar, debe agregar una pestaña de AJAX control Toolkit al cuadro de herramientas. Siga estos pasos.

1. Cree un nuevo sitio web de ASP.NET seleccionando el archivo de opción de menú, nuevo sitio Web. Haga doble clic en default. aspx en la ventana Explorador de soluciones para abrir el archivo en el editor.
2. Haga clic con el botón derecho en el cuadro de herramientas debajo de la pestaña General y seleccione la opción de menú **Agregar pestaña** (vea la figura 4).
3. Escriba una nueva pestaña denominada AJAX control Toolkit.

[![agregar una nueva pestaña](get-started-with-the-ajax-control-toolkit-vb/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image7.png)

**Figura 04**: agregar una nueva pestaña ([haga clic para ver la imagen de tamaño completo](get-started-with-the-ajax-control-toolkit-vb/_static/image8.png))

A continuación, debe agregar los controles de AJAX control Toolkit a la nueva pestaña. Siga estos pasos:

- Haga clic con el botón derecho en la pestaña AJAX control Toolkit y seleccione **elementos (vea la figura 5)** .
- Vaya a la ubicación donde descomprimió el kit de herramientas de control de AJAX y seleccione el ensamblado AjaxControlToolkit. dll.

[![elegir los elementos que se van a agregar al cuadro de herramientas](get-started-with-the-ajax-control-toolkit-vb/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image9.png)

**Figura 05**: elegir los elementos que se van a agregar al cuadro de herramientas ([haga clic para ver la imagen de tamaño completo](get-started-with-the-ajax-control-toolkit-vb/_static/image10.png))

Después de completar estos pasos, todos los controles del kit de herramientas aparecerán en el cuadro de herramientas.

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>Actualización a una nueva versión del kit de herramientas

Si usaba una versión anterior del kit de herramientas y ahora necesita pasar a una versión posterior, estos son los pasos recomendados:

- Binarios: Elimine la versión anterior del ensamblado AjaxControlToolkit. dll de la carpeta bin del sitio Web.
- Elementos del cuadro de herramientas: Elimine la pestaña AJAX control Toolkit y siga los pasos anteriores para volver a crear la pestaña con la nueva versión del ensamblado AjaxControlToolkit. dll.

> [!div class="step-by-step"]
> [Anterior](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
> [Siguiente](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
