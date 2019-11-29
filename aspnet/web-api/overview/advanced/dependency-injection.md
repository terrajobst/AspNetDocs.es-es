---
uid: web-api/overview/advanced/dependency-injection
title: Inserción de dependencias en ASP.NET Web API 2-ASP.NET 4. x
author: MikeWasson
description: En este tutorial se muestra cómo insertar dependencias en el controlador de ASP.NET Web API para ASP.NET 4. x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: f9c212af92168ac02644625b9aa8ec1bef329cab
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600420"
---
# <a name="dependency-injection-in-aspnet-web-api-2"></a>Inserción de dependencias en ASP.NET Web API 2

por [Mike Wasson](https://github.com/MikeWasson)

[Descargar proyecto completado](https://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> En este tutorial se muestra cómo insertar dependencias en el controlador de ASP.NET Web API.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - API Web 2
> - [Bloque de aplicaciones de Unity](https://www.nuget.org/packages/Unity/)
> - Entity Framework 6 (la versión 5 también funciona)

## <a name="what-is-dependency-injection"></a>¿Qué es la inserción de dependencias?

Una *dependencia* es cualquier objeto requerido por otro objeto. Por ejemplo, es habitual definir un [repositorio](http://martinfowler.com/eaaCatalog/repository.html) que controle el acceso a los datos. Vamos a ilustrar con un ejemplo. En primer lugar, definiremos un modelo de dominio:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

A continuación se muestra una clase de repositorio simple que almacena elementos en una base de datos mediante Entity Framework.

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Ahora vamos a definir un controlador de API Web que admita solicitudes GET para entidades `Product`. (He salido de POST y otros métodos de simplicidad). Este es un primer intento:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Tenga en cuenta que la clase del controlador depende de `ProductRepository`y de que el controlador cree la instancia de `ProductRepository`. Sin embargo, es una buena idea codificar la dependencia de esta manera, por varias razones.

- Si desea reemplazar `ProductRepository` por una implementación diferente, también debe modificar la clase de controlador.
- Si el `ProductRepository` tiene dependencias, debe configurarlas dentro del controlador. En un proyecto grande con varios controladores, el código de configuración se vuelve disperso en el proyecto.
- Es difícil realizar pruebas unitarias, ya que el controlador está codificado de forma rígida para consultar la base de datos. En el caso de una prueba unitaria, debe usar un repositorio ficticio o de código auxiliar, lo que no es posible con el diseño actual.

Podemos resolver estos problemas *insertando* el repositorio en el controlador. En primer lugar, refactorice la clase `ProductRepository` en una interfaz:

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

A continuación, proporcione el `IProductRepository` como un parámetro de constructor:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

En este ejemplo se usa la [inserción de constructores](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). También puede usar la *inyección de establecedor*, donde se establece la dependencia a través de un método o propiedad de establecedor.

Pero ahora hay un problema, porque la aplicación no crea el controlador directamente. La API Web crea el controlador cuando enruta la solicitud y la API Web no sabe nada sobre `IProductRepository`. Aquí es donde entra en el solucionador de dependencias de la API Web.

## <a name="the-web-api-dependency-resolver"></a>La resolución de dependencias de la API Web

Web API define la interfaz **objeto idependencyresolver** para resolver las dependencias. Esta es la definición de la interfaz:

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

La interfaz **IDependencyScope** tiene dos métodos:

- **GetService** crea una instancia de un tipo.
- **GetServices** crea una colección de objetos de un tipo especificado.

El método **objeto idependencyresolver** hereda **IDependencyScope** y agrega el método **BeginScope** . Hablaremos sobre los ámbitos más adelante en este tutorial.

Cuando la API Web crea una instancia del controlador, llama primero a **objeto idependencyresolver. GetService**, pasando el tipo de controlador. Puede usar este enlace de extensibilidad para crear el controlador y resolver las dependencias. Si **GetService** devuelve null, Web API busca un constructor sin parámetros en la clase de controlador.

## <a name="dependency-resolution-with-the-unity-container"></a>Resolución de dependencias con el contenedor de Unity

Aunque podría escribir una implementación completa de **objeto idependencyresolver** desde cero, la interfaz está diseñada realmente para actuar como puente entre la API Web y los contenedores de IOC existentes.

Un contenedor de IoC es un componente de software que es responsable de administrar las dependencias. Los tipos se registran con el contenedor y, a continuación, se usa el contenedor para crear objetos. El contenedor averigua automáticamente las relaciones de dependencia. Muchos contenedores de IoC también permiten controlar aspectos como la duración y el ámbito de los objetos.

> [!NOTE]
> "IoC" representa "inversion of control", que es un patrón general en el que un marco llama al código de la aplicación. Un contenedor de IoC crea los objetos automáticamente, lo que "invierte" el flujo de control habitual.

En este tutorial, usaremos [Unity](https://msdn.microsoft.com/library/ff647202.aspx) en Microsoft patterns &amp; Practices. (Otras bibliotecas populares incluyen el ejemplo de el [Windsor Windsor](http://www.castleproject.org/), [Spring.net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/)y [StructureMap](http://structuremap.github.io/documentation/)). Puede usar el administrador de paquetes NuGet para instalar Unity. En el menú **herramientas** de Visual Studio, seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**. En la ventana de la consola del administrador de paquetes, escriba el siguiente comando:

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

Esta es una implementación de **objeto idependencyresolver** que contiene un contenedor de Unity.

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> Si el método **GetService** no puede resolver un tipo, debe devolver **null**. Si el método **GetServices** no puede resolver un tipo, debe devolver un objeto de colección vacío. No inicie excepciones para tipos desconocidos.

## <a name="configuring-the-dependency-resolver"></a>Configuración de la resolución de dependencias

Establezca la resolución de dependencias en la propiedad **DependencyResolver** del objeto **HttpConfiguration** global.

En el código siguiente se registra la interfaz de `IProductRepository` con Unity y, a continuación, se crea un `UnityResolver`.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a>Ámbito de dependencia y duración del controlador

Los controladores se crean por solicitud. Para administrar la duración de los objetos, **objeto idependencyresolver** usa el concepto de un *ámbito*.

El solucionador de dependencias asociado al objeto **HttpConfiguration** tiene ámbito global. Cuando Web API crea un controlador, llama a **BeginScope**. Este método devuelve un **IDependencyScope** que representa un ámbito secundario.

A continuación, la API Web llama a **GetService** en el ámbito secundario para crear el controlador. Cuando se completa la solicitud, la API Web llama a **Dispose** en el ámbito secundario. Use el método **Dispose** para eliminar las dependencias del controlador.

La forma de implementar **BeginScope** depende del contenedor de IOC. Para Unity, el ámbito corresponde a un contenedor secundario:

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

La mayoría de los contenedores de IoC tienen equivalentes similares.
