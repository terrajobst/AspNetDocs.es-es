---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
title: Información general sobre el enrutamiento de ASP.NET MVC (VB) | Microsoft Docs
author: StephenWalther
description: En este tutorial, Stephen Walther muestra cómo el marco de ASP.NET MVC asigna las solicitudes del explorador a las acciones de controlador.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 4bc8d19a-80f1-44b4-adbf-95ed22d691ca
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: ed043d76b89ce31945cf3423b0c5afca9383cc21
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486901"
---
# <a name="aspnet-mvc-routing-overview-vb"></a>Información general sobre el enrutamiento de ASP.NET MVC (VB)

por [Stephen Walther](https://github.com/StephenWalther)

> En este tutorial, Stephen Walther muestra cómo el marco de ASP.NET MVC asigna las solicitudes del explorador a las acciones de controlador.

En este tutorial, se presenta una característica importante de cada aplicación de ASP.NET MVC denominada *enrutamiento de ASP.net*. El módulo de enrutamiento ASP.NET es responsable de asignar las solicitudes entrantes del explorador a determinadas acciones del controlador MVC. Al final de este tutorial, comprenderá cómo la tabla de rutas estándar asigna las solicitudes a las acciones de controlador.

## <a name="using-the-default-route-table"></a>Uso de la tabla de rutas predeterminada

Al crear una nueva aplicación ASP.NET MVC, la aplicación ya está configurada para usar el enrutamiento ASP.NET. El enrutamiento de ASP.NET se configura en dos lugares.

En primer lugar, se habilita el enrutamiento ASP.NET en el archivo de configuración Web de la aplicación (archivo Web. config). Hay cuatro secciones en el archivo de configuración que son relevantes para el enrutamiento: la sección System. Web. httpModules, la sección System. Web. httpHandlers, la sección System. WebServer. modules y la sección System. WebServer. Handlers. Tenga cuidado de no eliminar estas secciones porque sin estas secciones el enrutamiento dejará de funcionar.

En segundo lugar, y lo que es más importante, se crea una tabla de rutas en el archivo global. asax de la aplicación. El archivo global. asax es un archivo especial que contiene controladores de eventos para los eventos de ciclo de vida de la aplicación ASP.NET. La tabla de rutas se crea durante el evento de inicio de la aplicación.

El archivo de la lista 1 contiene el archivo global. asax predeterminado para una aplicación ASP.NET MVC.

**Lista 1: global. asax. VB**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample1.vb)]

Cuando se inicia por primera vez una aplicación MVC, se llama al método Start () de la aplicación\_. Este método, a su vez, llama al método RegisterRoutes (). El método RegisterRoutes () crea la tabla de rutas.

La tabla de rutas predeterminada contiene una única ruta (denominada predeterminada). La ruta predeterminada asigna el primer segmento de una dirección URL a un nombre de controlador, el segundo segmento de una dirección URL a una acción de controlador y el tercer segmento a un parámetro denominado **ID**.

Imagine que escribe la siguiente dirección URL en la barra de direcciones del explorador Web:

/Home/Index/3

La ruta predeterminada asigna esta dirección URL a los parámetros siguientes:

- controlador = Inicio

- Action = índice

- ID. = 3

Al solicitar la dirección URL/Home/Index/3, se ejecuta el siguiente código:

HomeController.Index(3)

La ruta predeterminada incluye los valores predeterminados para los tres parámetros. Si no proporciona un controlador, el parámetro Controller tiene como valor predeterminado el valor **Home**. Si no proporciona una acción, el parámetro de acción tiene como valor predeterminado el valor de **Índice**. Por último, si no proporciona un identificador, el parámetro ID tiene como valor predeterminado una cadena vacía.

Echemos un vistazo a algunos ejemplos de cómo la ruta predeterminada asigna las direcciones URL a las acciones de controlador. Imagine que escribe la siguiente dirección URL en la barra de direcciones del explorador:

/Home

Debido a los valores predeterminados de los parámetros de ruta predeterminados, al escribir esta dirección URL se llamará al método index () de la clase HomeController de la lista 2.

**Lista 2-HomeController. VB**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample2.vb)]

En la lista 2, la clase HomeController incluye un método denominado index () que acepta un único parámetro denominado ID. La dirección URL/Home hace que se llame al método index () con el valor Nothing como valor del parámetro id.

Debido a la manera en que el marco de MVC invoca las acciones del controlador, la dirección URL/Home también coincide con el método index () de la clase HomeController de la lista 3.

**Lista 3-HomeController. VB (acción de índice sin parámetro)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample3.vb)]

El método index () de la lista 3 no acepta ningún parámetro. La dirección URL/Home hará que se llame a este método index (). La dirección URL/Home/Index/3 también invoca este método (se omite el identificador).

La dirección URL/Home también coincide con el método index () de la clase HomeController en la lista 4.

**Lista 4-HomeController. VB (acción de índice con parámetro que acepta valores NULL)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample4.vb)]

En la lista 4, el método index () tiene un parámetro entero. Dado que el parámetro es un parámetro que acepta valores NULL (puede tener el valor Nothing), se puede llamar al índice () sin producir un error.

Por último, al invocar el método index () en la lista 5 con la URL/Home se produce una excepción, ya que el parámetro id *no es* un parámetro que acepta valores NULL. Si intenta invocar el método index (), obtendrá el error que se muestra en la figura 1.

**Lista 5-HomeController. VB (acción de índice con el parámetro id)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample5.vb)]

[![invocar una acción de controlador que espera un valor de parámetro](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)

**Figura 01**: invocación de una acción de controlador que espera un valor de parámetro ([haga clic para ver la imagen de tamaño completo](asp-net-mvc-routing-overview-vb/_static/image2.png))

La dirección URL/Home/Index/3, por otro lado, funciona perfectamente con la acción del controlador de índice en la lista 5. La solicitud/Home/Index/3 hace que se llame al método index () con un parámetro de identificador que tiene el valor 3.

## <a name="summary"></a>Resumen

El objetivo de este tutorial era proporcionarle una breve introducción al enrutamiento de ASP.NET. Hemos examinado la tabla de rutas predeterminada que obtiene con una nueva aplicación ASP.NET MVC. Ha aprendido cómo la ruta predeterminada asigna las direcciones URL a las acciones de controlador.

> [!div class="step-by-step"]
> [Anterior](creating-an-action-cs.md)
> [Siguiente](understanding-action-filters-vb.md)
