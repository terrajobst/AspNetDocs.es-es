---
uid: web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-cs
title: Varios ContentPlaceHolders y contenido predeterminado (C#) | Microsoft Docs
author: rick-anderson
description: Examina cómo agregar varios marcadores de posición de contenido a una página maestra y cómo especificar el contenido predeterminado en los titulares de la ubicación de contenido.
ms.author: riande
ms.date: 05/21/2008
ms.assetid: b9b9798b-027d-46cc-9636-473378e437ac
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-cs
msc.type: authoredcontent
ms.openlocfilehash: e902bcae05c0e7976a20293f2b01e5f2e2bee13a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74639435"
---
# <a name="multiple-contentplaceholders-and-default-content-c"></a>Varios ContentPlaceHolders y contenido predeterminado (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_02_CS.zip) o [Descargar PDF](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_02_CS.pdf)

> Examina cómo agregar varios marcadores de posición de contenido a una página maestra y cómo especificar el contenido predeterminado en los titulares de la ubicación de contenido.

## <a name="introduction"></a>Introducción

En el tutorial anterior, hemos examinado cómo las páginas maestras permiten a los desarrolladores de ASP.NET crear un diseño coherente en todo el sitio. Las páginas maestras definen el marcado que es común a todas sus páginas de contenido y regiones que se pueden personalizar página por página. En el tutorial anterior, creamos una página maestra simple (`Site.master`) y dos páginas de contenido (`Default.aspx` y `About.aspx`). Nuestra página maestra consta de dos ContentPlaceHolders con el nombre `head` y `MainContent`, que se encontraban en el elemento `<head>` y en el formulario web, respectivamente. Aunque cada una de las páginas de contenido tenía dos controles de contenido, solo se especifica el marcado para el que corresponde a `MainContent`.

Como se demuestra en los dos controles de ContentPlaceHolder en `Site.master`, una página maestra puede contener varios ContentPlaceHolders. Lo que es más, la página maestra puede especificar el marcado predeterminado para los controles de ContentPlaceHolder. Una página de contenido y, a continuación, puede especificar su propio marcado o usar el marcado predeterminado. En este tutorial se examina el uso de varios controles de contenido en la página maestra y se muestra cómo definir el marcado predeterminado en los controles ContentPlaceHolder.

## <a name="step-1-adding-additional-contentplaceholder-controls-to-the-master-page"></a>Paso 1: agregar controles ContentPlaceHolder adicionales a la página maestra

Muchos diseños de sitios web contienen varias áreas en la pantalla que se personalizan página por página. `Site.master`, la página maestra que creamos en el tutorial anterior, contiene un solo ContentPlaceHolder en el formulario web denominado `MainContent`. En concreto, este ContentPlaceHolder se encuentra dentro del elemento `mainContent` `<div>`.

En la figura 1 se muestra `Default.aspx` cuando se ve a través de un explorador. La región rodeada en rojo es el marcado específico de la página correspondiente a `MainContent`.

[![la región con círculo muestra el área que se personaliza actualmente en cada página](multiple-contentplaceholders-and-default-content-cs/_static/image2.png)](multiple-contentplaceholders-and-default-content-cs/_static/image1.png)

**Figura 01**: la región con círculo muestra el área que se personaliza actualmente en cada página ([haga clic para ver la imagen a tamaño completo](multiple-contentplaceholders-and-default-content-cs/_static/image3.png))

Imagine que, además de la región que se muestra en la figura 1, también necesitamos agregar elementos específicos de la página a la columna izquierda bajo las secciones lecciones y noticias. Para ello, agregamos otro control ContentPlaceHolder a la página maestra. Para continuar, abra la página maestra de `Site.master` en Visual Web Developer y, a continuación, arrastre un control ContentPlaceHolder desde el cuadro de herramientas hasta el diseñador después de la sección Noticias. Establezca el `ID` de ContentPlaceHolder en `LeftColumnContent`.

[![agregar un control ContentPlaceHolder a la columna izquierda de la página maestra](multiple-contentplaceholders-and-default-content-cs/_static/image5.png)](multiple-contentplaceholders-and-default-content-cs/_static/image4.png)

**Figura 02**: agregar un control ContentPlaceHolder a la columna izquierda de la página maestra ([haga clic para ver la imagen de tamaño completo](multiple-contentplaceholders-and-default-content-cs/_static/image6.png))

Con la adición del `LeftColumnContent` ContentPlaceHolder en la página maestra, podemos definir el contenido de esta región página por página incluyendo un control de contenido en la página cuya `ContentPlaceHolderID` está establecida en `LeftColumnContent`. Examinamos este proceso en el paso 2.

## <a name="step-2-defining-content-for-the-new-contentplaceholder-in-the-content-pages"></a>Paso 2: definir el contenido del nuevo ContentPlaceHolder en las páginas de contenido

Al agregar una nueva página de contenido al sitio web, Visual Web Developer crea automáticamente un control de contenido en la página para cada ContentPlaceHolder de la página maestra seleccionada. Una vez que haya agregado el `LeftColumnContent` ContentPlaceHolder a nuestra página maestra en el paso 1, las nuevas páginas de ASP.NET tendrán ahora tres controles de contenido.

Para ilustrar esto, agregue una nueva página de contenido al directorio raíz denominado `MultipleContentPlaceHolders.aspx` que esté enlazado a la página maestra de `Site.master`. Visual Web Developer crea esta página con el siguiente marcado declarativo:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample1.aspx)]

