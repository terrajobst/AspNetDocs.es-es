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
# <a name="razor-components-javascript-interop"></a><span data-ttu-id="ee6b3-103">Interoperabilidad de JavaScript de los componentes de Razor</span><span class="sxs-lookup"><span data-stu-id="ee6b3-103">Razor Components JavaScript interop</span></span>

<span data-ttu-id="ee6b3-104">Por [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ee6b3-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ee6b3-105">Una aplicación de los componentes de Razor puede invocar las funciones de JavaScript desde .NET y .NET métodos desde el código de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-105">A Razor Components app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="ee6b3-106">Invocar funciones de JavaScript desde los métodos de .NET</span><span class="sxs-lookup"><span data-stu-id="ee6b3-106">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="ee6b3-107">Hay veces que el código de .NET necesario para llamar a una función de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-107">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="ee6b3-108">Por ejemplo, una llamada de JavaScript puede exponer las funciones del explorador o la funcionalidad de una biblioteca de JavaScript a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-108">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span>

<span data-ttu-id="ee6b3-109">Para llamar a JavaScript desde. NET, use el `IJSRuntime` abstracción.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-109">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="ee6b3-110">El `InvokeAsync<T>` método `IJSRuntime` toma un identificador para la función de JavaScript que desea invocar junto con cualquier número de argumentos serializables con JSON.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-110">The `InvokeAsync<T>` method on `IJSRuntime` takes an identifier for the JavaScript function you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="ee6b3-111">Es el identificador de función en relación con el ámbito global (`window`).</span><span class="sxs-lookup"><span data-stu-id="ee6b3-111">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="ee6b3-112">Si desea llamar `window.someScope.someFunction`, el identificador es `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-112">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="ee6b3-113">No hay ninguna necesidad de registrar la función antes de que se llama.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-113">There's no need to register the function before it's called.</span></span> <span data-ttu-id="ee6b3-114">El tipo de valor devuelto `T` también debe ser JSON serializable.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-114">The return type `T` must also be JSON serializable.</span></span>

<span data-ttu-id="ee6b3-115">Para las aplicaciones de ASP.NET Core Razor componentes del lado servidor:</span><span class="sxs-lookup"><span data-stu-id="ee6b3-115">For server-side ASP.NET Core Razor Components apps:</span></span>

* <span data-ttu-id="ee6b3-116">Se procesan varias solicitudes de usuario mediante la aplicación del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-116">Multiple user requests are processed by the server-side app.</span></span> <span data-ttu-id="ee6b3-117">No llame a `JSRuntime.Current` en un componente para invocar las funciones de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-117">Don't call `JSRuntime.Current` in a component to invoke JavaScript functions.</span></span>
* <span data-ttu-id="ee6b3-118">Insertar el `IJSRuntime` abstracción y use el objeto insertado para emitir llamadas de interoperabilidad de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-118">Inject the `IJSRuntime` abstraction and use the injected object to issue JavaScript interop calls.</span></span>

<span data-ttu-id="ee6b3-119">En el siguiente ejemplo se basa en [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), un descodificador experimental basadas en JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-119">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="ee6b3-120">El ejemplo muestra cómo invocar una función de JavaScript desde un C# método.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-120">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="ee6b3-121">La función de JavaScript acepta una matriz de bytes desde un C# método, descodifica la matriz y devuelve el texto al componente para su presentación.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-121">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="ee6b3-122">Dentro de la `<head>` elemento de *wwwroot/index.HTML*, proporcione una función que usa `TextDecoder` para descodificar una matriz pasada:</span><span class="sxs-lookup"><span data-stu-id="ee6b3-122">Inside the `<head>` element of *wwwroot/index.html*, provide a function that uses `TextDecoder` to decode a passed array:</span></span>

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

<span data-ttu-id="ee6b3-123">Código de JavaScript, como el código se muestra en el ejemplo anterior, también puede cargar un archivo JavaScript con una referencia al archivo de script en el *wwwroot/index.HTML* archivo.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-123">JavaScript code, such as the code shown in the preceding example, can also be loaded by a JavaScript file with a reference to the script file in the *wwwroot/index.html* file.</span></span>

<span data-ttu-id="ee6b3-124">Los siguientes componentes:</span><span class="sxs-lookup"><span data-stu-id="ee6b3-124">The following component:</span></span>

* <span data-ttu-id="ee6b3-125">Invoca el `ConvertArray` función de JavaScript mediante `JsRuntime` al seleccionar un botón de componente (**convertir matriz**) está seleccionado.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-125">Invokes the `ConvertArray` JavaScript function using `JsRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="ee6b3-126">Después de llamar a la función de JavaScript, la matriz pasada se convierte en una cadena.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-126">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="ee6b3-127">Se devuelve la cadena para el componente para su presentación.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-127">The string is returned to the component for display.</span></span>

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

<span data-ttu-id="ee6b3-128">Para las aplicaciones de Blazor del lado cliente, el `IJSRuntime` abstracción es accesible desde `JSRuntime.Current`, que hace referencia a la solicitud del usuario actual.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-128">For client-side Blazor apps, the `IJSRuntime` abstraction is accessible from `JSRuntime.Current`, which refers to the current user's request.</span></span> <span data-ttu-id="ee6b3-129">Dado que hay un único usuario de una aplicación de Blazor del lado cliente, el uso `JSRuntime.Current` para invocar un JavaScript función funciona con normalidad.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-129">Because there's only a single user of a client-side Blazor app, using `JSRuntime.Current` to invoke a JavaScript function works normally.</span></span> <span data-ttu-id="ee6b3-130">Usar solo `JSRuntime.Current` en aplicaciones de Blazor del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-130">Only use `JSRuntime.Current` in client-side Blazor apps.</span></span>

<span data-ttu-id="ee6b3-131">En la aplicación de ejemplo de cliente que acompaña a este tema, dos funciones de JavaScript están disponibles para la aplicación del lado cliente que interactúan con el DOM para recibir entradas del usuario y mostrar un mensaje de bienvenida:</span><span class="sxs-lookup"><span data-stu-id="ee6b3-131">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the client-side app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="ee6b3-132">`showPrompt` &ndash; Genera un aviso para aceptar la entrada del usuario (el nombre del usuario) y devuelve el nombre al llamador.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-132">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="ee6b3-133">`displayWelcome` &ndash; Asigna un mensaje de bienvenida de la persona que llama a un objeto DOM con un `id` de `welcome`.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-133">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="ee6b3-134">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="ee6b3-134">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="ee6b3-135">Colocar el `<script>` etiqueta a la que hace referencia al archivo JavaScript en el *wwwroot/index.HTML* archivo:</span><span class="sxs-lookup"><span data-stu-id="ee6b3-135">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file:</span></span>

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=16)]

