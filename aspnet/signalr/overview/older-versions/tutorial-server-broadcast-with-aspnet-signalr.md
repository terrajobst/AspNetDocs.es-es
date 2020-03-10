---
uid: signalr/overview/older-versions/tutorial-server-broadcast-with-aspnet-signalr
title: 'Tutorial: difusión de servidores con ASP.NET Signalr 1. x | Microsoft Docs'
author: bradygaster
description: En este tutorial se muestra cómo crear una aplicación web que usa ASP.NET Signalr para proporcionar la funcionalidad de difusión del servidor. La difusión del servidor significa que el...
ms.author: bradyg
ms.date: 04/10/2013
ms.assetid: ab7b2554-956a-4f6d-b2a0-4ae0c62e8580
msc.legacyurl: /signalr/overview/older-versions/tutorial-server-broadcast-with-aspnet-signalr
msc.type: authoredcontent
ms.openlocfilehash: 68908be34f6b010e512677fe5f5e31bfdefab592
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467935"
---
# <a name="tutorial-server-broadcast-with-aspnet-signalr-1x"></a>Tutorial: difusión de servidores con ASP.NET Signalr 1. x

por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este tutorial se muestra cómo crear una aplicación web que usa ASP.NET Signalr para proporcionar la funcionalidad de difusión del servidor. Difusión de servidor significa que el servidor inicia las comunicaciones enviadas a los clientes. Este escenario requiere un enfoque de programación diferente que los escenarios punto a punto, como las aplicaciones de chat, en el que uno o varios clientes inician las comunicaciones enviadas a los clientes.
> 
> La aplicación que creará en este tutorial simula un tablero de cotizaciones, un escenario típico para la funcionalidad de difusión de servidor.
> 
> Los comentarios sobre el tutorial son bienvenidos. Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [stackoverflow.com](http://stackoverflow.com).

## <a name="overview"></a>Información general

El paquete de NuGet [Microsoft. Aspnet. signalr. Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) instala una aplicación de tablero de cotizaciones de ejemplo simulada en un proyecto de Visual Studio. En la primera parte de este tutorial, creará una versión simplificada de la aplicación desde cero. En el resto del tutorial, instalará el paquete NuGet y revisará las características y el código adicionales que crea.

La aplicación de cotización bursátil es un representante de un tipo de aplicación en tiempo real en el que desea "enviar" o difundir notificaciones del servidor a todos los clientes conectados periódicamente.

La aplicación que se va a compilar en la primera parte de este tutorial muestra una cuadrícula con datos de existencias.

![Versión inicial de StockTicker](tutorial-server-broadcast-with-aspnet-signalr/_static/image1.png)

Periódicamente, el servidor actualiza los precios de las acciones de forma aleatoria e envía las actualizaciones a todos los clientes conectados. En el explorador, los números y los símbolos de las columnas **cambio** y **%** cambian dinámicamente en respuesta a las notificaciones del servidor. Si abre más exploradores en la misma dirección URL, todos ellos muestran los mismos datos y los mismos cambios en los datos simultáneamente.

Este tutorial contiene las siguientes secciones:

- [Requisitos previos](#prerequisites)
- [Creación del proyecto](#createproject)
- [Agregar los paquetes de NuGet de Signalr](#nugetpackages)
- [Configurar el código de servidor](#server)
- [Configurar el código de cliente](#client)
- [Prueba de la aplicación](#test)
- [Habilitación del registro](#enablelogging)
- [Instalar y revisar el ejemplo completo de StockTicker](#fullsample)
- [Pasos siguientes](#nextsteps)

> [!NOTE]
> Si no desea trabajar a través de los pasos necesarios para compilar la aplicación, puede instalar el paquete Signalr. Sample en un nuevo proyecto de **aplicación Web ASP.net vacío** y leer estos pasos para obtener explicaciones del código. La primera parte del tutorial trata un subconjunto de Signalr. código de ejemplo y la segunda parte explica las características clave de la funcionalidad adicional en el paquete Signalr. sample.

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, asegúrese de que tiene Visual Studio 2012 o 2010 SP1 instalado en el equipo. Si no tiene Visual Studio, consulte [descargas de ASP.net](https://www.asp.net/downloads) para obtener la versión gratuita de visual Studio 2012 Express para Web.

Si tiene Visual Studio 2010, asegúrese de que [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) está instalado.

<a id="createproject"></a>

## <a name="create-the-project"></a>Crear el proyecto

1. En el menú **Archivo**, haga clic en **Nuevo proyecto**.
2. En el cuadro de diálogo **nuevo proyecto** , **C#** expanda en **plantillas** y seleccione **Web**.
3. Seleccione la plantilla de **aplicación Web vacía ASP.net** , asigne al proyecto el nombre *signalr. StockTicker*y haga clic en **Aceptar**.

    ![Cuadro de diálogo Nuevo proyecto](tutorial-server-broadcast-with-aspnet-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-nuget-packages"></a>Agregar los paquetes de NuGet de Signalr

### <a name="add-the-signalr-and-jquery-nuget-packages"></a>Agregar los paquetes de señalización y NuGet de JQuery

Puede Agregar la funcionalidad de Signalr a un proyecto mediante la instalación de un paquete NuGet.

1. Haga clic en **herramientas | Administrador de paquetes NuGet | Consola del administrador de paquetes**.
2. Escriba el siguiente comando en el administrador de paquetes.

    [!code-powershell[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample1.ps1)]

    El paquete de Signalr instala varios paquetes NuGet como dependencias. Una vez finalizada la instalación, tendrá todos los componentes de servidor y cliente necesarios para usar Signalr en una aplicación ASP.NET.

<a id="server"></a>

## <a name="set-up-the-server-code"></a>Configuración del código de servidor

En esta sección, configurará el código que se ejecuta en el servidor.

### <a name="create-the-stock-class"></a>Crear la clase stock

Empiece por crear la clase del modelo de existencias que usará para almacenar y transmitir información acerca de una cotización.

1. Cree un nuevo archivo de clase en la carpeta del proyecto, asígnele el nombre *stock.CS*y, a continuación, reemplace el código de plantilla por el código siguiente:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample2.cs)]

    Las dos propiedades que establecerá al crear las existencias son el símbolo (por ejemplo, MSFT para Microsoft) y el precio. Las demás propiedades dependen de cómo y cuándo se establece el precio. La primera vez que establezca Price, el valor se propagará a DayOpen. Las siguientes veces, cuando se establece Price, los valores de las propiedades Change y PercentChange se calculan en función de la diferencia entre Price y DayOpen.

### <a name="create-the-stockticker-and-stocktickerhub-classes"></a>Creación de las clases StockTicker y StockTickerHub

Usará la API de Signalr hub para controlar la interacción entre el servidor y el cliente. Una clase StockTickerHub que se deriva de la clase Signalr Hub controlará las conexiones de recepción y las llamadas a métodos de los clientes. También debe mantener los datos de stock y ejecutar un objeto de temporizador para activar periódicamente las actualizaciones de precios, independientemente de las conexiones de cliente. Estas funciones no se pueden incluir en una clase de concentrador, ya que las instancias de concentrador son transitorias. Se crea una instancia de clase de concentrador para cada operación en el concentrador, como conexiones y llamadas desde el cliente al servidor. Por lo tanto, el mecanismo que mantiene los datos de existencias, actualiza los precios y difunde las actualizaciones de precios tiene que ejecutarse en una clase independiente, que le dará nombre a StockTicker.

![Difusión desde StockTicker](tutorial-server-broadcast-with-aspnet-signalr/_static/image4.png)

Solo desea que se ejecute una instancia de la clase StockTicker en el servidor, por lo que deberá configurar una referencia de cada instancia de StockTickerHub a la instancia singleton de StockTicker. La clase StockTicker tiene que ser capaz de difundirse a los clientes porque tiene los datos bursátiles y desencadena actualizaciones, pero StockTicker no es una clase de concentrador. Por lo tanto, la clase StockTicker tiene que obtener una referencia al objeto de contexto de conexión del concentrador de Signalr. Después, puede usar el objeto de contexto de conexión de Signalr para difundir a los clientes.

1. En **Explorador de soluciones**, haga clic con el botón secundario en el proyecto y haga clic en **Agregar nuevo elemento**.
2. Si tiene Visual Studio 2012 con la [actualización de ASP.net and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=279941), haga clic en **Web** en **Visual C#**  y seleccione la plantilla de elemento **clase de concentrador de signalr** . En caso contrario, seleccione la plantilla **clase** .
3. Asigne a la nueva clase el nombre *StockTickerHub.CS*y, a continuación, haga clic en **Agregar**.

    ![Agregar StockTickerHub.cs](tutorial-server-broadcast-with-aspnet-signalr/_static/image5.png)
4. Reemplace el código de plantilla por el código siguiente:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample3.cs)]

    La clase [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) se utiliza para definir los métodos a los que los clientes pueden llamar en el servidor. Está definiendo un método: `GetAllStocks()`. Cuando un cliente se conecta inicialmente al servidor, llamará a este método para obtener una lista de todas las acciones con sus precios actuales. El método se puede ejecutar sincrónicamente y devolver `IEnumerable<Stock>` porque está devolviendo datos de la memoria. Si el método tuviera que obtener los datos haciendo algo que implique la espera, como una búsqueda en la base de datos o una llamada de servicio Web, especificaría `Task<IEnumerable<Stock>>` como el valor devuelto para habilitar el procesamiento asincrónico. Para obtener más información, consulte [Guía de la API de ASP.net signalr hubs-Server-when para ejecutar de forma asincrónica](index.md).

    El atributo HubName especifica cómo se hará referencia al concentrador en el código JavaScript en el cliente. El nombre predeterminado en el cliente si no se usa este atributo es una versión con mayúsculas y minúsculas Camel del nombre de clase, que en este caso sería stockTickerHub.

    Como verá más adelante cuando cree la clase StockTicker, se creará una instancia singleton de esa clase en su propiedad de instancia estática. Esa instancia singleton de StockTicker permanece en memoria independientemente del número de clientes que se conectan o desconectan, y esa instancia es lo que el método GetAllStocks usa para devolver la información bursátil actual.
5. Cree un nuevo archivo de clase en la carpeta del proyecto, asígnele el nombre *StockTicker.CS*y, a continuación, reemplace el código de plantilla por el código siguiente:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample4.cs)]

    Dado que varios subprocesos ejecutarán la misma instancia de código StockTicker, la clase StockTicker debe ser threadsafe.

    ### <a name="storing-the-singleton-instance-in-a-static-field"></a>Almacenar la instancia Singleton en un campo estático

    El código inicializa el campo de instancia de \_estático que respalda la propiedad de instancia con una instancia de la clase y esta es la única instancia de la clase que se puede crear, porque el constructor está marcado como privado. La [inicialización diferida](https://msdn.microsoft.com/library/dd997286.aspx) se usa para el campo instancia de \_, no por motivos de rendimiento, pero para asegurarse de que la creación de la instancia es threadsafe.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample5.cs)]

    Cada vez que un cliente se conecta al servidor, una nueva instancia de la clase StockTickerHub que se ejecuta en un subproceso independiente obtiene la instancia singleton de StockTicker de la propiedad estática StockTicker. Instance, como se vio anteriormente en la clase StockTickerHub.

    ### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Almacenar datos bursátiles en un ConcurrentDictionary

    El constructor inicializa la colección \_stocks con algunos datos bursátiles de ejemplo y GetAllStocks devuelve las acciones. Como vimos anteriormente, la colección de acciones la devuelve StockTickerHub. GetAllStocks, que es un método de servidor de la clase hub al que los clientes pueden llamar.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample6.cs)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample7.cs)]

    La colección de acciones se define como un tipo [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) para la seguridad para subprocesos. Como alternativa, puede usar un objeto de [Diccionario](https://msdn.microsoft.com/library/xfhwa508.aspx) y bloquear explícitamente el Diccionario al realizar cambios en él.

    En esta aplicación de ejemplo, es correcto almacenar los datos de la aplicación en la memoria y perder los datos cuando se desecha la instancia de StockTicker. En una aplicación real, trabajaría con un almacén de datos back-end, como una base de datos.

    ### <a name="periodically-updating-stock-prices"></a>Actualizar periódicamente los precios de las acciones

    El constructor inicia un objeto de temporizador que llama periódicamente a métodos que actualizan los precios de las acciones de forma aleatoria.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample8.cs)]

    El temporizador llama a UpdateStockPrices, que pasa null en el parámetro State. Antes de actualizar los precios, se realiza un bloqueo en el \_objeto updateStockPricesLock. El código comprueba si otro subproceso ya está actualizando los precios y, a continuación, llama a TryUpdateStockPrice en cada acción de la lista. El método TryUpdateStockPrice decide si se debe cambiar el precio de las acciones y cuánto se debe cambiar. Si se cambia el precio de las acciones, se llama a BroadcastStockPrice para difundir el cambio de precio de las acciones a todos los clientes conectados.

    La marca \_updatingStockPrices se marca como [volátil](https://msdn.microsoft.com/library/x13ttww7.aspx) para garantizar que el acceso a ella es threadsafe.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample9.cs)]

    En una aplicación real, el método TryUpdateStockPrice llamaría a un servicio web para buscar el precio; en este código, utiliza un generador de números aleatorios para realizar cambios de forma aleatoria.

    ### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Obtención del contexto de Signalr para que la clase StockTicker pueda difundirse a los clientes

    Dado que los cambios de precio se originan aquí en el objeto StockTicker, este es el objeto que necesita llamar a un método updateStockPrice en todos los clientes conectados. En una clase hub tiene una API para llamar a métodos de cliente, pero StockTicker no se deriva de la clase hub y no tiene una referencia a ningún objeto Hub. Por lo tanto, para difundir a los clientes conectados, la clase StockTicker tiene que obtener la instancia de contexto de Signalr para la clase StockTickerHub y usarla para llamar a métodos en los clientes.

    El código obtiene una referencia al contexto de Signalr cuando crea la instancia de la clase singleton, pasa esa referencia al constructor y el constructor lo coloca en la propiedad clients.

    Hay dos razones por las que desea obtener el contexto una sola vez: obtener el contexto es una operación costosa y obtenerlo una vez que se garantiza que se conserva el orden previsto de mensajes enviados a los clientes.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample10.cs)]

    Obtener la propiedad clients del contexto y colocarla en la propiedad StockTickerClient permite escribir código para llamar a métodos de cliente que tienen el mismo aspecto que en una clase hub. Por ejemplo, para difundir a todos los clientes, puede escribir clients. All. updateStockPrice (stock).

    El método updateStockPrice al que está llamando en BroadcastStockPrice no existe todavía; lo agregará más adelante cuando escriba código que se ejecuta en el cliente. Puede hacer referencia a updateStockPrice aquí porque clients. All es dinámico, lo que significa que la expresión se evaluará en tiempo de ejecución. Cuando se ejecuta esta llamada al método, Signalr enviará el nombre del método y el valor del parámetro al cliente, y si el cliente tiene un método denominado updateStockPrice, se llamará a ese método y se le pasará el valor del parámetro.

    Clients. All significa enviar a todos los clientes. Signalr ofrece otras opciones para especificar los clientes o grupos de clientes a los que se va a enviar. Para obtener más información, vea [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Registro de la ruta de Signalr

El servidor debe saber qué dirección URL se va a interceptar y dirigir a Signalr. Para ello, agregará código al archivo *global. asax* .

1. En **Explorador de soluciones**, haga clic con el botón secundario en el proyecto y, a continuación, haga clic en **Agregar nuevo elemento**.
2. Seleccione la plantilla de elemento de **clase de aplicación global** y, a continuación, haga clic en **Agregar**.

    ![Agregar global. asax](tutorial-server-broadcast-with-aspnet-signalr/_static/image6.png)
3. Agregue el código de registro de la ruta de Signalr al método de inicio de la aplicación\_:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample11.cs)]

    De forma predeterminada, la dirección URL base para todo el tráfico de Signalr es "/signalr" y "/signalr/hubs" se usa para recuperar un archivo JavaScript generado dinámicamente que define los proxies para todos los concentradores que tiene en la aplicación. El método MapHubs incluye sobrecargas que permiten especificar una dirección URL base diferente y ciertas opciones de Signalr en una instancia de la clase [HubConfiguration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubconfiguration(v=vs.111).aspx) .
4. Agregue una instrucción using al principio del archivo:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample12.cs)]
5. Guarde y cierre el archivo *global. asax* y compile el proyecto.

