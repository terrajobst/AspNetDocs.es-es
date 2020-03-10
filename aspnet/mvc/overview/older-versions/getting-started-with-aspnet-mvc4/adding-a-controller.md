---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: Agregando un controlador | Microsoft Docs
author: Rick-Anderson
description: 'Nota: hay disponible una versión actualizada de este tutorial que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro, mucho más fácil de seguir y demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: f528c56435976c7f31fce453c834ef9eaebe6244
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78485449"
---
# <a name="adding-a-controller"></a>Agregar un controlador

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > [Hay disponible una](../../getting-started/introduction/getting-started.md) versión actualizada de este tutorial que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro, mucho más fácil de seguir y demuestra más características.

MVC representa el *controlador de vista de modelos*. MVC es un patrón para desarrollar aplicaciones que están bien diseñadas, comprobables y fáciles de mantener. Las aplicaciones basadas en MVC contienen:

- **M** odelos: clases que representan los datos de la aplicación y que usan la lógica de validación para aplicar las reglas de negocios para esos datos.
- **V** istas: archivos de plantilla que usa la aplicación para generar dinámicamente respuestas HTML.
- **C** Ontroladores: clases que controlan las solicitudes entrantes del explorador, recuperan los datos del modelo y, a continuación, especifican plantillas de vista que devuelven una respuesta al explorador.

Trataremos todos estos conceptos de esta serie de tutoriales y le mostraremos cómo usarlos para compilar una aplicación.

Comencemos por crear una clase de controlador. En **Explorador de soluciones**, haga clic con el botón secundario en la carpeta *Controllers* y seleccione **Agregar controlador**.

![](adding-a-controller/_static/image1.png)

Asigne al nuevo controlador el nombre &quot;HelloWorldController&quot;. Deje la plantilla predeterminada como **controlador MVC vacío** y haga clic en **Agregar**.

![Agregar controlador](adding-a-controller/_static/image2.png)

Observe en **Explorador de soluciones** que se ha creado un nuevo archivo llamado *HelloWorldController.CS*. El archivo está abierto en el IDE.

![](adding-a-controller/_static/image3.png)

Reemplace el contenido del archivo por el código siguiente.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Los métodos de controlador devolverán una cadena de HTML como ejemplo. El controlador se denomina `HelloWorldController` y el primer método anterior se denomina `Index`. Vamos a invocarlo desde un explorador. Ejecute la aplicación (presione F5 o Ctrl + F5). En el explorador, anexe &quot;HelloWorld&quot; a la ruta de acceso en la barra de direcciones. (Por ejemplo, en la ilustración siguiente, es `http://localhost:1234/HelloWorld.`) La página en el explorador tendrá un aspecto similar al de la captura de pantalla siguiente. En el método anterior, el código devolvió una cadena directamente. Ha indicado al sistema que solo devuelva algún código HTML y lo hizo.

![](adding-a-controller/_static/image4.png)

ASP.NET MVC invoca diferentes clases de controlador (y métodos de acción diferentes) en función de la dirección URL de entrada. La lógica de enrutamiento de direcciones URL predeterminada usada por ASP.NET MVC usa un formato como este para determinar qué código debe invocar:

`/[Controller]/[ActionName]/[Parameters]`

La primera parte de la dirección URL determina la clase de controlador que se va a ejecutar. Por lo tanto, */HelloWorld* se asigna a la clase `HelloWorldController`. La segunda parte de la dirección URL determina el método de acción de la clase que se va a ejecutar. Por lo tanto, */HelloWorld/index* haría que se ejecutara el método `Index` de la clase `HelloWorldController`. Tenga en cuenta que solo tuvimos que ir a */HelloWorld* y el método de `Index` se usó de forma predeterminada. Esto se debe a que un método denominado `Index` es el método predeterminado al que se llamará en un controlador si no se especifica uno explícitamente.

Vaya a `http://localhost:xxxx/HelloWorld/Welcome`. El método `Welcome` se ejecuta y devuelve la cadena &quot;este es el método de acción de bienvenida...&quot;. La asignación MVC predeterminada es `/[Controller]/[ActionName]/[Parameters]`. Para esta dirección URL, el controlador es `HelloWorld` y `Welcome` es el método de acción. Todavía no ha usado el elemento `[Parameters]` de la dirección URL.

![](adding-a-controller/_static/image5.png)

Vamos a modificar ligeramente el ejemplo para que pueda pasar parte de la información de parámetros de la dirección URL al controlador (por ejemplo, */HelloWorld/Welcome? Name = Scott&amp;numtimes = 4*). Cambie el método de `Welcome` para que incluya dos parámetros, como se muestra a continuación. Tenga en cuenta que el código C# usa la característica opcional-Parameter para indicar que el parámetro `numTimes` debe tener como valor predeterminado 1 si no se pasa ningún valor para ese parámetro.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Ejecute la aplicación y vaya a la dirección URL del ejemplo (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Puede probar distintos valores para `name` y `numtimes` en la dirección URL. El [sistema de enlace de modelos de ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) asigna automáticamente los parámetros con nombre de la cadena de consulta de la barra de direcciones a los parámetros del método.

![](adding-a-controller/_static/image6.png)

En ambos ejemplos, el controlador ha estado realizando el &quot;VC&quot; parte de MVC, es decir, la vista y el controlador funcionan. El controlador devuelve HTML directamente. Normalmente, no desea que los controladores devuelvan HTML directamente, ya que es muy engorroso para el código. En su lugar, usaremos normalmente un archivo de plantilla de vista independiente para ayudar a generar la respuesta HTML. Vamos a ver cómo podemos hacerlo.

> [!div class="step-by-step"]
> [Anterior](intro-to-aspnet-mvc-4.md)
> [Siguiente](adding-a-view.md)
