---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Parte 5: lógica de negocios | Microsoft Docs'
author: JoeStagner
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks. La parte 5 agrega algunas lógica de negocios.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: c60eece9223c47304786d7b0d0ca4b11ac0572e9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78511549"
---
# <a name="part-5-business-logic"></a>Parte 5: lógica de negocios

por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks muestra cómo es extraordinariamente simple la creación de aplicaciones eficaces y escalables para la plataforma .NET. Muestra cómo usar las excelentes características nuevas de ASP.NET 4 para crear una tienda en línea, como compras, desprotección y administración.
> 
> En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks. La parte 5 agrega algunas lógica de negocios.

## <a id="_Toc260221671"></a>Adición de lógica de negocios

Queremos que nuestra experiencia de compra esté disponible cada vez que alguien visite nuestro sitio Web. Los visitantes podrán examinar y agregar elementos al carro de la compra, aunque no estén registrados o hayan iniciado sesión. Cuando estén preparados para desprotegerlos, se les ofrecerá la opción de autenticación y, si aún no son miembros, podrán crear una cuenta.

Esto significa que se debe implementar la lógica para convertir el carro de la compra de un estado anónimo a un estado "usuario registrado".

Vamos a crear un directorio denominado "classes", luego haga clic con el botón derecho en la carpeta y cree un nuevo archivo "class" denominado MyShoppingCart.cs

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

Como se mencionó anteriormente, extenderemos la clase que implementa la página MyShoppingCart. aspx y haremos esto con. La eficaz construcción "clase parcial" de NET.

La llamada generada para el archivo MyShoppingCart.aspx.cf tiene el siguiente aspecto.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

Tenga en cuenta el uso de la palabra clave "Partial".

El archivo de clase que acabamos de generar tiene el siguiente aspecto.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

Combinaremos nuestras implementaciones agregando también la palabra clave Partial a este archivo.

Nuestro nuevo archivo de clase ahora tiene este aspecto.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

El primer método que agregaremos a nuestra clase es el método "AddItem". Este es el método al que se llamará en última instancia cuando el usuario haga clic en los vínculos "Agregar a arte" en la lista de productos y en las páginas de detalles del producto.

Anexe lo siguiente a las instrucciones Using en la parte superior de la página.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

Y agregue este método a la clase MyShoppingCart.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

Estamos usando LINQ to Entities para ver si el elemento ya está en el carro. Si es así, se actualiza la cantidad de pedido del elemento; de lo contrario, se crea una nueva entrada para el elemento seleccionado.

Para llamar a este método, se implementará una página AddToCart. aspx que no solo clase este método, sino que se muestra la compra actual a = carro después de agregar el elemento.

Haga clic con el botón derecho en el nombre de la solución en el explorador de soluciones y agregue y una nueva página denominada AddToCart. aspx como hemos hecho anteriormente.

Aunque podríamos usar esta página para mostrar los resultados provisionales, como los problemas de baja existencias, etc., en nuestra implementación, la página no se representará realmente, sino que llamaremos a la lógica de "adición" y a la redirección.

Para ello, agregaremos el código siguiente a la página\_evento Load.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

Tenga en cuenta que estamos recuperando el producto para agregarlo al carro de la compra desde un parámetro QueryString y llamando al método AddItem de nuestra clase.

Si no se produce ningún error, el control se pasa a la página SHoppingCart. aspx que se implementará a continuación. Si se produce un error, se producirá una excepción.

En la actualidad todavía no hemos implementado un controlador de errores global, por lo que esta excepción no se controlaría en nuestra aplicación, pero solucionaremos esto en breve.

Tenga en cuenta también el uso de la instrucción Debug. FAIL () (disponible a través de `using System.Diagnostics;)`

¿La aplicación se ejecuta en el depurador), este método mostrará un cuadro de diálogo detallado con información sobre el estado de las aplicaciones junto con el mensaje de error que se especifica.

Cuando se ejecuta en producción, se omite la instrucción Debug. FAIL ().

Observará en el código anterior a una llamada a un método en nuestros nombres de clase de carro de la compra "GetShoppingCartId".

Agregue el código para implementar el método como se indica a continuación.

