---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: Usar AJAX para implementar escenarios de asignación | Microsoft Docs
author: microsoft
description: En el paso 11 se muestra cómo integrar la compatibilidad con la asignación de AJAX en nuestra aplicación de NerdDinner, lo que permite a los usuarios que crean, editan o ven las cenas ver la l...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 7fc90f978b9f9eca511feca70a3c0d02ec69b940
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78468541"
---
# <a name="use-ajax-to-implement-mapping-scenarios"></a>Usar AJAX para implementar escenarios de asignación

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 11 de un [tutorial de aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito en el que se explica cómo crear una aplicación web pequeña, pero completa, con ASP.NET MVC 1.
> 
> En el paso 11 se muestra cómo integrar la compatibilidad con la asignación de AJAX en nuestra aplicación NerdDinner, lo que permite a los usuarios que crean, editan o ven las cenas ver la ubicación de la cena gráficamente.
> 
> Si usa ASP.NET MVC 3, se recomienda seguir los tutoriales [de la introducción con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o MVC [Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>NerdDinner paso 11: integrar un mapa AJAX

Ahora haremos que nuestra aplicación sea un poco más emocionante visualmente integrando la compatibilidad con la asignación de AJAX. Esto permitirá a los usuarios que crean, editan o ven las cenas ver la ubicación de la cena gráficamente.

### <a name="creating-a-map-partial-view"></a>Crear una vista parcial de mapa

Vamos a usar la funcionalidad de asignación en varios lugares de la aplicación. Para mantener el código seco, encapsularemos la funcionalidad de asignación común en una única plantilla parcial que se puede volver a usar en varias vistas y acciones de controlador. Asignaremos el nombre "map. ascx" a la vista parcial y lo crearemos en el directorio \Views\Dinners

Para crear la parcial map. ascx, haga clic con el botón derecho en el directorio \Views\Dinners y elija el comando de menú Ver&gt;. Asignaremos un nombre a la vista "map. ascx", la comprobaremos como una vista parcial e indicar que vamos a pasarle una clase de modelo "cena" fuertemente tipada:

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

Cuando hacemos clic en el botón "agregar", se creará nuestra plantilla parcial. A continuación, actualizaremos el archivo map. ascx para que tenga el siguiente contenido:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

La primera &lt;script&gt; referencia apunta a la biblioteca de asignación de Microsoft Virtual Earth 6,2. El segundo script &lt;&gt; Reference apunta a un archivo map. js que se creará en breve, que encapsulará nuestra lógica de asignación de JavaScript común. El elemento &lt;div id = "theMap"&gt; es el contenedor HTML que Virtual Earth usará para hospedar la asignación.

Después, tenemos un script de &lt;incrustado&gt; bloque que contiene dos funciones de JavaScript específicas de esta vista. La primera función usa jQuery para conectar una función que se ejecuta cuando la página está lista para ejecutar el script del lado cliente. Llama a una función auxiliar LoadMap () que definiremos en el archivo de script map. js para cargar el control de mapa Virtual Earth. La segunda función es un controlador de eventos de devolución de llamada que agrega un PIN a la asignación que identifica una ubicación.

Observe cómo usamos un bloque &lt;% =%&gt; en el lado del servidor en el bloque de scripts del lado cliente para insertar la latitud y la longitud de la cena que queremos asignar al código JavaScript. Se trata de una técnica útil para generar valores dinámicos que puede usar el script del lado cliente (sin requerir una llamada AJAX independiente al servidor para recuperar los valores, lo que hace que sea más rápido). Los bloques &lt;% =%&gt; se ejecutarán cuando la vista se represente en el servidor, por lo que la salida del código HTML acabará con valores de JavaScript incrustados (por ejemplo: var latitud = 47,64312;).

### <a name="creating-a-mapjs-utility-library"></a>Creación de una biblioteca de utilidades de MAP. js

Vamos a crear ahora el archivo map. js que se puede usar para encapsular la funcionalidad de JavaScript para nuestra asignación (e implementar los métodos LoadMap y LoadPin anteriores). Para ello, haga clic con el botón derecho en el directorio \Scripts del proyecto y, a continuación, elija el comando de menú "agregar&gt;nuevo elemento", seleccione el elemento JScript y asígnele el nombre "map. js".

A continuación se muestra el código de JavaScript que agregaremos al archivo map. js que interactuará con Virtual Earth para mostrar el mapa y agregarle los pin de las oficinas para nuestras cenas:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>Integrar el mapa con crear y editar formularios

Ahora integraremos la compatibilidad con los mapas con nuestros escenarios de creación y edición existentes. La buena noticia es que esto es bastante fácil de hacer y no requiere que se cambie el código del controlador. Dado que las vistas de creación y edición comparten una vista parcial común "DinnerForm" para implementar la interfaz de usuario del formulario de cena, podemos agregar el mapa en un solo lugar y hacer que ambos escenarios de creación y edición lo usen.

Lo único que debemos hacer es abrir la vista parcial de \Views\Dinners\DinnerForm.ascx y actualizarla para incluir la nueva asignación parcial. A continuación se muestra el aspecto que tendrá el DinnerForm actualizado una vez agregado el mapa (Nota: los elementos de formulario HTML se omiten en el siguiente fragmento de código por motivos de brevedad):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

El DinnerForm parcial anterior toma un objeto de tipo "DinnerFormViewModel" como su tipo de modelo (porque necesita un objeto cena, así como un SelectList para rellenar el DropDownList de países). Nuestro mapa parcial solo necesita un objeto de tipo "cena" como tipo de modelo, por lo que, cuando se represente la asignación parcial, pasamos solo la subpropiedad cena de DinnerFormViewModel a ella:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

La función de JavaScript que hemos agregado a la parcial usa jQuery para adjuntar un evento "Blur" al cuadro de texto HTML "address". Probablemente ha oído hablar de los eventos de "enfoque" que se activan cuando un usuario hace clic en un cuadro de texto o las pestañas. Lo contrario es un evento de "Desenfoque" que se activa cuando un usuario sale de un cuadro de texto. El controlador de eventos anterior borra los valores de los cuadros de texto latitud y longitud cuando esto sucede y, a continuación, traza la nueva ubicación de la dirección en nuestro mapa. Un controlador de eventos de devolución de llamada que se haya definido en el archivo map. js actualizará los cuadros de texto longitud y latitud de nuestro formulario usando los valores devueltos por Virtual Earth en función de la dirección que se le haya proporcionado.

Y ahora, cuando ejecutemos de nuevo la aplicación y haga clic en la pestaña "cena de anfitrión", veremos que se muestra un mapa predeterminado junto con nuestros elementos de formulario de cena estándar:

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

Al escribir una dirección y, a continuación, la pestaña, el mapa se actualizará dinámicamente para mostrar la ubicación y el controlador de eventos rellenará los cuadros de texto latitud/longitud con los valores de Ubicación:

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

Si se guarda la nueva cena y luego se vuelve a abrir para editarla, veremos que la ubicación de la asignación se muestra cuando se carga la página:

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

Cada vez que se cambia el campo de dirección, se actualizan el mapa y las coordenadas de latitud y longitud.

Ahora que el mapa muestra la ubicación de la cena, también se pueden cambiar los campos de formulario de latitud y longitud desde cuadros de texto visibles a elementos ocultos (ya que el mapa se actualiza automáticamente cada vez que se escribe una dirección). Para ello, pasaremos de usar la aplicación auxiliar HTML. TextBox () de HTML a usar el método auxiliar HTML. Hidden ():

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

Y ahora nuestros formularios son un poco más fáciles de usar y evitan Mostrar la latitud o la longitud sin procesar (mientras se siguen almacenando con cada cena en la base de datos):

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>Integrar el mapa con la vista de detalles

Ahora que tenemos el mapa integrado con nuestros escenarios de creación y edición, vamos a integrarlo también con nuestro escenario de detalles. Lo único que debemos hacer es llamar a &lt;% HTML. RenderPartial ("map"); %&gt; dentro de la vista de detalles.

A continuación se muestra el aspecto del código fuente de la vista de detalles completa (con la integración de mapas):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

Y ahora, cuando un usuario navega a una dirección URL de/Dinners/Details/[id], verá los detalles sobre la cena, la ubicación de la cena en el mapa (completa con un PIN que, cuando se mantiene el puntero sobre él, muestra el título de la cena y la dirección del mismo) y tiene un vínculo de AJAX a RSVP para ella:

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>Implementación de la búsqueda de ubicación en nuestra base de datos y repositorio

Para finalizar la implementación de AJAX, vamos a agregar un mapa a la Página principal de la aplicación que permite a los usuarios buscar cenas cerca de ellos.

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

Comenzaremos implementando la compatibilidad dentro de nuestra base de datos y el nivel de repositorio de datos para realizar de forma eficaz una búsqueda de radio basada en la ubicación de cenas. Podríamos usar las nuevas [características geoespaciales de SQL 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx) para implementar esto, o como alternativa, podemos usar un enfoque de función de SQL que Gary Dryden describe en el artículo siguiente: [http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx) y Rob cónica escrito sobre el uso de con LINQ to SQL aquí: [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

Para implementar esta técnica, abriremos el "Explorador de servidores" dentro de Visual Studio, seleccionaremos la base de datos NerdDinner y, a continuación, hacer clic con el botón derecho en el subnodo "funciones" bajo él y elegir crear una nueva "función escalar":

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

A continuación, pegaremos en la siguiente función DistanceBetween:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

A continuación, crearemos una nueva función con valores de tabla en SQL Server que llamaremos "NearestDinners":

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

Esta función de tabla "NearestDinners" usa la función auxiliar DistanceBetween para devolver todas las cenas en las 100 millas de la latitud y la longitud que se proporcionan:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

Para llamar a esta función, primero abriremos el diseñador de LINQ to SQL haciendo doble clic en el archivo NerdDinner. dbml en nuestro directorio \Models:

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

A continuación, arrastraremos las funciones NearestDinners y DistanceBetween al diseñador de LINQ to SQL, lo que hará que se agreguen como métodos en nuestra LINQ to SQL clase NerdDinnerDataContext:

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

A continuación, podemos exponer un método de consulta "FindByLocation" en la clase DinnerRepository que usa la función NearestDinner para devolver las próximas cenas que están dentro de 100 millas de la ubicación especificada:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>Implementar un método de acción de búsqueda AJAX basado en JSON

Ahora implementaremos un método de acción de controlador que aprovecha el nuevo método de repositorio FindByLocation () para devolver una lista de datos de cenas que se pueden usar para rellenar un mapa. Este método de acción devolverá los datos de la cena en formato JSON (notación de objetos JavaScript) para que se pueda manipular fácilmente mediante JavaScript en el cliente.

Para implementarlo, vamos a crear una nueva clase "SearchController" haciendo clic con el botón derecho en el directorio \Controllers y eligiendo el comando de menú controlador de Add-&gt;. A continuación, implementaremos un método de acción "SearchByLocation" dentro de la nueva clase SearchController como se indica a continuación:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

El método de acción SearchByLocation de SearchController llama internamente al método FindByLocation en DinnerRepository para obtener una lista de las cenas cercanas. Sin embargo, en lugar de devolver los objetos cena directamente al cliente, en su lugar devuelve objetos JsonDinner. La clase JsonDinner expone un subconjunto de propiedades de cenas (por ejemplo, por motivos de seguridad, no revela los nombres de las personas que tienen RSVP para una cena). También incluye una propiedad RSVPCount que no existe en cena, y que se calcula dinámicamente contando el número de objetos RSVP asociados a una cena determinada.

A continuación, se usa el método auxiliar JSON () en la clase base del controlador para devolver la secuencia de cenas con un formato de conexión basado en JSON. JSON es un formato de texto estándar para representar estructuras de datos simples. A continuación se muestra un ejemplo de la apariencia de una lista con formato JSON de dos objetos JsonDinner cuando se devuelve desde nuestro método de acción:

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>Llamar al método AJAX basado en JSON mediante jQuery

Ahora estamos listos para actualizar la Página principal de la aplicación NerdDinner para que use el método de acción SearchByLocation de SearchController. Para ello, abriremos la plantilla de vista/Views/Home/Index.aspx y la actualizaremos para que tenga un cuadro de texto, un botón buscar, nuestro mapa y un elemento &lt;div&gt; denominado dinnerList:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

A continuación, podemos agregar dos funciones de JavaScript a la página:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

La primera función de JavaScript carga la asignación cuando la página se carga por primera vez. La segunda función de JavaScript conecta un controlador de eventos Click de JavaScript en el botón Buscar. Cuando se presiona el botón, se llama a la función JavaScript FindDinnersGivenLocation () que se va a agregar al archivo map. js:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

Esta función FindDinnersGivenLocation () llama a Map. Busque () en el control Virtual Earth para centrarlo en la ubicación especificada. Cuando el servicio de mapa Virtual Earth devuelve, la asignación. El método Find () invoca el método de devolución de llamada callbackUpdateMapDinners que pasamos como el argumento final.

El método callbackUpdateMapDinners () es el lugar en el que se realiza el trabajo real. Usa el método auxiliar $. post () de jQuery para realizar una llamada AJAX al método de acción SearchByLocation () de SearchController, pasándole la latitud y la longitud del mapa recién centrado. Define una función insertada a la que se llamará cuando se complete el método auxiliar $. post () y los resultados de la cena con formato JSON devueltos por el método de acción SearchByLocation () se pasarán mediante una variable denominada "cenas". Después, realiza una instrucción foreach en cada cena devuelta y usa la latitud y la longitud de la cena y otras propiedades para agregar un nuevo PIN en el mapa. También agrega una entrada de cena a la lista HTML de cenas a la derecha del mapa. A continuación, conecta un evento de mantener el mouse para los marcadores y la lista HTML, de modo que los detalles sobre la cena se muestren cuando un usuario se desplace sobre ellos:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

Y ahora, cuando ejecutemos la aplicación y visitemos la Página principal, se le presentará un mapa. Cuando se especifica el nombre de una ciudad, el mapa muestra las próximas cenas cerca de ella:

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

Al mantener el puntero sobre una cena se mostrarán los detalles.

Al hacer clic en el título de la cena, ya sea en la burbuja o en el lado derecho de la lista HTML, nos dirigirá a la cena, que, a continuación, puede usar opcionalmente RSVP para:

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>Paso siguiente

Ahora hemos implementado toda la funcionalidad de la aplicación de nuestra aplicación NerdDinner. Ahora veamos cómo podemos habilitar las pruebas unitarias automatizadas de ti.

> [!div class="step-by-step"]
> [Anterior](use-ajax-to-deliver-dynamic-updates.md)
> [Siguiente](enable-automated-unit-testing.md)
