---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: Usar controladores y vistas para implementar una interfaz de usuario de lista/detalles | Microsoft Docs
author: microsoft
description: En el paso 4 se muestra cómo agregar un controlador a la aplicación que aprovecha nuestro modelo para proporcionar a los usuarios una experiencia de navegación de lista de datos/detalles...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: 74319fe5ea4c79b50140834349e2fdf86420cfbb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486223"
---
# <a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>Usar controladores y vistas para implementar una interfaz de usuario de lista/detalles

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 4 de un [tutorial de aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito en el que se explica cómo crear una aplicación web pequeña, pero completa, con ASP.NET MVC 1.
> 
> En el paso 4 se muestra cómo agregar un controlador a la aplicación que aprovecha nuestro modelo para proporcionar a los usuarios una experiencia de navegación de lista y detalles de datos para cenas en nuestro sitio de NerdDinner.
> 
> Si usa ASP.NET MVC 3, se recomienda seguir los tutoriales [de la introducción con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o MVC [Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-4-controllers-and-views"></a>NerdDinner paso 4: Controladores y vistas

Con los marcos web tradicionales (ASP clásico, PHP, Web Forms de ASP.NET, etc.), las direcciones URL de entrada se asignan normalmente a archivos en disco. Por ejemplo: una solicitud para una dirección URL como "/Products.aspx" o "/Products.php" puede ser procesada por un archivo "Products. aspx" o "Products. php".

Los marcos MVC basados en Web asignan direcciones URL al código del servidor de una manera ligeramente diferente. En lugar de asignar las direcciones URL entrantes a los archivos, asignan direcciones URL a métodos en las clases. Estas clases se denominan "controladores" y son responsables del procesamiento de solicitudes HTTP entrantes, el control de la entrada del usuario, la recuperación y el almacenamiento de datos, y la determinación de la respuesta que se devuelve al cliente (mostrar HTML, descargar un archivo, redirigir a otro URL, etc.).

Ahora que hemos creado un modelo básico para nuestra aplicación de NerdDinner, el siguiente paso será agregar un controlador a la aplicación que aproveche las ventajas del mismo para proporcionar a los usuarios una experiencia de navegación de lista y detalles de datos para las cenas en nuestro sitio.

### <a name="adding-a-dinnerscontroller-controller"></a>Adición de un controlador de DinnersController

Comenzaremos haciendo clic con el botón derecho en la carpeta "Controllers" dentro de nuestro proyecto web y, a continuación, seleccionaremos el comando de menú **Agregar controlador de&gt;** (también puede ejecutar este comando escribiendo Ctrl-M, Ctrl-C):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

Se abrirá el cuadro de diálogo "agregar controlador":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

Asignaremos el nombre "DinnersController" al nuevo controlador y hacer clic en el botón "agregar". A continuación, Visual Studio agregará un archivo DinnersController.cs en nuestro directorio \Controllers:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

También abrirá la nueva clase DinnersController en el editor de código.

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>Agregar métodos de acción index () y Details () a la clase DinnersController

Queremos habilitar visitantes con nuestra aplicación para examinar una lista de las próximas cenas y permitirles hacer clic en cualquier cena de la lista para ver detalles específicos sobre ella. Haremos esto mediante la publicación de las siguientes direcciones URL desde nuestra aplicación:

| **URL** | **Propósito** |
| --- | --- |
| */Dinners/* | Mostrar una lista HTML de las próximas cenas |
| */Dinners/Details/[id.]* | Muestra detalles sobre una cena específica indicada por un parámetro "ID" insertado en la dirección URL, que coincidirá con el DinnerID de la cena en la base de datos. Por ejemplo:/Dinners/Details/2 mostraría una página HTML con detalles sobre la cena cuyo valor DinnerID es 2. |

Publicaremos implementaciones iniciales de estas direcciones URL agregando dos "métodos de acción" públicos a nuestra clase DinnersController como se muestra a continuación:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

A continuación, ejecutaremos la aplicación NerdDinner y usaremos nuestro explorador para invocarla. Al escribir la dirección URL *"/Dinners/"* , se ejecutará el método *index ()* y se devolverá la siguiente respuesta:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

Al escribir la dirección URL *"/Dinners/details/2"* , se ejecutará nuestro método *Details ()* y se devolverá la siguiente respuesta:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

Es posible que se pregunte: ¿cómo ASP.NET MVC sabía crear nuestra clase DinnersController e invocar esos métodos? Para comprenderlo, echemos un vistazo rápido a cómo funciona el enrutamiento.

### <a name="understanding-aspnet-mvc-routing"></a>Descripción del enrutamiento de ASP.NET MVC

ASP.NET MVC incluye un potente motor de enrutamiento de direcciones URL que proporciona una gran flexibilidad a la hora de controlar cómo se asignan las direcciones URL a las clases de controlador. Nos permite personalizar por completo cómo ASP.NET MVC elige qué clase de controlador se debe crear, qué método se debe invocar en ella, así como configurar diferentes maneras en que las variables se pueden analizar automáticamente desde la dirección URL/QueryString y pasar al método como argumentos de parámetro. Ofrece la flexibilidad de optimizar completamente un sitio para SEO (optimización del motor de búsqueda), así como publicar cualquier estructura de direcciones URL que desee de una aplicación.

De forma predeterminada, los nuevos proyectos de ASP.NET MVC acompañan a un conjunto preconfigurado de reglas de enrutamiento de direcciones URL ya registradas. Esto nos permite empezar a trabajar fácilmente en una aplicación sin tener que configurar nada explícitamente. Los registros de reglas de enrutamiento predeterminados se pueden encontrar dentro de la clase "Application" de nuestros proyectos, que podemos abrir haciendo doble clic en el archivo "global. asax" en la raíz del proyecto:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

Las reglas de enrutamiento de MVC de ASP.NET predeterminadas se registran en el método "RegisterRoutes" de esta clase:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

Las rutas. La llamada al método MapRoute () "registra una regla de enrutamiento predeterminada que asigna las direcciones URL entrantes a las clases de controlador mediante el formato de dirección URL:"/{Controller}/{Action}/{ID} ", donde" Controller "es el nombre de la clase de controlador para crear una instancia," Action "es el nombre de un método público que se va a invocar en él y" ID "es un parámetro opcional incrustado en la dirección URL que se puede pasar como argumento al método. El tercer parámetro pasado a la llamada al método "MapRoute ()" es un conjunto de valores predeterminados que se usarán para los valores de controlador/acción/identificador en caso de que no estén presentes en la dirección URL (Controller = "Home", Action = "index", ID = "").

A continuación se muestra una tabla que muestra cómo se asignan varias direcciones URL mediante la regla de ruta predeterminada "<em>/{Controllers}/{Action}/{ID}"</em>:

| **URL** | **Controller (clase)** | **Método de acción** | **Parámetros pasados** |
| --- | --- | --- | --- |
| */Dinners/Details/2* | DinnersController | Detalles (ID.) | ID. = 2 |
| */Dinners/Edit/5* | DinnersController | Editar (ID.) | ID. = 5 |
| */Dinners/Create* | DinnersController | Create() | N/D |
| */Dinners* | DinnersController | Index () | N/D |
| */Home* | HomeController | Index () | N/D |
| */* | HomeController | Index () | N/D |

Las tres últimas filas muestran los valores predeterminados (Controller = Home, Action = index, ID = "") que se están usando. Dado que el método "index" está registrado como el nombre de la acción predeterminada si no se especifica ninguno, las direcciones URL "/Dinners" y "/Home" hacen que se invoque el método de acción index () en sus clases de controlador. Dado que el controlador "Home" está registrado como controlador predeterminado si no se especifica ninguno, la dirección URL "/" hace que se cree el HomeController y el método de acción index () que se va a invocar.

Si no le gusta estas reglas de enrutamiento de direcciones URL predeterminadas, la buena noticia es que son fáciles de cambiar, simplemente editarlas en el método RegisterRoutes anterior. Sin embargo, para nuestra aplicación NerdDinner, no vamos a cambiar ninguna de las reglas de enrutamiento de direcciones URL predeterminadas, sino que solo las usaremos tal cual.

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>Uso del DinnerRepository de DinnersController

Vamos a reemplazar ahora nuestra implementación actual de los métodos de acción index () y Details () de DinnersController con implementaciones que usan nuestro modelo.

Usaremos la clase DinnerRepository que creamos anteriormente para implementar el comportamiento. Comenzaremos agregando una instrucción "Using" que haga referencia al espacio de nombres "NerdDinner. models" y, a continuación, declare una instancia de DinnerRepository como un campo en nuestra clase DinnerController.

Más adelante en este capítulo introduciremos el concepto de "inserción de dependencias" y mostraremos otra manera para que nuestros controladores obtengan una referencia a un DinnerRepository que permita una mejor prueba unitaria, pero, de momento, solo crearemos una instancia de nuestro DinnerRepository como se indica a continuación.

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

Ahora estamos preparados para volver a generar una respuesta HTML mediante nuestros objetos de modelo de datos recuperados.

### <a name="using-views-with-our-controller"></a>Uso de vistas con nuestro controlador

Aunque es posible escribir código dentro de los métodos de acción para ensamblar HTML y, a continuación, usar el método auxiliar *Response. Write ()* para devolverlo al cliente, este enfoque resulta bastante difícil de manejar rápidamente. Un enfoque mucho mejor es para nosotros solo realizar la lógica de la aplicación y los datos dentro de los métodos de acción de DinnersController y, a continuación, pasar los datos necesarios para representar una respuesta HTML a una plantilla de "vista" independiente responsable de generar la representación HTML. del mismo. Como veremos en un momento, una plantilla "vista" es un archivo de texto que normalmente contiene una combinación de marcado HTML y código de representación incrustado.

La separación de la lógica del controlador de nuestra representación de la vista aporta varias ventajas. En concreto, ayuda a aplicar una "separación de preocupaciones" clara entre el código de la aplicación y el código de formato y representación de la interfaz de usuario. Esto hace que sea mucho más fácil realizar pruebas unitarias de la lógica de la aplicación en el aislamiento de la lógica de representación de la interfaz de usuario. De este modo, resulta más fácil modificar las plantillas de representación de la interfaz de usuario sin tener que realizar cambios en el código de la aplicación. Y puede facilitar a los desarrolladores y diseñadores la colaboración en proyectos.

Podemos actualizar nuestra clase DinnersController para indicar que deseamos usar una plantilla de vista para devolver una respuesta de la interfaz de usuario HTML cambiando las signaturas de método de los dos métodos de acción de tener un tipo de valor devuelto de "Void" a en su lugar un tipo de valor devuelto de "ActionResult". A continuación, podemos llamar al método auxiliar *View ()* en la clase base del controlador para devolver un objeto "ViewResult" como el siguiente:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

La firma del método auxiliar *View ()* que usamos anteriormente tiene el siguiente aspecto:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

El primer parámetro del método auxiliar *View ()* es el nombre del archivo de plantilla de vista que se desea usar para representar la respuesta HTML. El segundo parámetro es un objeto de modelo que contiene los datos que necesita la plantilla de vista para representar la respuesta HTML.

En nuestro método de acción index (), se llama al método de la aplicación auxiliar *View ()* y se indica que queremos representar una lista HTML de cenas mediante una plantilla de vista "index". Vamos a pasar la plantilla de vista a una secuencia de objetos cenas de la que se va a generar la lista:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

En nuestro método de acción Details (), se intenta recuperar un objeto cena con el identificador proporcionado en la dirección URL. Si se encuentra una cena válida, llamamos al método auxiliar *View ()* , que indica que queremos usar una plantilla de vista "details" para representar el objeto cena recuperado. Si se solicita una cena no válida, se presenta un mensaje de error útil que indica que la cena no existe mediante una plantilla de vista "NotFound" (y una versión sobrecargada del método auxiliar *View ()* que solo toma el nombre de la plantilla):

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

Vamos a implementar ahora las plantillas de vista "NotFound", "details" y "index".

### <a name="implementing-the-notfound-view-template"></a>Implementación de la plantilla de vista "NotFound"

Comenzaremos implementando la plantilla de vista "NotFound", que muestra un mensaje de error descriptivo que indica que no se puede encontrar la cena solicitada.

Vamos a crear una nueva plantilla de vista colocando el cursor de texto dentro de un método de acción del controlador y, a continuación, hacer clic con el botón derecho y elegir el comando de menú "agregar vista" (también podemos ejecutar este comando escribiendo Ctrl-M, Ctrl-V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

Se abrirá un cuadro de diálogo "agregar vista" como se muestra a continuación. De forma predeterminada, el cuadro de diálogo rellenará previamente el nombre de la vista que se va a crear para que coincida con el nombre del método de acción en el que se encontraba el cursor cuando se inició el cuadro de diálogo (en este caso, "details"). Dado que queremos implementar primero la plantilla "NotFound", reemplazaremos este nombre de vista y lo estableceremos en, en su lugar, en "NotFound":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

Cuando hacemos clic en el botón "agregar", Visual Studio creará una nueva plantilla de vista "NotFound. aspx" para nosotros en el directorio "\Views\Dinners" (que también creará si el directorio no existe ya):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

También abrirá nuestra nueva plantilla de vista "NotFound. aspx" en el editor de código:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

De forma predeterminada, las plantillas de vista tienen dos "regiones de contenido" donde se pueden agregar contenido y código. Lo primero nos permite personalizar el "título" de la página HTML que se devuelve. La segunda nos permite personalizar el "contenido principal" de la página HTML que se devuelve.

Para implementar nuestra plantilla de vista "NotFound", agregaremos contenido básico:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

A continuación, podemos probarla en el explorador. Para ello, vamos a solicitar la dirección URL *"/Dinners/details/9999"* . Esto hará referencia a una cena que no existe actualmente en la base de datos y hará que nuestro método de acción DinnersController. Details () presente la plantilla de vista "NotFound":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

Una de las cosas que observará en la captura de pantalla anterior es que nuestra plantilla de vista básica ha heredado un montón de HTML que rodea el contenido principal de la pantalla. Esto se debe a que nuestra plantilla de vista está usando una plantilla de "página maestra" que nos permite aplicar un diseño coherente en todas las vistas del sitio. Veremos cómo funcionan más las páginas maestras más adelante en este tutorial.

### <a name="implementing-the-details-view-template"></a>Implementación de la plantilla de vista "details"

Ahora vamos a implementar la plantilla de vista "details", que generará HTML para un único modelo de cena.

Haremos esto colocando el cursor de texto en el método de acción de detalles y luego haga clic con el botón derecho y elija el comando de menú "agregar vista" (o presione Ctrl-M, Ctrl-V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

Se abrirá el cuadro de diálogo "agregar vista". Conservaremos el nombre de la vista predeterminada ("details"). También seleccionaremos la casilla "crear una vista fuertemente tipada" en el cuadro de diálogo y seleccionaremos (mediante la lista desplegable del cuadro combinado) el nombre del tipo de modelo que pasamos del controlador a la vista. En esta vista vamos a pasar un objeto cena (el nombre completo de este tipo es: "NerdDinner. Models. cena"):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

A diferencia de la plantilla anterior, donde decidimos crear una "vista vacía", esta vez elegiremos automáticamente "scaffolding" a la vista mediante una plantilla "details". Podemos indicarlo cambiando la lista desplegable "ver contenido" en el cuadro de diálogo anterior.

"Scaffolding" generará una implementación inicial de la plantilla de vista de detalles basada en el objeto cena que pasamos a ella. Esto proporciona una manera fácil de empezar a trabajar rápidamente en nuestra implementación de la plantilla de vista.

Cuando hacemos clic en el botón "agregar", Visual Studio creará un nuevo archivo de plantilla de vista "details. aspx" para nosotros en nuestro directorio "\Views\Dinners":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

También abrirá nuestra nueva plantilla de vista "details. aspx" en el editor de código. Contendrá una implementación de scaffolding inicial de una vista de detalles basada en un modelo de cena. El motor de scaffolding usa la reflexión de .NET para ver las propiedades públicas expuestas en la clase pasada y agregará el contenido adecuado en función de cada tipo que encuentre:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

Podemos solicitar la dirección URL *"/Dinners/details/1"* para ver cuál es el aspecto de la implementación de scaffolding "details" en el explorador. El uso de esta dirección URL mostrará una de las cenas que se agregaron manualmente a nuestra base de datos cuando se creó por primera vez:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

Esto nos permite empezar a trabajar rápidamente y nos proporciona una implementación inicial de la vista details. aspx. A continuación, podemos ir y retocarlo para personalizar la interfaz de usuario a nuestra satisfacción.

Cuando examinemos la plantilla details. aspx más detenidamente, veremos que contiene HTML estático, así como código de representación incrustado. &lt;%%&gt; código de información de código ejecuta código cuando se representa la plantilla de vista y &lt;% =%&gt; fragmentos de código ejecutan el código incluido en ellos y, a continuación, representan el resultado en el flujo de salida de la plantilla.

Podemos escribir código dentro de nuestra vista que tenga acceso al objeto de modelo "cena" que se pasó desde nuestro controlador mediante una propiedad de "modelo" fuertemente tipada. Visual Studio nos proporciona código completo (IntelliSense) al tener acceso a esta propiedad "Model" en el editor:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

Vamos a hacer algunos ajustes para que el origen de la plantilla de vista de detalles final tenga el siguiente aspecto:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

Cuando accedamos de nuevo a la dirección URL *"/Dinners/details/1"* , ahora se presentará como se indica a continuación:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>Implementar la plantilla de vista "índice"

Ahora vamos a implementar la plantilla de vista "index", que generará una lista de las próximas cenas. Para ello, colocaremos el cursor de texto en el método de acción de índice y, a continuación, hacer clic con el botón derecho y elegir el comando de menú "agregar vista" (o presionar Ctrl-M, Ctrl-V).

En el cuadro de diálogo "agregar vista", guardaremos la plantilla de vista denominada "índice" y seleccionaremos la casilla "crear una vista fuertemente tipada". Esta vez, elegiremos generar automáticamente una plantilla de vista "list" y seleccionaremos "NerdDinner. Models. cena" como el tipo de modelo que se pasa a la vista (lo que nos ha indicado que vamos a crear un scaffolding de "lista" hará que el cuadro de diálogo Agregar vista asuma que estamos pasar una secuencia de objetos cenas del controlador a la vista):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

Cuando hacemos clic en el botón "agregar", Visual Studio creará un nuevo archivo de plantilla de vista "index. aspx" para nosotros en nuestro directorio "\Views\Dinners". "Scaffolding" es una implementación inicial dentro de ella que proporciona una lista de tablas HTML de las cenas que pasamos a la vista.

Cuando se ejecuta la aplicación y se accede a la dirección URL *"/Dinners/"* , se presentará nuestra lista de cenas, como por ejemplo:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

La solución de tabla anterior nos proporciona un diseño similar a la red de los datos de la cena, que no es lo que queremos para nuestra lista de cenas orientada al consumidor. Podemos actualizar la plantilla de vista index. aspx y modificarla para que muestre menos columnas de datos y usar un elemento &lt;UL&gt; para representarlos en lugar de una tabla mediante el código siguiente:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

Usamos la palabra clave "var" dentro de la instrucción foreach anterior a medida que se recorre cada cena del modelo. Los que no están C# familiarizados con 3,0 pueden creer que el uso de "var" significa que el objeto cena está enlazado en tiempo de ejecución. En su lugar, significa que el compilador usa la inferencia de tipos en la propiedad "Model" fuertemente tipada (que es de tipo "IEnumerable&lt;cena&gt;") y compilar la variable local "cena" como tipo de cena, lo que significa que obtenemos la comprobación completa de IntelliSense y tiempo de compilación en bloques de código:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

Cuando se alcanza la actualización en la dirección URL de */Dinners* en nuestro explorador, nuestra vista actualizada ahora tiene el siguiente aspecto:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

Esto es más eficaz, pero aún no está completamente. El último paso consiste en permitir que los usuarios finales haga clic en las cenas individuales en la lista y vea los detalles sobre ellas. Lo implementaremos mediante la representación de elementos de hipervínculo HTML que se vinculan al método de acción details en nuestro DinnersController.

Podemos generar estos hipervínculos en nuestra vista de índice de una de estas dos maneras. La primera es crear manualmente HTML &lt;un&gt; elementos como el siguiente, donde se insertan &lt;bloques%%&gt; en el &lt;un elemento HTML&gt;:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

Un enfoque alternativo que se puede usar es aprovechar el método auxiliar integrado "HTML. ActionLink ()" en ASP.NET MVC que admite la creación mediante programación de un elemento HTML &lt;un&gt; elemento que se vincula a otro método de acción en un controlador:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

Primer parámetro para el código HTML. el método auxiliar ActionLink () es el texto del vínculo que se va a mostrar (en este caso, el título de la cena), el segundo parámetro es el nombre de la acción del controlador en el que se desea generar el vínculo (en este caso, el método Details) y el tercer parámetro es un conjunto de parámetros que se van a enviar a la acción (se implementa como un En este caso, estamos especificando el parámetro "ID" de la cena a la que queremos vincularnos y, dado que la regla de enrutamiento de direcciones URL predeterminada en ASP.NET MVC es "{Controller}/{Action}/{id}", el método auxiliar HTML. ActionLink () generará el siguiente resultado:

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

En nuestra vista index. aspx, usaremos el método de aplicación auxiliar HTML. ActionLink () y cada cena en la lista se vinculará a la dirección URL de detalles adecuada:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

Y ahora, cuando lleguemos a la dirección URL de */Dinners* , nuestra lista de cenas tiene el siguiente aspecto:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

Cuando hacemos clic en cualquiera de las cenas de la lista, navegaremos para ver detalles sobre ella:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>Nomenclatura basada en Convención y la estructura de directorios \Views

Las aplicaciones de ASP.NET MVC usan de forma predeterminada una estructura de nomenclatura de directorios basada en convenciones al resolver plantillas de vista. Esto permite a los desarrolladores evitar tener que calificar completamente una ruta de acceso de ubicación al hacer referencia a vistas desde una clase de controlador. De forma predeterminada, ASP.NET MVC buscará el archivo de plantilla de vista en el directorio * \Views\[ControllerName]\* debajo de la aplicación.

Por ejemplo, hemos estado trabajando en la clase DinnersController, que hace referencia explícitamente a tres plantillas de vista: "index", "details" y "NotFound". ASP.NET MVC buscará estas vistas en el directorio *\Views\Dinners* debajo del directorio raíz de la aplicación de forma predeterminada:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

Observe sobre cómo existen actualmente tres clases de controlador en el proyecto (DinnersController, HomeController y AccountController; los dos últimos se agregaron de forma predeterminada cuando se creó el proyecto) y hay tres subdirectorios (uno para cada uno). Controller) en el directorio \Views

Las vistas a las que se hace referencia desde los controladores Home y accounts resolverán automáticamente sus plantillas de vista a partir de los directorios *\Views\Home* y *\Views\Account* correspondientes. El subdirectorio *\Views\Shared* proporciona una manera de almacenar las plantillas de vista que se reutilizan en varios controladores dentro de la aplicación. Cuando ASP.NET MVC intenta resolver una plantilla de vista, primero comprobará en el directorio específico de *\Views\[Controller]* y, si no encuentra la plantilla de vista allí, se buscará en el directorio *\Views\Shared*

En lo que se refiere a las plantillas de vista individuales, la guía recomendada es hacer que la plantilla de vista comparta el mismo nombre que el método de acción que hizo que se representara. Por ejemplo, en el método de acción "índice" se usa la vista "índice" para representar el resultado de la vista y el método de acción "details" está usando la vista "details" para representar los resultados. Esto hace que sea fácil ver rápidamente qué plantilla está asociada a cada acción.

No es necesario que los desarrolladores especifiquen explícitamente el nombre de la plantilla de vista cuando la plantilla de vista tiene el mismo nombre que el método de acción que se invoca en el controlador. En su lugar, se puede pasar el objeto de modelo al método auxiliar "View ()" (sin especificar el nombre de vista) y ASP.NET MVC infiere que se desea usar la plantilla de vista *\Views\[ControllerName]\[ActionName]* en el disco para representarlo.

Esto nos permite limpiar el código del controlador un poco y evitar duplicar el nombre dos veces en nuestro código:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

El código anterior es todo lo que se necesita para implementar una buena experiencia de lista y detalles de la cena para el sitio.

#### <a name="next-step"></a>Paso siguiente

Ahora tenemos una buena experiencia de exploración de la cena creada.

Ahora vamos a habilitar la compatibilidad con la edición de formularios de datos CRUD (crear, leer, actualizar y eliminar).

> [!div class="step-by-step"]
> [Anterior](build-a-model-with-business-rule-validations.md)
> [Siguiente](provide-crud-create-read-update-delete-data-form-entry-support.md)
