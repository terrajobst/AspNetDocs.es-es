---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Parte 1: información general y creación del proyecto | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: a76a18f2bd95969358452085ef342fdca8a386e2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600325"
---
# <a name="part-1-overview-and-creating-the-project"></a><span data-ttu-id="354da-102">Parte 1: información general y creación del proyecto</span><span class="sxs-lookup"><span data-stu-id="354da-102">Part 1: Overview and Creating the Project</span></span>

<span data-ttu-id="354da-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="354da-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="354da-104">Descargar proyecto completado</span><span class="sxs-lookup"><span data-stu-id="354da-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

<span data-ttu-id="354da-105">Entity Framework es un marco de trabajo de asignación relacional/objeto.</span><span class="sxs-lookup"><span data-stu-id="354da-105">Entity Framework is an object/relational mapping framework.</span></span> <span data-ttu-id="354da-106">Asigna los objetos de dominio del código a las entidades de una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="354da-106">It maps the domain objects in your code to entities in a relational database.</span></span> <span data-ttu-id="354da-107">En su mayor parte, no tiene que preocuparse por el nivel de base de datos, porque Entity Framework se encarga de ello.</span><span class="sxs-lookup"><span data-stu-id="354da-107">For the most part, you do not have to worry about the database layer, because Entity Framework takes care of it for you.</span></span> <span data-ttu-id="354da-108">El código manipula los objetos y los cambios se conservan en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="354da-108">Your code manipulates the objects, and changes are persisted to a database.</span></span>

## <a name="about-the-tutorial"></a><span data-ttu-id="354da-109">Acerca del tutorial</span><span class="sxs-lookup"><span data-stu-id="354da-109">About the Tutorial</span></span>

<span data-ttu-id="354da-110">En este tutorial, creará una aplicación de almacenamiento simple.</span><span class="sxs-lookup"><span data-stu-id="354da-110">In this tutorial, you will create a simple store application.</span></span> <span data-ttu-id="354da-111">Hay dos partes principales en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="354da-111">There are two main parts to the application.</span></span> <span data-ttu-id="354da-112">Los usuarios normales pueden ver los productos y crear pedidos:</span><span class="sxs-lookup"><span data-stu-id="354da-112">Normal users can view products and create orders:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

<span data-ttu-id="354da-113">Los administradores pueden crear, eliminar o editar productos:</span><span class="sxs-lookup"><span data-stu-id="354da-113">Administrators can create, delete, or edit products:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="354da-114">Aptitudes que aprenderá</span><span class="sxs-lookup"><span data-stu-id="354da-114">Skills You'll Learn</span></span>

<span data-ttu-id="354da-115">Esto es lo que aprenderá:</span><span class="sxs-lookup"><span data-stu-id="354da-115">Here's what you'll learn:</span></span>

- <span data-ttu-id="354da-116">Cómo usar Entity Framework con ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="354da-116">How to use Entity Framework with ASP.NET Web API.</span></span>
- <span data-ttu-id="354da-117">Cómo usar Knockout. js para crear una interfaz de usuario de cliente dinámica.</span><span class="sxs-lookup"><span data-stu-id="354da-117">How to use knockout.js to create a dynamic client UI.</span></span>
- <span data-ttu-id="354da-118">Cómo usar la autenticación de formularios con la API Web para autenticar a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="354da-118">How to use forms authentication with Web API to authenticate users.</span></span>

<span data-ttu-id="354da-119">Aunque este tutorial es independiente, puede que desee leer los siguientes tutoriales en primer lugar:</span><span class="sxs-lookup"><span data-stu-id="354da-119">Although this tutorial is self-contained, you might want to read the following tutorials first:</span></span>

