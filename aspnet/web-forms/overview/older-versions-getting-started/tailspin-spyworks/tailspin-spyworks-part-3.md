---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Parte 3: menú diseño y categoría | Microsoft Docs'
author: JoeStagner
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks. La parte 3 cubre la adición del diseño y un menú de categorías.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: a223b97fd362ecf73ecde431e141021c1dcc6a6d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78519103"
---
# <a name="part-3-layout-and-category-menu"></a>Parte 3: menú diseño y categoría

por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks muestra cómo es extraordinariamente simple la creación de aplicaciones eficaces y escalables para la plataforma .NET. Muestra cómo usar las excelentes características nuevas de ASP.NET 4 para crear una tienda en línea, como compras, desprotección y administración.
> 
> En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks. La parte 3 cubre la adición del diseño y un menú de categorías.

## <a id="_Toc260221669"></a>Agregar algunos diseños y un menú de categorías

En nuestra página maestra del sitio, agregaremos un div para la columna del lado izquierdo que contendrá nuestro menú de categoría de producto.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

Tenga en cuenta que la clase CSS que se agregó al archivo Style. CSS proporcionará la alineación deseada y otros formatos.

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

El menú categoría de producto se creará dinámicamente en tiempo de ejecución mediante la consulta de la base de datos de comercio para las categorías de producto existentes y la creación de los elementos de menú y los vínculos correspondientes.

Para ello, usaremos dos de ASP. Controles de datos eficaces de la red. El control "origen de datos de entidad" y el control "ListView".

![](tailspin-spyworks-part-3/_static/image1.jpg)

Vamos a cambiar a "vista de diseño" y usar las aplicaciones auxiliares para configurar nuestros controles.

![](tailspin-spyworks-part-3/_static/image2.jpg)

Vamos a establecer la propiedad ID de EntityDataSource en el menú EDS\_categoría\_y hacer clic en "Configurar origen de datos".

![](tailspin-spyworks-part-3/_static/image3.jpg)

Seleccione la conexión CommerceEntities que se creó para nosotros cuando creamos el modelo de origen de datos de entidad para nuestra base de datos de comercio y hacemos clic en "siguiente".

![](tailspin-spyworks-part-3/_static/image4.jpg)

Seleccione el nombre del conjunto de entidades "Categories" y deje el resto de las opciones como predeterminado. Haga clic en "finalizar".

Ahora vamos a establecer la propiedad ID de la instancia del control ListView que colocamos en la página en ListView\_ProductsMenu y activamos su aplicación auxiliar.

![](tailspin-spyworks-part-3/_static/image5.jpg)

Aunque podríamos usar opciones de control para dar formato a la presentación y el formato de los elementos de datos, nuestra creación de menús solo requerirá un marcado sencillo, por lo que escribiremos el código en la vista Código fuente.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

Tenga en cuenta la instrucción "eval": &lt;% # eval ("CategoryName")%&gt;

La sintaxis ASP.NET &lt;% #%&gt; es una Convención abreviada que indica al Runtime que ejecute lo que contenga y genere los resultados "en línea".

La instrucción eval ("CategoryName") indica a que, para la entrada actual de la colección enlazada de elementos de datos, recupere el valor de los nombres de elemento de modelo de entidad "CategoryName". Esta es una sintaxis concisa para una característica muy eficaz.

Permite ejecutar la aplicación ahora.

![](tailspin-spyworks-part-3/_static/image6.jpg)

Tenga en cuenta que ahora se muestra nuestro menú de categoría de producto y cuando hacemos desplazarse sobre uno de los elementos de menú de categoría, podemos ver que el vínculo de elemento de menú apunta a una página que aún no se ha implementado con el nombre ProductsList. aspx y que hemos creado un argumento de cadena de consulta dinámica que contiene el  identificador de categoría.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-2.md)
> [Siguiente](tailspin-spyworks-part-4.md)
