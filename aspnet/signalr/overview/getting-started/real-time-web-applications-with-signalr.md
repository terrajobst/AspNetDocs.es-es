---
uid: signalr/overview/getting-started/real-time-web-applications-with-signalr
title: 'Laboratorio práctico: aplicaciones web en tiempo real con Signalr | Microsoft Docs'
author: bradygaster
description: Las aplicaciones web en tiempo real tienen la capacidad de insertar contenido del lado servidor en los clientes conectados, en tiempo real. Para desarrolladores de ASP.NET, ASP...
ms.author: bradyg
ms.date: 07/16/2014
ms.assetid: ba07958c-42e1-4da0-81db-ba6925ed6db0
msc.legacyurl: /signalr/overview/getting-started/real-time-web-applications-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 9e39fd3f2fc9d4e791002450085215096c222fcd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431671"
---
# <a name="hands-on-lab-real-time-web-applications-with-signalr"></a>Laboratorio práctico: aplicaciones web en tiempo real con Signalr

por [equipo de grupos de web](https://twitter.com/webcamps)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[Descargar el kit de aprendizaje de Web., versión de octubre de 2015](https://github.com/Microsoft-Web/WebCampTrainingKit/releases/tag/v2015.10.13b)

> Las aplicaciones web en tiempo real tienen la capacidad de insertar contenido del lado servidor en los clientes conectados, en tiempo real. Para desarrolladores de ASP.NET, **ASP.net signalr** es una biblioteca para agregar funcionalidad web en tiempo real a sus aplicaciones. Aprovecha varios transportes, seleccionando automáticamente el mejor transporte disponible en función del cliente y el mejor transporte disponible del servidor. Aprovecha **WebSocket**, una API de HTML5 que permite la comunicación bidireccional entre el explorador y el servidor.
> 
> **Signalr** también proporciona una API sencilla y de alto nivel para realizar la RPC de servidor a cliente (llamar a funciones de JavaScript en los exploradores de los clientes desde el código .net del lado servidor) en la aplicación de ASP.net, así como agregar enlaces útiles para la administración de conexiones, como eventos de conexión/desconexión, agrupación de conexiones y autorización.
> 
> **Signalr** es una abstracción en algunos de los transportes necesarios para realizar el trabajo en tiempo real entre el cliente y el servidor. Una conexión **signalr** se inicia como http y, después, se promueve a una conexión **WebSocket** , si está disponible. **WebSocket** es el transporte ideal para **signalr**, ya que hace el uso más eficaz de la memoria del servidor, tiene la latencia más baja y tiene las características más subyacentes (como la comunicación dúplex completa entre el cliente y el servidor), pero también tiene los requisitos más estrictos: **WebSocket** requiere que el servidor use **Windows Server 2012** o **Windows 8**, junto con **.NET Framework 4,5**. Si no se cumplen estos requisitos, **signalr** intentará usar otros transportes para realizar sus conexiones (como el *sondeo prolongado de Ajax*).
> 
> **Signalr** API contiene dos modelos para la comunicación entre clientes y servidores: conexiones y **concentradores** **persistentes** . Una **conexión** representa un extremo simple para el envío de mensajes de un solo destinatario, agrupados o de difusión. Un **concentrador** es una canalización de alto nivel que se basa en la API de conexión que permite que el cliente y el servidor llamen a métodos entre sí directamente.
> 
> ![Arquitectura de signalr](real-time-web-applications-with-signalr/_static/image1.png)
> 
> Todo el código de ejemplo y los fragmentos de código se incluyen en el kit de aprendizaje de Web de la versión 2015 de octubre, disponible en [https://github.com/Microsoft-Web/WebCampTrainingKit/releases/tag/v2015.10.13b](https://github.com/Microsoft-Web/WebCampTrainingKit/releases/tag/v2015.10.13b).  Tenga en cuenta que el vínculo del instalador de esa página ya no funciona; en su lugar, use uno de los vínculos de la sección activos.

<a id="Overview"></a>
## <a name="overview"></a>Información general

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá a:

- Enviar notificaciones del servidor al cliente mediante Signalr.
- Escalabilidad horizontal la aplicación Signalr mediante **SQL Server**.

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

Para completar este laboratorio práctico, es necesario lo siguiente:

- [Visual Studio Express 2013 para web](https://www.microsoft.com/visualstudio/) o superior

<a id="Setup"></a>
### <a name="setup"></a>Programa de instalación

Con el fin de ejecutar los ejercicios de este laboratorio práctico, deberá configurar el entorno primero.

1. Abra una ventana del explorador de Windows y vaya a la carpeta de **origen** del laboratorio.
2. Haga clic con el botón secundario en **setup. cmd** y seleccione **Ejecutar como administrador** para iniciar el proceso de instalación que configurará el entorno e instalará los fragmentos de código de Visual Studio para este laboratorio.
3. Si se muestra el cuadro de diálogo control de cuentas de usuario, confirme la acción para continuar.

> [!NOTE]
> Asegúrese de que ha comprobado todas las dependencias de este laboratorio antes de ejecutar el programa de instalación.

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Usar los fragmentos de código

En todo el documento de laboratorio, se le indicará que inserte bloques de código. Para su comodidad, la mayor parte de este código se proporciona como fragmentos de Visual Studio Code, a los que puede acceder desde Visual Studio 2013 para evitar tener que agregarlo manualmente.

> [!NOTE]
> Cada ejercicio va acompañado de una solución de inicio que se encuentra en la carpeta **Inicio** del ejercicio que le permite seguir cada ejercicio con independencia de los demás. Tenga en cuenta que los fragmentos de código que se agregan durante un ejercicio no se encuentran en estas soluciones de inicio y puede que no funcionen hasta que haya completado el ejercicio. Dentro del código fuente de un ejercicio, también encontrará una carpeta **final** que contiene una solución de Visual Studio con el código que es el resultado de completar los pasos del ejercicio correspondiente. Puede usar estas soluciones como guía si necesita ayuda adicional a medida que trabaja en este laboratorio práctico.

---

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Este laboratorio práctico incluye los siguientes ejercicios:

1. [Trabajar con datos en tiempo real mediante Signalr](#Exercise1)
2. [Escalado horizontal con SQL Server](#Exercise2)

Tiempo estimado para completar este laboratorio: **60 minutos**

> [!NOTE]
> Al iniciar Visual Studio por primera vez, debe seleccionar una de las colecciones de configuraciones predefinidas. Cada colección predefinida está diseñada para coincidir con un estilo de desarrollo determinado y determina los diseños de ventana, el comportamiento del editor, los fragmentos de código de IntelliSense y las opciones del cuadro de diálogo. En los procedimientos de este laboratorio se describen las acciones necesarias para realizar una tarea determinada en Visual Studio cuando se usa la colección de **configuración de desarrollo general** . Si elige una colección de valores de configuración diferente para el entorno de desarrollo, puede haber diferencias en los pasos que debe tener en cuenta.

<a id="Exercise1"></a>
### <a name="exercise-1-working-with-real-time-data-using-signalr"></a>Ejercicio 1: trabajar con datos en tiempo real mediante Signalr

Aunque el chat se usa a menudo como ejemplo, puede hacer mucho más con la funcionalidad web en tiempo real. Cada vez que un usuario actualiza una página web para ver los nuevos datos o la página implementa el sondeo de Ajax Long para recuperar los datos nuevos, puede usar Signalr.

Signalr admite la funcionalidad de instalación o **difusión** del **servidor** ; controla la administración de conexiones automáticamente. En las conexiones HTTP clásicas para la comunicación cliente-servidor, se restablece la conexión para cada solicitud, pero Signalr proporciona una conexión persistente entre el cliente y el servidor. En Signalr, el código de servidor llama a un código de cliente en el explorador mediante llamadas a procedimiento remoto (RPC), en lugar del modelo de solicitud-respuesta que conocemos hoy en día.

En este ejercicio, configurará la aplicación de **prueba** de la que se usa signalr para mostrar el panel de estadísticas con las métricas actualizadas sin necesidad de actualizar toda la página.

<a id="Ex1Task1"></a>
#### <a name="task-1--exploring-the-geek-quiz-statistics-page"></a>Tarea 1: explorar la página de estadísticas de las pruebas de los fanáticos

En esta tarea, irá a través de la aplicación y comprobará cómo se muestra la página estadísticas y cómo puede mejorar la forma en que se actualiza la información.

1. Abra **Visual Studio Express 2013 para web** y abra la solución **GeekQuiz. sln** que se encuentra en la carpeta **Source\Ex1-WorkingWithRealTimeData\Begin** .
2. Presione **F5** para ejecutar la solución. La página de **Inicio de sesión** debe aparecer en el explorador.

    ![Ejecutar la solución](real-time-web-applications-with-signalr/_static/image2.png "Ejecutar la solución")

    *Ejecutar la solución*
3. Haga clic en **registrar** en la esquina superior derecha de la página para crear un nuevo usuario en la aplicación.

    ![Vínculo de registro](real-time-web-applications-with-signalr/_static/image3.png "Vínculo de registro")

    *Vínculo de registro*
4. En la página **registrar** , escriba un **nombre de usuario** y una **contraseña**y, a continuación, haga clic en **registrar**.

    ![Registrar un usuario](real-time-web-applications-with-signalr/_static/image4.png "Registrar un usuario")

    *Registrar un usuario*
5. La aplicación registra la nueva cuenta y el usuario se autentica y redirige de nuevo a la Página principal que muestra la primera pregunta de la prueba.
6. Abra la página de **estadísticas** en una nueva ventana y coloque la página **principal** y la página de **estadísticas** en paralelo.

    ![Ventanas en paralelo](real-time-web-applications-with-signalr/_static/image5.png "Ventanas en paralelo")

    *Ventanas en paralelo*
7. En la página **principal** , haga clic en una de las opciones para responder a la pregunta.

    ![Responder a una pregunta](real-time-web-applications-with-signalr/_static/image6.png "Responder a una pregunta")

    *Responder a una pregunta*
8. Después de hacer clic en uno de los botones, debe aparecer la respuesta.

    ![Pregunta contestada correctamente](real-time-web-applications-with-signalr/_static/image7.png "Pregunta contestada correctamente")

    *Pregunta respondida correctamente*
9. Tenga en cuenta que la información proporcionada en la página estadísticas está obsoleta. Actualice la página para ver los resultados actualizados.

    ![Página estadísticas](real-time-web-applications-with-signalr/_static/image8.png "Página estadísticas")

    *Página estadísticas*
10. Vuelva a Visual Studio y detenga la depuración.

<a id="Ex1Task2"></a>
#### <a name="task-2--adding-signalr-to-geek-quiz-to-show-online-charts"></a>Tarea 2: adición de Signalr a una prueba de experto para mostrar gráficos en línea

En esta tarea, agregará Signalr a la solución y enviará actualizaciones a los clientes automáticamente cuando se envíe una nueva respuesta al servidor.

1. En el menú **herramientas** de Visual Studio, seleccione **Administrador de paquetes NuGet**y, a continuación, haga clic en **consola del administrador de paquetes**.
2. En la ventana de la **consola del administrador de paquetes** , ejecute el siguiente comando:

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample1.ps1)]

    ![Instalación del paquete de signalr](real-time-web-applications-with-signalr/_static/image9.png "Instalación del paquete de signalr")

    *Instalación del paquete de signalr*

   > [!NOTE]
   > Al instalar los paquetes NuGet de **signalr** versión 2.0.2 desde una nueva aplicación MVC 5, deberá actualizar manualmente los paquetes **OWIN** a la versión 2.0.1 (o superior) antes de instalar signalr. Para ello, puede ejecutar el siguiente script en la consola del **Administrador de paquetes**:
   > 
   > [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample2.ps1)]
   > 
   > En una versión futura de Signalr, las dependencias OWIN se actualizarán automáticamente.
3. En **Explorador de soluciones**, expanda la carpeta **scripts** y observe que los archivos de signalr *JS* se agregaron a la solución.

    ![Referencias de JavaScript de signalr](real-time-web-applications-with-signalr/_static/image10.png "Referencias de JavaScript de signalr")

    *Referencias de JavaScript de signalr*
4. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **GeekQuiz** , seleccione **Agregar** | **nueva carpeta**y asígnele el nombre **hubs**.
5. Haga clic con el botón derecho en la carpeta **hubs** y seleccione **Agregar | Nuevo elemento**.

    ![Agregar nuevo elemento](real-time-web-applications-with-signalr/_static/image11.png "Agregar nuevo elemento")

    *Agregar nuevo elemento*
6. En el cuadro de diálogo **Agregar nuevo elemento** , seleccione **el C# objeto visual | Web | Nodo signalr** en el panel izquierdo, seleccione **clase de concentrador de signalr (V2)** en el panel central, asigne al archivo el nombre **StatisticsHub.CS** y haga clic en **Agregar**.

    ![Cuadro de diálogo Agregar nuevo elemento](real-time-web-applications-with-signalr/_static/image12.png "Cuadro de diálogo Agregar nuevo elemento")

    *Cuadro de diálogo Agregar nuevo elemento*
7. Reemplace el código de la clase **StatisticsHub** por el código siguiente.

    (Fragmento de código- *RealTimeSignalR-EX1-StatisticsHubClass*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample3.cs)]
8. Abra **Startup.CS** y agregue la siguiente línea al final del método de **configuración** .

    (Fragmento de código- *RealTimeSignalR-EX1-MapSignalR*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample4.cs)]