- [<span data-ttu-id="354da-120">Su primer ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="354da-120">Your First ASP.NET Web API</span></span>](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [<span data-ttu-id="354da-121">Creación de una API Web que admita operaciones CRUD</span><span class="sxs-lookup"><span data-stu-id="354da-121">Creating a Web API that Supports CRUD Operations</span></span>](../creating-a-web-api-that-supports-crud-operations.md)

<span data-ttu-id="354da-122">Algunos conocimientos de [ASP.NET MVC](../../../../mvc/index.md) también son útiles.</span><span class="sxs-lookup"><span data-stu-id="354da-122">Some knowledge of [ASP.NET MVC](../../../../mvc/index.md) is also helpful.</span></span>

## <a name="overview"></a><span data-ttu-id="354da-123">Información general del</span><span class="sxs-lookup"><span data-stu-id="354da-123">Overview</span></span>

<span data-ttu-id="354da-124">En un nivel alto, esta es la arquitectura de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="354da-124">At a high level, here is the architecture of the application:</span></span>

- <span data-ttu-id="354da-125">ASP.NET MVC genera las páginas HTML para el cliente.</span><span class="sxs-lookup"><span data-stu-id="354da-125">ASP.NET MVC generates the HTML pages for the client.</span></span>
- <span data-ttu-id="354da-126">ASP.NET Web API expone operaciones CRUD en los datos (productos y pedidos).</span><span class="sxs-lookup"><span data-stu-id="354da-126">ASP.NET Web API exposes CRUD operations on the data (products and orders).</span></span>
- <span data-ttu-id="354da-127">Entity Framework traduce los C# modelos utilizados por Web API en entidades de base de datos.</span><span class="sxs-lookup"><span data-stu-id="354da-127">Entity Framework translates the C# models used by Web API into database entities.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

<span data-ttu-id="354da-128">En el diagrama siguiente se muestra cómo se representan los objetos de dominio en varias capas de la aplicación: la capa de base de datos, el modelo de objetos y, por último, el formato de conexión, que se usa para transmitir los datos al cliente a través de HTTP.</span><span class="sxs-lookup"><span data-stu-id="354da-128">The following diagram shows how the domain objects are represented at various layers of the application: The database layer, the object model, and finally the wire format, which is used to transmit data to the client via HTTP.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="354da-129">Crear el proyecto de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="354da-129">Create the Visual Studio Project</span></span>

<span data-ttu-id="354da-130">Puede crear el proyecto tutorial mediante Visual Web Developer Express o la versión completa de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="354da-130">You can create the tutorial project using either Visual Web Developer Express or the full version of Visual Studio.</span></span>

<span data-ttu-id="354da-131">En la página de **Inicio** , haga clic en **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="354da-131">From the **Start** page, click **New Project**.</span></span>

<span data-ttu-id="354da-132">En el panel **plantillas** , seleccione **plantillas instaladas** y expanda el nodo **Visual C#**  .</span><span class="sxs-lookup"><span data-stu-id="354da-132">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="354da-133">En **Visual C#** , seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="354da-133">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="354da-134">En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="354da-134">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="354da-135">Asigne al proyecto el nombre "ProductStore" y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="354da-135">Name the project "ProductStore" and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

<span data-ttu-id="354da-136">En el cuadro de diálogo **nuevo proyecto de ASP.NET MVC 4** , seleccione **aplicación de Internet** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="354da-136">In the **New ASP.NET MVC 4 Project** dialog, select **Internet Application** and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

<span data-ttu-id="354da-137">La plantilla "aplicación de Internet" crea una aplicación de ASP.NET MVC que admite la autenticación de formularios.</span><span class="sxs-lookup"><span data-stu-id="354da-137">The "Internet Application" template creates an ASP.NET MVC application that supports forms authentication.</span></span> <span data-ttu-id="354da-138">Si ejecuta la aplicación ahora, ya tiene algunas características:</span><span class="sxs-lookup"><span data-stu-id="354da-138">If you run the application now, it already has some features:</span></span>

- <span data-ttu-id="354da-139">Los usuarios nuevos pueden registrarse haciendo clic en el vínculo "Register" en la esquina superior derecha.</span><span class="sxs-lookup"><span data-stu-id="354da-139">New users can register by clicking the "Register" link in the upper right corner.</span></span>
- <span data-ttu-id="354da-140">Los usuarios registrados pueden iniciar sesión haciendo clic en el vínculo "iniciar sesión".</span><span class="sxs-lookup"><span data-stu-id="354da-140">Registered users can log in by clicking the "Log in" link.</span></span>

<span data-ttu-id="354da-141">La información de pertenencia se conserva en una base de datos que se crea automáticamente.</span><span class="sxs-lookup"><span data-stu-id="354da-141">Membership information is persisted in a database that gets created automatically.</span></span> <span data-ttu-id="354da-142">Para obtener más información sobre la autenticación de formularios en ASP.NET MVC, vea [Tutorial: usar la autenticación de formularios en ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span><span class="sxs-lookup"><span data-stu-id="354da-142">For more information about forms authentication in ASP.NET MVC, see [Walkthrough: Using Forms Authentication in ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span></span>

## <a name="update-the-css-file"></a><span data-ttu-id="354da-143">Actualizar el archivo CSS</span><span class="sxs-lookup"><span data-stu-id="354da-143">Update the CSS File</span></span>

<span data-ttu-id="354da-144">Este paso es cosmético, pero hará que las páginas se representen como las capturas de pantalla anteriores.</span><span class="sxs-lookup"><span data-stu-id="354da-144">This step is cosmetic, but it will make the pages render like the earlier screen shots.</span></span>

<span data-ttu-id="354da-145">En Explorador de soluciones, expanda la carpeta Content y abra el archivo denominado site. CSS.</span><span class="sxs-lookup"><span data-stu-id="354da-145">In Solution Explorer, expand the Content folder and open the file named Site.css.</span></span> <span data-ttu-id="354da-146">Agregue los siguientes estilos CSS:</span><span class="sxs-lookup"><span data-stu-id="354da-146">Add the following CSS styles:</span></span>

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [<span data-ttu-id="354da-147">Siguiente</span><span class="sxs-lookup"><span data-stu-id="354da-147">Next</span></span>](using-web-api-with-entity-framework-part-2.md)
