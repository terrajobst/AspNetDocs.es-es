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
# <a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="f357e-104">Herencia de tipos complejos en OData V4 con ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="f357e-104">Complex Type Inheritance in OData v4 with ASP.NET Web API</span></span>

<span data-ttu-id="f357e-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f357e-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="f357e-106">Según la [especificación](http://www.odata.org/documentation/odata-version-4-0/)de oData V4, un tipo complejo puede heredar de otro tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="f357e-106">According to the OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), a complex type can inherit from another complex type.</span></span> <span data-ttu-id="f357e-107">(Un tipo *complejo* es un tipo estructurado sin una clave). La API Web OData 5,3 admite la herencia de tipos complejos.</span><span class="sxs-lookup"><span data-stu-id="f357e-107">(A *complex* type is a structured type without a key.) Web API OData 5.3 supports complex type inheritance.</span></span>
> 
> <span data-ttu-id="f357e-108">En este tema se muestra cómo crear un Entity Data Model (EDM) con tipos de herencia complejos.</span><span class="sxs-lookup"><span data-stu-id="f357e-108">This topic shows how to build an entity data model (EDM) with complex inheritance types.</span></span> <span data-ttu-id="f357e-109">Para obtener el código fuente completo, vea [ejemplo de herencia de tipo complejo de oData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="f357e-109">For the complete source code, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f357e-110">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="f357e-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f357e-111">API Web OData 5,3</span><span class="sxs-lookup"><span data-stu-id="f357e-111">Web API OData 5.3</span></span>
> - <span data-ttu-id="f357e-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="f357e-112">OData v4</span></span>

## <a name="model-hierarchy"></a><span data-ttu-id="f357e-113">Jerarquía de modelos</span><span class="sxs-lookup"><span data-stu-id="f357e-113">Model Hierarchy</span></span>

<span data-ttu-id="f357e-114">Para ilustrar la herencia de tipos complejos, usaremos la siguiente jerarquía de clases.</span><span class="sxs-lookup"><span data-stu-id="f357e-114">To illustrate complex type inheritance, we'll use the following class hierarchy.</span></span>

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

<span data-ttu-id="f357e-115">`Shape` es un tipo complejo abstracto.</span><span class="sxs-lookup"><span data-stu-id="f357e-115">`Shape` is an abstract complex type.</span></span> <span data-ttu-id="f357e-116">`Rectangle`, `Triangle`y `Circle` son tipos complejos derivados de `Shape`y `RoundRectangle` deriva de `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="f357e-116">`Rectangle`, `Triangle`, and `Circle` are complex types derived from `Shape`, and `RoundRectangle` derives from `Rectangle`.</span></span> <span data-ttu-id="f357e-117">`Window` es un tipo de entidad y contiene una instancia de `Shape`.</span><span class="sxs-lookup"><span data-stu-id="f357e-117">`Window` is an entity type and contains a `Shape` instance.</span></span>

<span data-ttu-id="f357e-118">A continuación se muestran las clases de CLR que definen estos tipos.</span><span class="sxs-lookup"><span data-stu-id="f357e-118">Here are the CLR classes that define these types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a><span data-ttu-id="f357e-119">Compilación del modelo EDM</span><span class="sxs-lookup"><span data-stu-id="f357e-119">Build the EDM Model</span></span>

<span data-ttu-id="f357e-120">Para crear el EDM, puede usar **ODataConventionModelBuilder**, que deduce las relaciones de herencia de los tipos de CLR.</span><span class="sxs-lookup"><span data-stu-id="f357e-120">To create the EDM, you can use **ODataConventionModelBuilder**, which infers the inheritance relationships from the CLR types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="f357e-121">También puede compilar el EDM explícitamente mediante **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="f357e-121">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span> <span data-ttu-id="f357e-122">Esto requiere más código, pero le ofrece más control sobre el EDM.</span><span class="sxs-lookup"><span data-stu-id="f357e-122">This takes more code, but gives you more control over the EDM.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="f357e-123">En estos dos ejemplos se crea el mismo esquema de EDM.</span><span class="sxs-lookup"><span data-stu-id="f357e-123">These two examples create the same EDM schema.</span></span>

## <a name="metadata-document"></a><span data-ttu-id="f357e-124">Documento de metadatos</span><span class="sxs-lookup"><span data-stu-id="f357e-124">Metadata Document</span></span>

<span data-ttu-id="f357e-125">Este es el documento de metadatos de OData, que muestra la herencia de tipos complejos.</span><span class="sxs-lookup"><span data-stu-id="f357e-125">Here is the OData metadata document, showing complex type inheritance.</span></span>

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

<span data-ttu-id="f357e-126">En el documento de metadatos, puede ver lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f357e-126">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="f357e-127">El tipo complejo `Shape` es abstracto.</span><span class="sxs-lookup"><span data-stu-id="f357e-127">The `Shape` complex type is abstract.</span></span>
- <span data-ttu-id="f357e-128">Los `Rectangle`, `Triangle`y `Circle` tipo complejo tienen el tipo base `Shape`.</span><span class="sxs-lookup"><span data-stu-id="f357e-128">The `Rectangle`, `Triangle`, and `Circle` complex type have the base type `Shape`.</span></span>
- <span data-ttu-id="f357e-129">El tipo de `RoundRectangle` tiene el tipo base `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="f357e-129">The `RoundRectangle` type has the base type `Rectangle`.</span></span>

## <a name="casting-complex-types"></a><span data-ttu-id="f357e-130">Conversión de tipos complejos</span><span class="sxs-lookup"><span data-stu-id="f357e-130">Casting Complex Types</span></span>

<span data-ttu-id="f357e-131">Ahora se admite la conversión en tipos complejos.</span><span class="sxs-lookup"><span data-stu-id="f357e-131">Casting on complex types is now supported.</span></span> <span data-ttu-id="f357e-132">Por ejemplo, la consulta siguiente convierte una `Shape` en una `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="f357e-132">For example, the following query casts a `Shape` to a `Rectangle`.</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

<span data-ttu-id="f357e-133">Esta es la carga de respuesta:</span><span class="sxs-lookup"><span data-stu-id="f357e-133">Here's the response payload:</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
