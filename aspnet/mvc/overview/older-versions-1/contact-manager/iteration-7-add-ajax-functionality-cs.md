---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-cs
title: '#7 de iteración: agregar funcionalidadC#de Ajax () | Microsoft Docs'
author: microsoft
description: En la séptima iteración, se mejora la capacidad de respuesta y el rendimiento de la aplicación mediante la adición de compatibilidad con Ajax.
ms.author: riande
ms.date: 02/20/2009
ms.assetid: f1b0809e-8909-444e-b6bb-a5cd1dea3f72
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-cs
msc.type: authoredcontent
ms.openlocfilehash: 7c8eb3d3688674dd2c220b4bd1b5982f2610d0eb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437737"
---
# <a name="iteration-7--add-ajax-functionality-c"></a>#7 de iteración: agregar funcionalidadC#de Ajax ()

por [Microsoft](https://github.com/microsoft)

[Descargar código](iteration-7-add-ajax-functionality-cs/_static/contactmanager_7_cs1.zip)

> En la séptima iteración, se mejora la capacidad de respuesta y el rendimiento de la aplicación mediante la adición de compatibilidad con Ajax.

## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Creación de una aplicación ASP.NET MVC de administraciónC#de contactos ()

En esta serie de tutoriales, creamos una aplicación de administración de contactos completa de principio a fin. La aplicación Contact Manager le permite almacenar información de contacto: nombres, números de teléfono y direcciones de correo electrónico, para obtener una lista de personas.

Compilamos la aplicación en varias iteraciones. Con cada iteración, mejoramos gradualmente la aplicación. El objetivo de este enfoque de varias iteraciones es permitirle comprender la razón de cada cambio.

- #1 de iteración: cree la aplicación. En la primera iteración, se crea el administrador de contactos de la manera más sencilla posible. Agregamos compatibilidad con las operaciones básicas de base de datos: crear, leer, actualizar y eliminar (CRUD).

- #2 de iteración: haga que la aplicación tenga un buen aspecto. En esta iteración, mejoramos la apariencia de la aplicación modificando la página maestra de vista de MVC predeterminada de ASP.NET y la hoja de estilos en cascada.

- #3 de iteración: agregar validación de formulario. En la tercera iteración, agregamos la validación de formulario básica. Impedimos que los usuarios envíen un formulario sin completar los campos de formulario necesarios. También validamos las direcciones de correo electrónico y los números de teléfono.

- #4 de iteración: hacer que la aplicación esté acoplada de forma flexible. En esta cuarta iteración, se aprovechan varios patrones de diseño de software para facilitar el mantenimiento y la modificación de la aplicación Contact Manager. Por ejemplo, se refactoriza nuestra aplicación para usar el patrón de repositorio y el patrón de inserción de dependencias.

- #5 de iteración: crear pruebas unitarias. En la quinta iteración, se facilita el mantenimiento y la modificación de la aplicación mediante la adición de pruebas unitarias. Hemos simulado nuestras clases de modelo de datos y compilamos pruebas unitarias para nuestros controladores y la lógica de validación.

- #6 de iteración: Use el desarrollo controlado por pruebas. En esta sexta iteración, agregamos una nueva funcionalidad a nuestra aplicación escribiendo primero las pruebas unitarias y escribiendo código en las pruebas unitarias. En esta iteración, se agregan grupos de contactos.

- #7 de iteración: agregar funcionalidad de Ajax. En la séptima iteración, se mejora la capacidad de respuesta y el rendimiento de la aplicación mediante la adición de compatibilidad con Ajax.

## <a name="this-iteration"></a>Esta iteración

En esta iteración de la aplicación Contact Manager, se refactoriza nuestra aplicación para hacer uso de Ajax. Al aprovechar las ventajas de Ajax, hacemos que nuestra aplicación tenga mayor capacidad de respuesta. Se puede evitar la representación de una página completa cuando se necesita actualizar solo una región determinada en una página.

Refactorizaremos nuestra vista de índice para que no sea necesario volver a mostrar toda la página cada vez que alguien Seleccione un nuevo grupo de contactos. En su lugar, cuando alguien haga clic en un grupo de contactos, solo actualizaremos la lista de contactos y dejaremos el resto de la página.

También cambiaremos el modo en que funciona el vínculo de eliminación. En lugar de mostrar una página de confirmación independiente, se mostrará un cuadro de diálogo de confirmación de JavaScript. Si confirma que desea eliminar un contacto, se realiza una operación de eliminación HTTP en el servidor para eliminar el registro de contacto de la base de datos.

Además, usaremos jQuery para agregar efectos de animación a nuestra vista de índice. Mostraremos una animación cuando se Capture la nueva lista de contactos desde el servidor.

Por último, sacaremos provecho de la compatibilidad con el marco de ASP.NET AJAX para administrar el historial del explorador. Vamos a crear puntos de historial cada vez que realicemos una llamada AJAX para actualizar la lista de contactos. De este modo, los botones atrás y adelante del explorador funcionarán.

## <a name="why-use-ajax"></a>¿Por qué usar Ajax?

El uso de Ajax tiene muchas ventajas. En primer lugar, agregar la funcionalidad Ajax a una aplicación produce una mejor experiencia de usuario. En una aplicación web normal, la página completa debe volver a enviarse al servidor cada vez que un usuario realiza una acción. Siempre que se realiza una acción, el explorador se bloquea y el usuario debe esperar hasta que se recupere y se vuelva a mostrar toda la página.

Esto sería una experiencia inaceptable en el caso de una aplicación de escritorio. Pero tradicionalmente, hemos vivido esta experiencia de usuario incorrecta en el caso de una aplicación web porque no sabíamos que podríamos hacer todo mejor. Pensamos que se trata de una limitación de aplicaciones web cuando, en realidad, era simplemente una limitación de nuestras imaginacións.

En una aplicación Ajax, no es necesario que la experiencia del usuario se detenga solo para actualizar una página. En su lugar, puede realizar una solicitud asincrónica en segundo plano para actualizar la página. No se obliga al usuario a esperar mientras parte de la página se actualiza.

Al aprovechar las ventajas de Ajax, también puede mejorar el rendimiento de la aplicación. Tenga en cuenta el funcionamiento de la aplicación Contact Manager ahora sin la funcionalidad Ajax. Al hacer clic en un grupo de contactos, se debe volver a mostrar toda la vista de índice. La lista de contactos y la lista de grupos de contactos se deben recuperar del servidor de bases de datos. Todos estos datos se deben pasar a través de la conexión desde el servidor Web al explorador Web.

Sin embargo, después de agregar la funcionalidad Ajax a nuestra aplicación, podemos evitar volver a mostrar toda la página cuando un usuario hace clic en un grupo de contactos. Ya no es necesario captar los grupos de contactos de la base de datos. También es necesario enviar la vista de índice completa a través de la conexión. Al aprovechar las ventajas de Ajax, se reduce la cantidad de trabajo que el servidor de base de datos debe realizar y se reduce la cantidad de tráfico de red requerido por la aplicación.

## <a name="don-t-be-afraid-of-ajax"></a>No dude en usar Ajax

Algunos desarrolladores evitan usar Ajax porque se preocupan por los exploradores de nivel inferior. Quieren asegurarse de que sus aplicaciones web seguirán funcionando cuando accedan a un explorador que no admita JavaScript. Dado que Ajax depende de JavaScript, algunos desarrolladores evitan el uso de Ajax.

Sin embargo, si tiene cuidado con la implementación de Ajax, puede compilar aplicaciones que funcionen con exploradores de nivel superior y de nivel inferior. Nuestra aplicación de administrador de contactos funcionará con los exploradores que admiten JavaScript y exploradores que no lo hacen.

Si usa la aplicación Contact Manager con un explorador compatible con JavaScript, tendrá una mejor experiencia de usuario. Por ejemplo, al hacer clic en un grupo de contactos, solo se actualizará la región de la página que muestra los contactos.

Por otro lado, si usa la aplicación Contact Manager con un explorador que no admite JavaScript (o que tiene JavaScript deshabilitado), tendrá una experiencia de usuario ligeramente menos deseada. Por ejemplo, al hacer clic en un grupo de contactos, toda la vista de índice debe volver a enviarse al explorador para mostrar la lista de contactos correspondiente.

## <a name="adding-the-required-javascript-files"></a>Agregar los archivos JavaScript necesarios

Tendremos que usar tres archivos JavaScript para agregar la funcionalidad de Ajax a nuestra aplicación. Los tres archivos se incluyen en la carpeta scripts de una nueva aplicación ASP.NET MVC.

Si tiene previsto usar Ajax en varias páginas de la aplicación, tiene sentido incluir los archivos JavaScript necesarios en la página maestra de vista de la aplicación. De este modo, los archivos JavaScript se incluirán automáticamente en todas las páginas de la aplicación.

Agregue el siguiente código JavaScript incluido dentro de la etiqueta &lt;Head&gt; de la página maestra de vista:

[!code-html[Main](iteration-7-add-ajax-functionality-cs/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a>Refactorizar la vista de índice para usar Ajax

Permita que se inicie mediante la modificación de nuestra vista de índice para que al hacer clic en un grupo de contactos solo se actualice la región de la vista que muestra los contactos. El cuadro rojo de la figura 1 contiene la región que desea actualizar.

[![solo actualizar contactos](iteration-7-add-ajax-functionality-cs/_static/image1.jpg)](iteration-7-add-ajax-functionality-cs/_static/image1.png)

**Figura 01**: actualización solo de contactos ([haga clic para ver la imagen de tamaño completo](iteration-7-add-ajax-functionality-cs/_static/image2.png))

El primer paso consiste en separar la parte de la vista que se desea actualizar de forma asincrónica en una independiente (ver control de usuario). La sección de la vista de índice que muestra la tabla de contactos se ha pasado a la parte de la lista 1.

**Lista 1-Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample2.aspx)]

Observe que el parcial de la lista 1 tiene un modelo diferente al de la vista de índice. El atributo *Inherits* de la Directiva &lt;% @ Page%&gt; especifica que el parcial hereda de la clase&gt; del grupo ViewUserControl&lt;.

La vista de índice actualizada está incluida en la lista 2.

**Lista 2-Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample3.aspx)]

