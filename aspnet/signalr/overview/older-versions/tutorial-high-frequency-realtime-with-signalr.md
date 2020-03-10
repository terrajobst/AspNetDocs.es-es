---
uid: signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
title: Alta frecuencia en tiempo real con Signalr 1. x | Microsoft Docs
author: bradygaster
description: En este tutorial se muestra cómo crear una aplicación web que usa ASP.NET Signalr para proporcionar la funcionalidad de mensajería de alta frecuencia. Mensajería de alta frecuencia en...
ms.author: bradyg
ms.date: 04/16/2013
ms.assetid: ad2a5da5-2e79-40ea-bc84-028d327f5982
msc.legacyurl: /signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: e37e63a410952ec170cbbaadeeb54eae7e82b3b5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78505717"
---
# <a name="high-frequency-realtime-with-signalr-1x"></a>Alta frecuencia en tiempo real con SignalR 1.x

por [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este tutorial se muestra cómo crear una aplicación web que usa ASP.NET Signalr para proporcionar la funcionalidad de mensajería de alta frecuencia. La mensajería de alta frecuencia en este caso hace referencia a las actualizaciones que se envían a una velocidad fija; en el caso de esta aplicación, hasta 10 mensajes por segundo.
> 
> La aplicación que creará en este tutorial muestra una forma que los usuarios pueden arrastrar. La posición de la forma en todos los demás exploradores conectados se actualizará para que coincida con la posición de la forma arrastrada mediante actualizaciones con tiempo.
> 
> Los conceptos presentados en este tutorial tienen aplicaciones en juegos en tiempo real y otras aplicaciones de simulación.
> 
> Los comentarios sobre el tutorial son bienvenidos. Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [stackoverflow.com](http://stackoverflow.com).

## <a name="overview"></a>Información general

En este tutorial se muestra cómo crear una aplicación que comparte el estado de un objeto con otros exploradores en tiempo real. La aplicación que crearemos se denomina MoveShape. La página MoveShape mostrará un elemento HTML div que el usuario puede arrastrar; Cuando el usuario arrastra el elemento div, su nueva posición se enviará al servidor, lo que le indicará a todos los demás clientes conectados que actualicen la posición de la forma para que coincida.

![Ventana de la aplicación](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

La aplicación creada en este tutorial se basa en una demostración de Damian Edwards. [Aquí](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR)puede ver un vídeo que contiene esta demostración.

El tutorial comenzará mostrando cómo enviar mensajes de Signalr desde cada evento que se activa cuando se arrastra la forma. Cada cliente conectado actualizará la posición de la versión local de la forma cada vez que se reciba un mensaje.

Aunque la aplicación funcionará con este método, no es un modelo de programación recomendado, ya que no hay ningún límite superior para el número de mensajes que se envían, por lo que los clientes y el servidor podrían verse abrumados con los mensajes y el rendimiento se degradaría . La animación mostrada en el cliente también se separaría, ya que la forma se movería al instante por cada método, en lugar de moverse sin problemas a cada nueva ubicación. En secciones posteriores del tutorial se muestra cómo crear una función de temporizador que restringe la velocidad máxima a la que el cliente o el servidor envían los mensajes, y cómo se mueve la forma sin problemas entre las ubicaciones. La versión final de la aplicación creada en este tutorial se puede descargar desde la [Galería de código](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Este tutorial contiene las siguientes secciones:

- [Requisitos previos](#prerequisites)
- [Creación del proyecto](#createtheproject)
- [Agregue los paquetes NuGet de ASP.NET Signalr y JQuery. UI](#nugetpackages)
- [Crear la aplicación base](#baseapp)
- [Agregar el bucle de cliente](#clientloop)
- [Agregar el bucle de servidor](#serverloop)
- [Agregar la animación Smooth en el cliente](#animation)
- [Pasos adicionales](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Requisitos previos

En este tutorial se requiere Visual Studio 2012 o Visual Studio 2010. Si se usa Visual Studio 2010, el proyecto usará .NET Framework 4 en lugar de .NET Framework 4,5.

Si usa Visual Studio 2012, se recomienda que instale la [actualización de ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650). Esta actualización contiene nuevas características, como mejoras en la publicación, nueva funcionalidad y nuevas plantillas.

Si tiene Visual Studio 2010, asegúrese de que [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) está instalado.

<a id="createtheproject"></a>

## <a name="create-the-project"></a>Crear el proyecto

En esta sección, vamos a crear el proyecto en Visual Studio.

1. En el menú **Archivo**, haga clic en **Nuevo proyecto**.
2. En el cuadro de diálogo **nuevo proyecto** , **C#** expanda en **plantillas** y seleccione **Web**.
3. Seleccione la plantilla de **aplicación Web vacía ASP.net** , asigne al proyecto el nombre *MoveShapeDemo*y haga clic en **Aceptar**.

    ![Crear el nuevo proyecto](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-and-jqueryui-nuget-packages"></a>Agregar los paquetes NuGet Signalr y JQuery. UI

Puede Agregar la funcionalidad de Signalr a un proyecto mediante la instalación de un paquete NuGet. En este tutorial también se utilizará el paquete JQuery. UI para permitir arrastrar y animar la forma.

1. Haga clic en **herramientas | Administrador de paquetes NuGet | Consola del administrador de paquetes**.
2. Escriba el siguiente comando en el administrador de paquetes.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.ps1)]

    El paquete de Signalr instala varios paquetes NuGet como dependencias. Una vez finalizada la instalación, tendrá todos los componentes de servidor y cliente necesarios para usar Signalr en una aplicación ASP.NET.
3. Escriba el siguiente comando en la consola del administrador de paquetes para instalar los paquetes JQuery y JQuery. UI.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.ps1)]

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>Crear la aplicación base

En esta sección, vamos a crear una aplicación de explorador que envía la ubicación de la forma al servidor durante cada evento de movimiento del mouse. A continuación, el servidor difunde esta información a todos los demás clientes conectados a medida que se reciben. Expandiremos esta aplicación en secciones posteriores.

1. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar**, **clase.** ... Asigne a la clase el nombre **MoveShapeHub** y haga clic en **Agregar**.
2. Reemplace el código de la nueva clase **MoveShapeHub** por el código siguiente.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.cs)]

    La `MoveShapeHub` clase anterior es una implementación de un concentrador Signalr. Como en el tutorial [Introducción con signalr](index.md) , el concentrador tiene un método al que los clientes llamarán directamente. En este caso, el cliente enviará un objeto que contiene las nuevas coordenadas X e y de la forma al servidor, que se difunde a todos los demás clientes conectados. Signalr serializará automáticamente este objeto mediante JSON.

    El objeto que se enviará al cliente (`ShapeModel`) contiene miembros para almacenar la posición de la forma. La versión del objeto en el servidor también contiene un miembro para realizar el seguimiento de los datos del cliente que se están almacenando, para que no se envíen sus propios datos a un cliente determinado. Este miembro utiliza el atributo `JsonIgnore` para impedir que se serialice y se envíe al cliente.
3. A continuación, configuraremos el concentrador cuando se inicie la aplicación. En **Explorador de soluciones**, haga clic con el botón secundario en el proyecto y, a continuación, haga clic en **Agregar | Clase de aplicación global**. Acepte el nombre predeterminado *global* y haga clic en **Aceptar**.

    ![Agregar clase de aplicación global](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
4. Agregue la siguiente instrucción de `using` después de las instrucciones **using** proporcionadas en la clase global.asax.cs.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.cs)]
5. Agregue la siguiente línea de código en el método `Application_Start` de la clase global para registrar la ruta predeterminada de Signalr.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    El archivo global. asax debería tener un aspecto similar al siguiente:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.cs)]
6. A continuación, agregaremos el cliente. En **Explorador de soluciones**, haga clic con el botón secundario en el proyecto y, a continuación, haga clic en **Agregar | Nuevo elemento**. En el cuadro de diálogo **Agregar nuevo elemento** , seleccione **página html**. Asigne un nombre adecuado a la página (como **default. html**) y haga clic en **Agregar**.
7. En **Explorador de soluciones**, haga clic con el botón secundario en la página que acaba de crear y haga clic en **establecer como página de inicio**.
8. Reemplace el código predeterminado de la página HTML por el siguiente fragmento de código.

    > [!NOTE]
    > Compruebe que las siguientes referencias de script coinciden con los paquetes agregados al proyecto en la carpeta scripts. En Visual Studio 2010, la versión de JQuery y Signalr agregada al proyecto puede no coincidir con los números de versión siguientes.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample7.html)]

    En el código HTML y JavaScript anterior se crea un div rojo denominado Shape, se habilita el comportamiento de arrastre de la forma mediante la biblioteca jQuery y se usa el evento `drag` de la forma para enviar la posición de la forma al servidor.
9. Presione F5 para iniciar la aplicación. Copie la dirección URL de la página y péguela en una segunda ventana del explorador. Arrastre la forma en una de las ventanas del explorador; la forma de la otra ventana del explorador debe moverse.

    ![Ventana de la aplicación](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>Agregar el bucle de cliente

Puesto que el envío de la ubicación de la forma en cada evento de movimiento del mouse creará una cantidad innecesaria de tráfico de red, es necesario limitar los mensajes del cliente. Usaremos la función `setInterval` de JavaScript para configurar un bucle que envíe la nueva información de posición al servidor a una velocidad fija. Este bucle es una representación muy básica de un "bucle de juego", una función llamada repetidamente que impulsa toda la funcionalidad de un juego u otra simulación.

1. Actualice el código de cliente en la página HTML para que coincida con el siguiente fragmento de código.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample8.html)]

    La actualización anterior agrega la función `updateServerModel`, a la que se llama en una frecuencia fija. Esta función envía los datos de la posición al servidor cada vez que la marca `moved` indica que hay nuevos datos de la posición que se van a enviar.
2. Presione F5 para iniciar la aplicación. Copie la dirección URL de la página y péguela en una segunda ventana del explorador. Arrastre la forma en una de las ventanas del explorador; la forma de la otra ventana del explorador debe moverse. Dado que se limitará el número de mensajes que se envían al servidor, la animación no aparecerá tan suave como en la sección anterior.

    ![Ventana de la aplicación](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>Agregar el bucle de servidor

En la aplicación actual, los mensajes enviados desde el servidor al cliente salen con la frecuencia a la que se reciben. Esto presenta un problema similar al que se ha detectado en el cliente; los mensajes se pueden enviar con más frecuencia de la que se necesitan y la conexión podría verse inundada como resultado. En esta sección se describe cómo actualizar el servidor para implementar un temporizador que limite la velocidad de los mensajes salientes.

1. Reemplace el contenido de `MoveShapeHub.cs` por el siguiente fragmento de código.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample9.cs)]

    El código anterior expande el cliente para agregar la clase `Broadcaster`, que limita los mensajes salientes mediante la clase `Timer` de .NET Framework.

    Dado que el propio concentrador es transitorio (se crea cada vez que se necesita), el `Broadcaster` se creará como un singleton. La inicialización diferida (introducida en .NET 4) se usa para diferir la creación hasta que sea necesaria, asegurándose de que la primera instancia del concentrador se cree completamente antes de que se inicie el temporizador.

    La llamada a la función de `UpdateShape` de los clientes se mueve fuera del método de `UpdateModel` del centro, de modo que ya no se llama inmediatamente cuando se reciben los mensajes entrantes. En su lugar, los mensajes a los clientes se enviarán a una velocidad de 25 llamadas por segundo, administradas por el temporizador de `_broadcastLoop` desde dentro de la clase `Broadcaster`.

    Por último, en lugar de llamar directamente al método de cliente desde el concentrador, la clase `Broadcaster` debe obtener una referencia al centro operativo actualmente (`_hubContext`) mediante el `GlobalHost`.
