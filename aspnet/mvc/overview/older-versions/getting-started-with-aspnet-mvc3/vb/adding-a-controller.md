---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: Agregar un controlador (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web MVC de ASP.NET con Microsoft Visual Web Developer 2010 Express Service Pack 1, que es...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 2e77f62a9796211b0e59a99c71bc532659b7cb92
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457417"
---
# <a name="adding-a-controller-vb"></a>Agregar un controlador (VB)

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web MVC de ASP.NET con Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos que se enumeran a continuación. Para instalar todos ellos, haga clic en el vínculo siguiente: [instalador de plataforma web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar individualmente los requisitos previos con los siguientes vínculos:
> 
> - [Requisitos previos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(compatibilidad con herramientas y tiempo de ejecución)
> 
> Si utiliza Visual Studio 2010 en lugar de Visual Web Developer 2010, instale los requisitos previos haciendo clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un proyecto de Visual Web Developer con código fuente de VB.NET está disponible para acompañar este tema. [Descargue la versión de VB.net](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si lo prefiere C#, cambie a la [ C# versión](../cs/adding-a-controller.md) de este tutorial.

MVC representa el *controlador de vista de modelos*. MVC es un patrón para desarrollar aplicaciones de modo que cada parte tenga una responsabilidad independiente:

- Modelo: los datos de la aplicación.
- Vistas: los archivos de plantilla que usará la aplicación para generar dinámicamente respuestas HTML.
- Controladores: clases que controlan las solicitudes de dirección URL de entrada a la aplicación, recuperan los datos del modelo y, a continuación, especifican plantillas de vista que representan una respuesta al cliente.

Trataremos todos estos conceptos en este tutorial y le mostraremos cómo usarlos para compilar una aplicación.

Cree un controlador nuevo; para ello, haga clic con el botón secundario en la carpeta *Controllers* en **Explorador de soluciones** y, a continuación, seleccione **Agregar controlador**.

[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)

Asigne al nuevo controlador el nombre &quot;HelloWorldController&quot; y haga clic en **Agregar**.

[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)

Observe en **Explorador de soluciones** a la derecha que se ha creado un nuevo archivo llamado *HelloWorldController.CS* y que el archivo está abierto en el IDE.

Dentro del nuevo bloque de `public class HelloWorldController`, cree dos nuevos métodos que se parezcan al código siguiente. Se devolverá una cadena de HTML directamente desde el controlador como ejemplo.

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

El controlador se denomina `HelloWorldController` y el nuevo método se denomina `Index`. Ejecute la aplicación (presione F5 o Ctrl + F5). Una vez que se haya iniciado el explorador, anexe &quot;HelloWorld&quot; a la ruta de acceso en la barra de direcciones. (En mi PC, es `http://localhost:43246/HelloWorld`) El explorador se parecerá a la siguiente captura de pantalla. En el método anterior, el código devolvió una cadena directamente. Le indicamos al sistema que solo devuelva algún código HTML y que lo hizo.

![](adding-a-controller/_static/image5.png)

ASP.NET MVC invoca diferentes clases de controlador (y métodos de acción diferentes) en función de la dirección URL de entrada. La lógica de asignación predeterminada usada por ASP.NET MVC usa un formato como este para controlar qué código se invoca:

`/[Controller]/[ActionName]/[Parameters]`

La primera parte de la dirección URL determina la clase de controlador que se va a ejecutar. Por lo tanto, */HelloWorld* se asigna a la clase `HelloWorldController`. La segunda parte de la dirección URL determina el método de acción de la clase que se va a ejecutar. Por lo tanto, */HelloWorld/index* haría que se ejecutara el método `Index` de la clase `HelloWorldController`. Tenga en cuenta que solo tuvimos que visitar */HelloWorld* y el método de `Index` se usó de forma predeterminada. Esto se debe a que un método denominado `Index` es el método predeterminado al que se llamará en un controlador si no se especifica uno explícitamente.

Vaya a `http://localhost:xxxx/HelloWorld/Welcome`. El método `Welcome` se ejecuta y devuelve la cadena &quot;este es el método de acción de bienvenida...&quot;. La asignación MVC predeterminada es `/[Controller]/[ActionName]/[Parameters]`. Para esta dirección URL, el controlador se `HelloWorld` y `Welcome` es el método. Todavía no hemos usado la parte `[Parameters]` de la dirección URL.

![](adding-a-controller/_static/image6.png)

Vamos a modificar ligeramente el ejemplo para que podamos pasar parte de la información de los parámetros de la dirección URL al controlador (por ejemplo, */HelloWorld/Welcome? Name = Scott&amp;numtimes = 4*). Cambie el método de `Welcome` para que incluya dos parámetros, como se muestra a continuación. Tenga en cuenta que hemos usado la característica de parámetros opcionales de VB para indicar que el parámetro `numTimes` debe tener como valor predeterminado 1 si no se pasa ningún valor para ese parámetro.

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

Ejecute la aplicación y vaya a `http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4` **.** Puede probar valores diferentes para `name` y `numtimes`. El sistema asigna automáticamente los parámetros con nombre de la cadena de consulta en la barra de direcciones a los parámetros del método.

![](adding-a-controller/_static/image7.png)

En ambos ejemplos, el controlador ha estado realizando la parte VC de MVC, que es el trabajo de la vista y el controlador. El controlador devuelve HTML directamente. Normalmente, no se desea que los controladores devuelvan HTML directamente, ya que es muy engorroso para el código. En su lugar, usaremos normalmente un archivo de plantilla de vista independiente para ayudar a generar la respuesta HTML. Veamos cómo se puede hacer esto.

> [!div class="step-by-step"]
> [Anterior](intro-to-aspnet-mvc-3.md)
> [Siguiente](adding-a-view.md)
