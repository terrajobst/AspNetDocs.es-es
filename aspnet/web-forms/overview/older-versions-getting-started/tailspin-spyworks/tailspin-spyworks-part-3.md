---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Parte 3: menú diseño y categoría | Microsoft Docs'
author: JoeStagner
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks. La parte 3 cubre la adición del diseño y un menú de categorías.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: a223b97fd362ecf73ecde431e141021c1dcc6a6d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78519103"
---
# <a name="part-3-layout-and-category-menu"></a><span data-ttu-id="9bd17-104">Parte 3: menú diseño y categoría</span><span class="sxs-lookup"><span data-stu-id="9bd17-104">Part 3: Layout and Category Menu</span></span>

<span data-ttu-id="9bd17-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="9bd17-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="9bd17-106">Tailspin Spyworks muestra cómo es extraordinariamente simple la creación de aplicaciones eficaces y escalables para la plataforma .NET.</span><span class="sxs-lookup"><span data-stu-id="9bd17-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="9bd17-107">Muestra cómo usar las excelentes características nuevas de ASP.NET 4 para crear una tienda en línea, como compras, desprotección y administración.</span><span class="sxs-lookup"><span data-stu-id="9bd17-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="9bd17-108">En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="9bd17-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="9bd17-109">La parte 3 cubre la adición del diseño y un menú de categorías.</span><span class="sxs-lookup"><span data-stu-id="9bd17-109">Part 3 covers adding layout and a category menu.</span></span>

## <a id="_Toc260221669"></a><span data-ttu-id="9bd17-110">Agregar algunos diseños y un menú de categorías</span><span class="sxs-lookup"><span data-stu-id="9bd17-110">Adding Some Layout and a Category Menu</span></span>

<span data-ttu-id="9bd17-111">En nuestra página maestra del sitio, agregaremos un div para la columna del lado izquierdo que contendrá nuestro menú de categoría de producto.</span><span class="sxs-lookup"><span data-stu-id="9bd17-111">In our site master page we'll add a div for the left side column that will contain our product category menu.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

<span data-ttu-id="9bd17-112">Tenga en cuenta que la clase CSS que se agregó al archivo Style. CSS proporcionará la alineación deseada y otros formatos.</span><span class="sxs-lookup"><span data-stu-id="9bd17-112">Note that the desired aligning and other formatting will be provided by the CSS class that we added to our Style.css file.</span></span>

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

<span data-ttu-id="9bd17-113">El menú categoría de producto se creará dinámicamente en tiempo de ejecución mediante la consulta de la base de datos de comercio para las categorías de producto existentes y la creación de los elementos de menú y los vínculos correspondientes.</span><span class="sxs-lookup"><span data-stu-id="9bd17-113">The product category menu will be dynamically created at runtime by querying the Commerce database for existing product categories and creating the menu items and corresponding links.</span></span>

<span data-ttu-id="9bd17-114">Para ello, usaremos dos de ASP. Controles de datos eficaces de la red.</span><span class="sxs-lookup"><span data-stu-id="9bd17-114">To accomplish this we will use two of ASP.NET's powerful data controls.</span></span> <span data-ttu-id="9bd17-115">El control "origen de datos de entidad" y el control "ListView".</span><span class="sxs-lookup"><span data-stu-id="9bd17-115">The "Entity Data Source" control and the "ListView" control.</span></span>

![](tailspin-spyworks-part-3/_static/image1.jpg)

<span data-ttu-id="9bd17-116">Vamos a cambiar a "vista de diseño" y usar las aplicaciones auxiliares para configurar nuestros controles.</span><span class="sxs-lookup"><span data-stu-id="9bd17-116">Let's switch to "Design View" and use the helpers to configure our controls.</span></span>

![](tailspin-spyworks-part-3/_static/image2.jpg)

<span data-ttu-id="9bd17-117">Vamos a establecer la propiedad ID de EntityDataSource en el menú EDS\_categoría\_y hacer clic en "Configurar origen de datos".</span><span class="sxs-lookup"><span data-stu-id="9bd17-117">Let's set the EntityDataSource ID property to EDS\_Category\_Menu and click on "Configure Data Source".</span></span>

![](tailspin-spyworks-part-3/_static/image3.jpg)

