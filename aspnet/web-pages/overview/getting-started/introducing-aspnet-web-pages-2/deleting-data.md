---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: Presentación de ASP.NET Web Pages-eliminación de datos de base de datos | Microsoft Docs
author: Rick-Anderson
description: En este tutorial se muestra cómo eliminar una entrada de base de datos individual. Se supone que ha completado la serie a través de la actualización de los datos de la base de datos en ASP.NET Web PA...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: c8620fc1abc61d514bdc039c66f7a84e67e89abe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78510463"
---
# <a name="introducing-aspnet-web-pages---deleting-database-data"></a>Introducción a la eliminación de datos de la base de datos de ASP.NET Web Pages

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este tutorial se muestra cómo eliminar una entrada de base de datos individual. Se supone que ha completado las series a través [de la actualización de los datos de la base de datos en ASP.NET Web pages](updating-data.md).
> 
> Temas que se abordarán:
> 
> - Cómo seleccionar un registro individual de una lista de registros.
> - Cómo eliminar un solo registro de una base de datos.
> - Cómo comprobar que se hizo clic en un botón específico en un formulario.
>   
> 
> Características y tecnologías descritas:
> 
> - Aplicación auxiliar de `WebGrid`.
> - Comando de SQL `Delete`.
> - El método `Database.Execute` para ejecutar un comando de `Delete` SQL.

## <a name="what-youll-build"></a>Lo que creará

En el tutorial anterior, aprendió a actualizar un registro de base de datos existente. Este tutorial es similar, salvo que en lugar de actualizar el registro, lo eliminará. Los procesos son muy parecidos, salvo que la eliminación es más simple, por lo que este tutorial será breve.

En la página *películas* , actualizará la aplicación auxiliar de `WebGrid` para que muestre un vínculo **eliminar** junto a cada película que acompañe al vínculo de **edición** que agregó anteriormente.

![Página de películas que muestra un vínculo de eliminación para cada película](deleting-data/_static/image1.png)

Al igual que con la edición, al hacer clic en el vínculo **eliminar** , se le remite a una página diferente, donde la información de la película ya está en un formulario:

![Página eliminar película con una película mostrada](deleting-data/_static/image2.png)

Después, puede hacer clic en el botón para eliminar el registro de forma permanente.

## <a name="adding-a-delete-link-to-the-movie-listing"></a>Agregar un vínculo de eliminación a la lista de películas

Empezará agregando un vínculo de **eliminación** a la aplicación auxiliar de `WebGrid`. Este vínculo es similar al vínculo de **edición** que ha agregado en un tutorial anterior.

Abra el archivo *movies. cshtml* .

Cambie el marcado de `WebGrid` en el cuerpo de la página agregando una columna. Este es el marcado modificado:

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

La nueva columna es esta:

[!code-html[Main](deleting-data/samples/sample2.html)]

La forma en que se configura la cuadrícula, la columna **Editar** está más a la izquierda en la cuadrícula y la columna **eliminar** está en la derecha. (Ahora hay una coma después de la columna de `Year`, en caso de que no lo advierta). No hay nada especial sobre el lugar en el que se colocan estas columnas de vínculo y puede colocarlas fácilmente una junto a la otra. En este caso, son independientes para que sean más difíciles de combinar.

![Página de películas con vínculos de edición y detalles marcados para mostrar que no están junto a otros](deleting-data/_static/image3.png)

La nueva columna muestra un vínculo (elemento`<a>`) cuyo texto dice "eliminar". El destino del vínculo (su `href` atributo) es un código que se resuelve en última instancia en algo como esta dirección URL, con el valor `id` distinto para cada película:

[!code-css[Main](deleting-data/samples/sample3.css)]

Este vínculo invocará una página denominada *DeleteMovie* y la pasará al identificador de la película que ha seleccionado.

En este tutorial no se detallará cómo se construye este vínculo, ya que es casi idéntico al vínculo de **edición** del tutorial anterior ([actualización de datos de base de datos en ASP.NET Web pages](updating-data.md)).

## <a name="creating-the-delete-page"></a>Creación de la página eliminar

Ahora puede crear la página que será el destino del vínculo de **eliminación** en la cuadrícula.

> [!NOTE] 
> 
> **Importante** La técnica de seleccionar primero un registro que se va a eliminar y, a continuación, usar una página y un botón independientes para confirmar el proceso es extremadamente importante para la seguridad. Como ha leído en los tutoriales anteriores, realizar *cualquier* tipo de cambio en el sitio Web *siempre* debe realizarse mediante un formulario &mdash; es decir, mediante una operación http post. Si ha hecho posible cambiar el sitio simplemente haciendo clic en un vínculo (es decir, mediante una operación GET), los usuarios podrían realizar solicitudes simples al sitio y eliminar los datos. Incluso un rastreador del motor de búsqueda que está indizando su sitio podría eliminar accidentalmente los datos simplemente mediante los vínculos siguientes.
> 
> Cuando la aplicación permite a los usuarios cambiar un registro, tiene que presentar el registro al usuario para su edición. Pero podría verse tentado a omitir este paso para eliminar un registro. Sin embargo, no omita este paso. (También resulta útil para los usuarios ver el registro y confirmar que están eliminando el registro que pretendía).
> 
> En un conjunto de tutoriales posterior, verá cómo agregar la funcionalidad de inicio de sesión para que un usuario tenga que iniciar sesión antes de eliminar un registro.

Cree una página denominada *DeleteMovie. cshtml* y reemplace lo que hay en el archivo con el marcado siguiente:

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

