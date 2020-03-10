---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Parte 7: agregar características | Microsoft Docs'
author: JoeStagner
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks. En la parte 7 se agregan características adicionales, como la cuenta Revie...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: ffd2b862c727db9572c272b7b21bcc33c822fffa
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78521569"
---
# <a name="part-7-adding-features"></a>Parte 7: agregar características

por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks muestra cómo es extraordinariamente simple la creación de aplicaciones eficaces y escalables para la plataforma .NET. Muestra cómo usar las excelentes características nuevas de ASP.NET 4 para crear una tienda en línea, como compras, desprotección y administración.
> 
> En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks. La parte 7 agrega características adicionales, como la revisión de la cuenta, las revisiones del producto y los "elementos populares" y los controles de usuario "también adquiridos".

## <a id="_Toc260221673"></a>Agregar características

Aunque los usuarios pueden examinar nuestro catálogo, colocar los elementos en su carro de la compra y completar el proceso de retirada, hay una serie de características de soporte técnico que se incluirán para mejorar nuestro sitio.

1. Revisión de la cuenta (lista de pedidos colocados y ver detalles).
2. Agregue contenido específico del contexto a la Página principal.
3. Agregue una característica para permitir que los usuarios revisen los productos en el catálogo.
4. Cree un control de usuario para mostrar los elementos populares y coloque ese control en la Página principal.
5. Cree un control de usuario "comprado también" y agréguelo a la página de detalles del producto.
6. Agregue una página de contacto.
7. Agregue una página about.
8. Error global

## <a id="_Toc260221674"></a>Revisión de la cuenta

En la carpeta "cuenta", cree dos páginas. aspx una denominada OrderList. aspx y la otra denominada OrderDetails. aspx.

OrderList. aspx aprovechará los controles GridView y EntityDataSource de la misma forma que antes.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

EntityDataSource selecciona los registros de la tabla Orders filtrados en el nombre de usuario (vea WhereParameter), que se establece en una variable de sesión cuando el usuario inicia sesión.

Tenga en cuenta también estos parámetros en el HyperlinkField de GridView:

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

Estos parámetros especifican el vínculo a la vista de detalles de pedidos de cada producto que especifica el campo OrderID como parámetro QueryString en la página OrderDetails. aspx.

## <a id="_Toc260221675"></a>OrderDetails. aspx

Usaremos un control EntityDataSource para acceder a los pedidos y un objeto FormView para mostrar los datos de pedidos y otro EntityDataSource con un control GridView para mostrar todos los elementos de línea del pedido.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

En el archivo de código subyacente (OrderDetails.aspx.cs) tenemos dos pequeños bits de mantenimiento.

En primer lugar, debemos asegurarnos de que OrderDetails siempre obtiene un OrderId.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

También necesitamos calcular y mostrar el total del pedido a partir de los artículos de línea.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>Página principal

Vamos a agregar contenido estático a la página default. aspx.

En primer lugar, crearé una carpeta de "contenido" y, dentro de ella, una carpeta de imágenes (y incluiré una imagen que se usará en la Página principal).

En el marcador de posición inferior de la página default. aspx, agregue el marcado siguiente.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>Revisiones del producto

En primer lugar, vamos a agregar un botón con un vínculo a un formulario que se puede usar para escribir una revisión del producto.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

Tenga en cuenta que vamos a pasar el ProductID en la cadena de consulta.

A continuación, vamos a agregar la página denominada ReviewAdd. aspx.