Escriba parte del contenido en el control de contenido que hace referencia al `MainContent` ContentPlaceHolders (`Content2`). A continuación, agregue el marcado siguiente al control de contenido `Content3` (que hace referencia a la `LeftColumnContent` ContentPlaceHolder):

[!code-html[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample2.html)]

Después de agregar este marcado, visite la página a través de un explorador. Como se muestra en la figura 3, el marcado colocado en el control de contenido de `Content3` se muestra en la columna izquierda, debajo de la sección Noticias (con un círculo en rojo). El marcado colocado en `Content2` se muestra en la parte derecha de la página (con un círculo en azul).

[![la columna izquierda incluye ahora contenido específico de la página bajo la sección Noticias](multiple-contentplaceholders-and-default-content-cs/_static/image8.png)](multiple-contentplaceholders-and-default-content-cs/_static/image7.png)

**Figura 03**: ahora, la columna izquierda incluye contenido específico de la página bajo la sección Noticias ([haga clic para ver la imagen a tamaño completo](multiple-contentplaceholders-and-default-content-cs/_static/image9.png))

### <a name="defining-content-in-existing-content-pages"></a>Definir el contenido de las páginas de contenido existentes

Al crear una nueva página de contenido, se incorpora automáticamente el control ContentPlaceHolder que hemos agregado en el paso 1. Pero nuestras dos páginas de contenido existentes (`About.aspx` y `Default.aspx`) no tienen un control de contenido para el `LeftColumnContent` ContentPlaceHolder. Para especificar el contenido de este ContentPlaceHolder en estas dos páginas existentes, es necesario agregar un control de contenido.

A diferencia de la mayoría de los controles Web ASP.NET, el cuadro de herramientas Visual Web Developer no incluye un elemento de control de contenido. Podemos escribir manualmente el marcado declarativo del control de contenido en la vista de código fuente, pero un enfoque más sencillo y rápido es usar el Vista de diseño. Abra la página `About.aspx` y cambie al Vista de diseño. Como se muestra en la figura 4, el `LeftColumnContent` ContentPlaceHolder aparece en el Vista de diseño; al pasar el mouse sobre él, el título mostrado es: "LeftColumnContent (Master)". La inclusión de "Master" en el título indica que no hay ningún control de contenido definido en la página para este ContentPlaceHolder. Si existe un control de contenido para el ContentPlaceHolder, como en el caso de `MainContent`, el título leerá: "*ContentPlaceHolderID* (personalizado)".

Para agregar un control de contenido para el `LeftColumnContent` ContentPlaceHolder a `About.aspx`, expanda la etiqueta inteligente de ContentPlaceHolder y haga clic en el vínculo crear contenido personalizado.

[![la vista de diseño de about. aspx muestra el LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-cs/_static/image11.png)](multiple-contentplaceholders-and-default-content-cs/_static/image10.png)

**Figura 04**: la vista de diseño de `About.aspx` muestra el `LeftColumnContent` ContentPlaceHolder ([haga clic para ver la imagen de tamaño completo](multiple-contentplaceholders-and-default-content-cs/_static/image12.png))

