---
uid: web-pages/overview/testing-and-debugging/introduction-to-debugging
title: Introducción a la depuración de sitios de ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: La depuración es el proceso de búsqueda y corrección de errores en las páginas de códigos. En este capítulo se muestran algunas herramientas y técnicas que puede usar para depurar y analyz...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 68de4326-7611-4b9b-b5f6-79b7adc3069f
msc.legacyurl: /web-pages/overview/testing-and-debugging/introduction-to-debugging
msc.type: authoredcontent
ms.openlocfilehash: ae7d871e56326610c043dc20fe6e0919e1b4ac89
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78506461"
---
# <a name="introduction-to-debugging-aspnet-web-pages-razor-sites"></a>Introducción a la depuración de sitios de ASP.NET Web Pages (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explican varias maneras de depurar páginas en un sitio web de ASP.NET Web Pages (Razor). La depuración es el proceso de búsqueda y corrección de errores en las páginas de códigos.
>
> **Lo que aprenderá:**
>
> - Cómo Mostrar información que ayude a analizar y depurar páginas.
> - Cómo usar las herramientas de depuración en Visual Studio.
>
>
> Estas son las características de ASP.NET presentadas en el artículo:
>
> - Aplicación auxiliar de `ServerInfo`.
> - `ObjectInfo` ayudante.
>
>
> ## <a name="software-versions"></a>Versiones de software
>
>
> - ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
>
>
> Este tutorial también funciona con ASP.NET Web Pages 2. Puede usar WebMatrix 3, pero no se admite el depurador integrado.

Un aspecto importante de la solución de problemas y errores en el código es evitarlos en primer lugar. Puede hacerlo colocando secciones del código que probablemente causen errores en `try/catch` bloques. Para obtener más información, vea la sección sobre el control de errores en [Introducción a la programación web de ASP.net mediante la sintaxis de Razor](https://go.microsoft.com/fwlink/?LinkId=202890).

La aplicación auxiliar de `ServerInfo` es una herramienta de diagnóstico que proporciona información general sobre el entorno del servidor Web que hospeda la página. También se muestra la información de la solicitud HTTP que se envía cuando un explorador solicita la página. La aplicación auxiliar de `ServerInfo` muestra la identidad del usuario actual, el tipo de explorador que realizó la solicitud, etc. Este tipo de información puede ayudarle a solucionar problemas comunes.

1. Cree una nueva página web denominada *ServerInfo. cshtml*.
2. Al final de la página, justo antes de la etiqueta de cierre `</body>`, agregue `@ServerInfo.GetHtml()`:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample1.cshtml)]

    Puede Agregar el código de `ServerInfo` en cualquier parte de la página. Pero agregarlo al final mantendrá su salida independiente del contenido de la página, lo que facilita su lectura.

    > [!NOTE]
    >
    > **Importante** Debe quitar cualquier código de diagnóstico de las páginas web antes de trasladar las páginas web a un servidor de producción. Esto se aplica a la aplicación auxiliar de `ServerInfo`, así como a las otras técnicas de diagnóstico de este artículo que implican la adición de código a una página. No quiere que los visitantes del sitio Web vean información sobre el nombre del servidor, los nombres de usuario, las rutas de acceso del servidor y detalles similares, ya que este tipo de información puede ser útil para las personas con intenciones maliciosas.
