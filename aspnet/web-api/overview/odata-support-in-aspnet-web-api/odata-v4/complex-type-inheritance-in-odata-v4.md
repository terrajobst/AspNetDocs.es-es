---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Herencia de tipos complejos en OData V4 con ASP.NET Web API | Microsoft Docs
author: microsoft
description: Según la especificación de OData V4, un tipo complejo puede heredar de otro tipo complejo. (Un tipo complejo es un tipo estructurado sin una clave). API Web...
ms.author: riande
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 3d90216c8e594055f77577eb6d8b1d978ae4c24d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448135"
---
# <a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>Herencia de tipos complejos en OData V4 con ASP.NET Web API

por [Microsoft](https://github.com/microsoft)

> Según la [especificación](http://www.odata.org/documentation/odata-version-4-0/)de oData V4, un tipo complejo puede heredar de otro tipo complejo. (Un tipo *complejo* es un tipo estructurado sin una clave). La API Web OData 5,3 admite la herencia de tipos complejos.
> 
> En este tema se muestra cómo crear un Entity Data Model (EDM) con tipos de herencia complejos. Para obtener el código fuente completo, vea [ejemplo de herencia de tipo complejo de oData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - API Web OData 5,3
> - OData v4

## <a name="model-hierarchy"></a>Jerarquía de modelos

Para ilustrar la herencia de tipos complejos, usaremos la siguiente jerarquía de clases.

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape` es un tipo complejo abstracto. `Rectangle`, `Triangle`y `Circle` son tipos complejos derivados de `Shape`y `RoundRectangle` deriva de `Rectangle`. `Window` es un tipo de entidad y contiene una instancia de `Shape`.

A continuación se muestran las clases de CLR que definen estos tipos.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>Compilación del modelo EDM

Para crear el EDM, puede usar **ODataConventionModelBuilder**, que deduce las relaciones de herencia de los tipos de CLR.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

También puede compilar el EDM explícitamente mediante **ODataModelBuilder**. Esto requiere más código, pero le ofrece más control sobre el EDM.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

En estos dos ejemplos se crea el mismo esquema de EDM.

## <a name="metadata-document"></a>Documento de metadatos

Este es el documento de metadatos de OData, que muestra la herencia de tipos complejos.

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

En el documento de metadatos, puede ver lo siguiente:

- El tipo complejo `Shape` es abstracto.
- Los `Rectangle`, `Triangle`y `Circle` tipo complejo tienen el tipo base `Shape`.
- El tipo de `RoundRectangle` tiene el tipo base `Rectangle`.

## <a name="casting-complex-types"></a>Conversión de tipos complejos

Ahora se admite la conversión en tipos complejos. Por ejemplo, la consulta siguiente convierte una `Shape` en una `Rectangle`.

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

Esta es la carga de respuesta:

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