Al hacer clic en el vínculo crear contenido personalizado, se genera el control de contenido necesario en la página y se establece su propiedad `ContentPlaceHolderID` en el `ID`de ContentPlaceHolder. Por ejemplo, al hacer clic en el vínculo crear contenido personalizado para `LeftColumnContent` región de `About.aspx` se agrega el siguiente marcado declarativo a la página:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample3.aspx)]

### <a name="omitting-content-controls"></a>Omitir controles de contenido

ASP.NET no requiere que todas las páginas de contenido incluyan controles de contenido para cada una de las ContentPlaceHolder definidas en la página maestra. Si se omite un control de contenido, el motor ASP.NET utiliza el marcado definido dentro de ContentPlaceHolder en la página maestra. Este marcado se conoce como el *contenido predeterminado* de ContentPlaceHolder y es útil en escenarios en los que el contenido de una región es común entre la mayoría de las páginas, pero debe personalizarse para un número reducido de páginas. En el paso 3 se explora la especificación del contenido predeterminado en la página maestra.

Actualmente, `Default.aspx` contiene dos controles de contenido para los `head` y `MainContent` ContentPlaceHolders; no tiene un control de contenido para `LeftColumnContent`. Por consiguiente, cuando se representa `Default.aspx` se usa el contenido predeterminado de ContentPlaceHolder `LeftColumnContent`. Dado que aún no se ha definido ningún contenido predeterminado para este ContentPlaceHolder, el efecto neto es que no se emite ningún marcado para esta región. Para comprobar este comportamiento, visite `Default.aspx` a través de un explorador. Como se muestra en la figura 5, no se emite ningún marcado en la columna izquierda bajo la sección Noticias.

[![no se representa ningún contenido para el LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-cs/_static/image14.png)](multiple-contentplaceholders-and-default-content-cs/_static/image13.png)

**Figura 05**: no se representa ningún contenido para el `LeftColumnContent` ContentPlaceHolder ([haga clic para ver la imagen de tamaño completo](multiple-contentplaceholders-and-default-content-cs/_static/image15.png))

## <a name="step-3-specifying-default-content-in-the-master-page"></a>Paso 3: especificar el contenido predeterminado en la página maestra

Algunos diseños de sitios web incluyen una región cuyo contenido es el mismo para todas las páginas del sitio, excepto una o dos excepciones. Considere un sitio web que admita cuentas de usuario. Este tipo de sitio requiere una página de inicio de sesión en la que los visitantes pueden escribir sus credenciales para iniciar sesión en el sitio. Para agilizar el proceso de inicio de sesión, los diseñadores de sitios web pueden incluir cuadros de texto de nombre de usuario y contraseña en la esquina superior izquierda de cada página para permitir que los usuarios inicien sesión sin tener que visitar explícitamente la página de inicio de sesión. Aunque estos cuadros de texto de nombre de usuario y contraseña son útiles en la mayoría de las páginas, son redundantes en la página de inicio de sesión, que ya contiene cuadros de texto para las credenciales del usuario.

Para implementar este diseño, puede crear un control ContentPlaceHolder en la esquina superior izquierda de la página maestra. Cada página que necesitaba mostrar los cuadros de texto nombre de usuario y contraseña en la esquina superior izquierda crearía un control de contenido para este ContentPlaceHolder y agregaría la interfaz necesaria. La página de inicio de sesión, por otro lado, omitiría agregar un control de contenido para este ContentPlaceHolder o crearía un control de contenido sin marcado definido. El inconveniente de este enfoque es que tenemos que recordar agregar los cuadros de texto nombre de usuario y contraseña a todas las páginas que se agreguen al sitio (excepto la página de inicio de sesión). Esto le pide problemas. Es probable que se olvide de agregar estos cuadros de texto a una página o de dos o, lo que es peor, es posible que no implemente la interfaz correctamente (quizás solo agregue un cuadro de texto en lugar de dos).

Una solución mejor es definir los cuadros de texto de nombre de usuario y contraseña como el contenido predeterminado de ContentPlaceHolder. Al hacerlo, solo es necesario invalidar este contenido predeterminado en aquellas pocas páginas que no muestran los cuadros de texto nombre de usuario y contraseña (la página de inicio de sesión, por ejemplo). Para ilustrar la especificación del contenido predeterminado de un control ContentPlaceHolder, vamos a implementar el escenario que se acaba de describir.

