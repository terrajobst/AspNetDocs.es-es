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
# <a name="part-2-data-access-layer"></a><span data-ttu-id="be4ff-104">Parte 2: capa de acceso a datos</span><span class="sxs-lookup"><span data-stu-id="be4ff-104">Part 2: Data Access Layer</span></span>

<span data-ttu-id="be4ff-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="be4ff-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="be4ff-106">Tailspin Spyworks muestra cómo es extraordinariamente simple la creación de aplicaciones eficaces y escalables para la plataforma .NET.</span><span class="sxs-lookup"><span data-stu-id="be4ff-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="be4ff-107">Muestra cómo usar las excelentes características nuevas de ASP.NET 4 para crear una tienda en línea, como compras, desprotección y administración.</span><span class="sxs-lookup"><span data-stu-id="be4ff-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="be4ff-108">En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="be4ff-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="be4ff-109">La parte 2 cubre la adición de la capa de acceso a datos.</span><span class="sxs-lookup"><span data-stu-id="be4ff-109">Part 2 covers adding the data access layer.</span></span>

## <a id="_Toc260221668"></a><span data-ttu-id="be4ff-110">Agregar la capa de acceso a datos</span><span class="sxs-lookup"><span data-stu-id="be4ff-110">Adding the Data Access Layer</span></span>

<span data-ttu-id="be4ff-111">Nuestra aplicación de comercio electrónico dependerá de dos bases de datos.</span><span class="sxs-lookup"><span data-stu-id="be4ff-111">Our ecommerce application will depend on two databases.</span></span>

<span data-ttu-id="be4ff-112">Para obtener información sobre el cliente, usaremos la base de datos de pertenencia de ASP.NET estándar.</span><span class="sxs-lookup"><span data-stu-id="be4ff-112">For customer information we'll use the standard ASP.NET Membership database.</span></span> <span data-ttu-id="be4ff-113">Para el carro de la compra y el catálogo de productos, se va a implementar una base de datos de SQL Express como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="be4ff-113">For our shopping cart and product catalog we'll implement a SQL Express database as follows.</span></span>

![](tailspin-spyworks-part-2/_static/image1.jpg)

<span data-ttu-id="be4ff-114">Tras crear la base de datos (Commerce. MDF) en la aplicación de la aplicación\_la carpeta de datos, podemos crear nuestra capa de acceso a datos con .NET Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="be4ff-114">Having created the database (Commerce.mdf) in the application's App\_Data folder we can proceed to create our Data Access Layer using the .NET Entity Framework.</span></span>

<span data-ttu-id="be4ff-115">Vamos a crear una carpeta denominada "Data\_Access" y a hacer clic con el botón derecho en esa carpeta y seleccionar "Agregar nuevo elemento".</span><span class="sxs-lookup"><span data-stu-id="be4ff-115">We'll create a folder named "Data\_Access" and them right click on that folder and select "Add New Item".</span></span>

<span data-ttu-id="be4ff-116">En el elemento "plantillas instaladas" y seleccione "ADO.NET Entity Data Model", escriba EDM\_Commerce. edmx como nombre y haga clic en el botón "agregar".</span><span class="sxs-lookup"><span data-stu-id="be4ff-116">In the "Installed Templates" item and then select "ADO.NET Entity Data Model" enter EDM\_Commerce.edmx as the name and click the "Add" button.</span></span>

![](tailspin-spyworks-part-2/_static/image2.jpg)

<span data-ttu-id="be4ff-117">Elija "generar desde la base de datos".</span><span class="sxs-lookup"><span data-stu-id="be4ff-117">Choose "Generate from Database".</span></span>

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

<span data-ttu-id="be4ff-118">Guardar y compilar.</span><span class="sxs-lookup"><span data-stu-id="be4ff-118">Save and build.</span></span>

<span data-ttu-id="be4ff-119">Ahora estamos preparados para agregar la primera característica: un menú de categoría de producto.</span><span class="sxs-lookup"><span data-stu-id="be4ff-119">Now we are ready to add our first feature – a product category menu.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="be4ff-120">[Anterior](tailspin-spyworks-part-1.md)
> [Siguiente](tailspin-spyworks-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="be4ff-120">[Previous](tailspin-spyworks-part-1.md)
[Next](tailspin-spyworks-part-3.md)</span></span>