Ya ha completado la configuración del código de servidor. En la siguiente sección, configurará el cliente.

<a id="client"></a>

## <a name="set-up-the-client-code"></a>Configurar el código de cliente

1. Cree un nuevo archivo HTML en la carpeta del proyecto y asígnele el nombre *StockTicker. html*.
2. Reemplace el código de plantilla por el código siguiente:

    [!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample13.html)]

    El código HTML crea una tabla con 5 columnas, una fila de encabezado y una fila de datos con una sola celda que abarca las 5 columnas. La fila de datos muestra "cargando..." y solo se mostrarán en breve cuando se inicie la aplicación. El código de JavaScript quitará esa fila y agregará las filas de su ubicación con los datos de stock recuperados del servidor.

    Las etiquetas de script especifican el archivo de script de jQuery, el archivo de script Signalr Core, el archivo de script de proxies de Signalr y un archivo de script de StockTicker que creará más adelante. El archivo de script SignalRS proxies, que especifica la dirección URL "/signalr/hubs", se genera dinámicamente y define métodos de proxy para los métodos de la clase hub, en este caso para StockTickerHub. GetAllStocks. Si lo prefiere, puede generar este archivo JavaScript manualmente mediante las [utilidades de signalr](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) y deshabilitar la creación dinámica de archivos en la llamada al método MapHubs.
