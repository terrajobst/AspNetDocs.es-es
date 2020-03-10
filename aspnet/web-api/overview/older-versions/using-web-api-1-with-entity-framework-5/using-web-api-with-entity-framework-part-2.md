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
# <a name="part-2-creating-the-domain-models"></a><span data-ttu-id="28697-102">Parte 2: creación de los modelos de dominio</span><span class="sxs-lookup"><span data-stu-id="28697-102">Part 2: Creating the Domain Models</span></span>

<span data-ttu-id="28697-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="28697-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="28697-104">Descargar proyecto completado</span><span class="sxs-lookup"><span data-stu-id="28697-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a><span data-ttu-id="28697-105">Agregar modelos</span><span class="sxs-lookup"><span data-stu-id="28697-105">Add Models</span></span>

<span data-ttu-id="28697-106">Hay tres formas de abordar Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="28697-106">There are three ways to approach Entity Framework:</span></span>

- <span data-ttu-id="28697-107">Database-First: comienza con una base de datos y Entity Framework genera el código.</span><span class="sxs-lookup"><span data-stu-id="28697-107">Database-first: You start with a database, and Entity Framework generates the code.</span></span>
- <span data-ttu-id="28697-108">Modelo-primero: comienza con un modelo visual y Entity Framework genera la base de datos y el código.</span><span class="sxs-lookup"><span data-stu-id="28697-108">Model-first: You start with a visual model, and Entity Framework generates both the database and code.</span></span>
- <span data-ttu-id="28697-109">Code-First: comienza con el código y Entity Framework genera la base de datos.</span><span class="sxs-lookup"><span data-stu-id="28697-109">Code-first: You start with code, and Entity Framework generates the database.</span></span>

<span data-ttu-id="28697-110">Estamos usando el enfoque de Code First, por lo que comenzaremos definiendo los objetos de dominio como POCO (objetos CLR antiguos sin formato).</span><span class="sxs-lookup"><span data-stu-id="28697-110">We are using the code-first approach, so we start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="28697-111">Con el enfoque del código primero, los objetos de dominio no necesitan ningún código adicional para admitir el nivel de base de datos, como transacciones o persistencia.</span><span class="sxs-lookup"><span data-stu-id="28697-111">With the code-first approach, domain objects don't need any extra code to support the database layer, such as transactions or persistence.</span></span> <span data-ttu-id="28697-112">(En concreto, no es necesario heredar de la clase [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) ). Todavía puede usar anotaciones de datos para controlar cómo crea Entity Framework el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="28697-112">(Specifically, they do not need to inherit from the [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) class.) You can still use data annotations to control how Entity Framework creates the database schema.</span></span>

