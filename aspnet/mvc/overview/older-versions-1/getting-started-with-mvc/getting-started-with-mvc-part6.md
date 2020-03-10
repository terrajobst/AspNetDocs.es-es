---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: Agregar un método Create y Create View | Microsoft Docs
author: shanselman
description: Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Cree una aplicación web simple que lea y escriba en una base de datos.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 05a281720f76b107fe8d902ef60d5d2e72af3ef5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437293"
---
# <a name="adding-a-create-method-and-create-view"></a>Agregar un método Create y una vista Create

por [Scott Hanselman](https://github.com/shanselman)

> Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Creará una aplicación web simple que lee y escribe en una base de datos. Visite el [centro de aprendizaje de MVC de ASP.net](../../../index.md) para buscar otros tutoriales y ejemplos de ASP.NET MVC.

En esta sección vamos a implementar la compatibilidad necesaria para permitir que los usuarios creen nuevas películas en nuestra base de datos. Haremos esto implementando la acción de dirección URL de/Movies/Create.

La implementación de la dirección URL de/Movies/Create es un proceso de dos pasos. La primera vez que un usuario visita la dirección URL de/Movies/Create desea mostrarles un formulario HTML que pueden rellenar para escribir una nueva película. A continuación, cuando el usuario envía el formulario y envía los datos de vuelta al servidor, queremos recuperar el contenido publicado y guardarlo en nuestra base de datos.

Implementaremos estos dos pasos en dos métodos Create () dentro de la clase MoviesController. Un método mostrará el &lt;formulario&gt; que el usuario debe rellenar para crear una nueva película. El segundo método controlará el procesamiento de los datos enviados cuando el usuario envíe el &lt;formulario&gt; de vuelta al servidor y guardará una nueva película en la base de datos.

A continuación se muestra el código que agregaremos a nuestra clase MoviesController para implementar esto:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

El código anterior contiene todo el código que se necesitará en nuestro controlador.

Ahora vamos a implementar la plantilla de creación de vistas que vamos a usar para mostrar un formulario al usuario. Haremos clic con el botón derecho en el primer método de creación y seleccione "agregar vista" para crear la plantilla de vista para el formulario de película.

Seleccionaremos que vamos a pasar la plantilla de vista a "Movie" como su clase de datos de vista e indicar que queremos "scaffolding" a una plantilla "Create".

[![agregar vista](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)

Después de hacer clic en el botón Agregar, se creará una plantilla de vista de \Movies\Create.aspx. Dado que hemos seleccionado "crear" en la lista desplegable "ver contenido", el cuadro de diálogo Agregar vista automáticamente "scaffolding" es un contenido predeterminado para nosotros. La técnica de scaffolding crea un formulario de &lt;HTML&gt;, un lugar para los mensajes de error de validación que se van a pasar y, como la técnica scaffolding conoce las películas, creaba etiquetas y campos para cada propiedad de nuestra clase.

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

Dado que nuestra base de datos proporciona automáticamente un identificador a una película, vamos a quitar los campos que hacen referencia al modelo. Identificador de nuestra vista de creación. Quite las 7 líneas después de &lt;los campos&gt;de leyenda&lt;/Legend&gt;, ya que muestran el campo de identificador que no queremos.

Ahora vamos a crear una nueva película y a agregarla a la base de datos. Para ello, ejecute la aplicación de nuevo y visite la dirección URL "/Movies" y haga clic en el vínculo "crear" para agregar una nueva película.

[![crear: Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)

Cuando hacemos clic en el botón crear, volveremos a publicar (a través de HTTP POST) los datos de este formulario en el método/Movies/Create que acabamos de crear. Del mismo modo que cuando el sistema tomó automáticamente el parámetro "numTimes" y "Name" fuera de la dirección URL y los asignó a parámetros en un método anterior, el sistema tomará automáticamente los campos de formulario de una publicación y los asignará a un objeto. En este caso, los valores de los campos en HTML como "ReleaseDate" y "title" se colocarán automáticamente en las propiedades correctas de una nueva instancia de una película.

Echemos un vistazo al segundo método Create de nuestro MoviesController de nuevo. Observe cómo toma un objeto "Movie" como argumento:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

Este objeto de película se pasó a la versión [HttpPost] de nuestro método Create Action y lo hemos guardado en la base de datos y, a continuación, redirigió al usuario de nuevo al método de acción index (), que mostrará el resultado guardado en la lista de películas:

[![lista de películas: Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)

No estamos comprobando si nuestras películas son correctas, pero la base de datos no nos permitirá guardar una película sin ningún título. Estaría bien si se pudiera indicar al usuario que antes de que se produjeron errores en la base de datos. Haremos esto después agregando compatibilidad con la validación a nuestra aplicación.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part5.md)
> [Siguiente](getting-started-with-mvc-part7.md)
