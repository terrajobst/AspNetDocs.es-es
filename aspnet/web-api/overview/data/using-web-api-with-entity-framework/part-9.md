---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Agregar un nuevo elemento a la base de datos | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 692269a2c11e529af78f24feca74bba704b5b54b
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59415159"
---
# <a name="add-a-new-item-to-the-database"></a>Agregar un nuevo elemento a la base de datos

por [Mike Wasson](https://github.com/MikeWasson)

[Descargue el proyecto completado](https://github.com/MikeWasson/BookService)

En esta sección, agregará la capacidad de los usuarios crear un nuevo libro. En app.js, agregue el siguiente código al modelo de vista:

[!code-javascript[Main](part-9/samples/sample1.js)]

En Index.cshtml, reemplace el marcado siguiente:

[!code-html[Main](part-9/samples/sample2.html)]

Por:

[!code-html[Main](part-9/samples/sample3.html)]

Este marcado crea un formulario para enviar a un nuevo autor. Los valores de la lista desplegable de autor son datos enlazados a la `authors` perceptible en el modelo de vista. Para las entradas de formulario, los valores están enlazados a datos a la `newBook` propiedad del modelo de vista.

El controlador de envío del formulario se enlaza a la `addBook` función:

[!code-html[Main](part-9/samples/sample4.html)]

El `addBook` función lee los valores actuales de las entradas de formulario enlazado a datos para crear un objeto JSON. A continuación, envía el objeto JSON a `/api/books`.

> [!div class="step-by-step"]
> [Anterior](part-8.md)
> [Siguiente](part-10.md)
