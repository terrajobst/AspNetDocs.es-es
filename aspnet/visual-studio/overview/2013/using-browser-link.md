---
uid: visual-studio/overview/2013/using-browser-link
title: Usar el vínculo del explorador en Visual Studio 2013 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/04/2013
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: 723a38de4569b0bb58817c70aabb84fef8e19591
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78505207"
---
# <a name="using-browser-link-in-visual-studio-2013"></a>Usar el vínculo del explorador en Visual Studio 2013

por [Mike Wasson](https://github.com/MikeWasson)

El vínculo del explorador es una característica nueva de Visual Studio 2013 que crea un canal de comunicación entre el entorno de desarrollo y uno o varios exploradores Web. Puede usar el vínculo del explorador para actualizar la aplicación web en varios exploradores a la vez, lo que resulta útil para las pruebas entre exploradores.

- [Actualización del explorador](#browser-refresh)
- [Ver el panel de vínculos del explorador](#dashboard)
- [Habilitación del vínculo del explorador para archivos HTML estáticos](#static-html)
- [Deshabilitando vínculo de explorador](#disabling)
- [¿Cómo funciona?](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a>Actualización del explorador

Con la actualización del explorador, puede actualizar varios exploradores que están conectados a Visual Studio a través del vínculo del explorador.

Para usar la actualización del explorador, cree primero una aplicación ASP.NET con cualquiera de las plantillas de proyecto. Depure la aplicación presionando F5 o haciendo clic en el icono de flecha de la barra de herramientas:

![](using-browser-link/_static/image1.png)

También puede usar la lista desplegable para seleccionar un explorador específico para la depuración.

![](using-browser-link/_static/image2.png)

Para depurar con varios exploradores, seleccione **examinar con**. En el cuadro de diálogo **examinar con** , mantenga presionada la tecla Ctrl para seleccionar más de un explorador. Haga clic en **examinar** para depurar con los exploradores seleccionados. El vínculo del explorador también funciona si inicia un explorador desde fuera de Visual Studio y navega a la dirección URL de la aplicación.

![](using-browser-link/_static/image3.png)

Los controles de vínculo del explorador se encuentran en la lista desplegable con el icono de flecha circular. El icono de flecha es el botón **Actualizar** .

![](using-browser-link/_static/image4.png)

Para ver qué exploradores están conectados, mantenga el mouse sobre el botón de **actualización** durante la depuración. Los exploradores conectados se muestran en una ventana de información sobre herramientas.

![](using-browser-link/_static/image5.png)

Para actualizar los exploradores conectados, haga clic en el botón **Actualizar** o presione Ctrl + Alt + Entrar. Por ejemplo, en la captura de pantalla siguiente se muestra un proyecto de ASP.NET, que se creó con la plantilla de proyecto de MVC 5. Puede ver la aplicación que se ejecuta en dos exploradores en la parte superior. En la parte inferior, el proyecto está abierto en Visual Studio.

![](using-browser-link/_static/image6.png)

En Visual Studio, cambio el encabezado &lt;H1&gt; de la Página principal:

![](using-browser-link/_static/image7.png)

Cuando hago clic en el botón **Actualizar** , el cambio apareció en las dos ventanas del explorador:

![](using-browser-link/_static/image8.png)

**Notas**

- Para habilitar el vínculo del explorador, establezca `debug=true` en el elemento&gt;de la [compilación de&lt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) en el archivo Web. config del proyecto.
- La aplicación debe ejecutarse en localhost.
- La aplicación debe tener como destino .NET 4,0 o posterior.

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a>Ver el panel de vínculos del explorador

El panel vínculo del explorador muestra información acerca de las conexiones de vínculo del explorador. Para ver el panel, seleccione el menú desplegable vínculo del explorador (la flecha pequeña situada junto al botón **Actualizar** ). A continuación, haga clic en **Panel vínculo de explorador**.

![](using-browser-link/_static/image9.png)

En el panel se muestran los exploradores conectados y la dirección URL en la que se ha navegado por cada explorador.

![](using-browser-link/_static/image10.png)

En la sección de **requisitos previos** se muestran los pasos necesarios para habilitar el vínculo del explorador para ese proyecto. Por ejemplo, la captura de pantalla siguiente muestra un proyecto donde "debug" se establece en false en el archivo Web. config.

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a>Habilitación del vínculo del explorador para archivos HTML estáticos

Para habilitar el vínculo del explorador para los archivos HTML estáticos, agregue lo siguiente al archivo Web. config.

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

Por motivos de rendimiento, quite esta configuración al publicar el proyecto.

<a id="disabling"></a>
## <a name="disabling-browser-link"></a>Deshabilitando vínculo de explorador

El vínculo del explorador está habilitado de forma predeterminada. Hay varias maneras de deshabilitarlo:

- En el menú desplegable vínculo del explorador, desactive **Habilitar vínculo de explorador**. 

    ![](using-browser-link/_static/image12.png)
- En el archivo Web. config, agregue una clave denominada "vs: EnableBrowserLink" con el valor "false" en la sección appSettings. 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- En el archivo Web. config, establezca debug en false. 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a>¿Cómo funciona?

El vínculo del explorador usa [signalr](../../../signalr/index.md) para crear un canal de comunicación entre Visual Studio y el explorador. Cuando el vínculo del explorador está habilitado, Visual Studio actúa como un servidor de Signalr al que se pueden conectar varios clientes (exploradores). El vínculo del explorador también registra un módulo HTTP con ASP.NET. Este módulo inserta un script de &lt;especial&gt; referencias en cada solicitud de página del servidor. Puede ver las referencias del script seleccionando "ver código fuente" en el explorador.

![](using-browser-link/_static/image13.png)

Los archivos de origen no se modifican. El módulo HTTP inserta las referencias de script dinámicamente.

Dado que el código del explorador es todo JavaScript, funciona en todos los exploradores [compatibles con signalr](../../../signalr/overview/getting-started/supported-platforms.md), sin necesidad de ningún complemento del explorador.