Hay dos cosas que debe tener en cuenta sobre la vista actualizada en la lista 2. En primer lugar, tenga en cuenta que todo el contenido que se mueve a la parcial se reemplaza por una llamada a HTML. RenderPartial (). Se llama al método html. RenderPartial () cuando se solicita la vista de índice por primera vez para mostrar el conjunto inicial de contactos.

En segundo lugar, observe que el HTML. ActionLink () que se usa para mostrar los grupos de contactos se ha reemplazado por un Ajax. ActionLink (). Se llama a Ajax. ActionLink () con los parámetros siguientes:

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample4.aspx)]

El primer parámetro representa el texto que se va a mostrar para el vínculo, el segundo parámetro representa los valores de ruta y el tercer parámetro representa las opciones de Ajax. En este caso, usamos la opción UpdateTargetId Ajax para apuntar a la etiqueta HTML &lt;div&gt; que deseamos actualizar después de que se complete la solicitud de Ajax. Queremos actualizar la etiqueta &lt;div&gt; con la nueva lista de contactos.

El método index () actualizado del controlador de contactos está incluido en la lista 3.

**Lista 3-Controllers\ContactController.cs (método de índice)**

[!code-csharp[Main](iteration-7-add-ajax-functionality-cs/samples/sample5.cs)]

