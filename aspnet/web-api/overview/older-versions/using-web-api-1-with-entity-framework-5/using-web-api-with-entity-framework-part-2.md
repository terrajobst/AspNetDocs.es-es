---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Parte 2: creación de los modelos de dominio | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 7c5ed1bdb4b390c94907b14e208231f16ad42d96
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504277"
---
# <a name="part-2-creating-the-domain-models"></a>Parte 2: creación de los modelos de dominio

por [Mike Wasson](https://github.com/MikeWasson)

[Descargar proyecto completado](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a>Agregar modelos

Hay tres formas de abordar Entity Framework:

- Database-First: comienza con una base de datos y Entity Framework genera el código.
- Modelo-primero: comienza con un modelo visual y Entity Framework genera la base de datos y el código.
- Code-First: comienza con el código y Entity Framework genera la base de datos.

Estamos usando el enfoque de Code First, por lo que comenzaremos definiendo los objetos de dominio como POCO (objetos CLR antiguos sin formato). Con el enfoque del código primero, los objetos de dominio no necesitan ningún código adicional para admitir el nivel de base de datos, como transacciones o persistencia. (En concreto, no es necesario heredar de la clase [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) ). Todavía puede usar anotaciones de datos para controlar cómo crea Entity Framework el esquema de la base de datos.

Dado que los POCO no incluyen ninguna propiedad adicional que describa el estado de la [base de datos](https://msdn.microsoft.com/library/system.data.entitystate.aspx), se pueden serializar fácilmente a JSON o XML. Sin embargo, eso no significa que siempre se expongan los modelos de Entity Framework directamente a los clientes, como veremos más adelante en el tutorial.

Se crearán los siguientes POCO:

- Producto
- Ordenar
- OrderDetail

Para crear cada clase, haga clic con el botón secundario en la carpeta models en Explorador de soluciones. En el menú contextual, seleccione **Agregar** y, a continuación, seleccione **clase.**

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

Agregue una clase `Product` con la siguiente implementación:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

Por Convención, Entity Framework usa la propiedad `Id` como clave principal y la asigna a una columna de identidad en la tabla de base de datos. Cuando se crea una nueva instancia de `Product`, no se establece un valor para `Id`, porque la base de datos genera el valor.

El atributo **ScaffoldColumn** indica a ASP.NET MVC que omita la propiedad `Id` al generar un formulario del editor. El atributo **required** se utiliza para validar el modelo. Especifica que la propiedad `Name` debe ser una cadena no vacía.

Agregue la clase `Order`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

Agregue la clase `OrderDetail`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a>Relaciones de clave externa

Un pedido contiene muchos detalles de pedidos y cada detalle de pedido hace referencia a un único producto. Para representar estas relaciones, la clase `OrderDetail` define propiedades denominadas `OrderId` y `ProductId`. Entity Framework deducirá que estas propiedades representan claves externas y agregará restricciones de clave externa a la base de datos.

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

Las clases `Order` y `OrderDetail` también incluyen propiedades de "navegación", que contienen referencias a los objetos relacionados. Dado un pedido, puede navegar a los productos en el orden siguiendo las propiedades de navegación.

Compile el proyecto ahora. Entity Framework usa la reflexión para detectar las propiedades de los modelos, por lo que requiere un ensamblado compilado para crear el esquema de la base de datos.

## <a name="configure-the-media-type-formatters"></a>Configurar los formateadores de tipo de medio

Un [formateador de tipo de medio](../../formats-and-model-binding/media-formatters.md) es un objeto que serializa los datos cuando la API Web escribe el cuerpo de respuesta http. Los formateadores integrados admiten la salida de JSON y XML. De forma predeterminada, ambos formateadores serializan todos los objetos por valor.

La serialización por valor crea un problema si un gráfico de objetos contiene referencias circulares. Eso es exactamente el caso de las clases `Order` y `OrderDetail`, porque cada una contiene una referencia a la otra. El formateador seguirá las referencias, escribirá cada objeto por valor y irá en círculos. Por lo tanto, es necesario cambiar el comportamiento predeterminado.

En Explorador de soluciones, expanda la carpeta app\_Start y abra el archivo denominado WebApiConfig.cs. Agregue el código siguiente a la clase `WebApiConfig`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

Este código establece el formateador JSON para conservar las referencias de objeto y quita completamente el formateador XML de la canalización. (Puede configurar el formateador XML para conservar las referencias a objetos, pero es un poco más trabajo y solo necesitamos JSON para esta aplicación. Para obtener más información, vea [controlar referencias circulares de objetos](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)).

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-1.md)
> [Siguiente](using-web-api-with-entity-framework-part-3.md)
