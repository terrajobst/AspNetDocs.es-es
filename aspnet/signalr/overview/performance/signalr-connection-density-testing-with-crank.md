---
uid: signalr/overview/performance/signalr-connection-density-testing-with-crank
title: Pruebas de densidad de conexión de signalr con el Cigüeñado | Microsoft Docs
author: bradygaster
description: Pruebas de densidad de la conexión de SignalR con Crank
ms.author: bradyg
ms.date: 02/22/2015
ms.assetid: 148d9ca7-1af1-44b6-a9fb-91e261b9b463
msc.legacyurl: /signalr/overview/performance/signalr-connection-density-testing-with-crank
msc.type: authoredcontent
ms.openlocfilehash: 901e039fbb81651ed18d560c99745b7e7f716e01
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449875"
---
# <a name="signalr-connection-density-testing-with-crank"></a>Pruebas de densidad de la conexión de SignalR con Crank

por [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este artículo se describe cómo usar la herramienta de arranque para probar una aplicación con varios clientes simulados.

Una vez que la aplicación se ejecuta en su entorno de hospedaje (ya sea un rol Web de Azure, IIS o autohospedado mediante Owin), puede probar la respuesta de la aplicación a un alto nivel de densidad de conexión mediante la herramienta de arranque. El entorno de hospedaje puede ser un servidor Internet Information Services (IIS), un host Owin o un rol Web de Azure. (Nota: los contadores de rendimiento no están disponibles en Web Apps de Azure App Service, por lo que no podrá obtener datos de rendimiento de una prueba de densidad de conexión).

La densidad de conexión hace referencia al número de conexiones TCP simultáneas que se pueden establecer en un servidor. Cada conexión TCP incurre en su propia sobrecarga y, al abrir un gran número de conexiones inactivas, se creará un cuello de botella de memoria.

[El código base de signalr](https://github.com/signalr/signalr) incluye una herramienta de prueba de carga denominada **cigüeñal**. La versión más reciente del cigüeñal se puede encontrar en [la rama dev](https://github.com/SignalR/signalr/tree/dev) en github. [Aquí](https://github.com/SignalR/SignalR/archive/dev.zip)puede descargar un archivo zip de la rama dev del código base de signalr.

El cigüeñal se puede usar para saturar completamente la memoria del servidor con el fin de calcular el número total de conexiones inactivas posibles en el hardware del servidor. Como alternativa, también puede usar el equilibrio para la prueba de carga del servidor en una determinada cantidad de presión de memoria, mediante la puesta en marcha de las conexiones hasta que se alcance un determinado número o un umbral de memoria específico.

Al realizar las pruebas, es importante usar los clientes remotos para evitar la competición por los recursos (es decir, las conexiones TCP y la memoria). Supervise los clientes para asegurarse de que no alcanzan cuellos de botella que puedan impedir que el servidor alcance su capacidad total (memoria o CPU). Es posible que tenga que aumentar el número de clientes para poder cargar el servidor por completo.

### <a name="running-a-connection-density-test"></a>Ejecución de una prueba de densidad de conexión

En esta sección se describen los pasos necesarios para ejecutar una prueba de densidad de conexión en una aplicación Signalr.

1. Descargue y compile la [rama dev del código base de signalr](https://github.com/SignalR/SignalR/archive/dev.zip). En un símbolo del sistema, vaya a &lt;directorio del proyecto&gt;\src\Microsoft.AspNet.SignalR.Crank\bin\debug.
2. Implemente la aplicación en su entorno de hospedaje previsto. Tome nota del punto de conexión que usa la aplicación; por ejemplo, en la aplicación creada en el [tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md), el punto de conexión es `http://<yourhost>:8080/signalr`.
3. Instale los [contadores de rendimiento de signalr](signalr-performance.md#perfcounters) en el servidor. Si la aplicación se ejecuta en Azure, consulte [uso de contadores de rendimiento de signalr en un rol Web de Azure](using-signalr-performance-counters-in-an-azure-web-role.md).

Una vez que haya descargado y compilado el código base, así como los contadores de rendimiento instalados en el host, puede encontrar la herramienta de línea de comandos de un monitor en el `src\Microsoft.AspNet.SignalR.Crank\bin\Debug` carpeta.

Las opciones disponibles para la herramienta de arranque incluyen:

- **/?** : Muestra la pantalla de ayuda. También se muestran las opciones disponibles si se omite el parámetro **URL** .
- **/URL**: la dirección URL para las conexiones de signalr. Este parámetro es necesario. En el caso de una aplicación Signalr que use la asignación predeterminada, la ruta de acceso finalizará en "/signalr".
- **/Seguridad**: el nombre del transporte utilizado. El valor predeterminado es `auto`, que seleccionará el mejor protocolo disponible. Entre las opciones se incluyen `WebSockets`, `ServerSentEvents`y `LongPolling` (`ForeverFrame` no es una opción para el cigüeñal, ya que se usa el cliente de .NET en lugar de Internet Explorer). Para más información sobre cómo Signalr selecciona los transportes, consulte [transportes y reservas](../getting-started/introduction-to-signalr.md#transports).
- **/BatchSize**: número de clientes agregados en cada lote. El valor predeterminado es 50.
- **/ConnectInterval**: el intervalo en milisegundos entre agregar conexiones. El valor predeterminado es 500.
- **/Connections**: el número de conexiones usadas para cargar-probar la aplicación. El valor predeterminado es 100.000.
- **/ConnectTimeout**: tiempo de espera en segundos antes de anular la prueba. El valor predeterminado es 300.
- **MinServerMBytes**: los megabytes de servidor mínimos a los que llegar. El valor predeterminado es 500.
- **SendBytes**: el tamaño de la carga que se envía al servidor en bytes. El valor predeterminado es 0.
- **SendInterval**: el retraso en milisegundos entre los mensajes en el servidor. El valor predeterminado es 500.
- **SendTimeout**: tiempo de espera en milisegundos para los mensajes en el servidor. El valor predeterminado es 300.
- **ControllerUrl**: la dirección URL en la que un cliente hospedará un concentrador de controlador. El valor predeterminado es null (sin concentrador de controladores). El concentrador de controlador se inicia cuando se inicia la sesión de arranque; no se realiza ningún contacto adicional entre el concentrador del controlador y el cigüeñal.
- **NumClients**: número de clientes simulados que se van a conectar a la aplicación. El valor predeterminado es uno.
- **Logfile**: el nombre de archivo del archivo de registro de la ejecución de pruebas. El valor predeterminado es `crank.csv`.
- **SampleInterval**: el tiempo en milisegundos entre muestras del contador de rendimiento. El valor predeterminado es 1000.
- **SignalRInstance**: el nombre de la instancia para los contadores de rendimiento en el servidor. El valor predeterminado es usar el estado de conexión de cliente.

### <a name="example"></a>Ejemplo

El siguiente comando probará un sitio denominado `pfsignalr` en Azure que hospede una aplicación en el puerto 8080 con un concentrador denominado "ControllerHub", con conexiones de 100.

`crank /Connections:100 /Url:http://pfsignalr.cloudapp.net:8080/signalr`
