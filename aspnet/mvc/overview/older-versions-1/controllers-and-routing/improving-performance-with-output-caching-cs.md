---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
title: Mejorar el rendimiento con el almacenamientoC#en caché de resultados () | Microsoft Docs
author: microsoft
description: En este tutorial, obtendrá información sobre cómo puede mejorar drásticamente el rendimiento de las aplicaciones web MVC de ASP.NET aprovechando el almacenamiento en caché de la salida. Usted...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 521c9117-81cd-4d8d-9d96-0256dc7bf50f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
msc.type: authoredcontent
ms.openlocfilehash: 548c5bea2e9cf26e0574e72d2c0ea204dbd90f9c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486673"
---
# <a name="improving-performance-with-output-caching-c"></a>Mejorar el rendimiento con el almacenamiento en caché de resultados (C#)

por [Microsoft](https://github.com/microsoft)

> En este tutorial, obtendrá información sobre cómo puede mejorar drásticamente el rendimiento de las aplicaciones web MVC de ASP.NET aprovechando el almacenamiento en caché de la salida. Aprenderá a almacenar en caché el resultado devuelto por una acción del controlador para que no sea necesario crear el mismo contenido cada vez que un nuevo usuario invoque la acción.

El objetivo de este tutorial es explicar cómo puede mejorar drásticamente el rendimiento de una aplicación ASP.NET MVC aprovechando la memoria caché de resultados. La caché de resultados permite almacenar en caché el contenido devuelto por una acción de controlador. De este modo, no es necesario generar el mismo contenido cada vez que se invoca la misma acción de controlador.

Imagine, por ejemplo, que la aplicación ASP.NET MVC muestra una lista de registros de base de datos en una vista denominada index. Normalmente, cada y cada vez que un usuario invoca la acción de controlador que devuelve la vista de índice, el conjunto de registros de la base de datos debe recuperarse de la base de datos mediante la ejecución de una consulta de base de datos.

Por otro lado, si aprovecha la memoria caché de resultados, puede evitar ejecutar una consulta de base de datos cada vez que un usuario invoque la misma acción de controlador. La vista se puede recuperar de la memoria caché en lugar de volver a generarse desde la acción del controlador. El almacenamiento en caché le permite evitar el trabajo redundante en el servidor.

## <a name="enabling-output-caching"></a>Habilitar el almacenamiento en caché de resultados

Para habilitar el almacenamiento en caché de resultados, agregue un atributo [OutputCache] a una acción de controlador individual o a una clase de controlador completa. Por ejemplo, el controlador de la lista 1 expone una acción denominada index (). La salida de la acción index () se almacena en caché durante 10 segundos.

**Lista 1: Controllers\HomeController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample1.cs)]

