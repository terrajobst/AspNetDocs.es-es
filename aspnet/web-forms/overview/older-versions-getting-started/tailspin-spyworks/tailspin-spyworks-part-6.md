---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Parte 6: pertenencia a ASP.NET | Microsoft Docs'
author: JoeStagner
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks. La parte 6 agrega la pertenencia a ASP.NET.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: b0caa89dc9ffb5bb7451fa2d9d346c7db2bf1466
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78454885"
---
# <a name="part-6-aspnet-membership"></a>Parte 6: pertenencia a ASP.NET

por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks muestra cómo es extraordinariamente simple la creación de aplicaciones eficaces y escalables para la plataforma .NET. Muestra cómo usar las excelentes características nuevas de ASP.NET 4 para crear una tienda en línea, como compras, desprotección y administración.
> 
> En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks. La parte 6 agrega la pertenencia a ASP.NET.

## <a id="_Toc260221672"></a>Trabajar con pertenencia a ASP.NET

![](tailspin-spyworks-part-6/_static/image1.png)

Haga clic en seguridad

![](tailspin-spyworks-part-6/_static/image1.jpg)

Asegúrese de usar la autenticación de formularios.

![](tailspin-spyworks-part-6/_static/image2.jpg)

Use el vínculo "crear usuario" para crear un par de usuarios.

![](tailspin-spyworks-part-6/_static/image3.jpg)

Cuando haya terminado, consulte la ventana de Explorador de soluciones y actualice la vista.

![](tailspin-spyworks-part-6/_static/image2.png)

Tenga en cuenta que ASPNETDB. Se ha creado el estado de MDF. Este archivo contiene las tablas que admiten los servicios principales de ASP.NET, como la pertenencia.

Ahora podemos empezar a implementar el proceso de desprotección.

Empiece por crear una página CheckOut. aspx.

La página CheckOut. aspx solo debe estar disponible para los usuarios que hayan iniciado sesión, por lo que restringiremos el acceso a los usuarios que han iniciado sesión y redirigirá a los usuarios que no hayan iniciado sesión en la página de inicio de sesión.

Para ello, agregaremos lo siguiente a la sección de configuración del archivo Web. config.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

La plantilla para aplicaciones de formularios Web Forms de ASP.NET agregó automáticamente una sección de autenticación al archivo Web. config y estableció la página de inicio de sesión predeterminada.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

Se debe modificar el archivo de código subyacente login. aspx para migrar un carro de la compra anónimo cuando el usuario inicia sesión. Cambie la página\_evento de carga como se indica a continuación.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

Después, agregue un controlador de eventos "Loggedon" como este para establecer el nombre de sesión en el usuario que acaba de iniciar sesión y cambiar el identificador de sesión temporal en el carro de la compra por el del usuario mediante una llamada al método MigrateCart de nuestra clase MyShoppingCart. (Se implementa en el archivo. CS)

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

Implemente el método MigrateCart () como este.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

En Checkout. aspx, usaremos una EntityDataSource y un GridView en nuestra página de extracción, como hicimos en nuestra página de carro de la compra.

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

Tenga en cuenta que el control GridView especifica un controlador de eventos "OnDataBound" denominado mi lista\_RowDataBound, así que vamos a implementar ese controlador de eventos como este.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

Este método mantiene un total actualizado del carro de la compra, ya que cada fila está enlazada y actualiza la fila inferior de GridView.

En esta fase hemos implementado una presentación de "revisión" del pedido que se va a colocar.

Vamos a controlar un escenario de carro vacío agregando algunas líneas de código a nuestra página\_evento de carga:

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

Cuando el usuario haga clic en el botón "enviar", ejecutaremos el siguiente código en el controlador de eventos Click del botón Enviar.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

La "carne" del proceso de envío de pedidos se va a implementar en el método SubmitOrder () de nuestra clase MyShoppingCart.

SubmitOrder:

- Tome todos los elementos de línea en el carro de la compra y úselos para crear un nuevo registro de pedido y los registros OrderDetails asociados.
- Calcular la fecha de envío.
- Borre el carro de la compra.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

Para los fines de esta aplicación de ejemplo, calcularemos una fecha de envío simplemente agregando dos días a la fecha actual.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

Ejecutar la aplicación ahora nos permitirá probar el proceso de compra desde el principio hasta el final.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-5.md)
> [Siguiente](tailspin-spyworks-part-7.md)
