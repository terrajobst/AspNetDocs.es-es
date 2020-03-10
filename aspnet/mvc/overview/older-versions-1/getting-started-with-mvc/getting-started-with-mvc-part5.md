---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Acceso a los datos del modelo desde un controlador | Microsoft Docs
author: shanselman
description: Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Cree una aplicación web simple que lea y escriba en una base de datos.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 207ed880977d794d81efdc1ea458d17a68d501d8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437371"
---
# <a name="accessing-your-models-data-from-a-controller"></a>Obtener acceso a los datos del modelo desde un controlador

por [Scott Hanselman](https://github.com/shanselman)

> Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Creará una aplicación web simple que lee y escribe en una base de datos. Visite el [centro de aprendizaje de MVC de ASP.net](../../../index.md) para buscar otros tutoriales y ejemplos de ASP.NET MVC.

En esta sección vamos a crear una nueva clase MoviesController y escribiremos código que recupere los datos de la película y los mostrará de nuevo en el explorador con una plantilla de vista.

Haga clic con el botón derecho en la carpeta Controllers y cree un nuevo MoviesController.

[![agregar controlador](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)

Se creará un nuevo archivo "MoviesController.cs" debajo de nuestra carpeta \Controllers dentro de nuestro proyecto. Vamos a actualizar MovieController para recuperar la lista de películas de nuestra base de datos recién rellenada.

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

Vamos a realizar una consulta LINQ para que solo se recuperen las películas publicadas después del verano de 1984. Necesitaremos una plantilla de vista para volver a presentar esta lista de películas, así que haga clic con el botón derecho en el método y seleccione Agregar vista para crearla.

En el cuadro de diálogo Agregar vista, se indicará que se está pasando una lista&lt;películas. Models. Movie&gt; a nuestra plantilla de vista. A diferencia de las horas anteriores usamos el cuadro de diálogo Agregar vista y eligió crear una plantilla "vacía", esta vez indicaremos que queremos que Visual Studio "scaffolding" automáticamente sea una plantilla de vista para nosotros con algún contenido predeterminado. Haremos esto seleccionando el elemento "list" en el menú desplegable de vista de contenido.

Recuerde que si ha creado una nueva clase, deberá compilar la aplicación para que se muestre en el cuadro de diálogo Agregar vista.

![Agregar vista](getting-started-with-mvc-part5/_static/image3.png)

Haga clic en agregar y el sistema generará automáticamente el código de una vista que muestra nuestra lista de películas. Es un buen momento para cambiar el encabezado &lt;H2&gt; a algo parecido a "mi lista de películas", como hicimos anteriormente con la vista Hola mundo.

[Películas de ![: Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)

Ejecute la aplicación y visite/Movies en la barra de direcciones. Ahora se han recuperado los datos de la base de datos mediante una consulta básica en el controlador y se han devuelto los datos a una vista que conoce las películas. A continuación, esta vista gira a través de la lista de películas y crea una tabla de datos para nosotros.

[![lista de películas: Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)

No vamos a implementar la funcionalidad de edición, detalles y eliminación con esta aplicación, por lo que no necesitamos los vínculos predeterminados creados por nosotros. Abra el archivo/Movies/Index.aspx y quítelo.

Este es el código fuente del aspecto que debería tener nuestra plantilla de vista actualizada una vez que realizamos estos cambios:

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

Está creando vínculos que no necesitamos, por lo que los eliminaremos en este ejemplo. Sin embargo, conservaremos el vínculo crear nuevo, tal y como eso es próximo. Este es el aspecto de la aplicación con esa columna quitada.

[![lista de películas: Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)

Ahora tenemos una lista simple de los datos de la película. Sin embargo, si hacemos clic en el vínculo "crear nuevo", se producirá un error porque no está enlazado. Vamos a implementar un método de acción Create y permitir que un usuario escriba nuevas películas en nuestra base de datos.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part4.md)
> [Siguiente](getting-started-with-mvc-part6.md)
