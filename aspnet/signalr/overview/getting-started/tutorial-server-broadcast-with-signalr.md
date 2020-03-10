---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Tutorial: difusión de servidores con Signalr 2 | Microsoft Docs'
author: tdykstra
description: En este tutorial se muestra cómo crear una aplicación web que usa ASP.NET Signalr 2 para proporcionar la funcionalidad de difusión del servidor.
ms.author: bradyg
ms.date: 01/02/2019
ms.topic: tutorial
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 14924109fff8db3e537e6bc08b6dc868792ee660
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431245"
---
# <a name="tutorial-server-broadcast-with-signalr-2"></a>Tutorial: difusión de servidores con Signalr 2

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

En este tutorial se muestra cómo crear una aplicación web que usa ASP.NET Signalr 2 para proporcionar la funcionalidad de difusión del servidor. Difusión de servidor significa que el servidor inicia las comunicaciones enviadas a los clientes.

La aplicación que creará en este tutorial simula un tablero de cotizaciones, un escenario típico para la funcionalidad de difusión de servidor. Periódicamente, el servidor actualiza aleatoriamente los precios de las acciones y difunde las actualizaciones a todos los clientes conectados. En el explorador, los números y los símbolos de las columnas **cambiar** y **%** cambian dinámicamente en respuesta a las notificaciones del servidor. Si abre más exploradores en la misma dirección URL, todos ellos muestran los mismos datos y los mismos cambios en los datos simultáneamente.

![Crear Web](tutorial-server-broadcast-with-signalr/_static/image1.png)

En este tutorial va a:

> [!div class="checklist"]
> * Crear el proyecto
> * Configuración del código de servidor
> * Examinar el código del servidor
> * Configurar el código de cliente
> * Examinar el código de cliente
> * Probar la aplicación
> * Habilite el registro

