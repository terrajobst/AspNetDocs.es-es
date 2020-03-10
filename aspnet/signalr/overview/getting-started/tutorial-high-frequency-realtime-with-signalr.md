---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Tutorial: creación de una aplicación en tiempo real de alta frecuencia con Signalr 2 | Microsoft Docs'
author: bradygaster
description: En este tutorial se muestra cómo crear una aplicación web que usa ASP.NET Signalr para proporcionar la funcionalidad de mensajería de alta frecuencia.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 2503e90735d6cfa445ee08c9e43f8443aa106096
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78450121"
---
# <a name="tutorial-create-high-frequency-real-time-app-with-signalr-2"></a>Tutorial: creación de una aplicación en tiempo real de alta frecuencia con Signalr 2

En este tutorial se muestra cómo crear una aplicación web que usa ASP.NET Signalr 2 para proporcionar la funcionalidad de mensajería de alta frecuencia. En este caso, "mensajería de frecuencia alta" significa que el servidor envía actualizaciones a una velocidad fija. Puede enviar hasta 10 mensajes por segundo.

La aplicación que cree muestra una forma que los usuarios pueden arrastrar. El servidor actualiza la posición de la forma en todos los exploradores conectados para que coincida con la posición de la forma arrastrada mediante actualizaciones con tiempo.

Los conceptos presentados en este tutorial tienen aplicaciones en juegos en tiempo real y otras aplicaciones de simulación.

En este tutorial va a:

> [!div class="checklist"]
> * Configuración del proyecto
> * Crear la aplicación base
> * Asignar al centro cuando se inicia la aplicación
> * Agregar el cliente
> * Ejecutar la aplicación
> * Agregar el bucle de cliente
> * Agregar el bucle de servidor
> * Agregar animaciones suaves

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Requisitos previos

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con la carga de trabajo de **ASP.NET y desarrollo web**.

## <a name="set-up-the-project"></a>Configuración del proyecto

En esta sección, creará el proyecto en Visual Studio 2017.

En esta sección se muestra cómo usar Visual Studio 2017 para crear una aplicación Web de ASP.NET vacía y agregar las bibliotecas Signalr y jQuery. UI.

1. En Visual Studio, cree una aplicación Web ASP.NET.

    ![Crear Web](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

1. En la ventana **New ASP.NET Web Application-MoveShapeDemo** , deje **vacío** seleccionado y haga clic en **Aceptar**.

1. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **nuevo elemento**.

1. En **Agregar nuevo elemento-MoveShapeDemo**, seleccione **instalado** > **Visual C#**  > **Web** > **signalr** y, a continuación, seleccione **clase de concentrador de signalr (V2)** .

1. Asigne a la clase el nombre *MoveShapeHub* y agréguelo al proyecto.

    En este paso se crea el archivo de clase *MoveShapeHub.CS* . Simultáneamente, agrega un conjunto de archivos de script y referencias de ensamblado que admiten Signalr en el proyecto.

1. Seleccione **Herramientas** > **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.

1. En la **consola del administrador de paquetes**, ejecute este comando:

    ```console
    Install-Package jQuery.UI.Combined
    ```

    El comando instala la biblioteca de jQuery UI. Se usa para animar la forma.

1. En **Explorador de soluciones**, expanda el nodo scripts.

    ![Referencias de la biblioteca de scripts](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

    Las bibliotecas de scripts para jQuery, jQueryUI y Signalr están visibles en el proyecto.

## <a name="create-the-base-application"></a>Crear la aplicación base

En esta sección, creará una aplicación de explorador. La aplicación envía la ubicación de la forma al servidor durante cada evento de movimiento del mouse. El servidor difunde esta información a todos los demás clientes conectados en tiempo real. Puede obtener más información sobre esta aplicación en secciones posteriores.

1. Abra el archivo *MoveShapeHub.CS* .

1. Reemplace el código del archivo *MoveShapeHub.CS* por este código:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

1. Guarde el archivo.

La clase `MoveShapeHub` es una implementación de un concentrador Signalr. Como en el tutorial [Introducción con signalr](tutorial-getting-started-with-signalr.md) , el concentrador tiene un método al que los clientes llaman directamente. En este caso, el cliente envía un objeto con las nuevas coordenadas X e y de la forma al servidor. Esas coordenadas se difunden a todos los demás clientes conectados. Signalr serializa automáticamente este objeto mediante JSON.

La aplicación envía el objeto de `ShapeModel` al cliente. Tiene miembros para almacenar la posición de la forma. La versión del objeto en el servidor también tiene un miembro para realizar el seguimiento de los datos del cliente que se están almacenando. Este objeto evita que el servidor envíe de nuevo los datos de un cliente a sí mismo. Este miembro usa el atributo `JsonIgnore` para impedir que la aplicación serialice los datos y los envíe de vuelta al cliente.

## <a name="map-to-the-hub-when-app-starts"></a>Asignar al centro cuando se inicia la aplicación

A continuación, configure la asignación al concentrador cuando se inicie la aplicación. En Signalr 2, la adición de una clase de inicio OWIN crea la asignación.

1. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **nuevo elemento**.

1. En **Agregar nuevo elemento-MoveShapeDemo** , seleccione **instalado** > **Visual C#**  > **Web** y, a continuación, seleccione **clase de inicio OWIN**.

1. Asigne a la clase el nombre *Startup* y seleccione **Aceptar**.

1. Reemplace el código predeterminado del archivo *Startup.CS* por este código:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

La clase de inicio OWIN llama a `MapSignalR` cuando la aplicación ejecuta el método `Configuration`. La aplicación agrega la clase al proceso de inicio de OWIN mediante el `OwinStartup` atributo de ensamblado.

## <a name="add-the-client"></a>Agregar el cliente

Agregue la página HTML para el cliente.

1. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **página HTML**.

1. Asigne un nombre **predeterminado** a la página y seleccione **Aceptar**.

1. En **Explorador de soluciones**, haga clic con el botón secundario en *default. html* y seleccione **establecer como página de inicio**.

1. Reemplace el código predeterminado en el archivo *default. html* por este código:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html?highlight=14-16)]

