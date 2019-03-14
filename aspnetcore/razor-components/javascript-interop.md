---
title: Interoperabilidad de JavaScript de los componentes de Razor
author: guardrex
description: Obtenga información sobre cómo invocar funciones de JavaScript desde .NET y .NET los métodos de JavaScript en aplicaciones Blazor y componentes de Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/javascript-interop
ms.openlocfilehash: 9f822fee8990b03ff15ffa9857a2ddd95328a6ce
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57050372"
---
# <a name="razor-components-javascript-interop"></a>Interoperabilidad de JavaScript de los componentes de Razor

Por [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), y [Luke Latham](https://github.com/guardrex)

Una aplicación de los componentes de Razor puede invocar las funciones de JavaScript desde .NET y .NET métodos desde el código de JavaScript.

## <a name="invoke-javascript-functions-from-net-methods"></a>Invocar funciones de JavaScript desde los métodos de .NET

Hay veces que el código de .NET necesario para llamar a una función de JavaScript. Por ejemplo, una llamada de JavaScript puede exponer las funciones del explorador o la funcionalidad de una biblioteca de JavaScript a la aplicación.

Para llamar a JavaScript desde. NET, use el `IJSRuntime` abstracción. El `InvokeAsync<T>` método `IJSRuntime` toma un identificador para la función de JavaScript que desea invocar junto con cualquier número de argumentos serializables con JSON. Es el identificador de función en relación con el ámbito global (`window`). Si desea llamar `window.someScope.someFunction`, el identificador es `someScope.someFunction`. No hay ninguna necesidad de registrar la función antes de que se llama. El tipo de valor devuelto `T` también debe ser JSON serializable.

Para las aplicaciones de ASP.NET Core Razor componentes del lado servidor:

* Se procesan varias solicitudes de usuario mediante la aplicación del lado servidor. No llame a `JSRuntime.Current` en un componente para invocar las funciones de JavaScript.
* Insertar el `IJSRuntime` abstracción y use el objeto insertado para emitir llamadas de interoperabilidad de JavaScript.

En el siguiente ejemplo se basa en [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), un descodificador experimental basadas en JavaScript. El ejemplo muestra cómo invocar una función de JavaScript desde un C# método. La función de JavaScript acepta una matriz de bytes desde un C# método, descodifica la matriz y devuelve el texto al componente para su presentación.

Dentro de la `<head>` elemento de *wwwroot/index.HTML*, proporcione una función que usa `TextDecoder` para descodificar una matriz pasada:

```html
<script>
  window.ConvertArray = (win1251Array) => {
    var win1251decoder = new TextDecoder('windows-1251');
    var bytes = new Uint8Array(win1251Array);
    var decodedArray = win1251decoder.decode(bytes);
    console.log(decodedArray);
    return decodedArray;
  };
</script>
```

Código de JavaScript, como el código se muestra en el ejemplo anterior, también puede cargar un archivo JavaScript con una referencia al archivo de script en el *wwwroot/index.HTML* archivo.

Los siguientes componentes:

* Invoca el `ConvertArray` función de JavaScript mediante `JsRuntime` al seleccionar un botón de componente (**convertir matriz**) está seleccionado.
* Después de llamar a la función de JavaScript, la matriz pasada se convierte en una cadena. Se devuelve la cadena para el componente para su presentación.

```cshtml
@page "/"
@using Microsoft.JSInterop;
@inject IJSRuntime JsRuntime;

<h1>Call JavaScript Function Example</h1>

<button type="button" class="btn btn-primary" onclick="@ConvertArray">
    Convert Array
</button>

<p class="mt-2" style="font-size:1.6em">
    <span class="badge badge-success">
        @ConvertedText
    </span>
</p>

@functions {
    // Quote (c)2005 Universal Pictures: Serenity
    // https://www.uphe.com/movies/serenity
    // David Krumholtz on IMDB: https://www.imdb.com/name/nm0472710/

    private MarkupString ConvertedText =
        new MarkupString("Select the <b>Convert Array</b> button.");

    private uint[] QuoteArray = new uint[]
        {
            60, 101, 109, 62, 67, 97, 110, 39, 116, 32, 115, 116, 111, 112, 32,
            116, 104, 101, 32, 115, 105, 103, 110, 97, 108, 44, 32, 77, 97,
            108, 46, 60, 47, 101, 109, 62, 32, 45, 32, 77, 114, 46, 32, 85, 110,
            105, 118, 101, 114, 115, 101, 10, 10,
        };

    async void ConvertArray()
    {
        var text =
            await JsRuntime.InvokeAsync<string>("ConvertArray", QuoteArray);

        ConvertedText = new MarkupString(text);

        StateHasChanged();
    }
}
```

Para las aplicaciones de Blazor del lado cliente, el `IJSRuntime` abstracción es accesible desde `JSRuntime.Current`, que hace referencia a la solicitud del usuario actual. Dado que hay un único usuario de una aplicación de Blazor del lado cliente, el uso `JSRuntime.Current` para invocar un JavaScript función funciona con normalidad. Usar solo `JSRuntime.Current` en aplicaciones de Blazor del lado cliente.

En la aplicación de ejemplo de cliente que acompaña a este tema, dos funciones de JavaScript están disponibles para la aplicación del lado cliente que interactúan con el DOM para recibir entradas del usuario y mostrar un mensaje de bienvenida:

* `showPrompt` &ndash; Genera un aviso para aceptar la entrada del usuario (el nombre del usuario) y devuelve el nombre al llamador.
* `displayWelcome` &ndash; Asigna un mensaje de bienvenida de la persona que llama a un objeto DOM con un `id` de `welcome`.

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

Colocar el `<script>` etiqueta a la que hace referencia al archivo JavaScript en el *wwwroot/index.HTML* archivo:

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=16)]

