---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
title: Usar la clase TagBuilder para compilar aplicaciones auxiliaresC#HTML () | Microsoft Docs
author: StephenWalther
description: Stephen Walther presenta una útil clase de utilidad en el marco de ASP.NET MVC denominado la clase TagBuilder. Puede usar la clase TagBuilder con facilidad...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 3975a52f-bd15-4edd-8f3d-1df93672515b
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: c8eaea9932a30c744b9a69861619ce9458b5a23a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78485611"
---
# <a name="using-the-tagbuilder-class-to-build-html-helpers-c"></a>Usar la clase TagBuilder para compilar aplicaciones auxiliaresC#HTML ()

por [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther presenta una útil clase de utilidad en el marco de ASP.NET MVC denominado la clase TagBuilder. Puede usar la clase TagBuilder para compilar fácilmente etiquetas HTML.

El marco de MVC de ASP.NET incluye una clase de utilidad útil denominada clase TagBuilder que se puede usar al compilar aplicaciones auxiliares HTML. La clase TagBuilder, como sugiere el nombre de la clase, le permite compilar fácilmente etiquetas HTML. En este breve tutorial, se proporciona información general sobre la clase TagBuilder y aprenderá a usar esta clase al compilar una aplicación auxiliar HTML simple que representa etiquetas HTML &lt;IMG&gt;.

## <a name="overview-of-the-tagbuilder-class"></a>Información general de la clase TagBuilder

La clase TagBuilder se encuentra en el espacio de nombres System. Web. Mvc. Tiene cinco métodos:

- AddCssClass (): permite agregar un nuevo atributo *Class = ""* a una etiqueta.
- GenerateId (): permite agregar un atributo de identificador a una etiqueta. Este método reemplaza automáticamente los puntos en el identificador (de forma predeterminada, los puntos se sustituyen por un carácter de subrayado)
- MergeAttribute (): permite agregar atributos a una etiqueta. Hay varias sobrecargas de este método.
- SetInnerText (): permite establecer el texto interno de la etiqueta. El texto interno se codifica automáticamente en HTML.
- ToString (): permite representar la etiqueta. Puede especificar si desea crear una etiqueta normal, una etiqueta inicial, una etiqueta final o una etiqueta de autocierre.

La clase TagBuilder tiene cuatro propiedades importantes:

- Attributes: representa todos los atributos de la etiqueta.
- IdAttributeDotReplacement: representa el carácter usado por el método GenerateId () para reemplazar puntos (el valor predeterminado es un carácter de subrayado).
- InnerHTML: representa el contenido interno de la etiqueta. La asignación de una cadena a *esta propiedad no* codifica la cadena en HTML.
- TagName: representa el nombre de la etiqueta.

Estos métodos y propiedades proporcionan todos los métodos y propiedades básicos que necesita para crear una etiqueta HTML. En realidad, no es necesario usar la clase TagBuilder. En su lugar, podría usar una clase StringBuilder. Sin embargo, la clase TagBuilder hace que su vida sea un poco más fácil.

## <a name="creating-an-image-html-helper"></a>Crear una aplicación auxiliar HTML de imagen

Cuando se crea una instancia de la clase TagBuilder, se pasa el nombre de la etiqueta que se va a compilar al constructor TagBuilder. A continuación, puede llamar a métodos como los métodos AddCssClass y MergeAttribute () para modificar los atributos de la etiqueta. Por último, se llama al método ToString () para representar la etiqueta.

Por ejemplo, la lista 1 contiene una aplicación auxiliar HTML de imagen. La aplicación auxiliar de imágenes se implementa internamente con un TagBuilder que representa una etiqueta HTML &lt;IMG&gt;.

**Lista 1-Helpers\ImageHelper.cs**

[!code-csharp[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample1.cs)]

La clase de la lista 1 contiene dos métodos sobrecargados estáticos denominados Image. Cuando se llama al método Image (), se puede pasar un objeto que representa un conjunto de atributos HTML o no.

Observe cómo se usa el método TagBuilder. MergeAttribute () para agregar atributos individuales, como el atributo src a TagBuilder. Tenga en cuenta, además, cómo se usa el método TagBuilder. MergeAttributes () para agregar una colección de atributos al TagBuilder. El método MergeAttributes () acepta un diccionario&lt;cadena, objeto&gt; parámetro. La clase RouteValueDictionary se usa para convertir el objeto que representa la colección de atributos en un diccionario&lt;cadena, objeto&gt;.

Después de crear la aplicación auxiliar de imágenes, puede usar el ayudante en las vistas de MVC de ASP.NET como cualquier otra aplicación auxiliar HTML estándar. La vista de la lista 2 usa la aplicación auxiliar de imágenes para mostrar la misma imagen de una Xbox dos veces (consulte la figura 1). Se llama a la aplicación auxiliar Image () con y sin una colección de atributos HTML.

**Lista 2-Home\Index.aspx**

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample2.aspx)]

[![el cuadro de diálogo nuevo proyecto](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)

**Figura 01**: uso de la aplicación auxiliar de imágenes ([haga clic para ver la imagen de tamaño completo](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))

Tenga en cuenta que debe importar el espacio de nombres asociado a la aplicación auxiliar de imágenes en la parte superior de la vista index. aspx. La aplicación auxiliar se importa con la siguiente directiva:

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample3.aspx)]

> [!div class="step-by-step"]
> [Anterior](creating-custom-html-helpers-cs.md)
> [Siguiente](creating-page-layouts-with-view-master-pages-cs.md)