3. > [!IMPORTANT]
   > Asegúrese de que las referencias del archivo JavaScript en *StockTicker. html* son correctas. Es decir, asegúrese de que la versión de jQuery de la etiqueta de script (1.8.2 en el ejemplo) es la misma que la versión de jQuery en la carpeta *scripts* del proyecto y asegúrese de que la versión de signalr en la etiqueta de script sea la misma que la versión de signalr de la carpeta de *scripts* del proyecto. Si es necesario, cambie los nombres de archivo en las etiquetas de script.
4. En **Explorador de soluciones**, haga clic con el botón secundario en *StockTicker. html*y, a continuación, haga clic en **establecer como página de inicio**.
5. Cree un nuevo archivo JavaScript en la carpeta del proyecto y asígnele el nombre *StockTicker. js*.
6. Reemplace el código de plantilla por el código siguiente:

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample14.js)]

    $. Connection hace referencia a los servidores proxy de Signalr. El código obtiene una referencia al proxy de la clase StockTickerHub y la coloca en la variable de indicador de cotización. El nombre del proxy es el nombre que estableció el atributo [HubName]:

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample15.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample16.cs)]

    Una vez definidas todas las variables y funciones, la última línea de código del archivo inicializa la conexión de Signalr llamando a la función de inicio de Signalr. La función start se ejecuta de forma asincrónica y devuelve un [objeto de jQuery diferido](http://api.jquery.com/category/deferred-object/), lo que significa que se puede llamar a la función Done para especificar la función a la que se llamará cuando se complete la operación asincrónica.

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample17.js)]

    La función init llama a la función getAllStocks en el servidor y usa la información que el servidor devuelve para actualizar la tabla stock. Tenga en cuenta que, de forma predeterminada, tiene que usar mayúsculas y minúsculas Camel en el cliente, aunque el nombre del método es Pascal-Case en el servidor. La regla de grafía Camel solo se aplica a los métodos, no a los objetos. Por ejemplo, se hace referencia a las existencias. Symbol y stock. Precio, no stock. Symbol o stock. Price.

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample18.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample19.cs)]

    Si desea usar mayúsculas y minúsculas Pascal en el cliente, o si desea usar un nombre de método totalmente diferente, puede decorar el método de concentrador con el atributo HubMethodName de la misma manera que representaba la propia clase hub con el atributo HubName.

    En el método init, se crea HTML para una fila de la tabla para cada objeto estándar recibido del servidor mediante una llamada a formatStock para dar formato a las propiedades del objeto estándar y, a continuación, llamando a suplantador (que se define en la parte superior de *StockTicker. js*) para reemplazar los marcadores de posición en la variable rowTemplate por los valores de la propiedad de objeto stock. El HTML resultante se anexa a continuación a la tabla stock.

    Puede llamar a init pasándolo como una función de devolución de llamada que se ejecuta después de que se complete la función de inicio asincrónica. Si llamó a init como una instrucción de JavaScript independiente después de llamar a Start, se produciría un error en la función porque se ejecutaría inmediatamente sin esperar a que la función de inicio finalizara el establecimiento de la conexión. En ese caso, la función init intentaría llamar a la función getAllStocks antes de que se establezca la conexión con el servidor.

    Cuando el servidor cambia el precio de una cotización, llama a updateStockPrice en los clientes conectados. La función se agrega a la propiedad client del proxy de stockTicker para que esté disponible para las llamadas del servidor.

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample20.js)]

    La función updateStockPrice da formato a un objeto estándar recibido del servidor en una fila de la tabla de la misma forma que en la función init. Sin embargo, en lugar de anexar la fila a la tabla, busca la fila actual de las acciones en la tabla y la reemplaza por la nueva.

