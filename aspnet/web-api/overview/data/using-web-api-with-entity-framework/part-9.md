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
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448975"
---
# <a name="add-a-new-item-to-the-database"></a>Agregar un nuevo elemento a la base de datos

por [Mike Wasson](https://github.com/MikeWasson)

[Descargar proyecto completado](https://github.com/MikeWasson/BookService)

En esta sección, agregará la capacidad de los usuarios para crear un nuevo libro. En App. js, agregue el código siguiente al modelo de vista:

[!code-javascript[Main](part-9/samples/sample1.js)]

En index. cshtml, reemplace el marcado siguiente:

[!code-html[Main](part-9/samples/sample2.html)]

Por:

[!code-html[Main](part-9/samples/sample3.html)]

Este marcado crea un formulario para enviar un nuevo autor. Los valores de la lista desplegable autor están enlazados a datos con el `authors` observable en el modelo de vista. Para las demás entradas de formulario, los valores están enlazados a datos con la propiedad `newBook` del modelo de vista.

El controlador de envío del formulario se enlaza a la función `addBook`:

[!code-html[Main](part-9/samples/sample4.html)]

La función `addBook` Lee los valores actuales de las entradas de formularios enlazados a datos para crear un objeto JSON. Después, envía el objeto JSON a `/api/books`.

> [!div class="step-by-step"]
> [Anterior](part-8.md)
> [Siguiente](part-10.md)
