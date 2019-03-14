---
title: Bibliotecas de clases de componentes de Razor
author: guardrex
description: Descubra cómo los componentes se pueden incluir en las aplicaciones de componentes de Razor de una biblioteca de componentes externos.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/09/2019
uid: razor-components/class-libraries
ms.openlocfilehash: 0e644627178bae2b8880760335860b3e0ebef156
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57037412"
---
# <a name="razor-components-class-libraries"></a>Bibliotecas de clases de componentes de Razor

Por [Simon Timms](https://github.com/stimms)

> [!NOTE]
> El SDK de .NET Core 3.0 Preview 2 no incluye una plantilla de proyecto para las bibliotecas de clases de componente de Razor, pero esperamos poder agregar una plantilla en una vista previa futura. En mientras tanto, puede usar la plantilla de biblioteca de clases de componente Blazor explicada en este tema.

Los componentes se pueden compartir en las bibliotecas de componentes entre proyectos. Los componentes se pueden incluidos desde:

* Otro proyecto en la solución.
* Un paquete de NuGet.
* Biblioteca de .NET que se hace referencia.

Como los componentes son tipos de .NET normales, las bibliotecas de componentes son ensamblados de .NET normales.

Para crear una nueva biblioteca de componentes, use el `blazorlib` plantilla con el [dotnet nuevo](/dotnet/core/tools/dotnet-new) comando. La plantilla forma parte de las plantillas instaladas cuando [configurar los componentes de Razor](xref:razor-components/get-started).

```console
dotnet new blazorlib -o MyComponentLib1
```

Para agregar la biblioteca a un proyecto existente, use el [dotnet sln](/dotnet/core/tools/dotnet-sln) comando:

```console
dotnet sln add .\MyComponentLib1
```

Las bibliotecas de componentes pueden contener los archivos estáticos, como imágenes, JavaScript y hojas de estilos. En tiempo de compilación, los archivos estáticos se incrustan en el archivo de ensamblado compilado (*.dll*), que permite el consumo de los componentes sin tener que preocuparse sobre cómo incluir sus recursos. Los archivos incluidos en el `content` directory se marcan como recurso incrustado. 

## <a name="consume-a-library-component"></a>Consumir un componente de la biblioteca

Para consumir los componentes definidos en una biblioteca en otro proyecto, el [ @addTagHelper ](/aspnet/core/mvc/views/tag-helpers/intro#add-helper-label) se debe usar la directiva. Los componentes individuales se pueden agregar por nombre. Por ejemplo, se agrega la siguiente directiva `Component1` de `MyComponentLib1`:

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

El formato general de la directiva es:

```cshtml
@addTagHelper <namespaced component name>, <assembly name>
```

Sin embargo, es común para incluir todos los componentes de un ensamblado con un carácter comodín:

```cshtml
@addTagHelper *, MyComponentLib1
```

El `@addTagHelper` directiva puede incluirse en *_ViewImport.cshtml* para que los componentes disponibles para un proyecto completo o aplicada a una sola página o un conjunto de páginas dentro de una carpeta. Con el `@addTagHelper` la directiva en su lugar, los componentes de la biblioteca de componentes pueden utilizarse como si estuvieran en el mismo ensamblado que la aplicación. 

## <a name="build-pack-and-ship-to-nuget"></a>Compilación, pack y envío de NuGet

Dado que las bibliotecas de componentes son bibliotecas de .NET standard, empaquetado y envío a NuGet no es diferente de empaquetado y envío de cualquier biblioteca en NuGet. Empaquetado se realiza mediante el [dotnet pack](/dotnet/core/tools/dotnet-pack) comando:

```console
dotnet pack
```

Cargar el paquete en NuGet mediante la [publicar dotnet nuget](/dotnet/core/tools/dotnet-nuget-push) comando:

```console
dotnet nuget publish
```

Cualquier incluye recursos estáticos se incluyen en el paquete NuGet. Los consumidores de la biblioteca reciben automáticamente los scripts y hojas de estilos, por lo que no están necesarios instalar manualmente los recursos a los consumidores.
