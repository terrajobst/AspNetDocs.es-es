---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Agregando un controlador | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 6b38d757d37374b14979f8a079a46158ff64f9c3
ms.sourcegitcommit: f774732a3960fca079438a88a5472c37cf7be08a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2019
ms.locfileid: "68810763"
---
# <a name="adding-a-controller"></a>Agregar un controlador

por [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](sample/code-location.md)]

MVC representa el *controlador de vista de modelos*. MVC es un patrón para desarrollar aplicaciones que están bien diseñadas, comprobables y fáciles de mantener. Las aplicaciones basadas en MVC contienen:

- **M** odelos: Clases que representan los datos de la aplicación y que usan la lógica de validación para aplicar las reglas de negocios para esos datos.
- **V** istas: Archivos de plantilla que usa la aplicación para generar respuestas HTML dinámicamente.
- **C** ontroladores: Las clases que controlan las solicitudes entrantes del explorador, recuperan los datos del modelo y, a continuación, especifican plantillas de vista que devuelven una respuesta al explorador.

Trataremos todos estos conceptos de esta serie de tutoriales y le mostraremos cómo usarlos para compilar una aplicación.

Comencemos por crear una clase de controlador. En **Explorador de soluciones**, haga clic con el botón secundario en la carpeta *Controladores* y, a continuación, haga clic en **Agregar**y luego en **controlador**.

![](adding-a-controller/_static/image1.png)

En el cuadro de diálogo **Agregar scaffold** , haga clic en **controlador de MVC 5-vacío**y, a continuación, haga clic en **Agregar**.

![](adding-a-controller/_static/image2.png)  

Asigne al nuevo controlador el nombre "HelloWorldController" y haga clic en **Agregar**.

![Agregar controlador](adding-a-controller/_static/image3.png)

Observe en **Explorador de soluciones** que se ha creado un nuevo archivo denominado *HelloWorldController.CS* y una nueva carpeta *Views\HelloWorld*. El controlador está abierto en el IDE.

![](adding-a-controller/_static/image4.png)

Reemplace el contenido del archivo por el código siguiente.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Los métodos de controlador devolverán una cadena de HTML como ejemplo. El controlador se denomina `HelloWorldController` y el primer método se denomina `Index`. Vamos a invocarlo desde un explorador. Ejecute la aplicación (presione F5 o Ctrl + F5). En el explorador, anexe &quot;HelloWorld&quot; a la ruta de acceso en la barra de direcciones. (Por ejemplo, en la ilustración siguiente, `http://localhost:1234/HelloWorld.`es) la página en el explorador tendrá un aspecto similar al de la siguiente captura de pantalla. En el método anterior, el código devolvió una cadena directamente. Ha indicado al sistema que solo devuelva algún código HTML y lo hizo.

![](adding-a-controller/_static/image5.png)

ASP.NET MVC invoca diferentes clases de controlador (y métodos de acción diferentes) en función de la dirección URL de entrada. La lógica de enrutamiento de direcciones URL predeterminada usada por ASP.NET MVC usa un formato como este para determinar qué código debe invocar:

`/[Controller]/[ActionName]/[Parameters]`

El formato para el enrutamiento se establece en el archivo inicio de la *aplicación\_/RouteConfig. CS* .

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

Al ejecutar la aplicación y no proporcionar ningún segmento de dirección URL, el valor predeterminado es el controlador de "Inicio" y el método de acción "índice" especificado en la sección de valores predeterminados del código anterior.

La primera parte de la dirección URL determina la clase de controlador que se va a ejecutar. Por lo tanto,/HelloWorld `HelloWorldController` se asigna a la clase. La segunda parte de la dirección URL determina el método de acción de la clase que se va a ejecutar. Por lo tanto,/HelloWorld/index `Index` haría que el `HelloWorldController` método de la clase se ejecutara. Tenga en cuenta que solo tuvimos que ir a */HelloWorld* y `Index` el método se usó de forma predeterminada. Esto se debe a que un `Index` método denominado es el método predeterminado al que se llamará en un controlador si no se especifica uno explícitamente. La tercera parte del segmento de dirección URL (`Parameters`) es para los datos de ruta. Veremos los datos de ruta más adelante en este tutorial.

