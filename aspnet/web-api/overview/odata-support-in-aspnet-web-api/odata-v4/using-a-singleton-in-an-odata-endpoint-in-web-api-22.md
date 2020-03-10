---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Creación de un singleton en OData V4 con Web API 2,2 | Microsoft Docs
author: rick-anderson
description: En este tema se muestra cómo definir un singleton en un punto de conexión de OData en Web API 2,2.
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 218449c18759b306e425c55f8e7b573d837b4658
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504505"
---
# <a name="create-a-singleton-in-odata-v4-using-web-api-22"></a><span data-ttu-id="e55f7-103">Creación de un singleton en OData V4 mediante Web API 2,2</span><span class="sxs-lookup"><span data-stu-id="e55f7-103">Create a Singleton in OData v4 Using Web API 2.2</span></span>

<span data-ttu-id="e55f7-104">por Zoe Luo</span><span class="sxs-lookup"><span data-stu-id="e55f7-104">by Zoe Luo</span></span>

> <span data-ttu-id="e55f7-105">Tradicionalmente, solo se podía tener acceso a una entidad si estaba encapsulada dentro de un conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="e55f7-105">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="e55f7-106">Sin embargo, OData V4 proporciona dos opciones adicionales, singleton y contención, en las que se admite WebAPI 2,2.</span><span class="sxs-lookup"><span data-stu-id="e55f7-106">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>

