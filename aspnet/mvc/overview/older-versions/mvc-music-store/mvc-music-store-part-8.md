---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Parte 8: carro de la compra con actualizaciones de Ajax | Microsoft Docs'
author: jongalloway
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 8 abarca el carro de la compra con actualizaciones de Ajax.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 89897ad41b217764cbd17317d4bf5d6a5c5d488f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433519"
---
# <a name="part-8-shopping-cart-with-ajax-updates"></a>Parte 8: carro de la compra con actualizaciones de Ajax

por [Jon Galloway](https://github.com/jongalloway)

> El almacén de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> MVC Music Store es una implementación ligera de almacén de ejemplo que vende álbumes musicales en línea e implementa la funcionalidad básica de administración de sitios, Inicio de sesión de usuario y carro de la compra.  
>   
> En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 8 abarca el carro de la compra con actualizaciones de Ajax.

Permitiremos a los usuarios colocar álbumes en el carro sin registrarse, pero tendrán que registrarse como invitados para completar la desprotección. El proceso de compra y retirada se divide en dos controladores: un controlador ShoppingCart que permite agregar elementos de forma anónima a un carro y un controlador de desprotección que controla el proceso de desprotección. Comenzaremos con el carro de la compra en esta sección y, a continuación, compilaremos el proceso de desprotección en la sección siguiente.

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a>Adición de las clases de modelo Cart, order y OrderDetail

Nuestro carro de la compra y los procesos de retirada harán uso de algunas clases nuevas. Haga clic con el botón secundario en la carpeta models y agregue una clase Cart (Cart.cs) con el código siguiente.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

Esta clase es bastante similar a la de los demás que hemos usado hasta ahora, excepto el atributo [Key] de la propiedad RecordId. Nuestros elementos del carro tendrán un identificador de cadena denominado CartID para permitir compras anónimas, pero la tabla incluye una clave principal de entero denominada RecordId. Por Convención, Entity Framework Code-First espera que la clave principal de una tabla denominada Cart sea CartId o ID, pero se puede reemplazar fácilmente por las anotaciones o el código si se desea. Este es un ejemplo de cómo podemos usar las convenciones simples en Entity Framework Code-First cuando nos acompañan, pero no están restringidos por ellos cuando no lo están.

A continuación, agregue una clase de pedido (Order.cs) con el código siguiente.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

Esta clase realiza un seguimiento de la información de Resumen y entrega de un pedido. **No se compilará todavía**, porque tiene una propiedad de navegación OrderDetails que depende de una clase que aún no se ha creado. Vamos a corregir esto ahora agregando una clase denominada OrderDetail.cs, agregando el código siguiente.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

Haremos una última actualización a nuestra clase MusicStoreEntities para incluir DbSets que exponga las nuevas clases de modelo, incluido también un DbSet&lt;intérprete&gt;. La clase MusicStoreEntities actualizada aparece como se muestra a continuación.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a>Administrar la lógica de negocios de la cesta de la compra

A continuación, vamos a crear la clase ShoppingCart en la carpeta models. El modelo ShoppingCart controla el acceso a los datos a la tabla Cart. Además, administrará la lógica de negocios para agregar y quitar elementos del carro de la compra.

Dado que no es necesario que los usuarios se suscriban a una cuenta solo para agregar elementos a su carro de la compra, se asignará a los usuarios un identificador único temporal (mediante un GUID o un identificador único global) cuando accedan al carro de la compra. Almacenaremos este identificador mediante la clase de sesión ASP.NET.

*Nota: la sesión ASP.NET es un lugar adecuado para almacenar información específica del usuario que expirará después de abandonar el sitio. Aunque el uso incorrecto del estado de la sesión puede tener implicaciones en el rendimiento de los sitios de mayor tamaño, nuestro ligero uso funcionará bien con fines de demostración.*

La clase ShoppingCart expone los métodos siguientes:

**AddToCart** toma un álbum como parámetro y lo agrega al carro del usuario. Dado que la tabla Cart realiza un seguimiento de la cantidad de cada álbum, incluye la lógica para crear una nueva fila si es necesario o simplemente incrementar la cantidad si el usuario ya ha pedido una copia del álbum.

**RemoveFromCart** toma un identificador de álbum y lo quita del carro del usuario. Si el usuario solo tenía una copia del álbum en el carro, se quita la fila.

**EmptyCart** quita todos los elementos del carro de la compra de un usuario.

**GetCartItems** recupera una lista de CartItems para su presentación o procesamiento.

**GetCount** recupera el número total de álbumes que un usuario tiene en su carro de la compra.

**GetTotal** calcula el costo total de todos los elementos del carro.

**CreateOrder** convierte el carro de la compra en un pedido durante la fase de desprotección.

**GetCart** es un método estático que permite a nuestros controladores obtener un objeto Cart. Usa el método **GetCartId** para controlar la lectura de CartId desde la sesión del usuario. El método GetCartId requiere HttpContextBase para que pueda leer el CartId del usuario de la sesión del usuario.

Esta es la **clase ShoppingCart**completa:

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a>ViewModels

Nuestro controlador de carro de la compra deberá comunicar información compleja a sus vistas, lo que no se asigna correctamente a los objetos del modelo. No queremos modificar nuestros modelos para que se adapten a nuestras vistas; Las clases de modelo deben representar nuestro dominio, no la interfaz de usuario. Una solución sería pasar la información a nuestras vistas mediante la clase ViewBag, como hicimos con la información de la lista desplegable del administrador de la tienda, pero pasar mucha información a través de ViewBag es difícil de administrar.

Una solución a esto es usar el patrón *ViewModel* . Al usar este patrón, creamos clases fuertemente tipadas que están optimizadas para nuestros escenarios de vista específicos y que exponen propiedades para los valores dinámicos y el contenido que necesitan nuestras plantillas de vista. A continuación, las clases de controlador pueden rellenar y pasar estas clases optimizadas para vistas a nuestra plantilla de vista que se va a usar. Esto permite la seguridad de tipos, la comprobación en tiempo de compilación y el IntelliSense del editor dentro de las plantillas de vista.

Crearemos dos modelos de vista para su uso en nuestro controlador de carro de la compra: el ShoppingCartViewModel contendrá el contenido del carro de la compra del usuario y ShoppingCartRemoveViewModel se usará para mostrar información de confirmación cuando un usuario quite algo desde el carro.

Vamos a crear una nueva carpeta ViewModels en la raíz de nuestro proyecto para mantener las cosas organizadas. Haga clic con el botón derecho en el proyecto, seleccione Agregar o nueva carpeta.

![](mvc-music-store-part-8/_static/image1.jpg)

Asigne a la carpeta el nombre ViewModels.

![](mvc-music-store-part-8/_static/image1.png)

A continuación, agregue la clase ShoppingCartViewModel en la carpeta ViewModels. Tiene dos propiedades: una lista de elementos del carro y un valor decimal para contener el precio total de todos los artículos del carro.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

Ahora, agregue ShoppingCartRemoveViewModel a la carpeta ViewModels, con las cuatro propiedades siguientes.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a>Controlador de carro de la compra

El controlador de carro de la compra tiene tres propósitos principales: agregar elementos a un carro, quitar elementos del carro y ver los elementos del carro. Usará las tres clases que acabamos de crear: ShoppingCartViewModel, ShoppingCartRemoveViewModel y ShoppingCart. Como en StoreController y StoreManagerController, agregaremos un campo que contenga una instancia de MusicStoreEntities.

Agregue un nuevo controlador de carro de la compra al proyecto mediante la plantilla de controlador vacía.

![](mvc-music-store-part-8/_static/image2.png)

Este es el controlador ShoppingCart completo. Las acciones index y Add Controller deben tener un aspecto muy familiar. Las acciones de controlador Remove y CartSummary controlan dos casos especiales, que trataremos en la sección siguiente.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a>Actualizaciones de AJAX con jQuery

A continuación, crearemos una página de índice de carro de la compra fuertemente tipada a ShoppingCartViewModel y utilizará la plantilla de vista de lista con el mismo método que antes.

![](mvc-music-store-part-8/_static/image3.png)

Sin embargo, en lugar de usar un elemento HTML. ActionLink para quitar elementos del carro, usaremos jQuery para "conectar" el evento de clic de todos los vínculos de esta vista que tengan la clase HTML RemoveLink. En lugar de publicar el formulario, este controlador de eventos de clic solo realizará una devolución de llamada AJAX a nuestra acción del controlador RemoveFromCart. RemoveFromCart devuelve un resultado serializado de JSON, que la devolución de llamada de jQuery analiza y realiza cuatro actualizaciones rápidas en la página con jQuery:

- 1. Quita el álbum eliminado de la lista.
- 2. Actualiza el número de carros en el encabezado.
- 3. Muestra un mensaje de actualización al usuario
- 4. Actualiza el precio total del carro

Dado que el escenario de eliminación está siendo controlado por una devolución de llamada de Ajax en la vista de índice, no se necesita una vista adicional para la acción RemoveFromCart. Este es el código completo de la vista/ShoppingCart/Index:

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

Para probar esto, es necesario poder agregar elementos al carro de la compra. Actualizaremos la vista de detalles de la **tienda** para incluir un botón "agregar al carro". Aunque estamos en él, podemos incluir parte de la información adicional del álbum que hemos agregado desde la última vez que se actualizó esta vista: género, intérprete, precio y carátula del álbum. El código de vista de detalles del almacén actualizado aparece como se muestra a continuación.

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

Ahora podemos hacer clic en la tienda y probar agregar y quitar álbumes en el carro de la compra. Ejecute la aplicación y vaya al índice de la tienda.

![](mvc-music-store-part-8/_static/image4.png)

A continuación, haga clic en un género para ver una lista de álbumes.

![](mvc-music-store-part-8/_static/image5.png)

Al hacer clic en el título de un álbum, se muestra la vista de detalles del álbum actualizada, incluido el botón "agregar al carro".

![](mvc-music-store-part-8/_static/image6.png)

Al hacer clic en el botón "agregar al carro", se muestra nuestra vista de índice de carro de la compra con la lista de resumen del carro de la compra.

![](mvc-music-store-part-8/_static/image7.png)

Después de cargar el carro de la compra, puede hacer clic en el vínculo quitar del carro para ver la actualización de Ajax en el carro de la compra.

![](mvc-music-store-part-8/_static/image8.png)

Hemos creado un carro de la compra que funciona, lo que permite a los usuarios no registrados agregar elementos a su carro. En la sección siguiente, se les permitirá registrar y completar el proceso de desprotección.

> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-7.md)
> [Siguiente](mvc-music-store-part-9.md)
