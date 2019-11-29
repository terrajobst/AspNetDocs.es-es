---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
title: Carro de la compra | Microsoft Docs
author: Erikre
description: Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4,5 y Microsoft Visual Studio Express 2013 para nosotros...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 6898c601-6c31-432f-8388-e6843f8a17cb
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
msc.type: authoredcontent
ms.openlocfilehash: 46264a0ab2244cff24761ce94b41722e61e3f426
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74614922"
---
# <a name="shopping-cart"></a>Carro de la compra

por [Erik Reitan](https://github.com/Erikre)

[Descargar el proyecto de ejemplo deC#Wingtip Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar el libro electrónico (pdf)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4,5 y Microsoft Visual Studio Express 2013 para Web. Hay disponible un [proyecto C# de Visual Studio 2013 con código fuente](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) para acompañar esta serie de tutoriales.

En este tutorial se describe la lógica de negocios necesaria para agregar un carro de la compra a la aplicación de formularios Web Forms ASP.NET de Wingtip Toys. Este tutorial se basa en el tutorial anterior "Mostrar elementos de datos y detalles" y forma parte de la serie de tutoriales de Wingtip Toy Store. Cuando haya completado este tutorial, los usuarios de la aplicación de ejemplo podrán agregar, quitar y modificar los productos de su carro de la compra.

## <a name="what-youll-learn"></a>Lo que aprenderá:

1. Cómo crear un carro de la compra para la aplicación Web.
2. Cómo permitir que los usuarios agreguen elementos al carro de la compra.
3. Cómo agregar un control [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview(v=vs.110).aspx#introduction) para mostrar los detalles del carro de la compra.
4. Cómo calcular y mostrar el total del pedido.
5. Cómo quitar y actualizar elementos en el carro de la compra.
6. Cómo incluir un contador de carro de la compra.

## <a name="code-features-in-this-tutorial"></a>Características de código en este tutorial:

1. Entity Framework Code First
2. Anotaciones de datos
3. Controles de datos fuertemente tipados
4. Enlace de modelos

## <a name="creating-a-shopping-cart"></a>Creación de un carro de la compra

Anteriormente en esta serie de tutoriales, se han agregado páginas y código para ver los datos de productos de una base de datos. En este tutorial, creará un carro de la compra para administrar los productos que los usuarios están interesados en comprar. Los usuarios podrán examinar y agregar elementos al carro de la compra, aunque no estén registrados o hayan iniciado sesión. Para administrar el acceso al carro de la compra, asignará a los usuarios una `ID` única mediante un identificador único global (GUID) cuando el usuario tenga acceso al carro de la compra por primera vez. Almacenará este `ID` mediante el estado de sesión ASP.NET.

> [!NOTE] 
> 
> El estado de sesión ASP.NET es un lugar adecuado para almacenar información específica del usuario que expirará después de que el usuario abandone el sitio. Aunque el uso incorrecto del estado de la sesión puede tener implicaciones en el rendimiento en sitios más grandes, el uso de estado de la sesión es muy adecuado para fines de demostración. El proyecto de ejemplo Wingtip Toys muestra cómo usar el estado de sesión sin un proveedor externo, donde el estado de sesión se almacena en proceso en el servidor Web que hospeda el sitio. En el caso de sitios mayores que proporcionan varias instancias de una aplicación o para sitios que ejecutan varias instancias de una aplicación en servidores diferentes, considere la posibilidad de usar **Windows Azure cache Service**. Esta Cache Service proporciona un servicio de almacenamiento en caché distribuido externo al sitio web y resuelve el problema de usar el estado de sesión en proceso. Para obtener más información, vea [Cómo usar el estado de sesión ASP.net con sitios web de Windows Azure](https://docs.microsoft.com/azure/redis-cache/cache-aspnet-session-state-provider).

### <a name="add-cartitem-as-a-model-class"></a>Agregar CartItem como clase de modelo

Anteriormente en esta serie de tutoriales, definió el esquema para los datos de categoría y de producto mediante la creación de las clases `Category` y `Product` en la carpeta *Models* . Ahora, agregue una nueva clase para definir el esquema para el carro de la compra. Más adelante en este tutorial, agregará una clase para controlar el acceso a los datos a la tabla `CartItem`. Esta clase proporcionará la lógica de negocios para agregar, quitar y actualizar elementos en el carro de la compra.

1. Haga clic con el botón derecho en la carpeta *modelos* y seleccione **Agregar** -&gt; **nuevo elemento**. 

    ![Carro de la compra-nuevo elemento](shopping-cart/_static/image1.png)
2. Se abrirá el cuadro de diálogo **Agregar nuevo elemento**. Seleccione **código**y, a continuación, seleccione **clase**. 

    ![Carro de la compra-cuadro de diálogo Agregar nuevo elemento](shopping-cart/_static/image2.png)
3. Asigne a esta nueva clase el nombre *CartItem.CS*.
4. Haga clic en **Agregar**.  
   El nuevo archivo de clase se muestra en el editor.
5. Reemplace el código predeterminado por el código siguiente:   

    [!code-csharp[Main](shopping-cart/samples/sample1.cs)]

La clase `CartItem` contiene el esquema que va a definir cada producto que un usuario agrega al carro de la compra. Esta clase es similar a las demás clases de esquema que creó anteriormente en esta serie de tutoriales. Por Convención, Entity Framework Code First espera que la clave principal de la tabla `CartItem` sea `CartItemId` o `ID`. Sin embargo, el código invalida el comportamiento predeterminado mediante el atributo de anotación de datos `[Key]`. El atributo `Key` de la propiedad ItemId especifica que la propiedad `ItemID` es la clave principal.

La propiedad `CartId` especifica el `ID` del usuario asociado al elemento que se va a comprar. Agregará código para crear este usuario `ID` cuando el usuario tenga acceso al carro de la compra. Esta `ID` también se almacenará como una variable de sesión ASP.NET.

### <a name="update-the-product-context"></a>Actualizar el contexto del producto

Además de agregar la clase `CartItem`, tendrá que actualizar la clase de contexto de base de datos que administra las clases de entidad y que proporciona acceso a los datos a la base de datos. Para ello, agregará la clase de modelo de `CartItem` recién creada a la clase `ProductContext`.

1. En **Explorador de soluciones**, busque y abra el archivo *ProductContext.CS* en la carpeta *Models* .
2. Agregue el código resaltado al archivo *ProductContext.CS* como se indica a continuación:  

    [!code-csharp[Main](shopping-cart/samples/sample2.cs?highlight=14)]

Como se mencionó anteriormente en esta serie de tutoriales, el código del archivo *ProductContext.CS* agrega el espacio de nombres `System.Data.Entity` de forma que tenga acceso a toda la funcionalidad básica de la Entity Framework. Esta funcionalidad incluye la capacidad de consultar, insertar, actualizar y eliminar datos trabajando con objetos fuertemente tipados. La clase `ProductContext` agrega acceso a la clase de modelo de `CartItem` recién agregada.

### <a name="managing-the-shopping-cart-business-logic"></a>Administrar la lógica de negocios de la cesta de la compra

A continuación, creará la clase `ShoppingCart` en una nueva carpeta *lógica* . La clase `ShoppingCart` controla el acceso a los datos a la tabla `CartItem`. La clase también incluirá la lógica de negocios para agregar, quitar y actualizar elementos en el carro de la compra.

La lógica de carro de la compra que va a agregar contendrá la funcionalidad para administrar las siguientes acciones:

1. Agregar elementos al carro de la compra
2. Quitar elementos del carro de la compra
3. Obtención del ID. de carro de la compra
4. Recuperación de elementos desde el carro de la compra
5. Calcular el total de todos los elementos del carro de la compra
6. Actualización de los datos del carro de la compra

Una página de carro de la compra (*ShoppingCart. aspx*) y la clase de carro de la compra se utilizarán juntas para tener acceso a los datos del carro de la compra. La página carro de la compra mostrará todos los elementos que el usuario agrega al carro de la compra. Además de la página y la clase del carro de la compra, creará una página (*AddToCart. aspx*) para agregar productos al carro de la compra. También agregará código a la página *productlist. aspx* y a la página *productdetails. aspx* que proporcionará un vínculo a la página *AddToCart. aspx* para que el usuario pueda agregar productos al carro de la compra.

En el diagrama siguiente se muestra el proceso básico que se produce cuando el usuario agrega un producto al carro de la compra.

![Carro de la compra: adición al carro de la compra](shopping-cart/_static/image3.png)

Cuando el usuario haga clic en el vínculo **Agregar al carro** en la página *productlist. aspx* o en la página *productdetails. aspx* , la aplicación navegará a la página *AddToCart. aspx* y, a continuación, automáticamente a la página *ShoppingCart. aspx* . La página *AddToCart. aspx* agregará el producto Select al carro de la compra llamando a un método de la clase ShoppingCart. La página *ShoppingCart. aspx* mostrará los productos que se han agregado al carro de la compra.

#### <a name="creating-the-shopping-cart-class"></a>Creación de la clase de carro de la compra

La clase `ShoppingCart` se agregará a una carpeta independiente en la aplicación para que se produzca una distinción clara entre el modelo (carpeta modelos), las páginas (carpeta raíz) y la lógica (carpeta lógica).

1. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **WingtipToys**y seleccione **Agregar**-&gt;**nueva carpeta**. Asigne un nombre a la nueva *lógica*de carpeta.
2. Haga clic con el botón secundario en la carpeta *lógica* y, a continuación, seleccione **Agregar** -&gt; **nuevo elemento**.
3. Agregue un nuevo archivo de clase denominado *ShoppingCartActions.CS*.
4. Reemplace el código predeterminado por el código siguiente:   

    [!code-csharp[Main](shopping-cart/samples/sample3.cs)]

El método `AddToCart` permite incluir productos individuales en el carro de la compra en función del `ID`del producto. El producto se agrega al carro o, si el carro ya contiene un elemento para ese producto, se incrementa la cantidad.

El método `GetCartId` devuelve el `ID` de carro para el usuario. El `ID` del carro se usa para realizar el seguimiento de los elementos que tiene un usuario en su carro de la compra. Si el usuario no tiene un carro existente `ID`, se crea un nuevo carro `ID`. Si el usuario ha iniciado sesión como usuario registrado, el carro `ID` se establece en su nombre de usuario. Sin embargo, si el usuario no ha iniciado sesión, el carro `ID` se establece en un valor único (un GUID). Un GUID garantiza que solo se crea un carro para cada usuario, en función de la sesión.

El método `GetCartItems` devuelve una lista de los elementos del carro de la compra del usuario. Más adelante en este tutorial, verá que el enlace de modelos se usa para mostrar los elementos del carro en el carro de la compra con el método `GetCartItems`.

### <a name="creating-the-add-to-cart-functionality"></a>Creación de la funcionalidad de complementos

Como se mencionó anteriormente, creará una página de procesamiento denominada *AddToCart. aspx* que se utilizará para agregar nuevos productos al carro de la compra del usuario. Esta página llamará al método `AddToCart` en la clase `ShoppingCart` que acaba de crear. La página *AddToCart. aspx* esperará que se le pase un `ID` del producto. Este `ID` de producto se usará al llamar al método `AddToCart` en la clase `ShoppingCart`.

> [!NOTE] 
> 
> Modificará el código subyacente (*AddToCart.aspx.CS*) de esta página, no la interfaz de usuario de la página (*AddToCart. aspx*).

#### <a name="to-create-the-add-to-cart-functionality"></a>Para crear la funcionalidad del complemento de carro:

1. En **Explorador de soluciones**, haga clic con el botón secundario en el proyecto **WingtipToys**, haga clic en **Agregar** -&gt; **nuevo elemento**.  
   Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. Agregue una nueva página estándar (formulario web) a la aplicación denominada *AddToCart. aspx*. 

    ![Carro de la compra: agregar formulario web](shopping-cart/_static/image4.png)
3. En **Explorador de soluciones**, haga clic con el botón secundario en la página *AddToCart. aspx* y, a continuación, haga clic en **Ver código**. El archivo de código subyacente *AddToCart.aspx.CS* se abre en el editor.
4. Reemplace el código existente en el código subyacente de *AddToCart.aspx.CS* por el siguiente:   

    [!code-csharp[Main](shopping-cart/samples/sample4.cs)]

Cuando se carga la página *AddToCart. aspx* , el `ID` de producto se recupera de la cadena de consulta. A continuación, se crea una instancia de la clase carro de la compra y se usa para llamar al método `AddToCart` que agregó anteriormente en este tutorial. El método `AddToCart`, contenido en el archivo *ShoppingCartActions.CS* , incluye la lógica para agregar el producto seleccionado al carro de la compra o incrementar la cantidad de producto del producto seleccionado. Si el producto no se ha agregado al carro de la compra, el producto se agrega a la tabla de `CartItem` de la base de datos. Si el producto ya se ha agregado al carro de la compra y el usuario agrega un elemento adicional del mismo producto, la cantidad del producto se incrementa en la tabla `CartItem`. Por último, la página redirige de nuevo a la página *ShoppingCart. aspx* que agregará en el paso siguiente, donde el usuario ve una lista actualizada de los elementos del carro.

Como se mencionó anteriormente, un usuario `ID` se usa para identificar los productos que están asociados a un usuario específico. Este `ID` se agrega a una fila de la tabla `CartItem` cada vez que el usuario agrega un producto al carro de la compra.

### <a name="creating-the-shopping-cart-ui"></a>Creación de la interfaz de usuario del carro de la compra

La página *ShoppingCart. aspx* mostrará los productos que el usuario ha agregado a su carro de la compra. También proporciona la capacidad de agregar, quitar y actualizar elementos en el carro de la compra.

1. En **Explorador de soluciones**, haga clic con el botón secundario en **WingtipToys**, haga clic en **Agregar** -&gt; **nuevo elemento**.  
   Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. Agregue una nueva página (formulario web) que incluya una página maestra seleccionando **Web Forms mediante la página maestra**. Asigne a la nueva página el nombre *ShoppingCart. aspx*.
3. Seleccione **site. Master** para adjuntar la página maestra a la página *. aspx* recién creada.
4. En la página *ShoppingCart. aspx* , reemplace el marcado existente por el marcado siguiente:   

    [!code-aspx[Main](shopping-cart/samples/sample5.aspx)]

La página *ShoppingCart. aspx* incluye un control **GridView** denominado `CartList`. Este control utiliza el enlace de modelos para enlazar los datos del carro de la compra de la base de datos al control **GridView** . Al establecer la propiedad `ItemType` del control **GridView** , la expresión de enlace de datos `Item` está disponible en el marcado del control y el control se vuelve fuertemente tipado. Como se mencionó anteriormente en esta serie de tutoriales, puede seleccionar detalles del objeto de `Item` mediante IntelliSense. Para configurar un control de datos para que use el enlace de modelos para seleccionar datos, establezca la propiedad `SelectMethod` del control. En el marcado anterior, establezca el `SelectMethod` para utilizar el método GetShoppingCartItems que devuelve una lista de objetos `CartItem`. El control de datos **GridView** llama al método en el momento adecuado del ciclo de vida de la página y enlaza automáticamente los datos devueltos. Todavía se debe agregar el método `GetShoppingCartItems`.

#### <a name="retrieving-the-shopping-cart-items"></a>Recuperación de los elementos del carro de la compra

A continuación, agregue código al código subyacente de *ShoppingCart.aspx.CS* para recuperar y rellenar la interfaz de usuario del carro de la compra.

1. En **Explorador de soluciones**, haga clic con el botón secundario en la página *ShoppingCart. aspx* y, a continuación, haga clic en **Ver código**. El archivo de código subyacente *ShoppingCart.aspx.CS* se abre en el editor.
2. Reemplace el código existente por el siguiente:  

    [!code-csharp[Main](shopping-cart/samples/sample6.cs)]

Como se mencionó anteriormente, el control de datos `GridView` llama al método `GetShoppingCartItems` en el momento adecuado del ciclo de vida de la página y enlaza automáticamente los datos devueltos. El método `GetShoppingCartItems` crea una instancia del objeto `ShoppingCartActions`. A continuación, el código usa esa instancia para devolver los elementos del carro llamando al método `GetCartItems`.

### <a name="adding-products-to-the-shopping-cart"></a>Agregar productos al carro de la compra

Cuando se muestra la página *productlist. aspx* o *productdetails. aspx* , el usuario podrá agregar el producto al carro de la compra mediante un vínculo. Al hacer clic en el vínculo, la aplicación navega a la página de procesamiento denominada *AddToCart. aspx*. La página *AddToCart. aspx* llamará al método `AddToCart` de la clase `ShoppingCart` que agregó anteriormente en este tutorial.

Ahora, agregará un vínculo **Agregar al carro** a las páginas *productlist. aspx* y *productdetails. aspx* . Este vínculo incluirá el `ID` del producto que se recupera de la base de datos.

1. En **Explorador de soluciones**, busque y abra la página denominada *productlist. aspx*.
2. Agregue el marcado resaltado en amarillo a la página *productlist. aspx* para que toda la página aparezca de la manera siguiente:  

    [!code-aspx[Main](shopping-cart/samples/sample7.aspx?highlight=50-54)]

### <a name="testing-the-shopping-cart"></a>Probar el carro de la compra

Ejecute la aplicación para ver cómo agrega productos al carro de la compra.

1. Presione **F5** para ejecutar la aplicación.  
 Después de que el proyecto vuelva a crear la base de datos, el explorador se abrirá y mostrará la página *default. aspx* .
2. Seleccione **automóviles** en el menú de navegación categoría.  
 Se muestra la página *productlist. aspx* que muestra solo los productos incluidos en la categoría "automóviles". 

    ![Carro de la compra: automóviles](shopping-cart/_static/image5.png)
3. Haga clic en el vínculo **Agregar al carro** situado junto al primer producto que aparece (el automóvil convertible).   
 Se muestra la página *ShoppingCart. aspx* , que muestra la selección en el carro de la compra. 

    ![Carro de la compra: carro](shopping-cart/_static/image6.png)
4. Para ver productos adicionales, seleccione **aviones** en el menú de navegación categoría.
5. Haga clic en el vínculo **Agregar al carro** situado junto al primer producto de la lista.  
 La página *ShoppingCart. aspx* se muestra con el elemento adicional.
6. Cierre el explorador.

### <a name="calculating-and-displaying-the-order-total"></a>Calcular y mostrar el total del pedido

Además de agregar productos al carro de la compra, agregará un método `GetTotal` a la clase `ShoppingCart` y mostrará el importe total del pedido en la página carro de la compra.

1. En **Explorador de soluciones**, abra el archivo *ShoppingCartActions.CS* en la carpeta *Logic* .
2. Agregue el siguiente método de `GetTotal` resaltado en amarillo a la clase `ShoppingCart`, de modo que la clase aparezca como sigue:   

    [!code-csharp[Main](shopping-cart/samples/sample8.cs?highlight=85-97)]

En primer lugar, el método `GetTotal` obtiene el identificador del carro de la compra del usuario. Después, el método obtiene el total del carro multiplicando el precio del producto por la cantidad de producto de cada producto que se muestra en el carro.

> [!NOTE] 
> 
> En el código anterior se usa el tipo que acepta valores NULL "`int?`". Los tipos que aceptan valores NULL pueden representar todos los valores de un tipo subyacente y también como un valor null. Para obtener más información, vea [usar tipos que aceptan valores NULL](https://msdn.microsoft.com/library/2cf62fcy(v=vs.110).aspx).

### <a name="modify-the-shopping-cart-display"></a>Modificar la presentación del carro de la compra

A continuación, modificará el código de la página *ShoppingCart. aspx* para llamar al método `GetTotal` y mostrará el total en la página *ShoppingCart. aspx* cuando se cargue la página.

1. En **Explorador de soluciones**, haga clic con el botón secundario en la página *ShoppingCart. aspx* y seleccione **Ver código**.
2. En el archivo *ShoppingCart.aspx.CS* , actualice el controlador de `Page_Load` agregando el siguiente código resaltado en amarillo:   

    [!code-csharp[Main](shopping-cart/samples/sample9.cs?highlight=16-31)]

Cuando se carga la página *ShoppingCart. aspx* , carga el objeto de carro de la compra y, a continuación, recupera el total del carro de la compra llamando al método `GetTotal` de la clase `ShoppingCart`. Si el carro de la compra está vacío, se muestra un mensaje al efecto.

### <a name="testing-the-shopping-cart-total"></a>Prueba del total del carro de la compra

Ejecute la aplicación ahora para ver cómo no solo puede Agregar un producto al carro de la compra, pero puede ver el total del carro de la compra.

1. Presione **F5** para ejecutar la aplicación.  
 El explorador se abrirá y mostrará la página *default. aspx* .
2. Seleccione **automóviles** en el menú de navegación categoría.
3. Haga clic en el vínculo **Agregar al carro** junto al primer producto.   
 La página *ShoppingCart. aspx* se muestra con el total del pedido. 

    ![Carro de la compra: total del carro](shopping-cart/_static/image7.png)
4. Agregue otros productos (por ejemplo, un plano) al carro.
5. La página *ShoppingCart. aspx* se muestra con un total actualizado de todos los productos que ha agregado. 

    ![Carro de la compra-varios productos](shopping-cart/_static/image8.png)
6. Cierre la ventana del explorador para detener la aplicación en ejecución.

### <a name="adding-update-and-checkout-buttons-to-the-shopping-cart"></a>Adición de botones de actualización y desprotección al carro de la compra

Para permitir que los usuarios modifiquen el carro de la compra, agregará un botón **Actualizar** y un botón **Desproteger** en la página carro de la compra. El botón **Desproteger** no se utiliza hasta más adelante en esta serie de tutoriales.

1. En **Explorador de soluciones**, abra la página *ShoppingCart. aspx* en la raíz del proyecto de aplicación Web.
2. Para agregar el botón **Actualizar** y el botón **Desproteger** a la página *ShoppingCart. aspx* , agregue el marcado resaltado en amarillo al marcado existente, tal como se muestra en el código siguiente:   

    [!code-aspx[Main](shopping-cart/samples/sample10.aspx?highlight=36-45)]

Cuando el usuario haga clic en el botón **Actualizar** , se llamará al controlador de eventos `UpdateBtn_Click`. Este controlador de eventos llamará al código que agregará en el paso siguiente.

A continuación, puede actualizar el código contenido en el archivo *ShoppingCart.aspx.CS* para recorrer los elementos del carro y llamar a los métodos `RemoveItem` y `UpdateItem`.

1. En **Explorador de soluciones**, abra el archivo *ShoppingCart.aspx.CS* en la raíz del proyecto de aplicación Web.
2. Agregue las siguientes secciones de código resaltadas en amarillo al archivo *ShoppingCart.aspx.CS* :   

    [!code-csharp[Main](shopping-cart/samples/sample11.cs?highlight=9-11,33,44-89)]

Cuando el usuario hace clic en el botón **Actualizar** de la página *ShoppingCart. aspx* , se llama al método UpdateCartItems. El método UpdateCartItems obtiene los valores actualizados de cada elemento en el carro de la compra. A continuación, el método UpdateCartItems llama al método `UpdateShoppingCartDatabase` (agregado y explicado en el paso siguiente) para agregar o quitar elementos del carro de la compra. Una vez actualizada la base de datos para reflejar las actualizaciones en el carro de la compra, el control **GridView** se actualiza en la página del carro de la compra llamando al método `DataBind` para **GridView**. Además, el importe total del pedido en la página del carro de la compra se actualiza para reflejar la lista actualizada de elementos.

### <a name="updating-and-removing-shopping-cart-items"></a>Actualización y eliminación de elementos del carro de la compra

En la página *ShoppingCart. aspx* , puede ver que se han agregado controles para actualizar la cantidad de un elemento y quitar un elemento. Ahora, agregue el código que hará que estos controles funcionen.

1. En **Explorador de soluciones**, abra el archivo *ShoppingCartActions.CS* en la carpeta *Logic* .
2. Agregue el siguiente código resaltado en amarillo al archivo de clase *ShoppingCartActions.CS* :   

    [!code-csharp[Main](shopping-cart/samples/sample12.cs?highlight=99-213)]

El método `UpdateShoppingCartDatabase`, al que se llama desde el método `UpdateCartItems` de la página *ShoppingCart.aspx.CS* , contiene la lógica para actualizar o quitar elementos del carro de la compra. El método `UpdateShoppingCartDatabase` recorre en iteración todas las filas de la lista de carro de la compra. Si se ha marcado un elemento de carro de la compra para quitarlo o la cantidad es menor que uno, se llama al método `RemoveItem`. De lo contrario, se comprueba si hay actualizaciones en el elemento de carro de la compra cuando se llama al método `UpdateItem`. Una vez que se ha quitado o actualizado el elemento del carro de la compra, se guardan los cambios de la base de datos.

La estructura de `ShoppingCartUpdates` se usa para contener todos los elementos del carro de la compra. El método `UpdateShoppingCartDatabase` usa la estructura `ShoppingCartUpdates` para determinar si es necesario actualizar o quitar alguno de los elementos.

En el siguiente tutorial, usará el método `EmptyCart` para borrar el carro de la compra después de comprar productos. Pero por ahora, usará el método `GetCount` que acaba de agregar al archivo *ShoppingCartActions.CS* para determinar cuántos elementos hay en el carro de la compra.

### <a name="adding-a-shopping-cart-counter"></a>Agregar un contador de carro de la compra

Para permitir al usuario ver el número total de elementos del carro de la compra, agregará un contador a la página *site. Master* . Este contador también actuará como un vínculo al carro de la compra.

1. En **Explorador de soluciones**, abra la página *site. Master* .
2. Modifique el marcado agregando el vínculo del contador de carro de la compra como se muestra en amarillo a la sección de navegación para que aparezca de la manera siguiente:  

    [!code-html[Main](shopping-cart/samples/sample13.html?highlight=6)]
3. A continuación, actualice el código subyacente del archivo *site.Master.CS* agregando el código resaltado en amarillo como se indica a continuación:  

    [!code-csharp[Main](shopping-cart/samples/sample14.cs?highlight=11,77-84)]

Antes de que la página se represente como HTML, se genera el evento `Page_PreRender`. En el controlador de `Page_PreRender`, el recuento total del carro de la compra se determina mediante una llamada al método `GetCount`. El valor devuelto se agrega al intervalo de `cartCount` incluido en el marcado de la página *site. Master* . La `<span>` etiquetas permite representar correctamente los elementos internos. Cuando se muestre cualquier página del sitio, se mostrará el total del carro de la compra. El usuario también puede hacer clic en el total del carro de la compra para mostrar el carro de la compra.

## <a name="testing-the-completed-shopping-cart"></a>Probar el carro de la compra completado

Ahora puede ejecutar la aplicación para ver cómo puede Agregar, eliminar y actualizar elementos en el carro de la compra. El total del carro de la compra reflejará el costo total de todos los artículos en el carro de la compra.

1. Presione **F5** para ejecutar la aplicación.  
 El explorador se abre y muestra la página *default. aspx* .
2. Seleccione **automóviles** en el menú de navegación categoría.
3. Haga clic en el vínculo **Agregar al carro** junto al primer producto.   
 La página *ShoppingCart. aspx* se muestra con el total del pedido.
4. Seleccione **planes** en el menú de navegación categoría.
5. Haga clic en el vínculo **Agregar al carro** junto al primer producto.
6. Establezca la cantidad del primer elemento en el carro de la compra en 3 y active la casilla **quitar elemento** del segundo elemento.<a id="a"></a>
7. Haga clic en el botón **Actualizar** para actualizar la página del carro de la compra y mostrar el nuevo total del pedido. 

    ![Carro de la compra: actualización del carro](shopping-cart/_static/image9.png)

## <a name="summary"></a>Resumen

En este tutorial, ha creado un carro de la compra para la aplicación de ejemplo de formularios Web de Wingtip Toys. Durante este tutorial, ha usado Entity Framework Code First, anotaciones de datos, controles de datos fuertemente tipados y enlace de modelos.

El carro de la compra permite agregar, eliminar y actualizar los artículos que el usuario ha seleccionado para su compra. Además de implementar la funcionalidad de carro de la compra, ha aprendido cómo mostrar los elementos del carro de la compra en un control **GridView** y calcular el total del pedido.

## <a name="addition-information"></a>Información adicional

[Introducción al estado de sesión de ASP.NET](https://msdn.microsoft.com/library/ms178581.aspx)

> [!div class="step-by-step"]
> [Anterior](display_data_items_and_details.md)
> [Siguiente](checkout-and-payment-with-paypal.md)
