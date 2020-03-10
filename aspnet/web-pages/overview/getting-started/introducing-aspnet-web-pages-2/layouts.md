---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: 'Presentación de ASP.NET Web Pages: creación de un diseño coherente | Microsoft Docs'
author: Rick-Anderson
description: En este tutorial se muestra cómo usar diseños para crear una apariencia coherente de las páginas de un sitio que usa ASP.NET Web Pages. Se supone que ha completado la...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: 678eb7089e95e3d221d6b2d82034a62aefa75757
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78422749"
---
# <a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a>Presentación de ASP.NET Web Pages: creación de un diseño coherente

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este tutorial se muestra cómo usar *diseños* para crear una apariencia coherente de las páginas de un sitio que usa ASP.NET Web pages. Se supone que ha completado la serie a través [de la eliminación de datos de la base de datos en ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=251584).
> 
> Temas que se abordarán:
> 
> - Qué es una página de diseño.
> - Cómo combinar páginas de diseño con contenido dinámico.
> - Cómo pasar valores a una página de diseño.

## <a name="about-layouts"></a>Acerca de los diseños

Las páginas que ha creado hasta ahora se han completado, páginas independientes. Todos ellos pertenecen al mismo sitio, pero no tienen ningún elemento común o una apariencia estándar.

La mayoría de los sitios tienen una apariencia y un diseño coherentes. Por ejemplo, si va al sitio de [Microsoft.com/web](https://www.microsoft.com/web/) y observa que las páginas se adhieren a un diseño general y a un tema visual:

![Página del sitio de Microsoft.com/web que muestra el diseño del encabezado, el área de navegación, el área de contenido y el pie de página](layouts/_static/image1.png)

Una manera *ineficaz* de crear este diseño sería definir un encabezado, una barra de navegación y un pie de página por separado en cada una de las páginas. Se va a duplicar el mismo marcado cada vez. Si desea cambiar algo (por ejemplo, actualizar el pie de página), tendría que cambiar cada página por separado.

Aquí es donde se encuentran *las páginas de diseño* . En ASP.NET Web Pages, puede definir una página de diseño que proporcione un contenedor global para las páginas del sitio. Por ejemplo, la página de diseño puede contener el encabezado, el área de navegación y el pie de página. La página de diseño incluye un marcador de posición donde va el contenido principal.

Después, puede definir páginas de contenido individuales que contengan el marcado y el código solo para esa página. Las páginas de contenido no tienen que ser páginas HTML completas; ni siquiera tienen que tener un elemento `<body>`. También tienen una línea de código que indica a ASP.NET en qué página de diseño desea mostrar el contenido. Esta es una imagen que muestra aproximadamente cómo funciona esta relación:

![Diagrama conceptual que muestra dos páginas de contenido y una página de diseño en la que se ajustan](layouts/_static/image2.png)

Esta interacción es fácil de entender cuando se ve en acción. En este tutorial, cambiará las páginas de películas para usar un diseño.

## <a name="adding-a-layout-page"></a>Agregar una página de diseño

Comenzaremos por crear una página de diseño que defina un diseño de página típico con un encabezado, un pie de página y un área para el contenido principal. En el sitio de WebPagesMovies, agregue una página CSHTML denominada *\_layout. CSHTML*.

El carácter de subrayado inicial (`_`) es significativo. Si el nombre de una página comienza con un carácter de subrayado, ASP.NET no enviará directamente esa página al explorador. Esta Convención le permite definir las páginas necesarias para su sitio, pero que los usuarios no deben poder solicitar directamente.

Reemplace el contenido de la página por lo siguiente:

[!code-html[Main](layouts/samples/sample1.html)]

Como puede ver, este marcado es simplemente HTML que usa `<div>` elementos para definir tres secciones en la página más un elemento `<div>` más para contener las tres secciones. El pie de página contiene un poco de código Razor: `@DateTime.Now.Year`, que representará el año actual en esa ubicación de la página.

Observe que hay un vínculo a una hoja de estilos denominada *movies. CSS*. La hoja de estilos es donde se definirán los detalles del diseño físico de los elementos. Lo creará en un momento.

La única característica inusual en esta *\_página layout. cshtml* es la línea de `@Render.Body()`. Este es el marcador de posición al que irá el contenido cuando este diseño se combine con otra página.

## <a name="adding-a-css-file"></a>Agregar un archivo. CSS

La mejor manera de definir la disposición real (es decir, el aspecto) de los elementos de la página es usar reglas de hoja de estilos en cascada (CSS). Por lo tanto, creará un archivo *. CSS* con las reglas para el nuevo diseño.

En WebMatrix, seleccione la raíz del sitio. Después, en la pestaña **archivos** de la cinta de opciones, haga clic en la flecha situada debajo del botón **nuevo** y, a continuación, haga clic en **nueva carpeta**.

![La opción "nueva carpeta" en nuevo en la cinta de opciones.](layouts/_static/image3.png)

Asigne el nombre *estilos*a la nueva carpeta.

![Asignar un nombre a la nueva carpeta ' styles '](layouts/_static/image4.png)

Dentro de la nueva carpeta de *estilos* , cree un archivo denominado *movies. CSS*.

![Crear un nuevo archivo movies. CSS](layouts/_static/image5.png)

Reemplace el contenido del nuevo archivo *. CSS* por lo siguiente:

[!code-css[Main](layouts/samples/sample2.css)]

No tendremos en cuenta las siguientes reglas de CSS, excepto para anotar dos cosas. Una es que, además de establecer fuentes y tamaños, las reglas utilizan el posicionamiento absoluto para establecer la ubicación del encabezado, el pie de página y el área de contenido principal. Si no está familiarizado con la posición en CSS, puede leer el tutorial de [posicionamiento de CSS](http://www.w3schools.com/css/css_positioning.asp) en el sitio de w3schools.

Lo que hay que tener en cuenta es que, en la parte inferior, se han copiado las reglas de estilo que se definieron originalmente individualmente en el archivo *movies. cshtml* . Estas reglas se usaron en la [Introducción a la visualización de datos mediante ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial para que la aplicación auxiliar de `WebGrid` pueda representar el marcado que agregó bandas a la tabla. (Si va a utilizar un archivo *. CSS* para las definiciones de estilo, también puede colocar las reglas de estilo para todo el sitio en ella).

## <a name="updating-the-movies-file-to-use-the-layout"></a>Actualizar el archivo de películas para usar el diseño

Ahora puede actualizar los archivos existentes en el sitio para usar el nuevo diseño. Abra el archivo *movies. cshtml* . En la parte superior, como la primera línea de código, agregue lo siguiente:

[!code-csharp[Main](layouts/samples/sample3.cs)]

La página ahora empieza de esta manera:

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

Esta línea de código indica a ASP.NET que, cuando se ejecuta la página de *películas* , debe combinarse con el archivo *\_layout. cshtml* .

Dado que el archivo *movies. cshtml* ahora usa una página de diseño, puede quitar el marcado de la página *movies. cshtml* que se encarga de la *\_archivo layout. cshtml* . Saque el `<!DOCTYPE>`, `<html>`y `<body>` etiquetas de apertura y cierre. Saque todo el elemento `<head>` y su contenido, que incluye las reglas de estilo de la cuadrícula, ya que ahora tiene esas reglas en un archivo *. CSS* . Mientras esté en él, cambie el elemento de `<h1>` existente a un elemento `<h2>`; ya tiene un elemento `<h1>` en la página de diseño. Cambie el texto de la `<h2>` a "list Movies".

Normalmente, no tendría que realizar estos tipos de cambios en una página de contenido. Al iniciar el sitio con una página de diseño, se crean páginas de contenido sin todos estos elementos para comenzar. Sin embargo, en este caso, va a convertir una página independiente en una que usa un diseño, por lo que hay un poco de limpieza.

Cuando haya terminado, la página *movies. cshtml* tendrá un aspecto similar al siguiente:

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a>Probar el diseño

Ahora puede ver el aspecto del diseño. En WebMatrix, haga clic con el botón derecho en la página *movies. cshtml* y seleccione **iniciar en el explorador**. Cuando el explorador muestra la página, se parece a esta página:

![Página de películas representada mediante un diseño](layouts/_static/image6.png)

ASP.NET ha combinado el contenido de la página movies. cshtml en la página *\_layout. cshtml* en la que el método de `RenderBody` es. Y, por supuesto, la página *\_layout. cshtml* hace referencia a un archivo *. CSS* que define el aspecto de la página.

## <a name="updating-the-addmovie-page-to-use-the-layout"></a>Actualización de la página AddMovie para usar el diseño

La ventaja real de los diseños es que puede usarlos para todas las páginas del sitio. Abra la página *AddMovie. cshtml* .

Es posible que recuerde que en la página *AddMovie. cshtml* tenía algunas reglas CSS para definir la apariencia de los mensajes de error de validación. Puesto que ahora tiene un archivo *. CSS* para su sitio, puede trasladar esas reglas al archivo *. CSS* . Quítelos del archivo *AddMovie. cshtml* y agréguelos a la parte inferior del archivo *movies. CSS* . Va a mover las siguientes reglas:

[!code-css[Main](layouts/samples/sample6.css)]

Ahora realice el mismo tipo de cambios en *AddMovie. cshtml* que hizo para *movies. cshtml* : agregue `Layout="~/_Layout.cshtml;` y quite el marcado HTML que ahora es extraño. Cambie el elemento `<h1>` por `<h2>`. Cuando haya terminado, la página tendrá un aspecto similar al de este ejemplo:

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

Ejecute la página. Ahora tiene este aspecto:

![Página ' Agregar películas ' representada mediante un diseño](layouts/_static/image7.png)

Quiere realizar cambios similares en las páginas del sitio: *EditMovie. cshtml* y *DeleteMovie. cshtml*. Sin embargo, antes de hacerlo, puede realizar otro cambio en el diseño que lo hace un poco más flexible.

## <a name="passing-title-information-to-the-layout-page"></a>Pasar información de título a la página de diseño

La página *\_layout. cshtml* que ha creado tiene un elemento `<title>` que se establece en "mi sitio de película". La mayoría de los exploradores muestran el contenido de este elemento como texto en una pestaña:

![El título de la &lt;de la página&gt; elemento que se muestra en una pestaña del explorador](layouts/_static/image8.png)

Esta información de título es genérica. Supongamos que desea que el texto del título sea más específico en la página actual. (Los motores de búsqueda también usan el texto del título para determinar el contenido de la página). Puede pasar información de una página de contenido como *movies. cshtml* o *AddMovie. cshtml* a la página de diseño y, a continuación, utilizar esa información para personalizar lo que representa la página de diseño.

Vuelva a abrir la página *movies. cshtml* . En el código de la parte superior, agregue la siguiente línea:

[!code-csharp[Main](layouts/samples/sample8.cs)]

El objeto `Page` está disponible en todas las páginas *. cshtml* y es para este fin, es decir, para compartir información entre una página y su diseño.

Abra la página *\_layout. cshtml* . Cambie el elemento `<title>` para que tenga el siguiente aspecto:

[!code-html[Main](layouts/samples/sample9.html)]

Este código representa todo lo que hay en la propiedad `Page.Title` derecha en esa ubicación de la página.

Ejecute la página *movies. cshtml* . Esta vez, la pestaña del explorador muestra lo que pasó como el valor de `Page.Title`:

![Una pestaña del explorador que muestra el título creado dinámicamente](layouts/_static/image9.png)

Si lo desea, vea el origen de la página en el explorador. Puede ver que el elemento `<title>` se representa como `<title>List Movies</title>`.

> [!TIP] 
> 
> **El objeto Page**
> 
> Una característica útil de `Page` es que es un objeto dinámico: la propiedad `Title` no es un nombre fijo o reservado. Puede utilizar *cualquier* nombre para un valor del objeto `Page`. Por ejemplo, puede que le resulte tan fácil pasar el título mediante una propiedad denominada `Page.CurrentName` o `Page.MyPage`. La única restricción es que el nombre debe seguir las reglas normales para las propiedades a las que se puede asignar un nombre. (Por ejemplo, el nombre no puede contener un espacio).
> 
> Puede pasar cualquier número de valores mediante el `Page` objeto. Si desea pasar información de películas a la página de diseño, puede pasar valores mediante algo como `Page.MovieTitle` y `Page.Genre` y `Page.MovieYear`. (O cualquier otro nombre que se haya inventado para almacenar la información). El único requisito, que es probablemente obvio, es que tiene que usar los mismos nombres en la página de contenido y en la página de diseño.
> 
> La información que se pasa mediante el objeto `Page` no se limita al texto que se va a mostrar en la página de diseño. Puede pasar un valor a la página de diseño y, a continuación, el código de la página de diseño puede utilizar el valor para decidir si se va a mostrar una sección de la página, el archivo *. CSS* que se va a usar, etc. Los valores que se pasan en el objeto `Page` son como cualquier otro valor que se utilice en el código. Es simplemente que los valores se originan en la página de contenido y se pasan a la página de diseño.

Abra la página *AddMovie. cshtml* y agregue una línea en la parte superior del código que proporciona un título para la página *AddMovie. cshtml* :

[!code-csharp[Main](layouts/samples/sample10.cs)]

Ejecute la página *AddMovie. cshtml* . Verá el nuevo título allí:

![Una pestaña del explorador que muestra el título "agregar películas" creado dinámicamente](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a>Actualizar las páginas restantes para usar el diseño

Ahora puede finalizar las páginas restantes del sitio para que usen el nuevo diseño. Abra *EditMovie. cshtml* y *DeleteMovie. cshtml* a su vez y realice los mismos cambios en cada uno.

Agregue la línea de código que se vincula a la página de diseño:

[!code-csharp[Main](layouts/samples/sample11.cs)]

Agregue una línea para establecer el título de la página:

[!code-csharp[Main](layouts/samples/sample12.cs)]

O bien

[!code-csharp[Main](layouts/samples/sample13.cs)]

Quitar todo el marcado HTML extraño: básicamente, deje solo los bits que se encuentran dentro del elemento `<body>` (más el bloque de código en la parte superior).

Cambie el elemento `<h1>` para que sea un elemento `<h2>`.

Cuando haya realizado estos cambios, pruebe cada uno de ellos y asegúrese de que se muestra correctamente y de que el título es correcto.

## <a name="parting-thoughts-about-layout-pages"></a>Consideraciones sobre las páginas de diseño

En este tutorial, ha creado una *\_página layout. cshtml* y ha usado el método `RenderBody` para combinar el contenido de otra página. Este es el patrón básico para usar diseños en páginas Web.

Las páginas de diseño tienen características adicionales que no tratamos aquí. Por ejemplo, puede anidar las páginas de diseño; una página de diseño puede, a su vez, hacer referencia a otra. Los diseños anidados pueden ser útiles si está trabajando con subsecciones de un sitio que requiere diseños diferentes. También puede usar métodos adicionales (por ejemplo, `RenderSection`) para configurar secciones con nombre en la página de diseño.

La combinación de las páginas de diseño y los archivos *. CSS* es eficaz. Como verá en la siguiente serie de tutoriales, en WebMatrix, puede crear un sitio basado en una *plantilla*, lo que le ofrece un sitio que tiene una funcionalidad pregenerada. Las plantillas hacen un uso correcto de las páginas de diseño y CSS para crear sitios que tienen un aspecto excelente y que tienen características como menús. Esta es una captura de pantalla de la Página principal de un sitio basado en una plantilla, que muestra las características que usan páginas de diseño y CSS:

![Diseño creado por plantilla de sitio de WebMatrix que muestra el encabezado, el área de navegación, el área de contenido, la sección opcional y los vínculos de inicio de sesión](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a>Lista completa de la página de película (actualizada para usar una página de diseño)

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a>Completar la lista de páginas para agregar una página de película (actualizada para el diseño)

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a>Completar la lista de páginas para la página eliminar película (actualizada para el diseño)

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a>Completar la lista de páginas de la página Editar película (actualizada para el diseño)

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a>Próxima

En el siguiente tutorial, aprenderá a publicar su sitio en Internet para que todos los usuarios puedan verlo.

## <a name="additional-resources"></a>Recursos adicionales

- [Creación de una apariencia coherente](https://go.microsoft.com/fwlink/?LinkID=202891) : un artículo que proporciona información más detallada sobre cómo trabajar con diseños. También se describe cómo pasar un valor a una página de diseño que muestra u oculta parte del contenido.
- [Páginas de diseño anidadas con Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) : blogs de Mike salmuera un ejemplo de cómo anidar páginas de diseño. (Incluye una descarga de las páginas).

> [!div class="step-by-step"]
> [Anterior](deleting-data.md)
> [Siguiente](publishing.md)
