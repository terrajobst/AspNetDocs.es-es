---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Uso de contadores de rendimiento de Signalr en un rol Web de Azure | Microsoft Docs
author: guardrex
description: Cómo instalar y usar los contadores de rendimiento de Signalr en un rol Web de Azure.
keywords: ASP. NET, signalr, contador de rendimiento, rol Web de Azure
ms.author: bradyg
ms.date: 10/03/2018
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 969a2ce43a7cb8d649555daf282f900401c0c914
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467569"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a>Uso de contadores de rendimiento de Signalr en un rol Web de Azure

Por [Luke Latham](https://github.com/guardrex)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

Los contadores de rendimiento de signalr se usan para supervisar el rendimiento de la aplicación en un rol Web de Azure. Microsoft Azure Diagnostics captura los contadores. Los contadores de rendimiento de Signalr se instalan en Azure con *signalr. exe*, la misma herramienta que se usa para aplicaciones independientes o locales. Dado que los roles de Azure son transitorios, configure una aplicación para instalar y registrar los contadores de rendimiento de Signalr en el inicio.

## <a name="prerequisites"></a>Requisitos previos

* Visual Studio 2015 o [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
* [Microsoft Azure SDK para Visual Studio](https://azure.microsoft.com/downloads/) **Nota: reinicie el equipo después de instalar el SDK.**
* Microsoft Azure suscripción: para registrarse para obtener una cuenta de evaluación gratuita de Azure, consulte [evaluación gratuita de Azure](https://azure.microsoft.com/free/).

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a>Creación de una aplicación de rol Web de Azure que exponga los contadores de rendimiento de Signalr

1. Abra Visual Studio.

2. En Visual Studio, seleccione **Archivo** > **Nuevo** > **Proyecto**.

3. En el cuadro de diálogo **nuevo proyecto** , seleccione la categoría **nube** de  **C# visual** > a la izquierda y, a continuación, seleccione la plantilla servicio en la **nube de Azure** . Asigne a la aplicación el nombre **SignalRPerfCounters** y seleccione **Aceptar**.

   ![Nueva aplicación en la nube](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)

   > [!NOTE]
   > Si no ve la categoría de plantilla de **nube** o la plantilla de servicio en la **nube de Azure** , debe instalar la carga de trabajo de **desarrollo de Azure** para Visual Studio 2017. Elija el vínculo **abrir instalador de Visual Studio** en la parte inferior izquierda del cuadro de diálogo **nuevo proyecto** para abrir instalador de Visual Studio. Seleccione la carga de trabajo **desarrollo de Azure** y, después, elija **modificar** para empezar a instalar la carga de trabajo.
   >
   > ![Carga de trabajo de desarrollo de Azure en Instalador de Visual Studio](using-signalr-performance-counters-in-an-azure-web-role/_static/azure-development-workload.png)

4. En el cuadro de diálogo **nuevo Microsoft Azure servicio en la nube** , seleccione **rol Web ASP.net** y seleccione el botón > para agregar el rol al proyecto. Seleccione **Aceptar**.

   ![Agregar rol Web ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)

5. En el cuadro de diálogo **nueva aplicación Web de ASP.net-WebRole1** , seleccione la plantilla **MVC** y, después, haga clic en **Aceptar**.

   ![Agregar MVC y API Web](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)

6. En **Explorador de soluciones**, abra el archivo *Diagnostics. wadcfgx* en **WebRole1**.

   ![Explorador de soluciones Diagnostics. wadcfgx](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)

7. Reemplace el contenido del archivo con la configuración siguiente y guarde el archivo:

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]

8. Abra la **consola del administrador de paquetes** desde **herramientas** > **Administrador de paquetes NuGet**. Escriba los siguientes comandos para instalar la versión más reciente de Signalr y el paquete de utilidades de Signalr:

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]

9. Configure la aplicación para instalar los contadores de rendimiento de Signalr en la instancia de rol cuando se inicie o se recicle. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **WebRole1** y seleccione **Agregar** > **nueva carpeta**. Asigne a la nueva carpeta el nombre *Startup*.

   ![Agregar carpeta de inicio](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)

10. Copie el archivo *signalr. exe* (agregado con el paquete **Microsoft. Aspnet. Signalr. utils** ) desde \<carpeta del proyecto >/SignalRPerfCounters/Packages/Microsoft.Aspnet.SignalR.utils.\<versión >/Tools a la carpeta de *Inicio* que creó en el paso anterior.

11. En **Explorador de soluciones**, haga clic con el botón secundario en la carpeta de *Inicio* y seleccione **Agregar** > **elemento existente**. En el cuadro de diálogo que aparece, seleccione *signalr. exe* y seleccione **Agregar**.

    ![Agregar signalr. exe al proyecto](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)

12. Haga clic con el botón secundario en la carpeta de *Inicio* que creó. Seleccione **Agregar** > **Nuevo elemento**. Seleccione el nodo **General** , seleccione **archivo de texto**y asigne al nuevo elemento el nombre *SignalRPerfCounterInstall. cmd*. Este archivo de comandos instalará los contadores de rendimiento de Signalr en el rol Web.

    ![Crear archivo por lotes de instalación del contador de rendimiento de Signalr](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)

13. Cuando Visual Studio crea el archivo *SignalRPerfCounterInstall. cmd* , se abrirá automáticamente en la ventana principal. Reemplace el contenido del archivo por el siguiente script y, a continuación, guarde y cierre el archivo. Este script ejecuta *signalr. exe*, que agrega los contadores de rendimiento de signalr a la instancia de rol.

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]

14. Seleccione el archivo *signalr. exe* en **Explorador de soluciones**. En las **propiedades**del archivo, establezca **Copiar en el directorio de salida** en **copiar siempre**.

    ![Establecer copiar en el directorio de salida en copiar siempre](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)

15. Repita el paso anterior para el archivo *SignalRPerfCounterInstall. cmd* .

16. Haga clic con el botón derecho en el archivo *SignalRPerfCounterInstall. cmd* y seleccione **abrir con**. En el cuadro de diálogo que aparece, seleccione **Editor binario** y haga clic en **Aceptar**.

    ![Abrir con el Editor binario](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)

17. En el Editor binario, seleccione los bytes iniciales del archivo y elimínelos. Guarde y cierre el archivo.

    ![Eliminar los bytes iniciales](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)

18. Abra *ServiceDefinition. csdef* y agregue una tarea de inicio que ejecute el archivo *SignalrPerfCounterInstall. cmd* cuando se inicie el servicio:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]

