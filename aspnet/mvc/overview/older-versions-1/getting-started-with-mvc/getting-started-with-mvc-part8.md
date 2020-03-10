---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Agregar una columna al modelo | Microsoft Docs
author: shanselman
description: Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Cree una aplicación web simple que lea y escriba en una base de datos.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 1cf092c3db3959d6f47006f1be2ba82833c5dc06
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437227"
---
# <a name="adding-a-column-to-the-model"></a>Agregar una columna al modelo

por [Scott Hanselman](https://github.com/shanselman)

> Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Creará una aplicación web simple que lee y escribe en una base de datos. Visite el [centro de aprendizaje de MVC de ASP.net](../../../index.md) para buscar otros tutoriales y ejemplos de ASP.NET MVC.

En esta sección, vamos a examinar cómo podemos realizar cambios en el esquema de nuestra base de datos y controlar los cambios en nuestra aplicación.

Vamos a agregar una columna "rating" a la tabla Movie. Vuelva al IDE y haga clic en el Explorador de bases de datos. Haga clic con el botón derecho en la tabla de películas y seleccione Abrir definición de tabla.

Agregue una columna "rating" como se muestra a continuación. Dado que ahora no tenemos ninguna clasificación, la columna puede permitir valores NULL. Haga clic en Guardar.

[![tabla de películas de edición](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)

A continuación, vuelva al Explorador de soluciones y abra el archivo movies. edmx (que se encuentra en la carpeta \Models). Haga clic con el botón derecho en la superficie de diseño (el área en blanco) y seleccione Actualizar modelo desde base de datos.

[Películas de ![: Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)

Se iniciará el "Asistente para actualización". Haga clic en la pestaña actualizar dentro de ella y haga clic en finalizar. A continuación, nuestra clase de modelo de película se actualizará con la nueva columna.

![Asistente para actualización (2)](getting-started-with-mvc-part8/_static/image5.png)

Después de hacer clic en finalizar, puede ver que se ha agregado la nueva columna de clasificación a la entidad de película en nuestro modelo.

[![entidad de película](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)

Hemos agregado una columna en el modelo de base de datos, pero las vistas no lo saben.

## <a name="update-views-with-model-changes"></a>Actualizar vistas con cambios de modelo

Hay varias maneras de actualizar las plantillas de vista para que reflejen la nueva columna de clasificación. Dado que creamos estas vistas mediante su generación mediante el cuadro de diálogo Agregar vista, podríamos eliminarlas y volver a crearlas. Sin embargo, normalmente los usuarios ya habrán realizado modificaciones en sus plantillas de vista a partir de la generación con scaffolding inicial y querrán agregar o eliminar campos manualmente, tal y como hicimos con el campo de ID. para crear.

Abra la plantilla \Views\Movies\Index.aspx y agregue un &lt;&gt;clasificación&lt;/TH&gt; al encabezado de la tabla Movie. Agregué el mío después del género. A continuación, en la misma posición de la columna pero abajo, agregue una línea para generar la nueva clasificación.

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

Nuestra plantilla index. aspx final tendrá el siguiente aspecto:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

A continuación, vamos a abrir la plantilla \Views\Movies\Create.aspx y agregaremos una etiqueta y un cuadro de texto para la nueva propiedad Rating:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

Nuestra plantilla final Create. aspx tendrá el siguiente aspecto y cambiaremos el título del explorador y el &lt;de la versión H2 del&gt; del título a algo parecido a "crear una película" mientras estamos aquí.

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

Ejecute la aplicación y ahora tiene un nuevo campo en la base de datos que se ha agregado a la página crear. Agregue una nueva película (esta vez con una clasificación) y haga clic en crear.

[![crear una película (Windows Internet Explorer)](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)

Después de hacer clic en crear, se le enviará a la página de índice donde se muestra la nueva película con la nueva columna clasificación en la base de datos.

[![lista de películas: Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)

En este tutorial básico se empezó a crear controladores y a asociarlos con vistas y pasar los datos codificados de forma rígida. Después creamos y diseñamos una base de datos y colocamos algunos datos en ella. Recuperamos los datos de la base de datos y los mostramos en una tabla HTML. A continuación, se ha agregado un formulario de creación que permite al usuario agregar datos a la base de datos desde dentro de la aplicación Web. Hemos agregado la validación y, a continuación, hemos realizado la validación para usar JavaScript en el lado cliente. Por último, hemos cambiado la base de datos para incluir una nueva columna de datos y, a continuación, hemos actualizado nuestras dos páginas para crear y mostrar estos datos nuevos.

Ahora le animamos a pasar a nuestro tutorial de nivel intermedio "[almacén de música de MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)", así como a muchos vídeos y recursos de [https://asp.net/mvc](https://asp.net/mvc) para obtener más información sobre ASP.NET MVC!

Disfrútelo.

- Scott Hanselman- [http://hanselman.com](http://hanselman.com) y [@shanselman](http://twitter.com/shanselman) en Twitter.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part7.md)
