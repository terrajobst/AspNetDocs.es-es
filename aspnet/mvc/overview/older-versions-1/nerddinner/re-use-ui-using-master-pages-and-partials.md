---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: Volver a usar la interfaz de usuario con páginas maestras y parciales | Microsoft Docs
author: microsoft
description: El paso 7 examina las formas en las que se puede aplicar el "principio seco" dentro de las plantillas de vista para eliminar la duplicación de código mediante plantillas de vista parcial y páginas maestras.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: 0b17cb6ac14b7f187bf1f175097a37907689d46e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78468727"
---
# <a name="re-use-ui-using-master-pages-and-partials"></a>Reutilizar la interfaz de usuario con páginas maestras y parciales

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 7 de un [tutorial de aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito en el que se explica cómo crear una aplicación web pequeña, pero completa, con ASP.NET MVC 1.
> 
> El paso 7 examina las formas en las que se puede aplicar el "principio seco" dentro de las plantillas de vista para eliminar la duplicación de código, con plantillas de vista parcial y páginas maestras.
> 
> Si usa ASP.NET MVC 3, se recomienda seguir los tutoriales [de la introducción con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o MVC [Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-7-partials-and-master-pages"></a>NerdDinner paso 7: particiones y páginas maestras

Una de las filosofías de diseño que ASP.NET MVC adopta es el principio "no repetirse" (conocido comúnmente como "seco"). Un diseño seco ayuda a eliminar la duplicación de código y lógica, que en última instancia agiliza la compilación y el mantenimiento de las aplicaciones.

Ya hemos visto el principio seco aplicado en varios de nuestros escenarios de NerdDinner. Algunos ejemplos: nuestra lógica de validación se implementa dentro de nuestra capa del modelo, lo que permite que se aplique en los escenarios de edición y creación de nuestro controlador. vamos a usar la plantilla de vista "NotFound" en los métodos de acción de edición, detalles y eliminación. Usamos un patrón de nomenclatura de Convención con nuestras plantillas de vista, lo que elimina la necesidad de especificar explícitamente el nombre cuando se llama al método auxiliar View (). y vamos a usar la clase DinnerFormViewModel para escenarios de acción de edición y creación.

Veamos ahora las maneras en las que podemos aplicar el "principio seco" en nuestras plantillas de vista para eliminar también la duplicación de código.

### <a name="re-visiting-our-edit-and-create-view-templates"></a>Volver a visitar nuestras plantillas editar y crear vista

Actualmente se están usando dos plantillas de vista diferentes: "Edit. aspx" y "Create. aspx", para mostrar la interfaz de usuario del formulario de cena. Una comparación visual rápida de los aspectos destaca su similitud. A continuación se muestra el aspecto del formulario de creación:

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

Y este es el aspecto de nuestro formulario "Editar":

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

¿No existe una gran diferencia? Aparte del texto del título y del encabezado, el diseño del formulario y los controles de entrada son idénticos.

Si abrimos las plantillas de vista "Edit. aspx" y "Create. aspx", veremos que contienen un diseño de formulario y un código de control de entrada idénticos. Esta duplicación significa que tenemos que realizar cambios dos veces en cualquier momento en que se introduzca o cambie una nueva propiedad cena, lo que no es bueno.

### <a name="using-partial-view-templates"></a>Usar plantillas de vista parcial

ASP.NET MVC admite la capacidad de definir plantillas de "vista parcial" que se pueden usar para encapsular la lógica de representación de la vista para una subparte de una página. "Parciales" proporcionan una manera útil de definir la lógica de representación de la vista una vez y, a continuación, volver a usarla en varios lugares de una aplicación.

Para ayudar a "SECAr" la duplicación de la plantilla de vista Edit. aspx y Create. aspx, podemos crear una plantilla de vista parcial denominada "DinnerForm. ascx" que encapsula el diseño del formulario y los elementos de entrada comunes a ambos. Haremos esto haciendo clic con el botón derecho en nuestro directorio/Views/Dinners y eligiendo el comando de menú "agregar&gt;vista":

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

Se mostrará el cuadro de diálogo "agregar vista". Asignaremos un nombre a la nueva vista que queremos crear "DinnerForm", seleccionaremos la casilla "crear una vista parcial" en el cuadro de diálogo e indicar que le pasaremos a una clase DinnerFormViewModel:

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

Cuando hacemos clic en el botón "agregar", Visual Studio creará una nueva plantilla de vista "DinnerForm. ascx" en el directorio "\Views\Dinners".

A continuación, se puede copiar y pegar el código de control de entrada y diseño de formulario duplicado de las plantillas de vista de edición. aspx/Create. aspx en la nueva plantilla de vista parcial "DinnerForm. ascx":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

A continuación, podemos actualizar las plantillas editar y crear vista para llamar a la plantilla parcial DinnerForm y eliminar la duplicación del formulario. Para ello, se llama a HTML. RenderPartial ("DinnerForm") en nuestras plantillas de vista:

##### <a name="createaspx"></a>Create.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

Puede calificar explícitamente la ruta de acceso de la plantilla parcial que desea al llamar a HTML. RenderPartial (por ejemplo: ~ views/cenas/DinnerForm. ascx "). Sin embargo, en el código anterior, aprovechamos el patrón de nomenclatura basado en convenciones dentro de ASP.NET MVC y simplemente especificamos "DinnerForm" como el nombre de la parcial que se va a representar. Cuando hacemos esto, ASP.NET MVC buscará primero en el directorio de vistas basadas en convenciones (para DinnersController esto sería/Views/Dinners). Si no encuentra la plantilla parcial allí, la buscará en el directorio/Views/Shared

Cuando se llama a HTML. RenderPartial () con solo el nombre de la vista parcial, ASP.NET MVC pasará a la vista parcial los mismos objetos de modelo y de diccionario ViewData usados por la plantilla de vista de llamada. Como alternativa, hay versiones sobrecargadas de HTML. RenderPartial () que le permiten pasar un objeto de modelo alternativo y/o un diccionario ViewData para que lo use la vista parcial. Esto resulta útil en escenarios en los que solo desea pasar un subconjunto del modelo/ViewModel completo.

| **Tema del lado: ¿por qué &lt;%%&gt; en lugar de &lt;% =%&gt;?** |
| --- |
| Uno de los aspectos sutiles que podría haber observado con el código anterior es que usamos un bloque &lt;%%&gt; en lugar de un bloque &lt;% =%&gt; al llamar a HTML. RenderPartial (). &lt;% =%&gt; bloques en ASP.NET indican que un desarrollador desea representar un valor especificado (por ejemplo: &lt;% = "Hello"%&gt; presentaría "Hello"). en su lugar &lt;%%&gt; bloques indican que el desarrollador desea ejecutar código y que cualquier salida representada dentro de ellos debe realizarse explícitamente (por ejemplo: &lt;% Response. Write ("Hello")%&gt;. La razón por la que usamos un bloque &lt;%%&gt; con el código HTML. RenderPartial anterior es porque el método html. RenderPartial () no devuelve una cadena y, en su lugar, genera el contenido directamente en el flujo de salida de la plantilla de vista de llamada. Esto lo hace por motivos de eficiencia de rendimiento y, al hacerlo, evita la necesidad de crear un objeto de cadena temporal (potencialmente muy grande). Esto reduce el uso de memoria y mejora el rendimiento general de la aplicación. Un error común al usar HTML. RenderPartial () es olvidar agregar un punto y coma al final de la llamada cuando está dentro de un bloque &lt;%%&gt;. Por ejemplo, este código producirá un error del compilador: &lt;% HTML. RenderPartial ("DinnerForm")%&gt; en su lugar debe escribir: &lt;% HTML. RenderPartial ("DinnerForm"); %&gt; esto se debe a que los bloques &lt;%%&gt; son instrucciones de código independientes y cuando C# las instrucciones de código deben terminar con un punto y coma. |

### <a name="using-partial-view-templates-to-clarify-code"></a>Usar plantillas de vista parcial para aclarar el código

Creamos la plantilla de vista parcial "DinnerForm" para evitar la duplicación de la lógica de representación de la vista en varios lugares. Esta es la razón más común para crear plantillas de vista parcial.

A veces, tiene sentido crear vistas parciales incluso cuando solo se llama en un único lugar. Las plantillas de vista muy complicadas suelen ser mucho más fáciles de leer cuando se extrae la lógica de representación de la vista y se crean particiones en una o varias plantillas parciales con el mismo nombre.

Por ejemplo, considere el siguiente fragmento de código del archivo site. Master en nuestro proyecto (que veremos en breve). El código es relativamente sencillo de leer, en parte porque la lógica para mostrar un vínculo de inicio de sesión o cierre de sesión en la parte superior derecha de la pantalla se encapsula dentro de "control logonusercontrol" parcial:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

Siempre que se sienta confuso intentando comprender el marcado HTML/código dentro de una plantilla de vista, considere si no sería más claro si alguna de ellas se extrajo y refactorizase en vistas parciales con un nombre correcto.

### <a name="master-pages"></a>Páginas maestras

Además de admitir vistas parciales, ASP.NET MVC también admite la capacidad de crear plantillas de "página maestra" que se pueden usar para definir el diseño común y el código HTML de nivel superior de un sitio. Los controles de marcador de posición de contenido se pueden agregar a la página maestra para identificar las regiones reemplazables que se pueden reemplazar o "rellenar" en las vistas. Esto proporciona una manera muy eficaz (y seca) de aplicar un diseño común en una aplicación.

De forma predeterminada, se agregan automáticamente a los nuevos proyectos de ASP.NET MVC una plantilla de página maestra. Esta página maestra se denomina "site. Master" y vive dentro de la carpeta \Views\Shared\:

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

El archivo site. Master predeterminado tiene el siguiente aspecto. Define el HTML exterior del sitio, junto con un menú para la navegación en la parte superior. Contiene dos controles de marcador de posición de contenido reemplazable: uno para el título y el otro para el que se debe reemplazar el contenido principal de una página:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

Todas las plantillas de vista que hemos creado para nuestra aplicación NerdDinner ("list", "details", "Edit", "Create", "NotFound", etc.) se han basado en esta plantilla site. Master. Esto se indica mediante el atributo "MasterPageFile" que se agregó de forma predeterminada a la Directiva Top &lt;% @ Page%&gt; cuando se crearon las vistas mediante el cuadro de diálogo "agregar vista":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

Esto significa que se puede cambiar el contenido del sitio. Master y hacer que los cambios se apliquen y se usen automáticamente cuando se represente cualquiera de nuestras plantillas de vista.

Vamos a actualizar nuestra sección de encabezado de site. Master para que el encabezado de nuestra aplicación sea "NerdDinner" en lugar de "My MVC Application". Vamos a actualizar también el menú de navegación para que la primera pestaña sea "buscar una cena" (controlada por el método de acción index () de HomeController) y agregaremos una nueva pestaña denominada "anfitrión a cena" (controlada por el método de acción Create () de DinnersController):

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

Al guardar el archivo site. Master y actualizar nuestro explorador, veremos que los cambios de encabezado se muestran en todas las vistas de nuestra aplicación. Por ejemplo:

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

Y con la dirección URL de */Dinners/Edit/[id]* :

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>Paso siguiente

Las particiones y las páginas maestras proporcionan opciones muy flexibles que permiten organizar las vistas de forma limpia. Descubrirá que le ayudan a evitar la duplicación del código o el contenido de la vista, y facilitan la lectura y el mantenimiento de las plantillas de vista.

Vamos a revisar el escenario de enumeración que hemos creado anteriormente y habilitar la compatibilidad con paginación escalable.

> [!div class="step-by-step"]
> [Anterior](use-viewdata-and-implement-viewmodel-classes.md)
> [Siguiente](implement-efficient-data-paging.md)
