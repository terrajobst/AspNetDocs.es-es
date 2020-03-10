---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
title: Crear un controlador (C#) | Microsoft Docs
author: StephenWalther
description: En este tutorial, Stephen Walther muestra cómo puede Agregar un controlador a una aplicación ASP.NET MVC.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 719d50d4-2305-454c-98b4-bae64937c48f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
msc.type: authoredcontent
ms.openlocfilehash: 6e3d0bae7f07410637c2b06c500d94a02c821f5c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437653"
---
# <a name="creating-a-controller-c"></a>Crear un controlador (C#)

por [Stephen Walther](https://github.com/StephenWalther)

> En este tutorial, Stephen Walther muestra cómo puede Agregar un controlador a una aplicación ASP.NET MVC.

El objetivo de este tutorial es explicar cómo se pueden crear controladores ASP.NET MVC nuevos. Aprenderá a crear controladores con la opción de menú Agregar controlador de Visual Studio y mediante la creación de un archivo de clase a mano.

### <a name="using-the-add-controller-menu-option"></a>Uso de la opción de menú Agregar controlador

La forma más fácil de crear un nuevo controlador es hacer clic con el botón derecho en la carpeta Controllers de la ventana de Explorador de soluciones de Visual Studio y seleccionar la opción de menú **Agregar, controlador** (vea la ilustración 1). Al seleccionar esta opción de menú, se abre el cuadro de diálogo **Agregar controlador** (vea la figura 2).

[![el cuadro de diálogo nuevo proyecto](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)

**Figura 01**: adición de un nuevo controlador ([haga clic para ver la imagen de tamaño completo](creating-a-controller-cs/_static/image2.png))

[![el cuadro de diálogo nuevo proyecto](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)

**Figura 02**: cuadro de diálogo Agregar controlador ([haga clic para ver la imagen de tamaño completo](creating-a-controller-cs/_static/image4.png))

Observe que la primera parte del nombre del controlador se resalta en el cuadro de diálogo **Agregar controlador** . Cada nombre de controlador debe terminar con el *controlador*de sufijo. Por ejemplo, puede crear un controlador denominado *ProductController* pero no un controlador denominado *Product*.

Si crea un controlador que no tiene el sufijo del *controlador* , no podrá invocar el controlador. No haga esto--he perdido muchas horas de mi vida después de cometer este error.

**Lista 1-Controllers\ProductController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample1.cs)]

Siempre debe crear controladores en la carpeta Controllers. De lo contrario, infringirá las convenciones de ASP.NET MVC y otros desarrolladores tendrán un tiempo más difícil de entender su aplicación.

### <a name="scaffolding-action-methods"></a>Métodos de acción de scaffolding

Al crear un controlador, tiene la opción de generar automáticamente métodos de acción de creación, actualización y detalles (consulte la figura 3). Si selecciona esta opción, se genera la clase de controlador de la lista 2.

[![crear métodos de acción automáticamente](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)

**Figura 03**: creación automática de métodos de acción ([haga clic para ver la imagen de tamaño completo](creating-a-controller-cs/_static/image6.png))

**Lista 2-Controllers\CustomerController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample2.cs)]

Estos métodos generados son métodos de código auxiliar. Debe agregar la lógica real para crear, actualizar y mostrar los detalles de un cliente. Sin embargo, los métodos de código auxiliar proporcionan un buen punto de partida.

### <a name="creating-a-controller-class"></a>Crear una clase de controlador

El controlador ASP.NET MVC es simplemente una clase. Si lo prefiere, puede omitir el práctico scaffolding del controlador de Visual Studio y crear una clase de controlador a mano. Siga estos pasos:

1. Haga clic con el botón derecho en la carpeta Controllers y seleccione la opción de menú **Agregar, nuevo elemento** y seleccione la plantilla de **clase** (consulte la figura 4).
2. Asigne a la nueva clase el nombre PersonController.cs y haga clic en el botón **Agregar** .
3. Modifique el archivo de clase resultante para que la clase herede de la clase base System. Web. Mvc. Controller (vea la lista 3).

[![crear una nueva clase](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)

**Figura 04**: creación de una nueva clase ([haga clic para ver la imagen de tamaño completo](creating-a-controller-cs/_static/image8.png))

**Lista 3: Controllers\PersonController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample3.cs)]

El controlador de la lista 3 expone una acción denominada index () que devuelve la cadena "Hola mundo!". Para invocar esta acción del controlador, ejecute la aplicación y solicite una dirección URL similar a la siguiente:

`http://localhost:40071/Person`

> [!NOTE]
> 
> El Servidor de desarrollo de ASP.NET utiliza un número de Puerto aleatorio (por ejemplo, 40071). Al escribir una dirección URL para invocar un controlador, deberá proporcionar el número de puerto correcto. Puede determinar el número de puerto al mantener el mouse sobre el icono del Servidor de desarrollo de ASP.NET en el área de notificación de Windows (parte inferior derecha de la pantalla).
> 
> [!div class="step-by-step"]
> [Anterior](adding-dynamic-content-to-a-cached-page-cs.md)
> [Siguiente](creating-an-action-cs.md)
