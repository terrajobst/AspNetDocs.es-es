---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Parte 4: enumeración de productos | Microsoft Docs'
author: JoeStagner
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks. En la parte 4 se describe la lista de productos con el contr de GridView...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 7af1b8afa2ecc8df9846f2edd2091b26b93a811c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78457285"
---
# <a name="part-4-listing-products"></a>Parte 4: lista de productos

por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks muestra cómo es extraordinariamente simple la creación de aplicaciones eficaces y escalables para la plataforma .NET. Muestra cómo usar las excelentes características nuevas de ASP.NET 4 para crear una tienda en línea, como compras, desprotección y administración.
> 
> En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks. En la parte 4 se describe la lista de productos con el control GridView.

## <a id="_Toc260221670"></a>Enumerar productos con el control GridView

Comencemos a implementar nuestra página de ProductsList. aspx haciendo clic con el botón derecho en la solución y seleccionando "agregar" y "nuevo elemento".

![](tailspin-spyworks-part-4/_static/image1.jpg)

Elija "formulario web usando la página maestra" y escriba un nombre de página de ProductsList. aspx ".

Haga clic en "agregar".

![](tailspin-spyworks-part-4/_static/image2.jpg)

A continuación, elija la carpeta "Styles" donde colocamos la página site. Master y selecciónela en la ventana "contenido de la carpeta".

![](tailspin-spyworks-part-4/_static/image3.jpg)

Haga clic en "Aceptar" para crear la página.

Nuestra base de datos se rellena con datos de productos, tal como se muestra a continuación.

![](tailspin-spyworks-part-4/_static/image4.jpg)

Después de crear la página, volveremos a usar un origen de datos de entidad para acceder a los datos del producto, pero en esta instancia necesitamos seleccionar las entidades Product y necesitamos restringir los elementos que se devuelven solo a los de la categoría seleccionada.

Para ello, indicaremos a EntityDataSource que genere automáticamente la cláusula WHERE y especificaremos WhereParameter.

Recordará que cuando creamos los elementos de menú en nuestro "menú de categoría de producto", creamos dinámicamente el vínculo agregando el CategoryID a la cadena de incorporación de cada vínculo. Se indicará al origen de datos de la entidad que derive el parámetro WHERE de ese parámetro QueryString.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

A continuación, configuraremos el control ListView para mostrar una lista de productos. Para crear una experiencia de compra óptima, compactaremos varias características concisas en cada producto individual mostrado en nuestro ListVew.

- El nombre del producto será un vínculo a la vista de detalles del producto.
- Se mostrará el precio del producto.
- Se mostrará una imagen del producto y seleccionaremos dinámicamente la imagen en un directorio de imágenes de catálogo en nuestra aplicación.
- Se incluirá un vínculo para agregar inmediatamente el producto específico al carro de la compra.

Este es el marcado de la instancia del control ListView.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

Vamos a crear dinámicamente varios vínculos para cada producto mostrado.

Además, antes de probar la nueva página, es necesario crear la estructura de directorios para las imágenes del catálogo de productos como se indica a continuación.

![](tailspin-spyworks-part-4/_static/image1.png)

Una vez que se pueda acceder a nuestras imágenes de producto, podemos probar nuestra página de lista de productos.

![](tailspin-spyworks-part-4/_static/image5.jpg)

En la Página principal del sitio, haga clic en uno de los vínculos de la lista de categorías.

![](tailspin-spyworks-part-4/_static/image6.jpg)

Ahora es necesario implementar la página ProductDetails. aspx y la funcionalidad AddToCart.

Use el&gt;de archivo nuevo para crear un nombre de página ProductDetails. aspx mediante la página maestra del sitio como hicimos anteriormente.

De nuevo, usaremos un control EntityDataSource para tener acceso al registro del producto específico en la base de datos y usaremos un control ASP.NET FormView para mostrar los datos del producto como se indica a continuación.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

No se preocupe si el formato le parece un poco divertido. El marcado anterior deja espacio en el diseño de la pantalla para un par de características que se implementarán más adelante.

El carro de la compra representa la lógica más compleja de nuestra aplicación. Para empezar, use el&gt;de archivo nuevo para crear una página denominada MyShoppingCart. aspx.

Tenga en cuenta que no estamos eligiendo el nombre ShoppingCart. aspx.

Nuestra base de datos contiene una tabla denominada "ShoppingCart". Cuando se genera una Entity Data Model se creó una clase para cada tabla en la base de datos. Por lo tanto, el Entity Data Model genera una clase de entidad denominada "ShoppingCart". Podríamos editar el modelo para que podamos usar ese nombre para nuestra implementación de la cesta de la compra o ampliarlo según nuestras necesidades, pero optaremos por simplemente seleccionar un nombre que evite el conflicto.

También merece la pena tener en cuenta que vamos a crear un carro de la compra sencillo e insertar la lógica del carro de la compra con la pantalla del carro de la compra. También podemos optar por implementar el carro de la compra en un nivel de negocio completamente independiente.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-3.md)
> [Siguiente](tailspin-spyworks-part-5.md)
