---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Parte 9: registro y desprotección | Microsoft Docs'
author: jongalloway
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 9 cubre el registro y la desprotección.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 040bc0ccef889fb9a7c3d9b5ce88c75b7b754248
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78450901"
---
# <a name="part-9-registration-and-checkout"></a>Parte 9: registro y desprotección

por [Jon Galloway](https://github.com/jongalloway)

> El almacén de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> MVC Music Store es una implementación ligera de almacén de ejemplo que vende álbumes musicales en línea e implementa la funcionalidad básica de administración de sitios, Inicio de sesión de usuario y carro de la compra.  
>   
> En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 9 cubre el registro y la desprotección.

En esta sección, se creará un CheckoutController que recopilará la dirección del comprador y la información de pago. Se requerirá a los usuarios que se registren en nuestro sitio antes de desprotegerlo, por lo que este controlador necesitará autorización.

Los usuarios navegarán hasta el proceso de desprotección desde el carro de la compra haciendo clic en el botón "desproteger".

![](mvc-music-store-part-9/_static/image1.jpg)

Si el usuario no ha iniciado sesión, se le pedirá que lo indique.

![](mvc-music-store-part-9/_static/image1.png)

Una vez que el inicio de sesión se realiza correctamente, se muestra la vista dirección y pago en el usuario.

![](mvc-music-store-part-9/_static/image2.png)

Una vez que haya rellenado el formulario y enviado el pedido, se mostrará la pantalla de confirmación del pedido.

![](mvc-music-store-part-9/_static/image3.png)

Al intentar ver un pedido que no existe o un pedido que no pertenece a, se mostrará la vista de error.

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a>Migración del carro de la compra

Aunque el proceso de compra es anónimo, cuando el usuario hace clic en el botón de desprotección, se le pedirá que registre e inicie sesión. Los usuarios esperarán que mantengan la información de su carro de la compra entre visitas, por lo que tendremos que asociar la información del carro de la compra con un usuario cuando completen el registro o el inicio de sesión.

Esto es realmente muy sencillo, ya que nuestra clase ShoppingCart ya tiene un método que asociará todos los elementos del carro actual con un nombre de usuario. Solo tendremos que llamar a este método cuando un usuario complete el registro o el inicio de sesión.

Abra la clase **AccountController** que hemos agregado al configurar la pertenencia y la autorización. Agregue una instrucción using que haga referencia a MvcMusicStore. Models y, después, agregue el siguiente método MigrateShoppingCart:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

A continuación, modifique la acción de inicio de sesión para llamar a MigrateShoppingCart una vez que se haya validado el usuario, como se muestra a continuación:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

Realice el mismo cambio en la acción registrar entrada inmediatamente después de que se haya creado correctamente la cuenta de usuario:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

Es decir, ahora un carro de la compra anónimo se transferirá automáticamente a una cuenta de usuario al registrarse o iniciar sesión correctamente.

## <a name="creating-the-checkoutcontroller"></a>Crear CheckoutController

Haga clic con el botón derecho en la carpeta Controllers y agregue un nuevo controlador al proyecto denominado CheckoutController mediante la plantilla de controlador vacía.

![](mvc-music-store-part-9/_static/image5.png)

En primer lugar, agregue el atributo Authorize por encima de la declaración de clase de controlador para requerir que los usuarios se registren antes de la desprotección:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

*Nota: esto es similar al cambio realizado anteriormente en el StoreManagerController, pero en ese caso el atributo Authorize requiere que el usuario tenga un rol de administrador. En el controlador de desprotección, se requiere que el usuario inicie sesión pero no requiere que sean administradores.*

Por motivos de simplicidad, no trataremos la información de pago en este tutorial. En su lugar, se permite a los usuarios consultar mediante un código promocional. Almacenaremos este código promocional mediante una constante denominada PromoCode.

Como en StoreController, se declarará un campo que contenga una instancia de la clase MusicStoreEntities, denominada storeDB. Para hacer uso de la clase MusicStoreEntities, debemos agregar una instrucción using para el espacio de nombres MvcMusicStore. Models. A continuación se muestra la parte superior del controlador de retirada.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

CheckoutController tendrá las siguientes acciones de controlador:

**AddressAndPayment (get Method)** mostrará un formulario para permitir que el usuario escriba su información.

**AddressAndPayment (método post)** validará la entrada y procesará el pedido.

**Completar** se mostrará después de que un usuario haya finalizado correctamente el proceso de desprotección. Esta vista incluirá el número de pedido del usuario, como confirmación.

En primer lugar, vamos a cambiar el nombre de la acción del controlador de índice (que se generó cuando se creó el controlador) a AddressAndPayment. Esta acción del controlador solo muestra el formulario de desprotección, por lo que no requiere información del modelo.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

Nuestro método POST de AddressAndPayment seguirá el mismo patrón que usamos en StoreManagerController: intentará aceptar el envío del formulario y completar el pedido, y volverá a mostrar el formulario si se produce un error.

Después de validar que la entrada del formulario cumple nuestros requisitos de validación para un pedido, se comprobará el valor del formulario PromoCode directamente. Suponiendo que todo sea correcto, guardaremos la información actualizada con el pedido, le indicaremos al objeto ShoppingCart que complete el proceso de pedido y le redirigirá a la acción completa.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

Una vez finalizado correctamente el proceso de desprotección, se redirigirá a los usuarios a la acción de controlador completa. Esta acción realizará una comprobación simple para validar que el pedido pertenece realmente al usuario que ha iniciado sesión antes de mostrar el número de pedido como una confirmación.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

*Nota: la vista de error se creó automáticamente en la carpeta/Views/Shared cuando comenzó el proyecto.*

El código de CheckoutController completo es el siguiente:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a>Adición de la vista AddressAndPayment

Ahora, vamos a crear la vista AddressAndPayment. Haga clic con el botón derecho en una de las acciones del controlador AddressAndPayment y agregue una vista denominada AddressAndPayment que esté fuertemente tipada como un orden y use la plantilla Edit, como se muestra a continuación.

![](mvc-music-store-part-9/_static/image6.png)

Esta vista hará uso de dos de las técnicas que hemos examinado durante la compilación de la vista StoreManagerEdit:

- Usaremos HTML. EditorForModel () para mostrar campos de formulario para el modelo de pedido
- Se aprovecharán las reglas de validación mediante una clase Order con atributos de validación

Comenzaremos por actualizar el código de formulario para usar HTML. EditorForModel (), seguido de un cuadro de texto adicional para el código de promoción. A continuación se muestra el código completo de la vista AddressAndPayment.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a>Definir reglas de validación para el pedido

Ahora que nuestra vista está configurada, se configurarán las reglas de validación para nuestro modelo de pedido, tal y como hicimos anteriormente para el modelo de álbumes. Haga clic con el botón derecho en la carpeta modelos y agregue una clase denominada order. Además de los atributos de validación que se usaron anteriormente para el álbum, también se usará una expresión regular para validar la dirección de correo electrónico del usuario.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

Al intentar enviar el formulario con información que falta o no es válida, ahora se muestra un mensaje de error mediante la validación del lado cliente.

![](mvc-music-store-part-9/_static/image7.png)

Bien, hemos realizado la mayor parte del trabajo duro para el proceso de desprotección; solo tenemos algunas probabilidades y acaban de finalizar. Necesitamos agregar dos vistas simples, y es necesario encargarse de la entrega de la información del carro durante el proceso de inicio de sesión.

## <a name="adding-the-checkout-complete-view"></a>Adición de la vista de desprotección completada

La vista de desprotección completa es bastante sencilla, ya que solo necesita mostrar el identificador de pedido. Haga clic con el botón derecho en la acción de controlador completa y agregue una vista denominada completo que esté fuertemente tipada como un valor int.

![](mvc-music-store-part-9/_static/image8.png)

Ahora actualizaremos el código de vista para mostrar el identificador de pedido, como se muestra a continuación.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a>Actualización de la vista de errores

La plantilla predeterminada incluye una vista de error en la carpeta vistas compartidas para que se pueda volver a usar en cualquier parte del sitio. Esta vista de error contiene un error muy sencillo y no utiliza nuestro diseño del sitio, por lo que la actualizaremos.

Dado que se trata de una página de error genérica, el contenido es muy sencillo. Incluiremos un mensaje y un vínculo para navegar a la página anterior en el historial si el usuario desea volver a intentar la acción.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]

> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-8.md)
> [Siguiente](mvc-music-store-part-10.md)