<a id="test"></a>

## <a name="test-the-application"></a>Probar la aplicación

1. Presione F5 para ejecutar la aplicación en modo de depuración.

    La tabla Stock muestra inicialmente el valor "cargando..." , después de un breve retraso, se muestran los datos de existencias iniciales y, a continuación, los precios de las acciones comienzan a cambiar.

    ![Carga](tutorial-server-broadcast-with-aspnet-signalr/_static/image7.png)

    ![Tabla de acciones iniciales](tutorial-server-broadcast-with-aspnet-signalr/_static/image8.png)

    ![Tabla de acciones que recibe los cambios del servidor](tutorial-server-broadcast-with-aspnet-signalr/_static/image9.png)
2. Copie la dirección URL de la barra de direcciones del explorador y péguela en una o varias ventanas nuevas del explorador.

    La pantalla de acciones inicial es igual que el primer explorador y los cambios se producen simultáneamente.
3. Cierre todos los exploradores y abra un nuevo explorador y, a continuación, vaya a la misma dirección URL.

    El objeto singleton de StockTicker ha continuado ejecutándose en el servidor, por lo que la presentación de la tabla de acciones muestra que las existencias han continuado para cambiar. (No verá la tabla inicial con cero figuras de cambio).
4. Cierre el explorador.