<span data-ttu-id="e55f7-107">En este artículo se muestra cómo definir un singleton en un punto de conexión de OData en Web API 2,2.</span><span class="sxs-lookup"><span data-stu-id="e55f7-107">This article shows how to define a singleton in an OData endpoint in Web API 2.2.</span></span> <span data-ttu-id="e55f7-108">Para obtener información sobre qué es un singleton y cómo puede beneficiarse de su uso, consulte [uso de un singleton para definir la entidad especial](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span><span class="sxs-lookup"><span data-stu-id="e55f7-108">For information on what a singleton is and how you can benefit from using it, see [Using a singleton to define your special entity](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span></span> <span data-ttu-id="e55f7-109">Para crear un punto de conexión de OData V4 en Web API, consulte [crear un punto de conexión de oData V4 mediante ASP.NET Web API 2,2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="e55f7-109">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span> 

<span data-ttu-id="e55f7-110">Vamos a crear un singleton en el proyecto de API Web con el siguiente modelo de datos:</span><span class="sxs-lookup"><span data-stu-id="e55f7-110">We'll create a singleton in your Web API project using the following data model:</span></span>

![Modelo de datos](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

<span data-ttu-id="e55f7-112">Un singleton denominado `Umbrella` se definirá en función del tipo `Company`y un conjunto de entidades denominado `Employees` se definirá en función del tipo `Employee`.</span><span class="sxs-lookup"><span data-stu-id="e55f7-112">A singleton named `Umbrella` will be defined based on type `Company`, and an entity set named `Employees` will be defined based on type `Employee`.</span></span>

<span data-ttu-id="e55f7-113">La solución usada en este tutorial se puede descargar desde [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span><span class="sxs-lookup"><span data-stu-id="e55f7-113">The solution used in this tutorial can be downloaded from [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span></span>

## <a name="define-the-data-model"></a><span data-ttu-id="e55f7-114">Definir el modelo de datos</span><span class="sxs-lookup"><span data-stu-id="e55f7-114">Define the data model</span></span>

1. <span data-ttu-id="e55f7-115">Defina los tipos de CLR.</span><span class="sxs-lookup"><span data-stu-id="e55f7-115">Define the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. <span data-ttu-id="e55f7-116">Generar el modelo EDM basado en los tipos de CLR.</span><span class="sxs-lookup"><span data-stu-id="e55f7-116">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="e55f7-117">Aquí, `builder.Singleton<Company>("Umbrella")` indica al generador de modelos que cree un singleton denominado `Umbrella` en el modelo EDM.</span><span class="sxs-lookup"><span data-stu-id="e55f7-117">Here, `builder.Singleton<Company>("Umbrella")` tells the model builder to create a singleton named `Umbrella` in the EDM model.</span></span>

    <span data-ttu-id="e55f7-118">Los metadatos generados tendrán un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="e55f7-118">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    <span data-ttu-id="e55f7-119">A partir de los metadatos, podemos ver que la propiedad de navegación `Company` del conjunto de entidades `Employees` está enlazada a la `Umbrella`singleton.</span><span class="sxs-lookup"><span data-stu-id="e55f7-119">From the metadata we can see that the navigation property `Company` in the `Employees` entity set is bound to the singleton `Umbrella`.</span></span> <span data-ttu-id="e55f7-120">`ODataConventionModelBuilder`realiza automáticamente el enlace, ya que solo `Umbrella` tiene el tipo de `Company`.</span><span class="sxs-lookup"><span data-stu-id="e55f7-120">The binding is done automatically by `ODataConventionModelBuilder`, since only `Umbrella` has the `Company` type.</span></span> <span data-ttu-id="e55f7-121">Si hay alguna ambigüedad en el modelo, puede usar `HasSingletonBinding` para enlazar explícitamente una propiedad de navegación a un singleton. `HasSingletonBinding` tiene el mismo efecto que usar el atributo `Singleton` en la definición de tipo de CLR:</span><span class="sxs-lookup"><span data-stu-id="e55f7-121">If there is any ambiguity in the model, you can use `HasSingletonBinding` to explicitly bind a navigation property to a singleton; `HasSingletonBinding` has the same effect as using the `Singleton` attribute in the CLR type definition:</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a><span data-ttu-id="e55f7-122">Definir el controlador singleton</span><span class="sxs-lookup"><span data-stu-id="e55f7-122">Define the singleton controller</span></span>

<span data-ttu-id="e55f7-123">Al igual que el controlador EntitySet, el controlador singleton hereda de `ODataController`y el nombre del controlador singleton debe ser `[singletonName]Controller`.</span><span class="sxs-lookup"><span data-stu-id="e55f7-123">Like the EntitySet controller, the singleton controller inherits from `ODataController`, and the singleton controller name should be `[singletonName]Controller`.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

<span data-ttu-id="e55f7-124">Para controlar diferentes tipos de solicitudes, es necesario que las acciones estén predefinidas en el controlador.</span><span class="sxs-lookup"><span data-stu-id="e55f7-124">In order to handle different kinds of requests, actions are required to be pre-defined in the controller.</span></span> <span data-ttu-id="e55f7-125">El **enrutamiento de atributos** está habilitado de forma predeterminada en WebApi 2,2.</span><span class="sxs-lookup"><span data-stu-id="e55f7-125">**Attribute routing** is enabled by default in WebApi 2.2.</span></span> <span data-ttu-id="e55f7-126">Por ejemplo, para definir una acción para controlar las consultas `Revenue` desde `Company` mediante el enrutamiento de atributos, use lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="e55f7-126">For example, to define an action to handle querying `Revenue` from `Company` using attribute routing, use the following:</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

<span data-ttu-id="e55f7-127">Si no está dispuesto a definir atributos para cada acción, defina las acciones siguiendo las [convenciones de enrutamiento de oData](../odata-routing-conventions.md).</span><span class="sxs-lookup"><span data-stu-id="e55f7-127">If you are not willing to define attributes for each action, just define your actions following [OData Routing Conventions](../odata-routing-conventions.md).</span></span> <span data-ttu-id="e55f7-128">Dado que no se necesita una clave para consultar un singleton, las acciones definidas en el controlador singleton son ligeramente diferentes de las acciones definidas en el controlador EntitySet.</span><span class="sxs-lookup"><span data-stu-id="e55f7-128">Since a key is not required for querying a singleton, the actions defined in the singleton controller are slightly different from actions defined in the entityset controller.</span></span>

<span data-ttu-id="e55f7-129">Como referencia, a continuación se enumeran las firmas de método para cada definición de acción en el controlador singleton.</span><span class="sxs-lookup"><span data-stu-id="e55f7-129">For reference, method signatures for every action definition in the singleton controller are listed below.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

<span data-ttu-id="e55f7-130">Básicamente, esto es todo lo que necesita hacer en el lado del servicio.</span><span class="sxs-lookup"><span data-stu-id="e55f7-130">Basically, this is all you need to do on the service side.</span></span> <span data-ttu-id="e55f7-131">El [proyecto de ejemplo](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contiene todo el código de la solución y el cliente de oData que muestra cómo utilizar el singleton.</span><span class="sxs-lookup"><span data-stu-id="e55f7-131">The [sample project](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contains all of the code for the solution and the OData client that shows how to use the singleton.</span></span> <span data-ttu-id="e55f7-132">El cliente se crea siguiendo los pasos descritos en [creación de una aplicación cliente de oData V4](create-an-odata-v4-client-app.md).</span><span class="sxs-lookup"><span data-stu-id="e55f7-132">The client is built by following the steps in [Create an OData v4 Client App](create-an-odata-v4-client-app.md).</span></span>

<span data-ttu-id="e55f7-133">.</span><span class="sxs-lookup"><span data-stu-id="e55f7-133">.</span></span> 

<span data-ttu-id="e55f7-134">*Gracias a Leo hu en el contenido original de este artículo.*</span><span class="sxs-lookup"><span data-stu-id="e55f7-134">*Thanks to Leo Hu for the original content of this article.*</span></span>