<span data-ttu-id="ee6b3-136">No coloque una etiqueta de script en un archivo de componente porque no se puede actualizar dinámicamente la etiqueta script.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-136">Don't place a script tag in a component file because the script tag can't be updated dynamically.</span></span>

<span data-ttu-id="ee6b3-137">Interoperabilidad de los métodos de .NET con las funciones de JavaScript mediante una llamada a `InvokeAsync<T>` método `IJSRuntime`.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-137">.NET methods interop with the JavaScript functions by calling `InvokeAsync<T>` method on `IJSRuntime`.</span></span>

<span data-ttu-id="ee6b3-138">La aplicación de ejemplo usa un par de C# métodos, `Prompt` y `Display`, para invocar la `showPrompt` y `displayWelcome` las funciones de JavaScript:</span><span class="sxs-lookup"><span data-stu-id="ee6b3-138">The sample app uses a pair of C# methods, `Prompt` and `Display`, to invoke the `showPrompt` and `displayWelcome` JavaScript functions:</span></span>

<span data-ttu-id="ee6b3-139">*JsInteropClasses/ExampleJsInterop.cs*:</span><span class="sxs-lookup"><span data-stu-id="ee6b3-139">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=6-8,14-16)]

<span data-ttu-id="ee6b3-140">El `IJSRuntime` abstracción es asincrónica para permitir escenarios de servidor.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-140">The `IJSRuntime` abstraction is asynchronous to allow for server-side scenarios.</span></span> <span data-ttu-id="ee6b3-141">Si la aplicación se ejecuta del lado cliente y desea invocar una función de JavaScript de forma sincrónica, inferiores a `IJSInProcessRuntime` y llamar a `Invoke<T>` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-141">If the app runs client-side and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="ee6b3-142">Se recomienda que mayor uso de la interoperabilidad de las bibliotecas de JavaScript las API para garantizar las bibliotecas asincrónicas están disponibles en todos los escenarios, del lado cliente o servidor.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-142">We recommend that most JavaScript interop libraries use the async APIs to ensure the libraries are available in all scenarios, client-side or server-side.</span></span>

