---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Contención en OData V4 con Web API 2,2 | Microsoft Docs
author: rick-anderson
description: 'Tradicionalmente, solo se podía tener acceso a una entidad si estaba encapsulada dentro de un conjunto de entidades. Sin embargo, OData V4 proporciona dos opciones adicionales: singleton y con...'
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 50050e40c4c42bf6d769d077c27864ee6417d4db
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78421405"
---
# <a name="containment-in-odata-v4-using-web-api-22"></a><span data-ttu-id="3f408-104">Contención en OData V4 con Web API 2,2</span><span class="sxs-lookup"><span data-stu-id="3f408-104">Containment in OData v4 Using Web API 2.2</span></span>

<span data-ttu-id="3f408-105">por Jinfu tan</span><span class="sxs-lookup"><span data-stu-id="3f408-105">by Jinfu Tan</span></span>

> <span data-ttu-id="3f408-106">Tradicionalmente, solo se podía tener acceso a una entidad si estaba encapsulada dentro de un conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="3f408-106">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="3f408-107">Sin embargo, OData V4 proporciona dos opciones adicionales, singleton y contención, en las que se admite WebAPI 2,2.</span><span class="sxs-lookup"><span data-stu-id="3f408-107">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>

<span data-ttu-id="3f408-108">En este tema se muestra cómo definir una contención en un punto de conexión de OData en WebApi 2,2.</span><span class="sxs-lookup"><span data-stu-id="3f408-108">This topic shows how to define a containment in an OData endpoint in WebApi 2.2.</span></span> <span data-ttu-id="3f408-109">Para obtener más información acerca de la contención, vea [contención está llegando a OData V4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span><span class="sxs-lookup"><span data-stu-id="3f408-109">For more information about containment, see [Containment is coming with OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span></span> <span data-ttu-id="3f408-110">Para crear un punto de conexión de OData V4 en Web API, consulte [crear un punto de conexión de oData V4 mediante ASP.NET Web API 2,2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="3f408-110">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span>

<span data-ttu-id="3f408-111">En primer lugar, vamos a crear un modelo de dominio de contención en el servicio de OData con este modelo de datos:</span><span class="sxs-lookup"><span data-stu-id="3f408-111">First, we'll create a containment domain model in the OData service, using this data model:</span></span>

![Modelo de datos](odata-containment-in-web-api-22/_static/image1.png)

<span data-ttu-id="3f408-113">Una cuenta contiene muchos PaymentInstruments (PI), pero no se define un conjunto de entidades para PI.</span><span class="sxs-lookup"><span data-stu-id="3f408-113">An account contains many PaymentInstruments (PI), but we don't define an entity set for a PI.</span></span> <span data-ttu-id="3f408-114">En su lugar, solo se puede tener acceso a la PI a través de una cuenta.</span><span class="sxs-lookup"><span data-stu-id="3f408-114">Instead, the PIs can only be accessed through an Account.</span></span>

<span data-ttu-id="3f408-115">Puede descargar la solución que se usa en este tema desde [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span><span class="sxs-lookup"><span data-stu-id="3f408-115">You can download the solution used in this topic from [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span></span>

## <a name="defining-the-data-model"></a><span data-ttu-id="3f408-116">Definir el modelo de datos</span><span class="sxs-lookup"><span data-stu-id="3f408-116">Defining the data model</span></span>

1. <span data-ttu-id="3f408-117">Defina los tipos de CLR.</span><span class="sxs-lookup"><span data-stu-id="3f408-117">Define the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    <span data-ttu-id="3f408-118">El atributo `Contained` se usa para las propiedades de navegación de contención.</span><span class="sxs-lookup"><span data-stu-id="3f408-118">The `Contained` attribute is used for containment navigation properties.</span></span>
2. <span data-ttu-id="3f408-119">Generar el modelo EDM basado en los tipos de CLR.</span><span class="sxs-lookup"><span data-stu-id="3f408-119">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="3f408-120">El `ODataConventionModelBuilder` controlará la creación del modelo EDM si el atributo `Contained` se agrega a la propiedad de navegación correspondiente.</span><span class="sxs-lookup"><span data-stu-id="3f408-120">The `ODataConventionModelBuilder` will handle building the EDM model if the `Contained` attribute is added to the corresponding navigation property.</span></span> <span data-ttu-id="3f408-121">Si la propiedad es un tipo de colección, también se creará una función `GetCount(string NameContains)`.</span><span class="sxs-lookup"><span data-stu-id="3f408-121">If the property is a collection type, a `GetCount(string NameContains)` function will also be created.</span></span>

    <span data-ttu-id="3f408-122">Los metadatos generados tendrán un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="3f408-122">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    <span data-ttu-id="3f408-123">El atributo `ContainsTarget` indica que la propiedad de navegación es una contención.</span><span class="sxs-lookup"><span data-stu-id="3f408-123">The `ContainsTarget` attribute indicates that the navigation property is a containment.</span></span>

## <a name="define-the-containing-entity-set-controller"></a><span data-ttu-id="3f408-124">Definir el controlador de conjunto de entidades contenedoras</span><span class="sxs-lookup"><span data-stu-id="3f408-124">Define the containing entity set controller</span></span>

<span data-ttu-id="3f408-125">Las entidades contenidas no tienen su propio controlador; la acción se define en el controlador de conjunto de entidades contenedoras.</span><span class="sxs-lookup"><span data-stu-id="3f408-125">Contained entities don't have their own controller; the action is defined in the containing entity set controller.</span></span> <span data-ttu-id="3f408-126">En este ejemplo, hay un AccountsController, pero no PaymentInstrumentsController.</span><span class="sxs-lookup"><span data-stu-id="3f408-126">In this sample, there is an AccountsController, but no PaymentInstrumentsController.</span></span>

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

<span data-ttu-id="3f408-127">Si la ruta de acceso de OData es de 4 o más segmentos, solo funciona el enrutamiento de atributos, como `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` en el controlador anterior.</span><span class="sxs-lookup"><span data-stu-id="3f408-127">If the OData path is 4 or more segments, only attribute routing works, such as `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` in the above controller.</span></span> <span data-ttu-id="3f408-128">De lo contrario, funciona tanto el atributo como el enrutamiento convencional: por ejemplo, `GetPayInPIs(int key)` coincide con `GET ~/Accounts(1)/PayinPIs`.</span><span class="sxs-lookup"><span data-stu-id="3f408-128">Otherwise, both attribute and conventional routing works: for instance, `GetPayInPIs(int key)` matches `GET ~/Accounts(1)/PayinPIs`.</span></span>

<span data-ttu-id="3f408-129">*Gracias a Leo hu en el contenido original de este artículo.*</span><span class="sxs-lookup"><span data-stu-id="3f408-129">*Thanks to Leo Hu for the original content of this article.*</span></span>