<a id="enablelogging"></a>

## <a name="enable-logging"></a>Habilite el registro

Signalr tiene una función de registro integrada que puede habilitar en el cliente para ayudar en la solución de problemas. En esta sección, habilitará el registro y verá ejemplos en los que se muestra cómo los registros le indican cuál de los siguientes métodos de transporte señalizador está usando:

- [WebSockets](http://en.wikipedia.org/wiki/WebSocket), admitidos por IIS 8 y exploradores actuales.
- [Eventos enviados por el servidor](http://en.wikipedia.org/wiki/Server-sent_events), admitidos por exploradores distintos de Internet Explorer.
- El [marco Forever](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), compatible con Internet Explorer.
- [Sondeo prolongado de Ajax](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), compatible con todos los exploradores.

Para una conexión determinada, Signalr elige el mejor método de transporte que admiten tanto el servidor como el cliente.

1. Abra *StockTicker. js* y agregue una línea de código para habilitar el registro inmediatamente antes del código que inicializa la conexión al final del archivo:

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample21.js)]
2. Presione F5 para ejecutar el proyecto.
3. Abra la ventana herramientas de desarrollo del explorador y seleccione la consola para ver los registros. Es posible que tenga que actualizar la página para ver los registros de Signalr que negocian el método de transporte para una nueva conexión.

    Si ejecuta Internet Explorer 10 en Windows 8 (IIS 8), el método de transporte es WebSockets.

    ![Consola de IE 10 IIS 8](tutorial-server-broadcast-with-aspnet-signalr/_static/image10.png)

    Si ejecuta Internet Explorer 10 en Windows 7 (IIS 7,5), el método de transporte es iframe.

    ![Consola de IE 10, IIS 7,5](tutorial-server-broadcast-with-aspnet-signalr/_static/image11.png)

    En Firefox, instale el complemento de Firebug para obtener una ventana de consola. Si está ejecutando Firefox 19 en Windows 8 (IIS 8), el método de transporte es WebSockets.

    ![WebSockets de Firefox 19 IIS 8](tutorial-server-broadcast-with-aspnet-signalr/_static/image12.png)

    Si está ejecutando Firefox 19 en Windows 7 (IIS 7,5), el método de transporte es eventos enviados por el servidor.

    ![Consola Firefox 19 IIS 7,5](tutorial-server-broadcast-with-aspnet-signalr/_static/image13.png)

