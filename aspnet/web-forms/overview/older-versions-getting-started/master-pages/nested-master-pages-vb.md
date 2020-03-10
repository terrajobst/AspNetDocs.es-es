---
uid: web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
title: Páginas maestras anidadas (VB) | Microsoft Docs
author: rick-anderson
description: Muestra cómo anidar una página maestra dentro de otra.
ms.author: riande
ms.date: 07/28/2008
ms.assetid: 14d9aa1b-4dca-43a0-aa9d-a6e891fee019
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 9bb39712855c37f5cbcbb447f7691e9451b8dc92
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78461455"
---
# <a name="nested-master-pages-vb"></a>Páginas maestras anidadas (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.zip) o [Descargar PDF](https://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.pdf)

> Muestra cómo anidar una página maestra dentro de otra.

## <a name="introduction"></a>Introducción

En el transcurso de los últimos nueve tutoriales, hemos aprendido a implementar un diseño de todo el sitio con páginas maestras. En pocas palabras, las páginas maestras nos permiten al desarrollador de páginas definir el marcado común en la página maestra junto con regiones específicas que se pueden personalizar en una página de contenido por contenido. Los controles ContentPlaceHolder en una página maestra indican las regiones personalizables; el marcado personalizado para los controles de ContentPlaceHolder se define en la página de contenido a través de los controles de contenido.

Las técnicas de página maestra que hemos explorado hasta ahora son excelentes si tiene un diseño único que se usa en todo el sitio. Sin embargo, muchos sitios web grandes tienen un diseño de sitio que se personaliza en varias secciones. Por ejemplo, imagine una aplicación de asistencia sanitaria usada por el personal de hospital para administrar la información, las actividades y la facturación de los pacientes. Puede haber tres tipos de páginas web en esta aplicación:

- Páginas específicas del personal en las que los miembros del personal pueden actualizar la disponibilidad, Ver programaciones o solicitar tiempo de vacaciones.
- Páginas específicas del paciente en las que los miembros del personal ven o editan la información de un paciente específico.
- Páginas específicas de facturación en las que los contables revisan los Estados de notificaciones actuales y los informes financieros.

Cada página puede compartir un diseño común, como un menú en la parte superior y una serie de vínculos usados con frecuencia a lo largo de la parte inferior. Sin embargo, es posible que las páginas de personal, paciente y de facturación necesiten personalizar este diseño genérico. Por ejemplo, es posible que todas las páginas específicas del personal incluyan un calendario y una lista de tareas que muestre la disponibilidad y la programación diaria del usuario que ha iniciado sesión actualmente. Quizás todas las páginas específicas del paciente deben mostrar el nombre, la dirección y la información de seguros para el paciente cuya información se está editando.

Es posible crear diseños personalizados mediante el uso de *páginas maestras anidadas*. Para implementar el escenario anterior, comenzaremos creando una página maestra que definía el diseño de todo el sitio, el contenido del menú y del pie de página, con ContentPlaceHolders que define las regiones personalizables. A continuación, crearemos tres páginas maestras anidadas, una para cada tipo de página web. Cada página maestra anidada definiría el contenido entre el tipo de páginas de contenido que utiliza la página maestra. En otras palabras, la página maestra anidada para las páginas de contenido específicas del paciente incluiría marcado y lógica de programación para mostrar información sobre el paciente que se está editando. Al crear una nueva página específica del paciente, se enlazará a esta página maestra anidada.

Este tutorial comienza resaltando las ventajas de las páginas maestras anidadas. A continuación, se muestra cómo crear y usar páginas maestras anidadas.