La acción de índice () actualizada devuelve condicionalmente una de dos cosas. Si una solicitud de Ajax invoca la acción index (), el controlador devuelve una parcial. De lo contrario, la acción index () devuelve una vista completa.

Tenga en cuenta que la acción index () no necesita devolver tantos datos cuando los invoca una solicitud Ajax. En el contexto de una solicitud normal, la acción de índice devuelve una lista de todos los grupos de contactos y el grupo de contactos seleccionado. En el contexto de una solicitud Ajax, la acción index () solo devuelve el grupo seleccionado. Ajax significa menos trabajo en el servidor de base de datos.

Nuestra vista de índice modificada funciona en el caso de los exploradores de nivel superior y de nivel inferior. Si hace clic en un grupo de contactos y el explorador admite JavaScript, solo se actualizará la región de la vista que contiene la lista de contactos. Si, por otro lado, el explorador no es compatible con JavaScript, se actualiza toda la vista.

Nuestra vista de índice actualizada tiene un problema. Al hacer clic en un grupo de contactos, no se resalta el grupo seleccionado. Dado que la lista de grupos se muestra fuera de la región que se actualiza durante una solicitud Ajax, el grupo correcto no se resalta. Corregiremos este problema en la sección siguiente.

## <a name="adding-jquery-animation-effects"></a>Agregar efectos de animación de jQuery