3. Guarde la página y ejecútela en un explorador.

    ![Depuración-1](introduction-to-debugging/_static/image1.jpg)

    En la aplicación auxiliar de `ServerInfo` se muestran cuatro tablas de información en la página:

   - Configuración del servidor. En esta sección se proporciona información acerca del servidor Web de hospedaje, incluido el nombre del equipo, la versión de ASP.NET que se está ejecutando, el nombre de dominio y la hora del servidor.
   - Variables de servidor ASP.NET. En esta sección se proporcionan detalles sobre los numerosos detalles del protocolo HTTP (denominados variables HTTP) y los valores que forman parte de cada solicitud de página web.
   - Información del tiempo de ejecución de HTTP. En esta sección se proporcionan detalles sobre la versión de Microsoft .NET Framework en la que se ejecuta la página web, la ruta de acceso, los detalles de la memoria caché, etc. (Como aprendió en la [Introducción a la programación web de ASP.net mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkId=202890), ASP.NET Web pages el uso de la sintaxis Razor se basa en la tecnología de servidor Web ASP.net de Microsoft, que se basa en una amplia biblioteca de desarrollo de software denominada .NET Framework).
   - Variables de entorno. En esta sección se proporciona una lista de todas las variables de entorno locales y sus valores en el servidor Web.

     Una descripción completa de toda la información del servidor y de la solicitud queda fuera del ámbito de este artículo, pero puede ver que la aplicación auxiliar de `ServerInfo` devuelve una gran cantidad de información de diagnóstico. Para obtener más información sobre los valores que `ServerInfo` devuelve, vea [variables de entorno reconocidas](https://technet.microsoft.com/library/dd560744(WS.10).aspx) en el sitio web de Microsoft TechNet y [variables de servidor IIS](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) en el sitio web de MSDN.

## <a name="embedding-output-expressions-to-display-page-values"></a>Incrustar expresiones de salida para mostrar valores de página

Otra manera de ver lo que sucede en el código es insertar expresiones de salida en la página. Como sabe, puede generar directamente el valor de una variable agregando algo como `@myVariable` o `@(subTotal * 12)` a la página. Para la depuración, puede colocar estas expresiones de salida en puntos estratégicos del código. Esto le permite ver el valor de las variables clave o el resultado de los cálculos cuando se ejecuta la página. Cuando haya terminado la depuración, puede quitar las expresiones o comentarlos. En este procedimiento se muestra una forma típica de utilizar expresiones incrustadas para ayudar a depurar una página.

1. Cree una nueva página de WebMatrix denominada *OutputExpression. cshtml*.
2. Reemplace el contenido de la página por lo siguiente:

    [!code-html[Main](introduction-to-debugging/samples/sample2.html)]

    En el ejemplo se usa una instrucción `switch` para comprobar el valor de la variable `weekday` y, a continuación, mostrar un mensaje de salida diferente en función del día de la semana en el que se encuentra. En el ejemplo, el bloque `if` dentro del primer bloque de código cambia arbitrariamente el día de la semana agregando un día al valor actual de la semana. Se trata de un error que se presentó con fines ilustrativos.
3. Guarde la página y ejecútela en un explorador.

    La página muestra el mensaje para el día de la semana equivocado. Sea cual sea el día de la semana en que se encuentra realmente, verá el mensaje durante un día más tarde. Aunque en este caso sabe por qué el mensaje está desactivado (dado que el código establece deliberadamente el valor de día incorrecto), en realidad es difícil saber dónde está el problema en el código. Para depurar, debe averiguar lo que está sucediendo en el valor de los objetos clave y variables como `weekday`.
4. Agregue expresiones de salida insertando `@weekday` como se muestra en los dos lugares indicados por los comentarios en el código. Estas expresiones de salida mostrarán los valores de la variable en ese momento en la ejecución del código.

    [!code-csharp[Main](introduction-to-debugging/samples/sample3.cs?highlight=2-3,15-16)]
5. Guarde y ejecute la página en un explorador.

    En primer lugar, la página muestra el día real de la semana, el día de la semana actualizado que es el resultado de sumar un día y, a continuación, el mensaje resultante de la instrucción `switch`. La salida de las dos expresiones variables (`@weekday`) no tiene espacios entre los días porque no ha agregado ninguna etiqueta HTML `<p>` a la salida; las expresiones son solo para pruebas.

    ![Depuración-2](introduction-to-debugging/_static/image2.jpg)

    Ahora puede ver dónde se encuentra el error. La primera vez que se muestra la variable `weekday` en el código, se muestra el día correcto. Cuando se muestra la segunda vez, después del `if` bloque en el código, el día está desactivado en uno. Por lo tanto, sabe que se ha producido algo entre la primera y la segunda apariencia de la variable Weekday. Si se trata de un error real, este tipo de enfoque le ayudaría a reducir la ubicación del código que está causando el problema.
6. Corrija el código de la página quitando las dos expresiones de salida que ha agregado y quitando el código que cambia el día de la semana. El bloque de código completo restante es similar al del ejemplo siguiente:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample4.cshtml)]
7. Ejecute la página en un explorador. Esta vez verá el mensaje correcto que se muestra para el día real de la semana.

## <a name="using-the-objectinfo-helper-to-display-object-values"></a>Usar la aplicación auxiliar ObjectInfo para mostrar valores de objeto

La aplicación auxiliar de `ObjectInfo` muestra el tipo y el valor de cada objeto que se le pasa. Puede usarlo para ver el valor de las variables y los objetos del código (como hizo con las expresiones de salida en el ejemplo anterior), además de ver información sobre el tipo de datos sobre el objeto.

1. Abra el archivo denominado *OutputExpression. cshtml* que creó anteriormente.
2. Reemplace todo el código de la página por el siguiente bloque de código:

    [!code-html[Main](introduction-to-debugging/samples/sample5.html)]
3. Guarde y ejecute la página en un explorador.

    ![Depuración-4](introduction-to-debugging/_static/image3.jpg)

    En este ejemplo, la aplicación auxiliar de `ObjectInfo` muestra dos elementos:

   - Tipo. En el caso de la primera variable, el tipo es `DayOfWeek`. Para la segunda variable, el tipo es `String`.
   - Valor. En este caso, dado que ya se muestra el valor de la variable greeting en la página, el valor se muestra de nuevo cuando se pasa la variable a `ObjectInfo`.

     En el caso de objetos más complejos, el ayudante de `ObjectInfo` puede &#8212; Mostrar más información básicamente, puede mostrar los tipos y valores de todas las propiedades de un objeto.

## <a name="using-debugging-tools-in-visual-studio"></a>Usar las herramientas de depuración en Visual Studio

Para obtener una experiencia de depuración más completa, use [Visual Studio](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). Con Visual Studio, puede establecer un punto de interrupción en el código en la línea que desea inspeccionar.

![establecer punto de interrupción](introduction-to-debugging/_static/image1.png)

Al probar el sitio web, el código que se está ejecutando se detiene en el punto de interrupción.

![punto de interrupción de alcance](introduction-to-debugging/_static/image2.png)

Puede examinar los valores actuales de las variables y recorrer el código línea a línea.

![ver valores](introduction-to-debugging/_static/image3.png)

Para obtener información sobre cómo usar el depurador integrado en Visual Studio para depurar páginas de Razor ASP.NET, consulte [programación de ASP.NET Web pages (Razor) con Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854).

## <a name="additional-resources"></a>Recursos adicionales

- [ASP.NET Web Pages de programación (Razor) con Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854)
- [Variables de servidor IIS](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) (MSDN)
- [Variables de entorno reconocidas](https://technet.microsoft.com/library/dd560744(WS.10).aspx) (TechNet)