> [!NOTE]
> Las páginas maestras anidadas se han podido realizar desde la versión 2,0 del .NET Framework. Sin embargo, Visual Studio 2005 no incluía compatibilidad en tiempo de diseño para las páginas maestras anidadas. La buena noticia es que Visual Studio 2008 ofrece una experiencia enriquecida en tiempo de diseño para las páginas maestras anidadas. Si está interesado en usar páginas maestras anidadas pero sigue usando Visual Studio 2005, consulte la entrada de blog de [Scott Guthrie](https://weblogs.asp.net/scottgu/), [sugerencias para páginas maestras anidadas en vs 2005 en tiempo de diseño](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx).

## <a name="the-benefits-of-nested-master-pages"></a>Ventajas de las páginas maestras anidadas

Muchos sitios web tienen un diseño de sitio más general, así como diseños más personalizados específicos de determinados tipos de páginas. Por ejemplo, en nuestra aplicación Web de demostración, hemos creado una sección de administración rudimentaria (las páginas de la carpeta `~/Admin`). Actualmente, las páginas web de la carpeta `~/Admin` utilizan la misma página maestra que las páginas que no están en la sección de administración (es decir, `Site.master` o `Alternate.master`, en función de la selección del usuario).

> [!NOTE]
> Por ahora, supongamos que nuestro sitio tiene una sola página maestra, `Site.master`. Abordaremos el uso de páginas maestras anidadas con dos (o más) páginas maestras que empiecen por "usar una página maestra anidada para la sección de administración" más adelante en este tutorial.

Imagine que se le pidió personalizar el diseño de las páginas de administración para incluir información adicional o vínculos que, de otro modo, no estaban presentes en otras páginas del sitio. Hay cuatro técnicas para implementar este requisito:

1. Agregue manualmente la información específica de administración y los vínculos a todas las páginas de contenido de la carpeta `~/Admin`.
2. Actualice la página maestra de `Site.master` para incluir la información específica de la sección de administración y los vínculos y, a continuación, agregue el código a la página maestra para mostrar u ocultar estas secciones en función de si se está visitando una de las páginas de administración.
3. Cree una nueva página maestra específicamente para la sección de administración, copie el marcado de `Site.master`, agregue la información específica de la sección de administración y los vínculos y, a continuación, actualice las páginas de contenido de la carpeta `~/Admin` para usar esta nueva página maestra.
4. Cree una página maestra anidada que se enlace a `Site.master` y que las páginas de contenido de la carpeta `~/Admin` utilicen esta nueva página maestra anidada. Esta página maestra anidada incluiría solo la información adicional y los vínculos específicos de las páginas de administración y no necesitaría repetir el marcado ya definido en `Site.master`.

La primera opción es la menos palatable. Todo el punto de uso de las páginas maestras es dejar de tener que copiar y pegar manualmente el marcado común a las nuevas páginas de ASP.NET. La segunda opción es aceptable, pero hace que la aplicación sea menos fácil de mantener, ya que realiza una copia masiva de las páginas maestras con marcado que solo se muestra ocasionalmente y requiere que los desarrolladores editen la página maestra para solucionar este marcado y tener que recordar cuándo, exactamente, se muestra cierto marcado en lugar de ocultarse. Este enfoque sería menos tenable que las personalizaciones de más de tipos de páginas web que se debían incluir en esta única página maestra.

La tercera opción quita los problemas de desorden y complejidad que aparecen en la segunda opción. Sin embargo, la principal desventaja de la opción 3 es que requiere copiar y pegar el diseño común desde `Site.master` a la nueva página maestra específica de la sección de administración. Si más adelante decide cambiar el diseño de todo el sitio, tenemos que recordar cambiarlo en dos lugares.

La cuarta opción, las páginas maestras anidadas, nos ofrecen lo mejor de las opciones segunda y tercera. La información de diseño de todo el sitio se mantiene en un archivo: la página maestra de nivel superior, mientras que el contenido específico para determinadas regiones se separa en archivos diferentes.

En este tutorial se empieza con un vistazo a la creación y el uso de una página maestra anidada simple. Creamos una nueva página maestra de nivel superior, dos páginas maestras anidadas y dos páginas de contenido. A partir de la sección "uso de una página maestra anidada para la administración", veremos actualizar nuestra arquitectura de página maestra existente para incluir el uso de páginas maestras anidadas. En concreto, se crea una página maestra anidada y se usa para incluir contenido personalizado adicional para las páginas de contenido de la carpeta `~/Admin`.

## <a name="step-1-creating-a-simple-top-level-master-page"></a>Paso 1: crear una página maestra de nivel superior simple

La creación de un maestro anidado basado en una de las páginas maestras existentes y después la actualización de una página de contenido existente para usar esta nueva página maestra anidada en lugar de la página maestra de nivel superior conlleva cierta complejidad, ya que las páginas de contenido existentes ya esperan cierto Los controles ContentPlaceHolder definidos en la página maestra de nivel superior. Por lo tanto, la página maestra anidada también debe incluir los mismos controles de ContentPlaceHolder con los mismos nombres. Además, nuestra aplicación de demostración concreta tiene dos páginas maestras (`Site.master` y `Alternate.master`) que se asignan dinámicamente a una página de contenido en función de las preferencias de un usuario, que se agregan aún más a esta complejidad. Veremos cómo actualizar la aplicación existente para usar páginas maestras anidadas más adelante en este tutorial, pero vamos a centrarnos primero en un ejemplo de páginas maestras anidadas simples.

Cree una nueva carpeta denominada `NestedMasterPages` y, a continuación, agregue un nuevo archivo de página maestra a esa carpeta llamada `Simple.master`. (Consulte la figura 1 para ver una captura de pantalla de la Explorador de soluciones después de agregar esta carpeta y archivo). Arrastre el archivo de hoja de estilos `AlternateStyles.css` del Explorador de soluciones al diseñador. Esto agrega un elemento `<link>` al archivo de hoja de estilos en el elemento `<head>`, después del cual el marcado del elemento `<head>` de la página maestra debe ser similar al siguiente:

[!code-aspx[Main](nested-master-pages-vb/samples/sample1.aspx)]

A continuación, agregue el siguiente marcado en el formulario web de `Simple.master`:

[!code-aspx[Main](nested-master-pages-vb/samples/sample2.aspx)]

Este marcado muestra un vínculo titulado "páginas maestras anidadas (simple)" en la parte superior de la página en una fuente blanca grande en un fondo azul marino. Debajo es el `MainContent` ContentPlaceHolder. En la figura 1 se muestra la página maestra de `Simple.master` cuando se carga en el diseñador de Visual Studio.

[![la página maestra anidada define el contenido específico de las páginas de la sección de administración](nested-master-pages-vb/_static/image2.png)](nested-master-pages-vb/_static/image1.png)

**Figura 01**: la página maestra anidada define el contenido específico de las páginas de la sección Administración ([haga clic para ver la imagen a tamaño completo](nested-master-pages-vb/_static/image3.png))

## <a name="step-2-creating-a-simple-nested-master-page"></a>Paso 2: crear una página maestra anidada simple

`Simple.master` contiene dos controles ContentPlaceHolder: el `MainContent` ContentPlaceHolder que se ha agregado en el formulario web junto con el `head` ContentPlaceHolder en el elemento `<head>`. Si creara una página de contenido y enlazarla a `Simple.master` la página de contenido tendría dos controles de contenido que hacían referencia a los dos ContentPlaceHolders. Del mismo modo, si creamos una página maestra anidada y la enlazamos a `Simple.master`, la página maestra anidada tendrá dos controles de contenido.

Vamos a agregar una nueva página maestra anidada a la carpeta `NestedMasterPages` denominada `SimpleNested.master`. Haga clic con el botón derecho en la carpeta `NestedMasterPages` y elija Agregar nuevo elemento. Se abrirá el cuadro de diálogo Agregar nuevo elemento que se muestra en la figura 2. Seleccione el tipo de plantilla página maestra y escriba el nombre de la nueva página maestra. Para indicar que la nueva página maestra debe ser una página maestra anidada, active la casilla "seleccionar página maestra".

A continuación, haga clic en el botón Agregar. Se mostrará el mismo cuadro de diálogo Seleccionar una página maestra que verá al enlazar una página de contenido a una página maestra (vea la figura 3). Elija la página maestra de `Simple.master` en la carpeta `NestedMasterPages` y haga clic en Aceptar.

> [!NOTE]
> Si ha creado el sitio web de ASP.NET mediante el modelo de proyecto de aplicación web en lugar del modelo de proyecto de sitio web, no verá la casilla "seleccionar página maestra" en el cuadro de diálogo Agregar nuevo elemento que se muestra en la figura 2. Para crear una página maestra anidada al usar el modelo de proyecto de aplicación Web, debe elegir la plantilla de página maestra anidada (en lugar de la plantilla de página maestra). Después de seleccionar la plantilla página maestra anidada y hacer clic en agregar, aparecerá el mismo cuadro de diálogo Seleccionar una página maestra que se muestra en la figura 3.

[![Active la casilla &quot;seleccionar página maestra&quot; para agregar una página maestra anidada](nested-master-pages-vb/_static/image5.png)](nested-master-pages-vb/_static/image4.png)

**Figura 02**: Active la casilla "seleccionar página maestra" para agregar una página maestra anidada ([haga clic para ver la imagen a tamaño completo](nested-master-pages-vb/_static/image6.png))

[![enlazar la página maestra anidada a la página maestra simple. Master](nested-master-pages-vb/_static/image8.png)](nested-master-pages-vb/_static/image7.png)

**Figura 03**: enlace de la página maestra anidada a la página maestra de `Simple.master` ([haga clic para ver la imagen de tamaño completo](nested-master-pages-vb/_static/image9.png))

El marcado declarativo de la página maestra anidada, que se muestra a continuación, contiene dos controles de contenido que hacen referencia a los dos controles ContentPlaceHolder de la Página principal de nivel superior.

[!code-aspx[Main](nested-master-pages-vb/samples/sample3.aspx)]

A excepción de la Directiva `<%@ Master %>`, el marcado declarativo inicial de la página maestra anidada es idéntico al marcado que se genera inicialmente al enlazar una página de contenido a la misma página maestra de nivel superior. Al igual que la Directiva de `<%@ Page %>` de una página de contenido, la Directiva de `<%@ Master %>` aquí incluye un atributo `MasterPageFile` que especifica la página maestra primaria de la página maestra anidada. La diferencia principal entre la página maestra anidada y una página de contenido enlazada a la misma página maestra de nivel superior es que la página maestra anidada puede incluir controles ContentPlaceHolder. Los controles ContentPlaceHolder de la página maestra anidada definen las regiones en las que las páginas de contenido pueden personalizar el marcado.

Actualice esta página maestra anidada para que muestre el texto "Hello, from SimpleNested!" en el control de contenido que corresponde al control ContentPlaceHolder `MainContent`.

[!code-aspx[Main](nested-master-pages-vb/samples/sample4.aspx)]

Después de hacerlo, guarde la página maestra anidada y, a continuación, agregue una nueva página de contenido a la carpeta `NestedMasterPages` denominada `Default.aspx`y enlácelo a la página maestra `SimpleNested.master`. Al agregar esta página, es posible que le sorprenda ver que no contiene controles de contenido (consulte la figura 4). Una página de contenido solo puede tener acceso a la ContentPlaceHolders de su página maestra *primaria* . `SimpleNested.master` no contiene ningún control ContentPlaceHolder; por lo tanto, cualquier página de contenido enlazada a esta página maestra no puede contener controles de contenido.

[![la página de contenido nuevo no contiene controles de contenido](nested-master-pages-vb/_static/image11.png)](nested-master-pages-vb/_static/image10.png)

**Figura 04**: la página nuevo contenido no contiene controles de contenido ([haga clic para ver la imagen de tamaño completo](nested-master-pages-vb/_static/image12.png))

Lo que hay que hacer es actualizar la página maestra anidada (`SimpleNested.master`) para incluir los controles ContentPlaceHolder. Normalmente querrá que las páginas maestras anidadas incluyan un control ContentPlaceHolder para cada ContentPlaceHolder definido por su página maestra primaria, lo que permite que la página maestra secundaria o la página de contenido funcionen con cualquiera de los objetos ContentPlaceHolder de la página maestra de nivel superior. permite.

Actualice la página maestra de `SimpleNested.master` para incluir un control ContentPlaceHolder en sus dos controles de contenido. Asigne a ContentPlaceHolder el mismo nombre que el control ContentPlaceHolder al que hace referencia su control de contenido. Es decir, agregue un control ContentPlaceHolder denominado `MainContent` al control de contenido en `SimpleNested.master` que haga referencia a la `MainContent` ContentPlaceHolder en `Simple.master`. Haga lo mismo en el control de contenido que hace referencia a la `head` ContentPlaceHolder.

> [!NOTE]
> Aunque recomiendo asignar un nombre a los controles de ContentPlaceHolder en la página maestra anidada igual que ContentPlaceHolders en la página maestra de nivel superior, esta simetría de nomenclatura no es necesaria. Puede asignar a los controles ContentPlaceHolder en la página maestra anidada cualquier nombre que desee. Sin embargo, es más fácil recordar qué ContentPlaceHolders se corresponden con las regiones de la página si la página maestra de nivel superior y las páginas maestras anidadas usan los mismos nombres.

Después de hacer estas adiciones, el marcado declarativo de `SimpleNested.master` página maestra debe ser similar al siguiente:

[!code-aspx[Main](nested-master-pages-vb/samples/sample5.aspx)]

Elimine la `Default.aspx` página de contenido que acaba de crear y, a continuación, vuelva a agregarla, enlazándolo a la página maestra de `SimpleNested.master`. Esta vez, Visual Studio agrega dos controles de contenido al `Default.aspx`, haciendo referencia a ContentPlaceHolders ahora definido en `SimpleNested.master` (consulte la figura 6). Agregue el texto "Hello, desde default. aspx!" en el control de contenido al que hace referencia `MainContent`.

En la figura 5 se muestran las tres entidades implicadas aquí: `Simple.master`, `SimpleNested.master`y `Default.aspx`, y cómo se relacionan entre sí. Como se muestra en el diagrama, la página maestra anidada implementa controles de contenido para el ContentPlaceHolder de su elemento primario. Si es necesario tener acceso a estas regiones en la página de contenido, la página maestra anidada debe agregar su propio ContentPlaceHolders a los controles de contenido.

[![las páginas maestras de nivel superior y anidadas determinan el diseño de la página de contenido](nested-master-pages-vb/_static/image14.png)](nested-master-pages-vb/_static/image13.png)

**Figura 05**: las páginas maestras de nivel superior y anidadas dictan el diseño de la página de contenido ([haga clic para ver la imagen de tamaño completo](nested-master-pages-vb/_static/image15.png))

Este comportamiento ilustra el modo en que una página de contenido o una página maestra solo es Cognizant de su página maestra primaria. Este comportamiento también lo indica el diseñador de Visual Studio. En la ilustración 6 se muestra el diseñador de `Default.aspx`. Aunque el diseñador muestra claramente qué regiones se pueden editar en la página de contenido y qué partes no lo están, no se queda sin ambigüedades en lo que se refiere a las regiones no editables de la página maestra anidada y a qué regiones pertenecen a la página maestra de nivel superior.

[![la página de contenido ahora incluye controles de contenido para el ContentPlaceHolders de la página maestra anidada](nested-master-pages-vb/_static/image17.png)](nested-master-pages-vb/_static/image16.png)

**Figura 06**: la página de contenido ahora incluye controles de contenido para el ContentPlaceHolders de la página maestra anidada ([haga clic para ver la imagen de tamaño completo](nested-master-pages-vb/_static/image18.png))

## <a name="step-3-adding-a-second-simple-nested-master-page"></a>Paso 3: agregar una segunda página maestra anidada simple

La ventaja de las páginas maestras anidadas es más evidente cuando hay varias páginas maestras anidadas. Para ilustrar esta ventaja, cree otra página maestra anidada en la carpeta `NestedMasterPages`; asigne a esta nueva página maestra anidada el nombre `SimpleNestedAlternate.master` y enlácelo a la página maestra de `Simple.master`. Agregue controles ContentPlaceHolder en los dos controles de contenido de la página maestra anidada como hicimos en el paso 2. Agregue también el texto "Hello, from SimpleNestedAlternate!" en el control de contenido que corresponde al `MainContent` ContentPlaceHolder de la página maestra de nivel superior. Después de realizar estos cambios, el marcado declarativo de la nueva página maestra anidada debe ser similar al siguiente:

[!code-aspx[Main](nested-master-pages-vb/samples/sample6.aspx)]

Cree una página de contenido denominada `Alternate.aspx` en la carpeta `NestedMasterPages` y enlácelo a la `SimpleNestedAlternate.master` página maestra anidada. Agregue el texto "Hello, from Alternate". en el control de contenido que corresponde a `MainContent`. La figura 7 muestra `Alternate.aspx` cuando se ve a través del diseñador de Visual Studio.

[![. aspx alternativo está enlazado a la página maestra SimpleNestedAlternate. Master](nested-master-pages-vb/_static/image20.png)](nested-master-pages-vb/_static/image19.png)

**Figura 07**: `Alternate.aspx` está enlazado a la página maestra de `SimpleNestedAlternate.master` ([haga clic para ver la imagen de tamaño completo](nested-master-pages-vb/_static/image21.png))

Compare el diseñador de la ilustración 7 con el diseñador de la figura 6. Ambas páginas de contenido comparten el mismo diseño definido en la página maestra de nivel superior (`Simple.master`), es decir, el título "Tutorial de páginas maestras anidadas (simple)". Pero ambos tienen contenido distinto definido en sus páginas maestras principales: el texto "Hello, from SimpleNested!" en la figura 6 y "Hello, from SimpleNestedAlternate!" en la figura 7. Concedidos, estas diferencias son triviales, pero podría ampliar este ejemplo para incluir diferencias más significativas. Por ejemplo, la página `SimpleNested.master` podría incluir un menú con opciones específicas para sus páginas de contenido, mientras que `SimpleNestedAlternate.master` puede tener información pertinente para las páginas de contenido que se enlazan a ella.

Ahora, Imagine que necesitábamos realizar un cambio en el diseño del sitio de sobrepaso. Por ejemplo, Imagine que deseamos agregar una lista de vínculos comunes a todas las páginas de contenido. Para ello, se actualiza la página maestra de nivel superior, `Simple.master`. Los cambios que se produzcan se reflejarán inmediatamente en sus páginas maestras anidadas y, por extensión, en sus páginas de contenido.

Para demostrar la facilidad con la que se puede cambiar el diseño del sitio de superpatrón, abra la página maestra de `Simple.master` y agregue el siguiente marcado entre los elementos `topContent` y `mainContent` `<div>`:

[!code-aspx[Main](nested-master-pages-vb/samples/sample7.aspx)]

Esto agrega dos vínculos a la parte superior de todas las páginas que se enlazan a `Simple.master`, `SimpleNested.master`o `SimpleNestedAlternate.master`; Estos cambios se aplican a todas las páginas maestras anidadas y sus páginas de contenido inmediatamente. En la ilustración 8 se muestra `Alternate.aspx` cuando se ve a través de un explorador. Observe la adición de los vínculos en la parte superior de la página (en comparación con la figura 7).

[![cambiado a la página maestra de nivel superior se reflejan inmediatamente en sus páginas maestras anidadas y sus páginas de contenido](nested-master-pages-vb/_static/image23.png)](nested-master-pages-vb/_static/image22.png)

**Figura 08**: los cambios en la página maestra de nivel superior se reflejan inmediatamente en sus páginas maestras anidadas y sus páginas de contenido ([haga clic para ver la imagen a tamaño completo](nested-master-pages-vb/_static/image24.png))

## <a name="using-a-nested-master-page-for-the-administration-section"></a>Usar una página maestra anidada para la sección de administración

En este momento, hemos examinado las ventajas de las páginas maestras anidadas y hemos visto cómo crearlas y usarlas en una aplicación ASP.NET. Sin embargo, los ejemplos de los pasos 1, 2 y 3 implican la creación de una nueva página maestra de nivel superior, nuevas páginas maestras anidadas y nuevas páginas de contenido. ¿Qué ocurre cuando se agrega una nueva página maestra anidada a un sitio web con una página maestra de nivel superior y páginas de contenido existentes?

La integración de una página maestra anidada en un sitio web existente y su asociación con las páginas de contenido existentes requiere un poco más de esfuerzo que empezar desde cero. Los pasos 4, 5, 6 y 7 exploran estos desafíos a medida que aumentamos nuestra aplicación de demostración para incluir una nueva página maestra anidada denominada `AdminNested.master` que contiene instrucciones para el administrador y que usan las páginas de ASP.NET en la carpeta `~/Admin`.

La integración de una página maestra anidada en nuestra aplicación de demostración presenta los siguientes obstáculos:

- Las páginas de contenido existentes en la carpeta `~/Admin` tienen ciertas expectativas de su página maestra. En el caso de los iniciadores, esperan que haya determinados controles ContentPlaceHolder. Además, las páginas `~/Admin/AddProduct.aspx` y `~/Admin/Products.aspx` llaman al método de `RefreshRecentProductsGrid` público de la página maestra, establecen su propiedad `GridMessageText` o tienen un controlador de eventos para su evento `PricesDoubled`. Por consiguiente, nuestra página maestra anidada debe proporcionar los mismos ContentPlaceHolders y miembros públicos.
- En el tutorial anterior se ha mejorado la clase `BasePage` para establecer dinámicamente la propiedad `MasterPageFile` del objeto `Page` basándose en una variable de sesión. ¿Cómo se admiten las páginas maestras dinámicas al usar páginas maestras anidadas?

Estos dos desafíos se verán a medida que se crea la página maestra anidada y se usa en las páginas de contenido existentes. Investigaremos y superaremos estos problemas a medida que surjan.

## <a name="step-4-creating-the-nested-master-page"></a>Paso 4: crear la página maestra anidada

La primera tarea consiste en crear la página maestra anidada que usarán las páginas de la sección de administración. Como vimos en el paso 2, al agregar una nueva página maestra anidada, es necesario especificar la página maestra primaria de la página maestra anidada. Pero tenemos dos páginas maestras de nivel superior: `Site.master` y `Alternate.master`. Recuerde que hemos creado `Alternate.master` en el tutorial anterior y escrito código en la clase `BasePage` que establece la propiedad `MasterPageFile` del objeto `Page` en tiempo de ejecución en `Site.master` o `Alternate.master`, dependiendo del valor de la variable de sesión `MyMasterPage`.

¿Cómo se configura nuestra página maestra anidada para que use la página maestra de nivel superior adecuada? Tenemos dos opciones:

- Cree dos páginas maestras anidadas, `AdminNestedSite.master` y `AdminNestedAlternate.master`y enlazarlas a las páginas maestras de nivel superior `Site.master` y `Alternate.master`, respectivamente. En `BasePage`, se establecerá la `MasterPageFile` del objeto de `Page` en la página maestra anidada adecuada.
- Cree una única página maestra anidada y haga que las páginas de contenido usen esta página maestra concreta. A continuación, en tiempo de ejecución, es necesario establecer la propiedad `MasterPageFile` de la página maestra anidada en la página maestra de nivel superior adecuada en tiempo de ejecución. (Como podría haber descubierto ahora, las páginas maestras también tienen una propiedad `MasterPageFile`).

Vamos a usar la segunda opción. Cree un único archivo de página maestra anidada en la carpeta `~/Admin` denominada `AdminNested.master`. Dado que tanto `Site.master` como `Alternate.master` tienen el mismo conjunto de controles de ContentPlaceHolder, no importa a qué página maestra se enlaza, aunque recomiendo que lo enlace a `Site.master` por motivos de coherencia.

[![agregar una página maestra anidada a la carpeta ~/admin.](nested-master-pages-vb/_static/image26.png)](nested-master-pages-vb/_static/image25.png)

**Figura 09**: agregar una página maestra anidada a la carpeta `~/Admin`. ([Haga clic para ver la imagen de tamaño completo](nested-master-pages-vb/_static/image27.png))

Dado que la página maestra anidada se enlaza a una página maestra con cuatro controles ContentPlaceHolder, Visual Studio agrega cuatro controles de contenido al nuevo marcado inicial del archivo de la página maestra anidada. Como hicimos en los pasos 2 y 3, agregue un control ContentPlaceHolder en cada control de contenido y asígnele el mismo nombre que el control ContentPlaceHolder de la Página principal de nivel superior. Agregue también el marcado siguiente al control de contenido que corresponde a la `MainContent` ContentPlaceHolder:

[!code-html[Main](nested-master-pages-vb/samples/sample8.html)]

A continuación, defina la clase CSS de `instructions` en los archivos CSS `Styles.css` y `AlternateStyles.css`. Las siguientes reglas de CSS hacen que los elementos HTML con el estilo de la clase `instructions` se muestren con un color de fondo amarillo claro y un borde sólido negro:

[!code-css[Main](nested-master-pages-vb/samples/sample9.css)]

Dado que este marcado se ha agregado a la página maestra anidada, solo aparecerá en las páginas que utilizan esta página maestra anidada (es decir, las páginas de la sección de administración).

Después de hacer estas adiciones a la página maestra anidada, su marcado declarativo debe ser similar al siguiente:

[!code-aspx[Main](nested-master-pages-vb/samples/sample10.aspx)]

Tenga en cuenta que cada control de contenido tiene un control ContentPlaceHolder y que las propiedades `ID` de los controles ContentPlaceHolder tienen asignados los mismos valores que los controles ContentPlaceHolder correspondientes en la página maestra de nivel superior. Además, el marcado específico de la sección de administración aparece en el `MainContent` ContentPlaceHolder.

En la ilustración 10 se muestra la `AdminNested.master` página maestra anidada cuando se ve a través del diseñador de Visual Studio. Puede ver las instrucciones en el cuadro amarillo situado en la parte superior del control de contenido `MainContent`.

[![la página maestra anidada extiende la página maestra de nivel superior para incluir instrucciones para el administrador.](nested-master-pages-vb/_static/image29.png)](nested-master-pages-vb/_static/image28.png)

**Figura 10**: la página maestra anidada extiende la página maestra de nivel superior para incluir instrucciones para el administrador. ([Haga clic para ver la imagen de tamaño completo](nested-master-pages-vb/_static/image30.png))

## <a name="step-5-updating-the-existing-content-pages-to-use-the-new-nested-master-page"></a>Paso 5: actualizar las páginas de contenido existentes para usar la nueva página maestra anidada

Cada vez que se agrega una nueva página de contenido a la sección de administración, se debe enlazar con la `AdminNested.master` página maestra que se acaba de crear. Pero ¿qué ocurre con las páginas de contenido existentes? Actualmente, todas las páginas de contenido del sitio se derivan de la clase `BasePage`, que establece mediante programación la página maestra de la página de contenido en tiempo de ejecución. Este no es el comportamiento que queremos en las páginas de contenido de la sección de administración. En su lugar, queremos que estas páginas de contenido utilicen siempre la página `AdminNested.master`. Será responsabilidad de la página maestra anidada elegir la página de contenido de nivel superior derecha en tiempo de ejecución.

La mejor manera de lograr este comportamiento deseado es crear una nueva clase de página base personalizada denominada `AdminBasePage` que extienda la clase `BasePage`. a continuación, `AdminBasePage` puede invalidar el `SetMasterPageFile` y establecer el `MasterPageFile` del objeto `Page` en el valor codificado de forma rígida "~/Admin/AdminNested.master". De esta manera, cualquier página que se derive de `AdminBasePage` usará `AdminNested.master`, mientras que cualquier página que derive de `BasePage` tendrá su `MasterPageFile` propiedad establecida dinámicamente en "~/site.Master" o "~/Alternate.Master" según el valor de la variable de sesión `MyMasterPage`.

Empiece agregando un nuevo archivo de clase a la carpeta `App_Code` denominada `AdminBasePage.vb`. Tenga `AdminBasePage` extienda `BasePage` y, a continuación, invalide el método `SetMasterPageFile`. En ese método, asigne el `MasterPageFile` el valor "~/Admin/AdminNested.master". Después de realizar estos cambios, el archivo de clase debe ser similar al siguiente:

[!code-vb[Main](nested-master-pages-vb/samples/sample11.vb)]

Ahora es necesario que las páginas de contenido existentes en la sección de administración se deriven de `AdminBasePage` en lugar de `BasePage`. Vaya al archivo de clase de código subyacente para cada página de contenido de la carpeta `~/Admin` y realice este cambio. Por ejemplo, en `~/Admin/Default.aspx` cambiaría la declaración de clase de código subyacente de:

[!code-vb[Main](nested-master-pages-vb/samples/sample12.vb)]

A:

[!code-vb[Main](nested-master-pages-vb/samples/sample13.vb)]

En la figura 11 se muestra cómo se relacionan entre sí la página maestra de nivel superior (`Site.master` o `Alternate.master`), la página maestra anidada (`AdminNested.master`) y las páginas de contenido de la sección de administración.

[![la página maestra anidada define el contenido específico de las páginas de la sección de administración](nested-master-pages-vb/_static/image32.png)](nested-master-pages-vb/_static/image31.png)

**Figura 11**: la página maestra anidada define el contenido específico de las páginas de la sección Administración ([haga clic para ver la imagen a tamaño completo](nested-master-pages-vb/_static/image33.png))

## <a name="step-6-mirroring-the-master-pages-public-methods-and-properties"></a>Paso 6: crear un reflejo de los métodos y propiedades públicos de la página maestra

Recuerde que las páginas `~/Admin/AddProduct.aspx` y `~/Admin/Products.aspx` interactúan mediante programación con la página maestra: `~/Admin/AddProduct.aspx` llama al método de `RefreshRecentProductsGrid` público de la página maestra y establece su propiedad `GridMessageText`. `~/Admin/Products.aspx` tiene un controlador de eventos para el evento `PricesDoubled`. En el tutorial anterior, creamos una `MustInherit` `BaseMasterPage` clase que definía estos miembros públicos.

En las páginas `~/Admin/AddProduct.aspx` y `~/Admin/Products.aspx` se supone que su página maestra se deriva de la clase `BaseMasterPage`. Sin embargo, la página `AdminNested.master` extiende actualmente la clase `System.Web.UI.MasterPage`. Como resultado, al visitar `~/Admin/Products.aspx` se inicia una `InvalidCastException` con el mensaje: "no se puede convertir el objeto de tipo ' ASP. admin\_adminnested\_Master ' al tipo ' BaseMasterPage '".

Para solucionar este paso, es necesario que la clase de código subyacente `AdminNested.master` extienda `BaseMasterPage`. Actualice la declaración de clase de código subyacente de la página maestra anidada de:

[!code-vb[Main](nested-master-pages-vb/samples/sample14.vb)]

A:

[!code-vb[Main](nested-master-pages-vb/samples/sample15.vb)]

Todavía no hemos terminado. Tenemos que reemplazar los miembros marcados como `MustOverride`, es decir, `RefreshRecentProductsGrid` y `GridMessageText`. Estos miembros se utilizan en las páginas maestras de nivel superior para actualizar sus interfaces de usuario. (En realidad, solo el `Site.master` página maestra utiliza estos métodos, aunque las dos páginas maestras de nivel superior implementan estos métodos, ya que ambos `BaseMasterPage`extender).

Aunque necesitamos implementar estos miembros en `AdminNested.master`, todas estas implementaciones solo deben llamar al mismo miembro en la página maestra de nivel superior utilizada por la página maestra anidada. Por ejemplo, cuando una página de contenido de la sección de administración llama al método de `RefreshRecentProductsGrid` de la página maestra anidada, toda la página maestra anidada debe hacer, a su vez, llamar a `Site.master` o `Alternate.master`método de `RefreshRecentProductsGrid`.

Para ello, empiece agregando la siguiente directiva de `@MasterType` a la parte superior de `AdminNested.master`:

[!code-aspx[Main](nested-master-pages-vb/samples/sample16.aspx)]

Recuerde que la Directiva de `@MasterType` agrega una propiedad fuertemente tipada a la clase de código subyacente llamada `Master`. A continuación, invalide los miembros `RefreshRecentProductsGrid` y `GridMessageText` y, simplemente, delegue la llamada al método correspondiente del `Master`:

[!code-vb[Main](nested-master-pages-vb/samples/sample17.vb)]

Con este código en su lugar, debería poder visitar y usar las páginas de contenido de la sección de administración. En la ilustración 12 se muestra la página `~/Admin/Products.aspx` cuando se ve a través de un explorador. Como puede ver, la página incluye el cuadro instrucciones de administración, que se define en la página maestra anidada.

[![las páginas de contenido de la sección de administración incluyen instrucciones en la parte superior de cada página](nested-master-pages-vb/_static/image35.png)](nested-master-pages-vb/_static/image34.png)

**Figura 12**: las páginas de contenido de la sección de administración incluyen instrucciones en la parte superior de cada página ([haga clic para ver la imagen a tamaño completo](nested-master-pages-vb/_static/image36.png))

## <a name="step-7-using-the-appropriate-top-level-master-page-at-runtime"></a>Paso 7: usar la página maestra de nivel superior adecuada en tiempo de ejecución

Aunque todas las páginas de contenido de la sección de administración son totalmente funcionales, utilizan la misma página maestra de nivel superior y omiten la página maestra seleccionada por el usuario en `ChooseMasterPage.aspx`. Este comportamiento se debe al hecho de que la página maestra anidada tiene su propiedad `MasterPageFile` establecida estáticamente en `Site.master` en su Directiva `<%@ Master %>`.

Para usar la página maestra de nivel superior seleccionada por el usuario final, es necesario establecer la propiedad `MasterPageFile` del `AdminNested.master`en el valor de la variable de sesión `MyMasterPage`. Dado que se establecen las propiedades de `MasterPageFile` de las páginas de contenido en `BasePage`, puede pensar que estableceremos la propiedad `MasterPageFile` de la página maestra anidada en `BaseMasterPage` o en la clase de código subyacente de la `AdminNested.master`. Sin embargo, esto no funcionará porque es necesario establecer la propiedad `MasterPageFile` al final de la fase de PreInit. La primera vez que se pueda pulsar mediante programación en el ciclo de vida de la página desde una página maestra es la fase de inicialización (que se produce después de la fase de PreInit).

Por lo tanto, es necesario establecer la propiedad `MasterPageFile` de la página maestra anidada a partir de las páginas de contenido. Las únicas páginas de contenido que utilizan la página maestra de `AdminNested.master` derivan de `AdminBasePage`. Por lo tanto, podemos colocar allí esta lógica. En el paso 5 se reemplazó el método `SetMasterPageFile` y se establece la propiedad `MasterPageFile` del objeto de página en "~/Admin/AdminNested.master". Actualice `SetMasterPageFile` para establecer también la propiedad `MasterPageFile` de la página maestra en el resultado almacenado en la sesión:

[!code-vb[Main](nested-master-pages-vb/samples/sample18.vb)]

El método `GetMasterPageFileFromSession`, que se ha agregado a la clase `BasePage` en el tutorial anterior, devuelve la ruta de acceso del archivo de la página maestra adecuada según el valor de la variable de sesión.

Con este cambio, la selección de la página maestra del usuario se lleva a la sección de administración. En la figura 13 se muestra la misma página que la figura 12, pero después de que el usuario cambia la selección de la página maestra a `Alternate.master`.

[![la página de administración anidada usa la página maestra de nivel superior seleccionada por el usuario](nested-master-pages-vb/_static/image38.png)](nested-master-pages-vb/_static/image37.png)

**Figura 13**: la página de administración anidada usa la página maestra de nivel superior seleccionada por el usuario ([haga clic para ver la imagen a tamaño completo](nested-master-pages-vb/_static/image39.png))

## <a name="summary"></a>Resumen

Del mismo modo que las páginas de contenido se pueden enlazar a una página maestra, es posible crear páginas maestras anidadas haciendo que una página maestra secundaria se enlace a una página maestra primaria. La página maestra secundaria puede definir controles de contenido para cada uno de los ContentPlaceHolders de elementos primarios. después, puede agregar sus propios controles ContentPlaceHolder (así como otro marcado) a estos controles de contenido. Las páginas maestras anidadas son bastante útiles en aplicaciones Web de gran tamaño en las que todas las páginas comparten la apariencia y el funcionamiento, pero algunas secciones del sitio requieren personalizaciones únicas.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Páginas maestras de ASP.NET anidadas](https://msdn.microsoft.com/library/x2b3ktt7.aspx)
- [Sugerencias para páginas maestras anidadas y VS 2005 en tiempo de diseño](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)
- [Compatibilidad con la página maestra anidada de VS 2008](https://weblogs.asp.net/scottgu/archive/2007/07/09/vs-2008-nested-master-page-support.aspx)

### <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de varios libros de ASP/ASP. net y fundador de 4GuysFromRolla.com, ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 3,5 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Se puede acceder a Scott en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) o a través de su blog en [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. ¿Está interesado en revisar los próximos artículos de MSDN? Si es así, coloque una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](specifying-the-master-page-programmatically-vb.md)