<span data-ttu-id="9bd17-118">Seleccione la conexión CommerceEntities que se creó para nosotros cuando creamos el modelo de origen de datos de entidad para nuestra base de datos de comercio y hacemos clic en "siguiente".</span><span class="sxs-lookup"><span data-stu-id="9bd17-118">Select the CommerceEntities Connection that was created for us when we created the Entity Data Source Model for our Commerce Database and click "Next".</span></span>

![](tailspin-spyworks-part-3/_static/image4.jpg)

<span data-ttu-id="9bd17-119">Seleccione el nombre del conjunto de entidades "Categories" y deje el resto de las opciones como predeterminado.</span><span class="sxs-lookup"><span data-stu-id="9bd17-119">Select the "Categories" Entity set name and leave the rest of the options as default.</span></span> <span data-ttu-id="9bd17-120">Haga clic en "finalizar".</span><span class="sxs-lookup"><span data-stu-id="9bd17-120">Click "Finish".</span></span>

<span data-ttu-id="9bd17-121">Ahora vamos a establecer la propiedad ID de la instancia del control ListView que colocamos en la página en ListView\_ProductsMenu y activamos su aplicación auxiliar.</span><span class="sxs-lookup"><span data-stu-id="9bd17-121">Now let's set the ID property of the ListView control instance that we placed on our page to ListView\_ProductsMenu and activate its helper.</span></span>

![](tailspin-spyworks-part-3/_static/image5.jpg)

<span data-ttu-id="9bd17-122">Aunque podríamos usar opciones de control para dar formato a la presentación y el formato de los elementos de datos, nuestra creación de menús solo requerirá un marcado sencillo, por lo que escribiremos el código en la vista Código fuente.</span><span class="sxs-lookup"><span data-stu-id="9bd17-122">Though we could use control options to format the data item display and formatting, our menu creation will only require simple markup so we will enter the code in the source view.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

<span data-ttu-id="9bd17-123">Tenga en cuenta la instrucción "eval": &lt;% # eval ("CategoryName")%&gt;</span><span class="sxs-lookup"><span data-stu-id="9bd17-123">Please note the "Eval" statement : &lt;%# Eval("CategoryName") %&gt;</span></span>

<span data-ttu-id="9bd17-124">La sintaxis ASP.NET &lt;% #%&gt; es una Convención abreviada que indica al Runtime que ejecute lo que contenga y genere los resultados "en línea".</span><span class="sxs-lookup"><span data-stu-id="9bd17-124">The ASP.NET syntax &lt;%# %&gt; is a shorthand convention that instructs the runtime to execute whatever is contained within and output the results "in Line".</span></span>

<span data-ttu-id="9bd17-125">La instrucción eval ("CategoryName") indica a que, para la entrada actual de la colección enlazada de elementos de datos, recupere el valor de los nombres de elemento de modelo de entidad "CategoryName".</span><span class="sxs-lookup"><span data-stu-id="9bd17-125">The statement Eval("CategoryName") instructs that, for the current entry in the bound collection of data items, fetch the value of the Entity Model item names "CategoryName".</span></span> <span data-ttu-id="9bd17-126">Esta es una sintaxis concisa para una característica muy eficaz.</span><span class="sxs-lookup"><span data-stu-id="9bd17-126">This is concise syntax for a very powerful feature.</span></span>

<span data-ttu-id="9bd17-127">Permite ejecutar la aplicación ahora.</span><span class="sxs-lookup"><span data-stu-id="9bd17-127">Lets run the application now.</span></span>

![](tailspin-spyworks-part-3/_static/image6.jpg)

<span data-ttu-id="9bd17-128">Tenga en cuenta que ahora se muestra nuestro menú de categoría de producto y cuando hacemos desplazarse sobre uno de los elementos de menú de categoría, podemos ver que el vínculo de elemento de menú apunta a una página que aún no se ha implementado con el nombre ProductsList. aspx y que hemos creado un argumento de cadena de consulta dinámica que contiene el  identificador de categoría.</span><span class="sxs-lookup"><span data-stu-id="9bd17-128">Note that our product category menu is now displayed and when we hover over one of the category menu items we can see the menu item link points to a page we have yet to implement named ProductsList.aspx and that we have built a dynamic query string argument that contains the category id.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9bd17-129">[Anterior](tailspin-spyworks-part-2.md)
> [Siguiente](tailspin-spyworks-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="9bd17-129">[Previous](tailspin-spyworks-part-2.md)
[Next](tailspin-spyworks-part-4.md)</span></span>
