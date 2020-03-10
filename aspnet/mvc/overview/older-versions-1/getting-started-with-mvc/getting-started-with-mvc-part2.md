---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: Agregando un controlador | Microsoft Docs
author: shanselman
description: Una versión actualizada si este tutorial está disponible aquí mediante Visual Studio 2013. En el nuevo tutorial se usa ASP.NET MVC 5, que proporciona muchas mejoras sobre t...
ms.author: riande
ms.date: 08/14/2010
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: e2a298584473f57c2b14edf507f0f6886d906ea3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437569"
---
# <a name="adding-a-controller"></a>Agregar un controlador

por [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Una versión actualizada si este tutorial está disponible [aquí](../../getting-started/introduction/getting-started.md) mediante [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013). En el nuevo tutorial se usa ASP.NET MVC 5, que proporciona muchas mejoras en este tutorial.
>
>
> Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Creará una aplicación web simple que lee y escribe en una base de datos. Visite el [centro de aprendizaje de MVC de ASP.net](../../../index.md) para buscar otros tutoriales y ejemplos de ASP.NET MVC.

MVC representa el modelo, la vista y el controlador. MVC es un patrón para desarrollar aplicaciones, de modo que cada una de ellas tiene una responsabilidad diferente de otra.

- Modelo: los datos de la aplicación
- Vistas: los archivos de plantilla que usará la aplicación para generar dinámicamente respuestas HTML.
- Controladores: clases que controlan las solicitudes de dirección URL de entrada a la aplicación, recuperan los datos del modelo y, a continuación, especifican plantillas de vista que representan una respuesta en el cliente.

Trataremos todos estos conceptos en este tutorial y le mostraremos cómo usarlos para compilar una aplicación.

Vamos a crear un controlador nuevo; para ello, haga clic con el botón secundario en la carpeta Controllers en el explorador de soluciones y seleccione Agregar controlador.

[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)

Asigne al nuevo controlador el nombre "HelloWorldController" y haga clic en Agregar.

[![cuadro de diálogo Agregar controlador](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)

Tenga en cuenta que en la Explorador de soluciones de la derecha se ha creado un nuevo archivo llamado HelloWorldController.cs y que el archivo ya está abierto en el **IDE**.

[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)

Cree dos nuevos métodos que se parezcan a este dentro de la nueva clase pública HelloWorldController. Se devolverá una cadena de HTML directamente desde nuestro controlador como ejemplo.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

El controlador se denomina HelloWorldController y el nuevo método se denomina index. Vuelva a ejecutar la aplicación, igual que antes (haga clic en el botón reproducir o presione F5 para hacerlo). Una vez que se haya iniciado el explorador, cambie la ruta de acceso en la barra de direcciones a `http://localhost:xx/HelloWorld` donde XX es el número elegido por el equipo. Ahora el explorador debe ser similar a la captura de pantalla siguiente. En nuestro método anterior, se devolvió una cadena pasada en un método denominado "Content". Le indicamos que el sistema devolverá algo de HTML y lo hizo.

ASP.NET MVC invoca diferentes clases de controlador (y métodos de acción diferentes) en función de la dirección URL de entrada. La lógica de asignación predeterminada usada por ASP.NET MVC usa un formato como este para controlar qué código se ejecuta:

/[Controller]/[ActionName]/[parámetros]

La primera parte de la dirección URL determina la clase de controlador que se va a ejecutar. Por lo tanto,/HelloWorld se asigna a la clase HelloWorldController. La segunda parte de la dirección URL determina el método de acción de la clase que se va a ejecutar. Por lo tanto,/HelloWorld/Index haría que se ejecutara el método index () de la clase HelloWorldController. Tenga en cuenta que solo tuvimos que visitar/HelloWorld anteriormente y el índice de método era implícito. Esto se debe a que un método denominado "index" es el método predeterminado al que se llamará en un controlador si no se especifica uno explícitamente.

[![se trata de la acción predeterminada](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)

Ahora, vamos a visitar `http://localhost:xx/HelloWorld/Welcome.` ahora nuestro método de bienvenida se ha ejecutado y ha devuelto su cadena HTML.

De nuevo,/[Controller]/[ActionName]/[Parameters] el controlador es HelloWorld y Welcome es el método en este caso. Todavía no hemos hecho los parámetros.

[![este es el método de acción de bienvenida](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)

Vamos a modificar nuestro ejemplo ligeramente para que podamos pasar parte de la información de la dirección URL a nuestro controlador, como en este ejemplo:/HelloWorld/Welcome? Name = Scott&amp;numtimes = 4. Cambie el método de bienvenida para que incluya dos parámetros y actualícelo como se indica a continuación. Tenga en cuenta que hemos usado C# la característica opcional Parameter para indicar que el parámetro numTimes debe tener como valor predeterminado 1 si no se pasa.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

Ejecute la aplicación y visite `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` cambiar el valor de Name y numtimes como desee. El sistema asigna automáticamente los parámetros con nombre de la cadena de consulta en la barra de direcciones a los parámetros del método.

En ambos ejemplos, el controlador ha estado realizando todo el trabajo y ha devuelto el HTML directamente. Normalmente, no queremos que nuestros controladores devuelvan HTML directamente, ya que acaban siendo muy engorrosos para el código. En su lugar, usaremos normalmente un archivo de plantilla de vista independiente para ayudar a generar la respuesta HTML. Veamos cómo se puede hacer esto. Cierre el explorador y vuelva al IDE.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part1.md)
> [Siguiente](getting-started-with-mvc-part3.md)