<a id="fullsample"></a>

## <a name="install-and-review-the-full-stockticker-sample"></a>Instalar y revisar el ejemplo completo de StockTicker

La aplicación StockTicker instalada por el paquete de NuGet [Microsoft. Aspnet. signalr. Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) incluye más características que la versión simplificada que acaba de crear desde cero. En esta sección del tutorial, instalará el paquete NuGet y revisará las nuevas características y el código que los implementa.

### <a name="install-the-signalrsample-nuget-package"></a>Instalación del paquete de NuGet Signalr. Sample

1. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y haga clic en **administrar paquetes NuGet**.
2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **en línea**, escriba *signalr. Sample* en el cuadro **Buscar en línea** y, a continuación, haga clic en **instalar** en el paquete **signalr. Sample** .

    ![Instalación de Signalr. paquete de ejemplo](tutorial-server-broadcast-with-aspnet-signalr/_static/image14.png)
3. En el archivo *global. asax* , concomente como comentario el RouteTable. Routes. MapHubs (); línea que ha agregado anteriormente en el método de inicio de\_de la aplicación.

    El código de *global. asax* ya no es necesario porque el paquete signalr. Sample registra la ruta signalr en la *aplicación\_archivo Start/RegisterHubs. CS* :

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample22.cs)]

    La clase webactivator a la que hace referencia el atributo Assembly se incluye en el paquete NuGet WebActivatorEx, que se instala como una dependencia del paquete Signalr. sample.
