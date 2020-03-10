---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
title: 'Presentación de ASP.NET Web Pages: Mostrar datos | Microsoft Docs'
author: Rick-Anderson
description: En este tutorial se muestra cómo crear una base de datos en WebMatrix y cómo mostrar los datos de la base de datos en una página cuando se usa ASP.NET Web Pages (Razor). Se supone y...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: b3a006a0-3ea2-4d45-b833-e20e3a3c0a1a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
msc.type: authoredcontent
ms.openlocfilehash: 9e665ca8dd064c23a8b8bd3593014969d0c3da48
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78421489"
---
# <a name="introducing-aspnet-web-pages---displaying-data"></a>Presentación de ASP.NET Web Pages: Mostrar datos

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este tutorial se muestra cómo crear una base de datos en WebMatrix y cómo mostrar los datos de la base de datos en una página cuando se usa ASP.NET Web Pages (Razor). Se supone que ha completado la serie a través [de la introducción a la programación de ASP.NET Web pages](../introducing-razor-syntax-c.md).
> 
> Temas que se abordarán:
> 
> - Cómo usar las herramientas de WebMatrix para crear una base de datos y tablas de base de datos.
> - Cómo usar las herramientas de WebMatrix para agregar datos a una base de datos.
> - Cómo mostrar los datos de la base de datos en una página.
> - Cómo ejecutar comandos SQL en ASP.NET Web Pages.
> - Cómo personalizar la aplicación auxiliar de `WebGrid` para cambiar la presentación de los datos y agregar paginación y ordenación.
>   
> 
> Características y tecnologías descritas:
> 
> - Herramientas de base de datos WebMatrix.
> - `WebGrid` ayudante.

## <a name="what-youll-build"></a>Lo que creará

En el tutorial anterior, se presentó ASP.NET Web Pages (archivos *. cshtml* ), a los aspectos básicos de sintaxis Razor y a las aplicaciones auxiliares. En este tutorial, comenzará a crear la aplicación web real que usará para el resto de la serie. La aplicación es una aplicación de película simple que le permite ver, agregar, cambiar y eliminar información sobre películas.

Cuando haya terminado este tutorial, podrá ver una lista de películas similar a la de esta página:

![Presentación de WebGrid que incluye los parámetros establecidos en los nombres de clase CSS](displaying-data/_static/image1.png)

Pero, para empezar, tiene que crear una base de datos.

## <a name="a-very-brief-introduction-to-databases"></a>Una breve introducción a las bases de datos

En este tutorial solo se proporcionará la breve introducción a las bases de datos. Si tiene experiencia de base de datos, puede omitir esta breve sección.

Una base de datos contiene una o más tablas que contienen información &mdash; por ejemplo, tablas para clientes, pedidos y proveedores, o para estudiantes, profesores, clases y calificaciones. Estructuralmente, una tabla de base de datos es como una hoja de cálculo. Imagine una libreta de direcciones típica. Para cada entrada de la libreta de direcciones (es decir, para cada persona) tiene varios datos como el nombre, el apellido, la dirección, la dirección de correo electrónico y el número de teléfono.

![Tabla de base de datos de ejemplo como una cuadrícula simple](displaying-data/_static/image2.png)

(A veces se hace referencia a las filas como *registros*y las columnas a veces se denominan *campos*).

Para la mayoría de las tablas de base de datos, la tabla debe tener una columna que contenga un valor único, como un número de cliente, un número de cuenta, etc. Este valor se conoce como la *clave principal*de la tabla y se utiliza para identificar cada fila de la tabla. En el ejemplo, la columna ID es la clave principal de la libreta de direcciones mostrada en el ejemplo anterior.

Gran parte del trabajo que se realiza en las aplicaciones web consiste en leer información fuera de la base de datos y mostrarla en una página. A menudo, también recopila información de los usuarios y la agrega a una base de datos, o bien modifica los registros que ya están en la base de datos. (Trataremos todas estas operaciones en el transcurso de este conjunto de tutoriales).