9. Abra la página **StatisticsService.CS** dentro de la carpeta **servicios** y agregue las siguientes directivas using.

    (Fragmento de código- *RealTimeSignalR-EX1-UsingDirectives*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample5.cs)]
10. Para notificar a los clientes conectados de las actualizaciones, primero debe recuperar un objeto de **contexto** para la conexión actual. El objeto **Hub** contiene métodos para enviar mensajes a un solo cliente o difusión a todos los clientes conectados. Agregue el método siguiente a la clase **StatisticsService** para difundir los datos de las estadísticas.

    (Fragmento de código- *RealTimeSignalR-EX1-NotifyUpdatesMethod*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample6.cs)]

    > [!NOTE]
    > En el código anterior, se usa un nombre de método arbitrario para llamar a una función en el cliente (es decir: *updateStatistics*). El nombre de método que especifique se interpreta como un objeto dinámico, lo que significa que no hay ninguna validación de IntelliSense o de tiempo de compilación para él. La expresión se evalúa en tiempo de ejecución. Cuando se ejecuta la llamada al método, Signalr envía el nombre del método y los valores de parámetro al cliente. Si el cliente tiene un método que coincide con el nombre, se llama a ese método y se le pasan los valores de parámetro. Si no se encuentra ningún método coincidente en el cliente, no se produce ningún error. Para obtener más información, consulte la guía de la [API de ASP.net signalr hubs](../guide-to-the-api/hubs-api-guide-server.md).
