---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-cs
title: Prevención de ataques por inyecciónC#de código JavaScript () | Microsoft Docs
author: StephenWalther
description: Impedir que se produzcan ataques por inyección de código JavaScript y ataques de scripts entre sitios. En este tutorial, Stephen Walther explica cómo puede fácilmente...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: d0136da6-81a4-4815-b002-baa84744c09e
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-cs
msc.type: authoredcontent
ms.openlocfilehash: fb00ee8a7e3d678e824052060eb5d9fd5d4b6a42
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74594836"
---
# <a name="preventing-javascript-injection-attacks-c"></a>Prevenir ataques por inyección de código de JavaScript (C#)

por [Stephen Walther](https://github.com/StephenWalther)

[Descargar PDF](https://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_CS.pdf)

> Impedir que se produzcan ataques por inyección de código JavaScript y ataques de scripts entre sitios. En este tutorial, Stephen Walther explica cómo puede derrotar fácilmente estos tipos de ataques mediante la codificación HTML del contenido.

El objetivo de este tutorial es explicar cómo puede evitar ataques por inyección de código JavaScript en las aplicaciones de ASP.NET MVC. En este tutorial se describen dos métodos para defender el sitio web frente a un ataque por inyección de código JavaScript. Aprenderá a evitar ataques por inyección de código JavaScript mediante la codificación de los datos que se muestran. También aprenderá a evitar ataques por inyección de código JavaScript mediante la codificación de los datos que acepte.

## <a name="what-is-a-javascript-injection-attack"></a>¿Qué es un ataque por inyección de código de JavaScript?

Siempre que acepte la entrada del usuario y vuelva a mostrar la entrada del usuario, abra el sitio web en los ataques de inyección de JavaScript. Vamos a examinar una aplicación concreta que está abierta a ataques de inyección de JavaScript.

Imagine que ha creado un sitio web de comentarios de clientes (vea la ilustración 1). Los clientes pueden visitar el sitio web y escribir sus comentarios sobre su experiencia con sus productos. Cuando un cliente envía sus comentarios, los comentarios se muestran en la página de comentarios.

[![sitio web de comentarios de clientes](preventing-javascript-injection-attacks-cs/_static/image2.png)](preventing-javascript-injection-attacks-cs/_static/image1.png)

**Figura 01**: sitio web de comentarios del cliente ([haga clic para ver la imagen de tamaño completo](preventing-javascript-injection-attacks-cs/_static/image3.png))

El sitio web de comentarios del cliente usa el `controller` en la lista 1. Este `controller` contiene dos acciones denominadas `Index()` y `Create()`.

**Lista 1: `HomeController.cs`**

[!code-csharp[Main](preventing-javascript-injection-attacks-cs/samples/sample1.cs)]

El método `Index()` muestra la vista de `Index`. Este método pasa todos los comentarios del cliente anteriores a la vista `Index` recuperando los comentarios de la base de datos (mediante una consulta LINQ to SQL).

El método `Create()` crea un nuevo elemento de comentarios y lo agrega a la base de datos. El mensaje que escribe el cliente en el formulario se pasa al método `Create()` en el parámetro de mensaje. Se crea un elemento de comentarios y el mensaje se asigna a la propiedad `Message` del elemento de comentarios. El elemento de comentarios se envía a la base de datos con la llamada al método `DataContext.SubmitChanges()`. Por último, el visitante se redirige de nuevo a la vista `Index` en la que se muestran todos los comentarios.

La vista de `Index` está incluida en la lista 2.

**Lista 2: `Index.aspx`**

[!code-aspx[Main](preventing-javascript-injection-attacks-cs/samples/sample2.aspx)]

La vista `Index` tiene dos secciones. La sección superior contiene el formulario real de comentarios del cliente. La sección inferior contiene un para.. Cada bucle recorre en bucle todos los elementos de comentarios de clientes anteriores y muestra las propiedades EntryDate y Message para cada elemento de comentarios.

El sitio web de comentarios del cliente es un sitio web sencillo. Desafortunadamente, el sitio web está abierto a ataques por inyección de JavaScript.

Imagine que escribe el siguiente texto en el formulario de comentarios del cliente:

[!code-html[Main](preventing-javascript-injection-attacks-cs/samples/sample3.html)]

Este texto representa un script de JavaScript que muestra un cuadro de mensaje de alerta. Una vez que alguien envía este script al formulario de comentarios, el mensaje <em>Boo!</em> aparecerá cada vez que alguien visite el sitio web de comentarios del cliente en el futuro (consulte la figura 2).

[Inyección de JavaScript ![](preventing-javascript-injection-attacks-cs/_static/image5.png)](preventing-javascript-injection-attacks-cs/_static/image4.png)

**Figura 02**: inyección de JavaScript ([haga clic para ver la imagen de tamaño completo](preventing-javascript-injection-attacks-cs/_static/image6.png))

Ahora, la respuesta inicial a los ataques por inyección de código de JavaScript podría ser Apathy. Podría pensar que los ataques por inyección de código de JavaScript son simplemente un tipo de ataque de *desesfera* . Podría creer que nadie puede hacer nada realmente mal mediante la confirmación de un ataque por inyección de código JavaScript.

Desafortunadamente, un pirata informático puede realizar algunas cosas realmente, realmente mal, insertando JavaScript en un sitio Web. Puede usar un ataque por inyección de código de JavaScript para realizar un ataque de scripting entre sitios (XSS). En un ataque de scripting entre sitios, se roba información confidencial del usuario y se envía la información a otro sitio Web.

Por ejemplo, un pirata informático puede usar un ataque de inyección de JavaScript para robar los valores de las cookies del explorador de otros usuarios. Si la información confidencial (por ejemplo, contraseñas, números de tarjetas de crédito o números de la seguridad social) se almacena en las cookies del explorador, un pirata informático podría usar un ataque de inyección de JavaScript para robar esta información. O bien, si un usuario escribe información confidencial en un campo de formulario contenido en una página que se ha visto comprometida con un ataque de JavaScript, el pirata informático puede usar el código JavaScript insertado para captar los datos del formulario y enviarlos a otro sitio Web.

Vaya a *mezclarlas*. Tome serias ataques por inyección de código JavaScript y proteja la información confidencial del usuario. En las dos secciones siguientes, se describen dos técnicas que se pueden usar para defender las aplicaciones ASP.NET MVC de ataques de inyección de JavaScript.

## <a name="approach-1-html-encode-in-the-view"></a>Enfoque #1: codificación HTML en la vista

Un método sencillo para evitar ataques por inyección de código JavaScript es codificar en HTML los datos introducidos por los usuarios del sitio web al volver a mostrar los datos en una vista. La vista de `Index` actualizada de la lista 3 sigue este enfoque.

**Lista 3: `Index.aspx` (codificación HTML)**

[!code-aspx[Main](preventing-javascript-injection-attacks-cs/samples/sample4.aspx)]

Observe que el valor de `feedback.Message` se codifica en HTML antes de que se muestre el valor con el código siguiente:

[!code-aspx[Main](preventing-javascript-injection-attacks-cs/samples/sample5.aspx)]

¿Qué significa codificar en HTML una cadena? Cuando se codifica una cadena en HTML, los caracteres peligrosos como `<` y `>` se reemplazan por referencias de entidad HTML como `&lt;` y `&gt;`. Por tanto, cuando la cadena `<script>alert("Boo!")</script>` está codificada en HTML, se convierte en `&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`. La cadena codificada ya no se ejecuta como un script de JavaScript cuando lo interpreta un explorador. En su lugar, se obtiene la página inofensivo en la figura 3.

[Ataque de JavaScript de ![derrotado](preventing-javascript-injection-attacks-cs/_static/image8.png)](preventing-javascript-injection-attacks-cs/_static/image7.png)

**Figura 03**: ataque de derrotas de JavaScript ([haga clic para ver la imagen de tamaño completo](preventing-javascript-injection-attacks-cs/_static/image9.png))

Tenga en cuenta que en la vista de `Index` de la lista 3 solo se codifica el valor de `feedback.Message`. El valor de `feedback.EntryDate` no está codificado. Solo tiene que codificar los datos escritos por un usuario. Dado que el valor de EntryDate se generó en el controlador, no es necesario codificar en HTML este valor.

## <a name="approach-2-html-encode-in-the-controller"></a>Enfoque #2: codificación HTML en el controlador

En lugar de codificar en HTML los datos al mostrar los datos en una vista, puede codificar en HTML los datos justo antes de enviarlos a la base de datos. Este segundo enfoque se toma en el caso del `controller` de la lista 4.

**Lista 4: `HomeController.cs` (codificación HTML)**

[!code-csharp[Main](preventing-javascript-injection-attacks-cs/samples/sample6.cs)]

Tenga en cuenta que el valor del mensaje se codifica en HTML antes de que el valor se envíe a la base de datos dentro de la acción `Create()`. Cuando el mensaje se vuelve a mostrar en la vista, el mensaje se codifica en HTML y no se ejecuta ningún JavaScript insertado en el mensaje.

Normalmente, debe favorecer el primer enfoque descrito en este tutorial en este segundo enfoque. El problema con este segundo enfoque es que termina con los datos codificados en HTML en la base de datos. En otras palabras, los datos de la base de datos se traspasan con caracteres de aspecto divertido.

¿Por qué es malo? Si alguna vez necesita mostrar los datos de la base de datos en algo distinto de una página web, tendrá problemas. Por ejemplo, ya no puede mostrar fácilmente los datos en una aplicación Windows Forms.

## <a name="summary"></a>Resumen

El objetivo de este tutorial era SCARE sobre el potencial de un ataque por inyección de código JavaScript. En este tutorial se han descrito dos enfoques para la defensa de las aplicaciones de ASP.NET MVC contra ataques por inyección de JavaScript: puede codificar en HTML los datos enviados por el usuario en la vista o puede codificar en HTML los datos enviados por el usuario en el controlador.

> [!div class="step-by-step"]
> [Anterior](authenticating-users-with-windows-authentication-cs.md)
> [Siguiente](authenticating-users-with-forms-authentication-vb.md)
