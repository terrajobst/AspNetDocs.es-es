---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
title: Usar controles y extensores de control de AJAX control Toolkit | Microsoft Docs
author: microsoft
description: Obtenga información sobre cómo agregar controles y extensores del kit de herramientas de control de AJAX a las páginas de ASP.NET.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 763650a9-ffde-46a9-b779-7a9145dd5d88
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
msc.type: authoredcontent
ms.openlocfilehash: 90a6003ff50ba6e85196c25cf175e057810f0f84
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78497053"
---
# <a name="using-ajax-control-toolkit-controls-and-control-extenders-vb"></a>Usar los controles de AJAX Control Toolkit y los controles extensores (VB)

por [Microsoft](https://github.com/microsoft)

> Obtenga información sobre cómo agregar controles y extensores del kit de herramientas de control de AJAX a las páginas de ASP.NET.

AJAX control Toolkit contiene un conjunto de controles y extensores de control. En este breve tutorial, aprenderá a agregar controles y controles extensores a una página de ASP.NET.

> [!NOTE] 
> 
> Para obtener instrucciones sobre cómo instalar AJAX control Toolkit y cómo agregar AJAX control Toolkit al cuadro de herramientas de Visual Studio o Visual Web Developer, vea el tutorial Introducción [a Ajax control Toolkit](get-started-with-the-ajax-control-toolkit-vb.md).

## <a name="using-ajax-control-toolkit-controls"></a>Usar controles de AJAX control Toolkit

Un control de AJAX control Toolkit funciona igual que un control ASP.NET normal. Puede arrastrar el control desde el cuadro de herramientas hasta una página de ASP.NET. Puede Agregar el control a la página en Vista de diseño o en la vista Código fuente.

Hay un requisito especial al usar los controles de AJAX control Toolkit. La página debe contener un control ScriptManager. El control ScriptManager es responsable de incluir todos los JavaScript necesarios que requieren los controles de AJAX control Toolkit.

Por ejemplo, la pestaña AJAX control Toolkit incluye un control denominado control de editor. Este control muestra un editor HTML enriquecido. Siga estos pasos para agregar el control de editor a una página:

1. Cree una nueva página de ASP.NET denominada ShowEditor. aspx
2. Seleccione el control ScriptManager en la pestaña extensiones AJAX del cuadro de herramientas y arrastre el control hasta la página.
3. Seleccione el control de editor de debajo de la pestaña AJAX control Toolkit en el cuadro de herramientas y arrastre el control a la página (vea la ilustración 1). El diseñador debe ser similar al de la figura 2.
4. Ejecute el sitio web seleccionando la opción de menú **depurar, iniciar depuración** o presionando la tecla F5.
5. Debería ver la página en la figura 3.

[![seleccionar el control de editor HTML](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)

**Figura 01**: selección del control de editor HTML ([haga clic para ver la imagen de tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png))

[![diseñador de Visual Studio con ScriptManager y control de edición](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)

**Figura 02**: diseñador de Visual Studio con ScriptManager y control de edición ([haga clic para ver la imagen de tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png))

[![la página DisplayEditor. aspx](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)

**Figura 03**: Página DisplayEditor. aspx ([haga clic para ver la imagen de tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png))

## <a name="using-ajax-control-toolkit-control-extenders"></a>Usar los extensores de control de AJAX control Toolkit

AJAX control Toolkit también contiene extensores de control. Como su nombre sugiere, un extensor de control amplía la funcionalidad de un control existente. Por ejemplo, el extensor del control ConfirmButton extiende el control de botón estándar ASP.NET. El extensor cambia el comportamiento del control de botón para que el botón muestre un cuadro de diálogo de confirmación al hacer clic en él.

Un extensor de control, al igual que un control de AJAX control Toolkit, requiere un control ScriptManager. Debe agregar un control ScriptManager a una página antes de empezar a usar los extensores de control en la página.

Siga estos pasos para usar el extensor del control ConfirmButton:

1. Cree una nueva página de ASP.NET denominada ShowConfirmButton. aspx
2. Agregue un control ScriptManager a la página arrastrando el control a la página desde debajo de la pestaña extensiones AJAX.
3. Agregue un control botón estándar a la página arrastrando el botón de debajo de la pestaña estándar del cuadro de herramientas a la superficie del diseñador.
4. Haga clic en la opción **Agregar tarea extender** (consulte la figura 4).
5. En el cuadro de diálogo elegir extensor, seleccione ConfirmButtonExtender (vea la figura 5) y haga clic en el botón Aceptar.
6. Seleccione el control de botón en el diseñador y expanda el nodo extensores, button1\_ConfirmButtonExtender en el ventana Propiedades (vea la figura 6). Asigne el valor *realmente?* a la propiedad ConfirmText.
7. Ejecute la página seleccionando la opción de menú **depurar, iniciar depuración** o presionar la tecla F5.

[![la opción de tarea agregar extensor](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)

**Figura 04**: la opción Agregar tarea extender ([haga clic para ver la imagen de tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png))

[![seleccionar el extensor del control ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)

**Figura 05**: selección del extensor del control ConfirmButton ([haga clic para ver la imagen de tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png))

[![establecer una propiedad ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)

**Figura 06**: establecimiento de la propiedad ConfirmButton ([haga clic para ver la imagen de tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png))

Cuando se abra la página, debería ver un botón. Al hacer clic en el botón, se obtiene el cuadro de diálogo de confirmación en la figura 7.

[![mostrar el cuadro de diálogo de confirmación](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)

**Figura 07**: mostrar el cuadro de diálogo de confirmación ([haga clic para ver la imagen de tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png))

Tenga en cuenta que normalmente no se arrastra un extensor de control a una página. En su lugar, se usa la opción **Agregar tarea extender** para agregar un objeto extender a un control que ya se ha agregado a una página. Además, tenga en cuenta que las propiedades del extensor de control se establecen abriendo la hoja de propiedades del control que se extiende.

Un único control ASP.NET se puede extender mediante varios extensores de control. La hoja de propiedades del control que se está extendiendo enumerará todos los extensores de control asociados al control.

> [!div class="step-by-step"]
> [Anterior](get-started-with-the-ajax-control-toolkit-vb.md)
> [Siguiente](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)
