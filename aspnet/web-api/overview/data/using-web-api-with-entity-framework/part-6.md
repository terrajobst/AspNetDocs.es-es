---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Crear el cliente de JavaScript | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: 74f2cc4e5e401d690042b05b028dfc0c46ae282a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504727"
---
# <a name="create-the-javascript-client"></a>Crear el cliente de JavaScript

por [Mike Wasson](https://github.com/MikeWasson)

[Descargar proyecto completado](https://github.com/MikeWasson/BookService)

En esta sección, creará el cliente para la aplicación mediante HTML, JavaScript y la biblioteca [Knockout. js](http://knockoutjs.com/) . Compilaremos la aplicación cliente en etapas:

- Mostrar una lista de libros.
- Mostrando un detalle del libro.
- Agregar un nuevo libro.

La biblioteca de cobertura usa el patrón Model-View-ViewModel (MVVM):

- El **modelo** es la representación del lado servidor de los datos en el dominio empresarial (en nuestro caso, libros y autores).
- La **vista** es el nivel de presentación (html).
- El **modelo de vista** es un objeto de JavaScript que contiene los modelos. El modelo de vista es una abstracción de código de la interfaz de usuario. No tiene ningún conocimiento de la representación en HTML. En su lugar, representa características abstractas de la vista, como &quot;una lista de libros&quot;.

La vista está enlazada a datos con el modelo de vista. Las actualizaciones del modelo de vista se reflejan automáticamente en la vista. El modelo de vista también obtiene los eventos de la vista, como los clics de botón.

![](part-6/_static/image1.png)

Este enfoque facilita el cambio del diseño y la interfaz de usuario de la aplicación, ya que puede cambiar los enlaces sin necesidad de volver a escribir ningún código. Por ejemplo, puede mostrar una lista de elementos como `<ul>`y después cambiarlo más adelante a una tabla.

## <a name="add-the-knockout-library"></a>Agregar la biblioteca de cobertura

En Visual Studio, en el menú **herramientas** , seleccione **Administrador de paquetes NuGet**. Seleccione **Consola del Administrador de paquetes**. En la ventana Package Manager Console, escriba el siguiente comando:

[!code-console[Main](part-6/samples/sample1.cmd)]

Este comando agrega los archivos de cobertura a la carpeta scripts.

## <a name="create-the-view-model"></a>Crear el modelo de vista

Agregue un archivo de JavaScript denominado app. js a la carpeta scripts. (En Explorador de soluciones, haga clic con el botón derecho en la carpeta scripts, seleccione **Agregar**y, a continuación, **archivo JavaScript**). Pegue el código siguiente:

[!code-javascript[Main](part-6/samples/sample2.js)]

En la cobertura, la clase `observable` habilita el enlace de datos. Cuando el contenido de un objeto observable cambia, el objeto observable notifica a todos los controles enlazados a datos para que se puedan actualizar por sí mismos. (La clase `observableArray` es la versión de la matriz de *observable*). Para empezar, nuestro modelo de vista tiene dos observables:

- `books` contiene la lista de libros.
- `error` contiene un mensaje de error si se produce un error en una llamada AJAX.

El método `getAllBooks` realiza una llamada AJAX para obtener la lista de libros. Después, envía el resultado a la matriz de `books`.

El método `ko.applyBindings` forma parte de la biblioteca de cobertura. Toma el modelo de vista como parámetro y configura el enlace de datos.

## <a name="add-a-script-bundle"></a>Agregar un paquete de scripts

La Unión es una característica de ASP.NET 4,5 que facilita la combinación o agrupación de varios archivos en un único archivo. La agrupación reduce el número de solicitudes al servidor, lo que puede mejorar el tiempo de carga de la página.

Abra la aplicación de archivo\_Start/BundleConfig. cs. Agregue el código siguiente al método RegisterBundles.

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> [Anterior](part-5.md)
> [Siguiente](part-7.md)