<span data-ttu-id="ee6b3-143">La aplicación de ejemplo incluye un componente para demostrar la interoperabilidad JS.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-143">The sample app includes a component to demonstrate JS interop.</span></span> <span data-ttu-id="ee6b3-144">El componente:</span><span class="sxs-lookup"><span data-stu-id="ee6b3-144">The component:</span></span>

* <span data-ttu-id="ee6b3-145">Recibe la entrada de usuario a través de un símbolo del sistema JS.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-145">Receives user input via a JS prompt.</span></span>
* <span data-ttu-id="ee6b3-146">Devuelve el texto para el componente para su procesamiento.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-146">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="ee6b3-147">Llama a una segunda función JS que interactúa con el DOM para mostrar un mensaje de bienvenida.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-147">Calls a second JS function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="ee6b3-148">*Pages/JSInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ee6b3-148">*Pages/JSInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=1&end=21&highlight=2-3,9-11,13,16-20)]

1. <span data-ttu-id="ee6b3-149">Cuando `TriggerJsPrompt` se ejecuta al seleccionar el componente **desencadenador JavaScript Prompt** botón, el `ExampleJsInterop.Prompt` método C# se llama al código.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-149">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the `ExampleJsInterop.Prompt` method in C# code is called.</span></span>
1. <span data-ttu-id="ee6b3-150">El `Prompt` método ejecuta el código JavaScript `showPrompt` función incluida en el *wwwroot/exampleJsInterop.js* archivo.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-150">The `Prompt` method executes the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file.</span></span>
1. <span data-ttu-id="ee6b3-151">El `showPrompt` función acepta la entrada del usuario (el nombre del usuario), que está codificado en HTML y se devuelven a la `Prompt` método y en última instancia, de nuevo en el componente.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-151">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the `Prompt` method and ultimately back to the component.</span></span> <span data-ttu-id="ee6b3-152">El componente almacena el nombre del usuario en una variable local, `name`.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-152">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="ee6b3-153">La cadena almacenada en `name` se incorpora a un mensaje de bienvenida, que se pasa a un segundo C# método `ExampleJsInterop.Display`.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-153">The string stored in `name` is incorporated into a welcome message, which is passed to a second C# method, `ExampleJsInterop.Display`.</span></span>
1. <span data-ttu-id="ee6b3-154">`Display` llama a una función de JavaScript, `displayWelcome`, que representa el mensaje de bienvenida en una etiqueta de encabezado.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-154">`Display` calls a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="capture-references-to-elements"></a><span data-ttu-id="ee6b3-155">Capturar las referencias a elementos</span><span class="sxs-lookup"><span data-stu-id="ee6b3-155">Capture references to elements</span></span>

<span data-ttu-id="ee6b3-156">Algunos [JavaScript interoperabilidad](xref:razor-components/javascript-interop) escenarios requieren referencias a elementos HTML.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-156">Some [JavaScript interop](xref:razor-components/javascript-interop) scenarios require references to HTML elements.</span></span> <span data-ttu-id="ee6b3-157">Por ejemplo, una biblioteca de interfaz de usuario puede requerir una referencia de elemento para la inicialización, o es posible que deba llamar a API de comando en un elemento, como `focus` o `play`.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-157">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="ee6b3-158">Puede capturar las referencias a elementos HTML en un componente mediante la adición de un `ref` atributo al elemento HTML y, a continuación, definir un campo de tipo `ElementRef` cuyo nombre coincide con el valor de la `ref` atributo.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-158">You can capture references to HTML elements in a component by adding a `ref` attribute to the HTML element and then defining a field of type `ElementRef` whose name matches the value of the `ref` attribute.</span></span>

<span data-ttu-id="ee6b3-159">El ejemplo siguiente muestra la captura de una referencia al elemento de entrada de nombre de usuario:</span><span class="sxs-lookup"><span data-stu-id="ee6b3-159">The following example shows capturing a reference to the username input element:</span></span>

```csharp
<input ref="username" ... />

@functions {
    ElementRef username;
}
```

> [!NOTE]
> <span data-ttu-id="ee6b3-160">Hacer **no** utilizar referencias de elemento capturado como una manera de rellenar el DOM.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-160">Do **not** use captured element references as a way of populating the DOM.</span></span> <span data-ttu-id="ee6b3-161">Si lo hace, puede interferir con el modelo de representación declarativa.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-161">Doing so may interfere with the declarative rendering model.</span></span>