Esta página usará el kit de herramientas de control de ASP.NET AJAX. Si no lo ha hecho todavía, puede descargarlo de [DevExpress](http://devexpress.com/act) y encontrará instrucciones sobre cómo configurar el kit de herramientas para su uso con Visual Studio aquí [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).

En el modo de diseño, arrastre los controles y validadores desde el cuadro de herramientas y genere un formulario como el que aparece a continuación.

![](tailspin-spyworks-part-7/_static/image2.jpg)

El marcado tendrá un aspecto similar al siguiente.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

Ahora que podemos especificar revisiones, muestre esas revisiones en la página del producto.

Agregue este marcado a la página ProductDetails. aspx.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

La ejecución de la aplicación ahora y la navegación a un producto muestra la información del producto, incluidas las opiniones de los clientes.

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>Control de elementos populares (creación de controles de usuario)

Con el fin de aumentar las ventas en el sitio web, agregaremos un par de características a los productos más populares o relacionados.

La primera de estas características será una lista del producto más popular en el catálogo de productos.

Vamos a crear un "control de usuario" para mostrar los elementos más vendidos en la Página principal de la aplicación. Dado que se trata de un control, podemos usarlo en cualquier página arrastrando y colocando el control en el diseñador de Visual Studio en cualquier página que desee.

En el explorador de soluciones de Visual Studio, haga clic con el botón derecho en el nombre de la solución y cree un nuevo directorio denominado "controles". Aunque no es necesario hacerlo, le ayudaremos a mantener el proyecto organizado creando todos nuestros controles de usuario en el directorio "controles".

Haga clic con el botón derecho en la carpeta controles y elija "nuevo elemento":

![](tailspin-spyworks-part-7/_static/image4.jpg)

Especifique un nombre para el control "PopularItems". Tenga en cuenta que la extensión de archivo para los controles de usuario es. ascx no. aspx.

El control de usuario de los elementos más populares se definirá como se indica a continuación.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

Aquí vamos a usar un método que aún no se ha usado en esta aplicación. Usamos el control Repeater y, en lugar de usar un control de origen de datos, enlazamos el control Repeater a los resultados de una consulta LINQ to Entities.

En el código subyacente de nuestro control, hacemos esto como se indica a continuación.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

Tenga en cuenta también esta línea importante en la parte superior del marcado del control.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

Dado que los elementos más populares no cambiarán de un minuto a minuto, podemos agregar una directiva Aching para mejorar el rendimiento de la aplicación. Esta directiva hará que el código de controles solo se ejecute cuando expire la salida almacenada en caché del control. De lo contrario, se usará la versión almacenada en caché de la salida del control.

Ahora todo lo que tenemos que hacer es incluir nuestro nuevo control en la página default. aspx.

Use las opciones de arrastrar y colocar para colocar una instancia del control en la columna abrir del formulario predeterminado.

![](tailspin-spyworks-part-7/_static/image5.jpg)

Ahora, cuando se ejecuta la aplicación, la Página principal muestra los elementos más populares.

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>Control "también adquirido" (controles de usuario con parámetros)

El segundo control de usuario que crearemos llevará a cabo una venta por debajo del siguiente nivel agregando la especificidad del contexto.

La lógica para calcular los elementos principales "comprados también" no es trivial.

El control "también adquirido" seleccionará los registros de OrderDetails (previamente comprados) para el ProductID seleccionado actualmente y tomará el OrderIDs para cada orden único que se encuentre.

A continuación, seleccionaremos los productos de todos los pedidos y sumaremos las cantidades compradas. Ordenaremos los productos por esa suma de cantidad y mostraremos los cinco elementos principales.

Dada la complejidad de esta lógica, se implementará este algoritmo como un procedimiento almacenado.

El T-SQL para el procedimiento almacenado es el siguiente.

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

Tenga en cuenta que este procedimiento almacenado (SelectPurchasedWithProducts) existía en la base de datos cuando lo incluimos en nuestra aplicación y, cuando generamos el Entity Data Model especificamos que, además de las tablas y vistas que necesitábamos, el Entity Data Model debe incluir este procedimiento almacenado.

Para tener acceso al procedimiento almacenado desde el Entity Data Model necesitamos importar la función.

Haga doble clic en el Entity Data Model en el explorador de soluciones para abrirlo en el diseñador y abra el explorador de modelos, haga clic con el botón derecho en el diseñador y seleccione "agregar importación de función".

![](tailspin-spyworks-part-7/_static/image1.png)

Si lo hace, se abrirá este cuadro de diálogo.

![](tailspin-spyworks-part-7/_static/image2.png)

Rellene los campos como se muestra arriba, seleccionando "SelectPurchasedWithProducts" y usando el nombre del procedimiento para el nombre de la función importada.

Haga clic en "Aceptar".

Una vez hecho esto, podemos simplemente programar con el procedimiento almacenado, como podríamos hacer con cualquier otro elemento del modelo.

Por lo tanto, en la carpeta "controles", cree un nuevo control de usuario denominado AlsoPurchased. ascx.

El marcado de este control resultará muy familiar al control PopularItems.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

La diferencia más importante es que no almacena en caché la salida, ya que el elemento que se va a representar será diferente según el producto.

ProductId será una "propiedad" para el control.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

En el controlador de eventos de preprocesamiento del control, se EED hacer tres cosas.

1. Asegúrese de que ProductID está establecido.
2. Compruebe si hay algún producto que se haya comprado con el actual.
3. Genere algunos elementos según se determine en #2.

Tenga en cuenta lo fácil que es llamar al procedimiento almacenado a través del modelo.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

Después de determinar que hay "también adquirido", podemos simplemente enlazar el repetidor a los resultados devueltos por la consulta.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

Si no había ningún elemento "también adquirido", simplemente mostraremos otros elementos populares de nuestro catálogo.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

Para ver los elementos "comprados también", abra la página ProductDetails. aspx y arrastre el control AlsoPurchased desde el explorador de soluciones para que aparezca en esta posición en el marcado.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

Al hacerlo, se creará una referencia al control en la parte superior de la página ProductDetails.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

Dado que el control de usuario AlsoPurchased requiere un número ProductId, estableceremos la propiedad ProductID del control mediante una instrucción eval en el elemento de modelo de datos actual de la página.

![](tailspin-spyworks-part-7/_static/image3.png)

Al compilar y ejecutar ahora y buscar un producto, vemos los elementos "comprados también".

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-6.md)
> [Siguiente](tailspin-spyworks-part-8.md)