> [!NOTE]
> En el resto de este tutorial se actualiza el sitio web para incluir una interfaz de inicio de sesión en la columna izquierda para todas las páginas, pero la página de inicio de sesión. Sin embargo, en este tutorial no se examina cómo configurar el sitio web para admitir cuentas de usuario. Para obtener más información sobre este tema, consulte los tutoriales sobre la [autenticación de formularios, la autorización, las cuentas de usuario y los roles](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) .

### <a name="adding-a-contentplaceholder-and-specifying-its-default-content"></a>Agregar un ContentPlaceHolder y especificar su contenido predeterminado

Abra la página maestra de `Site.master` y agregue el siguiente marcado a la columna izquierda entre la etiqueta `DateDisplay` y la sección lecciones:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample4.aspx)]

Después de agregar este marcado, el Vista de diseño de la página maestra debe ser similar a la figura 6.

[![la página maestra incluye un control de inicio de sesión](multiple-contentplaceholders-and-default-content-cs/_static/image17.png)](multiple-contentplaceholders-and-default-content-cs/_static/image16.png)

**Figura 06**: la página maestra incluye un control de inicio de sesión ([haga clic para ver la imagen de tamaño completo](multiple-contentplaceholders-and-default-content-cs/_static/image18.png))

Este ContentPlaceHolder, `QuickLoginUI`, tiene un control Web de inicio de sesión como contenido predeterminado. El control de inicio de sesión muestra una interfaz de usuario que solicita al usuario su nombre de usuario y contraseña, junto con un botón iniciar sesión. Al hacer clic en el botón iniciar sesión, el control de inicio de sesión valida internamente las credenciales del usuario con la API de pertenencia. Para usar este control de inicio de sesión en la práctica, debe configurar el sitio para que use la pertenencia. Este tema está fuera del ámbito de este tutorial; Consulte los tutoriales de [autenticación de formularios, autorización, cuentas de usuario y roles](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) para obtener más información sobre cómo crear una aplicación web que admita cuentas de usuario.

No dude en personalizar la apariencia o el comportamiento del control de inicio de sesión. He establecido dos de sus propiedades: `TitleText` y `FailureAction`. El valor de la propiedad `TitleText`, cuyo valor predeterminado es "iniciar sesión", se muestra en la parte superior de la interfaz de usuario del control. He establecido esta propiedad para que muestre el texto "iniciar sesión" como un elemento `<h3>`. La propiedad `FailureAction` indica qué hacer si las credenciales del usuario no son válidas. Su valor predeterminado es `Refresh`, que deja al usuario en la misma página y muestra un mensaje de error en el control de inicio de sesión. Lo he cambiado a `RedirectToLoginPage`, que envía al usuario a la página de inicio de sesión en caso de credenciales no válidas. Prefiero enviar al usuario a la página de inicio de sesión cuando un usuario intenta iniciar sesión desde otra página, pero se produce un error, ya que la página de inicio de sesión puede contener instrucciones y opciones adicionales que no caben fácilmente en la columna izquierda. Por ejemplo, la página de inicio de sesión podría incluir opciones para recuperar una contraseña olvidada o para crear una cuenta nueva.

### <a name="creating-the-login-page-and-overriding-the-default-content"></a>Crear la página de inicio de sesión e invalidar el contenido predeterminado

Una vez completada la página maestra, el siguiente paso es crear la página de inicio de sesión. Agregue una página ASP.NET al directorio raíz del sitio denominado `Login.aspx`, enlazándolo a la página maestra de `Site.master`. Al hacerlo, se creará una página con cuatro controles de contenido, uno para cada uno de los ContentPlaceHolders definidos en `Site.master`.

Agregue un control de inicio de sesión al control de contenido `MainContent`. Del mismo modo, no dude en agregar contenido a la región `LeftColumnContent`. Sin embargo, asegúrese de dejar vacío el control de contenido para el `QuickLoginUI` ContentPlaceHolder. Así se asegurará de que el control de inicio de sesión no aparece en la columna izquierda de la página de inicio de sesión.

Después de definir el contenido de las regiones `MainContent` y `LeftColumnContent`, el marcado declarativo de la página de inicio de sesión debe tener un aspecto similar al siguiente:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample5.aspx)]

En la ilustración 7 se muestra esta página cuando se ve a través de un explorador. Dado que esta página especifica un control de contenido para el `QuickLoginUI` ContentPlaceHolder, invalida el contenido predeterminado especificado en la página maestra. El efecto neto es que el control de inicio de sesión que se muestra en la Vista de diseño de la página maestra (vea la figura 6) no se representa en esta página.