<span data-ttu-id="ee6b3-162">En cuanto a código. NET, un `ElementRef` es un identificador opaco.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-162">As far as .NET code is concerned, an `ElementRef` is an opaque handle.</span></span> <span data-ttu-id="ee6b3-163">El *sólo* lo puede hacer con ellos es pasar a través al código de JavaScript a través de interoperabilidad de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-163">The *only* thing you can do with it is pass it through to JavaScript code via JavaScript interop.</span></span> <span data-ttu-id="ee6b3-164">Al hacerlo, el código JavaScript del lado recibe un `HTMLElement` instancia, que puede usar con las API de DOM normal.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-164">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="ee6b3-165">Por ejemplo, el código siguiente define un método de extensión de .NET que permite establecer el foco en un elemento:</span><span class="sxs-lookup"><span data-stu-id="ee6b3-165">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="ee6b3-166">*mylib.js*:</span><span class="sxs-lookup"><span data-stu-id="ee6b3-166">*mylib.js*:</span></span>

```javascript
window.myLib = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="ee6b3-167">*ElementRefExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="ee6b3-167">*ElementRefExtensions.cs*:</span></span>

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

<span data-ttu-id="ee6b3-168">Ahora puede centrarse entradas en cualquiera de los componentes:</span><span class="sxs-lookup"><span data-stu-id="ee6b3-168">Now you can focus inputs in any of your components:</span></span>

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
> <span data-ttu-id="ee6b3-169">El `username` variable solo se rellena cuando se representa el componente y su salida incluye el `<input>` elemento.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-169">The `username` variable is only populated after the component renders and its output includes the `<input>` element.</span></span> <span data-ttu-id="ee6b3-170">Si intenta pasar un vacía `ElementRef` al código de JavaScript, recibe el código de JavaScript `null`.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-170">If you try to pass an unpopulated `ElementRef` to JavaScript code, the JavaScript code receives `null`.</span></span> <span data-ttu-id="ee6b3-171">Para manipular las referencias del elemento después de que el componente ha terminado de representación (para establecer el foco inicial en un elemento) use el `OnAfterRenderAsync` o `OnAfterRender` [métodos del ciclo de vida del componente](xref:razor-components/components#lifecycle-methods).</span><span class="sxs-lookup"><span data-stu-id="ee6b3-171">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the `OnAfterRenderAsync` or `OnAfterRender` [component lifecycle methods](xref:razor-components/components#lifecycle-methods).</span></span>

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="ee6b3-172">Invocar métodos de .NET desde las funciones de JavaScript</span><span class="sxs-lookup"><span data-stu-id="ee6b3-172">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="ee6b3-173">Llamada de método estática de .NET</span><span class="sxs-lookup"><span data-stu-id="ee6b3-173">Static .NET method call</span></span>

<span data-ttu-id="ee6b3-174">Para invocar un método estático de .NET desde JavaScript, use el `DotNet.invokeMethod` o `DotNet.invokeMethodAsync` funciones.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-174">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="ee6b3-175">Pasar el identificador del método estático que desea llamar, el nombre del ensamblado que contiene la función y los argumentos.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-175">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="ee6b3-176">De nuevo, se prefiere la versión asincrónica para admitir escenarios de servidor.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-176">Again, the async version is preferred to support server-side scenarios.</span></span> <span data-ttu-id="ee6b3-177">Para que se pueden invocar desde JavaScript, el método de .NET debe ser público, estático y decorados con `[JSInvokable]`.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-177">To be invokable from JavaScript, the .NET method must be public, static, and decorated with `[JSInvokable]`.</span></span> <span data-ttu-id="ee6b3-178">De forma predeterminada, el identificador de método es el nombre del método, pero puede especificar un identificador diferente utilizando el `JSInvokableAttribute` constructor.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-178">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="ee6b3-179">Actualmente no se admite llamar a métodos genéricos abiertos.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-179">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="ee6b3-180">La aplicación de ejemplo incluye un C# método para devolver una matriz de `int`s.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-180">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="ee6b3-181">El método está decorado con el `JSInvokable` atributo.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-181">The method is decorated with the `JSInvokable` attribute.</span></span>

<span data-ttu-id="ee6b3-182">*Pages/JsInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ee6b3-182">*Pages/JsInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=47&end=58&highlight=7-11)]

<span data-ttu-id="ee6b3-183">Proporciona al cliente de JavaScript invoca el C# método de. NET.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-183">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="ee6b3-184">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="ee6b3-184">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-12)]

<span data-ttu-id="ee6b3-185">Cuando el **método estático de .NET de desencadenador ReturnArrayAsync** botón está seleccionado, examine la salida de consola en herramientas de desarrollo del explorador web:</span><span class="sxs-lookup"><span data-stu-id="ee6b3-185">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="ee6b3-186">El cuarto valor de matriz se inserta en la matriz (`data.push(4);`) devuelto por `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-186">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="ee6b3-187">Llamada al método de instancia</span><span class="sxs-lookup"><span data-stu-id="ee6b3-187">Instance method call</span></span>

