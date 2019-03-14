---
title: Los modelos de hospedaje de componentes de Razor
author: guardrex
description: Comprender Blazor del lado cliente y servidor ASP.NET Core Razor componentes, modelos de hospedaje.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/hosting-models
ms.openlocfilehash: d1e0c472d7d10eeb4cef0da735cf703c98dd1645
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57042442"
---
# <a name="razor-components-hosting-models"></a>Los modelos de hospedaje de componentes de Razor

Por [Daniel Roth](https://github.com/danroth27)

Componentes de Razor es un marco web diseñado para ejecutarse del lado cliente en el explorador en un entorno de ejecución .NET basado en WebAssembly (*Blazor*) o de servidor en ASP.NET Core (*componentes de ASP.NET Core Razor*). Independientemente de los modelos de hospedaje de modelo, la aplicación y el componente *siguen siendo los mismos*. Este artículo describen los modelos de hospedaje disponibles.

## <a name="client-side-hosting-model"></a>Modelo de hospedaje del lado cliente

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

El modelo de hospedaje principal para Blazor es la ejecución del cliente en el explorador. En este modelo, la aplicación Blazor, sus dependencias y el tiempo de ejecución de .NET se descargan en el explorador. La aplicación se ejecuta directamente en el subproceso de interfaz de usuario del explorador. Todas las actualizaciones de la interfaz de usuario y control de eventos se produce dentro del mismo proceso. Los recursos de la aplicación pueden implementarse como archivos estáticos con cualquier servidor web es preferible (consulte [Host e implementar](xref:host-and-deploy/razor-components/index)).

![Blazor de cliente: La aplicación Blazor se ejecuta en un subproceso de interfaz de usuario dentro del explorador.](hosting-models/_static/client-side.png)

Para crear una aplicación de Blazor con el modelo de hospedaje del lado cliente, use el **Blazor** o **Blazor (ASP.NET Core se hospedan)** plantillas de proyecto (`blazor` o `blazorhosted` plantilla cuando se usa el [dotnet nuevo](/dotnet/core/tools/dotnet-new) comando en un símbolo del sistema). El incluye *blazor.webassembly.js* identificadores de secuencias de comandos:

* Descargando el tiempo de ejecución. NET, la aplicación y sus dependencias.
* Inicialización del tiempo de ejecución para ejecutar la aplicación.

El modelo de hospedaje del lado cliente ofrece varias ventajas. Blazor del lado cliente:

* No tiene ninguna dependencia de .NET del lado servidor.
* Tiene una interfaz de usuario interactiva enriquecido.
* Aprovecha completamente las capacidades y los recursos del cliente.
* Las descargas de trabajan desde el servidor al cliente.
* Es compatible con escenarios sin conexión.

Existen desventajas al hospedaje del lado cliente. Blazor del lado cliente:

* Restringe la aplicación a las capacidades del explorador.
* Requiere hardware de cliente compatible con y software (por ejemplo, WebAssembly soporte).
* Tiene un tamaño de descarga y aplicación mayor tiempo de carga.
* Tiene menos madurando en tiempo de ejecución de .NET y las herramientas de soporte técnico (por ejemplo, las limitaciones de compatibilidad con .NET Standard y depuración).

Visual Studio incluye la **Blazor (ASP.NET Core hospedada)** plantilla de proyecto para crear una aplicación Blazor que se ejecuta en WebAssembly y se hospeda en un servidor de ASP.NET Core. La aplicación de ASP.NET Core sirve la aplicación Blazor a los clientes, pero en caso contrario, es un proceso independiente. La aplicación de Blazor del lado cliente puede interactuar con el servidor a través de la red mediante llamadas a API Web o conexiones SignalR.

> [!IMPORTANT]
> Si una aplicación de cliente Blazor atendida por una aplicación de ASP.NET Core hospedada como una aplicación secundaria IIS, deshabilite al controlador heredado del módulo ASP.NET Core. Establecer la ruta de acceso de base de aplicación en la aplicación Blazor *index.html* archivo para el alias IIS utilizado al configurar la aplicación secundaria en IIS.
>
> Para obtener más información, consulte [ruta de acceso de base de aplicación](xref:host-and-deploy/razor-components/index#app-base-path).

## <a name="server-side-hosting-model"></a>Modelo de hospedaje del lado servidor

En el modelo de hospedaje de servidor de componentes de Razor de ASP.NET Core, la aplicación se ejecuta en el servidor desde dentro de una aplicación ASP.NET Core. Las actualizaciones de la interfaz de usuario, el control de eventos y las llamadas de JavaScript se controlan mediante una conexión de SignalR.

![ASP.NET Core Razor componentes del lado servidor: El explorador se interactúa con la aplicación (hospedada dentro de una aplicación ASP.NET Core) en el servidor a través de una conexión de SignalR.](hosting-models/_static/server-side.png)

Para crear una aplicación de los componentes de Razor con el modelo de hospedaje de servidor, utilice el **Blazor (lado del servidor en ASP.NET Core)** plantilla (`blazorserver` al usar [dotnet nuevo](/dotnet/core/tools/dotnet-new) en un símbolo del sistema). Una aplicación ASP.NET Core hospeda la aplicación de servidor de componentes de Razor y establece el punto de conexión de SignalR que se conectan los clientes. La aplicación hace referencia a la aplicación de ASP.NET Core `Startup` clase para agregar:

* Servicios de componentes de Razor del lado servidor.
* La aplicación a la solicitud de canalización de control.

[!code-csharp[](hosting-models/samples_snapshot/Startup.cs?highlight=5,27)]

El *blazor.server.js* script&dagger; establece la conexión de cliente. Es responsabilidad de la aplicación para conservar y restaurar el estado de la aplicación según sea necesario (por ejemplo, en el caso de una conexión de red perdidas).

El modelo de hospedaje de servidor ofrece varias ventajas:

* Le permite escribir toda la aplicación con .NET y C# mediante el modelo de componentes.
* Proporciona una idea interactiva enriquecida y evita actualizaciones innecesarias de página.
* Tiene un tamaño de la aplicación significativamente menor que una aplicación de cliente Blazor y carga con mayor rapidez.
* Lógica del componente puede aprovechar al máximo las capacidades de servidor, incluido el uso de las API compatibles de .NET Core.
* Se ejecuta en .NET Core en el servidor, por lo que las herramientas, como la depuración, de .NET existente funciona según lo previsto.
* Funciona con clientes ligeros (por ejemplo, los exploradores que no son compatibles con WebAssembly y recursos restringir dispositivos).

Existen desventajas al hospedaje de servidor:

* Tiene una latencia mayor: Cada interacción del usuario implica un salto de red.
* No ofrece ninguna compatibilidad sin conexión: Si se produce un error en la conexión de cliente, la aplicación deja de funcionar.
* Ha reducido la escalabilidad: El servidor debe administrar varias conexiones de cliente y administrar el estado de cliente.
* Requiere un servidor de ASP.NET Core para atender la aplicación. No es posible la implementación sin un servidor (por ejemplo, desde una red CDN).

&dagger;El *blazor.server.js* script se publica en la siguiente ruta: *bin / {depurar | La versión} / {.NET FRAMEWORK de destino} /publish/ {nombre de la aplicación}. Dist/aplicación/platafor_ma*.
