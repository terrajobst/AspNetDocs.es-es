---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-details-and-delete-methods
title: Examinar los métodos details y DELETE | Microsoft Docs
author: Rick-Anderson
description: 'Nota: hay disponible una versión actualizada de este tutorial que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro, mucho más fácil de seguir y demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 11425ff3-09fc-4efa-be9a-b53bce503460
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 37787a26f37473b9d36792a9f7715982cb274074
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78485317"
---
# <a name="examining-the-details-and-delete-methods"></a>Examinar los métodos Details y Delete

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > [Hay disponible una](../../getting-started/introduction/getting-started.md) versión actualizada de este tutorial que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro, mucho más fácil de seguir y demuestra más características.

En esta parte del tutorial, examinará los métodos `Details` y `Delete` generados automáticamente.

## <a name="examining-the-details-and-delete-methods"></a>Examinar los métodos Details y Delete

Abra el controlador de `Movie` y examine el método `Details`.

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

El motor de scaffolding de MVC que creó este método de acción agrega un comentario que muestra una solicitud HTTP que invoca el método. En este caso, se trata de una solicitud `GET` con tres segmentos de dirección URL, el controlador de `Movies`, el método `Details` y un valor de `ID`.

Code First facilita la búsqueda de datos mediante el método `Find`. Una característica de seguridad importante integrada en el método es que el código comprueba que el método `Find` ha encontrado una película antes de que el código intente hacer nada con ella. Por ejemplo, un pirata informático podría introducir errores en el sitio cambiando la dirección URL creada por los vínculos de `http://localhost:xxxx/Movies/Details/1` a algo parecido a `http://localhost:xxxx/Movies/Details/12345` (o a algún otro valor que no represente una película real). Si no ha buscado una película nula, una película nula produciría un error de base de datos.

Examine los métodos `Delete` y `DeleteConfirmed`.

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

Tenga en cuenta que el método `HTTP Get``Delete` no elimina la película especificada, sino que devuelve una vista de la película en la que puede enviar (`HttpPost`) la eliminación. La acción de efectuar una operación de eliminación en respuesta a una solicitud GET (o con este propósito efectuar una operación de edición, creación o cualquier otra operación que modifique los datos) genera una vulnerabilidad de seguridad. Para obtener más información, consulte la entrada de blog de Stephen Walther [ASP.NET MVC Tip #46: no use los vínculos de eliminación porque crean carencias de seguridad](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

El método `HttpPost` que elimina los datos se denomina `DeleteConfirmed` para proporcionar al método HTTP POST una firma o nombre únicos. Las dos firmas de método se muestran a continuación:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

Common Language Runtime (CLR) requiere métodos sobrecargados para disponer de una firma de parámetro única (mismo nombre de método, pero lista de parámetros diferente). Sin embargo, aquí necesita dos métodos de eliminación: uno para GET y otro para POST, que ambos tienen la misma firma de parámetro. (ambos deben aceptar un número entero como parámetro).

Para ordenar esto, puede hacer un par de cosas. Uno es asignar nombres distintos a los métodos. que es lo que hizo el mecanismo de scaffolding en el ejemplo anterior. Pero esto implica un pequeño problema: ASP.NET asigna segmentos de una dirección URL a los métodos de acción por nombre y, si cambia el nombre de un método, normalmente el enrutamiento no podría encontrar ese método. La solución es la que ve en el ejemplo, que consiste en agregar el atributo `ActionName("Delete")` al método `DeleteConfirmed`. De este modo, se realiza la asignación del sistema de enrutamiento de forma eficaz para que una dirección URL que incluya <em>/Delete/</em>para una solicitud post busque el método `DeleteConfirmed`.

Otra forma habitual de evitar un problema con los métodos que tienen nombres y firmas idénticos es cambiar artificialmente la firma del método POST para incluir un parámetro no utilizado. Por ejemplo, algunos desarrolladores agregan un tipo de parámetro `FormCollection` que se pasa al método POST y, a continuación, simplemente no usan el parámetro:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a>Resumen

Ahora tiene una aplicación ASP.NET MVC completa que almacena los datos en una base de datos local. Puede crear, leer, actualizar, eliminar y buscar películas.

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a>Pasos siguientes

Después de compilar y probar una aplicación Web, el siguiente paso es ponerla a disposición de otras personas a través de Internet. Para ello, tiene que implementarlo en un proveedor de hospedaje Web. Microsoft ofrece hospedaje web gratuito para un máximo de 10 sitios web en una [cuenta de prueba gratuita de Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Le sugiero que siga el tutorial [implementación de una aplicación ASP.NET MVC segura con pertenencia, OAuth y SQL Database en un sitio web de Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Un tutorial excelente es el nivel intermedio de Tom Dykstra, que [crea un modelo de datos Entity Framework para una aplicación ASP.NET MVC](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). [Stackoverflow](http://stackoverflow.com/help) y los [foros de ASP.NET MVC](https://forums.asp.net/1146.aspx) son un buen lugar para formular preguntas. Síganos en Twitter para que pueda obtener [actualizaciones en los](https://twitter.com/RickAndMSFT) tutoriales más recientes.

Los comentarios son bienvenidos.

— [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT)  
: [Scott Hanselman](http://www.hanselman.com/blog/) twitter: [@shanselman](https://twitter.com/shanselman)

> [!div class="step-by-step"]
> [Anterior](adding-validation-to-the-model.md)