<span data-ttu-id="ee6b3-188">También puede llamar a métodos de instancia de .NET desde JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-188">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="ee6b3-189">Para invocar un método de instancia de .NET desde JavaScript, primero pasa la instancia de .NET a JavaScript incluyéndolo en una `DotNetObjectRef` instancia.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-189">To invoke a .NET instance method from JavaScript, first pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectRef` instance.</span></span> <span data-ttu-id="ee6b3-190">La instancia de .NET se pasa por referencia a JavaScript, y puede invocar métodos de instancia de .NET en la instancia utilizando el `invokeMethod` o `invokeMethodAsync` funciones.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-190">The .NET instance is passed by reference to JavaScript, and you can invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="ee6b3-191">La instancia de .NET también se puede pasar como argumento al invocar otros métodos de .NET desde JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-191">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="ee6b3-192">La aplicación de ejemplo registra los mensajes en la consola de cliente.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-192">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="ee6b3-193">Los ejemplos siguientes que se muestra en la aplicación de ejemplo, examine la salida de la consola del explorador en herramientas de desarrollo del explorador.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-193">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="ee6b3-194">Cuando el **método de instancia de desencadenador .NET HelloHelper.SayHello** botón está seleccionado, `ExampleJsInterop.CallHelloHelperSayHello` se llama y se pasa un nombre, `Blazor`, al método.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-194">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="ee6b3-195">*Pages/JsInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ee6b3-195">*Pages/JsInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=60&end=69&highlight=8)]

<span data-ttu-id="ee6b3-196">`CallHelloHelperSayHello` invoca la función de JavaScript `sayHello` con una nueva instancia de `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-196">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="ee6b3-197">*JsInteropClasses/ExampleJsInterop.cs*:</span><span class="sxs-lookup"><span data-stu-id="ee6b3-197">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=19-25)]

<span data-ttu-id="ee6b3-198">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="ee6b3-198">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-17)]

<span data-ttu-id="ee6b3-199">El nombre se pasa a `HelloHelper`del constructor, que establece el `HelloHelper.Name` propiedad.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-199">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="ee6b3-200">Cuando la función de JavaScript `sayHello` se ejecuta, `HelloHelper.SayHello` devuelve el `Hello, {Name}!` mensaje, que se escribe en la consola de la función de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-200">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="ee6b3-201">*JsInteropClasses/HelloHelper.cs*:</span><span class="sxs-lookup"><span data-stu-id="ee6b3-201">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="ee6b3-202">Salida de herramientas para desarrolladores del explorador web en la consola:</span><span class="sxs-lookup"><span data-stu-id="ee6b3-202">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-razor-component-class-library"></a><span data-ttu-id="ee6b3-203">Compartir código de interoperabilidad en una biblioteca de clases de componente de Razor</span><span class="sxs-lookup"><span data-stu-id="ee6b3-203">Share interop code in a Razor Component class library</span></span>

<span data-ttu-id="ee6b3-204">Código de interoperabilidad de JavaScript se puede incluir en una biblioteca de clases de componente de Razor (`dotnet new blazorlib`), que le permite compartir el código en un paquete de NuGet.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-204">JavaScript interop code can be included in a Razor Component class library (`dotnet new blazorlib`), which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="ee6b3-205">La biblioteca de clases de componente de Razor controla la incrustación de recursos de JavaScript en el ensamblado compilado.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-205">The Razor Component class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="ee6b3-206">Los archivos JavaScript se colocan en el *wwwroot* carpeta y las herramientas se encarga de incrustación de recursos cuando se compila la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-206">The JavaScript files are placed in the *wwwroot* folder, and the tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="ee6b3-207">El paquete de NuGet integrado se hace referencia en el archivo de proyecto de la aplicación tal como se hace referencia a un paquete NuGet normal.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-207">The built NuGet package is referenced in the project file of the app just as any normal NuGet package is referenced.</span></span> <span data-ttu-id="ee6b3-208">Después de la aplicación se ha restaurado, puede llamar código de la aplicación en JavaScript como si fuese C#.</span><span class="sxs-lookup"><span data-stu-id="ee6b3-208">After the app has been restored, app code can call into JavaScript as if it were C#.</span></span>
