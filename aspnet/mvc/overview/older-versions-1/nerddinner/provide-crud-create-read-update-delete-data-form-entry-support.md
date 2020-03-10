---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: Proporcionar compatibilidad con la entrada de formulario de datos CRUD (crear, leer, actualizar, eliminar) | Microsoft Docs
author: microsoft
description: En el paso 5 se muestra cómo tomar la clase DinnersController más allá habilitando la compatibilidad con la edición, la creación y la eliminación de cenas también.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: b3123af9a1477bc496a0d229d628510fc202b6d2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78468919"
---
# <a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>Proporcionar la compatibilidad con entradas de formularios de datos CRUD (crear, leer, actualizar y eliminar)

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 5 de un [tutorial de aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito en el que se explica cómo crear una aplicación web pequeña, pero completa, con ASP.NET MVC 1.
> 
> En el paso 5 se muestra cómo tomar la clase DinnersController más allá habilitando la compatibilidad con la edición, la creación y la eliminación de cenas también.
> 
> Si usa ASP.NET MVC 3, se recomienda seguir los tutoriales [de la introducción con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o MVC [Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>NerdDinner paso 5: crear, actualizar y eliminar escenarios de formulario

Hemos introducido controladores y vistas, y hemos tratado cómo usarlos para implementar una experiencia de lista/detalles para cenas en el sitio. El siguiente paso será seguir nuestra clase DinnersController y habilitar la compatibilidad con la edición, la creación y la eliminación de cenas también.

### <a name="urls-handled-by-dinnerscontroller"></a>Direcciones URL administradas por DinnersController

Anteriormente hemos agregado métodos de acción a DinnersController que implementaban compatibilidad con dos direcciones URL: */Dinners* y */Dinners/details/[id]* .

| **URL** | **SOLICITUD** | **Propósito** |
| --- | --- | --- |
| */Dinners/* | GET | Mostrar una lista HTML de las cenas futuras. |
| */Dinners/Details/[id.]* | GET | Muestra detalles acerca de una cena específica. |

Ahora agregaremos métodos de acción para implementar tres direcciones URL adicionales: */Dinners/Edit/[id]* , */Dinners/Create*y */Dinners/delete/[id]* . Estas direcciones URL habilitarán la compatibilidad con la edición de cenas existentes, la creación de nuevas cenas y la eliminación de cenas.

Se admitirán las interacciones HTTP GET y HTTP POST con estas nuevas direcciones URL. Las solicitudes HTTP GET a estas direcciones URL mostrarán la vista HTML inicial de los datos (un formulario rellenado con los datos de la cena en el caso de "Edit", un formulario en blanco en el caso de "Create" y una pantalla de confirmación de eliminación en el caso de "Delete"). Las solicitudes HTTP POST a estas direcciones URL guardarán/actualizarán/eliminarán los datos de la cena en nuestro DinnerRepository (y de ahí en la base de datos).

| **URL** | **SOLICITUD** | **Propósito** |
| --- | --- | --- |
| */Dinners/Edit/[id.]* | GET | Muestra un formulario HTML modificable rellenado con datos de cenas. |
| EXPONER | Guarde los cambios del formulario de una cena determinada en la base de datos. |
| */Dinners/Create* | GET | Muestra un formulario HTML vacío que permite a los usuarios definir nuevas cenas. |
| EXPONER | Cree una nueva cena y guárdela en la base de datos. |
| */Dinners/Delete/[id.]* | GET | Muestra la pantalla de confirmación de eliminación. |
| EXPONER | Elimina la cena especificada de la base de datos. |

### <a name="edit-support"></a>Editar compatibilidad

Comencemos por implementar el escenario "Edit" (editar).

#### <a name="the-http-get-edit-action-method"></a>Método de acción de edición HTTP-GET

Comenzaremos implementando el comportamiento HTTP "GET" de nuestro método de acción de edición. Este método se invocará cuando se solicite la dirección URL de */Dinners/Edit/[id]* . Nuestra implementación tendrá el siguiente aspecto:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

El código anterior usa DinnerRepository para recuperar un objeto cena. Después representa una plantilla de vista con el objeto cena. Dado que no se ha pasado explícitamente un nombre de plantilla al método auxiliar *View ()* , se usará la Convención predeterminada basada en Convención para resolver la plantilla de vista:/views/Dinners/Edit.aspx.

Vamos a crear ahora esta plantilla de vista. Haremos esto haciendo clic con el botón derecho en el método editar y seleccionando el comando del menú contextual "agregar vista":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

En el cuadro de diálogo "agregar vista", indicaremos que vamos a pasar un objeto cena a nuestra plantilla de vista como modelo y a elegir aplicar scaffolding automáticamente a una plantilla "Editar":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

Cuando hacemos clic en el botón "agregar", Visual Studio agregará un nuevo archivo de plantilla de vista "Edit. aspx" en el directorio "\Views\Dinners". También abrirá la nueva plantilla de vista "Edit. aspx" en el editor de código, que se rellena con una implementación de scaffolding "Edit" inicial como la siguiente:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

Vamos a realizar algunos cambios en el scaffolding predeterminado "Edit" generado y actualizar la plantilla de vista de edición para que tenga el contenido siguiente (que quita algunas de las propiedades que no queremos exponer):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

Cuando ejecutemos la aplicación y solicitemos la dirección URL *"/Dinners/Edit/1"* , veremos la siguiente página:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

El formato HTML generado por nuestra vista tiene el siguiente aspecto. Es HTML estándar, con un formulario &lt;&gt; elemento que realiza una solicitud HTTP POST a la dirección URL de */Dinners/Edit/1* cuando se inserta el botón "guardar" &lt;tipo de entrada = "Submit"/&gt;. Se ha generado una salida HTML &lt;tipo de entrada = "Text"/&gt; para cada propiedad modificable:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>Métodos auxiliares HTML de HTML. BeginForm () y HTML. TextBox ()

Nuestra plantilla de vista "Edit. aspx" usa varios métodos de "aplicación auxiliar HTML": HTML. ValidationSummary (), HTML. BeginForm (), HTML. TextBox () y HTML. ValidationMessage (). Además de generar el marcado HTML para nosotros, estos métodos auxiliares proporcionan compatibilidad integrada con el control de errores y la validación.

##### <a name="htmlbeginform-helper-method"></a>Método auxiliar HTML. BeginForm ()

El método auxiliar HTML. BeginForm () es lo que genera el formato HTML &lt;&gt; elemento en el marcado. En la plantilla de vista Edit. aspx, observará que estamos aplicando C# una instrucción "Using" al usar este método. La llave de apertura indica el principio del formulario de &lt;&gt; contenido y la llave de cierre es lo que indica el final del elemento de&gt; de &lt;/Form.:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

Como alternativa, si encuentra el enfoque de la instrucción "Using" no natural para un escenario como este, puede usar una combinación HTML. BeginForm () y HTML. EndForm () (que hace lo mismo):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

Si se llama a HTML. BeginForm () sin ningún parámetro, se generará un elemento de formulario que realiza una POST HTTP en la dirección URL de la solicitud actual. Esta es la razón por la que nuestra vista de edición genera una *&lt;Form Action = "/Dinners/Edit/1" Method = "post"&gt;* elemento. Podríamos haber pasado parámetros explícitos a HTML. BeginForm () si quisiéramos publicar en una dirección URL diferente.

##### <a name="htmltextbox-helper-method"></a>Método auxiliar HTML. TextBox ()

Nuestra vista Edit. aspx usa el método auxiliar HTML. TextBox () para generar &lt;tipo de entrada = "Text"/&gt; elementos:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

El método html. TextBox () anterior toma un parámetro único, que se usa para especificar los atributos de identificador y nombre del &lt;tipo de entrada = "Text"/&gt; de salida, así como la propiedad de modelo de la que se va a rellenar el valor de cuadro de texto. Por ejemplo, el objeto cena que pasamos a la vista de edición tenía el valor de la propiedad "title" de ".NET Futures" y, por tanto, nuestra llamada al método de HTML. TextBox ("title"): *&lt;ID. de entrada = "title" Name = "title" type = "Text" Value = ". net Futures"/&gt;* .

Como alternativa, se puede usar el primer parámetro HTML. TextBox () para especificar el identificador o el nombre del elemento y, a continuación, pasar explícitamente el valor que se va a usar como segundo parámetro:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

A menudo, queremos realizar un formato personalizado en el valor que se genera. El método estático String. Format () integrado en .NET es útil para estos escenarios. La plantilla de vista Edit. aspx se usa para dar formato al valor EventDate (que es de tipo DateTime), de modo que no muestre los segundos para la hora:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Opcionalmente, se puede usar un tercer parámetro para HTML. TextBox () para generar atributos HTML adicionales. El siguiente fragmento de código muestra cómo representar un atributo size = "30" adicional y un atributo class = "mycssclass" en el elemento &lt;input type = "Text"/&gt;. Observe cómo vamos a escapar el nombre del atributo de clase mediante una "clase@" character because "" es una palabra clave reservada C#en:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>Implementar el método de acción de edición HTTP-POST

Ahora tenemos la versión HTTP-GET de nuestro método de acción de edición implementado. Cuando un usuario solicita la dirección URL de */Dinners/Edit/1* , recibe una página HTML como la siguiente:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

Si se presiona el botón "guardar", se produce un envío de formulario a la dirección URL de */Dinners/Edit/1* y se envía la entrada de &lt;HTML&gt; valores de formulario mediante el verbo http post. Ahora vamos a implementar el comportamiento HTTP POST de nuestro método de acción de edición, que controlará el guardado de la cena.

Comenzaremos agregando un método de acción "Edit" sobrecargado a nuestro DinnersController que tenga un atributo "AcceptVerbs" en él que indique que se trata de escenarios HTTP POST:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

Cuando se aplica el atributo [AcceptVerbs] a métodos de acción sobrecargados, ASP.NET MVC controla automáticamente las solicitudes de envío al método de acción adecuado según el verbo HTTP de entrada. Las solicitudes HTTP POST a las direcciones URL de */Dinners/Edit/[id]* Irán al método de edición anterior, mientras que todas las demás solicitudes HTTP Verb a las direcciones URL de */Dinners/Edit/[id]* Irán al primer método de edición implementado (que no tenía un atributo `[AcceptVerbs]`).

| **Tema del lado: ¿por qué diferenciar a través de verbos HTTP?** |
| --- |
| Podría preguntar: ¿por qué usamos una sola dirección URL y diferencia su comportamiento a través del verbo HTTP? ¿Por qué no solo hay dos direcciones URL independientes para controlar la carga y el guardado de los cambios de edición? Por ejemplo:/Dinners/Edit/[id] para mostrar el formulario inicial y/Dinners/Save/[id] para controlar el envío de formulario para guardarlo. El inconveniente de publicar dos direcciones URL independientes es que, en los casos en que se publican en/Dinners/Save/2 y, a continuación, es necesario volver a mostrar el formulario HTML debido a un error de entrada, el usuario final terminará teniendo la dirección URL de/Dinners/Save/2 en la barra de direcciones de su explorador (ya que se trata de la dirección URL en la que se publicó el formulario). Si el usuario final marca esta página que se vuelve a mostrar en la lista de favoritos del explorador, o copia o pega la dirección URL y la envía por correo electrónico a un amigo, terminará guardando una dirección URL que no funcionará en el futuro (ya que esa dirección URL depende de los valores post). Al exponer una sola dirección URL (por ejemplo:/Dinners/Edit/[id]) y diferenciar el procesamiento de la misma por el verbo HTTP, es seguro que los usuarios finales marquen la página de edición y/o envíen la dirección URL a otras personas. |

#### <a name="retrieving-form-post-values"></a>Recuperación de valores de envío de formulario

Hay varias maneras de acceder a los parámetros de formulario publicados en nuestro método "Edit" de HTTP POST. Un enfoque sencillo es usar simplemente la propiedad request en la clase base del controlador para tener acceso a la colección de formularios y recuperar los valores expuestos directamente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

No obstante, el enfoque anterior es un poco detallado, especialmente una vez que se agrega la lógica de control de errores.

Un mejor enfoque para este escenario es aprovechar el método auxiliar *UpdateModel ()* integrado en la clase base del controlador. Admite la actualización de las propiedades de un objeto que se pasa con los parámetros de formulario de entrada. Utiliza la reflexión para determinar los nombres de propiedad en el objeto y, a continuación, convierte y asigna valores automáticamente en función de los valores de entrada enviados por el cliente.

Podríamos usar el método UpdateModel () para simplificar nuestra acción de edición HTTP-POST mediante este código:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

Ahora podemos visitar la dirección URL de */Dinners/Edit/1* y cambiar el título de la cena:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

Cuando hacemos clic en el botón "guardar", realizaremos un envío de formulario a nuestra acción de edición y los valores actualizados se conservarán en la base de datos. A continuación, se le redirigirá a la dirección URL de detalles de la cena (que mostrará los valores recién guardados):

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>Control de errores de edición

Nuestra implementación HTTP-POST actual funciona bien, excepto cuando hay errores.

Cuando un usuario comete un error al editar un formulario, es necesario asegurarse de que el formulario se vuelve a mostrar con un mensaje de error informativo que lo guía para corregirlo. Esto incluye los casos en los que un usuario final envía una entrada incorrecta (por ejemplo: una cadena de fecha con formato incorrecto), así como los casos en los que el formato de entrada es válido, pero hay una infracción de la regla de negocios. Cuando se producen errores, el formulario debe conservar los datos de entrada que el usuario especificó originalmente para que no tengan que volver a rellenar los cambios manualmente. Este proceso debe repetirse tantas veces como sea necesario hasta que el formulario se complete correctamente.

ASP.NET MVC incluye algunas características integradas que facilitan el control de errores y la presentación de formularios. Para ver estas características en acción, vamos a actualizar nuestro método de acción de edición con el código siguiente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

El código anterior es similar a nuestra implementación anterior, con la excepción de que ahora estamos encapsulando un bloque de control de errores try/catch en torno a nuestro trabajo. Si se produce una excepción al llamar a UpdateModel () o al intentar y guardar DinnerRepository (lo que generará una excepción si el objeto cena que estamos intentando guardar no es válido debido a una infracción de la regla en nuestro modelo), nuestro bloque de control de errores de filtrado ejecut. Dentro de ella, se recorren las infracciones de las reglas que existen en el objeto cena y se agregan a un objeto ModelState (que hablaremos en breve). A continuación, se vuelve a mostrar la vista.

Para ver este trabajo, vamos a volver a ejecutar la aplicación, editar una cena y cambiarla para que tenga un título vacío, un EventDate de "fantasma" y usar un número de teléfono del Reino Unido con un valor de país de EE. UU. Cuando se presiona el botón "guardar", el método de edición HTTP POST no podrá guardar la cena (porque hay errores) y volverá a mostrar el formulario:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

Nuestra aplicación tiene una experiencia de error decente. Los elementos de texto con la entrada no válida se resaltan en rojo y los mensajes de error de validación se muestran al usuario final sobre ellos. El formulario también conserva los datos de entrada que el usuario entró originalmente, de modo que no tienen que rellenar nada.

¿Cómo se puede preguntar? ¿se produjo esto? ¿Cómo se resaltan los cuadros de texto título, EventDate y ContactPhone en rojo y sabe que generan los valores de usuario especificados originalmente? ¿Y cómo se muestran los mensajes de error en la lista en la parte superior? La buena noticia es que esto no se ha producido por Magic, sino que hemos usado algunas de las características integradas de ASP.NET MVC que facilitan la validación de entradas y los escenarios de control de errores.

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>Descripción de ModelState y los métodos auxiliares HTML de validación

Las clases de controlador tienen una colección de propiedades "ModelState" que proporciona una manera de indicar que existen errores con un objeto de modelo que se pasa a una vista. Las entradas de error dentro de la colección ModelState identifican el nombre de la propiedad del modelo con el problema (por ejemplo: "title", "EventDate" o "ContactPhone") y permiten especificar un mensaje de error descriptivo (por ejemplo: "el título es obligatorio").

El método auxiliar *UpdateModel ()* rellena automáticamente la colección ModelState cuando encuentra errores al intentar asignar valores de formulario a las propiedades del objeto de modelo. Por ejemplo, la propiedad EventDate del objeto cena es de tipo DateTime. Cuando el método UpdateModel () no pudo asignar el valor de cadena "fantasma" en el escenario anterior, el método UpdateModel () agregó una entrada a la colección ModelState que indica que se ha producido un error de asignación con esa propiedad.

Los desarrolladores también pueden escribir código para agregar entradas de error explícitamente en la colección ModelState como estamos haciendo a continuación en nuestro bloque de control de errores "Catch", que está rellenando la colección ModelState con entradas basadas en las infracciones de la regla activa en el Objeto cena:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>Integración de la aplicación auxiliar HTML con ModelState

Métodos auxiliares HTML: como HTML. TextBox (): Compruebe la colección ModelState al representar la salida. Si existe un error para el elemento, representan el valor especificado por el usuario y una clase de error de CSS.

Por ejemplo, en nuestra vista "Editar" usamos el método auxiliar HTML. TextBox () para representar el EventDate del objeto cena:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

Cuando la vista se representó en el escenario de error, el método html. TextBox () comprobó la colección ModelState para ver si hubo errores asociados a la propiedad "EventDate" del objeto cena. Cuando se determinó que se produjo un error, se representaba la entrada del usuario enviada ("fantasma") como valor y se agregaba una clase de error de CSS al &lt;tipo de entrada = "TextBox"/&gt; marcado que generaba:

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

Puede personalizar la apariencia de la clase de error de CSS para que tenga la apariencia que desee. La clase de error CSS predeterminada, "Input-Validation-error", se define en la hoja de estilos *\content\site.CSS* y tiene el siguiente aspecto:

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

Esta regla de CSS es lo que hizo que los elementos de entrada no válidos se resaltaran como se indica a continuación:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>Método auxiliar HTML. ValidationMessage ()

El método auxiliar HTML. ValidationMessage () se puede usar para generar el mensaje de error ModelState asociado a una propiedad de modelo determinada:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

El código anterior genera: *&lt;span class = "Field-Validation-error"&gt; el valor ' fantasma ' no es válido&lt;/span&gt;*

El método auxiliar HTML. ValidationMessage () también admite un segundo parámetro que permite a los desarrolladores reemplazar el mensaje de texto de error que se muestra:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

El código anterior genera: *&lt;span class = "Field-Validation-error"&gt;\*&lt;/span&gt;* en lugar del texto de error predeterminado cuando hay un error en la propiedad EventDate.

##### <a name="htmlvalidationsummary-helper-method"></a>Método auxiliar HTML. ValidationSummary ()

El método auxiliar HTML. ValidationSummary () se puede usar para representar un mensaje de error de Resumen, acompañado de un &lt;UL&gt;&lt;Li/&gt;&lt;/UL&gt; lista de todos los mensajes de error detallados de la colección ModelState:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

El método auxiliar HTML. ValidationSummary () toma un parámetro de cadena opcional, que define un mensaje de error de resumen para mostrar sobre la lista de errores detallados:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

Opcionalmente, puede usar CSS para reemplazar la apariencia de la lista de errores.

#### <a name="using-a-addruleviolations-helper-method"></a>Usar un método auxiliar AddRuleViolations

Nuestra implementación de edición HTTP-POST inicial usaba una instrucción foreach en el bloque catch para recorrer las infracciones de las reglas del objeto cena y agregarlas a la colección ModelState del controlador:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

Podemos hacer que este código sea un poco limpiador agregando una clase "ControllerHelpers" al proyecto NerdDinner e implementar un método de extensión "AddRuleViolations" en él que agregue un método auxiliar a la clase ASP.NET MVC ModelStateDictionary. Este método de extensión puede encapsular la lógica necesaria para rellenar ModelStateDictionary con una lista de errores de RuleViolation:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

A continuación, podemos actualizar nuestro método de acción de edición HTTP-POST para usar este método de extensión con el fin de rellenar la colección ModelState con nuestras infracciones de las reglas de la cena.

#### <a name="complete-edit-action-method-implementations"></a>Completar implementaciones de método de acción de edición

El código siguiente implementa toda la lógica de controlador necesaria para el escenario de edición:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

Lo mejor de nuestra implementación de edición es que ni nuestra clase de controlador ni nuestra plantilla de vista tiene que saber nada sobre la validación o las reglas de negocios específicas que aplica nuestro modelo de cena. Podemos agregar reglas adicionales a nuestro modelo en el futuro y no es necesario realizar ningún cambio en el código en nuestro controlador o vista para que se admitan. Esto nos proporciona la flexibilidad para desarrollar fácilmente los requisitos de la aplicación en el futuro con un mínimo de cambios en el código.

### <a name="create-support"></a>Crear soporte técnico

Hemos terminado de implementar el comportamiento "Edit" de la clase DinnersController. Ahora vamos a implementar la compatibilidad con "crear" en ella, lo que permitirá a los usuarios agregar nuevas cenas.

#### <a name="the-http-get-create-action-method"></a>El método de acción HTTP-GET Create

Comenzaremos implementando el comportamiento HTTP "GET" de nuestro método de acción Create. Se llamará a este método cuando alguien visite la dirección URL de */Dinners/Create* . Nuestra implementación tiene el siguiente aspecto:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

El código anterior crea un nuevo objeto cena y asigna su propiedad EventDate para que sea una semana en el futuro. Después representa una vista basada en el nuevo objeto cena. Dado que no se ha pasado explícitamente un nombre al método auxiliar *View ()* , se utilizará la Convención predeterminada basada en Convención para resolver la plantilla de vista:/views/Dinners/Create.aspx.

Vamos a crear ahora esta plantilla de vista. Para ello, haga clic con el botón secundario en el método de acción Create y seleccione el comando de menú contextual "agregar vista". En el cuadro de diálogo "agregar vista", indicaremos que vamos a pasar un objeto cena a la plantilla vista y que elegiría aplicar scaffolding automáticamente a una plantilla "crear":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

Cuando hacemos clic en el botón "agregar", Visual Studio guarda una nueva vista "crear. aspx" basada en scaffolding en el directorio "\Views\Dinners" y la abre en el IDE:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

Vamos a realizar algunos cambios en el archivo de scaffolding "crear" predeterminado que se generó para nosotros y modificarlo para que se parezca a lo siguiente:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

Y ahora, cuando ejecutemos la aplicación y accedamos a la dirección URL *"/Dinners/Create"* en el explorador, se representará la interfaz de usuario como la siguiente de nuestra implementación de acción de creación:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>Implementar el método de acción HTTP-POST Create

Tenemos implementada la versión HTTP-GET del método Create Action. Cuando un usuario hace clic en el botón "guardar", realiza un envío de formulario a la dirección URL de */Dinners/Create* y envía los valores de entrada de &lt;HTML&gt; formulario mediante el verbo http post.

Ahora vamos a implementar el comportamiento HTTP POST de nuestro método Create Action. Comenzaremos agregando un método de acción "Create" sobrecargado a nuestro DinnersController que tiene un atributo "AcceptVerbs" en él que indica que se trata de escenarios HTTP POST:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

Hay varias maneras de obtener acceso a los parámetros de formulario publicados en nuestro método "Create" de HTTP-POST habilitado.

Un enfoque consiste en crear un nuevo objeto cena y, a continuación, usar el método auxiliar *UpdateModel ()* (como hicimos con la acción editar) para rellenarlo con los valores de formulario publicados. A continuación, podemos agregarlo a nuestro DinnerRepository, guardarlo en la base de datos y redirigir al usuario a nuestra acción de detalles para mostrar la cena recién creada con el código siguiente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

Como alternativa, se puede usar un enfoque en el que nuestro método de acción Create () tome un objeto cena como parámetro de método. ASP.NET MVC creará automáticamente una instancia de un nuevo objeto cena para nosotros, rellenará sus propiedades con las entradas del formulario y la pasará a nuestro método de acción:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

Nuestro método de acción anterior comprueba que el objeto cena se ha rellenado correctamente con los valores post del formulario mediante la comprobación de la propiedad ModelState. IsValid. Esto devolverá FALSE si hay problemas de conversión de entrada (por ejemplo: una cadena de "fantasma" para la propiedad EventDate) y, si hay algún problema, el método de acción volverá a mostrar el formulario.

Si los valores de entrada son válidos, el método de acción intenta agregar y guardar la nueva cena en el DinnerRepository. Ajusta este trabajo en un bloque try/catch y vuelve a mostrar el formulario si hay alguna infracción de la regla de negocios (lo que haría que el método dinnerRepository. Save () generara una excepción).

Para ver este comportamiento de control de errores en acción, podemos solicitar la dirección URL de */Dinners/Create* y rellenar los detalles sobre una nueva cena. La entrada o los valores incorrectos harán que se vuelva a mostrar el formulario de creación con los errores resaltados como se indica a continuación:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

Observe cómo el formulario de creación está respetando exactamente la misma validación y las mismas reglas de negocios que el formulario de edición. Esto se debe a que nuestras reglas de validación y de negocios se definieron en el modelo y no se insertaron en la interfaz de usuario o el controlador de la aplicación. Esto significa que se pueden cambiar o desarrollar nuestras reglas de validación o de negocio en un único lugar y que se apliquen en toda la aplicación. No tendremos que cambiar ningún código en los métodos de acción editar o crear para respetar automáticamente las nuevas reglas o modificaciones de los existentes.

Cuando corrija los valores de entrada y vuelva a hacer clic en el botón "guardar", la adición a DinnerRepository se realizará correctamente y se agregará una nueva cena a la base de datos. A continuación, se le redirigirá a la dirección URL de */Dinners/details/[id]* , donde se mostrarán los detalles de la cena recién creada:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>Eliminar soporte técnico

Ahora vamos a agregar soporte de "eliminación" a nuestro DinnersController.

#### <a name="the-http-get-delete-action-method"></a>Método de acción de eliminación HTTP-GET

Comenzaremos implementando el comportamiento HTTP GET de nuestro método de acción de eliminación. Se llama a este método cuando alguien visita la dirección URL de */Dinners/delete/[id]* . A continuación se muestra la implementación:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

El método de acción intenta recuperar la cena que se va a eliminar. Si la cena existe, representa una vista basada en el objeto cena. Si el objeto no existe (o ya se ha eliminado), devuelve una vista que representa la plantilla de vista "NotFound" que hemos creado anteriormente para nuestro método de acción "details".

Para crear la plantilla de vista "eliminar", haga clic con el botón derecho en el método de acción eliminar y seleccione el comando de menú contextual "agregar vista". En el cuadro de diálogo "agregar vista" se indicará que vamos a pasar un objeto cena a nuestra plantilla de vista como modelo y a crear una plantilla vacía:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

Cuando hacemos clic en el botón "agregar", Visual Studio agregará un nuevo archivo de plantilla de vista "Delete. aspx" para nosotros en nuestro directorio "\Views\Dinners". Agregaremos código HTML y código a la plantilla para implementar una pantalla de confirmación de eliminación como la siguiente:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

El código anterior muestra el título de la cena que se va a eliminar y genera un formulario &lt;&gt; elemento que realiza una publicación en la dirección URL de/Dinners/Delete/[id] si el usuario final hace clic en el botón "eliminar" dentro de ella.

Cuando se ejecuta la aplicación y se accede a la dirección URL *"/Dinners/delete/[id]"* para un objeto cena válido, se representa la interfaz de usuario como se muestra a continuación:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **Tema secundario: ¿por qué hacemos una publicación?** |
| --- |
| Podría preguntar: ¿por qué se ha desplazado el esfuerzo de crear un &lt;formulario&gt; dentro de la pantalla de confirmación de eliminación? ¿Por qué no usar simplemente un hipervínculo estándar para vincular a un método de acción que realiza la operación de eliminación real? La razón es que queremos tener cuidado a la hora de protegerse frente a los rastreadores web y los motores de búsqueda que detectan nuestras direcciones URL y que provocan que se eliminen accidentalmente los datos al seguir los vínculos. Las direcciones URL basadas en HTTP-GET se consideran "seguras" para que tengan acceso o rastreo, y se supone que no siguen las publicaciones HTTP. Una buena regla es asegurarse de que siempre se colocan operaciones destructivas o de modificación de datos detrás de solicitudes HTTP POST. |

#### <a name="implementing-the-http-post-delete-action-method"></a>Implementar el método de acción de eliminación HTTP-POST

Ahora tenemos la versión HTTP-GET de nuestro método de acción de eliminación implementado, que muestra una pantalla de confirmación de eliminación. Cuando un usuario final hace clic en el botón "eliminar", realizará un envío de formulario a la dirección URL de */Dinners/Dinner/[id]* .

Ahora vamos a implementar el comportamiento HTTP "POST" del método de acción Delete mediante el código siguiente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

La versión HTTP-POST del método de acción de eliminación intenta recuperar el objeto cena que se va a eliminar. Si no lo encuentra (porque ya se ha eliminado), representa nuestra plantilla "NotFound". Si encuentra la cena, la elimina de DinnerRepository. Después representa una plantilla "eliminada".

Para implementar la plantilla "eliminado", hacemos clic con el botón derecho en el método de acción y elegimos el menú contextual "agregar vista". Asignaremos el nombre "eliminado" a nuestra vista y la dejaremos una plantilla vacía (y no tomaremos un objeto de modelo fuertemente tipado). A continuación, le agregaremos contenido HTML:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

Y ahora, cuando ejecutemos la aplicación y accedamos a la dirección URL *"/Dinners/delete/[id]"* para un objeto cena válido, se representará la pantalla de confirmación de la cena:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

Cuando hacemos clic en el botón "eliminar", realizará una POST HTTP en la dirección URL de */Dinners/delete/[id]* , que eliminará la cena de nuestra base de datos y mostrará nuestra plantilla de vista "eliminada":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>Seguridad de enlace de modelos

Hemos analizado dos maneras diferentes de usar las características de enlace de modelos integradas de ASP.NET MVC. La primera usa el método UpdateModel () para actualizar las propiedades de un objeto de modelo existente y la segunda con la compatibilidad de ASP.NET MVC para pasar objetos de modelo en como parámetros de método de acción. Ambas técnicas son muy eficaces y muy útiles.

Esta potencia también aporta responsabilidad de ti. Es importante que siempre se Paranoid sobre la seguridad al aceptar los datos proporcionados por el usuario, y esto también se aplica al enlazar objetos para formar entradas. Tenga cuidado de codificar siempre en HTML los valores especificados por el usuario para evitar ataques por inyección de código HTML y JavaScript, y tenga cuidado con los ataques por inyección de código SQL (Nota: usamos LINQ to SQL para nuestra aplicación, que codifica automáticamente los parámetros para evitarlos tipos de ataques). Nunca debe confiar solo en la validación del lado cliente y usar siempre la validación del lado servidor para protegerse frente a hackers que intentan enviarle valores ficticios.

Un elemento de seguridad adicional para asegurarse de que piensa al usar las características de enlace de ASP.NET MVC es el ámbito de los objetos que está enlazando. En concreto, querrá asegurarse de que comprende las implicaciones de seguridad de las propiedades que se permiten enlazar y asegurarse de que solo se permiten las propiedades que realmente debe actualizar un usuario final para que se actualice.

De forma predeterminada, el método UpdateModel () intentará actualizar todas las propiedades del objeto de modelo que coinciden con los valores de parámetro de formulario de entrada. Del mismo modo, los objetos que se pasan como parámetros de método de acción también pueden tener todas sus propiedades establecidas a través de parámetros de formulario.

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>Bloquear el enlace en función del uso

Puede bloquear la Directiva de enlace según el uso proporcionando una "lista de inclusión" explícita de las propiedades que se pueden actualizar. Esto se puede hacer pasando un parámetro de matriz de cadena adicional al método UpdateModel () como se indica a continuación:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

Los objetos que se pasan como parámetros de método de acción también admiten un atributo [BIND] que permite especificar una "lista de inclusión" de las propiedades permitidas como se indica a continuación:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>Bloquear el enlace de un tipo

También puede bloquear las reglas de enlace por tipo. Esto le permite especificar las reglas de enlace una vez y, a continuación, hacer que se apliquen en todos los escenarios (incluidos los escenarios de parámetros de método de acción y UpdateModel) en todos los controladores y métodos de acción.

Puede personalizar las reglas de enlace por tipo agregando un atributo [BIND] a un tipo o registrándose en el archivo global. asax de la aplicación (útil para escenarios en los que no posee el tipo). Después, puede usar las propiedades de inclusión y exclusión del atributo de enlace para controlar qué propiedades se pueden enlazar para la clase o interfaz determinada.

Usaremos esta técnica para la clase cena en nuestra aplicación NerdDinner y agregaremos un atributo [BIND] que restrinja la lista de propiedades enlazables a lo siguiente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

Tenga en cuenta que no se permite que la colección RSVP se manipule a través de un enlace, ni que se permita establecer las propiedades DinnerID o HostedBy a través del enlace. Por motivos de seguridad, en su lugar, solo manipularemos estas propiedades concretas con código explícito dentro de nuestros métodos de acción.

### <a name="crud-wrap-up"></a>Ajuste CRUD

ASP.NET MVC incluye una serie de características integradas que ayudan a implementar escenarios de publicación de formularios. Usamos una variedad de estas características para proporcionar compatibilidad con la interfaz de usuario CRUD en la parte superior de nuestro DinnerRepository.

Estamos usando un enfoque centrado en el modelo para implementar la aplicación. Esto significa que toda la lógica de validación y de reglas de negocios se define dentro de nuestra capa del modelo, y no dentro de nuestros controladores o vistas. Ni nuestra clase de controlador ni nuestras plantillas de vista saben nada sobre las reglas de negocios específicas que aplica nuestra clase de modelo de cena.

Esto mantendrá la arquitectura de la aplicación limpia y facilitará la prueba. Podemos agregar reglas de negocios adicionales a nuestra capa de modelo en el futuro y *no tener que realizar ningún cambio* en el código en nuestro controlador o vista para que se admitan. Esto nos proporcionará una gran agilidad para evolucionar y cambiar la aplicación en el futuro.

Nuestro DinnersController ahora habilita las listas y los detalles de la cena, así como crear, editar y eliminar soporte técnico. El código completo de la clase se puede encontrar a continuación:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>Paso siguiente

Ahora tenemos la compatibilidad básica CRUD (creación, lectura, actualización y eliminación) implementada dentro de nuestra clase DinnersController.

Veamos ahora cómo se pueden usar las clases ViewData y ViewModel para habilitar una interfaz de usuario aún más enriquecida en nuestros formularios.

> [!div class="step-by-step"]
> [Anterior](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
> [Siguiente](use-viewdata-and-implement-viewmodel-classes.md)
