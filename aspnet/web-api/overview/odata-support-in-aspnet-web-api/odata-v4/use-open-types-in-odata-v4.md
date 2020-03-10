---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Abrir tipos en OData V4 con ASP.NET Web API | Microsoft Docs
author: microsoft
description: En OData V4, un tipo abierto es un tipo estructurado que contiene propiedades dinámicas, además de las propiedades que se declaran en la definición de tipo. Abrir...
ms.author: riande
ms.date: 09/15/2014
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 950442c071bf50d2c8c1588971f13f85c4891436
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504583"
---
# <a name="open-types-in-odata-v4-with-aspnet-web-api"></a>Tipos abiertos en OData V4 con ASP.NET Web API

por [Microsoft](https://github.com/microsoft)

> En OData V4, un *tipo abierto* es un tipo estructurado que contiene propiedades dinámicas, además de las propiedades que se declaran en la definición de tipo. Los tipos abiertos permiten agregar flexibilidad a los modelos de datos. En este tutorial se muestra cómo usar tipos abiertos en ASP.NET Web API OData.
> 
> En este tutorial se supone que ya sabe cómo crear un punto de conexión de OData en ASP.NET Web API. En caso contrario, empiece por leer primero [crear un punto de conexión de oData V4](create-an-odata-v4-endpoint.md) .
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - API Web OData 5,3
> - OData v4

En primer lugar, algunas terminología de OData:

- Tipo de entidad: un tipo estructurado con una clave.
- Tipo complejo: tipo estructurado sin clave.
- Open type: un tipo con propiedades dinámicas. Los tipos de entidad y los tipos complejos pueden estar abiertos.

El valor de una propiedad dinámica puede ser un tipo primitivo, un tipo complejo o un tipo de enumeración; o una colección de cualquiera de esos tipos. Para obtener más información sobre los tipos abiertos, vea la [especificación OData V4](http://www.odata.org/documentation/odata-version-4-0/).

## <a name="install-the-web-odata-libraries"></a>Instalación de las bibliotecas de web OData

Use el administrador de paquetes NuGet para instalar las bibliotecas de OData API Web más recientes. Desde la ventana de la consola del administrador de paquetes:

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a>Definir los tipos CLR

Empiece por definir los modelos de EDM como tipos CLR.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

Cuando se crea el Entity Data Model (EDM),

- `Category` es un tipo de enumeración.
- `Address` es un tipo complejo. (No tiene una clave, por lo que no es un tipo de entidad).
- `Customer` es un tipo de entidad. (Tiene una clave).
- `Press` es un tipo complejo abierto.
- `Book` es un tipo de entidad abierto.

Para crear un tipo abierto, el tipo CLR debe tener una propiedad de tipo `IDictionary<string, object>`, que contiene las propiedades dinámicas.

## <a name="build-the-edm-model"></a>Compilación del modelo EDM

Si usa **ODataConventionModelBuilder** para crear el EDM, `Press` y `Book` se agregan automáticamente como tipos abiertos, en función de la presencia de una propiedad `IDictionary<string, object>`.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

También puede compilar el EDM explícitamente mediante **ODataModelBuilder**.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a>Agregar un controlador OData

A continuación, agregue un controlador OData. En este tutorial, usaremos un controlador simplificado que solo admita solicitudes GET y POST, y usa una lista en memoria para almacenar entidades.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

Observe que la primera instancia de `Book` no tiene propiedades dinámicas. La segunda instancia de `Book` tiene las siguientes propiedades dinámicas:

- "Publicado": tipo primitivo
- "Authors": colección de tipos primitivos
- "OtherCategories": colección de tipos de enumeración.

Además, la propiedad `Press` de esa instancia de `Book` tiene las siguientes propiedades dinámicas:

- "Blog": tipo primitivo
- "Address": tipo complejo

## <a name="query-the-metadata"></a>Consultar los metadatos

Para obtener el documento de metadatos de OData, envíe una solicitud GET a `~/$metadata`. El cuerpo de la respuesta debe ser similar al siguiente:

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

En el documento de metadatos, puede ver lo siguiente:

- En el caso de los tipos `Book` y `Press`, el valor del atributo `OpenType` es true. Los tipos `Customer` y `Address` no tienen este atributo.
- El tipo de entidad `Book` tiene tres propiedades declaradas: ISBN, title y Press. Los metadatos de OData no incluyen la propiedad `Book.Properties` de la clase CLR.
- Del mismo modo, el tipo complejo de `Press` solo tiene dos propiedades declaradas: nombre y categoría. Los metadatos no incluyen la propiedad `Press.DynamicProperties` de la clase CLR.

## <a name="query-an-entity"></a>Consultar una entidad

Para que el libro con ISBN sea igual a "978-0-7356-7942-9", envíe una solicitud GET a `~/Books('978-0-7356-7942-9')`. El cuerpo de la respuesta debe ser similar al siguiente. (Con sangría para que sea más legible).

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

Observe que las propiedades dinámicas se incluyen alineadas con las propiedades declaradas.

## <a name="post-an-entity"></a>PUBLICAR una entidad

Para agregar una entidad de libro, envíe una solicitud POST a `~/Books`. El cliente puede establecer propiedades dinámicas en la carga de la solicitud.

A continuación se muestra una solicitud de ejemplo. Tenga en cuenta las propiedades "Price" y "Published".

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

Si establece un punto de interrupción en el método de controlador, puede ver que la API Web agregó estas propiedades al Diccionario de `Properties`.

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a>Recursos adicionales

[Ejemplo de Open type de OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