11. Abra la página **TriviaController.CS** dentro de la carpeta **Controllers** y agregue las siguientes directivas using.

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample7.cs)]
12. Agregue el siguiente código resaltado al método de acción **post** .

    (Fragmento de código- *RealTimeSignalR-EX1-NotifyUpdatesCall*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample8.cs)]
13. Abra la página **Statistics. cshtml** dentro de las **vistas | Carpeta particular** . Busque la sección **scripts** y agregue las siguientes referencias de script al principio de la sección.

    (Fragmento de código- *RealTimeSignalR-EX1-SignalRScriptReferences*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample9.cshtml)]

    > [!NOTE]
    > Al agregar Signalr y otras bibliotecas de scripts al proyecto de Visual Studio, el administrador de paquetes puede instalar una versión del archivo de script de Signalr más reciente que la versión que se muestra en este tema. Asegúrese de que la referencia de script en el código coincide con la versión de la biblioteca de scripts instalada en el proyecto.
14. Agregue el siguiente código resaltado para conectar el cliente al concentrador de Signalr y actualizar los datos de estadísticas cuando se recibe un mensaje nuevo desde el centro.

    (Fragmento de código- *RealTimeSignalR-EX1-SignalRClientCode*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample10.cshtml)]

    En este código, se crea un proxy de concentrador y se registra un controlador de eventos para escuchar los mensajes enviados por el servidor. En este caso, escucha los mensajes enviados a través del método *updateStatistics* .

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Tarea 3: ejecución de la solución

