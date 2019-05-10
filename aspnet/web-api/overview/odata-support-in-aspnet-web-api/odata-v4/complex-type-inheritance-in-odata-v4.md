---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Herencia de tipo complejo en OData v4 con ASP.NET Web API | Microsoft Docs
author: microsoft
description: Según la especificación de OData v4, un tipo complejo puede heredar de otro tipo complejo. (Un tipo complejo es un tipo estructurado sin una clave). API de Web...
ms.author: riande
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 3d90216c8e594055f77577eb6d8b1d978ae4c24d
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65132743"
---
# <a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="7a732-104">Herencia de tipo complejo en OData v4 con ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="7a732-104">Complex Type Inheritance in OData v4 with ASP.NET Web API</span></span>

<span data-ttu-id="7a732-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="7a732-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="7a732-106">Según el OData v4 [especificación](http://www.odata.org/documentation/odata-version-4-0/), un tipo complejo puede heredar de otro tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="7a732-106">According to the OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), a complex type can inherit from another complex type.</span></span> <span data-ttu-id="7a732-107">(Un *complejos* tipo es un tipo estructurado sin una clave.) Web API OData 5.3 admite la herencia de tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="7a732-107">(A *complex* type is a structured type without a key.) Web API OData 5.3 supports complex type inheritance.</span></span>
> 
> <span data-ttu-id="7a732-108">En este tema se muestra cómo crear un entity data model (EDM) con tipos de herencia compleja.</span><span class="sxs-lookup"><span data-stu-id="7a732-108">This topic shows how to build an entity data model (EDM) with complex inheritance types.</span></span> <span data-ttu-id="7a732-109">Para el código fuente completo, vea [ejemplos de herencia de tipos complejos de OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="7a732-109">For the complete source code, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="7a732-110">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="7a732-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="7a732-111">Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="7a732-111">Web API OData 5.3</span></span>
> - <span data-ttu-id="7a732-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="7a732-112">OData v4</span></span>

## <a name="model-hierarchy"></a><span data-ttu-id="7a732-113">Jerarquía del modelo</span><span class="sxs-lookup"><span data-stu-id="7a732-113">Model Hierarchy</span></span>

<span data-ttu-id="7a732-114">Para ilustrar la herencia de tipos complejos, vamos a usar la siguiente jerarquía de clases.</span><span class="sxs-lookup"><span data-stu-id="7a732-114">To illustrate complex type inheritance, we'll use the following class hierarchy.</span></span>

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

<span data-ttu-id="7a732-115">`Shape` es un tipo complejo abstracto.</span><span class="sxs-lookup"><span data-stu-id="7a732-115">`Shape` is an abstract complex type.</span></span> <span data-ttu-id="7a732-116">`Rectangle`, `Triangle`, y `Circle` se derivan de tipos complejos `Shape`, y `RoundRectangle` deriva `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="7a732-116">`Rectangle`, `Triangle`, and `Circle` are complex types derived from `Shape`, and `RoundRectangle` derives from `Rectangle`.</span></span> <span data-ttu-id="7a732-117">`Window` es un tipo de entidad y contiene un `Shape` instancia.</span><span class="sxs-lookup"><span data-stu-id="7a732-117">`Window` is an entity type and contains a `Shape` instance.</span></span>

<span data-ttu-id="7a732-118">Estas son las clases CLR que definen estos tipos.</span><span class="sxs-lookup"><span data-stu-id="7a732-118">Here are the CLR classes that define these types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a><span data-ttu-id="7a732-119">Generar el modelo EDM</span><span class="sxs-lookup"><span data-stu-id="7a732-119">Build the EDM Model</span></span>

<span data-ttu-id="7a732-120">Para crear el EDM, puede usar **ODataConventionModelBuilder**, que infiere las relaciones de herencia entre los tipos CLR.</span><span class="sxs-lookup"><span data-stu-id="7a732-120">To create the EDM, you can use **ODataConventionModelBuilder**, which infers the inheritance relationships from the CLR types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="7a732-121">También puede generar el EDM explícitamente, mediante **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="7a732-121">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span> <span data-ttu-id="7a732-122">Esto toma más código, pero le ofrece más control sobre el modelo EDM.</span><span class="sxs-lookup"><span data-stu-id="7a732-122">This takes more code, but gives you more control over the EDM.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="7a732-123">Estos dos ejemplos crea el mismo esquema EDM.</span><span class="sxs-lookup"><span data-stu-id="7a732-123">These two examples create the same EDM schema.</span></span>

## <a name="metadata-document"></a><span data-ttu-id="7a732-124">Documento de metadatos</span><span class="sxs-lookup"><span data-stu-id="7a732-124">Metadata Document</span></span>

<span data-ttu-id="7a732-125">Este es el documento de metadatos de OData, que muestra herencia de tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="7a732-125">Here is the OData metadata document, showing complex type inheritance.</span></span>

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

<span data-ttu-id="7a732-126">En el documento de metadatos, puede ver:</span><span class="sxs-lookup"><span data-stu-id="7a732-126">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="7a732-127">El `Shape` tipo complejo es abstracto.</span><span class="sxs-lookup"><span data-stu-id="7a732-127">The `Shape` complex type is abstract.</span></span>
- <span data-ttu-id="7a732-128">El `Rectangle`, `Triangle`, y `Circle` tipo complejo tiene el tipo base `Shape`.</span><span class="sxs-lookup"><span data-stu-id="7a732-128">The `Rectangle`, `Triangle`, and `Circle` complex type have the base type `Shape`.</span></span>
- <span data-ttu-id="7a732-129">El `RoundRectangle` tipo tiene el tipo base `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="7a732-129">The `RoundRectangle` type has the base type `Rectangle`.</span></span>

## <a name="casting-complex-types"></a><span data-ttu-id="7a732-130">Conversión de tipos complejos</span><span class="sxs-lookup"><span data-stu-id="7a732-130">Casting Complex Types</span></span>

<span data-ttu-id="7a732-131">Ahora se admite la conversión de tipos complejos.</span><span class="sxs-lookup"><span data-stu-id="7a732-131">Casting on complex types is now supported.</span></span> <span data-ttu-id="7a732-132">Por ejemplo, la siguiente consulta convierte un `Shape` a un `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="7a732-132">For example, the following query casts a `Shape` to a `Rectangle`.</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

<span data-ttu-id="7a732-133">Esta es la carga de respuesta:</span><span class="sxs-lookup"><span data-stu-id="7a732-133">Here's the response payload:</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
