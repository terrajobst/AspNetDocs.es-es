---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: Introducción a ASP.NET Web Pages-HTML Forms | Microsoft Docs
author: Rick-Anderson
description: En este tutorial se muestran los aspectos básicos de cómo crear un formulario de entrada y cómo controlar la entrada del usuario cuando se usa ASP.NET Web Pages (Razor). Y ahora...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: f57661077ec3bb13f3d4ec41b130bda4d2fb9070
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78463543"
---
# <a name="introducing-aspnet-web-pages---html-form-basics"></a>Introducción a ASP.NET Web Pages: conceptos básicos de formularios HTML

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este tutorial se muestran los aspectos básicos de cómo crear un formulario de entrada y cómo controlar la entrada del usuario cuando se usa ASP.NET Web Pages (Razor). Y ahora que tiene una base de datos, usará sus conocimientos para que los usuarios puedan encontrar películas específicas en la base de datos. Se supone que ha completado la serie a través [de la introducción a la visualización de datos mediante ASP.NET Web pages](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data).
> 
> Temas que se abordarán:
> 
> - Cómo crear un formulario mediante elementos HTML estándar.
> - Cómo leer la entrada del usuario en un formulario.
> - Cómo crear una consulta SQL que obtiene datos de forma selectiva mediante un término de búsqueda que el usuario proporciona.
> - Cómo hacer que los campos de la página "recuerden" lo que el usuario ha escrito.
>   
> 
> Características y tecnologías descritas:
> 
> - Objeto `Request`.
> - La cláusula SQL `Where`.

## <a name="what-youll-build"></a>Lo que creará

En el tutorial anterior, creó una base de datos, agregó datos a ella y, a continuación, usó el ayudante de `WebGrid` para mostrar los datos. En este tutorial, agregará un cuadro de búsqueda que le permitirá buscar películas de un género específico o cuyo título contenga cualquier palabra que especifique. (Por ejemplo, podrá buscar todas las películas cuyo género sea "acción" o cuyo título contenga "Harry" o "Adventure").

Cuando haya terminado este tutorial, tendrá una página como esta:

![Página de películas con el género y la búsqueda de títulos](form-basics/_static/image1.png)

La parte de lista de la página es la misma que en el último tutorial &mdash; una cuadrícula. La diferencia será que la cuadrícula mostrará solo las películas que buscó.

## <a name="about-html-forms"></a>Acerca de los formularios HTML

(Si tiene experiencia en la creación de formularios HTML y la diferencia entre `GET` y `POST`, puede omitir esta sección).

Un formulario tiene elementos de entrada de usuario &mdash; cuadros de texto, botones, botones de radio, casillas, listas desplegables, etc. Los usuarios rellenan estos controles o realizan selecciones y después envían el formulario haciendo clic en un botón.

La sintaxis HTML básica de un formulario se muestra en este ejemplo:

[!code-html[Main](form-basics/samples/sample1.html)]

Cuando este marcado se ejecuta en una página, crea un formulario simple que se parece a esta ilustración:

![Formulario HTML básico tal y como se representa en el explorador](form-basics/_static/image2.png)

