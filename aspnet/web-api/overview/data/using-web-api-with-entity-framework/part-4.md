---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Control de las relaciones de entidad | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: be4948e5443a5eb4e1824c63dd0c445a7ee1928e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449125"
---
# <a name="handling-entity-relations"></a>Controlar las relaciones de entidad

por [Mike Wasson](https://github.com/MikeWasson)

[Descargar proyecto completado](https://github.com/MikeWasson/BookService)

En esta sección se describen algunos detalles de cómo EF carga las entidades relacionadas y cómo controlar las propiedades de navegación circulares en las clases de modelo. (En esta sección se proporciona información básica y no es necesario para completar el tutorial. Si lo prefiere, vaya a la [parte 5.](part-5.md).)

## <a name="eager-loading-versus-lazy-loading"></a>Carga diligente frente a carga diferida

Al usar EF con una base de datos relacional, es importante comprender cómo EF carga los datos relacionados.

También resulta útil ver las consultas SQL que genera EF. Para realizar el seguimiento de SQL, agregue la siguiente línea de código al constructor `BookServiceContext`:

[!code-csharp[Main](part-4/samples/sample1.cs)]

Si envía una solicitud GET a/API/Books, devuelve JSON similar al siguiente:

[!code-console[Main](part-4/samples/sample2.cmd)]

Puede ver que la propiedad Author es null, aunque el libro contenga una AuthorId válida. Esto se debe a que EF no está cargando las entidades de autor relacionadas. El registro de seguimiento de la consulta SQL confirma lo siguiente:

[!code-console[Main](part-4/samples/sample3.sql)]

La instrucción SELECT toma de la tabla Books y no hace referencia a la tabla Author.

Como referencia, este es el método de la clase `BooksController` que devuelve la lista de libros.

[!code-csharp[Main](part-4/samples/sample4.cs)]

Veamos cómo podemos devolver el autor como parte de los datos JSON. Hay tres maneras de cargar datos relacionados en Entity Framework: carga diligente, carga diferida y carga explícita. Existen ventajas e inconvenientes con cada técnica, por lo que es importante comprender cómo funcionan.

### <a name="eager-loading"></a>Carga diligente

Con la *carga diligente*, EF carga las entidades relacionadas como parte de la consulta de base de datos inicial. Para realizar la carga diligente, use el método de extensión **System. Data. Entity. include** .

[!code-csharp[Main](part-4/samples/sample5.cs)]

Esto indica a EF que incluya los datos de autor en la consulta. Si realiza este cambio y ejecuta la aplicación, ahora los datos JSON tienen el siguiente aspecto:

[!code-console[Main](part-4/samples/sample6.cmd)]

El registro de seguimiento muestra que EF realizó una combinación en las tablas Book y Author.

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a>Carga diferida

Con la carga diferida, EF carga automáticamente una entidad relacionada cuando se desreferencia la propiedad de navegación de esa entidad. Para habilitar la carga diferida, haga que la propiedad de navegación sea virtual. Por ejemplo, en la clase Book:

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

Ahora, considere el siguiente código:

[!code-csharp[Main](part-4/samples/sample9.cs)]

Cuando se habilita la carga diferida, el acceso a la propiedad `Author` en `books[0]` hace que EF consulte en la base de datos el autor.

La carga diferida requiere varios recorridos de base de datos, porque EF envía una consulta cada vez que recupera una entidad relacionada. Por lo general, desea deshabilitar la carga diferida para los objetos que se serializan. El serializador tiene que leer todas las propiedades del modelo, lo que desencadena la carga de las entidades relacionadas. Por ejemplo, estas son las consultas SQL cuando EF serializa la lista de libros con la carga diferida habilitada. Puede ver que EF realiza tres consultas independientes para los tres autores.

[!code-console[Main](part-4/samples/sample10.sql)]

Todavía hay ocasiones en las que podría querer usar la carga diferida. La carga diligente puede hacer que EF genere una combinación muy compleja. O bien, puede que necesite entidades relacionadas para un pequeño subconjunto de los datos y la carga diferida sería más eficaz.

Una manera de evitar problemas de serialización es serializar los objetos de transferencia de datos (DTO) en lugar de los objetos de entidad. Mostraré este enfoque más adelante en el artículo.

### <a name="explicit-loading"></a>Carga explícita

La carga explícita es similar a la carga diferida, salvo que se obtienen explícitamente los datos relacionados en el código. no se produce automáticamente cuando se obtiene acceso a una propiedad de navegación. La carga explícita le proporciona más control sobre cuándo cargar los datos relacionados, pero requiere código adicional. Para obtener más información sobre la carga explícita, vea [cargar entidades relacionadas](https://msdn.microsoft.com/data/jj574232#explicit).

## <a name="navigation-properties-and-circular-references"></a>Propiedades de navegación y referencias circulares

Al definir los modelos de libro y de autor, se define una propiedad de navegación en la clase `Book` para la relación de autor del libro, pero no se definió una propiedad de navegación en la otra dirección.

¿Qué ocurre si agrega la propiedad de navegación correspondiente a la clase `Author`?

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

Desafortunadamente, esto crea un problema al serializar los modelos. Si carga los datos relacionados, se crea un gráfico de objetos circulares.

![](part-4/_static/image1.png)

Cuando el formateador JSON o XML intenta serializar el gráfico, producirá una excepción. Los dos formateadores producen mensajes de excepción diferentes. Este es un ejemplo del formateador JSON:

[!code-console[Main](part-4/samples/sample12.cmd)]

Este es el formateador XML:

[!code-xml[Main](part-4/samples/sample13.xml)]

Una solución es usar dto, que se describe en la sección siguiente. Como alternativa, puede configurar los formateadores JSON y XML para controlar los ciclos del gráfico. Para obtener más información, vea [controlar referencias circulares de objetos](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).

En este tutorial, no necesita la propiedad de navegación `Author.Book`, por lo que puede dejarla fuera.

> [!div class="step-by-step"]
> [Anterior](part-3.md)
> [Siguiente](part-5.md)