[![la página de inicio de sesión, se presiona el contenido predeterminado de QuickLoginUI ContentPlaceHolder](multiple-contentplaceholders-and-default-content-cs/_static/image20.png)](multiple-contentplaceholders-and-default-content-cs/_static/image19.png)

**Figura 07**: la página de inicio de sesión de presiona el contenido predeterminado del `QuickLoginUI` ContentPlaceHolder ([haga clic para ver la imagen de tamaño completo](multiple-contentplaceholders-and-default-content-cs/_static/image21.png))

### <a name="using-the-default-content-in-new-pages"></a>Usar el contenido predeterminado en nuevas páginas

Queremos mostrar el control de inicio de sesión en la columna izquierda para todas las páginas excepto la página de inicio de sesión. Para ello, todas las páginas de contenido excepto la página de inicio de sesión deben omitir un control de contenido para el `QuickLoginUI` ContentPlaceHolder. Al omitir un control de contenido, se usará en su lugar el contenido predeterminado de ContentPlaceHolder.

Las páginas de contenido existentes (`Default.aspx`, `About.aspx`y `MultipleContentPlaceHolders.aspx` no incluyen un control de contenido para `QuickLoginUI` porque se crearon antes de que se agregara el control ContentPlaceHolder a la página maestra. Por lo tanto, no es necesario actualizar estas páginas existentes. Sin embargo, de forma predeterminada, las nuevas páginas que se agregan al sitio web incluyen un control de contenido para el `QuickLoginUI` ContentPlaceHolder. Por lo tanto, tenemos que recordar quitar estos controles de contenido cada vez que se agrega una nueva página de contenido (a menos que se desee reemplazar el contenido predeterminado de ContentPlaceHolder, como en el caso de la página de inicio de sesión).

Para quitar el control de contenido, puede eliminar manualmente su marcado declarativo de la vista de código fuente o, en el Vista de diseño, elegir el vínculo de contenido predeterminado al maestro de la etiqueta inteligente. Cualquier enfoque quita el control de contenido de la página y produce el mismo efecto neto.

En la ilustración 8 se muestra `Default.aspx` cuando se ve a través de un explorador. Recuerde que `Default.aspx` solo tiene dos controles de contenido especificados en su marcado declarativo: uno para `head` y otro para `MainContent`. Como resultado, se muestra el contenido predeterminado para los `LeftColumnContent` y `QuickLoginUI` ContentPlaceHolders.

[![se muestra el contenido predeterminado de LeftColumnContent y QuickLoginUI ContentPlaceHolders](multiple-contentplaceholders-and-default-content-cs/_static/image23.png)](multiple-contentplaceholders-and-default-content-cs/_static/image22.png)

**Figura 08**: se muestra el contenido predeterminado de los `LeftColumnContent` y `QuickLoginUI` ContentPlaceHolders ([haga clic para ver la imagen de tamaño completo](multiple-contentplaceholders-and-default-content-cs/_static/image24.png))

## <a name="summary"></a>Resumen

El modelo de página maestra ASP.NET permite un número arbitrario de ContentPlaceHolders en la página maestra. Lo que es más, ContentPlaceHolders incluye el contenido predeterminado, que se emite en el caso de que no haya ningún control de contenido correspondiente en la página de contenido. En este tutorial, vimos cómo incluir controles ContentPlaceHolder adicionales en la página maestra y cómo definir controles de contenido para estos nuevos ContentPlaceHolders en páginas ASP.NET nuevas y existentes. También se ha analizado la especificación de contenido predeterminado en un ContentPlaceHolder, lo que resulta útil en escenarios donde solo una minoría de páginas necesita personalizar el contenido normalizado de otro modo en una determinada región.

En el siguiente tutorial, examinaremos el `head` ContentPlaceHolder con más detalle, en el que veremos cómo definir de forma declarativa y mediante programación el título, las etiquetas meta y otros encabezados HTML página por página.

¡ Feliz programación!

### <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de varios libros de ASP/ASP. net y fundador de 4GuysFromRolla.com, ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 3,5 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Se puede acceder a Scott en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) o a través de su blog en [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor responsable de este tutorial fue Banerjee. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](creating-a-site-wide-layout-using-master-pages-cs.md)
> [Siguiente](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
