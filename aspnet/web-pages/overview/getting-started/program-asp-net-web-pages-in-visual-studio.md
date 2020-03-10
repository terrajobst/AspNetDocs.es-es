---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: ASP.NET Web Pages de programación (Razor) con Visual Studio | Microsoft Docs
author: Rick-Anderson
description: En este apéndice se explica cómo puede usar Visual Studio 2010 o Visual Web Developer 2010 Express para programar ASP.NET Web Pages con la sintaxis Razor.
ms.author: riande
ms.date: 02/13/2014
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 1a76098779d05912bf7bdf2de5fdce024770752c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78514297"
---
# <a name="programming-aspnet-web-pages-razor-using-visual-studio"></a>ASP.NET Web Pages de programación (Razor) con Visual Studio

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explica cómo puede usar Visual Studio o Visual Web Developer Express para programar ASP.NET Web Pages (Razor) sitios Web.
>
> Temas que se abordarán
>
> - Lo que necesita para instalar (en caso de que haya algo) para trabajar con ASP.NET Web Pages en su versión de Visual Studio.
> - Cómo agregar compatibilidad para ASP.NET Web Pages a Visual Web Developer 2010 Express.
> - Cómo usar las características de Visual Studio para trabajar con las páginas de Razor de ASP.NET, incluidas IntelliSense y el depurador.
>
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
>
>
> - ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>
>
> Este tutorial también funciona con ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010 y WebMatrix 2.

Puede programar páginas Web ASP.NET con sintaxis Razor mediante WebMatrix o muchos otros editores de código. También puede usar Microsoft Visual Studio, que es un entorno de desarrollo integrado (IDE) con todas las características que proporciona un eficaz conjunto de herramientas para crear muchos tipos de aplicaciones (no solo sitios web). Para trabajar con las páginas de Razor para ASP.NET, puede usar [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).

Dos características especialmente útiles que Visual Studio proporciona para la programación con páginas web de Razor para ASP.NET son:

- *IntelliSense*. La característica IntelliSense integrada en Visual Studio es más completa que IntelliSense en WebMatrix.
- *Depurador*. El depurador permite solucionar problemas del código al detener un programa mientras se ejecuta, examinar variables y recorrer el código línea a línea.

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a>Usar Visual Studio con versiones diferentes de ASP.NET Web Pages

Para desarrollar aplicaciones Web de ASP.NET en Visual Studio 2017, instale la carga de trabajo de **desarrollo de ASP.net y Web** .

Visual Studio 2012 y Visual Studio 2013 incluyen compatibilidad con ASP.NET Web Pages. (Los paquetes necesarios para admitir ASP.NET Web Pages se instalan al instalar Visual Studio).

Visual Studio 2010 no incluye compatibilidad de forma predeterminada para ASP.NET Web Pages. Para usar ASP.NET Web Pages con Visual Studio 2010, debe instalar el paquete ASP.NET MVC. Para obtener ASP.NET Web Pages 2, instale ASP.NET MVC 4.

En la tabla siguiente se resume la compatibilidad con ASP.NET Web Pages en diferentes versiones de Visual Studio.

|  | Visual Studio 2010 | Visual Studio 2012 | Visual Studio 2013 |
| --- | --- | --- | --- |
| **ASP.NET Web Pages 2** | Instalación de ASP.NET MVC 4 | Inclusión | Inclusión |
| **ASP.NET Web Pages 3** |  | Actualización de ASP.NET Web Pages 3 a NuGet | Inclusión |

