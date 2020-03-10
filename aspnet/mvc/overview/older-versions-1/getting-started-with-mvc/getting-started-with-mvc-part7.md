---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Agregar validación al modelo | Microsoft Docs
author: shanselman
description: Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Cree una aplicación web simple que lea y escriba en una base de datos.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 9403be574324c34edf93bef1e0e4fd7ba68a3a9d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437281"
---
# <a name="adding-validation-to-the-model"></a>Agregar la validación al modelo

por [Scott Hanselman](https://github.com/shanselman)

> Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Creará una aplicación web simple que lee y escribe en una base de datos. Visite el [centro de aprendizaje de MVC de ASP.net](../../../index.md) para buscar otros tutoriales y ejemplos de ASP.NET MVC.

En esta sección vamos a implementar la compatibilidad necesaria para habilitar la validación de entrada dentro de nuestra aplicación. Nos aseguraremos de que el contenido de la base de datos siempre sea correcto y proporcione mensajes de error útiles a los usuarios finales cuando intenten escribir datos de películas que no sean válidos. Comenzaremos agregando una pequeña lógica de validación a la clase Movie.

Haga clic con el botón derecho en la carpeta modelo y seleccione Agregar clase. Asigne un nombre a la clase Movie.

Cuando creamos el modelo de entidad de la película anterior, el IDE crea una clase de película. De hecho, parte de la clase Movie puede estar en un archivo y parte en otro. Esto se denomina clase parcial. Vamos a ampliar la clase Movie desde otro archivo.

Vamos a crear una clase de película parcial que apunta a una "clase relacionada" con algunos atributos que proporcionarán sugerencias de validación para el sistema. Marcaremos el título y el precio según sea necesario y también insistiremos en que el precio está dentro de un intervalo determinado. Haga clic con el botón derecho en la carpeta modelos y seleccione Agregar clase. Asigne a la clase el nombre Movie y haga clic en el botón Aceptar. Este es el aspecto de nuestra clase de película parcial.

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

Vuelva a ejecutar la aplicación e intente escribir una película con un precio superior a 100. Recibirá un error después de enviar el formulario. El error se detecta en el lado del servidor y se produce después de enviar el formulario. Observe cómo las aplicaciones auxiliares HTML integradas de ASP.NET MVC eran lo suficientemente inteligentes para mostrar el mensaje de error y mantener los valores para nosotros en los elementos de cuadro de texto:

[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)

Esto funciona bien, pero estaría bien si se pudiera indicar al usuario en el lado cliente, inmediatamente, antes de que el servidor se convenga.

Vamos a habilitar la validación del lado cliente con JavaScript.

## <a name="adding-client-side-validation"></a>Agregar la validación del lado cliente

Puesto que nuestra clase de película ya tiene algunos atributos de validación, solo necesitaremos agregar algunos archivos de JavaScript a nuestra plantilla de vista Create. aspx y agregar una línea de código para que se lleve a cabo la validación del lado cliente.

En vWD existente, vaya a nuestra carpeta views/Movie y abra Create. aspx.

Abra la carpeta scripts en el Explorador de soluciones y arrastre los tres scripts siguientes a dentro de la etiqueta de &lt;Head&gt;.

- MicrosoftAjax.js
- MicrosoftMvcValidation.js

Desea que estos archivos de script aparezcan en este orden.

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

Además, agregue esta línea única encima de HTML. BeginForm:

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

Este es el código que se muestra en el IDE.

[Películas de ![: Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)

Ejecute la aplicación y visite/Movies/Create de nuevo y haga clic en crear sin escribir ningún dato. Los mensajes de error aparecen inmediatamente sin el Flash de la página que se asocia con el envío de datos de vuelta al servidor. Esto se debe a que ASP.NET MVC está validando ahora la entrada en el cliente (mediante JavaScript) y en el servidor.

[![crear: Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)

¡ Esto es bueno! Ahora vamos a agregar una columna adicional a la base de datos.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part6.md)
> [Siguiente](getting-started-with-mvc-part8.md)