Vaya a `http://localhost:xxxx/HelloWorld/Welcome`. El `Welcome` método se ejecuta y devuelve la &quot;cadena. este es el método de acción de bienvenida... &quot;. La asignación MVC predeterminada es `/[Controller]/[ActionName]/[Parameters]`. Para esta dirección URL, el controlador es `HelloWorld` y `Welcome` es el método de acción. Todavía no ha usado el elemento `[Parameters]` de la dirección URL.

![](adding-a-controller/_static/image6.png)

Vamos a modificar ligeramente el ejemplo para que pueda pasar parte de la información de parámetros de la dirección URL al controlador (por ejemplo, */HelloWorld/Welcome? name&amp;= Scott numtimes = 4*). Cambie el `Welcome` método para que incluya dos parámetros, como se muestra a continuación. Tenga en cuenta que el código C# usa la característica opcional-Parameter para indicar `numTimes` que el parámetro debe tener como valor predeterminado 1 si no se pasa ningún valor para ese parámetro.

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> Nota de seguridad: El código anterior usa [HttpUtility. HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) para proteger la aplicación de entradas malintencionadas (es decir, JavaScript). Para obtener más información, vea [Cómo: Proteja contra ataques de scripts en una aplicación web aplicando codificación HTML a las](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx)cadenas.

 Ejecute la aplicación y vaya a la dirección URL del`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`ejemplo (). Puede probar distintos valores para `name` y `numtimes` en la dirección URL. El [sistema de enlace de modelos de ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) asigna automáticamente los parámetros con nombre de la cadena de consulta de la barra de direcciones a los parámetros del método.

![](adding-a-controller/_static/image7.png)

En el ejemplo anterior, el segmento de dirección `Parameters`URL () no se usa `name` , `numTimes` y los parámetros y se pasan como [cadenas de consulta](http://en.wikipedia.org/wiki/Query_string). El carácter comodín ? (signo de interrogación) en la dirección URL anterior es un separador y las cadenas de consulta siguen. El carácter &amp; separa las cadenas de consulta.

Reemplace el método de bienvenida por el código siguiente:

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

Ejecute la aplicación y escriba la siguiente dirección URL:`http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`

![](adding-a-controller/_static/image8.png)

Esta vez, el tercer segmento de dirección URL coincidió `ID.` con `Welcome` el parámetro de ruta, el`ID`método de acción contiene un parámetro () que `RegisterRoutes` coincidía con la especificación de la dirección URL en el método.

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

En las aplicaciones ASP.NET MVC, es más habitual pasar parámetros como datos de ruta (como hicimos con el identificador anterior) que pasarlos como cadenas de consulta. También puede Agregar una ruta para pasar los `name` parámetros y `numtimes` in como datos de ruta en la dirección URL. En el *archivo\_Start\RouteConfig.CS* de la aplicación, agregue la ruta "Hello":

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

Ejecute la aplicación y vaya a `/localhost:XXX/HelloWorld/Welcome/Scott/3`.

![](adding-a-controller/_static/image9.png)

Para muchas aplicaciones MVC, la ruta predeterminada funciona correctamente. Más adelante en este tutorial aprenderá a pasar datos mediante el enlazador de modelos y no tendrá que modificar la ruta predeterminada para ello.

En estos ejemplos, el controlador ha estado realizando &quot;la&quot; parte VC de MVC, es decir, la vista y el controlador funcionan. El controlador devuelve HTML directamente. Normalmente, no desea que los controladores devuelvan HTML directamente, ya que es muy engorroso para el código. En su lugar, usaremos normalmente un archivo de plantilla de vista independiente para ayudar a generar la respuesta HTML. Vamos a ver cómo podemos hacerlo.

> [!div class="step-by-step"]
> [Anterior](getting-started.md)
> [Siguiente](adding-a-view.md)