4. En **Explorador de soluciones**, expanda la carpeta *signalr. Sample* que se creó mediante la instalación del paquete signalr. sample.
5. En la carpeta *signalr. Sample* , haga clic con el botón secundario en *StockTicker. html*y, a continuación, haga clic en **establecer como página de inicio**.

    > [!NOTE]
    > La instalación de Signalr. ejemplo de paquete NuGet podría cambiar la versión de jQuery que tiene en la carpeta *scripts* . El nuevo archivo *StockTicker. html* que el paquete instala en la carpeta *Signalr. Sample* estará sincronizado con la versión de jQuery que instala el paquete, pero si desea ejecutar de nuevo el archivo *StockTicker. html* original, es posible que tenga que actualizar la referencia de jQuery en la etiqueta de script en primer lugar.

### <a name="run-the-application"></a>Ejecutar la aplicación

1. Presione F5 para ejecutar la aplicación.

    Además de la cuadrícula que vio anteriormente, la aplicación Full stock ticker muestra una ventana con desplazamiento horizontal que muestra los mismos datos bursátiles. Al ejecutar la aplicación por primera vez, el "mercado" se "cierra" y verá una cuadrícula estática y una ventana de tablero de mandos que no se desplaza.

    ![Inicio de pantalla de StockTicker](tutorial-server-broadcast-with-aspnet-signalr/_static/image15.png)

    Al hacer clic en **abrir mercado**, el cuadro **tablero de cotizaciones en directo** comienza a desplazarse horizontalmente y el servidor empieza a difundir periódicamente los cambios de precio de las acciones de forma aleatoria. Cada vez que cambia el precio de las acciones, se actualizan la cuadrícula de **tabla de acciones en directo** y el cuadro de tablero de **cotizaciones** . Cuando el cambio de precio de una cotización es positivo, la acción se muestra con un fondo verde y, cuando el cambio es negativo, las acciones se muestran con un fondo rojo.

    ![Aplicación StockTicker, abierta en el mercado](tutorial-server-broadcast-with-aspnet-signalr/_static/image16.png)

    El botón **cerrar mercado** detiene los cambios y detiene el desplazamiento del tablero de cotizaciones, mientras que el botón **restablecer** restablece todos los datos de existencias en el estado inicial antes de que se inicien los cambios de precio. Si abre más ventanas del explorador y va a la misma dirección URL, verá que los mismos datos se actualizan dinámicamente al mismo tiempo en cada explorador. Al hacer clic en uno de los botones, todos los exploradores responden de la misma manera al mismo tiempo.