Normalmente, al hacer clic en un vínculo de una página web, puede usar la barra de progreso del explorador para detectar si el explorador está capturando el contenido actualizado de forma activa o no. Al realizar una solicitud Ajax, por otro lado, la barra de progreso del explorador no muestra ningún progreso. Esto puede hacer que los usuarios sean nerviosos. ¿Cómo se puede saber si el explorador está inmovilizado?

Hay varias maneras de indicar a un usuario que se está realizando un trabajo mientras realiza una solicitud Ajax. Un enfoque consiste en mostrar una animación simple. Por ejemplo, puede atenuar una región cuando una solicitud Ajax comienza y atenúa la región cuando se completa la solicitud.

Usaremos la biblioteca jQuery que se incluye con el marco de Microsoft ASP.NET MVC para crear los efectos de animación. La vista de índice actualizada está incluida en la lista 4.

**Lista 4: Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample6.aspx)]

Observe que la vista de índice actualizada contiene tres nuevas funciones de JavaScript. Las dos primeras funciones usan jQuery para atenuar y atenuar la lista de contactos al hacer clic en un nuevo grupo de contactos. La tercera función muestra un mensaje de error cuando una solicitud Ajax produce un error (por ejemplo, tiempo de espera de red).

La primera función también se encarga de resaltar el grupo seleccionado. Se agrega un atributo class = Selected al elemento primario (el elemento LI) del elemento en el que se hace clic. De nuevo, jQuery facilita la selección del elemento correcto y la adición de la clase CSS.

Estos scripts están vinculados a los vínculos de grupo con la ayuda del parámetro AjaxOptions de Ajax. ActionLink (). La llamada al método de Ajax. ActionLink () actualizada tiene el siguiente aspecto:

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a>Agregar compatibilidad con el historial del explorador

Normalmente, al hacer clic en un vínculo para actualizar una página, se actualiza el historial del explorador. De este modo, puede hacer clic en el botón atrás del explorador para volver al estado anterior de la página. Por ejemplo, si hace clic en el grupo de contactos de amigos y, a continuación, hace clic en el grupo de contactos de negocios, puede hacer clic en el botón atrás del explorador para volver al estado de la página cuando se ha seleccionado el grupo de contactos de amigos.

Desafortunadamente, realizar una solicitud Ajax no actualiza automáticamente el historial del explorador. Si hace clic en un grupo de contactos y se recupera la lista de contactos coincidentes con una solicitud Ajax, no se actualizará el historial del explorador. No puede usar el botón atrás del explorador para volver a un grupo de contactos después de seleccionar un nuevo grupo de contactos.

Si desea que los usuarios puedan usar el botón atrás del explorador después de realizar solicitudes Ajax, debe realizar un poco más de trabajo. Debe aprovechar la funcionalidad de administración del historial del explorador integrada en el marco de ASP.NET AJAX.

ASP.NET el historial del explorador AJAX, debe hacer tres cosas:

1. Habilite el historial del explorador estableciendo la propiedad enableBrowserHistory en true.
2. Guarde los puntos del historial cuando el estado de una vista cambie llamando al método addHistoryPoint ().
3. Reconstruir el estado de la vista cuando se genera el evento Navigate.

La vista de índice actualizada está incluida en la lista 5.

**Lista 5-Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample8.aspx)]

En la lista 5, el historial del explorador está habilitado en la función pageInit (). La función pageInit () también se usa para configurar el controlador de eventos para el evento Navigate. El evento Navigate se desencadena cuando el botón reenviar o atrás del explorador hace que cambie el estado de la página.

Se llama al método beginContactList () al hacer clic en un grupo de contactos. Este método crea un nuevo punto del historial llamando al método addHistoryPoint (). El identificador del grupo de contactos en el que hizo clic se agrega al historial.

El identificador de grupo se recupera de un atributo expando en el vínculo del grupo de contactos. El vínculo se representa con la siguiente llamada a Ajax. ActionLink ().

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample9.aspx)]

El último parámetro pasado a Ajax. ActionLink () agrega un atributo expando denominado GROUPID al vínculo (en minúsculas para la compatibilidad con XHTML).

