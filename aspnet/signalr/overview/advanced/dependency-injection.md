---
uid: signalr/overview/advanced/dependency-injection
title: Inserción de dependencias en Signalr | Microsoft Docs
author: bradygaster
description: Las versiones de software que se usan en este tema Visual Studio 2013 versiones anteriores de .NET 4,5 Signalr, versión 2 de este tema, para obtener información acerca de las versiones anteriores de...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 52978b10b6c131ac8eff4535216cc60b43fdf3de
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431869"
---
# <a name="dependency-injection-in-signalr"></a>Inserción de dependencias en SignalR

por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a>Versiones de software utilizadas en este tema
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Signalr versión 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versiones anteriores de este tema
>
> Para obtener información sobre las versiones anteriores de Signalr, consulte [versiones anteriores de signalr](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
>
> Deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [stackoverflow.com](http://stackoverflow.com/).

La inserción de dependencias es una manera de quitar las dependencias codificadas de forma rígida entre objetos, lo que facilita reemplazar las dependencias de un objeto, ya sea para las pruebas (mediante objetos ficticios) o para cambiar el comportamiento en tiempo de ejecución. En este tutorial se muestra cómo realizar la inserción de dependencias en los concentradores de Signalr. También se muestra cómo usar los contenedores de IoC con Signalr. Un contenedor de IoC es un marco general para la inserción de dependencias.

## <a name="what-is-dependency-injection"></a>¿Qué es la inserción de dependencias?

Omita esta sección si ya está familiarizado con la inserción de dependencias.

La *inserción de dependencias* (di) es un patrón en el que los objetos no son responsables de crear sus propias dependencias. Este es un ejemplo sencillo para motivar DI. Supongamos que tiene un objeto que necesita registrar los mensajes. Puede definir una interfaz de registro:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

En el objeto, puede crear una `ILogger` para registrar los mensajes:

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Esto funciona, pero no es el mejor diseño. Si desea reemplazar `FileLogger` por otra implementación `ILogger`, tendrá que modificar `SomeComponent`. Suponiendo que muchos otros objetos usan `FileLogger`, tendrá que cambiarlos todos. O bien, si decide crear `FileLogger` un singleton, también tendrá que realizar cambios en toda la aplicación.

Un mejor enfoque consiste en "Insertar" un `ILogger` en el objeto, por ejemplo, mediante un argumento de constructor:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Ahora, el objeto no es responsable de seleccionar el `ILogger` que se va a usar. Puede cambiar `ILogger` implementaciones sin cambiar los objetos que dependen de ella.

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Este patrón se denomina [inyección de constructor](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Otro patrón es la inyección del establecedor, donde se establece la dependencia a través de un método o una propiedad de establecedor.

## <a name="simple-dependency-injection-in-signalr"></a>Inserción de dependencias simple en Signalr

Considere la aplicación de chat del tutorial [Introducción con signalr](../getting-started/tutorial-getting-started-with-signalr.md). Esta es la clase de concentrador de esa aplicación:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Supongamos que desea almacenar mensajes de chat en el servidor antes de enviarlos. Podría definir una interfaz que abstrae esta funcionalidad y usar DI para insertar la interfaz en la clase `ChatHub`.

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

El único problema es que una aplicación Signalr no crea centros directamente; Signalr los crea automáticamente. De forma predeterminada, Signalr espera que una clase de concentrador tenga un constructor sin parámetros. Sin embargo, puede registrar fácilmente una función para crear instancias de concentrador y usar esta función para realizar la inserción. Registre la función llamando a **host global. DependencyResolver. Register**.

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

Ahora Signalr invocará esta función anónima siempre que necesite crear una instancia de `ChatHub`.

## <a name="ioc-containers"></a>Contenedores de IoC

El código anterior está bien para casos sencillos. Pero tiene que escribir esto:

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

En una aplicación compleja con muchas dependencias, es posible que tenga que escribir gran parte de este código de "conexión". Este código puede ser difícil de mantener, especialmente si las dependencias están anidadas. También es difícil de hacer pruebas unitarias.

Una solución consiste en usar un contenedor de IoC. Un contenedor de IoC es un componente de software que es responsable de administrar las dependencias. Los tipos se registran con el contenedor y, a continuación, se usa el contenedor para crear objetos. El contenedor averigua automáticamente las relaciones de dependencia. Muchos contenedores de IoC también permiten controlar aspectos como la duración y el ámbito de los objetos.

> [!NOTE]
> "IoC" representa "inversion of control", que es un patrón general en el que un marco llama al código de la aplicación. Un contenedor de IoC crea los objetos automáticamente, lo que "invierte" el flujo de control habitual.

## <a name="using-ioc-containers-in-signalr"></a>Uso de contenedores de IoC en Signalr

Probablemente, la aplicación de chat es demasiado simple para beneficiarse de un contenedor de IoC. En su lugar, echemos un vistazo al ejemplo [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) .

El ejemplo de StockTicker define dos clases principales:

- `StockTickerHub`: la clase de concentrador, que administra las conexiones de cliente.
- `StockTicker`: singleton que contiene los precios de las acciones y los actualiza periódicamente.

`StockTickerHub` contiene una referencia al `StockTicker` singleton, mientras que `StockTicker` contiene una referencia a **IHubConnectionContext** para el `StockTickerHub`. Utiliza esta interfaz para comunicarse con instancias de `StockTickerHub`. (Para obtener más información, consulte [difusión de servidor con ASP.net signalr](../getting-started/tutorial-server-broadcast-with-signalr.md)).

Podemos usar un contenedor de IoC para desenredar estas dependencias de un bit. En primer lugar, vamos a simplificar las clases `StockTickerHub` y `StockTicker`. En el código siguiente, he comentado las partes que no necesitamos.

Quite el constructor sin parámetros de `StockTickerHub`. En su lugar, usaremos siempre DI para crear el centro.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

Para StockTicker, quite la instancia singleton. Más adelante, usaremos el contenedor de IoC para controlar la duración del StockTicker. Además, haga que el constructor sea público.

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

A continuación, se puede refactorizar el código mediante la creación de una interfaz para `StockTicker`. Usaremos esta interfaz para desacoplar el `StockTickerHub` de la clase `StockTicker`.

Visual Studio facilita este tipo de refactorización. Abra el archivo StockTicker.cs, haga clic con el botón derecho en la declaración de clase `StockTicker` y seleccione **refactorizar** ... **Extraer interfaz**.

![](dependency-injection/_static/image1.png)

En el cuadro de diálogo **Extraer interfaz** , haga clic en **seleccionar todo**. Deje los demás valores predeterminados. Haga clic en **Aceptar**.

![](dependency-injection/_static/image2.png)

Visual Studio crea una nueva interfaz denominada `IStockTicker`y también cambia `StockTicker` para derivar de `IStockTicker`.

Abra el archivo IStockTicker.cs y cambie la interfaz a **público**.

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

En la clase `StockTickerHub`, cambie las dos instancias de `StockTicker` a `IStockTicker`:

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

Crear una interfaz de `IStockTicker` no es estrictamente necesario, pero quería mostrar cómo DI puede ayudar a reducir el acoplamiento entre los componentes de la aplicación.

## <a name="add-the-ninject-library"></a>Agregar la biblioteca Ninject

Hay muchos contenedores de IoC de código abierto para .NET. En este tutorial, usaremos [Ninject](http://www.ninject.org/). (Otras bibliotecas populares incluyen el ejemplo de la [Windsor](http://www.castleproject.org/), [Spring.net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity)y [StructureMap](http://docs.structuremap.net)).

Use el administrador de paquetes NuGet para instalar la [biblioteca Ninject](https://nuget.org/packages/Ninject/3.0.1.10). En Visual Studio, en el menú **herramientas** , seleccione **Administrador de paquetes NuGet** > **consola del administrador de paquetes**. En la ventana Package Manager Console, escriba el siguiente comando:

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a>Reemplazar el solucionador de dependencias de Signalr

Para usar Ninject en Signalr, cree una clase que derive de **DefaultDependencyResolver**.

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

Esta clase invalida los métodos **GetService** y **GetServices** de **DefaultDependencyResolver**. Signalr llama a estos métodos para crear varios objetos en tiempo de ejecución, incluidas las instancias de Hub, así como varios servicios utilizados internamente por Signalr.

- El método **GetService** crea una única instancia de un tipo. Invalide este método para llamar al método **TryGet** del kernel de Ninject. Si ese método devuelve null, revierte al solucionador predeterminado.
- El método **GetServices** crea una colección de objetos de un tipo especificado. Invalide este método para concatenar los resultados de Ninject con los resultados del solucionador predeterminado.

## <a name="configure-ninject-bindings"></a>Configuración de enlaces de Ninject

Ahora usaremos Ninject para declarar enlaces de tipo.

Abra la clase Startup.cs de la aplicación (que creó manualmente según las instrucciones del paquete en `readme.txt`, o que se creó agregando la autenticación al proyecto). En el método `Startup.Configuration`, cree el contenedor Ninject, que Ninject llama al *kernel*.

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

Cree una instancia de la resolución de dependencias personalizada:

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

Cree un enlace para `IStockTicker` como se indica a continuación:

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

Este código dice dos cosas. En primer lugar, cada vez que la aplicación necesita un `IStockTicker`, el kernel debe crear una instancia de `StockTicker`. En segundo lugar, la clase `StockTicker` debe crearse como un objeto singleton. Ninject creará una instancia del objeto y devolverá la misma instancia para cada solicitud.

Cree un enlace para **IHubConnectionContext** como se indica a continuación:

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

Este código crea una función anónima que devuelve un **IHubConnection**. El método **WhenInjectedInto** indica a Ninject que use esta función solo al crear instancias de `IStockTicker`. La razón es que Signalr crea las instancias de **IHubConnectionContext** internamente y no queremos invalidar el modo en que signalr las crea. Esta función solo se aplica a la clase `StockTicker`.

Pase la resolución de dependencia al método **MapSignalR** agregando una configuración de concentrador:

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

Actualice el método startup. ConfigureSignalR en la clase de inicio del ejemplo con el nuevo parámetro:

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

Ahora Signalr usará el solucionador especificado en **MapSignalR**, en lugar del solucionador predeterminado.

Esta es la lista de código completa para `Startup.Configuration`.

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

Para ejecutar la aplicación StockTicker en Visual Studio, presione F5. En la ventana del explorador, vaya a `http://localhost:*port*/SignalR.Sample/StockTicker.html`.

![](dependency-injection/_static/image3.png)

La aplicación tiene exactamente la misma funcionalidad que antes. (Para obtener una descripción, consulte [difusión de servidor con ASP.net signalr](../getting-started/tutorial-server-broadcast-with-signalr.md)). No hemos cambiado el comportamiento. basta con hacer que el código sea más fácil de probar, mantener y desarrollar.