En las versiones beta de ASP.NET MVC, el almacenamiento en caché de resultados no funciona para una dirección URL como [http://www.MySite.com/](http://www.mysite.com/). En su lugar, debe escribir una dirección URL como [http://www.MySite.com/Home/Index](http://www.mysite.com/Home/Index). 

En la lista 1, la salida de la acción index () se almacena en caché durante 10 segundos. Si lo prefiere, puede especificar una duración de caché mucho mayor. Por ejemplo, si desea almacenar en caché el resultado de una acción de controlador durante un día, puede especificar una duración de caché de 86400 segundos (60 segundos \* 60 minutos \* 24 horas).

No hay ninguna garantía de que el contenido se almacene en caché durante el período de tiempo que especifique. Cuando se reduce el nivel de recursos de memoria, la memoria caché comienza a expulsar el contenido automáticamente.

El controlador Home de la lista 1 devuelve la vista de índice en la lista 2. No hay nada especial sobre esta vista. La vista de índice simplemente muestra la hora actual (vea la ilustración 1).

**Lista 2: Views\Home\Index.aspx**

[!code-aspx[Main](improving-performance-with-output-caching-cs/samples/sample2.aspx)]

**Figura 1: vista de índice almacenada en caché**

![clip_image002](improving-performance-with-output-caching-cs/_static/image1.jpg)

Si invoca la acción index () varias veces escribiendo la dirección URL/Home/Index en la barra de direcciones del explorador y pulsando el botón actualizar/volver a cargar en el explorador varias veces, la hora que se muestra en la vista de índice no cambia durante 10 segundos. Se muestra la misma hora porque la vista se almacena en la memoria caché.

Es importante comprender que la misma vista se almacena en la memoria caché para todos los usuarios que visitan la aplicación. Cualquier persona que invoque la acción index () obtendrá la misma versión almacenada en caché de la vista de índice. Esto significa que la cantidad de trabajo que debe realizar el servidor web para atender la vista de índice se reduce drásticamente.

La vista de la lista 2 da lugar a algo realmente sencillo. La vista solo muestra la hora actual. Sin embargo, puede almacenar en caché fácilmente una vista que muestre un conjunto de registros de la base de datos. En ese caso, no es necesario recuperar el conjunto de registros de la base de datos cada vez que se invoca la acción de controlador que devuelve la vista. El almacenamiento en caché puede reducir la cantidad de trabajo que debe realizar el servidor Web y el servidor de base de datos.

No use la página &lt;% @ OutputCache%&gt; Directiva en una vista de MVC. Esta Directiva se sangra del mundo de formularios Web Forms y no debe usarse en una aplicación ASP.NET MVC.

## <a name="where-content-is-cached"></a>Dónde se almacena en caché el contenido

De forma predeterminada, cuando se usa el atributo [OutputCache], el contenido se almacena en la memoria caché en tres ubicaciones: el servidor Web, los servidores proxy y el explorador Web. Puede controlar exactamente dónde se almacena en caché el contenido modificando la propiedad Location del atributo [OutputCache].

Puede establecer la propiedad Location en cualquiera de los siguientes valores:

> · Cualquier
> 
> · Nº
> 
> · Posterior
> 
> · Servidor
> 
> · Ninguna
> 
> · ServerAndClient

De forma predeterminada, la propiedad Location tiene el valor any. Sin embargo, hay situaciones en las que puede que quiera almacenar en caché solo en el explorador o solo en el servidor. Por ejemplo, si está almacenando en caché información que está personalizada para cada usuario, no debe almacenar en caché la información en el servidor. Si va a mostrar información diferente a distintos usuarios, debe almacenar en caché la información solo en el cliente.

Por ejemplo, el controlador de la lista 3 expone una acción denominada GetName () que devuelve el nombre de usuario actual. Si Jack inicia sesión en el sitio web e invoca la acción GetName (), la acción devuelve la cadena "HI Jack". Si, posteriormente, Jill inicia sesión en el sitio web e invoca la acción GetName (), entonces también obtendrá la cadena "HI Jack". La cadena se almacena en la memoria caché del servidor web para todos los usuarios después de que Jack invoque inicialmente la acción del controlador.

**Lista 3: Controllers\BadUserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample3.cs)]

Lo más probable es que el controlador de la lista 3 no funcione de la forma que desee. No desea mostrar el mensaje "HI Jack" a Jill.

Nunca debe almacenar en caché el contenido personalizado en la memoria caché del servidor. Sin embargo, es posible que quiera almacenar en caché el contenido personalizado en la memoria caché del explorador para mejorar el rendimiento. Si almacena en caché el contenido en el explorador y un usuario invoca la misma acción del controlador varias veces, se puede recuperar el contenido desde la memoria caché del explorador en lugar de hacerlo desde el servidor.

El controlador modificado de la lista 4 almacena en caché el resultado de la acción GetName (). Sin embargo, el contenido se almacena en caché únicamente en el explorador y no en el servidor. De este modo, cuando varios usuarios invoquen el método GetName (), cada persona obtiene su propio nombre de usuario y no el nombre de usuario de otra persona.

**Lista 4: Controllers\UserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample4.cs)]

Observe que el atributo [OutputCache] de la lista 4 incluye una propiedad Location establecida en el valor OutputCacheLocation. Client. El atributo [OutputCache] también incluye una propiedad nostore. La propiedad nostore se usa para informar a los servidores proxy y al explorador que no deben almacenar una copia permanente del contenido almacenado en caché.

## <a name="varying-the-output-cache"></a>Variar la memoria caché de resultados

En algunas situaciones, puede que desee diferentes versiones en caché del mismo contenido. Imagine, por ejemplo, que va a crear una página maestra/detalle. En la página maestra se muestra una lista de títulos de películas. Al hacer clic en un título, obtendrá los detalles de la película seleccionada.

Si almacena en caché la página de detalles, se mostrarán los detalles de la misma película independientemente de la película en la que haga clic. La primera película seleccionada por el primer usuario se mostrará a todos los usuarios futuros.