Tenga en cuenta que también hemos agregado botones actualizar y desproteger y una etiqueta en la que se puede mostrar el carro "total".

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

Ahora podemos agregar elementos al carro de la compra, pero no hemos implementado la lógica para mostrar el carro una vez que se ha agregado un producto.

Por lo tanto, en la página MyShoppingCart. aspx agregaremos un control EntityDataSource y un control GridVire como se indica a continuación.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

Llame al formulario en el diseñador para que pueda hacer doble clic en el botón actualizar carro y generar el controlador de eventos Click que se especifica en la declaración en el marcado.

Implementaremos los detalles más adelante, pero esto nos permitirá compilar y ejecutar la aplicación sin errores.

Al ejecutar la aplicación y agregar un elemento al carro de la compra, verá esto.

![](tailspin-spyworks-part-5/_static/image2.jpg)

Tenga en cuenta que hemos desviado la presentación de la cuadrícula "predeterminada" mediante la implementación de tres columnas personalizadas.

El primero es un campo "enlazado" que se puede editar para la cantidad:

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

La siguiente es una columna "calculada" que muestra el total del artículo de la línea (el costo del artículo, la cantidad que se va a ordenar):

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

Por último, tenemos una columna personalizada que contiene un control CheckBox que el usuario usará para indicar que el elemento se debe quitar del gráfico de compras.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

Como puede ver, la línea Order total está vacía, así que vamos a agregar cierta lógica para calcular el total del pedido.

Primero implementaremos un método "GetTotal" para nuestra clase MyShoppingCart.

En el archivo MyShoppingCart.cs, agregue el código siguiente.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

Después, en la página\_controlador de eventos de carga, podremos llamar al método GetTotal. Al mismo tiempo, agregaremos una prueba para ver si el carro de la compra está vacío y ajustar la presentación en consecuencia si es así.

Ahora, si el carro de la compra está vacío, obtenemos lo siguiente:

![](tailspin-spyworks-part-5/_static/image4.jpg)

Y, si no es así, vemos nuestro total.

![](tailspin-spyworks-part-5/_static/image5.jpg)

Sin embargo, esta página no se ha completado todavía.

Necesitaremos lógica adicional para recalcular el carro de la compra mediante la eliminación de los elementos marcados para su eliminación y la determinación de los nuevos valores de cantidad, ya que el usuario puede haber cambiado algunos en la cuadrícula.

Permite agregar un método "RemoveItem" a nuestra clase de carro de la compra de MyShoppingCart.cs para controlar el caso en el que un usuario marca un elemento para su eliminación.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

Ahora vamos a crear un método para controlar la circunstancia cuando un usuario simplemente cambia la calidad que se va a ordenar en GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

Con las características básicas de eliminación y actualización, podemos implementar la lógica que realmente actualiza el carro de la compra en la base de datos. (En MyShoppingCart.cs)

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

Observará que este método espera dos parámetros. Uno es el identificador del carro de la compra y el otro es una matriz de objetos de tipo definido por el usuario.

Para minimizar la dependencia de nuestra lógica en los detalles de la interfaz de usuario, se ha definido una estructura de datos que se puede usar para pasar los elementos del carro de la compra a nuestro código sin que nuestro método tenga que acceder directamente al control GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

En nuestro archivo MyShoppingCart.aspx.cs podemos usar esta estructura en el controlador de eventos Click del botón actualizar como se indica a continuación. Tenga en cuenta que, además de actualizar el carro, actualizaremos también el total del carro.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

Tenga en cuenta que esta línea de código le interesa concretamente:

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

GetValues () es una función auxiliar especial que se implementará en MyShoppingCart.aspx.cs como se indica a continuación.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

Esto proporciona una manera limpia de tener acceso a los valores de los elementos enlazados en el control GridView. Puesto que el control de la casilla "quitar elemento" no está enlazado, se tendrá acceso a él a través del método FindControl ().

En esta fase del desarrollo del proyecto, estamos preparando para implementar el proceso de desprotección.

Antes de hacerlo, vamos a usar Visual Studio para generar la base de datos de pertenencia y agregar un usuario al repositorio de pertenencia.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-4.md)
> [Siguiente](tailspin-spyworks-part-6.md)