19. Abra `Views/Shared/_Layout.cshtml` y quite el script de paquete de jQuery del final del archivo.

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]

20. Agregue un cliente de JavaScript que llame continuamente al método `increment` en el servidor. Abra `Views/Home/Index.cshtml` y reemplace el contenido por el código siguiente:

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]

21. Cree una nueva carpeta en el proyecto **WebRole1** denominado *hubs*. Haga clic con el botón derecho en la carpeta *hubs* en **Explorador de soluciones** y seleccione **Agregar** > **nuevo elemento**. En el cuadro de diálogo **Agregar nuevo elemento** , seleccione la categoría **Web** > **signalr** y, a continuación, seleccione la plantilla de elemento **clase de concentrador de signalr (V2)** . Asigne al nuevo concentrador el nombre *MyHub.CS* y seleccione **Agregar**.

    ![Adición de la clase Signalr Hub a la carpeta hubs en el cuadro de diálogo Agregar nuevo elemento](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. *MyHub.CS* se abrirá automáticamente en la ventana principal. Reemplace el contenido por el código siguiente y, a continuación, guarde y cierre el archivo:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]

23. *[. Exe](signalr-connection-density-testing-with-crank.md)* es una herramienta de prueba de densidad de conexión que se proporciona con el código base de signalr. Dado que el cigüeñal requiere una conexión persistente, agregue una al sitio para usarla al realizar las pruebas. Agregue una nueva carpeta al proyecto **WebRole1** denominado *PersistentConnections*. Haga clic con el botón secundario en esta carpeta y seleccione **agregar** > **clase**. Asigne al nuevo archivo de clase el nombre *MyPersistentConnections.CS* y seleccione **Agregar**.

24. Visual Studio abrirá el archivo *MyPersistentConnections.CS* en la ventana principal. Reemplace el contenido por el código siguiente y, a continuación, guarde y cierre el archivo:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]

25. Con la clase `Startup`, los objetos Signalr se inician cuando se inicia OWIN. Abra o cree *Startup.CS* y reemplace el contenido por el código siguiente:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]

    En el código anterior, el atributo `OwinStartup` marca esta clase para iniciar OWIN. El método `Configuration` inicia Signalr.

26. Pruebe la aplicación en el Microsoft Azure Emulator presionando **F5**.

    > [!NOTE]
    > Si encuentra un **FileLoadException** en **MapSignalR**, cambie las redirecciones de enlace en *Web. config* por lo siguiente:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]

27. Espere aproximadamente un minuto. Abra la ventana de herramientas de Cloud Explorer en Visual Studio (**ver** > **Cloud Explorer**) y expanda la ruta de acceso `(Local)/Storage Accounts/(Development)/Tables`. Haga doble clic en **WADPerformanceCountersTable**. Debería ver los contadores de Signalr en los datos de la tabla. Si no ve la tabla, es posible que tenga que volver a escribir sus credenciales de Azure Storage. Es posible que tenga que seleccionar el botón **Actualizar** para ver la tabla en **Cloud Explorer** o seleccionar el botón **Actualizar** de la ventana Abrir tabla para ver los datos de la tabla.

    ![Selección de la tabla de contadores de rendimiento de WAD en Visual Studio Cloud Explorer](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Mostrar los contadores recopilados en la tabla de contadores de rendimiento de WAD](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)

28. Para probar la aplicación en la nube, actualice el archivo **ServiceConfiguration. Cloud. cscfg** y establezca la `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` en una cadena de conexión de Azure Storage cuenta válida.

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. Implemente la aplicación en la suscripción de Azure. Para más información sobre cómo implementar una aplicación en Azure, consulte [creación e implementación de un servicio en la nube](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).

30. Espere unos minutos. En **Cloud Explorer**, busque la cuenta de almacenamiento que configuró anteriormente y busque la tabla `WADPerformanceCountersTable` en ella. Debería ver los contadores de Signalr en los datos de la tabla. Si no ve la tabla, es posible que tenga que volver a escribir sus credenciales de Azure Storage. Es posible que tenga que seleccionar el botón **Actualizar** para ver la tabla en **Cloud Explorer** o seleccionar el botón **Actualizar** de la ventana Abrir tabla para ver los datos de la tabla.

Especial gracias a [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) para el contenido original que se usa en este tutorial.
