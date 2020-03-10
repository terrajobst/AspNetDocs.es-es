---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
title: Agregar un controlador (C#) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web MVC de ASP.NET con Microsoft Visual Web Developer 2010 Express Service Pack 1, que...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 0b8c56b5-fdf3-42dd-a866-98fbe0ab78a0
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 959116ff773f4ef466cda6b172e8321590b50e5b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78434965"
---
# <a name="adding-a-controller-c"></a>Agregar un controlador (C#)

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > [Hay disponible una](../../../getting-started/introduction/getting-started.md) versión actualizada de este tutorial que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro, mucho más fácil de seguir y demuestra más características.
> 
> 
> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web MVC de ASP.NET con Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos que se enumeran a continuación. Para instalar todos ellos, haga clic en el vínculo siguiente: [instalador de plataforma web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar individualmente los requisitos previos con los siguientes vínculos:
> 
> - [Requisitos previos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(compatibilidad con herramientas y tiempo de ejecución)
> 
> Si utiliza Visual Studio 2010 en lugar de Visual Web Developer 2010, instale los requisitos previos haciendo clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Hay disponible un proyecto de Visual C# Web Developer con código fuente para acompañar este tema. [Descargue la C# versión](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si prefiere Visual Basic, cambie a la [versión Visual Basic](../vb/intro-to-aspnet-mvc-3.md) de este tutorial.

MVC representa el *controlador de vista de modelos*. MVC es un patrón para desarrollar aplicaciones que están bien diseñadas y fáciles de mantener. Las aplicaciones basadas en MVC contienen:

- Controladores: clases que controlan las solicitudes entrantes a la aplicación, recuperan los datos del modelo y, a continuación, especifican plantillas de vista que devuelven una respuesta al cliente.
- Modelos: clases que representan los datos de la aplicación y que usan la lógica de validación para aplicar las reglas de negocios para esos datos.
- Vistas: archivos de plantilla que usa la aplicación para generar dinámicamente respuestas HTML.

Trataremos todos estos conceptos de esta serie de tutoriales y le mostraremos cómo usarlos para compilar una aplicación.

Comencemos por crear una clase de controlador. En **Explorador de soluciones**, haga clic con el botón secundario en la carpeta *Controllers* y seleccione **Agregar controlador**.

[![](adding-a-controller/_static/image2.png)](adding-a-controller/_static/image1.png)

Asigne al nuevo controlador el nombre "HelloWorldController". Deje la plantilla predeterminada como **controlador vacío** y haga clic en **Agregar**.

[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)

Observe en **Explorador de soluciones** que se ha creado un nuevo archivo llamado *HelloWorldController.CS*. El archivo está abierto en el IDE.

![](adding-a-controller/_static/image5.png)

Dentro del bloque `public class HelloWorldController`, cree dos métodos que tengan un aspecto similar al código siguiente. El controlador devolverá una cadena de HTML como ejemplo.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

El controlador se denomina `HelloWorldController` y el primer método anterior se denomina `Index`. Vamos a invocarlo desde un explorador. Ejecute la aplicación (presione F5 o Ctrl + F5). En el explorador, anexe "HelloWorld" a la ruta de acceso en la barra de direcciones. (Por ejemplo, en la ilustración siguiente, es `http://localhost:43246/HelloWorld.`) La página en el explorador tendrá un aspecto similar al de la captura de pantalla siguiente. En el método anterior, el código devolvió una cadena directamente. Ha indicado al sistema que solo devuelva algún código HTML y lo hizo.

![](adding-a-controller/_static/image6.png)

ASP.NET MVC invoca diferentes clases de controlador (y métodos de acción diferentes) en función de la dirección URL de entrada. La lógica de asignación predeterminada usada por ASP.NET MVC usa un formato como este para determinar qué código debe invocar:

`/[Controller]/[ActionName]/[Parameters]`

La primera parte de la dirección URL determina la clase de controlador que se va a ejecutar. Por lo tanto, */HelloWorld* se asigna a la clase `HelloWorldController`. La segunda parte de la dirección URL determina el método de acción de la clase que se va a ejecutar. Por lo tanto, */HelloWorld/index* haría que se ejecutara el método `Index` de la clase `HelloWorldController`. Tenga en cuenta que solo tuvimos que ir a */HelloWorld* y el método de `Index` se usó de forma predeterminada. Esto se debe a que un método denominado `Index` es el método predeterminado al que se llamará en un controlador si no se especifica uno explícitamente.

Vaya a `http://localhost:xxxx/HelloWorld/Welcome`. El método `Welcome` se ejecuta y devuelve la cadena "This is the Welcome action method..." (Este es el método de acción de bienvenida). La asignación MVC predeterminada es `/[Controller]/[ActionName]/[Parameters]`. Para esta dirección URL, el controlador es `HelloWorld` y `Welcome` es el método de acción. Todavía no ha usado el elemento `[Parameters]` de la dirección URL.

![](adding-a-controller/_static/image7.png)

Vamos a modificar ligeramente el ejemplo para que pueda pasar parte de la información de parámetros de la dirección URL al controlador (por ejemplo, */HelloWorld/Welcome? Name = Scott&amp;numtimes = 4*). Cambie el método de `Welcome` para que incluya dos parámetros, como se muestra a continuación. Tenga en cuenta que el código C# usa la característica opcional-Parameter para indicar que el parámetro `numTimes` debe tener como valor predeterminado 1 si no se pasa ningún valor para ese parámetro.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Ejecute la aplicación y vaya a la dirección URL del ejemplo (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Puede probar distintos valores para `name` y `numtimes` en la dirección URL. El sistema asigna automáticamente los parámetros con nombre de la cadena de consulta de la barra de direcciones a los parámetros del método.

![](adding-a-controller/_static/image8.png)

En ambos ejemplos, el controlador ha estado realizando la parte "VC" de MVC, es decir, la vista y el controlador funcionan. El controlador devuelve HTML directamente. Normalmente, no desea que los controladores devuelvan HTML directamente, ya que es muy engorroso para el código. En su lugar, usaremos normalmente un archivo de plantilla de vista independiente para ayudar a generar la respuesta HTML. Vamos a ver cómo podemos hacerlo.

> [!div class="step-by-step"]
> [Anterior](intro-to-aspnet-mvc-3.md)
> [Siguiente](adding-a-view.md)