Para trabajar con Visual Studio 2010, consulte [instalación de la compatibilidad con ASP.NET Web pages en visual studio 2010](#vs2010support).

## <a name="launching-visual-studio-from-webmatrix"></a>Inicio de Visual Studio desde WebMatrix

Si ha iniciado un proyecto en WebMatrix y desea cambiar a Visual Studio, WebMatrix proporciona un botón para abrir fácilmente el proyecto en Visual Studio. Para que este botón esté habilitado, debe tener Visual Studio instalado en el equipo. En la imagen siguiente se muestra el botón de WebMatrix.

![iniciar Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

Al hacer clic en el botón, el proyecto se abre en Visual Studio. Puede alternar entre WebMatrix y Visual Studio sin problemas. Se le notificará si algún archivo ha cambiado en el otro entorno y debe volver a cargarse para obtener los cambios más recientes.

## <a name="creating-aspnet-razor-site-in-visual-studio"></a>Creación de un sitio de ASP.NET Razor en Visual Studio

Para crear un sitio web de Razor ASP.NET en Visual Studio:

1. Abra Visual Studio.
2. En el menú **archivo** , haga clic en **nuevo sitio web**.

    ![crear nuevo sitio web](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. En el cuadro de diálogo **nuevo sitio web** , seleccione el idioma que desea usar C# (visual o Visual Basic).
4. Seleccione la plantilla **sitio web de ASP.net (Razor)** .

    ![sitio de Razor](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. Haga clic en **Aceptar**.

El nuevo proyecto existe y se rellena con algunas páginas Web predeterminadas para ayudarle a empezar.

### <a name="using-intellisense"></a>Using IntelliSense

Ahora que ha creado un sitio, puede ver cómo funciona IntelliSense en Visual Studio.

1. En el sitio web que acaba de crear, abra la página *default. cshtml* .
2. Después de la `<h3>` etiquetas en la página, escriba `@ServerInfo.` (incluido el punto). Observe cómo IntelliSense muestra los métodos disponibles para la aplicación auxiliar de `ServerInfo` en una lista desplegable.

    ![IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. Seleccione el método de `GetHtml` de la lista y, a continuación, presione Entrar. IntelliSense rellena automáticamente el método. (Como con cualquier método de C#, debe agregar `()` caracteres después del método). El código completado para el método `GetHtml` es similar al ejemplo siguiente:

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. Presione Ctrl + F5 para ejecutar la página. Este es el aspecto de la página cuando se muestra en un explorador:

    ![página predeterminada en el explorador](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. Cierre el explorador.

### <a name="using-the-debugger"></a>Usar el depurador

1. En la parte superior de la página *default. cshtml* , después de la línea que comienza con `Page.Title`, agregue la siguiente línea de código:

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. En el margen gris del editor a la izquierda del código, haga clic junto a esta nueva línea para agregar un *punto de interrupción*. Un punto de interrupción es un marcador que indica al depurador que detenga la ejecución del programa en ese momento para que pueda ver lo que está ocurriendo.

    ![establecer punto de interrupción](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. Quite la llamada al método `ServerInfo.GetHtml` y agregue una llamada a la variable `@myTime` en su lugar. Esta llamada muestra el valor de hora actual devuelto por la nueva línea de código.
4. Presione F5 para ejecutar la página en el depurador. La página se detiene en el punto de interrupción que estableció. En la imagen siguiente se muestra el aspecto de la página en el editor con el punto de interrupción (en amarillo).

    ![depurar punto de interrupción](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. En la barra de herramientas Depurar, haga clic en el botón **paso a paso por instrucciones** (o presione F11) para ejecutar la siguiente línea de código. Cada vez que haga clic en este botón, avanzará la ejecución a la siguiente línea de código.

    ![Botón paso a paso por instrucciones](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. Examine el valor de la variable `myTime` manteniendo el puntero del mouse sobre él o inspeccionando los valores mostrados en las ventanas **variables locales** y **pila de llamadas** . Visual Studio muestra el valor de la variable.

    ![Mostrar valor de tiempo](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. Cuando haya terminado de examinar la variable y recorrer el código, presione F5 para continuar con la ejecución de la página sin detener en cada línea. Cuando haya terminado de recorrer todo el código, el explorador muestra la página.

Para obtener más información sobre el depurador y sobre cómo depurar código en Visual Studio, vea [Tutorial: depurar páginas web en Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a>Usar Razor en proyectos de MVC de ASP.NET con Visual Studio

La sintaxis Razor también se usa en los proyectos de MVC de ASP.NET. MVC es un método eficaz basado en patrones para crear sitios web dinámicos. Si el sitio de ASP.NET Web Pages resulta difícil de mantener, puede considerar la posibilidad de convertirlo en una aplicación ASP.NET MVC. Para obtener un ejemplo de cómo crear una aplicación MVC, vea [Introducción con ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a>Instalación de compatibilidad para ASP.NET Web Pages en Visual Studio 2010

En esta sección se muestra cómo instalar Visual Web Developer Express 2010 y las herramientas de ASP.NET Web Pages para Visual Studio.

1. Si aún no tiene el instalador de plataforma web, descárguelo de la siguiente dirección URL:

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. Ejecute el instalador de plataforma Web.
3. Haga clic en la pestaña **productos** .

    ![Pestaña productos de WebPI](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. Busque **ASP.NET MVC 4** (para ASP.NET Web Pages 2) y, a continuación, haga clic en **Agregar**. Estos productos incluyen herramientas de Visual Studio para compilar sitios web de Razor ASP.NET.

    ![Opciones de instalación de WebPi](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. Haga clic en **instalar** para completar la instalación.