Cuando un usuario presiona el botón atrás o adelante del explorador, se genera el evento Navigate y se llama al método Navigate (). Este método actualiza los contactos mostrados en la página para que coincidan con el estado de la página que corresponde al punto de historial del explorador que se ha pasado al método Navigate.

## <a name="performing-ajax-deletes"></a>Realización de eliminaciones de Ajax

Actualmente, para eliminar un contacto, debe hacer clic en el vínculo eliminar y, a continuación, hacer clic en el botón eliminar que se muestra en la página de confirmación de eliminación (consulte la figura 2). Esto parece una gran cantidad de solicitudes de página para hacer algo tan sencillo como eliminar un registro de base de datos.

[![la página de confirmación de eliminación](iteration-7-add-ajax-functionality-cs/_static/image2.jpg)](iteration-7-add-ajax-functionality-cs/_static/image3.png)

**Figura 02**: Página de confirmación de eliminación ([haga clic para ver la imagen de tamaño completo](iteration-7-add-ajax-functionality-cs/_static/image4.png))

Es tentador omitir la página de confirmación de eliminación y eliminar un contacto directamente de la vista de índice. Debe evitar esta tentación porque si toma este enfoque, la aplicación se abrirá en vulnerabilidades de seguridad. En general, no quiere realizar una operación GET de HTTP al invocar una acción que modifica el estado de la aplicación Web. Al realizar una eliminación, desea realizar una operación HTTP POST o una operación de eliminación HTTP.

El vínculo de eliminación se encuentra en el ContactList parcial. En la enumeración 6 se incluye una versión actualizada de ContactList Partial.

**Listado 6-Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample10.aspx)]

El vínculo de eliminación se representa con la siguiente llamada al método Ajax. ImageActionLink ():

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample11.aspx)]

> [!NOTE] 
> 
> Ajax. ImageActionLink () no es una parte estándar del marco ASP.NET MVC. Ajax. ImageActionLink () es un método auxiliar personalizado incluido en el proyecto de Contact Manager.

El parámetro AjaxOptions tiene dos propiedades. En primer lugar, se usa la propiedad CONFIRM para mostrar un cuadro de diálogo emergente de confirmación de JavaScript. En segundo lugar, la propiedad HttpMethod se utiliza para realizar una operación de eliminación HTTP.

La lista 7 contiene una nueva acción AjaxDelete () que se ha agregado al controlador de contactos.

**Listado 7-Controllers\ContactController.cs (AjaxDelete)**

[!code-csharp[Main](iteration-7-add-ajax-functionality-cs/samples/sample12.cs)]

La acción AjaxDelete () se decora con un atributo AcceptVerbs. Este atributo evita que se invoque la acción excepto por cualquier operación HTTP que no sea una operación de eliminación HTTP. En concreto, no puede invocar esta acción con un HTTP GET.

Después de eliminar el registro de base de datos, debe mostrar la lista actualizada de contactos que no contiene el registro eliminado. El método AjaxDelete () devuelve el ContactList parcial y la lista actualizada de contactos.

## <a name="summary"></a>Resumen

En esta iteración, se ha agregado la funcionalidad Ajax a nuestra aplicación Contact Manager. Hemos usado Ajax para mejorar la capacidad de respuesta y el rendimiento de la aplicación.

En primer lugar, se refactoriza la vista de índice para que al hacer clic en un grupo de contactos no se actualice toda la vista. En su lugar, al hacer clic en un grupo de contactos solo se actualiza la lista de contactos.

A continuación, usamos efectos de animación de jQuery para atenuar y atenuar la lista de contactos. La adición de animaciones a una aplicación Ajax se puede usar para proporcionar a los usuarios de la aplicación el equivalente de una barra de progreso del explorador.

También hemos agregado compatibilidad con el historial del explorador a nuestra aplicación Ajax. Hemos permitido a los usuarios hacer clic en los botones atrás y adelante del explorador para cambiar el estado de la vista de índice.

Por último, creamos un vínculo de eliminación que admite operaciones de eliminación HTTP. Mediante la realización de operaciones de eliminación de Ajax, se permite a los usuarios eliminar registros de la base de datos sin necesidad de que el usuario solicite una página de confirmación de eliminación adicional.

> [!div class="step-by-step"]
> [Anterior](iteration-6-use-test-driven-development-cs.md)
> [Siguiente](iteration-1-create-the-application-vb.md)
