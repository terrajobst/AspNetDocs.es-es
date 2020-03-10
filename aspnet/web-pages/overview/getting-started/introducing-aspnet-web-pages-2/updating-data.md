---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
title: Introducción a la actualización de datos de la base de datos de ASP.NET Web Pages | Microsoft Docs
author: Rick-Anderson
description: En este tutorial se muestra cómo actualizar (cambiar) una entrada de base de datos existente cuando se usa ASP.NET Web Pages (Razor). Se supone que ha completado la serie...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: ac86ec9c-6b69-485b-b9e0-8b9127b13e6b
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
msc.type: authoredcontent
ms.openlocfilehash: 8f8bcfb7d9d2416a2699776cadbdaae8e12415ba
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78463495"
---
# <a name="introducing-aspnet-web-pages---updating-database-data"></a>Introducción a la actualización de datos de base de datos de ASP.NET Web Pages

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este tutorial se muestra cómo actualizar (cambiar) una entrada de base de datos existente cuando se usa ASP.NET Web Pages (Razor). Se supone que ha completado la serie a través de la [especificación de datos mediante el uso de formularios ASP.NET Web pages](entering-data.md).
> 
> Temas que se abordarán:
> 
> - Cómo seleccionar un registro individual en la aplicación auxiliar de `WebGrid`.
> - Cómo leer un solo registro de una base de datos.
> - Cómo cargar previamente un formulario con valores del registro de base de datos.
> - Cómo actualizar un registro existente en una base de datos.
> - Cómo almacenar información en la página sin mostrarla.
> - Cómo usar un campo oculto para almacenar información.
>   
> 
> Características y tecnologías descritas:
> 
> - Aplicación auxiliar de `WebGrid`.
> - Comando de SQL `Update`.
> - El método `Database.Execute` .
> - Campos ocultos (`<input type="hidden">`).

## <a name="what-youll-build"></a>Lo que creará

En el tutorial anterior, aprendió a agregar un registro a una base de datos. Aquí aprenderá a mostrar un registro para editarlo. En la página *películas* , actualizará la aplicación auxiliar de `WebGrid` para que muestre un vínculo de **edición** junto a cada película:

![Presentación de WebGrid que incluye un vínculo de edición para cada película](updating-data/_static/image1.png)

Al hacer clic en el vínculo **Editar** , se le remite a una página diferente, donde la información de la película ya está en un formulario:

![Editar la página de película que muestra la película que se va a editar](updating-data/_static/image2.png)

Puede cambiar cualquiera de los valores. Al enviar los cambios, el código de la página actualiza la base de datos y vuelve a la lista de películas.

Esta parte del proceso funciona casi exactamente igual que la página *AddMovie. cshtml* que creó en el tutorial anterior, por lo que gran parte de este tutorial le resultará familiar.

Hay varias maneras de implementar una forma de editar una película individual. Se ha elegido el enfoque que se muestra porque es fácil de implementar y fácil de comprender.

## <a name="adding-an-edit-link-to-the-movie-listing"></a>Agregar un vínculo de edición a la lista de películas

Para empezar, deberá actualizar la página de *películas* para que cada lista de películas también contenga un vínculo de **edición** .

Abra el archivo *movies. cshtml* .

En el cuerpo de la página, cambie el marcado `WebGrid` agregando una columna. Este es el marcado modificado:

[!code-html[Main](updating-data/samples/sample1.html?highlight=6)]

La nueva columna es esta:

[!code-html[Main](updating-data/samples/sample2.html)]

El punto de esta columna es mostrar un vínculo (elemento`<a>`) cuyo texto dice "Edit". Lo que es después es crear un vínculo similar al siguiente cuando se ejecuta la página, con el valor `id` diferente para cada película:

[!code-css[Main](updating-data/samples/sample3.css)]

Este vínculo invocará una página denominada *EditMovie*y pasará la cadena de consulta `?id=7` a esa página.

La sintaxis de la nueva columna podría parecer un poco compleja, pero esto solo se debe a que une varios elementos. Cada elemento individual es sencillo. Si se concentra solo en el elemento `<a>`, verá este marcado:

[!code-html[Main](updating-data/samples/sample4.html)]