### <a name="live-stock-ticker-display"></a>Presentación del tablero de cotizaciones en directo

La presentación del **tablero de cotizaciones en directo** es una lista sin ordenar de un elemento div que está formateada en una sola línea mediante estilos CSS. El teletipo se inicializa y se actualiza de la misma forma que la tabla: reemplazando los marcadores de posición en una cadena de plantilla de &lt;Li&gt; y agregando dinámicamente los elementos de&gt; de &lt;Li al elemento &lt;UL&gt;. El desplazamiento se realiza mediante la función Animate de jQuery para variar el margen izquierdo de la lista sin ordenar en el elemento div.

El HTML del tablero de cotizaciones:

[!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample23.html)]

CSS del tablero de cotizaciones:

[!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample24.html)]

Código jQuery que hace que se desplace:

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample25.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Métodos adicionales en el servidor a los que el cliente puede llamar

La clase StockTickerHub define cuatro métodos adicionales a los que el cliente puede llamar:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample26.cs)]

Se llama a OpenMarket, CloseMarket y RESET en respuesta a los botones de la parte superior de la página. Demuestran el patrón de un cliente que desencadena un cambio de estado que se propaga inmediatamente a todos los clientes. Cada uno de estos métodos llama a un método de la clase StockTicker que afecta el cambio de estado del mercado y, a continuación, difunde el nuevo estado.

En la clase StockTicker, el estado del mercado se mantiene con una propiedad MarketState que devuelve un valor de enumeración MarketState:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample27.cs)]

Cada uno de los métodos que cambian el estado del mercado lo hacen dentro de un bloque de bloqueo porque la clase StockTicker debe ser threadsafe:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample28.cs)]

Para asegurarse de que este código es threadsafe, el campo de marketState \_que respalda la propiedad MarketState está marcado como volatile.

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample29.cs)]

Los métodos BroadcastMarketStateChange y BroadcastMarketReset son similares al método BroadcastStockPrice que ya vio, salvo que llaman a métodos diferentes definidos en el cliente:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample30.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Funciones adicionales en el cliente a las que el servidor puede llamar

La función updateStockPrice ahora controla la cuadrícula y la presentación del tablero, y utiliza jQuery. color para parpadear los colores rojo y verde.

Las nuevas funciones de *signalr. StockTicker. js* habilitan y deshabilitan los botones en función del estado del mercado, y detienen o inician el desplazamiento horizontal de la ventana del tablero de cotizaciones. Dado que se agregan varias funciones a ticker. Client, se usa la [función Extend de jQuery](http://api.jquery.com/jQuery.extend/) para agregarlas.

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample31.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Configuración adicional del cliente después de establecer la conexión

Una vez que el cliente establece la conexión, tiene que hacer algo más: averiguar si el mercado está abierto o cerrado con el fin de llamar a la función marketOpened o marketClosed adecuada y asociar las llamadas al método de servidor a los botones.

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample32.js)]

Los métodos de servidor no se conectan a los botones hasta que se establece la conexión, de modo que el código no puede intentar llamar a los métodos de servidor antes de que estén disponibles.

<a id="nextsteps"></a>

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a programar una aplicación Signalr que difunde los mensajes del servidor a todos los clientes conectados, tanto de forma periódica como en respuesta a las notificaciones de cualquier cliente. El patrón de uso de una instancia singleton multiproceso para mantener el estado del servidor también se puede usar en escenarios de juegos en línea de varios jugadores. Para ver un ejemplo, consulte [el juego del captador que se basa en signalr](https://github.com/NTaylorMullen/ShootR).

Para ver tutoriales que muestran escenarios de comunicación punto a punto, consulte [Introducción con signalr](index.md) y [la actualización en tiempo real con signalr](index.md).

Para obtener más información sobre los conceptos de desarrollo de Signalr más avanzado, visite los siguientes sitios para ver el código fuente y los recursos de Signalr:

- [ASP.NET SignalR](https://asp.net/signalr/)
- [Proyecto signalr](http://signalr.net/)
- [Github y ejemplos de signalr](https://github.com/SignalR/SignalR)
- [Wiki de signalr](https://github.com/SignalR/SignalR/wiki)
