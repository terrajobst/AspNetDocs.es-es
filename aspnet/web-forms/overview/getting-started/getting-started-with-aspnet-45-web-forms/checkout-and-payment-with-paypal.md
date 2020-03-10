---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: Retirada y pago con PayPal | Microsoft Docs
author: Erikre
description: Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4,5 y Microsoft Visual Studio Express 2013 para nosotros...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: 62d00a86c6c5845fb894896df65002c7086d039f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78455935"
---
# <a name="checkout-and-payment-with-paypal"></a>Finalización de la compra y pago con PayPal

por [Erik Reitan](https://github.com/Erikre)

[Descargar el proyecto de ejemplo deC#Wingtip Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar el libro electrónico (pdf)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4,5 y Microsoft Visual Studio Express 2013 para Web. Hay disponible un [proyecto C# de Visual Studio 2013 con código fuente](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) para acompañar esta serie de tutoriales.

En este tutorial se describe cómo modificar la aplicación de ejemplo Wingtip Toys para incluir la autorización de usuario, el registro y el pago mediante PayPal. Solo los usuarios que han iniciado sesión tendrán autorización para comprar productos. La funcionalidad de registro de usuario integrada de la plantilla de proyecto de ASP.NET 4,5 Web Forms ya incluye gran parte de lo que necesita. Agregará la funcionalidad de retirada de PayPal Express. En este tutorial utilizará el entorno de pruebas para desarrolladores de PayPal, por lo que no se transferirán fondos reales. Al final del tutorial, probará la aplicación seleccionando productos para agregar al carro de la compra, haciendo clic en el botón desproteger y transfiriendo datos al sitio web de pruebas de PayPal. En el sitio web de pruebas de PayPal, confirmará su información de envío y pago y, a continuación, volverá a la aplicación de ejemplo Wingtip Toys local para confirmar y completar la compra.

Hay varios procesadores de pago de terceros experimentados que se especializan en la compra en línea que abordan la escalabilidad y la seguridad. Los desarrolladores de ASP.NET deben tener en cuenta las ventajas de usar una solución de pago de terceros antes de implementar una solución de compras y compra.

> [!NOTE] 
> 
> La aplicación de ejemplo Wingtip Toys se ha diseñado para mostrar los conceptos y características específicos de ASP.NET disponibles para los desarrolladores web de ASP.NET. Esta aplicación de ejemplo no se optimizó en todas las circunstancias posibles con respecto a la escalabilidad y la seguridad.

## <a name="what-youll-learn"></a>Temas que se abordarán:

- Cómo restringir el acceso a páginas específicas de una carpeta.
- Cómo crear un carro de la compra conocido desde un carro de la compra anónimo.
- Cómo habilitar SSL para el proyecto.
- Cómo agregar un proveedor de OAuth al proyecto.
- Cómo usar PayPal para comprar productos mediante el entorno de pruebas de PayPal.
- Cómo mostrar los detalles de PayPal en un control **DetailsView** .
- Cómo actualizar la base de datos de la aplicación Wingtip Toys con los detalles obtenidos de PayPal.

## <a name="adding-order-tracking"></a>Adición del seguimiento de pedidos

En este tutorial, creará dos clases nuevas para realizar el seguimiento de los datos desde el orden en que se ha creado un usuario. Las clases realizarán un seguimiento de los datos relacionados con la información de envío, el total de compras y la confirmación del pago.

### <a name="add-the-order-and-orderdetail-model-classes"></a>Adición de las clases de modelo order y OrderDetail

Anteriormente en esta serie de tutoriales, definió el esquema para las categorías, los productos y los elementos del carro de la compra mediante la creación de las clases `Category`, `Product`y `CartItem` en la carpeta *modelos* . Ahora, agregará dos clases nuevas para definir el esquema del pedido de producto y los detalles del pedido.

1. En la carpeta **Models** , agregue una nueva clase denominada *Order.CS*.   
   El nuevo archivo de clase se muestra en el editor.
2. Reemplace el código predeterminado con lo siguiente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. Agregue una clase *OrderDetail.CS* a la carpeta *Models* .
4. Reemplace el código predeterminado por el siguiente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

Las clases `Order` y `OrderDetail` contienen el esquema para definir la información de pedidos que se usa para la compra y el envío.

Además, deberá actualizar la clase de contexto de base de datos que administra las clases de entidad y que proporciona acceso a los datos a la base de datos. Para ello, agregue las clases de modelo de orden y `OrderDetail` recién creadas a `ProductContext` clase.

1. En **Explorador de soluciones**, busque y abra el archivo *ProductContext.CS* .
2. Agregue el código resaltado al archivo *ProductContext.CS* como se muestra a continuación:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

Como se mencionó anteriormente en esta serie de tutoriales, el código del archivo *ProductContext.CS* agrega el espacio de nombres `System.Data.Entity` de forma que tenga acceso a toda la funcionalidad básica de la Entity Framework. Esta funcionalidad incluye la capacidad de consultar, insertar, actualizar y eliminar datos trabajando con objetos fuertemente tipados. El código anterior de la clase `ProductContext` agrega Entity Framework acceso a las clases `Order` y `OrderDetail` que se acaban de agregar.

## <a name="adding-checkout-access"></a>Agregar acceso de desprotección

La aplicación de ejemplo Wingtip Toys permite a los usuarios anónimos revisar y agregar productos a un carro de la compra. Sin embargo, cuando los usuarios anónimos eligen comprar los productos que han agregado al carro de la compra, deben iniciar sesión en el sitio. Una vez que hayan iniciado sesión, podrán tener acceso a las páginas restringidas de la aplicación web que controlan el proceso de desprotección y compra. Estas páginas restringidas están contenidas en la carpeta de *desprotección* de la aplicación.

### <a name="add-a-checkout-folder-and-pages"></a>Agregar una carpeta de desprotección y páginas

Ahora creará la carpeta de *desprotección* y las páginas que verá el cliente durante el proceso de desprotección. Estas páginas se actualizarán más adelante en este tutorial.

1. Haga clic con el botón derecho en el nombre del proyecto (**Wingtip Toys**) en **Explorador de soluciones** y seleccione **Agregar una nueva carpeta**. 

    ![Retirada y pago con PayPal-nueva carpeta](checkout-and-payment-with-paypal/_static/image1.png)
2. Asigne a la nueva carpeta el nombre *Checkout*.
3. Haga clic con el botón secundario en la carpeta de *retirada* y seleccione **Agregar**-&gt;**nuevo elemento**. 

    ![Retirada y pago con PayPal-nuevo elemento](checkout-and-payment-with-paypal/_static/image2.png)
4. Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
5. Seleccione el grupo plantillas **Web** de **Visual C#**  -&gt; a la izquierda. A continuación, en el panel central, seleccione **Web Forms con la página maestra**y asígnele el nombre *CheckoutStart. aspx*. 

    ![Cuadro de diálogo de comprobación y pago con PayPal: agregar nuevo elemento](checkout-and-payment-with-paypal/_static/image3.png)
6. Como antes, seleccione el archivo *site. Master* como la página maestra.
7. Agregue las siguientes páginas adicionales a la carpeta de *desprotección* siguiendo los mismos pasos anteriores:   

    - CheckoutReview.aspx
    - CheckoutComplete.aspx
    - CheckoutCancel.aspx
    - CheckoutError.aspx

### <a name="add-a-webconfig-file"></a>Agregar un archivo Web. config

Al agregar un nuevo archivo *Web. config* a la carpeta de *desprotección* , podrá restringir el acceso a todas las páginas contenidas en la carpeta.

1. Haga clic con el botón derecho en la carpeta de *retirada* y seleccione **Agregar** -&gt; **nuevo elemento**.  
   Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. Seleccione el grupo plantillas **Web** de **Visual C#**  -&gt; a la izquierda. A continuación, en el panel central, seleccione **archivo de configuración Web**, acepte el nombre predeterminado de *Web. config*y, a continuación, seleccione **Agregar**.
3. Reemplace el contenido XML del archivo *Web.config* por el siguiente:  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. Guarde el archivo *Web.config* .

El archivo *Web. config* especifica que a todos los usuarios desconocidos de la aplicación web se les debe denegar el acceso a las páginas contenidas en la carpeta de *desprotección* . Sin embargo, si el usuario ha registrado una cuenta y ha iniciado sesión, será un usuario conocido y tendrá acceso a las páginas de la carpeta de *desprotección* .

Es importante tener en cuenta que la configuración de ASP.NET sigue una jerarquía, donde cada archivo *Web. config* aplica los valores de configuración a la carpeta en la que se encuentra y a todos los directorios secundarios que hay debajo de él.

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a>Habilitación de SSL para el proyecto

 Capa de sockets seguros (SSL) es un protocolo definido para permitir a los servidores y clientes web comunicarse de forma más segura mediante el uso de cifrado. Cuando no se usa SSL, cualquiera que tenga acceso físico a la red puede abrir los datos que se envían entre el cliente y el servidor para examinar el paquete. Además, varios esquemas de autenticación habituales no son seguros en HTTP plano. En particular, la autenticación básica y la autenticación mediante formularios envían credenciales no cifradas. Para ser seguros, estos esquemas de autenticación deben usar SSL. 

1. En **Explorador de soluciones**, haga clic en el proyecto **WingtipToys** y, a continuación, presione **F4** para mostrar la ventana **propiedades** .
2. Cambie **SSL habilitado** a `true`.
3. Copie la **Dirección URL de SSL** para usarla más adelante.   
 La dirección URL de SSL se `https://localhost:44300/` a menos que haya creado previamente sitios Web SSL (como se muestra a continuación).   
    ![Propiedades del proyecto](checkout-and-payment-with-paypal/_static/image4.png)
4. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **WingtipToys** y haga clic en **propiedades**.
5. En el panel izquierdo, haga clic en **Web**.
6. Cambie la **dirección URL del proyecto** para que use la **dirección URL de SSL** que guardó anteriormente.   
    ![Propiedades de Project Web](checkout-and-payment-with-paypal/_static/image5.png)
7. Presione **CTRL+S**para guardar la página.
8. Presione **Ctrl+F5** para ejecutar la aplicación. Visual Studio mostrará una opción que le permite evitar advertencias de SSL.
9. Haga clic en **Sí** para confiar en el certificado SSL de IIS Express y continúe.   
    ![Detalles del certificado SSL de IIS Express](checkout-and-payment-with-paypal/_static/image6.png)  
 Se mostrará una advertencia de seguridad.
10. Haga clic en **Sí** para instalar el certificado en el host local.   
    ![Cuadro de diálogo Advertencia de seguridad](checkout-and-payment-with-paypal/_static/image7.png)  
 Se mostrará la ventana del explorador.

Ahora puede probar fácilmente su aplicación Web de forma local mediante SSL.

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a>Incorporación de un proveedor de OAuth 2.0

ASP.NET Web Forms proporciona opciones mejoradas para suscripciones y autenticación. Estas mejoras incluyen OAuth. OAuth es un protocolo abierto que ofrece autorización segura a través de un método estándar sencillo para aplicaciones web, móviles y de escritorio. La plantilla de formularios Web Forms de ASP.NET usa OAuth para exponer Facebook, Twitter, Google y Microsoft como proveedores de autenticación. Aunque este tutorial solo utiliza Google como proveedor de autenticación, puede modificar fácilmente el código para utilizar cualquiera de los proveedores. Los pasos que hay que seguir para implementar otros proveedores son muy similares a los que verá en este tutorial.

Además de la autenticación, este tutorial también utiliza roles para implementar la autorización. Únicamente los usuarios que agregue al rol `canEdit` podrán cambiar datos (crear, editar o eliminar contactos).

> [!NOTE] 
> 
> Las aplicaciones de Windows Live solo aceptan una dirección URL activa para un sitio web de trabajo, por lo que no puede usar una dirección URL de sitio Web local para probar los inicios de sesión.

Los pasos siguientes permiten agregar un proveedor de autenticación de Google.

1. Abra el archivo Start\Startup.Auth.cs de la *aplicación\_* .
2. Quite los caracteres de comentario del método `app.UseGoogleAuthentication()` para que tenga el siguiente aspecto: 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. Navegue a la [consola de desarrolladores de Google](https://console.developers.google.com/). Debe iniciar sesión también con su cuenta de correo electrónico de desarrollador de Google (gmail.com). Si no tiene una cuenta de Google, seleccione el vínculo **Crear una cuenta** .   
   A continuación, verá la **consola de desarrolladores de Google**.   
    ![consola de desarrolladores de Google](checkout-and-payment-with-paypal/_static/image8.png)
4. Haga clic en el botón **crear proyecto** y escriba un nombre y un identificador de proyecto (puede usar los valores predeterminados). A continuación, haga clic en la **casilla contrato** y en el botón **crear** .  

    ![Google: nuevo proyecto](checkout-and-payment-with-paypal/_static/image9.png)

   En unos segundos, se creará el nuevo proyecto y el explorador mostrará la página proyectos nuevos.
5. En la pestaña de la izquierda, haga clic en **api &amp; auth**y, a continuación, haga clic en **credenciales**.
6. Haga clic en **crear nuevo ID** . de cliente en **OAuth**.   
   Se mostrará el cuadro de diálogo **Crear id. de cliente** .   
    ![Google: crear el ID. de cliente](checkout-and-payment-with-paypal/_static/image10.png)
7. En el cuadro de diálogo **crear ID** . de cliente, mantenga la **aplicación web** predeterminada para el tipo de aplicación.
8. Establezca los **orígenes de JavaScript autorizados** en la dirección URL de SSL que usó anteriormente en este tutorial (`https://localhost:44300/` a menos que haya creado otros proyectos SSL).   
   Esta dirección URL es el origen de su aplicación. Para este ejemplo, solo proporcionará la dirección URL de prueba del localhost. Sin embargo, puede especificar varias direcciones URL para tener en cuenta el localhost y la producción.
9. Establezca el **URI de redireccionamiento autorizado** en lo siguiente: 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   Este valor es el URI que utiliza ASP.NET OAuth para comunicarse con el servidor OAuth de Google. Recuerde la dirección URL de SSL que usó anteriormente (`https://localhost:44300/` a menos que haya creado otros proyectos SSL).
10. Haga clic en el botón **crear ID** . de cliente.
11. En el menú de la izquierda de la consola de desarrolladores de Google, haga clic en el elemento de menú de la **pantalla de consentimiento** y establezca la dirección de correo electrónico y el nombre del producto. Cuando haya completado el formulario, haga clic en **Guardar**.
12. Haga clic en el elemento de menú **API** , desplácese hacia abajo y haga clic en el botón **desactivado** situado junto a **Google + API**.   
    Al aceptar esta opción se habilitará la API de Google +.
13. También debe actualizar el paquete NuGet **Microsoft. Owin** a la versión 3.0.0.   
    En el menú **herramientas** , seleccione **Administrador de paquetes Nuget** y, a continuación, seleccione **administrar paquetes Nuget para la solución**.  
    En la ventana **administrar paquetes NuGet** , busque y actualice el paquete **Microsoft. Owin** a la versión 3.0.0.
14. En Visual Studio, actualice el método `UseGoogleAuthentication` de la página *Startup.auth.CS* copiando y pegando el **identificador de cliente** y el **secreto de cliente** en el método. Los valores de ID. de **cliente** y **secreto de cliente** que se muestran a continuación son ejemplos y no funcionarán. 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. Presione **CTRL+F5** para compilar y ejecutar la aplicación. Haga clic en el vínculo **Iniciar sesión** .
16. En **usar otro servicio para iniciar sesión**, haga clic en **Google**.  
    ![Iniciar sesión](checkout-and-payment-with-paypal/_static/image11.png)
17. Si tiene que proporcionar credenciales, se le redirigirá al sitio de Google donde podrá especificarlas.  
    ![Google: Inicio de sesión](checkout-and-payment-with-paypal/_static/image12.png)
18. Después de escribir sus credenciales, se le pedirá que conceda permisos a la aplicación web que acaba de crear.  
    ![Cuenta de servicio de proyecto predeterminada](checkout-and-payment-with-paypal/_static/image13.png)
19. Haga clic en **Aceptar**. Ahora se le redirigirá a la página de **registro** de la aplicación **WingtipToys** donde puede registrar su cuenta de Google.  
    ![Registro con una cuenta de Google](checkout-and-payment-with-paypal/_static/image14.png)
20. Tiene la opción de cambiar el nombre de registro de correo electrónico local que usa para su cuenta de Gmail, pero suele ser más práctico mantener el mismo alias de correo electrónico predeterminado (es decir, el que usó para la autenticación). Haga clic en **iniciar sesión** como se mostró anteriormente.

### <a name="modifying-login-functionality"></a>Modificar la funcionalidad de inicio de sesión

Como se mencionó anteriormente en esta serie de tutoriales, gran parte de la funcionalidad de registro del usuario se ha incluido en la plantilla de formularios Web Forms de ASP.NET de forma predeterminada. Ahora modificará las páginas default *login. aspx* y *Register. aspx* para llamar al método `MigrateCart`. El método `MigrateCart` asocia un usuario que acaba de iniciar sesión con un carro de la compra anónimo. Al asociar el carro de la compra y el usuario, la aplicación de ejemplo Wingtip Toys podrá mantener el carro de la compra del usuario entre visitas.

1. En **Explorador de soluciones**, busque y abra la carpeta de la *cuenta* .
2. Modifique la página de código subyacente denominada *login.aspx.CS* para incluir el código resaltado en amarillo, de modo que aparezca de la manera siguiente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. Guarde el archivo *login.aspx.CS* .

Por ahora, puede omitir la advertencia de que no hay ninguna definición para el método `MigrateCart`. Lo agregará un poco más adelante en este tutorial.

El archivo de código subyacente *login.aspx.CS* admite un método de inicio de sesión. Al inspeccionar la página login. aspx, verá que esta página incluye un botón "iniciar sesión" que, cuando se hace clic, se desencadena el controlador de `LogIn` en el código subyacente.

Cuando se llama al método `Login` en *login.aspx.CS* , se crea una nueva instancia del carro de la compra denominado `usersShoppingCart`. Se recupera el identificador del carro de la compra (un GUID) y se establece en la variable `cartId`. A continuación, se llama al método `MigrateCart`, pasando tanto el `cartId` como el nombre del usuario que ha iniciado sesión a este método. Cuando se migra el carro de la compra, el GUID que se usa para identificar el carro de la compra anónima se reemplaza por el nombre de usuario.

Además de modificar el archivo de código subyacente de *login.aspx.CS* para migrar el carro de la compra cuando el usuario inicia sesión, también debe modificar el *archivo de código subyacente Register.aspx.CS* para migrar el carro de la compra cuando el usuario crea una cuenta nueva e inicia sesión.

1. En la carpeta de la *cuenta* , abra el archivo de código subyacente llamado *Register.aspx.CS*.
2. Modifique el archivo de código subyacente incluyendo el código en amarillo, para que aparezca de la manera siguiente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. Guarde el archivo *Register.aspx.CS* . Una vez más, omita la advertencia sobre el método `MigrateCart`.

Observe que el código que utilizó en el controlador de eventos `CreateUser_Click` es muy similar al código que usó en el método `LogIn`. Cuando el usuario se registra o inicia sesión en el sitio, se realizará una llamada al método `MigrateCart`.

## <a name="migrating-the-shopping-cart"></a>Migración del carro de la compra

Ahora que ha actualizado el registro y el proceso de registro, puede Agregar el código para migrar el carro de la compra con el método `MigrateCart`.

1. En **Explorador de soluciones**, busque la carpeta *Logic* y abra el archivo de clase *ShoppingCartActions.CS* .
2. Agregue el código resaltado en amarillo al código existente en el archivo *ShoppingCartActions.CS* , de modo que el código del archivo *ShoppingCartActions.CS* aparezca de la manera siguiente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

El método `MigrateCart` usa el cartId existente para buscar el carro de la compra del usuario. A continuación, el código recorre en bucle todos los elementos del carro de la compra y reemplaza la propiedad `CartId` (tal y como se especifica en el esquema de `CartItem`) con el nombre de usuario que ha iniciado sesión.

### <a name="updating-the-database-connection"></a>Actualización de la conexión de base de datos

Si sigue este tutorial con la aplicación de ejemplo de Wingtip Toys **pregenerada** , debe volver a crear la base de datos de pertenencia predeterminada. Al modificar la cadena de conexión predeterminada, la base de datos de pertenencia se creará la próxima vez que se ejecute la aplicación.

1. Abra el archivo *Web. config* en la raíz del proyecto.
2. Actualice la cadena de conexión predeterminada para que aparezca de la manera siguiente:   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a>Integración de PayPal

PayPal es una plataforma de facturación basada en Web que acepta pagos por comerciantes en línea. En este tutorial se explica cómo integrar la funcionalidad de protección rápida de PayPal en la aplicación. La desprotección rápida permite a los clientes usar PayPal para pagar los artículos que han agregado a su carro de la compra.

### <a name="create-paypal-test-accounts"></a>Crear cuentas de prueba de PayPal

Para usar el entorno de pruebas de PayPal, debe crear y comprobar una cuenta de prueba para desarrolladores. Usará la cuenta de prueba de desarrollador para crear una cuenta de prueba de comprador y una cuenta de prueba de vendedor. Las credenciales de la cuenta de prueba del desarrollador también permitirán a la aplicación de ejemplo Wingtip Toys tener acceso al entorno de pruebas de PayPal.

1. En un explorador, vaya al sitio de pruebas para desarrolladores de PayPal:   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. Si no tiene una cuenta de desarrollador de PayPal, cree una nueva cuenta; para ello, haga clic en **registrarse**y siga los pasos de suscripción. Si tiene una cuenta de desarrollador de PayPal existente, inicie sesión haciendo clic en **iniciar sesión**. Necesitará su cuenta de desarrollador de PayPal para probar la aplicación de ejemplo Wingtip Toys más adelante en este tutorial.
3. Si acaba de suscribirse a su cuenta de desarrollador de PayPal, es posible que deba comprobar su cuenta de desarrollador de PayPal con PayPal. Puede comprobar su cuenta siguiendo los pasos que PayPal envía a su cuenta de correo electrónico. Una vez que haya comprobado su cuenta de desarrollador de PayPal, vuelva a iniciar sesión en el sitio de pruebas de desarrollo de PayPal.
4. Una vez que haya iniciado sesión en el sitio para desarrolladores de PayPal con su cuenta de desarrollador de PayPal, deberá crear una cuenta de prueba de PayPal Buyer si aún no tiene una. Para crear una cuenta de prueba de comprador, en el sitio de PayPal haga clic en la pestaña **aplicaciones** y, a continuación, haga clic en **cuentas de espacio aislado**.   
 Se muestra la página **cuentas de prueba de espacio aislado** .   

    > [!NOTE] 
    > 
    > El sitio para desarrolladores de PayPal ya proporciona una cuenta de prueba de comerciante.

    ![Retirada y pago con cuentas de prueba de espacio aislado de PayPal](checkout-and-payment-with-paypal/_static/image15.png)
5. En la página cuentas de prueba de espacio aislado, haga clic en **crear cuenta**.
6. En la página **crear cuenta de prueba** , elija el correo electrónico y la contraseña de la cuenta de prueba de comprador que prefiera.   

    > [!NOTE] 
    > 
    > Necesitará las direcciones de correo electrónico y la contraseña del comprador para probar la aplicación de ejemplo Wingtip Toys al final de este tutorial.

    ![Retirada y pago con cuentas de prueba de espacio aislado de PayPal](checkout-and-payment-with-paypal/_static/image16.png)
7. Para crear la cuenta de prueba de comprador, haga clic en el botón **crear cuenta** .  
 Se muestra la página **cuentas de prueba de espacio aislado** . 

    ![Retirada y pago con PayPal: cuentas de PayPal](checkout-and-payment-with-paypal/_static/image17.png)
8. En la página **cuentas de prueba de espacio aislado** , haga clic en la cuenta de correo electrónico del **facilitador** .  
    Aparecen las opciones de **perfil** y **notificación** .
9. Seleccione la opción **perfil** y, a continuación, haga clic en **credenciales de API** para ver las credenciales de API de la cuenta de prueba de comerciante.
10. Copie las credenciales de la API de prueba en el Bloc de notas.

Necesitará las credenciales de la API de prueba clásica mostradas (nombre de usuario, contraseña y firma) para realizar llamadas API desde la aplicación de ejemplo Wingtip Toys al entorno de pruebas de PayPal. Agregará las credenciales en el paso siguiente.

### <a name="add-paypal-class-and-api-credentials"></a>Agregar credenciales de clase y API de PayPal

La mayoría del código de PayPal se colocará en una sola clase. Esta clase contiene los métodos utilizados para comunicarse con PayPal. Además, agregará las credenciales de PayPal a esta clase.

1. En la aplicación de ejemplo Wingtip Toys de Visual Studio, haga clic con el botón derecho en la carpeta **lógica** y seleccione **Agregar** -&gt; **nuevo elemento**.   
   Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. En **Visual C#**  , en el panel **instalado** de la izquierda, seleccione **código**.
3. En el panel central, seleccione **clase**. Asigne a esta nueva clase el nombre **PayPalFunctions.CS**.
4. Haga clic en **Agregar**.  
   El nuevo archivo de clase se muestra en el editor.
5. Reemplace el código predeterminado por el siguiente:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. Agregue las credenciales de la API de comerciante (nombre de usuario, contraseña y firma) que mostró anteriormente en este tutorial para poder realizar llamadas de función al entorno de pruebas de PayPal.  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> En esta aplicación de ejemplo, simplemente se agregan credenciales a un C# archivo (. CS). Sin embargo, en una solución implementada, considere la posibilidad de cifrar las credenciales en un archivo de configuración.

La clase NVPAPICaller contiene la mayoría de la funcionalidad de PayPal. El código de la clase proporciona los métodos necesarios para realizar una compra de prueba desde el entorno de pruebas de PayPal. Las tres funciones de PayPal siguientes se usan para realizar compras:

- Función `SetExpressCheckout`
- Función `GetExpressCheckoutDetails`
- Función `DoExpressCheckoutPayment`

El método `ShortcutExpressCheckout` recopila la información de compra de la prueba y los detalles del producto desde el carro de la compra y llama a la función `SetExpressCheckout` PayPal. El método `GetCheckoutDetails` confirma los detalles de compra y llama a la función `GetExpressCheckoutDetails` PayPal antes de realizar la compra de la prueba. El método `DoCheckoutPayment` completa la compra de prueba desde el entorno de pruebas mediante una llamada a la función `DoExpressCheckoutPayment` PayPal. El código restante es compatible con los métodos y procesos de PayPal, como la codificación de cadenas, la descodificación de cadenas, el procesamiento de matrices y la determinación de credenciales.

> [!NOTE] 
> 
> PayPal le permite incluir detalles de compra opcionales basados en [la especificación de la API de PayPal](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout). Al extender el código en la aplicación de ejemplo Wingtip Toys, puede incluir detalles de localización, descripciones de productos, impuestos, un número de servicio de cliente, así como muchos otros campos opcionales.

Tenga en cuenta que las direcciones URL de retorno y de cancelación que se especifican en el método **ShortcutExpressCheckout** utilizan un número de puerto.

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

Cuando Visual Web Developer ejecuta un proyecto web mediante SSL, normalmente se usa el puerto 44300 para el servidor Web. Como se indicó anteriormente, el número de puerto es 44300. Al ejecutar la aplicación, podría ver un número de puerto diferente. El número de puerto debe estar establecido correctamente en el código para que pueda ejecutar correctamente la aplicación de ejemplo Wingtip Toys al final de este tutorial. En la siguiente sección de este tutorial se explica cómo recuperar el número de puerto del host local y cómo actualizar la clase PayPal.

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a>Actualización del número de Puerto LocalHost en la clase PayPal

La aplicación de ejemplo Wingtip Toys compra productos navegando al sitio de pruebas de PayPal y volviendo a la instancia local de la aplicación de ejemplo Wingtip Toys. Para que PayPal vuelva a la dirección URL correcta, debe especificar el número de puerto de la aplicación de ejemplo que se ejecuta localmente en el código de PayPal mencionado anteriormente.

1. Haga clic con el botón secundario en el nombre del proyecto (**WingtipToys**) en **Explorador de soluciones** y seleccione **propiedades**.
2. En la columna izquierda, seleccione la pestaña **Web** .
3. Recupere el número de puerto en el cuadro **dirección URL del proyecto** .
4. Si es necesario, actualice el `returnURL` y `cancelURL` de la clase PayPal (`NVPAPICaller`) en el archivo *PayPalFunctions.CS* para usar el número de puerto de la aplicación web:   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

Ahora, el código que agregó coincidirá con el puerto esperado para la aplicación Web local. PayPal podrá volver a la dirección URL correcta en el equipo local.

### <a name="add-the-paypal-checkout-button"></a>Agregar el botón de retirada de PayPal

Ahora que las funciones principales de PayPal se han agregado a la aplicación de ejemplo, puede empezar a agregar el marcado y el código necesarios para llamar a estas funciones. En primer lugar, debe agregar el botón de desprotección que el usuario verá en la página carro de la compra.

1. Abra el archivo *ShoppingCart. aspx* .
2. Desplácese hasta la parte inferior del archivo y busque el `<!--Checkout Placeholder -->` comentario.
3. Reemplace el comentario por un control `ImageButton` para que la marca se reemplace como se indica a continuación:  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. En el archivo *ShoppingCart.aspx.CS* , después del controlador de eventos `UpdateBtn_Click` cerca del final del archivo, agregue el controlador de eventos `CheckOutBtn_Click`:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. También en el archivo *ShoppingCart.aspx.CS* , agregue una referencia al `CheckoutBtn`, de modo que se haga referencia al botón nueva imagen como se indica a continuación:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. Guarde los cambios en el archivo *ShoppingCart. aspx* y en el archivo *ShoppingCart.aspx.CS* .
7. En el menú, seleccione **Depurar**-&gt;**compilación WingtipToys**.  
   El proyecto se volverá a generar con el control **ImageButton** recién agregado.

### <a name="send-purchase-details-to-paypal"></a>Enviar detalles de compra a PayPal

Cuando el usuario haga clic en el botón **Desproteger** de la página del carro de la compra (*ShoppingCart. aspx*), comenzará el proceso de compra. El código siguiente llama a la primera función PayPal necesaria para comprar productos.

1. En la carpeta de *desprotección* , abra el archivo de código subyacente llamado *CheckoutStart.aspx.CS*.   
   Asegúrese de abrir el archivo de código subyacente.
2. Reemplace el código existente por el siguiente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

Cuando el usuario de la aplicación hace clic en el botón **Desproteger** en la página del carro de la compra, el explorador navega a la página *CheckoutStart. aspx* . Cuando se carga la página *CheckoutStart. aspx* , se llama al método `ShortcutExpressCheckout`. En este momento, el usuario se transfiere al sitio web de pruebas de PayPal. En el sitio de PayPal, el usuario escribe sus credenciales de PayPal, revisa los detalles de la compra, acepta el contrato de PayPal y vuelve a la aplicación de ejemplo Wingtip Toys en la que se completa el método de `ShortcutExpressCheckout`. Una vez completado el método `ShortcutExpressCheckout`, redirigirá al usuario a la página *CheckoutReview. aspx* especificada en el método `ShortcutExpressCheckout`. Esto permite al usuario revisar los detalles del pedido desde la aplicación de ejemplo Wingtip Toys.

### <a name="review-order-details"></a>Revisar los detalles del pedido

Después de volver de PayPal, la página *CheckoutReview. aspx* de la aplicación de ejemplo Wingtip Toys muestra los detalles del pedido. Esta página permite al usuario revisar los detalles del pedido antes de adquirir los productos. La página *CheckoutReview. aspx* debe crearse como se indica a continuación:

1. En la carpeta de *desprotección* , abra la página denominada *CheckoutReview. aspx*.
2. Reemplace el marcado existente por lo siguiente:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. Abra la página de código subyacente denominada *CheckoutReview.aspx.CS* y reemplace el código existente por lo siguiente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

El control **DetailsView** se usa para mostrar los detalles del pedido que se han devuelto desde PayPal. Además, el código anterior guarda los detalles del pedido en la base de datos de Wingtip Toys como un objeto `OrderDetail`. Cuando el usuario hace clic en el botón **completar pedido** , se le redirige a la página *CheckoutComplete. aspx* .

> [!NOTE] 
> 
> **Sugerencia**
> 
> En el marcado de la página *CheckoutReview. aspx* , observe que la etiqueta `<ItemStyle>` se usa para cambiar el estilo de los elementos dentro del control **DetailsView** cerca de la parte inferior de la página. Al ver la página en la **vista Diseño** (seleccionando **diseño** en la esquina inferior izquierda de Visual Studio), seleccionando el control **DetailsView** y seleccionando la **etiqueta inteligente** (el icono de flecha en la parte superior derecha del control), podrá ver las tareas de **DetailsView**.
> 
> ![Retirada y pago con PayPal: editar campos](checkout-and-payment-with-paypal/_static/image18.png)
> 
> Al seleccionar **Editar campos**, aparecerá el cuadro de diálogo **campos** . En este cuadro de diálogo puede controlar fácilmente las propiedades visuales, como **ItemStyle**, del control **DetailsView** .
> 
> ![Cuadro de diálogo desproteger y pago con PayPal-campos](checkout-and-payment-with-paypal/_static/image19.png)

### <a name="complete-purchase"></a>Completar compra

La página *CheckoutComplete. aspx* realiza la compra de PayPal. Como se mencionó anteriormente, el usuario debe hacer clic en el botón **completar pedido** antes de que la aplicación vaya a la página *CheckoutComplete. aspx* .

1. En la carpeta de *desprotección* , abra la página denominada *CheckoutComplete. aspx*.
2. Reemplace el marcado existente por lo siguiente:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. Abra la página de código subyacente denominada *CheckoutComplete.aspx.CS* y reemplace el código existente por lo siguiente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

Cuando se carga la página *CheckoutComplete. aspx* , se llama al método `DoCheckoutPayment`. Como se mencionó anteriormente, el método de `DoCheckoutPayment` completa la compra del entorno de pruebas de PayPal. Una vez que PayPal haya completado la compra del pedido, la página *CheckoutComplete. aspx* mostrará una transacción de pago `ID` al comprador.

### <a name="handle-cancel-purchase"></a>Controlar cancelar compra

Si el usuario decide cancelar la compra, se le dirigirá a la página *CheckoutCancel. aspx* , donde verá que se ha cancelado su pedido.

1. Abra la página denominada *CheckoutCancel. aspx* en la carpeta de *desprotección* .
2. Reemplace el marcado existente por lo siguiente:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a>Control de errores de compra

Los errores que se produzcan durante el proceso de compra se controlarán mediante la página *CheckoutError. aspx* . El código subyacente de la página *CheckoutStart. aspx* , la página *CheckoutReview. aspx* y la página *CheckoutComplete. aspx* redirigirá a la página *CheckoutError. aspx* si se produce un error.

1. Abra la página denominada *CheckoutError. aspx* en la carpeta de *desprotección* .
2. Reemplace el marcado existente por lo siguiente:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

La página *CheckoutError. aspx* se muestra con los detalles del error cuando se produce un error durante el proceso de desprotección.

## <a name="running-the-application"></a>Ejecutar la aplicación

Ejecute la aplicación para ver cómo comprar productos. Tenga en cuenta que se ejecutará en el entorno de pruebas de PayPal. No se intercambia dinero real.

1. Asegúrese de que todos los archivos se guardan en Visual Studio.
2. Abra un explorador Web y vaya a [https://developer.paypal.com](https://developer.paypal.com/).
3. Inicie sesión con su cuenta de desarrollador de PayPal que creó anteriormente en este tutorial.  
   Para el espacio aislado para desarrolladores de PayPal, debe iniciar sesión en [https://developer.paypal.com](https://developer.paypal.com/) para probar la protección rápida. Esto solo se aplica a las pruebas de espacio aislado de PayPal, no al entorno activo de PayPal.
4. En Visual Studio, presione **F5** para ejecutar la aplicación de ejemplo Wingtip Toys.  
   Después de volver a generar la base de datos, el explorador se abrirá y mostrará la página *default. aspx* .
5. Agregue tres productos diferentes al carro de la compra; para ello, seleccione la categoría de producto, como "automóviles" y, a continuación, haga clic en **Agregar al carro** junto a cada producto.  
   El carro de la compra mostrará el producto que ha seleccionado.
6. Haga clic en el botón **PayPal** para desprotegerlo. 

    ![Retirada y pago con PayPal-Cart](checkout-and-payment-with-paypal/_static/image20.png)

   La desprotección requerirá que tenga una cuenta de usuario para la aplicación de ejemplo Wingtip Toys.
7. Haga clic en el vínculo de **Google** de la derecha de la página para iniciar sesión con una cuenta de correo electrónico de gmail.com existente.  
   Si no tiene una cuenta de gmail.com, puede crear una con fines de prueba en [www.gmail.com](https://www.gmail.com/). También puede usar una cuenta local estándar haciendo clic en "registrar". 

    ![Retirada y pago con PayPal: Inicio de sesión](checkout-and-payment-with-paypal/_static/image21.png)
8. Inicie sesión con su cuenta y contraseña de gmail. 

    ![Retirada y pago con PayPal: Inicio de sesión de gmail](checkout-and-payment-with-paypal/_static/image22.png)
9. Haga clic en el botón **iniciar sesión** para registrar su cuenta de gmail con el nombre de usuario de la aplicación de ejemplo Wingtip Toys. 

    ![Retirada y pago con PayPal: registrar cuenta](checkout-and-payment-with-paypal/_static/image23.png)
10. En el sitio de prueba de PayPal, agregue la dirección de correo electrónico y la contraseña del **comprador** que creó anteriormente en este tutorial y, después, haga clic en el botón **iniciar sesión** . 

    ![Retirada y pago con PayPal-inicio de sesión de PayPal](checkout-and-payment-with-paypal/_static/image24.png)
11. Acepte la Directiva PayPal y haga clic en el botón **Aceptar y continuar** .  
    Tenga en cuenta que esta página solo se muestra la primera vez que usa esta cuenta de PayPal. De nuevo, tenga en cuenta que se trata de una cuenta de prueba, no se intercambia dinero real. 

    ![Retirada y pago con PayPal: Directiva PayPal](checkout-and-payment-with-paypal/_static/image25.png)
12. Revise la información del pedido en la página de revisión del entorno de pruebas de PayPal y haga clic en **continuar**. 

    ![Retirada y pago con PayPal: revisar información](checkout-and-payment-with-paypal/_static/image26.png)
13. En la página *CheckoutReview. aspx* , compruebe el importe del pedido y vea la dirección de envío generada. A continuación, haga clic en el botón **completar pedido** . 

    ![Retirada y pago con PayPal: revisión de pedidos](checkout-and-payment-with-paypal/_static/image27.png)
14. La página **CheckoutComplete. aspx** se muestra con un ID. de transacción de pago. 

    ![Retirada y pago con PayPal-desprotección completada](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a>Revisar la base de datos

Al revisar los datos actualizados en la base de datos de la aplicación de ejemplo Wingtip Toys después de ejecutar la aplicación, puede ver que la aplicación registró correctamente la compra de los productos.

Puede inspeccionar los datos contenidos en el archivo de base de datos *wingtiptoys. MDF* mediante la ventana de **Explorador de bases de datos** (**Explorador de servidores** ventana de Visual Studio) como hizo anteriormente en esta serie de tutoriales.

1. Cierre la ventana del explorador si aún está abierta.
2. En Visual Studio, seleccione el icono **Mostrar todos los archivos** situado en la parte superior de **Explorador de soluciones** para que pueda expandir la carpeta **App\_Data** .
3. Expanda la carpeta **App\_Data** .  
 Es posible que tenga que seleccionar el icono **Mostrar todos los archivos** de la carpeta.
4. Haga clic con el botón secundario en el archivo de base de datos *wingtiptoys. MDF* y seleccione **abrir**.  
    Se muestra **Explorador de servidores** .
5. Expanda la carpeta **Tablas** .
6. Haga clic con el botón secundario en la tabla **Orders**y seleccione **Mostrar datos de tabla**.  
 Se muestra la tabla **pedidos** .
7. Revise la columna **PaymentTransactionID** para confirmar las transacciones correctas. 

    ![Retirada y pago con PayPal: revisar base de datos](checkout-and-payment-with-paypal/_static/image29.png)
8. Cierre la ventana de la tabla **Orders** .
9. En el Explorador de servidores, haga clic con el botón secundario en la tabla **OrderDetails** y seleccione **Mostrar datos de tabla**.
10. Revise los valores `OrderId` y `Username` de la tabla **OrderDetails** . Tenga en cuenta que estos valores coinciden con los valores `OrderId` y `Username` incluidos en la tabla **Orders** .
11. Cierre la ventana de la tabla **OrderDetails** .
12. Haga clic con el botón secundario en el archivo de base de datos de Wingtip Toys (*wingtiptoys. MDF*) y seleccione **cerrar conexión**.
13. Si no ve la ventana **Explorador de soluciones** , haga clic en **Explorador de soluciones** en la parte inferior de la ventana de **Explorador de servidores** para volver a mostrar el **Explorador de soluciones** .

## <a name="summary"></a>Resumen

En este tutorial, ha agregado esquemas de pedidos y detalles de pedidos para realizar un seguimiento de la compra de productos. También ha integrado la funcionalidad de PayPal en la aplicación de ejemplo Wingtip Toys.

## <a name="additional-resources"></a>Recursos adicionales

[Información general sobre la configuración de ASP.NET](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)  
[Implementar una aplicación de formularios Web Forms de ASP.NET segura con pertenencia, OAuth y SQL Database para Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Evaluación gratuita de Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a>Declinación de responsabilidades

Este tutorial contiene código de ejemplo. Este código de ejemplo se proporciona "tal cual" sin garantía de ningún tipo. En consecuencia, Microsoft no garantiza la precisión, la integridad o la calidad del código de ejemplo. Acepta usar el código de ejemplo bajo su responsabilidad. En ningún caso, Microsoft será responsable de ningún modo por cualquier código de ejemplo, contenido, incluidos, entre otros, errores u omisiones en cualquier código de ejemplo, contenido, o cualquier pérdida o daño de cualquier tipo que se incurra como resultado del uso de cualquier código de ejemplo. Se le notificará y, por el presente acuerdo, se compromete a indemnizar, a mantener a Microsoft en contra de cualquier pérdida, reclamaciones de pérdida, lesiones o daños de cualquier naturaleza, incluidas, sin limitación, las supuestos que ocasionaron o que proceden del material que publica, transmitir, usar o confiar en incluir, entre otras, las vistas expresadas en él.

> [!div class="step-by-step"]
> [Anterior](shopping-cart.md)
> [Siguiente](membership-and-administration.md)