En esta tarea, ejecutará la solución para comprobar que la vista de estadísticas se actualiza automáticamente mediante Signalr después de responder a una nueva pregunta.

1. Presione **F5** para ejecutar la solución.

    > [!NOTE]
    > Si aún no ha iniciado sesión en la aplicación, inicie sesión con el usuario que creó en la tarea 1.
2. Abra la página **estadísticas** en una nueva ventana y coloque la página **principal** y la página de **estadísticas** en paralelo como hizo en la tarea 1.
3. En la página **principal** , haga clic en una de las opciones para responder a la pregunta.

    ![Responder a otra pregunta](real-time-web-applications-with-signalr/_static/image13.png "Responder a otra pregunta")

    *Responder a otra pregunta*
4. Después de hacer clic en uno de los botones, debe aparecer la respuesta. Tenga en cuenta que la información de estadísticas de la página se actualiza automáticamente después de responder a la pregunta con la información actualizada sin necesidad de actualizar toda la página.

    ![Página de estadísticas actualizada después de la respuesta](real-time-web-applications-with-signalr/_static/image14.png "Página de estadísticas actualizada después de la respuesta")

    *Página de estadísticas actualizada después de la respuesta*

<a id="Exercise2"></a>
### <a name="exercise-2-scaling-out-using-sql-server"></a>Ejercicio 2: escalado horizontal con SQL Server

