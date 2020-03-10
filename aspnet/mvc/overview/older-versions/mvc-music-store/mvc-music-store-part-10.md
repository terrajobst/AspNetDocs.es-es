---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Parte 10: actualizaciones finales para la navegación y el diseño del sitio, conclusión | Microsoft Docs'
author: jongalloway
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 10 abarca las actualizaciones finales de navegación y...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: f701d1fbabc3e1a97c3750d00e96bf8dba1105cd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433615"
---
# <a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a>Parte 10: actualizaciones finales para la navegación y el diseño del sitio, conclusión

por [Jon Galloway](https://github.com/jongalloway)

> El almacén de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> MVC Music Store es una implementación ligera de almacén de ejemplo que vende álbumes musicales en línea e implementa la funcionalidad básica de administración de sitios, Inicio de sesión de usuario y carro de la compra.  
>   
> En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 10 cubre las actualizaciones finales de la navegación y el diseño del sitio, conclusión.

Hemos completado toda la funcionalidad principal de nuestro sitio, pero todavía tenemos algunas características para agregar a la navegación del sitio, la Página principal y la página de exploración de la tienda.

## <a name="creating-the-shopping-cart-summary-partial-view"></a>Crear la vista parcial de resumen del carro de la compra

Queremos exponer el número de artículos en el carro de la compra del usuario en todo el sitio.

![](mvc-music-store-part-10/_static/image1.png)

Esto se puede implementar fácilmente mediante la creación de una vista parcial que se agrega a nuestro sitio. Master.

Como se mostró anteriormente, el controlador ShoppingCart incluye un método de acción CartSummary que devuelve una vista parcial:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

Para crear la vista parcial CartSummary, haga clic con el botón derecho en la carpeta views/ShoppingCart y seleccione Agregar vista. Asigne a la vista el nombre CartSummary y active la casilla "crear una vista parcial" como se muestra a continuación.

![](mvc-music-store-part-10/_static/image2.png)

La vista parcial CartSummary es realmente sencilla, solo es un vínculo a la vista de índice ShoppingCart, que muestra el número de elementos del carro. El código completo para CartSummary. cshtml es el siguiente:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

Podemos incluir una vista parcial en cualquier página del sitio, incluido el maestro del sitio, mediante el método html. RenderAction. RenderAction requiere que se especifique el nombre de la acción ("CartSummary") y el nombre del controlador ("ShoppingCart") como se indica a continuación.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

Antes de agregarlo al diseño del sitio, también crearemos el menú género para que podamos hacer todas las actualizaciones de site. Master al mismo tiempo.

## <a name="creating-the-genre-menu-partial-view"></a>Crear la vista parcial del menú género

Podemos facilitar a los usuarios la navegación por la tienda mediante la adición de un menú de género que enumera todos los géneros disponibles en nuestra tienda.

![](mvc-music-store-part-10/_static/image3.png)

Seguiremos los mismos pasos también creará una vista parcial de GenreMenu y, a continuación, se pueden agregar ambos al maestro de sitio. En primer lugar, agregue la siguiente acción del controlador GenreMenu a StoreController:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

Esta acción devuelve una lista de géneros que se mostrarán en la vista parcial, que se creará a continuación.

*Nota: hemos agregado el atributo [ChildActionOnly] a esta acción del controlador, lo que indica que solo deseamos usar esta acción desde una vista parcial. Este atributo impedirá que se ejecute la acción de controlador examinando/Store/GenreMenu. Esto no es necesario para las vistas parciales, pero es recomendable, ya que queremos asegurarnos de que las acciones del controlador se usen como se pretende. También devolvemos PartialView en lugar de View, lo que permite que el motor de vistas sepa que no debe usar el diseño de esta vista, ya que se incluye en otras vistas.*

Haga clic con el botón derecho en la acción del controlador GenreMenu y cree una vista parcial denominada GenreMenu que esté fuertemente tipada mediante la clase de datos de la vista Genre, como se muestra a continuación.

![](mvc-music-store-part-10/_static/image4.png)

Actualice el código de vista de la vista parcial GenreMenu para mostrar los elementos mediante una lista sin ordenar como se indica a continuación.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a>Actualizar el diseño del sitio para mostrar nuestras vistas parciales

Podemos agregar nuestras vistas parciales al diseño del sitio (/Views/Shared/\_layout. cshtml) llamando a HTML. RenderAction (). Los agregaremos en, así como algunas marcas adicionales para mostrarlas, como se muestra a continuación:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

Ahora, cuando ejecutemos la aplicación, veremos el género en el área de navegación izquierda y el Resumen de la cesta en la parte superior.

## <a name="update-to-the-store-browse-page"></a>Actualizar a la página de exploración de la tienda

La página de exploración del almacén es funcional, pero no se ve muy bien. Podemos actualizar la página para mostrar los álbumes en un mejor diseño; para ello, actualice el código de vista (que se encuentra en/Views/Store/Browse.cshtml) de la manera siguiente:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

Aquí estamos haciendo uso de URL. Action en lugar de HTML. ActionLink para que podamos aplicar formato especial al vínculo para incluir la ilustración del álbum.

*Nota: se muestra una portada de álbum genérica para estos álbumes. Esta información se almacena en la base de datos y se puede modificar a través del administrador de la tienda. Le agradecemos que agregue su propia ilustración.*

Ahora, cuando examinemos un género, veremos que los álbumes se muestran en una cuadrícula con la ilustración del álbum.

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a>Actualización de la Página principal para mostrar los principales álbumes de ventas

Queremos incluir nuestros principales álbumes de ventas en la Página principal para aumentar las ventas. Realizaremos algunas actualizaciones en el HomeController para controlar eso y agregar también algunos gráficos adicionales.

En primer lugar, vamos a agregar una propiedad de navegación a nuestra clase album para que EntityFramework sepa que están asociadas. Las últimas líneas de la clase **album** deben tener ahora el siguiente aspecto:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

*Nota: Esto requerirá agregar una instrucción using para traer el espacio de nombres System. Collections. Generic.*

En primer lugar, vamos a agregar un campo storeDB y las instrucciones MvcMusicStore. Models con, como en nuestros otros controladores. A continuación, agregaremos el método siguiente al HomeController, que consulta nuestra base de datos para encontrar los álbumes principales de ventas según OrderDetails.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

Se trata de un método privado, ya que no queremos que esté disponible como una acción de controlador. Lo incluimos en el HomeController por simplicidad, pero le recomendamos que mueva la lógica de negocios a clases de servicio independientes, según corresponda.

Con eso en su lugar, podemos actualizar la acción del controlador de índice para consultar los 5 álbumes principales y devolverlos a la vista.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

El código completo para el HomeController actualizado es como se muestra a continuación.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

Por último, deberá actualizar nuestra vista de índice principal para que pueda mostrar una lista de álbumes; para ello, actualice el tipo de modelo y agregue la lista de álbumes a la parte inferior. Tomaremos esta oportunidad para agregar también un encabezado y una sección de promoción a la página.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

Ahora, cuando ejecutemos la aplicación, veremos nuestra página principal actualizada con los principales álbumes de ventas y nuestro mensaje promocional.

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a>Conclusión

Hemos visto que ASP.NET MVC facilita la creación de un sitio web sofisticado con acceso a bases de datos, pertenencia, AJAX, etc. con bastante rapidez. Espero que este tutorial le haya proporcionado las herramientas que necesita para empezar a crear sus propias aplicaciones ASP.NET MVC.

> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-9.md)