<span data-ttu-id="28697-113">Dado que los POCO no incluyen ninguna propiedad adicional que describa el estado de la [base de datos](https://msdn.microsoft.com/library/system.data.entitystate.aspx), se pueden serializar fácilmente a JSON o XML.</span><span class="sxs-lookup"><span data-stu-id="28697-113">Because POCOs do not carry any extra properties that describe [database state](https://msdn.microsoft.com/library/system.data.entitystate.aspx), they can easily be serialized to JSON or XML.</span></span> <span data-ttu-id="28697-114">Sin embargo, eso no significa que siempre se expongan los modelos de Entity Framework directamente a los clientes, como veremos más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="28697-114">However, that does not mean you should always expose your Entity Framework models directly to clients, as we'll see later in the tutorial.</span></span>

<span data-ttu-id="28697-115">Se crearán los siguientes POCO:</span><span class="sxs-lookup"><span data-stu-id="28697-115">We will create the following POCOs:</span></span>

- <span data-ttu-id="28697-116">Producto</span><span class="sxs-lookup"><span data-stu-id="28697-116">Product</span></span>
- <span data-ttu-id="28697-117">Ordenar</span><span class="sxs-lookup"><span data-stu-id="28697-117">Order</span></span>
- <span data-ttu-id="28697-118">OrderDetail</span><span class="sxs-lookup"><span data-stu-id="28697-118">OrderDetail</span></span>

<span data-ttu-id="28697-119">Para crear cada clase, haga clic con el botón secundario en la carpeta models en Explorador de soluciones.</span><span class="sxs-lookup"><span data-stu-id="28697-119">To create each class, right-click the Models folder in Solution Explorer.</span></span> <span data-ttu-id="28697-120">En el menú contextual, seleccione **Agregar** y, a continuación, seleccione **clase.**</span><span class="sxs-lookup"><span data-stu-id="28697-120">From the context menu, select **Add** and then select **Class.**</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

<span data-ttu-id="28697-121">Agregue una clase `Product` con la siguiente implementación:</span><span class="sxs-lookup"><span data-stu-id="28697-121">Add a `Product` class with the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

<span data-ttu-id="28697-122">Por Convención, Entity Framework usa la propiedad `Id` como clave principal y la asigna a una columna de identidad en la tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="28697-122">By convention, Entity Framework uses the `Id` property as the primary key and maps it to an identity column in the database table.</span></span> <span data-ttu-id="28697-123">Cuando se crea una nueva instancia de `Product`, no se establece un valor para `Id`, porque la base de datos genera el valor.</span><span class="sxs-lookup"><span data-stu-id="28697-123">When you create a new `Product` instance, you won't set a value for `Id`, because the database generates the value.</span></span>

<span data-ttu-id="28697-124">El atributo **ScaffoldColumn** indica a ASP.NET MVC que omita la propiedad `Id` al generar un formulario del editor.</span><span class="sxs-lookup"><span data-stu-id="28697-124">The **ScaffoldColumn** attribute tells ASP.NET MVC to skip the `Id` property when generating an editor form.</span></span> <span data-ttu-id="28697-125">El atributo **required** se utiliza para validar el modelo.</span><span class="sxs-lookup"><span data-stu-id="28697-125">The **Required** attribute is used to validate the model.</span></span> <span data-ttu-id="28697-126">Especifica que la propiedad `Name` debe ser una cadena no vacía.</span><span class="sxs-lookup"><span data-stu-id="28697-126">It specifies that the `Name` property must be a non-empty string.</span></span>

<span data-ttu-id="28697-127">Agregue la clase `Order`:</span><span class="sxs-lookup"><span data-stu-id="28697-127">Add the `Order` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

<span data-ttu-id="28697-128">Agregue la clase `OrderDetail`:</span><span class="sxs-lookup"><span data-stu-id="28697-128">Add the `OrderDetail` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a><span data-ttu-id="28697-129">Relaciones de clave externa</span><span class="sxs-lookup"><span data-stu-id="28697-129">Foreign Key Relations</span></span>

<span data-ttu-id="28697-130">Un pedido contiene muchos detalles de pedidos y cada detalle de pedido hace referencia a un único producto.</span><span class="sxs-lookup"><span data-stu-id="28697-130">An order contains many order details, and each order detail refers to a single product.</span></span> <span data-ttu-id="28697-131">Para representar estas relaciones, la clase `OrderDetail` define propiedades denominadas `OrderId` y `ProductId`.</span><span class="sxs-lookup"><span data-stu-id="28697-131">To represent these relations, the `OrderDetail` class defines properties named `OrderId` and `ProductId`.</span></span> <span data-ttu-id="28697-132">Entity Framework deducirá que estas propiedades representan claves externas y agregará restricciones de clave externa a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="28697-132">Entity Framework will infer that these properties represent foreign keys, and will add foreign-key constraints to the database.</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

<span data-ttu-id="28697-133">Las clases `Order` y `OrderDetail` también incluyen propiedades de "navegación", que contienen referencias a los objetos relacionados.</span><span class="sxs-lookup"><span data-stu-id="28697-133">The `Order` and `OrderDetail` classes also include "navigation" properties, which contain references to the related objects.</span></span> <span data-ttu-id="28697-134">Dado un pedido, puede navegar a los productos en el orden siguiendo las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="28697-134">Given an order, you can navigate to the products in the order by following the navigation properties.</span></span>

<span data-ttu-id="28697-135">Compile el proyecto ahora.</span><span class="sxs-lookup"><span data-stu-id="28697-135">Compile the project now.</span></span> <span data-ttu-id="28697-136">Entity Framework usa la reflexión para detectar las propiedades de los modelos, por lo que requiere un ensamblado compilado para crear el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="28697-136">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

## <a name="configure-the-media-type-formatters"></a><span data-ttu-id="28697-137">Configurar los formateadores de tipo de medio</span><span class="sxs-lookup"><span data-stu-id="28697-137">Configure the Media-Type Formatters</span></span>

<span data-ttu-id="28697-138">Un [formateador de tipo de medio](../../formats-and-model-binding/media-formatters.md) es un objeto que serializa los datos cuando la API Web escribe el cuerpo de respuesta http.</span><span class="sxs-lookup"><span data-stu-id="28697-138">A [media-type formatter](../../formats-and-model-binding/media-formatters.md) is an object that serializes your data when Web API writes the HTTP response body.</span></span> <span data-ttu-id="28697-139">Los formateadores integrados admiten la salida de JSON y XML.</span><span class="sxs-lookup"><span data-stu-id="28697-139">The built-in formatters support JSON and XML output.</span></span> <span data-ttu-id="28697-140">De forma predeterminada, ambos formateadores serializan todos los objetos por valor.</span><span class="sxs-lookup"><span data-stu-id="28697-140">By default, both of these formatters serialize all objects by value.</span></span>

<span data-ttu-id="28697-141">La serialización por valor crea un problema si un gráfico de objetos contiene referencias circulares.</span><span class="sxs-lookup"><span data-stu-id="28697-141">Serialization by value creates a problem if an object graph contains circular references.</span></span> <span data-ttu-id="28697-142">Eso es exactamente el caso de las clases `Order` y `OrderDetail`, porque cada una contiene una referencia a la otra.</span><span class="sxs-lookup"><span data-stu-id="28697-142">That's exactly the case with the `Order` and `OrderDetail` classes, because each holds a reference to the other.</span></span> <span data-ttu-id="28697-143">El formateador seguirá las referencias, escribirá cada objeto por valor y irá en círculos.</span><span class="sxs-lookup"><span data-stu-id="28697-143">The formatter will follow the references, writing each object by value, and go in circles.</span></span> <span data-ttu-id="28697-144">Por lo tanto, es necesario cambiar el comportamiento predeterminado.</span><span class="sxs-lookup"><span data-stu-id="28697-144">Therefore, we need to change the default behavior.</span></span>

<span data-ttu-id="28697-145">En Explorador de soluciones, expanda la carpeta app\_Start y abra el archivo denominado WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="28697-145">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="28697-146">Agregue el código siguiente a la clase `WebApiConfig`:</span><span class="sxs-lookup"><span data-stu-id="28697-146">Add the following code to the `WebApiConfig` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

<span data-ttu-id="28697-147">Este código establece el formateador JSON para conservar las referencias de objeto y quita completamente el formateador XML de la canalización.</span><span class="sxs-lookup"><span data-stu-id="28697-147">This code sets the JSON formatter to preserve object references, and removes the XML formatter from the pipeline entirely.</span></span> <span data-ttu-id="28697-148">(Puede configurar el formateador XML para conservar las referencias a objetos, pero es un poco más trabajo y solo necesitamos JSON para esta aplicación.</span><span class="sxs-lookup"><span data-stu-id="28697-148">(You can configure the XML formatter to preserve object references, but it's a little more work, and we only need JSON for this application.</span></span> <span data-ttu-id="28697-149">Para obtener más información, vea [controlar referencias circulares de objetos](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)).</span><span class="sxs-lookup"><span data-stu-id="28697-149">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="28697-150">[Anterior](using-web-api-with-entity-framework-part-1.md)
> [Siguiente](using-web-api-with-entity-framework-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="28697-150">[Previous](using-web-api-with-entity-framework-part-1.md)
[Next](using-web-api-with-entity-framework-part-3.md)</span></span>