No coloque una etiqueta de script en un archivo de componente porque no se puede actualizar dinámicamente la etiqueta script.

Interoperabilidad de los métodos de .NET con las funciones de JavaScript mediante una llamada a `InvokeAsync<T>` método `IJSRuntime`.

La aplicación de ejemplo usa un par de C# métodos, `Prompt` y `Display`, para invocar la `showPrompt` y `displayWelcome` las funciones de JavaScript:

*JsInteropClasses/ExampleJsInterop.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=6-8,14-16)]

El `IJSRuntime` abstracción es asincrónica para permitir escenarios de servidor. Si la aplicación se ejecuta del lado cliente y desea invocar una función de JavaScript de forma sincrónica, inferiores a `IJSInProcessRuntime` y llamar a `Invoke<T>` en su lugar. Se recomienda que mayor uso de la interoperabilidad de las bibliotecas de JavaScript las API para garantizar las bibliotecas asincrónicas están disponibles en todos los escenarios, del lado cliente o servidor.

La aplicación de ejemplo incluye un componente para demostrar la interoperabilidad JS. El componente:

* Recibe la entrada de usuario a través de un símbolo del sistema JS.
* Devuelve el texto para el componente para su procesamiento.
* Llama a una segunda función JS que interactúa con el DOM para mostrar un mensaje de bienvenida.

*Pages/JSInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=1&end=21&highlight=2-3,9-11,13,16-20)]

1. Cuando `TriggerJsPrompt` se ejecuta al seleccionar el componente **desencadenador JavaScript Prompt** botón, el `ExampleJsInterop.Prompt` método C# se llama al código.
1. El `Prompt` método ejecuta el código JavaScript `showPrompt` función incluida en el *wwwroot/exampleJsInterop.js* archivo.
1. El `showPrompt` función acepta la entrada del usuario (el nombre del usuario), que está codificado en HTML y se devuelven a la `Prompt` método y en última instancia, de nuevo en el componente. El componente almacena el nombre del usuario en una variable local, `name`.
1. La cadena almacenada en `name` se incorpora a un mensaje de bienvenida, que se pasa a un segundo C# método `ExampleJsInterop.Display`.
1. `Display` llama a una función de JavaScript, `displayWelcome`, que representa el mensaje de bienvenida en una etiqueta de encabezado.

## <a name="capture-references-to-elements"></a>Capturar las referencias a elementos

Algunos [JavaScript interoperabilidad](xref:razor-components/javascript-interop) escenarios requieren referencias a elementos HTML. Por ejemplo, una biblioteca de interfaz de usuario puede requerir una referencia de elemento para la inicialización, o es posible que deba llamar a API de comando en un elemento, como `focus` o `play`.

Puede capturar las referencias a elementos HTML en un componente mediante la adición de un `ref` atributo al elemento HTML y, a continuación, definir un campo de tipo `ElementRef` cuyo nombre coincide con el valor de la `ref` atributo.

El ejemplo siguiente muestra la captura de una referencia al elemento de entrada de nombre de usuario:

```csharp
<input ref="username" ... />

@functions {
    ElementRef username;
}
```

> [!NOTE]
> Hacer **no** utilizar referencias de elemento capturado como una manera de rellenar el DOM. Si lo hace, puede interferir con el modelo de representación declarativa.

En cuanto a código. NET, un `ElementRef` es un identificador opaco. El *sólo* lo puede hacer con ellos es pasar a través al código de JavaScript a través de interoperabilidad de JavaScript. Al hacerlo, el código JavaScript del lado recibe un `HTMLElement` instancia, que puede usar con las API de DOM normal.

Por ejemplo, el código siguiente define un método de extensión de .NET que permite establecer el foco en un elemento:

*mylib.js*:

```javascript
window.myLib = {
  focusElement : function (element) {
    element.focus();
  }
}
```

*ElementRefExtensions.cs*:

```csharp
using Microsoft.AspNetCore.Blazor;
using Microsoft.JSInterop;
using System.Threading.Tasks;

namespace MyLib
{
    public static class MyLibElementRefExtensions
    {
        public static Task Focus(this ElementRef elementRef)
        {
            return JSRuntime.Current.InvokeAsync<object>("myLib.focusElement", elementRef);
        }
    }
}
```