1. En **Explorador de soluciones**, expanda **scripts**.

    Las bibliotecas de scripts para jQuery y Signalr están visibles en el proyecto.

    > [!IMPORTANT]
    > El administrador de paquetes instala una versión posterior de los scripts de Signalr.

1. Actualice las referencias de script en el bloque de código para que correspondan a las versiones de los archivos de script del proyecto.

Este código HTML y JavaScript crea un `div` rojo denominado `shape`. Habilita el comportamiento de arrastre de la forma mediante la biblioteca de jQuery y usa el evento `drag` para enviar la posición de la forma al servidor.

## <a name="run-the-app"></a>Ejecutar la aplicación

Puede ejecutar la aplicación para se'e que funcione. Al arrastrar la forma alrededor de una ventana del explorador, la forma también se mueve en los otros exploradores.

1. En la barra de herramientas, active la **depuración de script** y, a continuación, seleccione el botón reproducir para ejecutar la aplicación en modo de depuración.

    ![Captura de pantalla del usuario que activa el modo de depuración y selecciona reproducir.](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)

    Se abre una ventana del explorador con la forma roja en la esquina superior derecha.

1. Copie la dirección URL de la página.

1. Abra otro explorador y pegue la dirección URL en la barra de direcciones.

1. Arrastre la forma en una de las ventanas del explorador. A continuación se muestra la forma en la otra ventana del explorador.

Mientras que la aplicación funciona con este método, no es un modelo de programación recomendado. No hay ningún límite superior para el número de mensajes que se envían. Como resultado, los clientes y el servidor se sobrecargan con los mensajes y la degradación del rendimiento. Además, la aplicación muestra una animación separada en el cliente. Esta animación irregular se produce porque la forma se mueve al instante por cada método. Es mejor si la forma se mueve sin problemas a cada nueva ubicación. A continuación, aprenderá a corregir esos problemas.

## <a name="add-the-client-loop"></a>Agregar el bucle de cliente

El envío de la ubicación de la forma en cada evento de movimiento del mouse crea una cantidad innecesaria de tráfico de red. La aplicación debe limitar los mensajes del cliente.

Utilice la función `setInterval` de JavaScript para configurar un bucle que envíe la nueva información de posición al servidor a una velocidad fija. Este bucle es una representación básica de un "bucle de juego". Se trata de una función llamada repetidamente que impulsa toda la funcionalidad de un juego.

1. Reemplace el código de cliente en el archivo *default. html* por este código:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html?highlight=14-16)]

    > [!IMPORTANT]
    > Tendrá que reemplazar las referencias de script de nuevo. Deben coincidir con las versiones de los scripts del proyecto.

    Este nuevo código agrega la función `updateServerModel`. Se llama en una frecuencia fija. La función envía los datos de la posición al servidor cada vez que la marca `moved` indica que hay nuevos datos de posición que se van a enviar.

1. Seleccione el botón reproducir para iniciar la aplicación

1. Copie la dirección URL de la página.

1. Abra otro explorador y pegue la dirección URL en la barra de direcciones.

1. Arrastre la forma en una de las ventanas del explorador. A continuación se muestra la forma en la otra ventana del explorador.