Al escalar una aplicación Web, normalmente puede elegir entre las opciones *de escalado* vertical y *escalado* horizontal. *Escalar* verticalmente significa usar un servidor mayor, con más recursos (CPU, RAM, etc.), mientras que el *escalado horizontal* significa agregar más servidores para controlar la carga. El problema con este último es que los clientes se pueden enrutar a diferentes servidores. Un cliente que está conectado a un servidor no recibirá los mensajes enviados desde otro servidor.

Puede resolver estos problemas mediante un componente denominado *backplane*, para reenviar mensajes entre servidores. Con un backplane habilitado, cada instancia de aplicación envía mensajes al plano posterior y el plano posterior los reenvía a las otras instancias de la aplicación.

Actualmente hay tres tipos de Backplanes para Signalr:

- **Azure Service Bus de Windows**. Service Bus es una infraestructura de mensajería que permite a los componentes enviar mensajes de acoplamiento flexible.
- **SQL Server**. El backplane SQL Server escribe mensajes en las tablas SQL. El backplane usa Service Broker para la mensajería eficaz. Sin embargo, también funciona si Service Broker no está habilitado.
- **Redis**. Redis es un almacén de clave-valor en memoria. Redis admite un patrón de publicación/suscripción ("pub/sub") para enviar mensajes.

Cada mensaje se envía a través de un bus de mensajes. Un bus de mensajes implementa la interfaz [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) , que proporciona una abstracción de publicación/suscripción. Los Backplanes funcionan reemplazando el valor predeterminado de **IMessageBus** por un bus diseñado para ese backplane.

Cada instancia de servidor se conecta al plano posterior a través del bus. Cuando se envía un mensaje, se dirige al backplane y el plano posterior lo envía a todos los servidores. Cuando un servidor recibe un mensaje del plano posterior, lo almacena en la memoria caché local. A continuación, el servidor entrega mensajes a los clientes desde su caché local.

