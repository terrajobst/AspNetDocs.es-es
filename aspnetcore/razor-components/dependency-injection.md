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
# <a name="razor-components-dependency-injection"></a>Inserción de dependencias de los componentes de Razor

Por [Rainer Stropek](https://www.timecockpit.com)

[Inserción de dependencias (DI)](/aspnet/core/fundamentals/dependency-injection) está integrada. Las aplicaciones pueden usar los servicios integrados por los insertan en componentes. Las aplicaciones también pueden definir los servicios personalizados y que estén disponibles a través de DI.

## <a name="dependency-injection"></a>Inserción de dependencias

Inserción de dependencias es una técnica para tener acceso a los servicios configurados en una ubicación central. Esto puede ser útil para:

* Compartir una única instancia de una clase de servicio a través de muchos componentes (conocido como un *singleton* service).
* Desacoplar componentes de las clases de servicio concreto en particular y solo hacen referencia a las abstracciones. Por ejemplo, una interfaz `IDataAccess` se implementa mediante una clase concreta `DataAccess`. Cuando un componente usa DI para recibir un `IDataAccess` implementación, el componente no es estrictamente con el tipo concreto. La implementación se puede intercambiar, quizás a una implementación ficticia en pruebas unitarias.

El sistema de DI es responsable de suministrar las instancias de servicios de componentes. Inserción de dependencias también resuelve las dependencias recursivamente para que los propios servicios pueden depender de otros servicios. Inserción de dependencias se configura durante el inicio de la aplicación. Más adelante en este tema se muestra un ejemplo.

## <a name="add-services-to-di"></a>Agregar servicios a la inserción de dependencias

Después de crear una nueva aplicación, examine el `Startup.ConfigureServices` método:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

El `ConfigureServices` se pasa al método un [IServiceCollection](/dotnet/api/microsoft.extensions.dependencyinjection.iservicecollection), que es una lista de objetos de descriptor de servicio ([ServiceDescriptor](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor)). Los servicios se agregan al proporcionar descriptores del servicio a la colección de servicios. Ejemplo de código siguiente muestra el concepto:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

Los servicios pueden configurarse con las duraciones siguientes:

| Método      | Descripción |
| ----------- | ----------- |
| [Singleton](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.singleton#Microsoft_Extensions_DependencyInjection_ServiceDescriptor_Singleton__1_System_Func_System_IServiceProvider___0__) | Inserción de dependencias se crea un *única instancia* del servicio. Todos los componentes que requieren este servicio reciben una referencia a esta instancia. |
| [Transitoria](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.transient) | Cada vez que un componente requiere este servicio, recibe un *nueva instancia* del servicio. |
| [Con ámbito](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.scoped) | Del lado cliente Blazor actualmente no tiene el concepto de ámbitos de DI. `Scoped` se comporta como `Singleton`. Sin embargo, los componentes de Razor de ASP.NET Core admiten la `Scoped` duración. En un componente de Razor, un registro de servicio con ámbito se limita a la conexión. Por este motivo, es preferible para los servicios que se deben acotar al usuario actual utilizando los servicios de ámbito (incluso si la intención actual es ejecutar el cliente en el explorador). |

El sistema de DI se basa en el sistema de DI en ASP.NET Core. Para obtener más información, consulte [inserción de dependencias en ASP.NET Core](/aspnet/core/fundamentals/dependency-injection).

## <a name="default-services"></a>Servicios predeterminados

Servicios predeterminados se agregan automáticamente a la colección de servicios de una aplicación. La siguiente tabla muestra algunos de los servicios útiles predeterminado proporcionados.

| Método       | Descripción |
| ------------ | ----------- |
| [HttpClient](/dotnet/api/system.net.http.httpclient) | Proporciona métodos para enviar solicitudes HTTP y recibir respuestas HTTP de un recurso identificado por un URI (singleton). Tenga en cuenta que esta instancia de `HttpClient` utiliza el explorador para controlar el tráfico HTTP en segundo plano. [HttpClient.BaseAddress](/dotnet/api/system.net.http.httpclient.baseaddress) se establece automáticamente en el prefijo URI base de la aplicación. `HttpClient` solo se proporciona a las aplicaciones de Blazor del lado cliente. |
| `IJSRuntime` | Representa una instancia de un tiempo de ejecución de JavaScript a la que se pueden enviar las llamadas. Para obtener más información, consulta <xref:razor-components/javascript-interop>. |
| `IUriHelper` | Aplicaciones auxiliares para trabajar con el estado de los URI y la navegación (singleton). `IUriHelper` se proporciona para ambas aplicaciones del lado cliente, Blazor y componentes de Razor de ASP.NET Core. |

Tenga en cuenta que es posible usar un proveedor de servicios personalizados en lugar del proveedor de servicio predeterminado que se agrega mediante la plantilla predeterminada. Un proveedor de servicios personalizada no proporciona automáticamente los servicios predeterminados que se muestran en la tabla. Esos servicios deben agregarse explícitamente para el nuevo proveedor de servicios.

## <a name="request-a-service-in-a-component"></a>Solicitar un servicio en un componente

Una vez que los servicios se agregan a la colección de servicios, puede insertarse en plantillas de Razor de los componentes mediante la `@inject` directiva Razor. `@inject` tiene dos parámetros:

* Nombre del tipo: El tipo del servicio que se va a insertar.
* Nombre de propiedad: El nombre de la propiedad recibe el servicio de aplicación insertado. Tenga en cuenta que la propiedad no requiere la creación manual. El compilador crea la propiedad.

Varios `@inject` instrucciones se pueden utilizar para insertar los diferentes servicios.

En el ejemplo siguiente se muestra cómo utilizar `@inject`. El servicio que implementa `Services.IDataAccess` se inserta en la propiedad del componente `DataRepository`. Tenga en cuenta cómo el código solo usa el `IDataAccess` abstracción:

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

Internamente, la propiedad generada (`DataRepository`) está decorada con el `InjectAttribute` atributo. Normalmente, este atributo no se usa directamente. Si es necesaria para los componentes de una clase base e insertadas propiedades también son necesarias para la clase base, `InjectAttribute` pueden agregarse manualmente:

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

En los componentes derivados de la clase base, el `@inject` directiva no es necesaria. El `InjectAttribute` de la clase base es suficiente:

```csharp
@page "/demo"
@inherits ComponentBase

<h1>...</h1>
...
```

## <a name="dependency-injection-in-services"></a>Inserción de dependencias en servicios

Servicios complejos pueden requerir servicios adicionales. En el ejemplo anterior, `DataAccess` podría requerir el `HttpClient` servicio predeterminado. `@inject` o `InjectAttribute` no pueden usarse en servicios. *Inserción de constructor* debe usarse en su lugar. Los servicios necesarios se agregan mediante la adición de parámetros al constructor del servicio. Cuando la inserción de dependencias, crea el servicio, reconoce los servicios que requiere en el constructor y las proporciona según corresponda.

Ejemplo de código siguiente muestra el concepto:

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

Tenga en cuenta los siguientes requisitos previos para la inyección de constructor:

* Debe haber un constructor cuyos argumentos pueden cumplir la inserción de dependencias. Tenga en cuenta que se permiten parámetros adicionales no cubiertos por la inserción de dependencias si se especifican los valores predeterminados para ellos.
* El constructor correspondiente debe ser *pública*.
* Solo debe haber un constructor aplicable. En el caso de ambigüedad, inserción de dependencias produce una excepción.

## <a name="additional-resources"></a>Recursos adicionales

* [Inserción de dependencias en ASP.NET Core](/aspnet/core/fundamentals/dependency-injection)
