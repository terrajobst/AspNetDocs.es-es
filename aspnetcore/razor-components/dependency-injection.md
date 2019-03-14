---
title: Inserción de dependencias de los componentes de Razor
author: guardrex
description: Consulte cómo Blazor y componentes de Razor de aplicaciones pueden usar servicios integrados por los insertan en componentes.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/dependency-injection
ms.openlocfilehash: 6ce8fa74f20145f48797d267c20ef2593368b941
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57042432"
---
# <a name="razor-components-dependency-injection"></a><span data-ttu-id="8fdda-103">Inserción de dependencias de los componentes de Razor</span><span class="sxs-lookup"><span data-stu-id="8fdda-103">Razor Components dependency injection</span></span>

<span data-ttu-id="8fdda-104">Por [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="8fdda-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="8fdda-105">[Inserción de dependencias (DI)](/aspnet/core/fundamentals/dependency-injection) está integrada.</span><span class="sxs-lookup"><span data-stu-id="8fdda-105">[Dependency injection (DI)](/aspnet/core/fundamentals/dependency-injection) is built-in.</span></span> <span data-ttu-id="8fdda-106">Las aplicaciones pueden usar los servicios integrados por los insertan en componentes.</span><span class="sxs-lookup"><span data-stu-id="8fdda-106">Apps can use built-in services by having them injected into components.</span></span> <span data-ttu-id="8fdda-107">Las aplicaciones también pueden definir los servicios personalizados y que estén disponibles a través de DI.</span><span class="sxs-lookup"><span data-stu-id="8fdda-107">Apps can also define custom services and make them available via DI.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="8fdda-108">Inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="8fdda-108">Dependency injection</span></span>

<span data-ttu-id="8fdda-109">Inserción de dependencias es una técnica para tener acceso a los servicios configurados en una ubicación central.</span><span class="sxs-lookup"><span data-stu-id="8fdda-109">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="8fdda-110">Esto puede ser útil para:</span><span class="sxs-lookup"><span data-stu-id="8fdda-110">This can be useful to:</span></span>

* <span data-ttu-id="8fdda-111">Compartir una única instancia de una clase de servicio a través de muchos componentes (conocido como un *singleton* service).</span><span class="sxs-lookup"><span data-stu-id="8fdda-111">Share a single instance of a service class across many components (known as a *singleton* service).</span></span>
* <span data-ttu-id="8fdda-112">Desacoplar componentes de las clases de servicio concreto en particular y solo hacen referencia a las abstracciones.</span><span class="sxs-lookup"><span data-stu-id="8fdda-112">Decouple components from particular concrete service classes and only reference abstractions.</span></span> <span data-ttu-id="8fdda-113">Por ejemplo, una interfaz `IDataAccess` se implementa mediante una clase concreta `DataAccess`.</span><span class="sxs-lookup"><span data-stu-id="8fdda-113">For example, an interface `IDataAccess` is implemented by a concrete class `DataAccess`.</span></span> <span data-ttu-id="8fdda-114">Cuando un componente usa DI para recibir un `IDataAccess` implementación, el componente no es estrictamente con el tipo concreto.</span><span class="sxs-lookup"><span data-stu-id="8fdda-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="8fdda-115">La implementación se puede intercambiar, quizás a una implementación ficticia en pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="8fdda-115">The implementation can be swapped, perhaps to a mock implementation in unit tests.</span></span>

<span data-ttu-id="8fdda-116">El sistema de DI es responsable de suministrar las instancias de servicios de componentes.</span><span class="sxs-lookup"><span data-stu-id="8fdda-116">The DI system is responsible for supplying instances of services to components.</span></span> <span data-ttu-id="8fdda-117">Inserción de dependencias también resuelve las dependencias recursivamente para que los propios servicios pueden depender de otros servicios.</span><span class="sxs-lookup"><span data-stu-id="8fdda-117">DI also resolves dependencies recursively so that services themselves can depend on further services.</span></span> <span data-ttu-id="8fdda-118">Inserción de dependencias se configura durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8fdda-118">DI is configured during startup of the app.</span></span> <span data-ttu-id="8fdda-119">Más adelante en este tema se muestra un ejemplo.</span><span class="sxs-lookup"><span data-stu-id="8fdda-119">An example is shown later in this topic.</span></span>

## <a name="add-services-to-di"></a><span data-ttu-id="8fdda-120">Agregar servicios a la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="8fdda-120">Add services to DI</span></span>

<span data-ttu-id="8fdda-121">Después de crear una nueva aplicación, examine el `Startup.ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="8fdda-121">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="8fdda-122">El `ConfigureServices` se pasa al método un [IServiceCollection](/dotnet/api/microsoft.extensions.dependencyinjection.iservicecollection), que es una lista de objetos de descriptor de servicio ([ServiceDescriptor](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor)).</span><span class="sxs-lookup"><span data-stu-id="8fdda-122">The `ConfigureServices` method is passed an [IServiceCollection](/dotnet/api/microsoft.extensions.dependencyinjection.iservicecollection), which is a list of service descriptor objects ([ServiceDescriptor](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor)).</span></span> <span data-ttu-id="8fdda-123">Los servicios se agregan al proporcionar descriptores del servicio a la colección de servicios.</span><span class="sxs-lookup"><span data-stu-id="8fdda-123">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="8fdda-124">Ejemplo de código siguiente muestra el concepto:</span><span class="sxs-lookup"><span data-stu-id="8fdda-124">The following code sample demonstrates the concept:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="8fdda-125">Los servicios pueden configurarse con las duraciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="8fdda-125">Services can be configured with the following lifetimes:</span></span>

| <span data-ttu-id="8fdda-126">Método</span><span class="sxs-lookup"><span data-stu-id="8fdda-126">Method</span></span>      | <span data-ttu-id="8fdda-127">Descripción</span><span class="sxs-lookup"><span data-stu-id="8fdda-127">Description</span></span> |
| ----------- | ----------- |
| [<span data-ttu-id="8fdda-128">Singleton</span><span class="sxs-lookup"><span data-stu-id="8fdda-128">Singleton</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.singleton#Microsoft_Extensions_DependencyInjection_ServiceDescriptor_Singleton__1_System_Func_System_IServiceProvider___0__) | <span data-ttu-id="8fdda-129">Inserción de dependencias se crea un *única instancia* del servicio.</span><span class="sxs-lookup"><span data-stu-id="8fdda-129">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="8fdda-130">Todos los componentes que requieren este servicio reciben una referencia a esta instancia.</span><span class="sxs-lookup"><span data-stu-id="8fdda-130">All components requiring this service receive a reference to this instance.</span></span> |
| [<span data-ttu-id="8fdda-131">Transitoria</span><span class="sxs-lookup"><span data-stu-id="8fdda-131">Transient</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.transient) | <span data-ttu-id="8fdda-132">Cada vez que un componente requiere este servicio, recibe un *nueva instancia* del servicio.</span><span class="sxs-lookup"><span data-stu-id="8fdda-132">Whenever a component requires this service, it receives a *new instance* of the service.</span></span> |
| [<span data-ttu-id="8fdda-133">Con ámbito</span><span class="sxs-lookup"><span data-stu-id="8fdda-133">Scoped</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.scoped) | <span data-ttu-id="8fdda-134">Del lado cliente Blazor actualmente no tiene el concepto de ámbitos de DI.</span><span class="sxs-lookup"><span data-stu-id="8fdda-134">Client-side Blazor doesn't currently have the concept of DI scopes.</span></span> <span data-ttu-id="8fdda-135">`Scoped` se comporta como `Singleton`.</span><span class="sxs-lookup"><span data-stu-id="8fdda-135">`Scoped` behaves like `Singleton`.</span></span> <span data-ttu-id="8fdda-136">Sin embargo, los componentes de Razor de ASP.NET Core admiten la `Scoped` duración.</span><span class="sxs-lookup"><span data-stu-id="8fdda-136">However, ASP.NET Core Razor Components support the `Scoped` lifetime.</span></span> <span data-ttu-id="8fdda-137">En un componente de Razor, un registro de servicio con ámbito se limita a la conexión.</span><span class="sxs-lookup"><span data-stu-id="8fdda-137">In a Razor Component, a scoped service registration is scoped to the connection.</span></span> <span data-ttu-id="8fdda-138">Por este motivo, es preferible para los servicios que se deben acotar al usuario actual utilizando los servicios de ámbito (incluso si la intención actual es ejecutar el cliente en el explorador).</span><span class="sxs-lookup"><span data-stu-id="8fdda-138">For this reason, using scoped services is preferred for services that should be scoped to the current user (even if the current intent is to run client-side in the browser).</span></span> |

<span data-ttu-id="8fdda-139">El sistema de DI se basa en el sistema de DI en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8fdda-139">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="8fdda-140">Para obtener más información, consulte [inserción de dependencias en ASP.NET Core](/aspnet/core/fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8fdda-140">For more information, see [Dependency injection in ASP.NET Core](/aspnet/core/fundamentals/dependency-injection).</span></span>

## <a name="default-services"></a><span data-ttu-id="8fdda-141">Servicios predeterminados</span><span class="sxs-lookup"><span data-stu-id="8fdda-141">Default services</span></span>

<span data-ttu-id="8fdda-142">Servicios predeterminados se agregan automáticamente a la colección de servicios de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="8fdda-142">Default services are automatically added to the service collection of an app.</span></span> <span data-ttu-id="8fdda-143">La siguiente tabla muestra algunos de los servicios útiles predeterminado proporcionados.</span><span class="sxs-lookup"><span data-stu-id="8fdda-143">The following table shows some of the useful default services provided.</span></span>

| <span data-ttu-id="8fdda-144">Método</span><span class="sxs-lookup"><span data-stu-id="8fdda-144">Method</span></span>       | <span data-ttu-id="8fdda-145">Descripción</span><span class="sxs-lookup"><span data-stu-id="8fdda-145">Description</span></span> |
| ------------ | ----------- |
| [<span data-ttu-id="8fdda-146">HttpClient</span><span class="sxs-lookup"><span data-stu-id="8fdda-146">HttpClient</span></span>](/dotnet/api/system.net.http.httpclient) | <span data-ttu-id="8fdda-147">Proporciona métodos para enviar solicitudes HTTP y recibir respuestas HTTP de un recurso identificado por un URI (singleton).</span><span class="sxs-lookup"><span data-stu-id="8fdda-147">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI (singleton).</span></span> <span data-ttu-id="8fdda-148">Tenga en cuenta que esta instancia de `HttpClient` utiliza el explorador para controlar el tráfico HTTP en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="8fdda-148">Note that this instance of `HttpClient` uses the browser for handling the HTTP traffic in the background.</span></span> <span data-ttu-id="8fdda-149">[HttpClient.BaseAddress](/dotnet/api/system.net.http.httpclient.baseaddress) se establece automáticamente en el prefijo URI base de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8fdda-149">[HttpClient.BaseAddress](/dotnet/api/system.net.http.httpclient.baseaddress) is automatically set to the base URI prefix of the app.</span></span> <span data-ttu-id="8fdda-150">`HttpClient` solo se proporciona a las aplicaciones de Blazor del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="8fdda-150">`HttpClient` is only provided to client-side Blazor apps.</span></span> |
| `IJSRuntime` | <span data-ttu-id="8fdda-151">Representa una instancia de un tiempo de ejecución de JavaScript a la que se pueden enviar las llamadas.</span><span class="sxs-lookup"><span data-stu-id="8fdda-151">Represents an instance of a JavaScript runtime to which calls may be dispatched.</span></span> <span data-ttu-id="8fdda-152">Para obtener más información, consulta <xref:razor-components/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="8fdda-152">For more information, see <xref:razor-components/javascript-interop>.</span></span> |
| `IUriHelper` | <span data-ttu-id="8fdda-153">Aplicaciones auxiliares para trabajar con el estado de los URI y la navegación (singleton).</span><span class="sxs-lookup"><span data-stu-id="8fdda-153">Helpers for working with URIs and navigation state (singleton).</span></span> <span data-ttu-id="8fdda-154">`IUriHelper` se proporciona para ambas aplicaciones del lado cliente, Blazor y componentes de Razor de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8fdda-154">`IUriHelper` is provided to both client-side Blazor and ASP.NET Core Razor Components apps.</span></span> |

<span data-ttu-id="8fdda-155">Tenga en cuenta que es posible usar un proveedor de servicios personalizados en lugar del proveedor de servicio predeterminado que se agrega mediante la plantilla predeterminada.</span><span class="sxs-lookup"><span data-stu-id="8fdda-155">Note that it is possible to use a custom services provider instead of the default service provider that's added by the default template.</span></span> <span data-ttu-id="8fdda-156">Un proveedor de servicios personalizada no proporciona automáticamente los servicios predeterminados que se muestran en la tabla.</span><span class="sxs-lookup"><span data-stu-id="8fdda-156">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="8fdda-157">Esos servicios deben agregarse explícitamente para el nuevo proveedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="8fdda-157">Those services must be added to the new service provider explicitly.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="8fdda-158">Solicitar un servicio en un componente</span><span class="sxs-lookup"><span data-stu-id="8fdda-158">Request a service in a component</span></span>

<span data-ttu-id="8fdda-159">Una vez que los servicios se agregan a la colección de servicios, puede insertarse en plantillas de Razor de los componentes mediante la `@inject` directiva Razor.</span><span class="sxs-lookup"><span data-stu-id="8fdda-159">Once services are added to the service collection, they can be injected into the components' Razor templates using the `@inject` Razor directive.</span></span> <span data-ttu-id="8fdda-160">`@inject` tiene dos parámetros:</span><span class="sxs-lookup"><span data-stu-id="8fdda-160">`@inject` has two parameters:</span></span>

* <span data-ttu-id="8fdda-161">Nombre del tipo: El tipo del servicio que se va a insertar.</span><span class="sxs-lookup"><span data-stu-id="8fdda-161">Type name: The type of the service to inject.</span></span>
* <span data-ttu-id="8fdda-162">Nombre de propiedad: El nombre de la propiedad recibe el servicio de aplicación insertado.</span><span class="sxs-lookup"><span data-stu-id="8fdda-162">Property name: The name of the property receiving the injected app service.</span></span> <span data-ttu-id="8fdda-163">Tenga en cuenta que la propiedad no requiere la creación manual.</span><span class="sxs-lookup"><span data-stu-id="8fdda-163">Note that the property doesn't require manual creation.</span></span> <span data-ttu-id="8fdda-164">El compilador crea la propiedad.</span><span class="sxs-lookup"><span data-stu-id="8fdda-164">The compiler creates the property.</span></span>

<span data-ttu-id="8fdda-165">Varios `@inject` instrucciones se pueden utilizar para insertar los diferentes servicios.</span><span class="sxs-lookup"><span data-stu-id="8fdda-165">Multiple `@inject` statements can be used to inject different services.</span></span>

<span data-ttu-id="8fdda-166">En el ejemplo siguiente se muestra cómo utilizar `@inject`.</span><span class="sxs-lookup"><span data-stu-id="8fdda-166">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="8fdda-167">El servicio que implementa `Services.IDataAccess` se inserta en la propiedad del componente `DataRepository`.</span><span class="sxs-lookup"><span data-stu-id="8fdda-167">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="8fdda-168">Tenga en cuenta cómo el código solo usa el `IDataAccess` abstracción:</span><span class="sxs-lookup"><span data-stu-id="8fdda-168">Note how the code is only using the `IDataAccess` abstraction:</span></span>

```csharp
@page "/customer-list"
@using Services
@inject IDataAccess DataRepository

<ul>
    @if (Customers != null)
    {
        @foreach (var customer in Customers)
        {
            <li>@customer.FirstName @customer.LastName</li>
        }
    }
</ul>

@functions {
    private IReadOnlyList<Customer> Customers;

    protected override async Task OnInitAsync()
    {
        // The property DataRepository received an implementation
        // of IDataAccess through dependency injection. Use 
        // DataRepository to obtain data from the server.
        Customers = await DataRepository.GetAllCustomersAsync();
    }
}
```

<span data-ttu-id="8fdda-169">Internamente, la propiedad generada (`DataRepository`) está decorada con el `InjectAttribute` atributo.</span><span class="sxs-lookup"><span data-stu-id="8fdda-169">Internally, the generated property (`DataRepository`) is decorated with the `InjectAttribute` attribute.</span></span> <span data-ttu-id="8fdda-170">Normalmente, este atributo no se usa directamente.</span><span class="sxs-lookup"><span data-stu-id="8fdda-170">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="8fdda-171">Si es necesaria para los componentes de una clase base e insertadas propiedades también son necesarias para la clase base, `InjectAttribute` pueden agregarse manualmente:</span><span class="sxs-lookup"><span data-stu-id="8fdda-171">If a base class is required for components and injected properties are also required for the base class, `InjectAttribute` can be manually added:</span></span>

```csharp
public class ComponentBase : BlazorComponent
{
    // Dependency injection works even if using the
    // InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="8fdda-172">En los componentes derivados de la clase base, el `@inject` directiva no es necesaria.</span><span class="sxs-lookup"><span data-stu-id="8fdda-172">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="8fdda-173">El `InjectAttribute` de la clase base es suficiente:</span><span class="sxs-lookup"><span data-stu-id="8fdda-173">The `InjectAttribute` of the base class is sufficient:</span></span>

```csharp
@page "/demo"
@inherits ComponentBase

<h1>...</h1>
...
```

## <a name="dependency-injection-in-services"></a><span data-ttu-id="8fdda-174">Inserción de dependencias en servicios</span><span class="sxs-lookup"><span data-stu-id="8fdda-174">Dependency injection in services</span></span>

<span data-ttu-id="8fdda-175">Servicios complejos pueden requerir servicios adicionales.</span><span class="sxs-lookup"><span data-stu-id="8fdda-175">Complex services might require additional services.</span></span> <span data-ttu-id="8fdda-176">En el ejemplo anterior, `DataAccess` podría requerir el `HttpClient` servicio predeterminado.</span><span class="sxs-lookup"><span data-stu-id="8fdda-176">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="8fdda-177">`@inject` o `InjectAttribute` no pueden usarse en servicios.</span><span class="sxs-lookup"><span data-stu-id="8fdda-177">`@inject` or the `InjectAttribute` can't be used in services.</span></span> <span data-ttu-id="8fdda-178">*Inserción de constructor* debe usarse en su lugar.</span><span class="sxs-lookup"><span data-stu-id="8fdda-178">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="8fdda-179">Los servicios necesarios se agregan mediante la adición de parámetros al constructor del servicio.</span><span class="sxs-lookup"><span data-stu-id="8fdda-179">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="8fdda-180">Cuando la inserción de dependencias, crea el servicio, reconoce los servicios que requiere en el constructor y las proporciona según corresponda.</span><span class="sxs-lookup"><span data-stu-id="8fdda-180">When dependency injection creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

<span data-ttu-id="8fdda-181">Ejemplo de código siguiente muestra el concepto:</span><span class="sxs-lookup"><span data-stu-id="8fdda-181">The following code sample demonstrates the concept:</span></span>

```csharp
public class DataAccess : IDataAccess
{
    // The constructor receives an HttpClient via dependency
    // injection. HttpClient is a default service.
    public DataAccess(HttpClient client)
    {
        ...
    }
    ...
}
```

<span data-ttu-id="8fdda-182">Tenga en cuenta los siguientes requisitos previos para la inyección de constructor:</span><span class="sxs-lookup"><span data-stu-id="8fdda-182">Note the following prerequisites for constructor injection:</span></span>

* <span data-ttu-id="8fdda-183">Debe haber un constructor cuyos argumentos pueden cumplir la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="8fdda-183">There must be one constructor whose arguments can all be fulfilled by dependency injection.</span></span> <span data-ttu-id="8fdda-184">Tenga en cuenta que se permiten parámetros adicionales no cubiertos por la inserción de dependencias si se especifican los valores predeterminados para ellos.</span><span class="sxs-lookup"><span data-stu-id="8fdda-184">Note that additional parameters not covered by DI are allowed if default values are specified for them.</span></span>
* <span data-ttu-id="8fdda-185">El constructor correspondiente debe ser *pública*.</span><span class="sxs-lookup"><span data-stu-id="8fdda-185">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="8fdda-186">Solo debe haber un constructor aplicable.</span><span class="sxs-lookup"><span data-stu-id="8fdda-186">There must only be one applicable constructor.</span></span> <span data-ttu-id="8fdda-187">En el caso de ambigüedad, inserción de dependencias produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="8fdda-187">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8fdda-188">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="8fdda-188">Additional resources</span></span>

* [<span data-ttu-id="8fdda-189">Inserción de dependencias en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8fdda-189">Dependency injection in ASP.NET Core</span></span>](/aspnet/core/fundamentals/dependency-injection)