> [!IMPORTANT]
> Si no desea trabajar a través de los pasos necesarios para compilar la aplicación, puede instalar el paquete Signalr. Sample en un nuevo proyecto de aplicación Web ASP.NET vacío. Si instala el paquete de NuGet sin realizar los pasos de este tutorial, debe seguir las instrucciones del archivo *README. txt* . Para ejecutar el paquete, debe agregar una clase de inicio OWIN que llame al método `ConfigureSignalR` del paquete instalado. Recibirá un error si no agrega la clase de inicio OWIN. Consulte la sección [instalación del ejemplo de StockTicker](#install-the-stockticker-sample) de este artículo.

## <a name="prerequisites"></a>Requisitos previos

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con la carga de trabajo de **ASP.NET y desarrollo web**.

## <a name="create-the-project"></a>Crear el proyecto

En esta sección se muestra cómo usar Visual Studio 2017 para crear una aplicación Web de ASP.NET vacía.

1. En Visual Studio, cree una aplicación Web ASP.NET.

    ![Crear Web](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. En la ventana **New ASP.NET Web Application-signalr. StockTicker** , deje **vacío** seleccionado y haga clic en **Aceptar**.

## <a name="set-up-the-server-code"></a>Configuración del código de servidor

En esta sección, configurará el código que se ejecuta en el servidor.

### <a name="create-the-stock-class"></a>Crear la clase stock

Empiece por crear la clase del modelo de *existencias* que usará para almacenar y transmitir información acerca de una cotización.

1. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **agregar** **clase**de > .

1. Asigne a la clase el nombre *stock* y agréguelo al proyecto.

1. Reemplace el código del archivo *stock.CS* por este código:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    Las dos propiedades que establecerá al crear acciones son `Symbol` (por ejemplo, MSFT para Microsoft) y `Price`. Las demás propiedades dependen de cómo y cuándo se establece `Price`. La primera vez que se establece `Price`, el valor se propaga a `DayOpen`. Después, al establecer `Price`, la aplicación calcula los valores de las propiedades `Change` y `PercentChange` en función de la diferencia entre `Price` y `DayOpen`.

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a>Creación de las clases StockTickerHub y StockTicker

Usará la API de Signalr hub para controlar la interacción entre el servidor y el cliente. Una clase `StockTickerHub` que se deriva de la clase Signalr `Hub` controlará las conexiones de recepción y las llamadas a métodos de los clientes. También debe mantener los datos de stock y ejecutar un objeto de `Timer`. El objeto `Timer` desencadenará periódicamente actualizaciones de precios independientes de las conexiones de cliente. Estas funciones no se pueden colocar en una clase `Hub`, ya que los concentradores son transitorios. La aplicación crea una instancia de clase `Hub` para cada tarea en el concentrador, como conexiones y llamadas desde el cliente al servidor. Por lo tanto, el mecanismo que mantiene los datos de existencias, actualiza los precios y difunde las actualizaciones de precios tiene que ejecutarse en una clase independiente. El nombre de la clase `StockTicker`.

![Difusión desde StockTicker](tutorial-server-broadcast-with-signalr/_static/image3.png)

Solo desea que una instancia de la clase `StockTicker` se ejecute en el servidor, por lo que tendrá que configurar una referencia de cada instancia de `StockTickerHub` a la instancia de `StockTicker` singleton. La clase `StockTicker` tiene que difundir a los clientes porque tiene los datos bursátiles y desencadena actualizaciones, pero `StockTicker` no es una clase `Hub`. La clase `StockTicker` tiene que obtener una referencia al objeto de contexto de conexión del concentrador de Signalr. Después, puede usar el objeto de contexto de conexión de Signalr para difundir a los clientes.

#### <a name="create-stocktickerhubcs"></a>Create StockTickerHub.cs

1. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **nuevo elemento**.

1. En **Agregar nuevo elemento-signalr. StockTicker**, seleccione **instalado** > **Visual C#**  > **Web** > **signalr** y, a continuación, seleccione **clase de concentrador de signalr (V2)** .

1. Asigne a la clase el nombre *StockTickerHub* y agréguelo al proyecto.

    En este paso se crea el archivo de clase *StockTickerHub.CS* . Simultáneamente, agrega un conjunto de archivos de script y referencias de ensamblado que admite Signalr en el proyecto.

1. Reemplace el código del archivo *StockTickerHub.CS* por este código:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. Guarde el archivo.

La aplicación usa la clase [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) para definir los métodos a los que los clientes pueden llamar en el servidor. Está definiendo un método: `GetAllStocks()`. Cuando un cliente se conecta inicialmente al servidor, llamará a este método para obtener una lista de todas las acciones con sus precios actuales. El método se puede ejecutar sincrónicamente y devolver `IEnumerable<Stock>` porque está devolviendo datos de la memoria.

Si el método tuviera que obtener los datos haciendo algo que implique la espera, como una búsqueda en la base de datos o una llamada de servicio Web, especificaría `Task<IEnumerable<Stock>>` como el valor devuelto para habilitar el procesamiento asincrónico. Para obtener más información, consulte [Guía de la API de ASP.net signalr hubs-Server-when para ejecutar de forma asincrónica](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).

El atributo `HubName` especifica cómo la aplicación hará referencia al centro en el código JavaScript en el cliente. El nombre predeterminado en el cliente si no se usa este atributo, es una versión camelCase del nombre de clase, que en este caso sería `stockTickerHub`.

Como verá más adelante cuando cree la clase `StockTicker`, la aplicación crea una instancia singleton de esa clase en su propiedad `Instance` estática. Esa instancia singleton de `StockTicker` está en memoria independientemente de cuántos clientes se conectan o desconectan. Esa instancia es lo que el método `GetAllStocks()` utiliza para devolver la información de existencias actual.

#### <a name="create-stocktickercs"></a>Crear StockTicker.cs

1. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **agregar** **clase**de > .

1. Asigne a la clase el nombre *StockTicker* y agréguelo al proyecto.

1. Reemplace el código del archivo *StockTicker.CS* por este código:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

Dado que todos los subprocesos ejecutarán la misma instancia de código StockTicker, la clase StockTicker debe ser segura para subprocesos.

### <a name="examine-the-server-code"></a>Examinar el código del servidor

Si examina el código del servidor, le ayudará a comprender el funcionamiento de la aplicación.

#### <a name="storing-the-singleton-instance-in-a-static-field"></a>Almacenar la instancia Singleton en un campo estático

El código inicializa el campo `_instance` estático que respalda la propiedad `Instance` con una instancia de la clase. Dado que el constructor es privado, es la única instancia de la clase que puede crear la aplicación. La aplicación usa la [inicialización diferida](/dotnet/framework/performance/lazy-initialization) para el campo `_instance`. No es por motivos de rendimiento. Es para asegurarse de que la creación de la instancia es segura para subprocesos.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

Cada vez que un cliente se conecta al servidor, una nueva instancia de la clase StockTickerHub que se ejecuta en un subproceso independiente obtiene la instancia singleton de StockTicker de la `StockTicker.Instance` propiedad estática, como se vio anteriormente en la clase `StockTickerHub`.

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Almacenar datos bursátiles en un ConcurrentDictionary

El constructor inicializa la colección de `_stocks` con algunos datos bursátiles de ejemplo y `GetAllStocks` devuelve las existencias. Como vimos anteriormente, `StockTickerHub.GetAllStocks`devuelve esta colección de acciones, que es un método de servidor de la clase `Hub` que los clientes pueden llamar.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

La colección de acciones se define como un tipo [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) para la seguridad para subprocesos. Como alternativa, puede usar un objeto de [Diccionario](https://msdn.microsoft.com/library/xfhwa508.aspx) y bloquear explícitamente el Diccionario al realizar cambios en él.

En esta aplicación de ejemplo, es correcto almacenar los datos de la aplicación en memoria y perder los datos cuando la aplicación desecha la instancia de `StockTicker`. En una aplicación real, se podía trabajar con un almacén de datos back-end como una base de datos.

#### <a name="periodically-updating-stock-prices"></a>Actualizar periódicamente los precios de las acciones

El constructor inicia un objeto `Timer` que llama periódicamente a métodos que actualizan los precios de las acciones de forma aleatoria.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 `Timer` llama a `UpdateStockPrices`, que pasa null en el parámetro State. Antes de actualizar los precios, la aplicación toma un bloqueo en el objeto de `_updateStockPricesLock`. El código comprueba si otro subproceso ya está actualizando los precios y, a continuación, llama a `TryUpdateStockPrice` en cada acción de la lista. El método `TryUpdateStockPrice` decide si se debe cambiar el precio de las acciones y cuánto se debe cambiar. Si cambia el precio de las acciones, la aplicación llama a `BroadcastStockPrice` para difundir el cambio de precio de las acciones a todos los clientes conectados.

La marca `_updatingStockPrices` designada como [volátil](https://msdn.microsoft.com/library/x13ttww7.aspx) para asegurarse de que es segura para subprocesos.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

En una aplicación real, el método `TryUpdateStockPrice` llamaría a un servicio web para buscar el precio. En este código, la aplicación usa un generador de números aleatorios para realizar cambios de forma aleatoria.

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Obtención del contexto de Signalr para que la clase StockTicker pueda difundirse a los clientes

Dado que los cambios de precio se originan aquí en el objeto `StockTicker`, es el objeto el que necesita llamar a un método `updateStockPrice` en todos los clientes conectados. En una clase `Hub`, tiene una API para llamar a métodos de cliente, pero `StockTicker` no se deriva de la clase `Hub` y no tiene una referencia a ningún objeto `Hub`. Para difundir a los clientes conectados, la clase `StockTicker` tiene que obtener la instancia de contexto de Signalr para la clase `StockTickerHub` y utilizarla para llamar a métodos en los clientes.

El código obtiene una referencia al contexto de Signalr cuando crea la instancia de la clase singleton, pasa esa referencia al constructor y el constructor lo coloca en la propiedad `Clients`.

Hay dos razones por las que desea obtener el contexto solo una vez: obtener el contexto es una tarea costosa y obtenerlo una vez que se asegura de que la aplicación conserva el orden previsto de mensajes enviados a los clientes.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

Obtener la propiedad `Clients` del contexto y colocarla en el `StockTickerClient` propiedad permite escribir código para llamar a métodos de cliente que tienen el mismo aspecto que en una clase `Hub`. Por ejemplo, para difundir a todos los clientes, puede escribir `Clients.All.updateStockPrice(stock)`.

El método de `updateStockPrice` al que está llamando en `BroadcastStockPrice` todavía no existe. Lo agregará más adelante cuando escriba código que se ejecuta en el cliente. Puede hacer referencia a `updateStockPrice` aquí porque `Clients.All` es dinámico, lo que significa que la aplicación evaluará la expresión en tiempo de ejecución. Cuando se ejecuta esta llamada al método, Signalr enviará el nombre del método y el valor del parámetro al cliente, y si el cliente tiene un método denominado `updateStockPrice`, la aplicación llamará a ese método y le pasará el valor del parámetro.

`Clients.All` significa enviar a todos los clientes. Signalr ofrece otras opciones para especificar los clientes o grupos de clientes a los que se va a enviar. Para obtener más información, vea [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Registro de la ruta de Signalr

El servidor debe saber qué dirección URL se va a interceptar y dirigir a Signalr. Para ello, agregue una clase de inicio OWIN:

1. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **nuevo elemento**.

1. En **Agregar nuevo elemento-signalr. StockTicker** seleccione **instalado** > **Visual C#**  > **Web** y, a continuación, seleccione **clase de inicio OWIN**.

1. Asigne a la clase el nombre *Startup* y seleccione **Aceptar**.

1. Reemplace el código predeterminado del archivo *Startup.CS* por este código:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

Ya ha terminado de configurar el código del servidor. En la siguiente sección, configurará el cliente.

## <a name="set-up-the-client-code"></a>Configurar el código de cliente

En esta sección, configurará el código que se ejecuta en el cliente.

### <a name="create-the-html-page-and-javascript-file"></a>Crear la página HTML y el archivo JavaScript

En la página HTML se mostrarán los datos y el archivo JavaScript organizará los datos.

#### <a name="create-stocktickerhtml"></a>Create StockTicker.html

En primer lugar, agregará el cliente HTML.

1. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **página HTML**.

1. Asigne al archivo el nombre *StockTicker* y seleccione **Aceptar**.

1. Reemplace el código predeterminado del archivo *StockTicker. html* por este código:

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    El código HTML crea una tabla con cinco columnas, una fila de encabezado y una fila de datos con una sola celda que abarca las cinco columnas. La fila de datos muestra "cargando..." momentáneamente cuando se inicia la aplicación. El código de JavaScript quitará esa fila y agregará las filas de su ubicación con los datos de stock recuperados del servidor.

    Las etiquetas de script especifican:

    * Archivo de script de jQuery.

    * Archivo de script de Signalr Core.

    * Archivo de script de proxy de Signalr.

    * Un archivo de script de StockTicker que creará más adelante.

    La aplicación genera dinámicamente el archivo de script de los proxies de Signalr. Especifica la dirección URL "/signalr/hubs" y define métodos de proxy para los métodos de la clase hub, en este caso, para `StockTickerHub.GetAllStocks`. Si lo prefiere, puede generar este archivo JavaScript manualmente mediante las [utilidades de signalr](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/). No olvide deshabilitar la creación dinámica de archivos en la llamada al método `MapHubs`.

1. En **Explorador de soluciones**, expanda **scripts**.

    Las bibliotecas de scripts para jQuery y Signalr están visibles en el proyecto.

    > [!IMPORTANT]
    > El administrador de paquetes instalará una versión posterior de los scripts de Signalr.

1. Actualice las referencias de script en el bloque de código para que correspondan a las versiones de los archivos de script del proyecto.

1. En **Explorador de soluciones**, haga clic con el botón secundario en *StockTicker. html*y seleccione **establecer como página principal**.

#### <a name="create-stocktickerjs"></a>Crear StockTicker. js

Ahora cree el archivo JavaScript.

1. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **archivo JavaScript**.

1. Asigne al archivo el nombre *StockTicker* y seleccione **Aceptar**.

1. Agregue este código al archivo *StockTicker. js* :

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a>Examinar el código de cliente

Si examina el código de cliente, le ayudará a saber cómo interactúa el código de cliente con el código de servidor para que la aplicación funcione.

#### <a name="starting-the-connection"></a>Inicio de la conexión

`$.connection` hace referencia a los servidores proxy de Signalr. El código obtiene una referencia al proxy para la clase `StockTickerHub` y la coloca en la variable `ticker`. El nombre del proxy es el nombre que estableció el atributo `HubName`:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

Después de definir todas las variables y funciones, la última línea de código del archivo inicializa la conexión Signalr llamando a la función Signalr `start`. La función `start` se ejecuta de forma asincrónica y devuelve un [objeto de jQuery diferido](http://api.jquery.com/category/deferred-object/). Puede llamar a la función Done para especificar la función a la que se llama cuando la aplicación finaliza la acción asincrónica.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a>Obtención de todas las acciones

La función `init` llama a la función `getAllStocks` en el servidor y usa la información que el servidor devuelve para actualizar la tabla stock. Tenga en cuenta que, de forma predeterminada, tiene que usar alterne en el cliente aunque el nombre del método sea Pascal-Case en el servidor. La regla alterne solo se aplica a los métodos, no a los objetos. Por ejemplo, se hace referencia a `stock.Symbol` y `stock.Price`, no `stock.symbol` o `stock.price`.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

En el método `init`, la aplicación crea HTML para una fila de la tabla para cada objeto estándar recibido del servidor mediante una llamada a `formatStock` para dar formato a las propiedades del objeto `stock` y, a continuación, al llamar a `supplant` para reemplazar los marcadores de posición en la variable `rowTemplate` por los valores de propiedad del objeto `stock`. El HTML resultante se anexa a continuación a la tabla stock.

> [!NOTE]
> Llame a `init` pasándolo como una función de `callback` que se ejecuta después de que finalice la función de `start` asincrónica. Si llamó a `init` como una instrucción de JavaScript independiente después de llamar a `start`, la función produciría un error porque se ejecutaría inmediatamente sin esperar a que la función de inicio finalizara el establecimiento de la conexión. En ese caso, la función de `init` intentaría llamar a la función de `getAllStocks` antes de que la aplicación establezca una conexión de servidor.

#### <a name="getting-updated-stock-prices"></a>Obtención de precios de cotización actualizados

Cuando el servidor cambia el precio de una cotización, llama al `updateStockPrice` en los clientes conectados. La aplicación agrega la función a la propiedad client del proxy de `stockTicker` para que esté disponible para las llamadas del servidor.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

La función `updateStockPrice` da formato a un objeto stock recibido del servidor en una fila de la tabla de la misma forma que en la función `init`. En lugar de anexar la fila a la tabla, busca la fila actual de las acciones en la tabla y la reemplaza por la nueva.

## <a name="test-the-application"></a>Probar la aplicación

Puede probar la aplicación para asegurarse de que funciona. Verá que todas las ventanas del explorador muestran la tabla de acciones en directo con los precios de las acciones fluctuando.

1. En la barra de herramientas, active la **depuración de scripts** y, después, seleccione el botón reproducir para ejecutar la aplicación en modo de depuración.

    ![Captura de pantalla del usuario que activa el modo de depuración y selecciona reproducir.](tutorial-server-broadcast-with-signalr/_static/image4.png)

    Se abrirá una ventana del explorador que muestra la **tabla de acciones en directo**. La tabla Stock muestra inicialmente el valor "cargando..." la línea, después, después de un breve período, la aplicación muestra los datos de existencias iniciales y, a continuación, los precios de las acciones comienzan a cambiar.

1. Copie la dirección URL del explorador, abra otros dos exploradores y pegue las direcciones URL en las barras de direcciones.

    La pantalla de acciones inicial es igual que el primer explorador y los cambios se producen simultáneamente.

1. Cierre todos los exploradores, abra un nuevo explorador y vaya a la misma dirección URL.

    El objeto singleton de StockTicker continuó ejecutándose en el servidor. La **tabla de acciones en directo** muestra que las existencias han ido a cambiar. No se ve la tabla inicial con cero figuras de cambios.

1. Cierre el explorador.

## <a name="enable-logging"></a>Habilite el registro

Signalr tiene una función de registro integrada que puede habilitar en el cliente para ayudar en la solución de problemas. En esta sección, habilitará el registro y verá ejemplos que muestran cómo los registros le indican cuál de los siguientes métodos de transporte señalizador está usando:

* [WebSockets](http://en.wikipedia.org/wiki/WebSocket), admitidos por IIS 8 y exploradores actuales.

* [Eventos enviados por el servidor](http://en.wikipedia.org/wiki/Server-sent_events), admitidos por exploradores distintos de Internet Explorer.

* El [marco Forever](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), compatible con Internet Explorer.

* [Sondeo prolongado de Ajax](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), compatible con todos los exploradores.

Para una conexión determinada, Signalr elige el mejor método de transporte que admiten tanto el servidor como el cliente.

1. Abra *StockTicker. js*.

1. Agregue esta línea de código resaltada para habilitar el registro inmediatamente antes del código que inicializa la conexión al final del archivo:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. Presione **F5** para ejecutar el proyecto.

1. Abra la ventana herramientas de desarrollo del explorador y seleccione la consola para ver los registros. Es posible que tenga que actualizar la página para ver los registros de Signalr que negocian el método de transporte para una nueva conexión.

    * Si está ejecutando Internet Explorer 10 en Windows 8 (IIS 8), el método de transporte es **WebSockets**.

    * Si está ejecutando Internet Explorer 10 en Windows 7 (IIS 7,5), el método de transporte es **iframe**.

    * Si está ejecutando Firefox 19 en Windows 8 (IIS 8), el método de transporte es **WebSockets**.

        > [!TIP]
        > En Firefox, instale el complemento de Firebug para obtener una ventana de consola.

    * Si está ejecutando Firefox 19 en Windows 7 (IIS 7,5), el método de transporte es eventos **enviados por el servidor** .

## <a name="install-the-stockticker-sample"></a>Instalación del ejemplo de StockTicker

[Microsoft. Aspnet. signalr. Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) instala la aplicación StockTicker. El paquete NuGet incluye más características que la versión simplificada que creó desde cero. En esta sección del tutorial, instalará el paquete NuGet y revisará las nuevas características y el código que los implementa.

> [!IMPORTANT]
> Si instala el paquete sin realizar los pasos anteriores de este tutorial, debe agregar una clase de inicio OWIN al proyecto. Este archivo README. txt para el paquete NuGet explica este paso.

### <a name="install-the-signalrsample-nuget-package"></a>Instalación del paquete de NuGet Signalr. Sample

1. En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Administrar paquetes NuGet**.

1. En el **Administrador de paquetes NuGet: signalr. StockTicker**, seleccione **examinar**.

1. En **origen del paquete**, seleccione **Nuget.org**.

1. Escriba *signalr. Sample* en el cuadro de búsqueda y seleccione **Microsoft. Aspnet. signalr. Sample** > **instalar**.

1. En **Explorador de soluciones**, expanda la carpeta *signalr. Sample* .

    La instalación del paquete Signalr. Sample creó la carpeta y su contenido.

1. En la carpeta *signalr. Sample* , haga clic con el botón secundario en *StockTicker. html*y seleccione **establecer como página de inicio**.

    > [!NOTE]
    > La instalación de Signalr. ejemplo de paquete NuGet podría cambiar la versión de jQuery que tiene en la carpeta *scripts* . El nuevo archivo *StockTicker. html* que el paquete instala en la carpeta *Signalr. Sample* estará sincronizado con la versión de jQuery que instala el paquete, pero si desea ejecutar de nuevo el archivo *StockTicker. html* original, es posible que tenga que actualizar la referencia de jQuery en la etiqueta de script en primer lugar.

### <a name="run-the-application"></a>Ejecutar la aplicación

 La tabla que vio en la primera aplicación tenía características útiles. La aplicación Full stock ticker muestra las nuevas características: una ventana con desplazamiento horizontal que muestra los datos y los valores bursátiles que cambian de color a medida que aumentan y disminuyen.

1. Presione **F5** para ejecutar la aplicación.

     Al ejecutar la aplicación por primera vez, el "mercado" se "cierra" y verá una tabla estática y una ventana de tablero de cotizaciones que no se desplazan.

1. Seleccione **abrir mercado**.

    ![Captura de pantalla del tablero de cotizaciones activo.](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * El cuadro **tablero de cotizaciones en directo** comienza a desplazarse horizontalmente y el servidor empieza a difundir periódicamente los cambios de precio de las acciones de forma aleatoria.

    * Cada vez que cambia el precio de las acciones, la aplicación actualiza la **tabla de acciones en directo** y el tablero de **cotizaciones en directo**.

    * Cuando el cambio de precio de una acción es positivo, la aplicación muestra las existencias con un fondo verde.

    * Cuando el cambio es negativo, la aplicación muestra los valores con un fondo rojo.

1. Seleccione **cerrar mercado**.

    * Se detiene la actualización de la tabla.

    * El teletipo deja de desplazarse.

1. Seleccione **Restablecer**.

    * Se restablecen todos los datos bursátiles.

    * La aplicación restaura el estado inicial antes de que se inicien los cambios de precio.

1. Copie la dirección URL del explorador, abra otros dos exploradores y pegue las direcciones URL en las barras de direcciones.

1. Verá que los mismos datos se actualizan dinámicamente al mismo tiempo en cada explorador.

1. Al seleccionar cualquiera de los controles, todos los exploradores responden de la misma manera al mismo tiempo.

### <a name="live-stock-ticker-display"></a>Presentación del tablero de cotizaciones en directo

La presentación del **tablero de cotizaciones en directo** es una lista sin ordenar de un elemento `<div>` con formato en una sola línea mediante estilos CSS. La aplicación inicializa y actualiza el tablero de la misma forma que la tabla: reemplazando los marcadores de posición en una cadena de plantilla `<li>` y agregando dinámicamente los elementos de `<li>` al elemento `<ul>`. La aplicación incluye el desplazamiento mediante la función `animate` de jQuery para modificar el margen izquierdo de la lista sin ordenar en el `<div>`.

#### <a name="signalrsample-stocktickerhtml"></a>Signalr. Sample StockTicker. html

Código HTML del tablero de cotizaciones:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a>Signalr. Sample StockTicker. CSS

Código CSS del tablero de cotizaciones:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a>Signalr. Sample Signalr. StockTicker. js

Código jQuery que hace que se desplace:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Métodos adicionales en el servidor a los que el cliente puede llamar

Para agregar flexibilidad a la aplicación, hay métodos adicionales a los que puede llamar la aplicación.

#### <a name="signalrsample-stocktickerhubcs"></a>Signalr. Sample StockTickerHub.cs

La clase `StockTickerHub` define cuatro métodos adicionales a los que el cliente puede llamar:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

La aplicación llama a `OpenMarket`, `CloseMarket`y `Reset` en respuesta a los botones de la parte superior de la página. Demuestran el patrón de un cliente que desencadena un cambio en el estado propagado inmediatamente a todos los clientes. Cada uno de estos métodos llama a un método en la clase `StockTicker` que provoca el cambio de estado del mercado y, a continuación, difunde el nuevo estado.

#### <a name="signalrsample-stocktickercs"></a>Signalr. Sample StockTicker.cs

En la clase `StockTicker`, la aplicación mantiene el estado del mercado con una propiedad `MarketState` que devuelve un valor de enumeración `MarketState`:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

Cada uno de los métodos que cambian el estado del mercado lo hacen dentro de un bloque de bloqueo porque la clase `StockTicker` tiene que ser segura para subprocesos:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

Para asegurarse de que este código es seguro para subprocesos, el `_marketState` campo que respalda la propiedad `MarketState` designada `volatile`:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

Los métodos `BroadcastMarketStateChange` y `BroadcastMarketReset` son similares al método BroadcastStockPrice que ya vio, salvo que llaman a métodos diferentes definidos en el cliente:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Funciones adicionales en el cliente a las que el servidor puede llamar

La función `updateStockPrice` ahora controla la tabla y la presentación del tablero, y utiliza `jQuery.Color` para los colores rojo y verde.

Las nuevas funciones de *signalr. StockTicker. js* habilitan y deshabilitan los botones en función del estado del mercado. También detienen o inician el desplazamiento horizontal del **tablero de cotizaciones en directo** . Dado que muchas funciones se agregan a `ticker.client`, la aplicación usa la [función Extend de jQuery](http://api.jquery.com/jQuery.extend/) para agregarlas.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Configuración adicional del cliente después de establecer la conexión

Una vez que el cliente establece la conexión, tiene que hacer algo más:

* Averigüe si el mercado está abierto o cerrado para llamar a la función `marketOpened` o `marketClosed` adecuada.

* Adjunte las llamadas de método de servidor a los botones.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

Los métodos de servidor no se conectan a los botones hasta que la aplicación establece la conexión. Es para que el código no pueda llamar a los métodos de servidor antes de que estén disponibles.

## <a name="additional-resources"></a>Recursos adicionales

En este tutorial ha aprendido a programar una aplicación Signalr que difunde mensajes del servidor a todos los clientes conectados. Ahora puede difundir mensajes periódicamente y en respuesta a las notificaciones de cualquier cliente. Puede usar el concepto de instancia de singleton multiproceso para mantener el estado del servidor en escenarios de juegos en línea de varios jugadores. Para ver un ejemplo, consulte [el juego del captador basado en signalr](https://github.com/NTaylorMullen/ShootR).

Para ver tutoriales que muestran escenarios de comunicación punto a punto, consulte [Introducción con signalr](introduction-to-signalr.md) y [la actualización en tiempo real con signalr](tutorial-high-frequency-realtime-with-signalr.md).

Para obtener más información acerca de Signalr, consulte los siguientes recursos:

* [ASP.NET SignalR](../../index.md)
* [Proyecto signalr](http://signalr.net/)
* [GitHub y ejemplos de signalr](https://github.com/SignalR/SignalR)
* [Wiki de signalr](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial va a:

> [!div class="checklist"]
> * Creó el proyecto
> * Configuración del código de servidor
> * Examen del código de servidor
> * Configurar el código de cliente
> * Examina el código de cliente
> * Probar la aplicación
> * Registro habilitado

Avance al siguiente artículo para aprender a crear una aplicación web en tiempo real que use ASP.NET Signalr 2.
> [!div class="nextstepaction"]
> [Creación de una aplicación web en tiempo real con Signalr](real-time-web-applications-with-signalr.md)