Información general sobre cómo funciona la cuadrícula: la cuadrícula muestra las filas, una para cada registro de base de datos, y muestra las columnas de cada campo en el registro de la base de datos. Mientras se construye cada fila de la cuadrícula, el objeto `item` contiene el registro de base de datos (elemento) de esa fila. Esta disposición le proporciona una forma de obtener el código para obtener los datos de esa fila. Eso es lo que se ve aquí: la expresión `item.ID` obtiene el valor de identificador del elemento de base de datos actual. Puede obtener cualquiera de los valores de la base de datos (title, Genre o Year) del mismo modo mediante `item.Title`, `item.Genre`o `item.Year`.

La expresión `"~/EditMovie?id=@item.ID` combina la parte codificada de forma rígida de la dirección URL de destino (`~/EditMovie?id=`) con este identificador derivado de forma dinámica. (Vio el operador `~` en el tutorial anterior; es un operador ASP.NET que representa la raíz del sitio web actual).

El resultado es que esta parte del marcado en la columna simplemente genera algo parecido al siguiente marcado en tiempo de ejecución:

[!code-xml[Main](updating-data/samples/sample5.xml)]

Naturalmente, el valor real de `id` será diferente para cada fila.

## <a name="creating-a-custom-display-for-a-grid-column"></a>Crear una presentación personalizada para una columna de cuadrícula

Ahora vuelva a la columna Grid. Las tres columnas que originalmente tenía en la cuadrícula solo muestran valores de datos (título, género y año). Para especificar esta presentación, debe pasar el nombre de la columna de base de datos &mdash; por ejemplo, `grid.Column("Title")`.

Esta nueva columna de **edición** de vínculo es diferente. En lugar de especificar un nombre de columna, está pasando un parámetro `format`. Este parámetro permite definir el marcado que se representará en la aplicación auxiliar de `WebGrid` junto con el valor `item` para mostrar los datos de la columna en negrita o verde, o en el formato que desee. Por ejemplo, si desea que el título aparezca en negrita, puede crear una columna como en este ejemplo:

[!code-html[Main](updating-data/samples/sample6.html)]

(Los distintos `@` caracteres que se ven en la propiedad `format` marcan la transición entre el marcado y un valor de código).

Una vez que conozca la propiedad `format`, es más fácil comprender cómo se agrupa la nueva columna de vínculo de **edición** :

[!code-html[Main](updating-data/samples/sample7.html)]

La columna *solo* está formada por el marcado que representa el vínculo, más parte de la información (el ID.) que se extrae del registro de la base de datos de la fila.

> [!TIP]
> 
> **Parámetros con nombre y parámetros posicionales para un método**
> 
> Muchas veces, cuando se llama a un método y se le pasan parámetros, simplemente se muestran los valores de los parámetros separados por comas. A continuación se muestran un par de ejemplos:
> 
> `db.Execute(insertCommand, title, genre, year)`
> 
> `Validation.RequireField("title", "You must enter a title")`
> 
> No mencionamos el problema la primera vez que vio este código, pero en cada caso, va a pasar parámetros a los métodos en un orden específico &mdash; nombre, el orden en el que se definen los parámetros en dicho método. Por `db.Execute` y `Validation.RequireFields`, si mezcla el orden de los valores que pasa, recibirá un mensaje de error al ejecutar la página o al menos algunos resultados extraños. Claramente, tiene que conocer el orden en el que debe pasar los parámetros. (En WebMatrix, IntelliSense puede ayudarle a averiguar el nombre, el tipo y el orden de los parámetros).
> 
> Como alternativa a pasar valores en orden, puede usar *parámetros con nombre*. (El paso de parámetros en orden se conoce como usar *parámetros posicionales*). En el caso de los parámetros con nombre, se incluye explícitamente el nombre del parámetro al pasar su valor. Ha usado parámetros con nombre ya varias veces en estos tutoriales. Por ejemplo:
> 
> [!code-csharp[Main](updating-data/samples/sample8.cs)]
> 
> y
> 
> [!code-css[Main](updating-data/samples/sample9.css)]
> 
> Los parámetros con nombre son útiles para un par de situaciones, especialmente cuando un método toma muchos parámetros. Una es cuando desea pasar solo uno o dos parámetros, pero los valores que desea pasar no están entre las primeras posiciones de la lista de parámetros. Otra situación es cuando desea hacer que el código sea más legible pasando los parámetros en el orden que le resulte más conveniente.
> 
> Obviamente, para usar parámetros con nombre, debe conocer los nombres de los parámetros. IntelliSense de WebMatrix puede *Mostrar* los nombres, pero actualmente no puede rellenarlos.

## <a name="creating-the-edit-page"></a>Crear la página de edición

Ahora puede crear la página *EditMovie* . Cuando los usuarios hagan clic en el vínculo **Editar** , terminarán en esta página.