Este marcado es como las páginas *EditMovie* , salvo que en lugar de usar cuadros de texto (`<input type="text">`), el marcado incluye elementos `<span>`. No hay nada que editar. Lo único que tiene que hacer es mostrar los detalles de la película para que los usuarios puedan asegurarse de que están eliminando la película adecuada.

El marcado ya contiene un vínculo que permite al usuario volver a la página de lista de películas.

Como en la página *EditMovie* , el identificador de la película seleccionada se almacena en un campo oculto. (Se pasa a la página en primer lugar como valor de cadena de consulta). Hay una llamada `Html.ValidationSummary` que mostrará los errores de validación. En este caso, el error puede ser que no se ha pasado ningún ID. de película a la página o que el ID. de la película no es válido. Esta situación puede producirse si alguien ejecutó esta página sin seleccionar primero una película en la página *películas* .

El título del botón es **eliminar película**y su atributo de nombre está establecido en `buttonDelete`. El atributo `name` se usará en el código para identificar el botón que envió el formulario.

Tendrá que escribir código en 1) leer los detalles de la película cuando se muestre la página por primera vez y 2) eliminar realmente la película cuando el usuario haga clic en el botón.

## <a name="adding-code-to-read-a-single-movie"></a>Agregar código para leer una sola película

En la parte superior de la página *DeleteMovie. cshtml* , agregue el siguiente bloque de código:

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

Este marcado es el mismo que el código correspondiente en la página *EditMovie* . Obtiene el ID. de la película de la cadena de consulta y usa el identificador para leer un registro de la base de datos. El código incluye la prueba de validación (`IsInt()` y `row != null`) para asegurarse de que el ID. de película que se pasa a la página es válido.

Recuerde que este código solo debe ejecutarse la primera vez que se ejecute la página. No desea volver a leer el registro de la película en la base de datos cuando el usuario hace clic en el botón **eliminar película** . Por lo tanto, el código para leer la película se encuentra dentro de una prueba que indica `if(!IsPost)` &mdash; es decir, *si la solicitud no es una operación post (envío de formulario)* .

## <a name="adding-code-to-delete-the-selected-movie"></a>Agregar código para eliminar la película seleccionada

Para eliminar la película cuando el usuario haga clic en el botón, agregue el código siguiente justo dentro de la llave de cierre del bloque `@`:

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

Este código es similar al código para actualizar un registro existente, pero más sencillo. Básicamente, el código ejecuta una instrucción de `Delete` de SQL.

 Como en la página *EditMovie* , el código se encuentra en un bloque `if(IsPost)`. Esta vez, la condición `if()` es un poco más complicada: 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

Aquí hay dos condiciones. La primera es que se envía la página, como ha visto antes &mdash; `if(IsPost)`.

La segunda condición es `!Request["buttonDelete"].IsEmpty()`, lo que significa que la solicitud tiene un objeto denominado `buttonDelete`. De forma indirecta, es un modo indirecto de probar qué botón envió el formulario. Si un formulario contiene varios botones de envío, solo aparece en la solicitud el nombre del botón en el que se hizo clic. Por lo tanto, lógicamente, si el nombre de un botón determinado aparece en la solicitud &mdash; o como se indica en el código, si ese botón no está vacío &mdash; es el botón que envió el formulario.

El operador `&&` significa "and" (AND lógico). Por lo tanto, toda la condición de `if` es...

*Esta solicitud es post (no es una solicitud por primera vez)*  
  
 AND  
  
*El* *botón `buttonDelete`era el botón que envió el formulario.*

Este formulario (de hecho, esta página) contiene un solo botón, por lo que técnicamente no es necesaria la prueba adicional para `buttonDelete`. Todavía está a punto de realizar una operación que quitará los datos permanentemente. Por lo tanto, es posible que desee asegurarse de que está realizando la operación solo cuando el usuario la ha solicitado explícitamente. Por ejemplo, supongamos que ha expandido esta página más adelante y le ha agregado otros botones. Incluso después, el código que elimina la película solo se ejecutará si se hizo clic en el botón `buttonDelete`.

Como en la página *EditMovie* , obtiene el identificador del campo oculto y, a continuación, ejecuta el comando SQL. La sintaxis de la instrucción `Delete` es:

`DELETE FROM table WHERE ID = value`

Es fundamental incluir la cláusula `WHERE` y el identificador. Si omite la cláusula WHERE, se *eliminarán todos los registros de la tabla*. Como ha aprendido, puede pasar el valor del identificador al comando SQL mediante un marcador de posición.

## <a name="testing-the-movie-delete-process"></a>Probar el proceso de eliminación de películas

Ahora puede probar. Ejecute la página *películas* y haga clic en **eliminar** junto a una película. Cuando aparezca la página *DeleteMovie* , haga clic en **eliminar película**.

![Página eliminar película con el botón Eliminar película resaltado](deleting-data/_static/image4.png)

Al hacer clic en el botón, el código elimina las películas y vuelve a la lista de películas. Allí puede buscar la película eliminada y confirmar que se ha eliminado.

## <a name="coming-up-next"></a>Próxima

En el siguiente tutorial se muestra cómo dar a todas las páginas del sitio una apariencia y un diseño comunes.

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a>Lista completa de la página de película (actualizada con vínculos de eliminación)

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a>Lista completa de la página de DeleteMovie

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a>Recursos adicionales

- [Introducción a la programación web de ASP.NET mediante la sintaxis Razor](../introducing-razor-syntax-c.md)
- [Instrucción SQL Delete](http://www.w3schools.com/sql/sql_delete.asp) en el sitio w3schools

> [!div class="step-by-step"]
> [Anterior](updating-data.md)
> [Siguiente](layouts.md)
