---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: Agregar una vista | Microsoft Docs
author: shanselman
description: Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Cree una aplicación web simple que lea y escriba en una base de datos.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: 462b1210c45da67058899193afcea973f3daf122
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78469777"
---
# <a name="adding-a-view"></a>Agregar una vista

por [Scott Hanselman](https://github.com/shanselman)

> Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Creará una aplicación web simple que lee y escribe en una base de datos. Visite el [centro de aprendizaje de MVC de ASP.net](../../../index.md) para buscar otros tutoriales y ejemplos de ASP.NET MVC.

En esta sección vamos a ver cómo podemos hacer que nuestra clase HelloWorldController use un archivo de plantilla de vista para encapsular limpiamente la generación de respuestas HTML de vuelta a un cliente.

Comencemos con el uso de una plantilla de vista con nuestro método de índice. Nuestro método se denomina index y está en HelloWorldController. Actualmente, nuestro método index () devuelve una cadena con un mensaje que está codificado de forma rígida dentro de la clase Controller.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

Ahora, vamos a cambiar el método de índice para que tenga el siguiente aspecto:

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

Ahora vamos a agregar una plantilla de vista a nuestro proyecto que se puede usar para nuestro método index (). Para ello, haga clic con el botón secundario del mouse en algún lugar del método de índice y haga clic en agregar vista...

![imagen](getting-started-with-mvc-part3/_static/image1.png)

Se abrirá el cuadro de diálogo "agregar vista", que nos proporciona algunas opciones sobre cómo deseamos crear una plantilla de vista que pueda usar nuestro método de índice. Por ahora, no cambie nada y haga clic en el botón Agregar.

[![cuadro de diálogo Agregar vista](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)

Después de hacer clic en agregar, aparecerá una nueva carpeta y un nuevo archivo en la carpeta de la solución, tal como se muestra aquí. Ahora tengo una carpeta HelloWorld en views y un archivo index. aspx dentro de esa carpeta.

[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)

El nuevo archivo de índice también está abierto y listo para su edición. Agregue texto debajo del primer &lt;H2&gt;índice&lt;/H2&gt; como "Hola mundo".

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

Ejecute la aplicación y visite [`http://localhost:xx/HelloWorld`](http://localhostxx) de nuevo en el explorador. El método index de nuestro controlador en este ejemplo no realizaba ningún trabajo, pero hacía llamada a "Return View ()", que indicaba que queríamos usar un archivo de plantilla de vista para volver a presentar una respuesta al cliente. Dado que no especificamos explícitamente el nombre del archivo de plantilla de vista que se va a usar, ASP.NET MVC tenía como valor predeterminado el uso del archivo de vista index. aspx dentro de la carpeta \Views\HelloWorld. Ahora vemos la cadena que hemos codificado de forma rígida en nuestra vista.

[Índice de ![(Windows Internet Explorer)](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)

Parece bastante bueno. Sin embargo, tenga en cuenta que el título del explorador indica "índice" y el título grande de la página indica "mi aplicación MVC". Vamos a cambiarlos.

### <a name="changing-views-and-master-pages"></a>Cambiar vistas y páginas maestras

En primer lugar, vamos a cambiar el texto "My MVC Application". Ese texto se comparte y aparece en todas las páginas. Realmente aparece solo en un lugar de nuestro código, aunque esté en cada página de la aplicación. Vaya a la carpeta/Views/Shared en el Explorador de soluciones y abra el archivo site. Master. Este archivo se denomina página maestra y es el "Shell" compartido que usan todas las otras páginas.

Observe un texto que dice ContentPlaceholder "MainContent" en este archivo.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

Ese marcador de posición es donde se mostrarán todas las páginas que cree, "encapsuladas" en la página maestra. Intente cambiar el título y, a continuación, ejecute la aplicación y visite varias páginas. Observará que el cambio aparece en varias páginas.

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

Ahora todas las páginas tendrán el encabezado principal, que es H1-of "My MVC Movie Application". Que controla el texto en blanco en la parte superior que se comparte entre todas las páginas.

Este es el sitio. Master en su totalidad con nuestro título cambiado:

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

Ahora, vamos a cambiar el título de la página de índice.

Abrir/HelloWorld/Index.aspx. Hay dos lugares para cambiar. En primer lugar, el título que aparece en el título del explorador y, a continuación, el encabezado secundario, que es el H2. Los haré de forma ligeramente diferente para que pueda ver qué fragmento de código cambia la parte de la aplicación.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

Ejecute la aplicación y visite/movies. Observe que el título del explorador, el encabezado principal y los encabezados secundarios han cambiado. Es fácil realizar cambios importantes en la aplicación con pequeños cambios en la vista.

[![lista de películas: Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)

Nuestro pequeño bit de "datos" (en este caso, el "Hola mundo!" Message) no obstante, se ha codificado de forma rígida. Tenemos V (vistas) y tenemos C (controladores), pero todavía no hay M (modelo). En breve veremos cómo crear una base de datos y recuperar los datos del modelo.

## <a name="passing-a-viewmodel"></a>Pasar un ViewModel

Sin embargo, antes de ir a una base de datos y hablar acerca de los modelos, vamos a hablar primero sobre "ViewModels". Se trata de objetos que representan lo que requiere una plantilla de vista para volver a presentar una respuesta HTML a un cliente. Normalmente se crean y pasan por una clase de controlador a una plantilla de vista, y solo deben contener los datos que requiere la plantilla de vista, y no más.

Anteriormente, con nuestro ejemplo HelloWorld, nuestro método de acción Welcome () tomaba un nombre y un parámetro numTimes y lo envía al explorador. En lugar de hacer que el controlador siga representando esta respuesta directamente, vamos a crear una pequeña clase para almacenar los datos y, a continuación, pasarlos a una plantilla de vista para representarla con la respuesta HTML. De este modo, el controlador se preocupa de una cosa y de la plantilla de vista, lo que nos permite mantener una "separación de preocupaciones" limpia dentro de nuestra aplicación.

Vuelva al archivo HelloWorldController.cs y agregue una nueva clase "WelcomeViewModel" y cambie el método de bienvenida dentro del controlador. Este es el HelloWorldController.cs completo con la nueva clase en el mismo archivo.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

Aunque se trata de varias líneas, nuestro método de bienvenida es en realidad solo dos instrucciones de código. La primera instrucción empaqueta nuestros dos parámetros en un objeto ViewModel y el segundo pasa el objeto resultante a la vista.

Ahora necesitamos una plantilla de vista de bienvenida. Haga clic con el botón derecho en el método de bienvenida y seleccione Agregar vista. Esta vez, haremos clic en "crear una vista fuertemente tipada" y seleccionaremos nuestra clase WelcomeViewModel en la lista desplegable. Esta nueva vista solo conocerá WelcomeViewModels y ningún otro tipo de objeto.

> *Nota: debe haber compilado una vez después de agregar el WelcomeViewModel para que aparezca en la lista desplegable.*

Este es el aspecto que debería tener el cuadro de diálogo Agregar vista. Haga clic en el botón Agregar. ![Agregar vista en círculo](getting-started-with-mvc-part3/_static/image10.png)

Agregue este código bajo el &lt;H2&gt; en el nuevo welcome. aspx. Haremos un bucle y digamos Hola tantas veces como se indica en el usuario.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

Además, tenga en cuenta que, al escribir esto, ya le indicamos que se trata de una vista acerca del WelcomeViewModel (es el casado, recuerde?) que obtenemos un IntelliSense útil cada vez que hacemos referencia a nuestro objeto del modelo, tal como se muestra en la siguiente captura de pantalla:

[![código fuente de NumTime](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)

Ejecute la aplicación y visite `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` de nuevo. Ahora que vamos a tomar datos de la dirección URL, se pasa a nuestro controlador automáticamente, nuestro controlador empaqueta los datos en un ViewModel y pasa ese objeto a nuestra vista. La vista que muestra los datos como HTML para el usuario.

[![Bienvenido: Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)

Bueno, eso era un tipo de "M" para el modelo, pero no el tipo de base de datos. Vamos a tomar lo que hemos aprendido y crear una base de datos de películas.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part2.md)
> [Siguiente](getting-started-with-mvc-part4.md)