El trabajo de base de datos puede ser enormemente complejo y puede requerir conocimientos especializados. Sin embargo, para este tutorial, debe comprender solo los conceptos básicos, que se explicarán a medida que avanza.

## <a name="creating-a-database"></a>Crear una base de datos

WebMatrix incluye herramientas que facilitan la creación de una base de datos y la creación de tablas en la base de datos. (La estructura de una base de datos se conoce como el *esquema*de la base de datos). En este conjunto de tutoriales, creará una base de datos que solo tenga una tabla en ella &mdash; películas.

Abra WebMatrix si todavía no lo ha hecho y abra el sitio de WebPagesMovies que creó en el tutorial anterior.

En el panel izquierdo, haga clic en el área de trabajo **base de datos** .

![Pestaña del área de trabajo de base de datos WebMatrix](displaying-data/_static/image3.png)

La cinta de opciones cambia para mostrar las tareas relacionadas con la base de datos. En la cinta de opciones, haga clic en **nueva base de datos**.

![Botón "nueva base de datos" en la cinta WebMatrix](displaying-data/_static/image4.png)

WebMatrix crea una base de datos de SQL Server CE (archivo *. sdf* ) que tiene el mismo nombre que el sitio &mdash; *WebPagesMovies. sdf*. (Aquí no lo hará, pero puede cambiar el nombre del archivo a cualquier elemento que desee, siempre que tenga una extensión *. sdf* ).

## <a name="creating-a-table"></a>Creación de una tabla

En la cinta de opciones, haga clic en **nueva tabla**. WebMatrix abre el diseñador de tablas en una pestaña nueva. (si la opción nueva tabla no está disponible, asegúrese de que la nueva base de datos está seleccionada en la vista de árbol de la izquierda).

![Botón ' nueva tabla ' en la cinta WebMatrix](displaying-data/_static/image5.png)

En el cuadro de texto de la parte superior (donde la marca de agua dice "Escriba el nombre de la tabla"), escriba "películas".

![Escribir un nombre de tabla en el diseñador de bases de datos WebMatrix](displaying-data/_static/image6.png)

El panel debajo del nombre de la tabla es donde se definen las columnas individuales. En la tabla *películas* de este tutorial, creará solo algunas columnas: *ID*, *title*, *Genre*y *Year*.

En el cuadro **nombre** , escriba "ID". Al escribir un valor aquí se activan todos los controles de la nueva columna.

En la lista **tipo de datos** y elija **int**. Este valor especifica que la columna de identificador contendrá datos enteros (número).

> [!NOTE]
> No lo haremos más aquí (mucho), pero puede usar gestos de teclado estándar de Windows para navegar por esta cuadrícula. Por ejemplo, puede hacer una tabulación entre campos, simplemente empezar a escribir para seleccionar un elemento de una lista, etc.

Mantenga el tabulador más allá del cuadro **valor predeterminado** (es decir, déjelo en blanco). En la casilla **es la clave principal** y selecciónela. Esta opción indica a la base de datos que la columna *ID* contendrá los datos que identifican filas individuales. (Es decir, cada fila tendrá un valor único en la columna ID que puede usar para buscar esa fila).

Elija la opción **identidad** . Esta opción indica a la base de datos que debe generar automáticamente el siguiente número secuencial para cada nueva fila. (La opción **is Identity** solo funciona si se ha seleccionado también la opción **is PRIMARY KEY** ).

Haga clic en la siguiente fila de la cuadrícula o presione la tecla TAB dos veces para finalizar la fila actual. Cualquier gesto guarda la fila actual e inicia la siguiente. Observe que la columna **valor predeterminado** ahora indica **null**. (NULL es el valor predeterminado del valor predeterminado, por lo que debe hablar).

Cuando haya terminado de definir la nueva columna **ID** , el diseñador tendrá el aspecto siguiente:

![Diseñador de bases de datos WebMatrix después de definir la columna ID de la tabla películas](displaying-data/_static/image7.png)

Para crear la columna siguiente, haga clic en el cuadro de la columna **nombre** . Escriba "title" para la columna y, a continuación, seleccione **nvarchar** como valor de **tipo de datos** . La parte "var" de **nvarchar** indica a la base de datos que los datos de esta columna serán una cadena cuyo tamaño puede variar de un registro a otro. (El prefijo "n" representa "National", que indica que el campo puede contener datos de caracteres para cualquier alfabeto o sistema de escritura, es decir, el campo contiene datos Unicode.)

Cuando elige **nvarchar**, aparece otro cuadro en el que puede especificar la longitud máxima del campo. Escriba 50, suponiendo que ningún título de película con el que trabajará en este tutorial tendrá más de 50 caracteres.

Omitir el **valor predeterminado** y desactivar la opción **permitir valores NULL** . No quiere que la base de datos permita que se escriban películas en la base de datos que no tengan un título.

Cuando haya terminado y pase a la siguiente fila, el diseñador tendrá el siguiente aspecto:

![Diseñador de bases de datos WebMatrix después de definir la columna title de la tabla Movies](displaying-data/_static/image8.png)

Repita estos pasos para crear una columna denominada "género", salvo la longitud, establézcala en solo 30.

Cree otra columna denominada "Year". Para el tipo de datos, elija **nchar** (no **nvarchar**) y establezca la longitud en 4. Durante el año, va a usar un número de 4 dígitos como "1995" o "2010", por lo que no necesita una columna de tamaño variable.

Este es el aspecto del diseño terminado:

![Diseñador de bases de datos WebMatrix una vez definidos todos los campos para la tabla películas](displaying-data/_static/image9.png)

Presione CTRL + S o haga clic en el botón **Guardar** en la barra de herramientas de acceso rápido. Cierre la pestaña para cerrar el diseñador de bases de datos.

## <a name="adding-some-example-data"></a>Agregar algunos datos de ejemplo

Más adelante en esta serie de tutoriales, creará una página en la que podrá escribir nuevas películas en un formulario. Por el momento, sin embargo, puede agregar algunos datos de ejemplo que se pueden mostrar en una página.

En el área de trabajo de **base de datos** de WebMatrix, observe que hay un árbol que muestra el archivo *. sdf* que creó anteriormente. Abra el nodo del nuevo archivo *. sdf* y, a continuación, abra el nodo **tablas** .

![Área de trabajo de base de datos WebMatrix con árbol abierto en tabla de películas](displaying-data/_static/image10.png)

Haga clic con el botón secundario en el nodo **películas** y, a continuación, elija **datos**. WebMatrix abre una cuadrícula en la que puede escribir datos para la tabla *películas* :

![Cuadrícula de entrada de base de datos en WebMatrix (vacío)](displaying-data/_static/image11.png)

Haga clic en la columna **title** y escriba "when Harry mét Sally". Desplácese a la columna **género** (puede usar la tecla TAB) y escriba "Romantic comedia". Vaya a la columna **Year** y escriba "1989":

![Cuadrícula de entrada de base de datos en WebMatrix con un registro](displaying-data/_static/image12.png)

Presione entrar y WebMatrix guarda la nueva película. Observe que se ha rellenado la columna **ID** .

![Cuadrícula de entrada de base de datos en WebMatrix con un registro y un identificador generado automáticamente](displaying-data/_static/image13.png)

Escriba otra película (por ejemplo, "ido con el viento", "series", "1939"). La columna ID se rellena de nuevo:

![Cuadrícula de entrada de base de datos en WebMatrix con dos registros e identificadores generados automáticamente](displaying-data/_static/image14.png)

