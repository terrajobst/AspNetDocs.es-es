---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: Pruebas unitarias ASP.NET Web API 2 | Microsoft Docs
author: Rick-Anderson
description: En esta guía y en la aplicación se muestra cómo crear pruebas unitarias simples para la aplicación web API 2. En este tutorial se muestra cómo incluir un proj de prueba unitaria...
ms.author: riande
ms.date: 06/05/2014
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f2d60b977475e048a3a74aabff4adc768ee22baf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78446989"
---
# <a name="unit-testing-aspnet-web-api-2"></a>Pruebas unitarias ASP.NET Web API 2

por [Tom FitzMacken](https://github.com/tfitzmac)

[Descargar proyecto completado](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> En esta guía y en la aplicación se muestra cómo crear pruebas unitarias simples para la aplicación web API 2. En este tutorial se muestra cómo incluir un proyecto de prueba unitaria en la solución y cómo escribir métodos de prueba que comprueben los valores devueltos de un método de controlador.
>
> En este tutorial se da por supuesto que está familiarizado con los conceptos básicos de ASP.NET Web API. Para ver un tutorial introductorio, consulte [Introducción con ASP.net web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
>
> Las pruebas unitarias de este tema se limitan intencionalmente a escenarios de datos simples. Para realizar pruebas unitarias de escenarios de datos más avanzados, consulte [Entity Framework ficticios cuando las pruebas unitarias ASP.net web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
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
    - [Agregar proyecto de prueba unitaria al crear la aplicación](#whencreate)
    - [Agregar un proyecto de prueba unitaria a una aplicación existente](#addtoexisting)
- [Configuración de la aplicación web API 2](#setupproject)
- [Instalación de paquetes NuGet en el proyecto de prueba](#testpackages)
- [Crear pruebas](#tests)
- [Ejecutar pruebas](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a>Requisitos previos

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional o Enterprise Edition

<a id="download"></a>
## <a name="download-code"></a>Descarga de código

Descargue el [proyecto completado](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). El proyecto descargable incluye el código de prueba unitaria de este tema y el [Entity Framework ficticio cuando se ASP.net web API tema de pruebas unitarias](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) .

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Crear aplicación con proyecto de prueba unitaria

Puede crear un proyecto de prueba unitaria al crear la aplicación o agregar un proyecto de prueba unitaria a una aplicación existente. En este tutorial se muestran los dos métodos para crear un proyecto de prueba unitaria. Para seguir este tutorial, puede usar cualquiera de estos enfoques.

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a>Agregar proyecto de prueba unitaria al crear la aplicación

Cree una nueva aplicación Web de ASP.NET denominada **StoreApp**.

![creación de proyecto](unit-testing-with-aspnet-web-api/_static/image1.png)

En las nuevas ventanas del proyecto ASP.NET, seleccione la plantilla **vacía** y agregue las carpetas y las referencias básicas para la API Web. Seleccione la opción **Agregar pruebas unitarias** . El proyecto de prueba unitaria se denomina automáticamente **StoreApp. tests**. Puede mantener este nombre.

![crear proyecto de prueba unitaria](unit-testing-with-aspnet-web-api/_static/image2.png)

Después de crear la aplicación, verá que contiene dos proyectos.

![dos proyectos](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a>Agregar un proyecto de prueba unitaria a una aplicación existente

Si no creó el proyecto de prueba unitaria al crear la aplicación, puede Agregar una en cualquier momento. Por ejemplo, supongamos que ya tiene una aplicación denominada StoreApp y desea agregar pruebas unitarias. Para agregar un proyecto de prueba unitaria, haga clic con el botón derecho en la solución y seleccione **Agregar** y **nuevo proyecto**.

![Agregar nuevo proyecto a la solución](unit-testing-with-aspnet-web-api/_static/image4.png)

Seleccione **prueba** en el panel izquierdo y, a continuación, seleccione **proyecto de prueba unitaria** para el tipo de proyecto. Asigne al proyecto el nombre **StoreApp. tests**.

![Agregar proyecto de prueba unitaria](unit-testing-with-aspnet-web-api/_static/image5.png)

Verá el proyecto de prueba unitaria en la solución.

En el proyecto de prueba unitaria, agregue una referencia de proyecto al proyecto original.

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a>Configuración de la aplicación web API 2

En el proyecto StoreApp, agregue un archivo de clase a la carpeta **Models** denominada **product.CS**. Reemplace el contenido del archivo por el código siguiente.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

Compile la solución.

Haga clic con el botón secundario en la carpeta Controllers y seleccione **Agregar** y **nuevo elemento con scaffolding**. Seleccione **controlador de Web API 2-vacío**.

![Agregar nuevo controlador](unit-testing-with-aspnet-web-api/_static/image6.png)

Establezca el nombre del controlador en **SimpleProductController**y haga clic en **Agregar**.

![especificar controlador](unit-testing-with-aspnet-web-api/_static/image7.png)

Reemplace el código existente por el siguiente código. Para simplificar este ejemplo, los datos se almacenan en una lista en lugar de en una base de datos. La lista definida en esta clase representa los datos de producción. Observe que el controlador incluye un constructor que toma como parámetro una lista de objetos product. Este constructor le permite pasar datos de prueba al realizar pruebas unitarias. El controlador también incluye dos métodos **asincrónicos** para ilustrar métodos asincrónicos de pruebas unitarias. Estos métodos asincrónicos se implementan mediante una llamada a **Task. FromResult** para minimizar el código extraño, pero normalmente los métodos incluyen operaciones que consumen muchos recursos.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

El método GetProduct devuelve una instancia de la interfaz **IHttpActionResult** . IHttpActionResult es una de las nuevas características de Web API 2 y simplifica el desarrollo de pruebas unitarias. Las clases que implementan la interfaz IHttpActionResult se encuentran en el espacio de nombres [System. Web. http. Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) . Estas clases representan las posibles respuestas de una solicitud de acción y se corresponden con códigos de Estado HTTP.

Compile la solución.

Ahora está listo para configurar el proyecto de prueba.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Instalación de paquetes NuGet en el proyecto de prueba

Cuando se usa la plantilla vacía para crear una aplicación, el proyecto de prueba unitaria (StoreApp. tests) no incluye ningún paquete NuGet instalado. Otras plantillas, como la plantilla de API Web, incluyen algunos paquetes NuGet en el proyecto de prueba unitaria. En este tutorial, debe incluir el paquete de Microsoft ASP.NET Web API 2 Core en el proyecto de prueba.

Haga clic con el botón derecho en el proyecto StoreApp. tests y seleccione **administrar paquetes NuGet**. Debe seleccionar el proyecto StoreApp. tests para agregar los paquetes a ese proyecto.

![administrar paquetes](unit-testing-with-aspnet-web-api/_static/image8.png)

Busque e instale Microsoft ASP.NET paquete principal de Web API 2.

![instalación del paquete principal de Web API](unit-testing-with-aspnet-web-api/_static/image9.png)

Cierre la ventana administrar paquetes NuGet.

<a id="tests"></a>
## <a name="create-tests"></a>Crear pruebas

De forma predeterminada, el proyecto de prueba incluye un archivo de prueba vacío denominado UnitTest1.cs. Este archivo muestra los atributos que se usan para crear métodos de prueba. En el caso de las pruebas unitarias, puede usar este archivo o crear su propio archivo.

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

En este tutorial, creará su propia clase de prueba. Puede eliminar el archivo UnitTest1.cs. Agregue una clase denominada **TestSimpleProductController.CS**y reemplace el código por el código siguiente.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Ejecutar pruebas

Ahora está listo para ejecutar las pruebas. Se probará todo el método marcado con el atributo **TestMethod** . En el elemento de menú **prueba** , ejecute las pruebas.

![ejecución de pruebas](unit-testing-with-aspnet-web-api/_static/image11.png)

Abra la ventana **Explorador de pruebas** y observe los resultados de las pruebas.

![resultados de pruebas](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a>Resumen

Ha completado este tutorial. Los datos de este tutorial se han simplificado intencionalmente para centrarse en las condiciones de las pruebas unitarias. Para realizar pruebas unitarias de escenarios de datos más avanzados, consulte [Entity Framework ficticios cuando las pruebas unitarias ASP.net web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