Para obtener más información sobre cómo funciona el backplane Signalr, lea este [artículo](../performance/scaleout-in-signalr.md).

> [!NOTE]
> Hay algunos escenarios en los que un backplane puede convertirse en un cuello de botella. Estos son algunos escenarios típicos de Signalr:
> 
> - [Difusión del servidor](tutorial-server-broadcast-with-signalr.md) (por ejemplo, el tablero de cotizaciones): los Backplanes funcionan bien en este escenario, ya que el servidor controla la velocidad a la que se envían los mensajes.
> - [Cliente a cliente](tutorial-getting-started-with-signalr.md) (por ejemplo, chat): en este escenario, el backplane podría ser un cuello de botella si el número de mensajes se escala con el número de clientes; es decir, si la tasa de mensajes aumenta proporcionalmente a medida que se unen más clientes.
> - [Alta frecuencia en tiempo real](tutorial-high-frequency-realtime-with-signalr.md) (por ejemplo, juegos en tiempo real): no se recomienda un backplane para este escenario.

En este ejercicio, usará **SQL Server** para distribuir los mensajes a través de la aplicación de prueba de los **fanáticos** . Estas tareas se ejecutarán en una única máquina de pruebas para aprender a configurar la configuración, pero para obtener el efecto completo, deberá implementar la aplicación Signalr en dos o más servidores. También debe instalar SQL Server en uno de los servidores o en un servidor dedicado independiente.

![Escalabilidad horizontal mediante SQL Server diagrama](real-time-web-applications-with-signalr/_static/image15.png)

<a id="Ex2Task1"></a>
#### <a name="task-1---understanding-the-scenario"></a>Tarea 1: Descripción del escenario

En esta tarea, ejecutará dos instancias de la **prueba de experto** en la simulación de varias instancias de IIS en el equipo local. En este escenario, al responder a las preguntas Curiosidades en una aplicación, no se notificará a la actualización en la página estadísticas de la segunda instancia. Esta simulación es similar a un entorno en el que la aplicación se implementa en varias instancias y con un equilibrador de carga para comunicarse con ellas.

1. Abra la solución **Begin. sln** que se encuentra en la carpeta **source/Ex2-ScalingOutWithSQLServer/Begin** . Una vez cargados, observará en el **Explorador de servidores** que la solución tiene dos proyectos con estructuras idénticas pero nombres diferentes. Esto simulará la ejecución de dos instancias de la misma aplicación en el equipo local.

    ![Comenzar la solución simulando 2 instancias de una prueba de experto](real-time-web-applications-with-signalr/_static/image16.png "Comenzar la solución simulando 2 instancias de una prueba de experto")

    *Comenzar la solución simulando 2 instancias de una prueba de experto*
2. Abra la página de propiedades de la solución haciendo clic con el botón secundario en el nodo de la solución y seleccionando **propiedades**. En **proyecto de inicio**, seleccione **proyectos de inicio múltiples** y cambie el valor de la **acción** de ambos proyectos a *iniciar*.

    ![Iniciar varios proyectos](real-time-web-applications-with-signalr/_static/image17.png "Iniciar varios proyectos")

    *Iniciar varios proyectos*
3. Presione **F5** para ejecutar la solución. La aplicación iniciará dos instancias de una **prueba** integrada en diferentes puertos y simulará varias instancias de la misma aplicación. Ancle uno de los exploradores a la izquierda y el otro a la derecha de la pantalla. Inicie sesión con sus credenciales o registre un nuevo usuario. Una vez que haya iniciado sesión, mantenga la página de curiosidades a la izquierda y vaya a la página de **estadísticas** en el explorador de la derecha.

    ![Prueba de los fanáticos en paralelo](real-time-web-applications-with-signalr/_static/image18.png)

    *Prueba de los fanáticos en paralelo*

    ![Prueba integrada en distintos puertos](real-time-web-applications-with-signalr/_static/image19.png)

    *Prueba integrada en distintos puertos*