Escriba una tercera película (por ejemplo, "Ghostbusters", "comedia"). Como experimento, deje en blanco la columna **Year** y, a continuación, presione Entrar. Dado que no seleccionó la opción **permitir valores NULL** , la base de datos muestra un error:

![Se muestra el error "datos no válidos" Si un valor de columna requerido se deja en blanco](displaying-data/_static/image15.png)

Haga clic en **Aceptar** para volver atrás y corregir la entrada (el año para "Ghostbusters" es 1984) y, a continuación, presione Entrar.

Rellene varias películas hasta que tenga 8 o más. (Al escribir 8 se facilita el trabajo con la paginación más adelante. Pero si es demasiado elevado, escriba algunos por ahora. Los datos reales no importan.

![Cuadrícula de entrada de base de datos en WebMatrix con registros](displaying-data/_static/image16.png)

Si ha especificado todas las películas sin errores, los valores de ID. son secuenciales. Si ha intentado guardar un registro de película incompleto, los números de identificación podrían no ser secuenciales. Si es así, es correcto. Los números no tienen ningún significado inherente y lo único que es importante es que son únicos en la tabla de *películas* .

Cierre la pestaña que contiene el diseñador de bases de datos.

Ahora puede activar la visualización de estos datos en una página web.

## <a name="displaying-data-in-a-page-by-using-the-webgrid-helper"></a>Mostrar datos en una página mediante la aplicación auxiliar WebGrid

Para mostrar los datos en una página, va a utilizar la aplicación auxiliar de `WebGrid`. Esta aplicación auxiliar genera una presentación en una cuadrícula o una tabla (filas y columnas). Como verá, podrá refinar la cuadrícula con formato y otras características.

Para ejecutar la cuadrícula, tendrá que escribir algunas líneas de código. Estas pocas líneas servirán como un tipo de patrón para casi todo el acceso a datos que se realiza en este tutorial.

> [!NOTE]
> Realmente tiene muchas opciones para Mostrar datos en una página. la aplicación auxiliar `WebGrid` es solo una. Lo hemos elegido para este tutorial, ya que es la forma más fácil de Mostrar datos y porque es razonablemente flexible. En el siguiente tutorial, verá cómo usar una forma más "manual" de trabajar con datos en la página, lo que le proporciona un control más directo sobre cómo mostrar los datos.

En el panel izquierdo de WebMatrix, haga clic en el área de trabajo **archivos** .

La nueva base de datos que creó está en la carpeta *App\_Data* . Si la carpeta todavía no existía, WebMatrix la creó para la nueva base de datos. (Es posible que haya existido la carpeta si previamente instaló aplicaciones auxiliares).

En la vista de árbol, seleccione la raíz del sitio Web. Debe seleccionar la raíz del sitio web; de lo contrario, el nuevo archivo podría agregarse a la carpeta de datos de la\_de la aplicación.

En la cinta de opciones, haga clic en **nuevo**. En el cuadro **Elija un tipo de archivo** , elija **CSHTML**.

En el cuadro **nombre** , asigne a la nueva página el nombre "movies. cshtml":

![Cuadro de diálogo "elegir un tipo de archivo" que muestra la página "películas"](displaying-data/_static/image17.png)

Haga clic en el botón **Aceptar** . WebMatrix abre un nuevo archivo que contiene algunos elementos de esqueleto. En primer lugar, escribirá código para obtener los datos de la base de datos. A continuación, agregará marcado a la página para mostrar realmente los datos.

### <a name="writing-the-data-query-code"></a>Escribir el código de consulta de datos

En la parte superior de la página, entre los caracteres `@{` y `}`, escriba el código siguiente. (Asegúrese de escribir este código entre las llaves de apertura y cierre).

[!code-csharp[Main](displaying-data/samples/sample1.cs)]

La primera línea abre la base de datos que creó anteriormente, que es siempre el primer paso antes de hacer algo con la base de datos. Se indica al nombre del método de `Database.Open` de la base de datos que se va a abrir. Tenga en cuenta que no incluye *. sdf* en el nombre. El método `Open` supone que está buscando un archivo *. sdf* (es decir, *WebPagesMovies. sdf*) y que el archivo *. sdf* está en la carpeta *App\_Data* . (Antes se indicó que la carpeta de datos de la\_de la *aplicación* está reservada; este escenario es uno de los lugares en los que ASP.net hace suposiciones sobre ese nombre).

Cuando se abre la base de datos, se coloca una referencia a ella en la variable denominada `db`. (Que podría denominarse cualquier cosa). La variable de `db` es la forma en que finalizará la interacción con la base de datos.

La segunda línea busca realmente los datos de la base de datos mediante el método `Query`. Observe cómo funciona este código: la variable `db` tiene una referencia a la base de datos abierta e invoca el método `Query` mediante la variable `db` (`db.Query`).

La propia consulta es una instrucción de `Select` de SQL. (Para obtener un pequeño fondo sobre SQL, vea la explicación más adelante). En la instrucción, `Movies` identifica la tabla que se va a consultar. El carácter `*` especifica que la consulta debe devolver todas las columnas de la tabla. (También puede enumerar columnas individualmente, separadas por comas).

Los resultados de la consulta, si los hay, se devuelven y se ponen a disposición en la variable `selectedData`. Una vez más, la variable podría denominarse cualquier cosa.

Por último, la tercera línea indica a ASP.NET que desea usar una instancia de la aplicación auxiliar de `WebGrid`. Cree (cree*una instancia*) del objeto auxiliar mediante la palabra clave `new` y pásele los resultados de la consulta a través de la variable `selectedData`. El nuevo objeto `WebGrid`, junto con los resultados de la consulta de base de datos, están disponibles en la variable `grid`. Necesitará que el resultado sea un momento para mostrar realmente los datos en la página.

En esta fase, la base de datos se ha abierto, ha obtenido los datos que desea y ha preparado la aplicación auxiliar de `WebGrid` con esos datos. A continuación, se crea el marcado en la página.

> [!TIP] 
> 
> **Lenguaje de consulta estructurado (SQL)**
> 
> SQL es un lenguaje que se usa en la mayoría de las bases de datos relacionales para administrar datos en una base de datos. Incluye comandos que permiten recuperar datos y actualizarlos, y que permiten crear, modificar y administrar datos en tablas de base de datos. SQL es diferente de un lenguaje de programación ( C#como). Con SQL, se indica a la base de datos qué desea, y es el trabajo de la base de datos para averiguar cómo obtener los datos o realizar la tarea. Estos son algunos ejemplos de algunos comandos SQL y lo que hacen:
> 
> `Select * From Movies`
> 
> `SELECT ID, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> La primera instrucción `Select` obtiene todas las columnas (especificadas por `*`) de la tabla *películas* .
> 
> La segunda instrucción `Select` captura las columnas ID, Name y Price de los registros de la tabla *Product* cuyo valor de la columna Price sea superior a 10. El comando devuelve los resultados en orden alfabético en función de los valores de la columna Name. Si no hay registros que coincidan con los criterios de precio, el comando devuelve un conjunto vacío.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ('Croissant', 'A flaky delight', 1.99)`
> 
> Este comando inserta un nuevo registro en la tabla *Product* , estableciendo la columna Name en "croissant", la columna Description en "alegría" y el precio en 1,99.
> 
> Observe que, cuando se especifica un valor no numérico, el valor se incluye entre comillas simples (no entre comillas dobles, como en C#). Estas comillas se usan alrededor de los valores de texto o de fecha, pero no de los números.
> 
> `DELETE FROM Product WHERE ExpirationDate < '01/01/2008'`
> 
> Este comando elimina los registros de la tabla *Product* cuya columna fecha de expiración sea anterior al 1 de enero de 2008. (El comando supone que la tabla *Product* tiene una columna de este tipo, por supuesto). La fecha se escribe aquí en formato MM/DD/AAAA, pero debe escribirse en el formato que se usa para la configuración regional.
> 
> Los comandos `Insert` y `Delete` no devuelven conjuntos de resultados. En su lugar, devuelven un número que indica cuántos registros se vieron afectados por el comando.
> 
> En algunas de estas operaciones (como la inserción y eliminación de registros), el proceso que solicita la operación debe tener los permisos adecuados en la base de datos. Este es el motivo por el que las bases de datos de producción suelen tener que proporcionar un nombre de usuario y una contraseña al conectarse a la base de datos.
> 
> Hay docenas de comandos SQL, pero todos siguen un patrón como los comandos que se ven aquí. Puede usar comandos SQL para crear tablas de base de datos, contar el número de registros de una tabla, calcular precios y realizar muchas más operaciones.

### <a name="adding-markup-to-display-the-data"></a>Agregar marcado para mostrar los datos

Dentro del elemento `<head>`, establezca el contenido del elemento `<title>` en "Movies":

[!code-html[Main](displaying-data/samples/sample2.html?highlight=3)]

En el elemento `<body>` de la página, agregue lo siguiente:

[!code-html[Main](displaying-data/samples/sample3.html)]

Ya está. La variable `grid` es el valor que creó al crear el objeto `WebGrid` en el código anterior.

En la vista de árbol de WebMatrix, haga clic con el botón derecho en la página y seleccione **iniciar en el explorador**. Verá algo parecido a esta página:

![Resultado de la aplicación auxiliar de WebGrid predeterminada de la tabla películas](displaying-data/_static/image18.png)

Haga clic en un vínculo de encabezado de columna para ordenar por esa columna. Poder ordenar haciendo clic en un encabezado es una característica que está integrada en la aplicación auxiliar **WebGrid** .

El método `GetHtml`, como sugiere su nombre, genera marcado que muestra los datos. De forma predeterminada, el método `GetHtml` genera un elemento de `<table>` HTML. (Si lo desea, puede comprobar la representación examinando el origen de la página en el explorador).

## <a name="modifying-the-look-of-the-grid"></a>Modificar la apariencia de la cuadrícula

El uso de la aplicación auxiliar `WebGrid` como acaba de hacer es sencillo, pero la pantalla resultante es sin formato. La aplicación auxiliar de `WebGrid` tiene todo tipo de opciones que permiten controlar cómo se muestran los datos. Hay demasiados para explorar en este tutorial, pero esta sección le dará una idea de algunas de esas opciones. En los tutoriales posteriores de esta serie se tratarán algunas opciones adicionales.

### <a name="specifying-individual-columns-to-display"></a>Especificar columnas individuales para mostrar

Para empezar, puede especificar que desea mostrar solo determinadas columnas. De forma predeterminada, como se ha visto, la cuadrícula muestra las cuatro columnas de la tabla *películas* .

En el archivo *movies. cshtml* , reemplace el `@grid.GetHtml()` marcado que acaba de agregar con lo siguiente:

[!code-css[Main](displaying-data/samples/sample4.css)]

Para indicar a la aplicación auxiliar Qué columnas se van a mostrar, incluya un parámetro `columns` para el método `GetHtml` y pase una colección de columnas. En la colección, se especifica cada columna que se va a incluir. Especifique una columna individual para mostrar mediante la inclusión de un objeto `grid.Column` y pase el nombre de la columna de datos que desee. (Estas columnas deben incluirse en los resultados de la consulta SQL; el ayudante de `WebGrid` no puede mostrar las columnas que no devolvió la consulta).

Vuelva a iniciar la página *movies. cshtml* en el explorador y, esta vez, obtenga una pantalla similar a la siguiente (Observe que no se muestra ninguna columna ID):

![Pantalla de WebGrid que muestra solo las columnas seleccionadas](displaying-data/_static/image19.png)

### <a name="changing-the-look-of-the-grid"></a>Cambiar el aspecto de la cuadrícula

Hay algunas opciones más para mostrar columnas, algunas de las cuales se explorarán en los tutoriales posteriores de este conjunto. Por ahora, en esta sección se presentan las distintas formas en que se puede aplicar estilo a la cuadrícula en su totalidad.

Dentro de la sección `<head>` de la página, justo antes de la etiqueta de cierre `</head>`, agregue el siguiente elemento `<style>`:

[!code-css[Main](displaying-data/samples/sample5.css)]

Este marcado CSS define clases denominadas `grid`, `head`, etc. También puede colocar estas definiciones de estilo en un archivo *. CSS* independiente y vincularlo a la página. (De hecho, lo hará más adelante en este tutorial). Pero para facilitar las cosas en este tutorial, están dentro de la misma página que muestra los datos.

Ahora puede obtener la aplicación auxiliar `WebGrid` para usar estas clases de estilo. La aplicación auxiliar tiene una serie de propiedades (por ejemplo, `tableStyle`) para este propósito: le asigna un nombre de clase de estilo CSS y ese nombre de clase se representa como parte del marcado que representa el ayudante.

Cambie el marcado de `grid.GetHtml` para que ahora tenga el siguiente aspecto:

[!code-css[Main](displaying-data/samples/sample6.css)]

Lo nuevo aquí es que ha agregado `tableStyle`, `headerStyle`y `alternatingRowStyle` parámetros al método `GetHtml`. Estos parámetros se han establecido en los nombres de los estilos CSS que ha agregado hace un momento.

Ejecute la página y esta vez verá una cuadrícula que parece mucho menos sencilla que antes:

![Presentación de WebGrid que incluye los parámetros establecidos en los nombres de clase CSS](displaying-data/_static/image20.png)

Para ver lo que ha generado el método de `GetHtml`, puede examinar el origen de la página en el explorador. Aquí no entraremos en detalle, pero lo importante es que al especificar parámetros como `tableStyle`, se hizo que la cuadrícula generara etiquetas HTML como las siguientes:

`<table class="grid">`

Se ha agregado un atributo `class` a la etiqueta `<table>` que hace referencia a una de las reglas de CSS que agregó anteriormente. Este código muestra el patrón básico &mdash; diferentes parámetros para el método `GetHtml` permiten hacer referencia a las clases CSS que el método genera junto con el marcado. Lo que hace con las clases CSS depende de usted.

## <a name="adding-paging"></a>Agregar paginación

Como última tarea de este tutorial, agregará la paginación a la cuadrícula. En este momento no hay ningún problema para mostrar todas las películas a la vez. Pero si agregó cientos de películas, la pantalla de la página se verá larga.

En el código de la página, cambie la línea que crea el objeto `WebGrid` al código siguiente:

[!code-csharp[Main](displaying-data/samples/sample7.cs)]

La única diferencia de antes es que se ha agregado un parámetro `rowsPerPage` que se establece en 3.

Ejecute la página. La cuadrícula muestra 3 filas a la vez, además de vínculos de navegación que permiten paginar las películas en la base de datos:

![Presentación de WebGrid con paginación](displaying-data/_static/image21.png)

## <a name="coming-up-next"></a>Próxima

En el siguiente tutorial, aprenderá a usar Razor y C# el código para obtener los datos proporcionados por el usuario en un formulario. Agregará un cuadro de búsqueda a la página películas para que pueda buscar películas por título o género.

## <a name="complete-listing-for-movies-page"></a>Lista completa de la página de películas

[!code-cshtml[Main](displaying-data/samples/sample8.cshtml)]

## <a name="additional-resources"></a>Recursos adicionales

- [Introducción a la programación web de ASP.NET mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkID=202890)

> [!div class="step-by-step"]
> [Anterior](intro-to-web-pages-programming.md)
> [Siguiente](form-basics.md)