Ahora puede centrarse entradas en cualquiera de los componentes:

```cshtml
@using MyLib

<input ref="username" />
<button onclick="@SetFocus">Set focus</button>

@functions {
    ElementRef username;

    void SetFocus()
    {
        username.Focus();
    }
}
```

> [!IMPORTANT]
> El `username` variable solo se rellena cuando se representa el componente y su salida incluye el `<input>` elemento. Si intenta pasar un vacía `ElementRef` al código de JavaScript, recibe el código de JavaScript `null`. Para manipular las referencias del elemento después de que el componente ha terminado de representación (para establecer el foco inicial en un elemento) use el `OnAfterRenderAsync` o `OnAfterRender` [métodos del ciclo de vida del componente](xref:razor-components/components#lifecycle-methods).

## <a name="invoke-net-methods-from-javascript-functions"></a>Invocar métodos de .NET desde las funciones de JavaScript

### <a name="static-net-method-call"></a>Llamada de método estática de .NET

Para invocar un método estático de .NET desde JavaScript, use el `DotNet.invokeMethod` o `DotNet.invokeMethodAsync` funciones. Pasar el identificador del método estático que desea llamar, el nombre del ensamblado que contiene la función y los argumentos. De nuevo, se prefiere la versión asincrónica para admitir escenarios de servidor. Para que se pueden invocar desde JavaScript, el método de .NET debe ser público, estático y decorados con `[JSInvokable]`. De forma predeterminada, el identificador de método es el nombre del método, pero puede especificar un identificador diferente utilizando el `JSInvokableAttribute` constructor. Actualmente no se admite llamar a métodos genéricos abiertos.

La aplicación de ejemplo incluye un C# método para devolver una matriz de `int`s. El método está decorado con el `JSInvokable` atributo.

*Pages/JsInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=47&end=58&highlight=7-11)]

Proporciona al cliente de JavaScript invoca el C# método de. NET.

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-12)]

Cuando el **método estático de .NET de desencadenador ReturnArrayAsync** botón está seleccionado, examine la salida de consola en herramientas de desarrollo del explorador web:

```console
Array(4) [ 1, 2, 3, 4 ]
```

El cuarto valor de matriz se inserta en la matriz (`data.push(4);`) devuelto por `ReturnArrayAsync`.

### <a name="instance-method-call"></a>Llamada al método de instancia

También puede llamar a métodos de instancia de .NET desde JavaScript. Para invocar un método de instancia de .NET desde JavaScript, primero pasa la instancia de .NET a JavaScript incluyéndolo en una `DotNetObjectRef` instancia. La instancia de .NET se pasa por referencia a JavaScript, y puede invocar métodos de instancia de .NET en la instancia utilizando el `invokeMethod` o `invokeMethodAsync` funciones. La instancia de .NET también se puede pasar como argumento al invocar otros métodos de .NET desde JavaScript.

> [!NOTE]
> La aplicación de ejemplo registra los mensajes en la consola de cliente. Los ejemplos siguientes que se muestra en la aplicación de ejemplo, examine la salida de la consola del explorador en herramientas de desarrollo del explorador.

Cuando el **método de instancia de desencadenador .NET HelloHelper.SayHello** botón está seleccionado, `ExampleJsInterop.CallHelloHelperSayHello` se llama y se pasa un nombre, `Blazor`, al método.

*Pages/JsInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=60&end=69&highlight=8)]

`CallHelloHelperSayHello` invoca la función de JavaScript `sayHello` con una nueva instancia de `HelloHelper`.

*JsInteropClasses/ExampleJsInterop.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=19-25)]

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-17)]

El nombre se pasa a `HelloHelper`del constructor, que establece el `HelloHelper.Name` propiedad. Cuando la función de JavaScript `sayHello` se ejecuta, `HelloHelper.SayHello` devuelve el `Hello, {Name}!` mensaje, que se escribe en la consola de la función de JavaScript.

*JsInteropClasses/HelloHelper.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

Salida de herramientas para desarrolladores del explorador web en la consola:

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-razor-component-class-library"></a>Compartir código de interoperabilidad en una biblioteca de clases de componente de Razor

Código de interoperabilidad de JavaScript se puede incluir en una biblioteca de clases de componente de Razor (`dotnet new blazorlib`), que le permite compartir el código en un paquete de NuGet.

La biblioteca de clases de componente de Razor controla la incrustación de recursos de JavaScript en el ensamblado compilado. Los archivos JavaScript se colocan en el *wwwroot* carpeta y las herramientas se encarga de incrustación de recursos cuando se compila la biblioteca.

El paquete de NuGet integrado se hace referencia en el archivo de proyecto de la aplicación tal como se hace referencia a un paquete NuGet normal. Después de la aplicación se ha restaurado, puede llamar código de la aplicación en JavaScript como si fuese C#.