4. Empiece a responder a preguntas en el explorador izquierdo y observará que la página de **estadísticas** del explorador derecho no se está actualizando. Esto se debe a que **signalr** usa una memoria caché local para distribuir los mensajes entre sus clientes y este escenario está simulando varias instancias, por lo que la memoria caché no se comparte entre ellas. Puede comprobar que **signalr** funciona probando los mismos pasos, pero usando una sola aplicación. En las tareas siguientes, configurará un backplane para replicar los mensajes entre las instancias.
5. Vuelva a Visual Studio y detenga la depuración.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-sql-server-backplane"></a>Tarea 2: crear el backplane SQL Server

En esta tarea, creará una base de datos que servirá como un plano posterior para la aplicación de **prueba** de la que se trata. Usará **Explorador de objetos de SQL Server** para examinar el servidor e inicializar la base de datos. Además, habilitará el **Service Broker**.

1. En **Visual Studio**, abra la **vista** de menú y seleccione **Explorador de objetos de SQL Server**.
2. Conéctese a la instancia de LocalDB haciendo clic con el botón secundario en el nodo **SQL Server** y seleccionando la opción **Agregar SQL Server...** .

    ![Agregar una instancia de SQL Server](real-time-web-applications-with-signalr/_static/image20.png "Agregar una instancia de SQL Server")

    *Agregar una instancia de SQL Server a Explorador de objetos de SQL Server*
3. Establezca el **nombre del servidor** en *(LocalDB) \v11.0* y deje la **autenticación de Windows** como modo de autenticación. Haga clic en **Conectar** para continuar.

    ![Conectar con LocalDB](real-time-web-applications-with-signalr/_static/image21.png "Conectar con LocalDB")

    *Conectar con LocalDB*
4. Ahora que está conectado a la instancia de LocalDB, tendrá que crear una base de datos que represente el SQL Server backplane de Signalr. Para ello, haga clic con el botón secundario en el nodo **bases** de **datos y seleccione Agregar nueva base de datos**.

    ![Agregar una nueva base de datos](real-time-web-applications-with-signalr/_static/image22.png "Agregar una nueva base de datos")

    *Agregar una nueva base de datos*
5. Establezca el nombre de la base de datos en *signalr* y haga clic en **Aceptar** para crearlo.

    ![Crear la base de datos de Signalr](real-time-web-applications-with-signalr/_static/image23.png "Crear la base de datos de Signalr")

    *Crear la base de datos de Signalr*

    > [!NOTE]
    > Puede elegir cualquier nombre para la base de datos.
6. Para recibir actualizaciones de forma más eficaz desde el backplane, se recomienda habilitar Service Broker para la base de datos. Service Broker proporciona compatibilidad nativa para mensajería y puesta en cola en SQL Server. El backplane también funciona sin Service Broker. Abra una nueva consulta; para ello, haga clic con el botón secundario en la base de datos y seleccione **nueva consulta**.

    ![Abrir una nueva consulta](real-time-web-applications-with-signalr/_static/image24.png "Abrir una nueva consulta")

    *Abrir una nueva consulta*
7. Para comprobar si Service Broker está habilitado, consulte la columna **is\_broker\_Enabled** en la vista de catálogo **Sys. Databases** . Ejecute el siguiente script en la ventana de consulta abierta recientemente.

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample11.sql)]

    ![Consulta del estado de la Service Broker](real-time-web-applications-with-signalr/_static/image25.png "Consulta del estado de la Service Broker")

    *Consulta del estado de la Service Broker*
8. Si el valor de la columna **está\_broker\_habilitado** en la base de datos es &quot;0&quot;, use el siguiente comando para habilitarlo. Reemplace **&lt;la base de datos&gt;** por el nombre que estableció al crear la base de datos (por ejemplo: signalr).

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample12.sql)]

    ![Habilitar Service Broker](real-time-web-applications-with-signalr/_static/image26.png "Habilitar Service Broker")

    *Habilitar Service Broker*

    > [!NOTE]
    > Si esta consulta aparece en interbloqueo, asegúrese de que no haya ninguna aplicación conectada a la base de BD.

