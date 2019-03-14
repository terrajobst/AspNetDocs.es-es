---
title: Depurar componentes de Razor
author: guardrex
description: Obtenga información sobre cómo depurar aplicaciones Blazor y componentes de Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/debug
ms.openlocfilehash: fb7ddcf3ae40ec28a372adf724a293b375be28a1
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57034122"
---
# <a name="debug-razor-components"></a>Depurar componentes de Razor

[Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Componentes de Razor tiene algunas *muy temprana* compatibilidad para depurar aplicaciones de Blazor del lado cliente que se ejecutan en WebAssembly en Chrome. Aunque esta compatibilidad inicial con la depuración es muy limitado y unpolished, muestra la infraestructura de depuración básica que se unen.

Para depurar una aplicación de cliente Blazor en Chrome:

* Compilar una aplicación Blazor en `Debug` configuración (el valor predeterminado para las aplicaciones no publicadas).
* Ejecute la aplicación Blazor en Chrome (versión 70 o superior).
* Con el foco del teclado en la aplicación (no en el panel de herramientas de desarrollo, que probablemente debe cerrar para una experiencia de depuración menos confusa), seleccione el siguiente método abreviado de teclado de Blazor específica:
  * `Shift+Alt+D` en Windows/Linux
  * `Shift+Cmd+D` en macOS

Ejecute Chrome con la depuración remota habilitada para depurar la aplicación Blazor. Si se deshabilita la depuración remota, Chrome genera una página de error. La página de error contiene instrucciones sobre cómo ejecutar Chrome con el puerto de depuración abierto para que el proxy de depuración Blazor pueda conectarse a la aplicación. *Cierre todas las instancias de Chrome* y reinicie Chrome como se indica.

![Página de error de depuración Blazor](https://user-images.githubusercontent.com/1874516/43123091-01ec0796-8ed8-11e8-844c-23b4e6e9d069.png)

Una vez que se está ejecutando Chrome con la depuración remota habilitada, el método abreviado de teclado depuración abre una nueva pestaña del depurador. Tras unos instantes, el *orígenes* pestaña muestra una lista de los ensamblados de .NET en la aplicación. Expanda cada ensamblado y busque el *.cs*/*.cshtml* disponible para la depuración de archivos de origen. Establezca puntos de interrupción, vuelva a la pestaña de la aplicación y los puntos de interrupción se alcanzan. Después de un punto de interrupción es el alcance, paso a paso (`F10`) o reanudar (`F8`) con normalidad.

![Depuración Blazor](https://user-images.githubusercontent.com/1874516/43123060-efb0b3b0-8ed7-11e8-9ea5-97aa34247a0b.png)

Blazor proporciona un proxy de depuración que implementa el [protocolo Chrome DevTools](https://chromedevtools.github.io/devtools-protocol/) y aumenta el protocolo con. Información específica de la red. Cuando se presiona el método abreviado de teclado depuración, Blazor señala el Chrome DevTools en el servidor proxy. El proxy se conecta a la ventana del explorador que está buscando para depurar (por lo tanto, la necesidad de habilitar la depuración remota).

Tal vez se pregunte por qué no simplemente usamos mapas de código fuente del explorador. Mapas de código fuente permite que el explorador asignar archivos compilados a sus archivos de origen original. Sin embargo, no se asignan Blazor C# directamente a JS/WASM (al menos no todavía). En su lugar, Blazor hace interacción IL dentro del explorador, por lo que no son pertinentes mapas de código fuente.

Tenga en cuenta que las capacidades del depurador son **muy limitado**. Actualmente solo puede:

* Establecer y quitar puntos de interrupción.
* Paso a paso a través del código o reanudar (`F8`).
* En el *variables locales* mostrar, observe los valores de las variables locales de tipo `int`, `string`, y `bool`.
* Ver la pila de llamadas, incluidas las cadenas de llamadas que se envían desde JavaScript en .NET y .NET a JavaScript.

Le *no*:

* Observe los valores de las variables locales que no son un `int`, `string`, o `bool`.
* Observe los valores de campos o propiedades de la clase.
* Mantenga el mouse sobre las variables para ver sus valores
* Evaluar expresiones en la consola.
* Paso a través de las llamadas asincrónicas.
* Realizar la mayoría de los demás escenarios de depuración normales.

Desarrollo de escenarios de depuración adicional es un enfoque en curso del equipo de ingeniería.

## <a name="troubleshooting-tip"></a>Sugerencia de solución de problemas

Si experimenta errores, puede ayudar la siguiente sugerencia:

En el **depurador** pestaña, abra las herramientas de desarrollo en el explorador. En la consola, ejecute `localStorage.clear()` para quitar los puntos de interrupción.