Puede corregir este problema aprovechando las ventajas de la propiedad VaryByParam del atributo [OutputCache]. Esta propiedad le permite crear diferentes versiones en caché del mismo contenido cuando un parámetro de formulario o un parámetro de cadena de consulta varía.

Por ejemplo, el controlador de la lista 5 expone dos acciones denominadas Master () y Details (). La acción Master () devuelve una lista de títulos de películas y la acción Details () devuelve los detalles de la película seleccionada.

**Lista 5: Controllers\MoviesController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample5.cs)]

La acción Master () incluye una propiedad VaryByParam con el valor "none". Cuando se invoca la acción Master (), se devuelve la misma versión en caché de la vista maestra. Se omiten los parámetros de formulario o los parámetros de cadena de consulta (vea la figura 2).

**Figura 2: vista de/Movies/Master**

![clip_image004](improving-performance-with-output-caching-cs/_static/image2.jpg)

**Ilustración 3: vista de/Movies/Details**

![clip_image006](improving-performance-with-output-caching-cs/_static/image3.jpg)

La acción Details () incluye una propiedad VaryByParam con el valor "ID". Cuando se pasan valores diferentes del parámetro id a la acción del controlador, se generan diferentes versiones en caché de la vista de detalles.

Es importante comprender que el uso de la propiedad VaryByParam da como resultado más almacenamiento en caché y no menos. Se crea una versión en caché diferente de la vista de detalles para cada versión diferente del parámetro id.

Puede establecer la propiedad VaryByParam en los valores siguientes:

> \* = cree una versión en caché diferente cada vez que varíe un parámetro de cadena de consulta o formulario.
> 
> ninguno = no crear nunca diferentes versiones en caché
> 
> Lista de parámetros de punto y coma = crear diferentes versiones en caché siempre que el formato o los parámetros de cadena de consulta de la lista varíe

## <a name="creating-a-cache-profile"></a>Crear un perfil de caché

Como alternativa a la configuración de las propiedades de la caché de resultados mediante la modificación de las propiedades del atributo [OutputCache], puede crear un perfil de caché en el archivo de configuración Web (Web. config). La creación de un perfil de caché en el archivo de configuración web ofrece un par de ventajas importantes.

En primer lugar, configurando el almacenamiento en caché de resultados en el archivo de configuración Web, puede controlar cómo las acciones de controlador almacenan el contenido en una ubicación central. Puede crear un perfil de caché y aplicar el perfil a varios controladores o acciones de controlador.

En segundo lugar, puede modificar el archivo de configuración Web sin volver a compilar la aplicación. Si necesita deshabilitar el almacenamiento en caché para una aplicación que ya se ha implementado en producción, puede simplemente modificar los perfiles de caché definidos en el archivo de configuración Web. Los cambios en el archivo de configuración Web se detectarán automáticamente y se aplicarán.

Por ejemplo, la sección &lt;Caching&gt; web Configuration de la lista 6 define un perfil de caché denominado Cache1Hour. La sección &lt;Caching&gt; debe aparecer dentro de la sección &lt;System. Web&gt; de un archivo de configuración Web.

**Lista 6: sección de almacenamiento en caché de Web. config**

[!code-xml[Main](improving-performance-with-output-caching-cs/samples/sample6.xml)]

El controlador de la lista 7 muestra cómo puede aplicar el perfil Cache1Hour a una acción de controlador con el atributo [OutputCache].

**Listado 7: Controllers\ProfileController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample7.cs)]

Si invoca la acción index () expuesta por el controlador en la lista 7, se devolverá la misma hora durante 1 hora.

## <a name="summary"></a>Resumen

El almacenamiento en caché de resultados proporciona un método muy sencillo de mejorar drásticamente el rendimiento de las aplicaciones ASP.NET MVC. En este tutorial, aprendió a usar el atributo [OutputCache] para almacenar en caché la salida de las acciones de controlador. También aprendió a modificar las propiedades del atributo [OutputCache], como las propiedades Duration y VaryByParam para modificar el modo en que se almacena en caché el contenido. Por último, ha aprendido a definir perfiles de caché en el archivo de configuración Web.

> [!div class="step-by-step"]
> [Anterior](understanding-action-filters-cs.md)
> [Siguiente](adding-dynamic-content-to-a-cached-page-cs.md)
