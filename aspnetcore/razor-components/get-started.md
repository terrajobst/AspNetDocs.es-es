---
title: Empezar a trabajar con componentes de Razor
author: guardrex
description: Obtenga información sobre cómo empezar a trabajar con componentes de Razor mediante la creación y modificación de un proyecto de componentes de Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/03/2019
uid: razor-components/get-started
ms.openlocfilehash: a9ada603e5ed4e0e75c4aebc5105c331118666e6
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57036392"
---
# <a name="get-started-with-razor-components"></a>Empezar a trabajar con componentes de Razor

Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Requisitos previos:

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

Para crear su primer proyecto de componentes de Razor en Visual Studio:

1. Seleccione **archivo** > **nuevo proyecto** > **Web** > **aplicación Web ASP.NET Core**.
1. Asegúrese de que **.NET Core** y **ASP.NET Core 3.0** están seleccionados en la parte superior.
1. Elija la **Razor componentes** plantilla y seleccione **Aceptar**.

   ![Cuadro de diálogo nueva aplicación](https://msdnshared.blob.core.windows.net/media/2019/01/razor-components-template.png)

1. Presione **F5** para ejecutar la aplicación.

¡Enhorabuena! Acaba de ejecutar su primera aplicación de los componentes de Razor.

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Razor Components project in Visual Studio Code:

1. Execute the following command from a command shell:

   ```console
   dotnet new razorcomponents -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Add a *.vscode* folder.

1. Add a *tasks.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/tasks.json)]

1. Add a *launch.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/launch.json)]

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Razor Components app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.


[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Razor Components project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **ASP.NET Core Razor Components** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Razor Components app!
-->

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli/)

Requisitos previos:

* [Obtener una vista previa de .NET core SDK 3.0](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. Para crear su primer proyecto de Razor componentes desde un shell de comandos:

   ```console
   dotnet new razorcomponents -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. En un navegador, vaya a `https://localhost:5001`.

¡Enhorabuena! Acaba de ejecutar su primera aplicación de los componentes de Razor.

---

## <a name="razor-components-project"></a>Proyecto de componentes de Razor

La solución creada por la plantilla de Razor componentes contiene dos proyectos:

* *WebApplication1.Server* &ndash; el proyecto de servidor es un proyecto de ASP.NET Core configurado para hospedar la aplicación de los componentes de Razor.
* *WebApplication1.App* &ndash; el proyecto de interfaz de usuario web del lado cliente que usa componentes de Razor.

La lógica de la interfaz de usuario en el *WebApplication1.App* proyecto está separado del resto de la aplicación debido a una limitación técnica en ASP.NET Core 3.0 Preview 2. La extensión de archivo Razor (*.cshtml*) utilizado para los componentes de Razor se usa también para las vistas de páginas de Razor y MVC. Actualmente, los componentes de Razor y MVC o páginas de Razor tienen modelos de compilación diferentes, por lo que los archivos de Razor de componentes de Razor se mantienen separados. En una vista previa futura, tenemos previsto introducir una nueva extensión de archivo para los componentes de Razor (*.razor*). Componentes, las páginas y las vistas se hospedará *en el mismo proyecto*.

Cuando se ejecuta la aplicación, están disponibles las fichas en la barra lateral de varias páginas:

* Página principal
* Contador
* Recuperar datos

En la página Contador, seleccione el botón **Click me** para aumentar el contador sin una actualización de página. Aumentar un contador en una página web suele requerir la escritura de JavaScript, pero Razor Components proporciona una mejor manera de usar C#.

*WebApplication1.App/Pages/Counter.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

Una solicitud para `/counter` en el explorador, según lo especificado por el `@page` directiva en la parte superior, hace que el componente de contador representar su contenido. Los componentes se representan en una representación en memoria del árbol de representación que puede usarse para actualizar la interfaz de usuario de una manera flexible y eficaz.

Cada vez que el **Click me** está seleccionado:

* El `onclick` desencadena el evento.
* Se llama al método `IncrementCount` .
* El `currentCount` se incrementa.
* El componente se representa de nuevo.

El tiempo de ejecución compara el contenido nuevo para el contenido anterior y solo aplica el contenido cambiado a Document Object Model (DOM).

Agregar un componente a otro componente mediante una sintaxis similar a HTML. Parámetros del componente se especifican mediante atributos o contenido secundario. Por ejemplo, un componente de contador se puede agregar a la página principal de la aplicación agregando un `<Counter />` elemento para el componente del índice.

*WebApplication1.App/Pages/Index.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

Ejecutar la aplicación. La página principal tiene su propio contador.

Para agregar un parámetro para el componente de contador, actualice el componente `@functions` bloque:

* Agregue una propiedad para `IncrementAmount` decorada con el `[Parameter]` atributo.
* Cambie el método `IncrementCount` para usar `IncrementAmount` al aumentar el valor de `currentCount`.

*WebApplication1.App/Pages/Counter.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

Especifique un parámetro `IncrementAmount` en el elemento `<Counter>` del componente Home mediante un atributo.

*WebApplication1.App/Pages/Index.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

Ejecutar la aplicación. La página principal tiene su propio contador que se incrementa en diez cada vez que el **Click me** botón está seleccionado.

## <a name="next-steps"></a>Pasos siguientes

<xref:tutorials/first-razor-components-app>
