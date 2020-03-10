---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Usar AJAX para ofrecer actualizaciones dinámicas | Microsoft Docs
author: microsoft
description: El paso 10 implementa la compatibilidad de los usuarios que han iniciado sesión en RSVP su interés en asistir a una cena, mediante un enfoque basado en Ajax integrado en los detalles de la cena...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 3edc02fec546609505b5e085440fa684abe7acd0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486313"
---
# <a name="use-ajax-to-deliver-dynamic-updates"></a>Usar AJAX para entregar actualizaciones dinámicas

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 10 de un [tutorial de aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito en el que se explica cómo crear una aplicación web pequeña, pero completa, con ASP.NET MVC 1.
> 
> El paso 10 implementa la compatibilidad de los usuarios que han iniciado sesión en RSVP su interés en asistir a una cena, mediante un enfoque basado en Ajax integrado en la página de detalles de la cena.
> 
> Si usa ASP.NET MVC 3, se recomienda seguir los tutoriales [de la introducción con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o MVC [Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>NerdDinner paso 10: aceptación de habilitación de AJAX

Vamos a implementar ahora soporte técnico para que los usuarios que han iniciado sesión tengan su interés en asistir a una cena. Lo habilitaremos mediante un enfoque basado en AJAX integrado en la página de detalles de la cena.

### <a name="indicating-whether-the-user-is-rsvpd"></a>Que indica si el usuario es RSVP

Los usuarios pueden visitar la dirección URL de */Dinners/details/[id*] para ver los detalles de una cena determinada:

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

El método de acción Details () se implementa de la siguiente manera:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

Nuestro primer paso para implementar la compatibilidad con RSVP será agregar un método auxiliar "IsUserRegistered (nombre de usuario)" a nuestro objeto cena (dentro de la clase parcial Dinner.cs que hemos creado anteriormente). Este método auxiliar devuelve true o false en función de si el usuario está actualmente RSVP para la cena:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

A continuación, podemos agregar el código siguiente a nuestra plantilla de la vista details. aspx para mostrar un mensaje adecuado que indique si el usuario está registrado o no para el evento:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

Y ahora, cuando un usuario visita una cena registrada para ellos, verá este mensaje:

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

Y cuando visiten una cena que no estén registradas, verán el mensaje siguiente:

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>Implementar el método de acción Register

Vamos a agregar ahora la funcionalidad necesaria para permitir a los usuarios confirmar una cena en la página de detalles.

Para implementarlo, vamos a crear una nueva clase "RSVPController" haciendo clic con el botón derecho en el directorio \Controllers y eligiendo el comando de menú controlador de Add-&gt;.

Implementaremos un método de acción "Register" dentro de la nueva clase RSVPController que toma un identificador para una cena como argumento, recupera el objeto cena adecuado, comprueba si el usuario que ha iniciado sesión está actualmente en la lista de usuarios que se han registrado para él, y si no agrega un objeto RSVP para ellos:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

Observe cómo se devuelve una cadena simple como salida del método de acción. Podríamos haber incrustado este mensaje en una plantilla de vista, pero dado que es tan pequeño, simplemente usaremos el método auxiliar Content () en la clase base del controlador y devolveremos un mensaje de cadena como el anterior.

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>Llamar al método de acción RSVPForEvent mediante AJAX

Usaremos AJAX para invocar el método de acción Register de nuestra vista de detalles. La implementación de esto es bastante fácil. En primer lugar, vamos a agregar dos referencias de la biblioteca de scripts:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

La primera biblioteca hace referencia a la biblioteca de scripts del lado cliente de AJAX de ASP.NET básica. Este archivo tiene aproximadamente 24K de tamaño (comprimido) y contiene la funcionalidad básica de AJAX del lado cliente. La segunda biblioteca contiene funciones de utilidad que se integran con los métodos auxiliares de AJAX integrados de ASP.NET MVC (que usaremos en breve).

A continuación, podemos actualizar el código de la plantilla de vista que agregamos anteriormente para que en lugar de generar un mensaje "no se ha registrado para este evento", se represente un vínculo que, cuando se inserta, realiza una llamada AJAX que invoca nuestro método de acción RSVPForEvent en nuestro controlador de RSVP y RSVP al usuario:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

El método auxiliar Ajax. ActionLink () que se ha usado anteriormente está integrado en ASP.NET MVC y es similar al método auxiliar HTML. ActionLink (), salvo que en lugar de realizar una navegación estándar, realiza una llamada AJAX al método de acción cuando se hace clic en el vínculo. Anteriormente, se llama al método de acción "Register" en el controlador "RSVP" y se pasa DinnerID como el parámetro "ID". El parámetro AjaxOptions final que se pasa indica que queremos tomar el contenido devuelto por el método de acción y actualizar el elemento HTML &lt;div&gt; en la página cuyo identificador es "rsvpmsg".

Y ahora, cuando un usuario navega a una cena en la que no se ha registrado, verá un vínculo a RSVP para ella:

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

Si hace clic en el vínculo "RSVP para este evento", realizará una llamada AJAX al método de acción Register en el controlador RSVP y, cuando se complete, verá un mensaje actualizado como el siguiente:

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

El ancho de banda de red y el tráfico implicados al crear esta llamada AJAX es realmente ligero. Cuando el usuario hace clic en el vínculo "RSVP para este evento", se realiza una pequeña solicitud de red HTTP POST a la dirección URL de */Dinners/Register/1* que tiene el siguiente aspecto en la conexión:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

Y la respuesta del método de acción Register es simplemente:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

Esta llamada ligera es rápida y funcionará incluso a través de una red lenta.

### <a name="adding-a-jquery-animation"></a>Agregar una animación de jQuery

La funcionalidad de AJAX que hemos implementado funciona bien y rápido. En ocasiones, es posible que esto suceda con rapidez, por lo que es posible que un usuario no tenga en cuenta que el vínculo RSVP se ha reemplazado por texto nuevo. Para que el resultado sea un poco más obvio, podemos agregar una animación simple para llamar la atención sobre el mensaje de actualización.

La plantilla de proyecto ASP.NET MVC predeterminada incluye jQuery, una biblioteca JavaScript de código abierto excelente (y muy popular) que también es compatible con Microsoft. jQuery proporciona varias características, entre las que se incluyen una buena biblioteca HTML de selección y efectos de DOM.

Para usar jQuery, primero agregaremos una referencia de script. Como vamos a usar jQuery en una variedad de lugares dentro de nuestro sitio, agregaremos la referencia de script en nuestro archivo de página maestra site. Master para que todas las páginas puedan usarla.

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*Sugerencia: Asegúrese de que ha instalado la revisión de IntelliSense para JavaScript para VS 2008 SP1 que habilita la compatibilidad con IntelliSense más enriquecida para los archivos JavaScript (incluido jQuery). Puede descargarlo desde: http://tinyurl.com/vs2008javascripthotfix*

El código escrito mediante JQuery suele usar un método JavaScript global "$ ()" que recupera uno o varios elementos HTML mediante un selector de CSS. Por ejemplo, *$ ("#rsvpmsg")* selecciona cualquier elemento HTML con el identificador de rsvpmsg, mientras que *$ (". Something")* seleccionaría todos los elementos con el nombre de clase CSS "algo". También puede escribir consultas más avanzadas como "devolver todos los botones de radio seleccionados" mediante una consulta de selector como: *$ ("entrada [@type= radio] [@checked]")* .

Una vez seleccionados los elementos, puede llamar a métodos en ellos para realizar acciones, como ocultarlos: *$ ("#rsvpmsg"). Hide ();*

En nuestro escenario de RSVP, vamos a definir una sencilla función de JavaScript denominada "AnimateRSVPMessage" que selecciona el&gt; "rsvpmsg" &lt;div y anima el tamaño del contenido de texto. El código siguiente inicia el texto pequeño y, a continuación, hace que aumente durante un período de 400 milisegundos:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

A continuación, podemos conectar esta función de JavaScript para que se llame después de que la llamada AJAX se complete correctamente pasando su nombre a nuestro método auxiliar de Ajax. ActionLink () (a través de la propiedad de evento AjaxOptions "Success"):

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

Y ahora, cuando se hace clic en el vínculo "RSVP para este evento" y nuestra llamada AJAX se completa correctamente, el mensaje de contenido que se devuelve se animará y aumentará de tamaño:

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

Además de proporcionar un evento "Success", el objeto AjaxOptions expone eventos de inicio, de error y de finalización que puede controlar (junto con otras propiedades y opciones útiles).

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>Limpieza: refactorizar una vista parcial de RSVP

Nuestra plantilla de vista de detalles empieza a ser muy larga, lo que lo hará un poco más difícil de entender. Para ayudar a mejorar la legibilidad del código, vamos a crear una vista parcial (RSVPStatus. ascx) que encapsula todo el código de vista de RSVP de nuestra página de detalles.

Para ello, haga clic con el botón derecho en la carpeta \Views\Dinners y, a continuación, elija el comando de menú Ver&gt;. Tendremos que tomar un objeto cena como ViewModel fuertemente tipado. A continuación, se puede copiar y pegar el contenido RSVP de la vista details. aspx.

Una vez hecho esto, vamos a crear otra vista parcial: EditAndDeleteLinks. ascx, que encapsula el código de la vista de vínculos de edición y eliminación. También tendremos que tomar un objeto cena como ViewModel fuertemente tipado y copiar y pegar la lógica de edición y eliminación de la vista details. aspx.

La plantilla de vista de detalles puede simplemente incluir dos llamadas al método html. RenderPartial () en la parte inferior:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

Esto hace que el limpiador de código se lea y se mantenga.

### <a name="next-step"></a>Paso siguiente

Ahora veamos cómo podemos usar AJAX aún más y agregar compatibilidad de asignación interactiva a nuestra aplicación.

> [!div class="step-by-step"]
> [Anterior](secure-applications-using-authentication-and-authorization.md)
> [Siguiente](use-ajax-to-implement-mapping-scenarios.md)