<a id="Ex2Task3"></a>
#### <a name="task-3--configuring-the-signalr-application"></a>Tarea 3: configuración de la aplicación Signalr

En esta tarea, configurará una **prueba de experto** para conectarse al backplane SQL Server. En primer lugar, agregará el paquete NuGet **signalr. SqlServer** y establecerá la cadena de conexión en la base de datos de backplane.

1. Abra la **consola del administrador de paquetes** desde **herramientas** > **Administrador de paquetes NuGet**. Asegúrese de que el proyecto **GeekQuiz** está seleccionado en la lista desplegable **proyecto predeterminado** . Escriba el siguiente comando para instalar el paquete NuGet **Microsoft. Aspnet. signalr. SqlServer** .

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample13.ps1)]
2. Repita el paso anterior, pero esta vez para el proyecto **GeekQuiz2**.
3. Para configurar la SQL Server backplane, abra el archivo **Startup.CS** del proyecto **GeekQuiz** y agregue el código siguiente al método **Configure** . Reemplace **&lt;la base de datos&gt;** por el nombre de la base de datos que usó al crear el backplane SQL Server. Repita este paso para el proyecto **GeekQuiz2** .

    (Fragmento de código- *RealTimeSignalR-Ex2-StartupConfiguration*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample14.cs)]
4. Ahora que ambos proyectos están configurados para usar el backplane SQL Server, presione **F5** para ejecutarlos simultáneamente.
5. De nuevo, **Visual Studio** iniciará dos instancias de una **prueba de experto** en distintos puertos. Ancle uno de los exploradores de la izquierda y el otro a la derecha de la pantalla e inicie sesión con sus credenciales. Mantenga la página de curiosidades a la izquierda y vaya a la página de **estadísticas** en el explorador adecuado.
6. Empiece a responder a preguntas en el explorador izquierdo. Esta vez, la página de **estadísticas** se actualiza gracias al backplane. Cambiar entre aplicaciones (las**estadísticas** están ahora a la izquierda y la **curiosidad** está a la derecha) y repetir la prueba para comprobar que funciona para ambas instancias. El backplane actúa como una *caché compartida* de mensajes para cada servidor conectado y cada servidor almacenará los mensajes en su propia memoria caché local para distribuirlos a los clientes conectados.
7. Vuelva a Visual Studio y detenga la depuración.
8. El componente de backplane SQL Server genera automáticamente las tablas necesarias en la base de datos especificada. En el panel de **Explorador de objetos de SQL Server** , abra la base de datos que creó para el backplane (por ejemplo: signalr) y expanda sus tablas. Debería ver las tablas siguientes:

    ![Tablas generadas por el backplane](real-time-web-applications-with-signalr/_static/image27.png)

    *Tablas generadas por el backplane*
9. Haga clic con el botón derecho en la tabla **signalr. messages\_0** y seleccione **ver datos**.

    ![Ver tabla de mensajes de backplane de Signalr](real-time-web-applications-with-signalr/_static/image28.png)

    *Ver tabla de mensajes de backplane de Signalr*
10. Puede ver los distintos mensajes que se envían al **Centro** al responder a las preguntas de curiosidades. El backplane distribuye estos mensajes a cualquier instancia conectada.

    ![Tabla de mensajes de backplane](real-time-web-applications-with-signalr/_static/image29.png)

    *Tabla de mensajes de backplane*

---

<a id="Summary"></a>
## <a name="summary"></a>Resumen

En este laboratorio práctico, ha aprendido a agregar **signalr** a la aplicación y enviar notificaciones desde el servidor a los clientes conectados mediante **hubs**. Además, ha aprendido a escalar horizontalmente la aplicación mediante un componente de *backplane* cuando la aplicación se implementa en varias instancias de IIS.
