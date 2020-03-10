---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
title: ¿Cómo usar el control de editor HTML? (C#) | Microsoft Docs
author: microsoft
description: HTMLEditor es un control de ASP.NET AJAX que le permite crear y editar fácilmente contenido HTML a través de los botones de una barra de herramientas.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: f47e6224-c2e5-4472-b069-b6c7b6115200
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
msc.type: authoredcontent
ms.openlocfilehash: cb7d75b59b1361abeb6d3c38ad6e42e34d6e3f7b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78466609"
---
# <a name="how-do-i-use-the-html-editor-control-c"></a>¿Cómo usar el control de editor HTML? (C#)

por [Microsoft](https://github.com/microsoft)

> HTMLEditor es un control de ASP.NET AJAX que le permite crear y editar fácilmente contenido HTML a través de los botones de una barra de herramientas.

El objetivo de este tutorial es proporcionarle información general sobre el control de editor HTML incluido en el kit de herramientas de control de AJAX. El editor HTML incluye opciones para cambiar el tamaño de fuente, seleccionar una fuente, cambiar el color de fondo, modificar el color de primer plano, agregar vínculos, agregar imágenes, cambiar la alineación del texto y realizar operaciones de cortar, copiar y pegar (vea la ilustración 1).

[![el editor HTML](how-do-i-use-the-html-editor-control-cs/_static/image1.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image1.png)

**Figura 01**: editor HTML ([haga clic para ver la imagen de tamaño completo](how-do-i-use-the-html-editor-control-cs/_static/image2.png))

El editor HTML le permite escribir contenido mediante un modo de diseño o puede escribir HTML directamente. También se ofrece la opción de obtener una vista previa del contenido HTML (vea la ilustración 2).

[botones de ![diseño, HTML y vista previa](how-do-i-use-the-html-editor-control-cs/_static/image2.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image3.png)

**Figura 02**: botones de diseño, HTML y vista previa ([haga clic para ver la imagen de tamaño completo](how-do-i-use-the-html-editor-control-cs/_static/image4.png))

En este tutorial, obtendrá información sobre cómo mostrar el editor HTML, cómo personalizar los botones de la barra de herramientas que aparecen en el editor HTML y cómo evitar ataques de scripts entre sitios.

## <a name="displaying-the-html-editor"></a>Mostrar el editor HTML

Antes de poder usar el editor HTML en una página de ASP.NET, primero debe agregar un control ScriptManager a la página. El control ScriptManager se encuentra debajo de la pestaña extensiones AJAX en el cuadro de herramientas de Visual Studio/Visual Web Developer Express.

Debe colocar el control ScriptManager en la parte superior de la página antes que cualquier otro control de la página. Por ejemplo, puede colocarlo inmediatamente debajo del formulario de &lt;de apertura del lado servidor&gt; etiqueta.

El control de editor HTML se encuentra en el cuadro de herramientas con el resto de los controles de AJAX control Toolkit. Se denomina control de editor (consulte la figura 3).

[![el control de editor HTML](how-do-i-use-the-html-editor-control-cs/_static/image3.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image5.png)

**Figura 03**: control de editor HTML ([haga clic para ver la imagen de tamaño completo](how-do-i-use-the-html-editor-control-cs/_static/image6.png))

Después de arrastrar el editor HTML a una página, puede establecer sus propiedades en la hoja de propiedades. Por ejemplo, normalmente desea establecer las propiedades ancho y alto. La lista 1 contiene el origen de una página ASP.NET que contiene un editor HTML.

**Lista 1-SimpleEditor. aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample1.aspx)]

La página de la lista 1 contiene un control de editor HTML, un control de botón y un control literal. Al hacer clic en el botón, el contenido del editor HTML aparece en el control literal (consulte la figura 4).

[![enviar un formulario con un editor HTML](how-do-i-use-the-html-editor-control-cs/_static/image4.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image7.png)

**Figura 04**: envío de un formulario con un editor HTML ([haga clic para ver la imagen de tamaño completo](how-do-i-use-the-html-editor-control-cs/_static/image8.png))

La propiedad Content del editor HTML se usa para recuperar el contenido HTML especificado en el editor HTML. Tenga en cuenta que este contenido HTML puede contener JavaScript. En la siguiente sección, se explica cómo puede evitar ataques por inyección de código JavaScript.

## <a name="customizing-the-html-editor-toolbar"></a>Personalización de la barra de herramientas del editor HTML

Puede personalizar exactamente qué botones aparecen en el editor. Por ejemplo, puede que desee quitar la pestaña HTML para evitar que los usuarios cambien el editor HTML por el modo HTML. O bien, puede que desee quitar la lista desplegable Tamaño de fuente para evitar que los usuarios creen texto demasiado grande en un mensaje de foro post (consulte la figura 5).

[![un editor HTML personalizado](how-do-i-use-the-html-editor-control-cs/_static/image5.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image9.png)

**Figura 05**: editor HTML personalizado ([haga clic para ver la imagen de tamaño completo](how-do-i-use-the-html-editor-control-cs/_static/image10.png))

Para personalizar los botones de la barra de herramientas, puede derivar un nuevo editor HTML de la clase base editor. Por ejemplo, el editor personalizado de la lista 2 solo contiene botones de la barra de herramientas para negrita y cursiva. Se han quitado todos los demás botones de la barra de herramientas. Además, la pestaña HTML se ha quitado de la parte inferior del editor (pero las pestañas diseño y vista previa todavía están allí).

**Lista 2-App\_Code\CustomEditor.cs**

[!code-csharp[Main](how-do-i-use-the-html-editor-control-cs/samples/sample2.cs)]

Debe agregar la clase en la lista 2 a la carpeta de código de la aplicación\_para que la clase se compile automáticamente. Si la carpeta de código del\_de la aplicación no existe en el sitio web, puede agregar simplemente la carpeta.

Después de crear un editor personalizado, puede agregarlo a una página de ASP.NET de la misma manera en que agrega el editor HTML normal (vea la lista 3).

**Lista 3-ShowCustomEditor. aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>Evitar ataques de scripting entre sitios (XSS)

Siempre que acepte la entrada de un usuario y vuelva a mostrar esa entrada en el sitio web, puede abrir el sitio web a ataques de scripting entre sitios (XSS). En teoría, un pirata informático malintencionado podría enviar código JavaScript que se ejecuta cuando se vuelve a mostrar la entrada. El código JavaScript podría usarse para robar contraseñas de usuario u otra información confidencial.

Normalmente, puede derrotar los ataques XSS mediante la codificación HTML, independientemente de la entrada que recupere de un usuario antes de mostrarlo en una página web. Sin embargo, la codificación HTML de la salida del editor HTML no solo codificaría &lt;scripts&gt; etiquetas, sino que también codificaría todas las etiquetas HTML. En otras palabras, perdería todo el formato, como el tipo de fuente, el tamaño de fuente y el color de fondo.

Si va a recopilar información confidencial de los usuarios (por ejemplo, contraseñas, números de tarjetas de crédito y números de la seguridad social), no debe mostrar el contenido no codificado que recupere de un usuario con el editor HTML. Solo debe usar el editor HTML en situaciones en las que no vuelva a mostrar el contenido HTML, o bien que una entidad de confianza envíe el contenido HTML a su sitio Web.

Imagine, por ejemplo, que va a crear una aplicación de blog. En esta situación, tiene sentido usar el editor HTML al redactar entradas de blog. Es el único que envía una entrada de blog y, presumiblemente, puede confiar en que no envíe JavaScript malintencionado. Sin embargo, no tiene sentido usar el editor HTML al permitir que los usuarios anónimos publiquen comentarios. Debe ser especialmente cuidadoso en situaciones en las que los usuarios envían información confidencial, como contraseñas. Potencialmente, un usuario malintencionado podría publicar un comentario que contuviera el código JavaScript correcto para robar una contraseña.

## <a name="summary"></a>Resumen

En este tutorial, se proporcionó una breve introducción al control de editor HTML incluido en AJAX control Toolkit. Ha aprendido a usar el editor HTML para aceptar contenido enriquecido de un usuario y enviar el contenido al servidor. También se describe cómo puede personalizar los botones de la barra de herramientas que se muestran en el editor HTML. Por último, aprendió a evitar ataques de scripts entre sitios al usar el editor HTML para aceptar entradas potencialmente malintencionadas.

> [!div class="step-by-step"]
> [Siguiente](how-do-i-use-the-html-editor-control-vb.md)
