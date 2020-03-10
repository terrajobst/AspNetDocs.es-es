---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Parte 2: capa de acceso a datos | Microsoft Docs'
author: JoeStagner
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks. La parte 2 cubre la adición de la capa de acceso a datos.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 342d2c54dfba5d052570e890f85dcf9739f9884f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78462655"
---
# <a name="part-2-data-access-layer"></a>Parte 2: capa de acceso a datos

por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks muestra cómo es extraordinariamente simple la creación de aplicaciones eficaces y escalables para la plataforma .NET. Muestra cómo usar las excelentes características nuevas de ASP.NET 4 para crear una tienda en línea, como compras, desprotección y administración.
> 
> En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks. La parte 2 cubre la adición de la capa de acceso a datos.

## <a id="_Toc260221668"></a>Agregar la capa de acceso a datos

Nuestra aplicación de comercio electrónico dependerá de dos bases de datos.

Para obtener información sobre el cliente, usaremos la base de datos de pertenencia de ASP.NET estándar. Para el carro de la compra y el catálogo de productos, se va a implementar una base de datos de SQL Express como se indica a continuación.

![](tailspin-spyworks-part-2/_static/image1.jpg)

Tras crear la base de datos (Commerce. MDF) en la aplicación de la aplicación\_la carpeta de datos, podemos crear nuestra capa de acceso a datos con .NET Entity Framework.

Vamos a crear una carpeta denominada "Data\_Access" y a hacer clic con el botón derecho en esa carpeta y seleccionar "Agregar nuevo elemento".

En el elemento "plantillas instaladas" y seleccione "ADO.NET Entity Data Model", escriba EDM\_Commerce. edmx como nombre y haga clic en el botón "agregar".

![](tailspin-spyworks-part-2/_static/image2.jpg)

Elija "generar desde la base de datos".

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

Guardar y compilar.

Ahora estamos preparados para agregar la primera característica: un menú de categoría de producto.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-1.md)
> [Siguiente](tailspin-spyworks-part-3.md)
