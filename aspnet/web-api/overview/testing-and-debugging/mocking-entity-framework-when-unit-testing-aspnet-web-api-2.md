---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Entity Framework ficticios cuando las pruebas unitarias ASP.NET Web API 2 | Microsoft Docs
author: Rick-Anderson
description: Esta guía y la aplicación demuestran cómo crear pruebas unitarias para la aplicación web API 2 que usa el Entity Framework. Muestra cómo modificar...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 258450107ee7443c4efd43a3b8e4851249745227
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447091"
---
# <a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a>Entity Framework ficticios cuando las pruebas unitarias ASP.NET Web API 2

por [Tom FitzMacken](https://github.com/tfitzmac)

[Descargar proyecto completado](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> Esta guía y la aplicación demuestran cómo crear pruebas unitarias para la aplicación web API 2 que usa el Entity Framework. Muestra cómo modificar el controlador scaffolding para habilitar el paso de un objeto de contexto para las pruebas y cómo crear objetos de prueba que funcionen con Entity Framework.
>
> Para ver una introducción a las pruebas unitarias con ASP.NET Web API, consulte [pruebas unitarias con ASP.net web API 2](unit-testing-with-aspnet-web-api.md).
>
> En este tutorial se da por supuesto que está familiarizado con los conceptos básicos de ASP.NET Web API. Para ver un tutorial introductorio, consulte [Introducción con ASP.net web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - API Web 2

## <a name="in-this-topic"></a>En este tema

Este tema contiene las siguientes secciones:

- [Requisitos previos](#prereqs)
- [Descargar código](#download)
- [Crear aplicación con proyecto de prueba unitaria](#appwithunittest)
- [Crear la clase de modelo](#modelclass)
- [Agregar el controlador](#controller)
- [Agregar inyección de dependencia](#dependency)
- [Instalación de paquetes NuGet en el proyecto de prueba](#testpackages)
- [Crear contexto de prueba](#testcontext)
- [Crear pruebas](#tests)
- [Ejecutar pruebas](#runtests)

Si ya ha completado los pasos de las [pruebas unitarias con ASP.net web API 2](unit-testing-with-aspnet-web-api.md), puede ir directamente a la sección [Agregar el controlador](#controller).

<a id="prereqs"></a>
## <a name="prerequisites"></a>Requisitos previos

Visual Studio 2017 Community, Professional o Enterprise Edition

<a id="download"></a>
## <a name="download-code"></a>Descarga de código

Descargue el [proyecto completado](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). El proyecto descargable incluye el código de prueba unitaria de este tema y el tema [ASP.net web API 2 de las pruebas unitarias](unit-testing-with-aspnet-web-api.md) .

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Crear aplicación con proyecto de prueba unitaria

Puede crear un proyecto de prueba unitaria al crear la aplicación o agregar un proyecto de prueba unitaria a una aplicación existente. En este tutorial se muestra cómo crear un proyecto de prueba unitaria al crear la aplicación.

Cree una nueva aplicación Web de ASP.NET denominada **StoreApp**.

En las nuevas ventanas del proyecto ASP.NET, seleccione la plantilla **vacía** y agregue las carpetas y las referencias básicas para la API Web. Seleccione la opción **Agregar pruebas unitarias** . El proyecto de prueba unitaria se denomina automáticamente **StoreApp. tests**. Puede mantener este nombre.

![crear proyecto de prueba unitaria](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

Después de crear la aplicación, verá que contiene dos proyectos: **StoreApp** y **StoreApp. tests**.

<a id="modelclass"></a>
## <a name="create-the-model-class"></a>Crear la clase de modelo

En el proyecto StoreApp, agregue un archivo de clase a la carpeta **Models** denominada **product.CS**. Reemplace el contenido del archivo por el código siguiente.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

Compile la solución.

<a id="controller"></a>
## <a name="add-the-controller"></a>Agregar el controlador

Haga clic con el botón secundario en la carpeta Controllers y seleccione **Agregar** y **nuevo elemento con scaffolding**. Seleccione controlador de Web API 2 con acciones mediante Entity Framework.

![Agregar nuevo controlador](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

Establezca los siguientes valores:

- Nombre del controlador: **ProductController**
- Clase de modelo: **producto**
- Clase de contexto de datos: [Seleccione el botón **nuevo contexto de datos** que rellenará los valores que se muestran a continuación]

![especificar controlador](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

Haga clic en **Agregar** para crear el controlador con código generado automáticamente. El código incluye métodos para crear, recuperar, actualizar y eliminar instancias de la clase product. En el código siguiente se muestra el método para agregar un producto. Observe que el método devuelve una instancia de **IHttpActionResult**.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

IHttpActionResult es una de las nuevas características de Web API 2 y simplifica el desarrollo de pruebas unitarias.

En la siguiente sección, personalizará el código generado para facilitar el paso de objetos de prueba al controlador.

<a id="dependency"></a>
## <a name="add-dependency-injection"></a>Agregar inyección de dependencia

Actualmente, la clase ProductController está codificada de forma rígida para usar una instancia de la clase StoreAppContext. Usará un patrón denominado inyección de dependencia para modificar la aplicación y quitar esa dependencia codificada de forma rígida. Al interrumpir esta dependencia, puede pasar un objeto ficticio al realizar las pruebas.

Haga clic con el botón secundario en la carpeta **modelos** y agregue una nueva interfaz denominada **IStoreAppContext**.

Reemplace el código por el siguiente código.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

Abra el archivo StoreAppContext.cs y realice los siguientes cambios resaltados. Los cambios importantes que se deben tener en cuenta son:

- La clase StoreAppContext implementa la interfaz IStoreAppContext
- El método MarkAsModified está implementado

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

Abra el archivo ProductController.cs. Cambie el código existente para que coincida con el código resaltado. Estos cambios interrumpen la dependencia de StoreAppContext y permiten que otras clases pasen un objeto diferente para la clase de contexto. Este cambio le permitirá pasar un contexto de prueba durante las pruebas unitarias.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

Hay un cambio más que debe hacer en ProductController. En el método **PutProduct** , reemplace la línea que establece el estado de la entidad a Modified con una llamada al método MarkAsModified.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

Compile la solución.

Ahora está listo para configurar el proyecto de prueba.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Instalación de paquetes NuGet en el proyecto de prueba

Cuando se usa la plantilla vacía para crear una aplicación, el proyecto de prueba unitaria (StoreApp. tests) no incluye ningún paquete NuGet instalado. Otras plantillas, como la plantilla de API Web, incluyen algunos paquetes NuGet en el proyecto de prueba unitaria. En este tutorial, debe incluir el paquete de Entity Framework y el paquete de Microsoft ASP.NET Web API 2 Core en el proyecto de prueba.

Haga clic con el botón derecho en el proyecto StoreApp. tests y seleccione **administrar paquetes NuGet**. Debe seleccionar el proyecto StoreApp. tests para agregar los paquetes a ese proyecto.

![administrar paquetes](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

En los paquetes en línea, busque e instale el paquete EntityFramework (versión 6,0 o posterior). Si parece que el paquete EntityFramework ya está instalado, es posible que haya seleccionado el proyecto StoreApp en lugar del proyecto StoreApp. tests.

![Agregar Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

Busque e instale Microsoft ASP.NET paquete principal de Web API 2.

![instalación del paquete principal de Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

Cierre la ventana administrar paquetes NuGet.

<a id="testcontext"></a>
## <a name="create-test-context"></a>Crear contexto de prueba

Agregue una clase denominada **TestDbSet** al proyecto de prueba. Esta clase actúa como la clase base para el conjunto de datos de prueba. Reemplace el código por el siguiente código.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

Agregue una clase denominada **TestProductDbSet** al proyecto de prueba que contiene el código siguiente.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

Agregue una clase denominada **TestStoreAppContext** y reemplace el código existente por el código siguiente.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a>Crear pruebas

De forma predeterminada, el proyecto de prueba incluye un archivo de prueba vacío denominado **UnitTest1.CS**. Este archivo muestra los atributos que se usan para crear métodos de prueba. En este tutorial, puede eliminar este archivo porque agregará una nueva clase de prueba.

Agregue una clase denominada **TestProductController** al proyecto de prueba. Reemplace el código por el siguiente código.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Ejecutar pruebas

Ahora está listo para ejecutar las pruebas. Se probará todo el método marcado con el atributo **TestMethod** . En el elemento de menú **prueba** , ejecute las pruebas.

![ejecución de pruebas](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

Abra la ventana **Explorador de pruebas** y observe los resultados de las pruebas.

![resultados de pruebas](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
