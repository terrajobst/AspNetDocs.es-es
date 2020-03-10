---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
title: Presentación de ASP.NET Web Pages-escribir datos de base de datos mediante formularios | Microsoft Docs
author: Rick-Anderson
description: En este tutorial se muestra cómo crear un formulario de entrada y, a continuación, especificar los datos que se obtienen del formulario en una tabla de base de datos cuando se usa ASP.NET Web Pages (...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: d37c93fc-25fd-4e94-8671-0d437beef206
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
msc.type: authoredcontent
ms.openlocfilehash: b9354a7b97a7df9020a681f709e16a92650cfcf0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78506605"
---
# <a name="introducing-aspnet-web-pages---entering-database-data-by-using-forms"></a>Introducción a ASP.NET Web Pages-escribir datos de base de datos mediante formularios

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este tutorial se muestra cómo crear un formulario de entrada y, a continuación, especificar los datos que se obtienen del formulario en una tabla de base de datos cuando se usa ASP.NET Web Pages (Razor). Se supone que ha completado la serie a través [de los aspectos básicos de los formularios HTML en ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=251581).
> 
> Temas que se abordarán:
> 
> - Más información sobre cómo procesar los formularios de entrada.
> - Cómo agregar (insertar) datos en una base de datos.
> - Cómo asegurarse de que los usuarios han escrito un valor obligatorio en un formulario (cómo validar los datos proporcionados por el usuario).
> - Cómo mostrar los errores de validación.
> - Cómo saltar a otra página de la página actual.
>   
> 
> Características y tecnologías descritas:
> 
> - El método `Database.Execute` .
> - Instrucción de `Insert Into` de SQL
> - Aplicación auxiliar de `Validation`.
> - El método `Response.Redirect` .

## <a name="what-youll-build"></a>Lo que creará

En el tutorial anterior que mostró cómo crear una base de datos, especificó la base de datos mediante la edición de la base de datos directamente en WebMatrix, trabajando en el área de trabajo de la **base** de datos. Sin embargo, en la mayoría de las aplicaciones, no es una forma práctica de colocar datos en la base de datos. Por lo tanto, en este tutorial, creará una interfaz basada en Web que le permite escribir datos y guardarlos en la base de datos.

Creará una página donde podrá escribir nuevas películas. La página contendrá un formulario de entrada que contiene campos (cuadros de texto) en los que puede especificar el título, el género y el año de una película. La página se parecerá a esta página:

![Página ' agregar película ' en el explorador](entering-data/_static/image1.png)

Los cuadros de texto serán HTML `<input>` elementos que serán similares a este marcado:

`<input type="text" name="genre" value="" />`

## <a name="creating-the-basic-entry-form"></a>Crear el formulario de entrada básico

Cree una página denominada *AddMovie. cshtml*.

Reemplace el contenido del archivo por el marcado siguiente. Sobrescribir todo; en breve, agregará un bloque de código.

[!code-cshtml[Main](entering-data/samples/sample1.cshtml)]

En este ejemplo se muestra HTML típico para crear un formulario. Utiliza `<input>` elementos para los cuadros de texto y para el botón de envío. Los títulos de los cuadros de texto se crean mediante los elementos de `<label>` estándar. Los elementos `<fieldset>` y `<legend>` colocan un cuadro muy bonito alrededor del formulario.

Observe que en esta página, el elemento `<form>` usa `post` como valor para el atributo `method`. En el tutorial anterior, creó un formulario que usaba el método `get`. Eso era correcto, porque aunque el formulario envió valores al servidor, la solicitud no realizaba ningún cambio. Todo lo que hacía fue capturar datos de maneras diferentes. Sin embargo, en esta página *realizará* cambios: va a agregar nuevos registros de base de datos. Por lo tanto, este formulario debe utilizar el método `post`. (Para obtener más información sobre la diferencia entre las operaciones de `GET` y `POST`, vea la barra lateral de[seguridad del verbo GET, post y http](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety) en el tutorial anterior).

Tenga en cuenta que cada cuadro de texto tiene un elemento `name` (`title`, `genre`, `year`). Como vio en el tutorial anterior, estos nombres son importantes porque debe tener esos nombres para poder obtener la entrada del usuario más adelante. Puede usar cualquier nombre. Resulta útil usar nombres significativos que le ayuden a recordar los datos con los que está trabajando.

El `value` atributo de cada elemento `<input>` contiene un bit de código Razor (por ejemplo, `Request.Form["title"]`). Ha aprendido una versión de este truco en el tutorial anterior para conservar el valor especificado en el cuadro de texto (si existe) después de enviar el formulario.

## <a name="getting-the-form-values"></a>Obtener los valores del formulario

A continuación, agregue el código que procesa el formulario. En esquema, hará lo siguiente:

1. Compruebe si la página se está publicando (se envió). Desea que el código se ejecute solo cuando los usuarios hayan pulsado el botón, no cuando se ejecute la página por primera vez.
2. Obtiene los valores que el usuario especificó en los cuadros de texto. En este caso, dado que el formulario utiliza el verbo `POST`, obtendrá los valores del formulario de la colección `Request.Form`.
3. Inserte los valores como un nuevo registro en la tabla de la base de datos de *películas* .

En la parte superior del archivo, agregue el siguiente código:

[!code-cshtml[Main](entering-data/samples/sample2.cshtml)]

Las primeras líneas crean variables (`title`, `genre`y `year`) para contener los valores de los cuadros de texto. La `if(IsPost)` de línea garantiza que las variables se establecen *solo* cuando los usuarios hacen clic en el botón **Agregar película** , es decir, cuando se ha publicado el formulario.

Como vimos en un tutorial anterior, obtiene el valor de un cuadro de texto mediante una expresión como `Request.Form["name"]`, donde *nombre* es el nombre del elemento `<input>`.

Los nombres de las variables (`title`, `genre`y `year`) son arbitrarios. Al igual que los nombres que se asignan a los elementos de `<input>`, puede llamarlos como desee. (Los nombres de las variables no tienen que coincidir con los atributos de nombre de los elementos `<input>` del formulario). Sin embargo, al igual que con los elementos `<input>`, se recomienda usar nombres de variable que reflejen los datos que contienen. Al escribir código, los nombres coherentes facilitan la tarea de recordar los datos con los que está trabajando.

## <a name="adding-data-to-the-database"></a>Agregar datos a la base de datos

En el bloque de código que acaba de agregar, justo *dentro* de la llave de cierre (`}`) del bloque de `if` (no solo dentro del bloque de código), agregue el código siguiente:

[!code-csharp[Main](entering-data/samples/sample3.cs)]

Este ejemplo es similar al código que usó en un tutorial anterior para capturar y Mostrar datos. La línea que comienza por `db =` abre la base de datos, como antes, y la línea siguiente define una instrucción SQL, de nuevo como vio antes. Sin embargo, esta vez define una instrucción de `Insert Into` de SQL. En el ejemplo siguiente se muestra la sintaxis general de la instrucción `Insert Into`:

`INSERT INTO table (column1, column2, column3, ...) VALUES (value1, value2, value3, ...)`

En otras palabras, se especifica la tabla en la que se va a insertar y, a continuación, se muestran las columnas que se van a insertar en y, a continuación, se muestran los valores que se van a insertar. (Como se indicó antes, SQL no distingue entre mayúsculas y minúsculas, pero algunas personas ponen en mayúsculas las palabras clave para facilitar la lectura del comando).

Las columnas que va a insertar en ya aparecen en el comando: `(Title, Genre, Year)`. La parte interesante es la forma de obtener los valores de los cuadros de texto en el `VALUES` parte del comando. En lugar de los valores reales, verá `@0`, `@1`y `@2`, que son marcadores de posición de los cursos. Al ejecutar el comando (en la línea `db.Execute`), se pasan los valores que se obtuvieron de los cuadros de texto.

**¡Importante!** Recuerde que la única manera de incluir los datos en línea por un usuario en una instrucción SQL es usar los marcadores de posición, tal como se ve aquí (`VALUES(@0, @1, @2)`). Si concatena los datos proporcionados por el usuario en una instrucción SQL, se abre a través de un ataque por inyección de SQL, como se explica en los [conceptos básicos de los formularios ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=251581) (el tutorial anterior).

Dentro del bloque `if`, agregue la siguiente línea después de la línea `db.Execute`:

[!code-css[Main](entering-data/samples/sample4.css)]

Una vez insertada la nueva película en la base de datos, esta línea le salta (redirige) a la página *películas* para que pueda ver la película que acaba de escribir. El operador `~` significa "raíz del sitio web". (El operador `~` solo funciona en páginas de ASP.NET, no en HTML en general).

El bloque de código completo es similar al de este ejemplo:

[!code-cshtml[Main](entering-data/samples/sample5.cshtml)]

## <a name="testing-the-insert-command-so-far"></a>Probar el comando INSERT (hasta ahora)

Todavía no está listo, pero ahora es un buen momento para probarlo.

En la vista de árbol de los archivos de WebMatrix, haga clic con el botón secundario en la página *AddMovie. cshtml* y, a continuación, haga clic en **iniciar en el explorador**.

![Página ' agregar película ' en el explorador](entering-data/_static/image2.png)

(Si termina con otra página en el explorador, asegúrese de que la dirección URL sea `http://localhost:nnnnn/AddMovie`), donde *nnnnn* es el número de puerto que está usando.

¿Ha recibido una página de error? Si es así, lea atentamente y asegúrese de que el código tenga exactamente lo que se ha indicado anteriormente.

Escriba una película con el formato &mdash; por ejemplo, use "ciudadano Kane", "series" y "1941". (O cualquier). A continuación, haga clic en **Agregar película**.

Si todo va bien, se le redirigirá a la página *películas* . Asegúrese de que la nueva película aparece en la lista.

![Página de películas que muestra la película recién agregada](entering-data/_static/image3.png)

## <a name="validating-user-input"></a>Validar los datos introducidos por el usuario

Vuelva a la página *AddMovie* o ejecútela de nuevo. Escriba otra película, pero esta vez, escriba solo el título &mdash; por ejemplo, escriba "Singin' in the Rain". A continuación, haga clic en **Agregar película**.

Se le redirigirá a la página de *películas* de nuevo. Puede encontrar la nueva película, pero está incompleta.

![Página de películas que muestra una nueva película a la que le faltan algunos valores](entering-data/_static/image4.png)

Cuando creó la tabla *películas* , dijo explícitamente que ninguno de los campos podría ser null. Aquí tiene un formulario de entrada para nuevas películas y está saliendo de campos en blanco. Este es un error.

En este caso, la base de datos no producía (o *producía*) un error. No proporcionó ningún género ni año, por lo que el código de la página *AddMovie* trató esos valores como *cadenas vacías*. Cuando se ejecutó el comando de SQL `Insert Into`, los campos género y año no tenían datos útiles en ellos, pero no eran null.

Obviamente, no desea permitir que los usuarios escriban información de películas semivacías en la base de datos. La solución consiste en validar la entrada del usuario. Inicialmente, la validación simplemente se asegura de que el usuario ha escrito un valor para todos los campos (es decir, que ninguno de ellos contiene una cadena vacía).

> [!TIP]
> 
> **Cadenas nulas y vacías**
> 
> En programación, hay una diferencia entre las distintas nociones de "sin valor". En general, un valor es *null* si nunca se ha establecido o inicializado de ningún modo. En cambio, una variable que espera que los datos de caracteres (cadenas) se pueden establecer en una *cadena vacía*. En ese caso, el valor no es null; se acaba de establecer explícitamente en una cadena de caracteres cuya longitud es cero. Estas dos instrucciones muestran la diferencia:
> 
> [!code-csharp[Main](entering-data/samples/sample6.cs)]
> 
> Es un poco más complicado que eso, pero lo importante es que `null` representa un tipo de estado indeterminado.
> 
> Ahora, es importante comprender exactamente cuándo un valor es NULL y Cuándo es simplemente una cadena vacía. En el código de la página *AddMovie* , obtiene los valores de los cuadros de texto mediante `Request.Form["title"]`, etc. La primera vez que se ejecuta la página (antes de hacer clic en el botón), el valor de `Request.Form["title"]` es NULL. Sin embargo, al enviar el formulario, `Request.Form["title"]` obtiene el valor del cuadro de texto `title`. No es obvio, pero un cuadro de texto vacío no es NULL. simplemente tiene una cadena vacía. Por tanto, cuando el código se ejecuta en respuesta al clic del botón, `Request.Form["title"]` tiene una cadena vacía.
> 
> ¿Por qué es importante esta distinción? Cuando creó la tabla *películas* , dijo explícitamente que ninguno de los campos podría ser null. Pero aquí tiene un formulario de entrada para nuevas películas y está saliendo de campos en blanco. Cabría esperar razonablemente que la base de datos se quejase al intentar guardar nuevas películas que no tenían valores para el género o el año. Pero este es el punto &mdash; incluso si deja en blanco esos cuadros de texto, los valores no son NULL; son cadenas vacías. Como resultado, puede guardar nuevas películas en la base de datos con estas columnas vacías &mdash; pero no NULL. valores &mdash;. Por lo tanto, debe asegurarse de que los usuarios no envían una cadena vacía, lo que puede hacer mediante la validación de la entrada del usuario.

### <a name="the-validation-helper"></a>La aplicación auxiliar de validación

ASP.NET Web Pages incluye una aplicación auxiliar &mdash; la aplicación auxiliar `Validation` &mdash; que puede usar para asegurarse de que los usuarios escriban los datos que cumplan los requisitos. La aplicación auxiliar `Validation` es una de las aplicaciones auxiliares integradas en ASP.NET Web Pages, por lo que no tiene que instalarla como un paquete mediante NuGet, la forma en que instaló la aplicación auxiliar Gravatar en un tutorial anterior.

Para validar la entrada del usuario, haga lo siguiente:

- Use el código para especificar que desea requerir valores en los cuadros de texto de la página.
- Coloque una prueba en el código para que la información de la película se agregue a la base de datos solo si todo se valida correctamente.
- Agregue código en el marcado para mostrar los mensajes de error.

En el bloque de código de la página *AddMovie* , haga clic en la parte superior delante de las declaraciones de variables y agregue el código siguiente:

[!code-csharp[Main](entering-data/samples/sample7.cs)]

Llame a `Validation.RequireField` una vez por cada campo (elemento`<input>`) en el que desee solicitar una entrada. También puede Agregar un mensaje de error personalizado para cada llamada, como se ve aquí. (Hemos variado los mensajes para mostrar que puede colocar todo lo que le guste).

Si hay algún problema, quiere evitar que la nueva información de la película se inserte en la base de datos. En el bloque `if(IsPost)`, use `&&` (AND lógico) para agregar otra condición que pruebe `Validation.IsValid()`. Cuando haya terminado, todo el bloque de `if(IsPost)` se parecerá a este código:

[!code-csharp[Main](entering-data/samples/sample8.cs)]

Si hay un error de validación con cualquiera de los campos que registró mediante la aplicación auxiliar `Validation`, el método `Validation.IsValid` devuelve false. En ese caso, no se ejecutará ninguno de los códigos de ese bloque, por lo que no se insertará ninguna entrada de película no válida en la base de datos. Y, por supuesto, no se le redirigirá a la página *películas* .

El bloque de código completo, incluido el código de validación, ahora es similar a este ejemplo:

[!code-cshtml[Main](entering-data/samples/sample9.cshtml?highlight=10)]

## <a name="displaying-validation-errors"></a>Mostrar errores de validación

El último paso es mostrar los mensajes de error. Puede mostrar mensajes individuales para cada error de validación, o puede mostrar un resumen o ambos. En este tutorial, hará lo mismo para que pueda ver cómo funciona.

Junto a cada `<input>` elemento que está validando, llame al método `Html.ValidationMessage` y pásele el nombre del elemento `<input>` que está validando. Coloque el método de `Html.ValidationMessage` justo donde desee que aparezca el mensaje de error. Cuando se ejecuta la página, el método `Html.ValidationMessage` representa un elemento `<span>` en el que irá el error de validación. (Si no hay ningún error, el elemento `<span>` se representa, pero no hay texto en él).

Cambie el marcado en la página para que incluya un `Html.ValidationMessage` método para cada uno de los tres elementos `<input>` de la página, como en este ejemplo:

[!code-cshtml[Main](entering-data/samples/sample10.cshtml?highlight=3,8,13)]

Para ver cómo funciona el Resumen, agregue también el marcado y el código siguientes justo después del elemento `<h1>Add a Movie</h1>` en la página:

[!code-cshtml[Main](entering-data/samples/sample11.cshtml)]

De forma predeterminada, el método `Html.ValidationSummary` muestra todos los mensajes de validación de una lista (un elemento `<ul>` que se encuentra dentro de un elemento `<div>`). Al igual que con el método `Html.ValidationMessage`, el marcado para el Resumen de validación siempre se representa; Si no hay ningún error, no se representa ningún elemento de la lista.

El resumen puede ser una forma alternativa de mostrar los mensajes de validación en lugar de utilizar el método `Html.ValidationMessage` para mostrar cada error específico del campo. También puede usar un resumen y los detalles. O bien, puede usar el método `Html.ValidationSummary` para mostrar un error genérico y, a continuación, utilizar llamadas de `Html.ValidationMessage` individuales para mostrar los detalles.

La página completa ahora es similar a la de este ejemplo:

[!code-cshtml[Main](entering-data/samples/sample12.cshtml)]

Ya está. Ahora puede probar la página agregando una película pero saliendo de uno o varios de los campos. Cuando lo haga, verá la siguiente pantalla de error:

![Página agregar película que muestra mensajes de error de validación](entering-data/_static/image5.png)

## <a name="styling-the-validation-error-messages"></a>Aplicar estilo a los mensajes de error de validación

Puede ver que hay mensajes de error, pero que realmente no destacan bien. Sin embargo, hay una manera sencilla de aplicar estilo a los mensajes de error.

Para aplicar estilo a los mensajes de error individuales que se muestran en `Html.ValidationMessage`, cree una clase de estilo CSS denominada `field-validation-error`. Para definir el aspecto del Resumen de validación, cree una clase de estilo CSS denominada `validation-summary-errors`.

Para ver cómo funciona esta técnica, agregue un elemento `<style>` dentro de la sección `<head>` de la página. A continuación, defina las clases de estilo denominadas `field-validation-error` y `validation-summary-errors` que contengan las siguientes reglas:

[!code-cshtml[Main](entering-data/samples/sample13.cshtml?highlight=4-17)]

Normalmente, la información de estilo podría incluirse en un archivo *. CSS* independiente, pero para simplificar puede colocarlos en la página por ahora. (Más adelante en este tutorial se establecerán las reglas de CSS en un archivo *. CSS* independiente).

Si hay un error de validación, el método `Html.ValidationMessage` representa un elemento `<span>` que incluye `class="field-validation-error"`. Al agregar una definición de estilo para esa clase, puede configurar el aspecto del mensaje. Si hay errores, el método `ValidationSummary` también representa dinámicamente el atributo `class="validation-summary-errors"`.

Vuelva a ejecutar la página y deje deliberadamente un par de campos. Ahora, los errores son más evidentes. (De hecho, son overdone, pero eso es solo para mostrar lo que puede hacer).

![Página agregar película que muestra los errores de validación con estilo](entering-data/_static/image6.png)

## <a name="adding-a-link-to-the-movies-page"></a>Agregar un vínculo a la página películas

Un paso final es hacer que sea conveniente llegar a la página de *AddMovie* desde la lista de películas original.

Vuelva a abrir la página *películas* . Después de la etiqueta de cierre `</div>` que sigue a la aplicación auxiliar de `WebGrid`, agregue el siguiente marcado:

[!code-cshtml[Main](entering-data/samples/sample14.cshtml)]

Como vio antes, ASP.NET interpreta el operador `~` como la raíz del sitio Web. No tiene que utilizar el operador `~`; puede usar el marcado `<a href="./AddMovie">Add a movie</a>` o alguna otra forma de definir la ruta de acceso que HTML entiende. Pero el operador `~` es un buen enfoque general cuando se crean vínculos para las páginas de Razor, ya que hace que el sitio sea más flexible: si se mueve la página actual a una subcarpeta, el vínculo seguirá a la página *AddMovie* . (Recuerde que el operador de `~` solo funciona en páginas *. cshtml* . ASP.NET lo entiende, pero no es HTML estándar).

Cuando haya terminado, ejecute la página *películas* . Tendrá un aspecto similar al de esta página:

![Página de películas con vínculo a la página "agregar películas"](entering-data/_static/image7.png)

Haga clic en el vínculo **Agregar una película** para asegurarse de que va a la página *AddMovie* .

## <a name="coming-up-next"></a>Próxima

En el siguiente tutorial, obtendrá información sobre cómo permitir que los usuarios editen los datos que ya están en la base de datos.

## <a name="complete-listing-for-addmovie-page"></a>Lista completa de la página de AddMovie

[!code-cshtml[Main](entering-data/samples/sample15.cshtml)]

## <a name="additional-resources"></a>Recursos adicionales

- [Introducción a la programación web de ASP.NET mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Instrucción SQL INSERT INTO](http://www.w3schools.com/sql/sql_insert.asp) en el sitio de w3schools
- [Validación de la entrada del usuario en sitios de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=253002). Más información sobre cómo trabajar con la aplicación auxiliar de `Validation`.

> [!div class="step-by-step"]
> [Anterior](form-basics.md)
> [Siguiente](updating-data.md)