2. Presione F5 para iniciar la aplicación. Copie la dirección URL de la página y péguela en una segunda ventana del explorador. Arrastre la forma en una de las ventanas del explorador; la forma de la otra ventana del explorador debe moverse. No habrá ninguna diferencia visible en el explorador de la sección anterior, pero se limitará el número de mensajes que se envían al cliente.

    ![Ventana de la aplicación](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>Agregar la animación Smooth en el cliente

La aplicación está casi completa, pero podríamos mejorarla en el movimiento de la forma en el cliente, ya que se mueve en respuesta a los mensajes del servidor. En lugar de establecer la posición de la forma en la nueva ubicación proporcionada por el servidor, usaremos la función `animate` de la biblioteca de la interfaz de usuario de JQuery para mover la forma suavemente entre su posición actual y la nueva.

1. Actualice el método de `updateShape` del cliente para que se parezca al código resaltado siguiente:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample10.html?highlight=35-42)]

    En el código anterior se mueve la forma de la ubicación anterior al nuevo proporcionado por el servidor durante el transcurso del intervalo de animación (en este caso, 100 milisegundos). Cualquier animación anterior que se ejecute en la forma se borra antes de que se inicie la nueva animación.
2. Presione F5 para iniciar la aplicación. Copie la dirección URL de la página y péguela en una segunda ventana del explorador. Arrastre la forma en una de las ventanas del explorador; la forma de la otra ventana del explorador debe moverse. El movimiento de la forma en la otra ventana debe aparecer menos entrecortado, ya que su movimiento se interpola a lo largo del tiempo, en lugar de establecerse una vez por cada mensaje entrante.

    ![Ventana de la aplicación](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>Pasos adicionales

En este tutorial, ha aprendido a programar una aplicación Signalr que envía mensajes de alta frecuencia entre clientes y servidores. Este paradigma de comunicación es útil para desarrollar juegos en línea y otras simulaciones, como [el juego del captador creado con signalr](http://shootr.signalr.net).

La aplicación completa creada en este tutorial se puede descargar desde la [Galería de código](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Para obtener más información sobre los conceptos de desarrollo de Signalr, visite los siguientes sitios para ver el código fuente y los recursos de Signalr:

- [Proyecto signalr](http://signalr.net)
- [Github y ejemplos de signalr](https://github.com/SignalR/SignalR)
- [Wiki de signalr](https://github.com/SignalR/SignalR/wiki)