El elemento `<form>` incluye los elementos HTML que se van a enviar. (Un error sencillo de hacer es agregar elementos a la página pero, a continuación, olvidar colocarlos dentro de un elemento `<form>`. En ese caso, no se envía nada). El atributo `method` indica al explorador cómo enviar la entrada del usuario. Establezca este valor en `post` si está realizando una actualización en el servidor o en `get` si solo está obteniendo datos del servidor.

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> **Seguridad de los verbos GET, POST y HTTP**
> 
> HTTP, el protocolo que usan los exploradores y servidores para intercambiar información, es muy sencillo en sus operaciones básicas. Los exploradores usan solo unos cuantos verbos para hacer solicitudes a los servidores. Al escribir código para la web, resulta útil comprender estos verbos y cómo los usan el explorador y el servidor. Los verbos que se usan con más frecuencia son los siguientes:
> 
> - `GET`Operador El explorador utiliza este verbo para capturar algo del servidor. Por ejemplo, al escribir una dirección URL en el explorador, el explorador realiza una operación `GET` para solicitar la página que desee. Si la página incluye gráficos, el explorador realiza operaciones de `GET` adicionales para obtener las imágenes. Si la operación de `GET` tiene que pasar información al servidor, la información se pasa como parte de la dirección URL en la cadena de consulta.
> - `POST`Operador El explorador envía una solicitud de `POST` para enviar los datos que se van a agregar o cambiar en el servidor. Por ejemplo, el verbo `POST` se usa para crear registros en una base de datos o para cambiar los existentes. La mayoría de las veces, al rellenar un formulario y hacer clic en el botón Enviar, el explorador realiza una operación de `POST`. En una operación de `POST`, los datos que se pasan al servidor se encontraban en el cuerpo de la página.
> 
> Una diferencia importante entre estos verbos es que una operación de `GET` no se supone para cambiar nada en el servidor (o para colocarlo de una manera ligeramente más abstracta, una operación de `GET` no produce un cambio de estado en el servidor. Puede realizar una operación de `GET` en los mismos recursos tantas veces como desee, y esos recursos no cambian. (A menudo se dice que una operación `GET` es "segura" o usar un término técnico, es *idempotente*). Por el contrario, un `POST` solicitud cambia algo en el servidor cada vez que se realiza la operación.
> 
> Dos ejemplos le ayudarán a ilustrar esta distinción. Cuando realice una búsqueda con un motor como Bing o Google, rellene un formulario compuesto por un cuadro de texto y, a continuación, haga clic en el botón Buscar. El explorador realiza una operación de `GET`, con el valor especificado en el cuadro que se pasa como parte de la dirección URL. El uso de una operación de `GET` para este tipo de formulario es correcto, ya que una operación de búsqueda no cambia ningún recurso del servidor, sino que simplemente captura la información.
> 
> Ahora considere el proceso de ordenar algo en línea. Rellene los detalles del pedido y, a continuación, haga clic en el botón Enviar. Esta operación será una solicitud de `POST`, porque la operación provocará cambios en el servidor, como un nuevo registro de pedido, un cambio en la información de la cuenta y quizás muchos otros cambios. A diferencia de la operación de `GET`, no puede repetir la solicitud de `POST`; si lo hizo, cada vez que reenvió la solicitud, debe generar un nuevo pedido en el servidor. (En casos como este, los sitios web a menudo le avisarán de que no hace clic más de una vez en un botón de envío o deshabilitará el botón Enviar para que no vuelva a enviar el formulario accidentalmente).
> 
> En el transcurso de este tutorial, usará una operación de `GET` y una operación de `POST` para trabajar con formularios HTML. En cada caso, explicaremos por qué el verbo que se usa es el adecuado.
> 
> (Para obtener más información sobre los verbos HTTP, vea el artículo [definiciones de métodos](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) en el sitio de W3C).

La mayoría de los elementos de entrada del usuario son elementos HTML `<input>`. Tienen el siguiente aspecto `<input type="type" name="name">,` donde *tipo* indica el tipo de control de entrada de usuario que desea. Estos elementos son los comunes:

- Cuadro de texto: `<input type="text">`
- Casilla: `<input type="check">`
- Botón de radio: `<input type="radio">`
- Botón: `<input type="button">`
- Botón Enviar: `<input type="submit">`

También puede usar el elemento `<textarea>` para crear un cuadro de texto multilínea y el elemento `<select>` para crear una lista desplegable o una lista desplazable. (Para obtener más información sobre los elementos de formulario HTML, vea [HTML Forms and INPUT](http://www.w3schools.com/html/html_forms.asp) in the w3schools site).

El atributo `name` es muy importante, ya que el nombre es cómo obtendrá más adelante el valor del elemento, como veremos en breve.

La parte interesante es lo que usted, el desarrollador de páginas, hacen con la entrada del usuario. No hay ningún comportamiento integrado asociado a estos elementos. En su lugar, tiene que obtener los valores que el usuario ha escrito o seleccionado y hacer algo con ellos. Eso es lo que aprenderá en este tutorial.

> [!TIP] 
> 
> **HTML5 y formularios de entrada**
> 
> Como podría saber, HTML está en transición y la versión más reciente (HTML5) incluye compatibilidad con formas más intuitivas para que los usuarios escriban información. Por ejemplo, en HTML5, usted (el desarrollador de páginas) puede indicar a la página que desea que el usuario escriba una fecha. Después, el explorador puede mostrar automáticamente un calendario en lugar de solicitar al usuario que escriba una fecha manualmente. Sin embargo, HTML5 es nuevo y aún no se admite en todos los exploradores.
> 
> ASP.NET Web Pages admite la entrada de HTML5 en la medida en que lo hace el explorador del usuario. Para obtener una idea de los nuevos atributos del elemento `<input>` en HTML5, consulte [HTML &lt;input&gt; Attribute Type](http://www.w3schools.com/html/html_form_input_types.asp) en el sitio w3schools.

## <a name="creating-the-form"></a>Crear el formulario

En WebMatrix, en el área de trabajo **archivos** , abra la página *movies. cshtml* .

Después de la etiqueta de `</h1>` de cierre y antes de la etiqueta de apertura `<div>` de la llamada a `grid.GetHtml`, agregue el marcado siguiente:

[!code-html[Main](form-basics/samples/sample2.html)]

Este marcado crea un formulario que tiene un cuadro de texto denominado `searchGenre` y un botón de envío. El cuadro de texto y el botón Enviar se incluyen en un elemento `<form>` cuyo atributo `method` está establecido en `get`. (Recuerde que si no coloca el cuadro de texto y el botón de envío dentro de un elemento `<form>`, no se enviará nada al hacer clic en el botón). Aquí puede usar el verbo `GET` porque está creando un formulario que no realiza ningún cambio en el servidor; simplemente se produce una búsqueda. (En el tutorial anterior, ha usado un método `post`, que es cómo se envían los cambios al servidor. Verá que en el siguiente tutorial de nuevo).

Ejecute la página. Aunque no haya definido ningún comportamiento para el formulario, puede ver su aspecto:

![Página de películas con el cuadro de búsqueda para el género](form-basics/_static/image3.png)

Escriba un valor en el cuadro de texto, como "comedia". A continuación, haga clic en **Buscar género**.

Tome nota de la dirección URL de la página. Dado que se establece el atributo `method` del elemento de `<form>` en `get`, el valor especificado ahora forma parte de la cadena de consulta en la dirección URL, de la siguiente manera:

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a>Leer valores de formulario

La página ya contiene código que obtiene datos de la base de datos y muestra los resultados en una cuadrícula. Ahora tiene que agregar código que lee el valor del cuadro de texto para que pueda ejecutar una consulta SQL que incluya el término de búsqueda.

Dado que el método del formulario se establece en `get`, puede leer el valor que se especificó en el cuadro de texto mediante código como el siguiente:

`var searchTerm = Request.QueryString["searchGenre"];`

El objeto `Request.QueryString` (la propiedad `QueryString` del objeto `Request`) incluye los valores de los elementos que se enviaron como parte de la operación de `GET`. La propiedad `Request.QueryString` contiene una *colección* (una lista) de los valores que se envían en el formulario. Para obtener cualquier valor individual, especifique el nombre del elemento que desee. Este es el motivo por el que tiene un atributo `name` en el elemento `<input>` (`searchTerm`) que crea el cuadro de texto. (Para obtener más información sobre el objeto `Request`, consulte la [barra lateral](#BKMK_TheRequestObject) más adelante).

Es bastante sencillo leer el valor del cuadro de texto. Pero si el usuario no especificó nada en el cuadro de texto pero hizo clic en **Buscar** de todas formas, puede omitir ese clic, ya que no hay nada que buscar.

El código siguiente es un ejemplo que muestra cómo implementar estas condiciones. (No tiene que agregar este código todavía; lo hará en un momento).

[!code-csharp[Main](form-basics/samples/sample3.cs)]

La prueba se interrumpe de esta manera:

- Obtiene el valor de `Request.QueryString["searchGenre"]`, es decir, el valor que se especificó en el elemento de `<input>` denominado `searchGenre`.
- Averigüe si está vacío con el método `IsEmpty`. Este método es la manera estándar de determinar si algo (por ejemplo, un elemento de formulario) contiene un valor. Pero en realidad solo le interesa si *no* está vacío, por lo tanto,...
- Agregue el operador `!` delante de la prueba de `IsEmpty`. (El operador `!` significa no lógico).

En inglés, toda la condición de `if` se traduce en lo siguiente: *si el elemento searchGenre del formulario no está vacío,..* .

Este bloque establece la fase de creación de una consulta que usa el término de búsqueda. Lo hará en la sección siguiente.

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> **El objeto de solicitud**
> 
> El objeto `Request` contiene toda la información que el explorador envía a la aplicación cuando se solicita o se envía una página. Este objeto incluye toda la información que proporciona el usuario, como los valores de cuadro de texto o un archivo que se va a cargar. También incluye todo tipo de información adicional, como las cookies, los valores de la cadena de consulta de dirección URL (si existe), la ruta de acceso de archivo de la página que se está ejecutando, el tipo de explorador que usa el usuario, la lista de idiomas que se establecen en el explorador. y mucho más.
> 
> El objeto `Request` es una *colección* (lista) de valores. Obtiene un valor individual de la colección especificando su nombre:
> 
> `var someValue = Request["name"];`
> 
> En realidad, el objeto `Request` expone varios subconjuntos. Por ejemplo:
> 
> - `Request.Form` proporciona valores de elementos dentro del elemento de `<form>` enviado si la solicitud es una solicitud de `POST`.
> - `Request.QueryString` proporciona solo los valores de la cadena de consulta de la dirección URL. (En una dirección URL como `http://mysite/myapp/page?searchGenre=action&page=2`, la sección `?searchGenre=action&page=2` de la dirección URL es la cadena de consulta).
> - `Request.Cookies` colección le proporciona acceso a las cookies enviadas por el explorador.
> 
> Para obtener un valor que sepa que está en el formulario enviado, puede usar `Request["name"]`. Como alternativa, puede usar las versiones más específicas `Request.Form["name"]` (para solicitudes de `POST`) o `Request.QueryString["name"]` (para solicitudes de `GET`). Por supuesto, *Name* es el nombre del elemento que se va a obtener.
> 
> El nombre del elemento que quiere obtener debe ser único dentro de la colección que está usando. Este es el motivo por el que el objeto `Request` proporciona los subconjuntos como `Request.Form` y `Request.QueryString`. Supongamos que la página contiene un elemento de formulario denominado `userName` y que *también* contiene una cookie denominada `userName`. Si obtiene `Request["userName"]`, es ambiguo si desea el valor del formulario o la cookie. Sin embargo, si obtiene `Request.Form["userName"]` o `Request.Cookie["userName"]`, es explícito sobre qué valor obtener.
> 
> Es recomendable ser específico y usar el subconjunto de `Request` que le interesa, como `Request.Form` o `Request.QueryString`. En el caso de las páginas simples que va a crear en este tutorial, es probable que no haga realmente ninguna diferencia. Sin embargo, a medida que crea páginas más complejas, el uso de la versión explícita `Request.Form` o `Request.QueryString` puede ayudarle a evitar problemas que pueden surgir cuando la página contiene un formulario (o varios formularios), cookies, valores de cadena de consulta, etc.

## <a name="creating-a-query-by-using-a-search-term"></a>Crear una consulta mediante un término de búsqueda

Ahora que sabe cómo obtener el término de búsqueda que ha escrito el usuario, puede crear una consulta que lo use. Recuerde que, para obtener todos los elementos de la película de la base de datos, está usando una consulta SQL que tiene el siguiente aspecto:

`SELECT * FROM Movies`

Para obtener solo determinadas películas, tiene que usar una consulta que incluya una cláusula `Where`. Esta cláusula permite establecer una condición en la que la consulta devuelve las filas. Por ejemplo:

`SELECT * FROM Movies WHERE Genre = 'Action'`

El formato básico es `WHERE column = value`. Puede usar operadores diferentes además de `=`, como `>` (mayor que), `<` (menor que), `<>` (no es igual a), `<=` (menor o igual que), etc., en función de lo que esté buscando.

En caso de que se pregunte, las instrucciones SQL no distinguen mayúsculas de minúsculas &mdash; `SELECT` es igual que `Select` (o incluso `select`). Sin embargo, las personas suelen poner en mayúscula palabras clave en una instrucción SQL, como `SELECT` y `WHERE`, para facilitar la lectura.

### <a name="passing-the-search-term-as-a-parameter"></a>Pasar el término de búsqueda como parámetro

La búsqueda de un género específico es lo suficientemente sencilla (`WHERE Genre = 'Action'`), pero desea poder buscar cualquier género que escriba el usuario. Para ello, cree como consulta SQL que incluya un marcador de posición para el valor que se va a buscar. Tendrá un aspecto similar a este comando:

`SELECT * FROM Movies WHERE Genre = @0`

El marcador de posición es el carácter `@` seguido de cero. Como puede imaginar, una consulta puede contener varios marcadores de posición y se les denomina `@0`, `@1`, `@2`, etc.

Para configurar la consulta y, en realidad, pasarle el valor, use el código similar al siguiente:

[!code-sql[Main](form-basics/samples/sample4.sql)]

Este código es similar a lo que ya ha hecho para mostrar los datos en la cuadrícula. Las únicas diferencias son:

- La consulta contiene un marcador de posición (`WHERE Genre = @0"`).
- La consulta se coloca en una variable (`selectCommand`); antes, pasó la consulta directamente al método `db.Query`.
- Cuando se llama al método `db.Query`, se pasa la consulta y el valor que se van a utilizar para el marcador de posición. (Si la consulta tuviera varios marcadores de posición, los pasaría todos como valores independientes al método).

Si coloca todos estos elementos juntos, obtendrá el código siguiente:

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> **¡Importante!** El uso de marcadores de posición (como `@0`) para pasar valores a un comando SQL es *extremadamente importante* para la seguridad. La forma en que se ve aquí, con marcadores de posición para datos variables, es la única manera en que debe construir comandos SQL.
> 
> No construya nunca una instrucción SQL colocando (concatenando) texto literal y valores que obtiene del usuario. La concatenación de los datos proporcionados por el usuario en una instrucción SQL abre el sitio en un *ataque de inyección de SQL* en el que un usuario malintencionado envía valores a la página para atacar la base de datos. (Puede obtener más información en el artículo [inyección de SQL](https://msdn.microsoft.com/library/ms161953.aspx) en el sitio web de MSDN).

## <a name="updating-the-movies-page-with-search-code"></a>Actualización de la página de películas con el código de búsqueda

Ahora puede actualizar el código en el archivo *movies. cshtml* . Para empezar, reemplace el código del bloque de código situado en la parte superior de la página con este código:

[!code-csharp[Main](form-basics/samples/sample6.cs)]

La diferencia es que ha colocado la consulta en la `selectCommand` variable, que pasará a `db.Query` más adelante. La colocación de la instrucción SQL en una variable permite cambiar la instrucción, que es lo que hará para realizar la búsqueda.

También ha quitado estas dos líneas, que volverá a colocar más adelante:

[!code-csharp[Main](form-basics/samples/sample7.cs)]

No desea ejecutar la consulta todavía (es decir, llamar a `db.Query`) y no desea inicializar la aplicación auxiliar de `WebGrid` todavía. Podrá hacerlo una vez que haya descubierto qué instrucción SQL tiene que ejecutar.

Después de este bloque reescrito, puede Agregar la nueva lógica para controlar la búsqueda. El código completado tendrá un aspecto similar al siguiente. Actualice el código de la página para que coincida con este ejemplo:

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

La página ahora funciona como esta. Cada vez que se ejecuta la página, el código abre la base de datos y la variable `selectCommand` se establece en la instrucción SQL que obtiene todos los registros de la tabla `Movies`. El código también inicializa la variable `searchTerm`.

Sin embargo, si la solicitud actual incluye un valor para el elemento `searchGenre`, el código establece `selectCommand` en una consulta diferente (es decir, en una que incluye la cláusula `Where` para buscar un género). También establece `searchTerm` en el que se pasó para el cuadro de búsqueda (que podría no ser nada).

Independientemente de la instrucción SQL que se encuentra en `selectCommand`, el código llama a `db.Query` para ejecutar la consulta y pasarle lo que esté en `searchTerm`. Si no hay nada en `searchTerm`, no importa, porque en ese caso no hay ningún parámetro para pasar el valor a `selectCommand` de todos modos.

Por último, el código inicializa la aplicación auxiliar de `WebGrid` con los resultados de la consulta, como antes.

Puede ver que al colocar la instrucción SQL y el término de búsqueda en variables, ha agregado flexibilidad al código. Como verá más adelante en este tutorial, puede usar este marco de trabajo básico y seguir agregando lógica para los diferentes tipos de búsquedas.

## <a name="testing-the-search-by-genre-feature"></a>Probar la característica de búsqueda por género

En WebMatrix, ejecute la página *movies. cshtml* . Verá la página con el cuadro de texto de genre.

Escriba un género que haya especificado para uno de los registros de prueba y, a continuación, haga clic en **Buscar**. Esta vez verá una lista de las películas que coinciden con ese género:

![Lista de páginas de películas después de buscar el género ' Comedies '](form-basics/_static/image4.png)

Escriba otro género y busque de nuevo. Pruebe a escribir el género con todas las letras en minúsculas o todas mayúsculas para que pueda ver que la búsqueda no distingue mayúsculas de minúsculas.

## <a name="remembering-what-the-user-entered"></a>"Recordar" lo que el usuario escribió

Es posible que haya observado que, después de haber escrito un género y de hacer clic en el **género buscar**, vio una lista para ese género. Sin embargo, el cuadro de texto de búsqueda estaba vacío &mdash; en otras palabras, no recuerda lo que especificó.

Es importante comprender por qué se produce este comportamiento. Cuando se envía una página, el explorador envía una solicitud al servidor Web. Cuando ASP.NET obtiene la solicitud, crea una nueva instancia de la página, ejecuta el código en ella y, a continuación, vuelve a representar la página en el explorador. En efecto, sin embargo, la página no sabe que acaba de trabajar con una versión anterior de sí mismo. Todo lo que sabe es que obtuvo una solicitud que contenía algunos datos del formulario.

Cada vez que solicita una página &mdash; ya sea por primera vez o enviándola &mdash; está obteniendo una nueva página. El servidor Web no tiene memoria de la última solicitud. Ni ASP.NET ni el explorador. La única conexión entre estas instancias independientes de la página son los datos que se transmiten entre ellas. Si envía una página, por ejemplo, la nueva instancia de la página puede obtener los datos de formulario enviados por la instancia anterior. (Otra manera de pasar datos entre páginas es usar cookies).

Una manera formal de describir esta situación es decir que las páginas web no tienen *Estado*. Los servidores web y las páginas y los elementos de la página no mantienen ninguna información sobre el estado anterior de una página. La web se diseñó de esta manera porque el mantenimiento del estado de las solicitudes individuales agotaría rápidamente los recursos de los servidores Web, que a menudo administran miles, quizás incluso cientos de miles de solicitudes por segundo.

Por eso el cuadro de texto estaba vacío. Después de enviar la página, ASP.NET creó una nueva instancia de la página y ejecutó el código y el marcado. No había nada en ese código que indicara a ASP.NET que colocara un valor en el cuadro de texto. Por lo tanto, ASP.NET no hace nada y el cuadro de texto se representó sin un valor en él.

En realidad, hay una manera fácil de solucionar este problema. El género que escribió en el cuadro de texto *está* disponible en el código &mdash; está en `Request.QueryString["searchGenre"]`.

Actualice el marcado del cuadro de texto para que el `value` atributo obtenga su valor de `searchTerm`, como en este ejemplo:

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

En esta página, también podría haber establecido el atributo `value` en la variable `searchTerm`, ya que esa variable también contiene el género que ha escrito. Pero usar el objeto `Request` para establecer el atributo `value` como se muestra aquí es la manera estándar de llevar a cabo esta tarea. (Suponiendo que incluso desea hacer esto &mdash; en algunas situaciones, puede que desee representar la página *sin* valores en los campos. Todo depende de lo que ocurre con la aplicación).

> [!NOTE]
> No se puede "recordar" el valor de un cuadro de texto que se usa para las contraseñas. Sería un agujero de seguridad para permitir que los usuarios rellenen un campo de contraseña mediante el uso de código.

Vuelva a ejecutar la página, escriba un género y haga clic en **Buscar género**. Esta vez, no solo verá los resultados de la búsqueda, pero el cuadro de texto recuerda lo que escribió la última vez:

![Página que muestra que el cuadro de texto tiene ' recordado ' en la entrada anterior](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a>Buscar cualquier palabra en el título

Ahora puede buscar cualquier género, pero también podría querer buscar un título. Es difícil obtener un título exactamente a la derecha al buscar, de modo que puede buscar una palabra que aparece en cualquier parte dentro de un título. Para ello en SQL, use el operador de `LIKE` y la sintaxis como la siguiente:

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

Este comando obtiene todas las películas cuyos títulos contienen "Adventure". Cuando se usa el operador `LIKE`, se incluye el carácter comodín `%` como parte del término de búsqueda. La `LIKE 'adventure%'` de búsqueda significa "comenzar por" Adventure ". (Técnicamente, significa "la cadena ' Adventure ' seguida de cualquier cosa). Del mismo modo, el término de búsqueda `LIKE '%adventure'` significa "cualquier cosa seguido por la cadena ' Adventure '", que es otra manera de decir "finalizar con ' Adventure '".

Por lo tanto, el término de búsqueda `LIKE '%adventure%'` significa "con ' Adventure ' en cualquier parte del título". (Técnicamente, "todo en el título, seguido de ' Adventure ', seguido de cualquier cosa).

Dentro del elemento `<form>`, agregue el siguiente marcado justo debajo de la etiqueta de cierre `</div>` de la búsqueda de género (justo antes del elemento `</form>` de cierre):

[!code-html[Main](form-basics/samples/sample10.html)]

El código para controlar esta búsqueda es similar al código de la búsqueda de género, salvo que tiene que ensamblar la `LIKE` búsqueda. Dentro del bloque de código situado en la parte superior de la página, agregue este bloque `if` justo después del bloque de `if` de la búsqueda de género:

[!code-csharp[Main](form-basics/samples/sample11.cs)]

Este código usa la misma lógica que vio anteriormente, salvo que la búsqueda usa un operador `LIKE` y el código coloca "`%`" antes y después del término de búsqueda.

Observe cómo era fácil agregar otra búsqueda a la página. Lo único que debía hacer era:

- Cree un bloque `if` que se probó para ver si el cuadro de búsqueda pertinente tenía un valor.
- Establezca la variable de `selectCommand` en una nueva instrucción SQL.
- Establezca la variable `searchTerm` en el valor que se va a pasar a la consulta.

Este es el bloque de código completo, que contiene la nueva lógica para una búsqueda de título:

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

A continuación se muestra un resumen de lo que hace este código:

- Las variables `searchTerm` y `selectCommand` se inicializan en la parte superior. Va a establecer estas variables en el término de búsqueda adecuado (si existe) y el comando SQL adecuado en función de lo que hace el usuario en la página. La búsqueda predeterminada es el caso simple de obtener todas las películas de la base de datos.
- En las pruebas de `searchGenre` y `searchTitle`, el código establece `searchTerm` en el valor que desea buscar. Estos bloques de código también establecen `selectCommand` en un comando SQL adecuado para esa búsqueda.
- El método `db.Query` se invoca solo una vez, utilizando el comando SQL que se encuentra en `selectedCommand` y cualquier valor que esté en `searchTerm`. Si no hay ningún término de búsqueda (ningún género y ninguna palabra de título), el valor de `searchTerm` es una cadena vacía. Sin embargo, esto no importa, porque en ese caso la consulta no requiere un parámetro.

## <a name="testing-the-title-search-feature"></a>Probar la característica de búsqueda de títulos

Ahora puede probar la página de búsqueda completada. Ejecute *movies. cshtml*.

Escriba un género y haga clic en **Buscar género**. La cuadrícula muestra las películas de ese género, como antes.

Escriba una palabra de título y haga clic en **Buscar título**. La cuadrícula muestra las películas que tienen esa palabra en el título.

![Lista de páginas de películas después de buscar ' The ' en el título](form-basics/_static/image6.png)

Deje ambos cuadros de texto en blanco y haga clic en cualquiera de los botones. La cuadrícula muestra todas las películas.

## <a name="combining-the-queries"></a>Combinar las consultas

Es posible que observe que las búsquedas que puede realizar son exclusivas. No se puede buscar el título y el género al mismo tiempo, aunque ambos cuadros de búsqueda tengan valores en ellos. Por ejemplo, no puede buscar todas las películas de acción cuyo título contiene "Adventure". (Como la página se codifica ahora, si escribe valores para el género y el título, la búsqueda del título tiene prioridad). Para crear una búsqueda que combine las condiciones, tendría que crear una consulta SQL que tenga una sintaxis similar a la siguiente:

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

Y tendría que ejecutar la consulta mediante una instrucción similar a la siguiente (aproximadamente hablando):

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

Crear lógica para permitir muchas permutaciones de criterios de búsqueda puede ser un poco implicado, como puede ver. Por lo tanto, se detendremos aquí.

## <a name="coming-up-next"></a>Próxima

En el siguiente tutorial, creará una página que utiliza un formulario para que los usuarios puedan agregar películas a la base de datos.

## <a name="complete-listing-for-movie-page-updated-with-search"></a>Lista completa de la página de la película (actualizada con la búsqueda)

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a>Recursos adicionales

- [Introducción a la programación web de ASP.NET mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Cláusula WHERE de SQL](http://www.w3schools.com/sql/sql_where.asp) en el sitio de w3schools
- Artículo [definiciones de métodos](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) en el sitio de W3C

> [!div class="step-by-step"]
> [Anterior](displaying-data.md)
> [Siguiente](entering-data.md)
