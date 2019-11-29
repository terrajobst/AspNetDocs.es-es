---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Parte 5: creación de una interfaz de usuario dinámica con Knockout. js | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: bbdeba756de7986cfeb92aa10a57f4f101382f99
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599993"
---
# <a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a>Parte 5: creación de una interfaz de usuario dinámica con Knockout. js

por [Mike Wasson](https://github.com/MikeWasson)

[Descargar proyecto completado](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a>Crear una IU dinámica con Knockout.js

En esta sección, usaremos Knockout. js para agregar funcionalidad a la vista de administración.

[Knockout. js](http://knockoutjs.com/) es una biblioteca de JavaScript que facilita el enlace de controles HTML a los datos. Knockout. js usa el patrón Model-View-ViewModel (MVVM).

- El *modelo* es la representación del lado servidor de los datos en el dominio empresarial (en nuestro caso, productos y pedidos).
- La *vista* es el nivel de presentación (html).
- El *modelo de vista* es un objeto de JavaScript que contiene los datos del modelo. El modelo de vista es una abstracción de código de la interfaz de usuario. No tiene ningún conocimiento de la representación en HTML. En su lugar, representa características abstractas de la vista, como "una lista de elementos".

La vista está enlazada a datos con el modelo de vista. Las actualizaciones del modelo de vista se reflejan automáticamente en la vista. El modelo de vista también obtiene los eventos de la vista, como los clics de botón, y realiza operaciones en el modelo, como la creación de un pedido.

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

En primer lugar, vamos a definir el modelo de vista. Después, enlazaremos el marcado HTML al modelo de vista.

Agregue la siguiente sección de Razor a admin. cshtml:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

Puede Agregar esta sección en cualquier parte del archivo. Cuando se representa la vista, la sección aparece en la parte inferior de la página HTML, justo antes de la etiqueta de cierre &lt;/Body&gt;.

Todo el script de esta página irá dentro de la etiqueta de script indicada por el comentario:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

En primer lugar, defina una clase de modelo de vista:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

**Ko. observableArray** es un tipo especial de objeto en Knockout, denominado *observable*. En la [documentación de Knockout. js](http://knockoutjs.com/documentation/observables.html): un objeto observable es un "objeto de JavaScript que puede notificar los cambios a los suscriptores". Cuando el contenido de un objeto observable cambia, la vista se actualiza automáticamente para que coincida.

Para rellenar la matriz de `products`, realice una solicitud AJAX a la API Web. Recuerde que hemos almacenado el URI base para la API en el contenedor de vistas (consulte la [parte 4](using-web-api-with-entity-framework-part-4.md) del tutorial).

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

A continuación, agregue funciones al modelo de vista para crear, actualizar y eliminar productos. Estas funciones envían llamadas AJAX a la API Web y usan los resultados para actualizar el modelo de vista.

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

Ahora la parte más importante: cuando el DOM está lleno, llame a la función **Ko. applyBindings** y pase una nueva instancia de la `ProductsViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

El método **Ko. applyBindings** activa la cobertura y conecta el modelo de vista a la vista.

Ahora que tenemos un modelo de vista, podemos crear los enlaces. En Knockout. js, esto se hace agregando `data-bind` atributos a los elementos HTML. Por ejemplo, para enlazar una lista HTML a una matriz, use el enlace de `foreach`:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

El enlace de `foreach` recorre en iteración la matriz y crea elementos secundarios para cada objeto de la matriz. Los enlaces de los elementos secundarios pueden hacer referencia a las propiedades de los objetos de matriz.

Agregue los siguientes enlaces a la lista "Update-Products":

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

El elemento `<li>` aparece dentro del ámbito del enlace **foreach** . Esto significa que la cobertura representará el elemento una vez por cada producto de la matriz de `products`. Todos los enlaces dentro del elemento `<li>` hacen referencia a esa instancia del producto. Por ejemplo, `$data.Name` hace referencia a la propiedad `Name` del producto.

Para establecer los valores de las entradas de texto, utilice el enlace de `value`. Los botones se enlazan a las funciones de la vista modelo, mediante el enlace de `click`. La instancia del producto se pasa como un parámetro a cada función. Para obtener más información, la [documentación de Knockout. js](http://knockoutjs.com/documentation/observables.html) contiene buenas descripciones de los distintos enlaces.

A continuación, agregue un enlace para el evento de **envío** en el formulario agregar producto:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

Este enlace llama a la función `create` en el modelo de vista para crear un nuevo producto.

Este es el código completo de la vista de administración:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

Ejecute la aplicación, inicie sesión con la cuenta de administrador y haga clic en el vínculo "admin". Debería ver la lista de productos y poder crear, actualizar o eliminar productos.

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-4.md)
> [Siguiente](using-web-api-with-entity-framework-part-6.md)