Cree una página denominada *EditMovie. cshtml* y reemplace lo que hay en el archivo con el marcado siguiente:

[!code-cshtml[Main](updating-data/samples/sample10.cshtml)]

Este marcado y código es similar a lo que tiene en la página *AddMovie* . Hay una pequeña diferencia en el texto del botón Enviar. Al igual que en la página *AddMovie* , hay una llamada `Html.ValidationSummary` que mostrará los errores de validación, si hay alguno. Esta vez se omiten las llamadas a `Validation.Message`, ya que los errores se mostrarán en el Resumen de la validación. Como se indicó en el tutorial anterior, puede utilizar el Resumen de validación y los mensajes de error individuales en diversas combinaciones.

Observe de nuevo que el atributo `method` del elemento `<form>` está establecido en `post`. Como con la página *AddMovie. cshtml* , esta página realiza cambios en la base de datos. Por lo tanto, este formulario debe realizar una operación de `POST`. (Para obtener más información sobre la diferencia entre las operaciones de `GET` y `POST`, vea la barra lateral de [seguridad del verbo GET, post y http](form-basics.md#GET,_POST,_and_HTTP_Verb_Safety) en el tutorial sobre formularios HTML).

Como vimos en un tutorial anterior, los `value` atributos de los cuadros de texto se establecen con código Razor para cargarlos previamente. Sin embargo, esta vez se usan variables como `title` y `genre` para esa tarea en lugar de `Request.Form["title"]`:

`<input type="text" name="title" value="@title" />`

Como antes, este marcado cargará previamente los valores del cuadro de texto con los valores de la película. Verá en un momento por qué es práctico usar variables esta vez en lugar de usar el objeto `Request`.

También hay un `<input type="hidden">` elemento en esta página. Este elemento almacena el ID. de la película sin que esté visible en la página. El identificador se pasa inicialmente a la página mediante el uso de un valor de cadena de consulta (`?id=7` o similar en la dirección URL). Al colocar el valor de identificador en un campo oculto, puede asegurarse de que está disponible cuando se envía el formulario, aunque ya no tenga acceso a la dirección URL original con la que se invocó la página.

A diferencia de la página *AddMovie* , el código de la página *EditMovie* tiene dos funciones distintas. La primera función es que cuando se muestra la página por primera vez (y *solo* después), el código obtiene el ID. de la película de la cadena de consulta. A continuación, el código usa el identificador para leer la película correspondiente de la base de datos y mostrarla (cargar previamente) en los cuadros de texto.

La segunda función es que cuando el usuario hace clic en el botón **Enviar cambios** , el código tiene que leer los valores de los cuadros de texto y validarlos. El código también tiene que actualizar el elemento de base de datos con los nuevos valores. Esta técnica es similar a agregar un registro, como se vio en *AddMovie*.

## <a name="adding-code-to-read-a-single-movie"></a>Agregar código para leer una sola película

Para realizar la primera función, agregue este código en la parte superior de la página:

[!code-cshtml[Main](updating-data/samples/sample11.cshtml)]

La mayor parte de este código se encuentra dentro de un bloque que inicia `if(!IsPost)`. El operador `!` significa "no", por lo que la expresión significa *si esta solicitud no es un envío post*, lo que es una forma indirecta de indicar *si esta solicitud es la primera vez que se ha ejecutado esta página*. Como se indicó anteriormente, este código debe ejecutarse *solo* la primera vez que se ejecuta la página. Si no incluyó el código en `if(!IsPost)`, se ejecutaría cada vez que se invoque la página, ya sea la primera vez o en respuesta a un clic de botón.

Tenga en cuenta que el código incluye un bloque `else` esta vez. Como hemos comentado cuando hemos introducido `if` bloques, a veces desea ejecutar código alternativo si la condición que está probando no es verdadera. Este es el caso aquí. Si la condición se supera (es decir, si el identificador pasado a la página es correcto), se lee una fila de la base de datos. Sin embargo, si no se supera la condición, se ejecuta el bloque `else` y el código establece un mensaje de error.

## <a name="validating-a-value-passed-to-the-page"></a>Validar un valor que se pasa a la página

El código usa `Request.QueryString["id"]` para obtener el identificador que se pasa a la página. El código garantiza que un valor se ha pasado realmente para el identificador. Si no se ha pasado ningún valor, el código establece un error de validación.

Este código muestra una manera diferente de validar la información. En el tutorial anterior, ha trabajado con el ayudante de `Validation`. Ha registrado campos para validar, y ASP.NET ha realizado automáticamente la validación y ha mostrado errores mediante `Html.ValidationMessage` y `Html.ValidationSummary`. Sin embargo, en este caso, no está validando realmente la entrada del usuario. En su lugar, está validando un valor que se pasó a la página desde otra parte. La aplicación auxiliar de `Validation` no lo hace por usted.

Por lo tanto, para comprobar el valor, pruébelo con `if(!Request.QueryString["ID"].IsEmpty()`). Si hay algún problema, puede mostrar el error mediante `Html.ValidationSummary`, como hizo con la aplicación auxiliar de `Validation`. Para ello, llame a `Validation.AddFormError` y pásele un mensaje para mostrarlo. `Validation.AddFormError` es un método integrado que le permite definir mensajes personalizados que se asocian con el sistema de validación con el que ya está familiarizado. (Más adelante en este tutorial hablaremos acerca de cómo hacer que este proceso de validación sea un poco más robusto).

Después de asegurarse de que hay un identificador para la película, el código lee la base de datos y solo busca un elemento de base de datos. (Probablemente ha observado el patrón general para las operaciones de base de datos: Abra la base de datos, defina una instrucción SQL y ejecute la instrucción). Esta vez, la instrucción de SQL `Select` incluye `WHERE ID = @0`. Dado que el identificador es único, solo se puede devolver un registro.

La consulta se realiza mediante `db.QuerySingle` (no `db.Query`, como se ha usado para la lista de películas) y el código coloca el resultado en la variable `row`. El nombre `row` es arbitrario; puede asignar el nombre que desee a las variables. Las variables inicializadas en la parte superior se rellenan con los detalles de la película para que estos valores se puedan mostrar en los cuadros de texto.

## <a name="testing-the-edit-page-so-far"></a>Probar la página de edición (hasta ahora)

Si desea probar la página, ejecute la página *películas* ahora y haga clic en un vínculo de **edición** junto a cualquier película. Verá la página *EditMovie* con los detalles rellenados para la película seleccionada:

![Editar la página de película que muestra la película que se va a editar](updating-data/_static/image3.png)

Observe que la dirección URL de la página incluye algo como `?id=10` (o algún otro número). Hasta ahora, ha probado que los vínculos de **edición** en la página de la *película* funcionan, que la página está leyendo el identificador de la cadena de consulta y que la consulta de base de datos para obtener un único registro de película funciona.

Puede cambiar la información de la película, pero no ocurre nada al hacer clic en **Enviar cambios**.

## <a name="adding-code-to-update-the-movie-with-the-users-changes"></a>Agregar código para actualizar la película con los cambios del usuario

En el archivo *EditMovie. cshtml* , para implementar la segunda función (guardar cambios), agregue el código siguiente justo dentro de la llave de cierre del bloque `@`. (Si no está seguro de dónde colocar el código exactamente, puede ver la [lista de código completa de la página Editar película](#Complete_Page_Listing_for_EditMovie) que aparece al final de este tutorial).

[!code-csharp[Main](updating-data/samples/sample12.cs)]

De nuevo, este marcado y el código son similares al código de *AddMovie*. El código está en un bloque `if(IsPost)`, ya que este código solo se ejecuta cuando el usuario hace clic en el botón **Enviar cambios** &mdash; es decir, cuando se ha publicado el formulario (y solo cuando). En este caso, no está usando una prueba como `if(IsPost && Validation.IsValid())`, es decir, no está combinando ambas pruebas mediante y. En esta página, primero se determina si hay un envío de formulario (`if(IsPost)`) y solo se registran los campos para la validación. A continuación, puede probar los resultados de la validación (`if(Validation.IsValid()`). El flujo es ligeramente diferente que en la página *AddMovie. cshtml* , pero el efecto es el mismo.

Los valores de los cuadros de texto se obtienen mediante `Request.Form["title"]` y un código similar para los demás elementos de `<input>`. Observe que esta vez, el código obtiene el ID. de película del campo oculto (`<input type="hidden">`). Cuando se ejecutó la página por primera vez, el código obtenía el ID. de la cadena de consulta. Obtiene el valor del campo oculto para asegurarse de que está obteniendo el identificador de la película que se mostró originalmente, en caso de que la cadena de consulta se modificara de algún modo desde entonces.

La diferencia realmente importante entre el código *AddMovie* y este código es que en este código se utiliza la instrucción SQL `Update` en lugar de la instrucción `Insert Into`. En el ejemplo siguiente se muestra la sintaxis de la instrucción de SQL `Update`:

`UPDATE table SET col1="value", col2="value", col3="value" ... WHERE ID = value`

Puede especificar cualquier columna en cualquier orden y no es necesario actualizar todas las columnas durante una operación de `Update`. (No se puede actualizar el propio identificador, ya que esto haría en efecto que el registro se guardara como un registro nuevo y eso no se permite para una operación `Update`).

> [!NOTE] 
> 
> **Importante** La cláusula `Where` con el identificador es muy importante, ya que es la forma en que la base de datos sabe qué registro de base de datos desea actualizar. Si deja fuera de la cláusula `Where`, la base de datos actualizaría *todos* los registros de la base de datos. En la mayoría de los casos, esto sería un desastre.

En el código, los valores que se van a actualizar se pasan a la instrucción SQL mediante marcadores de posición. Para repetir lo que hemos dicho antes: por razones de seguridad, use *solo* los marcadores de posición para pasar valores a una instrucción SQL.

Una vez que el código usa `db.Execute` para ejecutar la instrucción de `Update`, redirige de nuevo a la página de lista, donde puede ver los cambios.

> [!TIP] 
> 
> **Diferentes instrucciones SQL, métodos diferentes**
> 
> Es posible que haya observado que usa métodos ligeramente diferentes para ejecutar diferentes instrucciones SQL. Para ejecutar una consulta `Select` que puede devolver varios registros, se usa el método `Query`. Para ejecutar una consulta de `Select` que sepa que solo devolverá un elemento de base de datos, use el método `QuerySingle`. Para ejecutar comandos que realizan cambios pero que no devuelven elementos de base de datos, use el método `Execute`.
> 
> Debe tener métodos diferentes porque cada uno de ellos devuelve resultados diferentes, como ya vio en la diferencia entre `Query` y `QuerySingle`. (El método `Execute` devuelve realmente un valor que también &mdash; nombre, el número de filas de base de datos afectadas por el comando &mdash; pero se ha omitido hasta ahora).
> 
> Por supuesto, el método `Query` podría devolver solo una fila de base de datos. Sin embargo, ASP.NET siempre trata los resultados del método `Query` como una colección. Incluso si el método devuelve solo una fila, tendrá que extraer esa fila de la colección. Por lo tanto, en situaciones en las que *sepa* que solo obtendrá una fila, es un poco más práctico usar `QuerySingle`.
> 
> Hay otros métodos que realizan tipos específicos de operaciones de base de datos. Puede encontrar una lista de métodos de base de datos en la [referencia rápida de ASP.NET Web pages API](../../api-reference/asp-net-web-pages-api-reference.md#Data).

## <a name="making-validation-for-the-id-more-robust"></a>Realización de la validación del identificador más sólida

La primera vez que se ejecuta la página, obtiene el ID. de la película de la cadena de consulta para que pueda obtener esa película desde la base de datos. Asegúrese de que realmente haya un valor para buscar, que ha hecho con este código:

[!code-csharp[Main](updating-data/samples/sample13.cs)]

Usó este código para asegurarse de que si un usuario llega a la página de *EditMovies* sin seleccionar primero una película en la página *películas* , la página mostraría un mensaje de error descriptivo. (De lo contrario, los usuarios verán un error que probablemente simplemente les confundiría).

Sin embargo, esta validación no es muy robusta. La página también se puede invocar con estos errores:

- El identificador no es un número. Por ejemplo, la página se podría invocar con una dirección URL como `http://localhost:nnnnn/EditMovie?id=abc`.
- El identificador es un número, pero hace referencia a una película que no existe (por ejemplo, `http://localhost:nnnnn/EditMovie?id=100934`).

Si tiene curiosidad por ver los errores resultantes de estas direcciones URL, ejecute la página *películas* . Seleccione una película para editarla y, a continuación, cambie la dirección URL de la página *EditMovie* por una dirección URL que contenga un identificador alfabético o el ID. de una película no existente.

¿Qué debe hacer? La primera corrección es asegurarse de que no solo se pasa un identificador a la página, pero que el identificador es un entero. Cambie el código para que la prueba de `!IsPost` sea similar a la de este ejemplo:

[!code-csharp[Main](updating-data/samples/sample14.cs)]

Ha agregado una segunda condición a la prueba de `IsEmpty`, vinculada con `&&` (AND lógico):

[!code-csharp[Main](updating-data/samples/sample15.cs)]

Es posible que recuerde de la [Introducción a ASP.NET Web pages](../introducing-razor-syntax-c.md) tutorial de programación que métodos como `AsBool` un `AsInt` convertir una cadena de caracteres a algún otro tipo de datos. El método `IsInt` (y otros, como `IsBool` y `IsDateTime`) son similares. Sin embargo, solo prueban si *puede* convertir la cadena, sin realizar realmente la conversión. Por lo tanto, aquí está diciendo *en esencia si el valor de la cadena de consulta se puede convertir en un entero...* .

El otro problema potencial es buscar una película que no existe. El código para obtener una película es similar a este código:

[!code-csharp[Main](updating-data/samples/sample16.cs)]

Si pasa un valor `movieId` al método `QuerySingle` que no se corresponde con una película real, no se devuelve nada y las instrucciones siguientes (por ejemplo, `title=row.Title`) producen errores.

Una vez más, hay una solución fácil. Si el método `db.QuerySingle` no devuelve ningún resultado, la variable `row` será null. Por lo tanto, puede comprobar si la variable `row` es NULL antes de intentar obtener valores de ella. En el código siguiente se agrega un bloque `if` alrededor de las instrucciones que obtienen los valores del objeto `row`:

[!code-csharp[Main](updating-data/samples/sample17.cs)]

Con estas dos pruebas de validación adicionales, la página se vuelve más detallada. El código completo de la rama `!IsPost` ahora es similar a este ejemplo:

[!code-csharp[Main](updating-data/samples/sample18.cs)]

Tendremos en cuenta que esta tarea es un buen uso para un bloque de `else`. Si no se superan las pruebas, los bloques de `else` establecen mensajes de error.

## <a name="adding-a-link-to-return-to-the-movies-page"></a>Agregar un vínculo para volver a la página de películas

Un detalle final y útil es volver a agregar un vínculo a la página *películas* . En el flujo de eventos normal, los usuarios se inician en la página *películas* y hacen clic en un vínculo **Editar** . Esto los coloca en la página *EditMovie* , donde pueden editar la película y hacer clic en el botón. Una vez que el código procesa el cambio, redirige de nuevo a la página de *películas* .

Pero:

- El usuario puede decidir no cambiar nada.
- Es posible que el usuario haya llegado a esta página sin hacer clic primero en el vínculo **Editar** de la página *películas* .

En cualquier caso, desea que resulte fácil volver a la lista principal. Es una solución sencilla &mdash; agregar el marcado siguiente justo después de la etiqueta de cierre `</form>` en el marcado:

[!code-html[Main](updating-data/samples/sample19.html)]

Este marcado usa la misma sintaxis para un elemento `<a>` que ha visto en otro lugar. La dirección URL incluye `~` que significa "raíz del sitio web".

## <a name="testing-the-movie-update-process"></a>Probar el proceso de actualización de la película

Ahora puede probar. Ejecute la página *películas* y haga clic en **Editar** junto a una película. Cuando aparezca la página *EditMovie* , realice cambios en la película y haga clic en **Enviar cambios**. Cuando aparezca la lista de películas, asegúrese de que se muestran los cambios.

Para asegurarse de que la validación funciona, haga clic en **Editar** para otra película. Cuando llegue a la página *EditMovie* , borre el campo **Genre** (o el campo **Year** , o ambos) e intente enviar los cambios. Verá un error, como cabría esperar:

![Página Editar película que muestra los errores de validación](updating-data/_static/image4.png)

Haga clic en el vínculo **volver a la lista de películas** para abandonar los cambios y volver a la página *películas* .

## <a name="coming-up-next"></a>Próxima

En el siguiente tutorial, verá cómo eliminar un registro de película.

## <a name="complete-listing-for-movie-page-updated-with-edit-links"></a>Lista completa de la página de la película (actualizada con los vínculos de edición)

[!code-cshtml[Main](updating-data/samples/sample20.cshtml)]

<a id="Complete_Page_Listing_for_EditMovie"></a>
## <a name="complete-page-listing-for-edit-movie-page"></a>Lista completa de páginas para editar película

[!code-cshtml[Main](updating-data/samples/sample21.cshtml)]

## <a name="additional-resources"></a>Recursos adicionales

- [Introducción a la programación web de ASP.NET mediante la sintaxis Razor](../../getting-started/introducing-razor-syntax-c.md)
- [Instrucción UPDATE de SQL](http://www.w3schools.com/sql/sql_update.asp) en el sitio de w3schools

> [!div class="step-by-step"]
> [Anterior](entering-data.md)
> [Siguiente](deleting-data.md)