Dado que la aplicación limita el número de mensajes que se envían al servidor, la animación no aparecerá como Smooth en primer lugar.

## <a name="add-the-server-loop"></a>Agregar el bucle de servidor

En la aplicación actual, los mensajes enviados desde el servidor al cliente salen con tanta frecuencia como se reciben. Este tráfico de red presenta un problema similar al que vemos en el cliente.

La aplicación puede enviar mensajes con más frecuencia de lo que son necesarios. La conexión se puede saturar como resultado. En esta sección se describe cómo actualizar el servidor para agregar un temporizador que limite la velocidad de los mensajes salientes.

1. Reemplace el contenido de `MoveShapeHub.cs` por este código:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

1. Seleccione el botón reproducir para iniciar la aplicación.

1. Copie la dirección URL de la página.

1. Abra otro explorador y pegue la dirección URL en la barra de direcciones.

1. Arrastre la forma en una de las ventanas del explorador.

Este código expande el cliente para agregar la clase `Broadcaster`. La nueva clase limita los mensajes salientes mediante la `Timer` clase de .NET Framework.

Es conveniente saber que el propio concentrador es transitorio. Se crea cada vez que se necesita. Por lo tanto, la aplicación crea el `Broadcaster` como un singleton. Utiliza la inicialización diferida para aplazar la creación del `Broadcaster`hasta que sea necesario. Esto garantiza que la aplicación crea la primera instancia de concentrador completamente antes de iniciar el temporizador.

La llamada a la función de `UpdateShape` de los clientes se mueve fuera del método de `UpdateModel` del centro. Ya no se llama inmediatamente cuando la aplicación recibe los mensajes entrantes. En su lugar, la aplicación envía los mensajes a los clientes a una velocidad de 25 llamadas por segundo. El proceso se administra mediante el temporizador de `_broadcastLoop` desde dentro de la clase `Broadcaster`.

Por último, en lugar de llamar directamente al método de cliente desde el concentrador, la clase `Broadcaster` debe obtener una referencia al concentrador de `_hubContext` operativo actualmente. Obtiene la referencia con el `GlobalHost`.

## <a name="add-smooth-animation"></a>Agregar animaciones suaves

La aplicación está casi finalizada, pero podríamos mejorarla. La aplicación mueve la forma en el cliente en respuesta a los mensajes del servidor. En lugar de establecer la posición de la forma en la nueva ubicación proporcionada por el servidor, use la función `animate` de la biblioteca de interfaz de usuario de JQuery. Puede mover la forma suavemente entre su posición actual y la nueva.

1. Actualice el método de `updateShape` del cliente en el archivo *default. html* para que tenga un aspecto similar al código resaltado:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

1. Seleccione el botón reproducir para iniciar la aplicación.

1. Copie la dirección URL de la página.

1. Abra otro explorador y pegue la dirección URL en la barra de direcciones.

1. Arrastre la forma en una de las ventanas del explorador.

El movimiento de la forma en la otra ventana aparece menos entrecortado. La aplicación interpola su movimiento a lo largo del tiempo, en lugar de establecerse una vez por cada mensaje entrante.

Este código mueve la forma de la ubicación anterior al nuevo. El servidor proporciona la posición de la forma en el curso del intervalo de animación. En este caso, eso es 100 milisegundos. La aplicación borra cualquier animación anterior que se ejecute en la forma antes de que se inicie la nueva animación.

## <a name="get-the-code"></a>Obtención del código

[Descargar proyecto completado](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

## <a name="additional-resources"></a>Recursos adicionales

El paradigma de comunicación que acaba de aprender es útil para desarrollar juegos en línea y otras simulaciones, como [el juego del captador creado con signalr](https://shootr.azurewebsites.net/).

Para obtener más información acerca de Signalr, consulte los siguientes recursos:

* [Proyecto signalr](http://signalr.net)

* [GitHub y ejemplos de signalr](https://github.com/SignalR/SignalR)

* [Wiki de signalr](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial va a:

> [!div class="checklist"]
> * Configuración del proyecto
> * Creación de la aplicación base
> * Asignado al centro cuando se inicia la aplicación
> * Agregado el cliente
> * Ejecución de la aplicación
> * Agregado el bucle de cliente
> * Se agregó el bucle de servidor
> * Animación suave agregada

Avance al siguiente artículo para aprender a crear una aplicación web que usa ASP.NET Signalr 2 para proporcionar la funcionalidad de difusión de servidor.
> [!div class="nextstepaction"]
> [Signalr 2 y difusión de servidor](tutorial-server-broadcast-with-signalr.md)