---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Parte 4: agregar una vista de administración | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: 664aeb33031e933322886a6d6bdd989277e9fda2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600012"
---
# <a name="part-4-adding-an-admin-view"></a><span data-ttu-id="ad575-102">Parte 4: adición de una vista de administración</span><span class="sxs-lookup"><span data-stu-id="ad575-102">Part 4: Adding an Admin View</span></span>

<span data-ttu-id="ad575-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ad575-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ad575-104">Descargar proyecto completado</span><span class="sxs-lookup"><span data-stu-id="ad575-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a><span data-ttu-id="ad575-105">Agregar una vista de administración</span><span class="sxs-lookup"><span data-stu-id="ad575-105">Add an Admin View</span></span>

<span data-ttu-id="ad575-106">Ahora se va a activar el lado del cliente y se agregará una página que puede consumir datos del controlador de administración.</span><span class="sxs-lookup"><span data-stu-id="ad575-106">Now we'll turn to the client side, and add a page that can consume data from the Admin controller.</span></span> <span data-ttu-id="ad575-107">La página permitirá a los usuarios crear, editar o eliminar productos mediante el envío de solicitudes AJAX al controlador.</span><span class="sxs-lookup"><span data-stu-id="ad575-107">The page will allow users to create, edit, or delete products, by sending AJAX requests to the controller.</span></span>

<span data-ttu-id="ad575-108">En Explorador de soluciones, expanda la carpeta Controllers y abra el archivo denominado HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="ad575-108">In Solution Explorer, expand the Controllers folder and open the file named HomeController.cs.</span></span> <span data-ttu-id="ad575-109">Este archivo contiene un controlador de MVC.</span><span class="sxs-lookup"><span data-stu-id="ad575-109">This file contains an MVC controller.</span></span> <span data-ttu-id="ad575-110">Agregue un método denominado `Admin`:</span><span class="sxs-lookup"><span data-stu-id="ad575-110">Add a method named `Admin`:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

<span data-ttu-id="ad575-111">El método **HttpRouteUrl** crea el URI para la API Web y lo almacenamos en el contenedor de vistas para más adelante.</span><span class="sxs-lookup"><span data-stu-id="ad575-111">The **HttpRouteUrl** method creates the URI to the web API, and we store this in the view bag for later.</span></span>

<span data-ttu-id="ad575-112">A continuación, coloque el cursor de texto dentro del método de acción `Admin`, haga clic con el botón derecho y seleccione **Agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="ad575-112">Next, position the text cursor within the `Admin` action method, then right-click and select **Add View**.</span></span> <span data-ttu-id="ad575-113">Se abrirá el cuadro de diálogo **Agregar vista** .</span><span class="sxs-lookup"><span data-stu-id="ad575-113">This will bring up the **Add View** dialog.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

<span data-ttu-id="ad575-114">En el cuadro de diálogo **Agregar vista** , asigne a la vista el nombre "admin".</span><span class="sxs-lookup"><span data-stu-id="ad575-114">In the **Add View** dialog, name the view "Admin".</span></span> <span data-ttu-id="ad575-115">Active la casilla de verificación **crear una vista fuertemente tipada**.</span><span class="sxs-lookup"><span data-stu-id="ad575-115">Select the check box labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="ad575-116">En **clase de modelo**, seleccione "Product (ProductStore. Models)".</span><span class="sxs-lookup"><span data-stu-id="ad575-116">Under **Model Class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="ad575-117">Deje las demás opciones como valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="ad575-117">Leave all the other options as their default values.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

<span data-ttu-id="ad575-118">Al hacer clic en **Agregar** , se agrega un archivo denominado admin. cshtml en vistas/Inicio.</span><span class="sxs-lookup"><span data-stu-id="ad575-118">Clicking **Add** adds a file named Admin.cshtml under Views/Home.</span></span> <span data-ttu-id="ad575-119">Abra este archivo y agregue el siguiente código HTML.</span><span class="sxs-lookup"><span data-stu-id="ad575-119">Open this file and add the following HTML.</span></span> <span data-ttu-id="ad575-120">Este código HTML define la estructura de la página, pero aún no se ha conectado ninguna funcionalidad.</span><span class="sxs-lookup"><span data-stu-id="ad575-120">This HTML defines the structure of the page, but no functionality is wired up yet.</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a><span data-ttu-id="ad575-121">Crear un vínculo a la página de administración</span><span class="sxs-lookup"><span data-stu-id="ad575-121">Create a Link to the Admin Page</span></span>

<span data-ttu-id="ad575-122">En Explorador de soluciones, expanda la carpeta vistas y, a continuación, expanda la carpeta compartida.</span><span class="sxs-lookup"><span data-stu-id="ad575-122">In Solution Explorer, expand the Views folder and then expand the Shared folder.</span></span> <span data-ttu-id="ad575-123">Abra el archivo denominado \_layout. cshtml.</span><span class="sxs-lookup"><span data-stu-id="ad575-123">Open the file named \_Layout.cshtml.</span></span> <span data-ttu-id="ad575-124">Busque el elemento **UL** con ID = "MENU" y un vínculo de acción para la vista de administración:</span><span class="sxs-lookup"><span data-stu-id="ad575-124">Locate the **ul** element with id = "menu", and an action link for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> <span data-ttu-id="ad575-125">En el proyecto de ejemplo, he realizado algunos cambios cosméticos, como reemplazar la cadena "su logotipo aquí".</span><span class="sxs-lookup"><span data-stu-id="ad575-125">In the sample project, I made a few other cosmetic changes, such as replacing the string "Your logo here".</span></span> <span data-ttu-id="ad575-126">Estos no afectan a la funcionalidad de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ad575-126">These don't affect the functionality of the application.</span></span> <span data-ttu-id="ad575-127">Puede descargar el proyecto y comparar los archivos.</span><span class="sxs-lookup"><span data-stu-id="ad575-127">You can download the project and compare the files.</span></span>

<span data-ttu-id="ad575-128">Ejecute la aplicación y haga clic en el vínculo "admin" que aparece en la parte superior de la Página principal.</span><span class="sxs-lookup"><span data-stu-id="ad575-128">Run the application and click the "Admin" link that appears at the top of the home page.</span></span> <span data-ttu-id="ad575-129">La página de administración debería tener un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="ad575-129">The Admin page should look like the following:</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

<span data-ttu-id="ad575-130">En este momento, la página no hace nada.</span><span class="sxs-lookup"><span data-stu-id="ad575-130">Right now, the page doesn't do anything.</span></span> <span data-ttu-id="ad575-131">En la siguiente sección, usaremos Knockout. js para crear una interfaz de usuario dinámica.</span><span class="sxs-lookup"><span data-stu-id="ad575-131">In the next section, we'll use Knockout.js to create a dynamic UI.</span></span>

## <a name="add-authorization"></a><span data-ttu-id="ad575-132">Agregar autorización</span><span class="sxs-lookup"><span data-stu-id="ad575-132">Add Authorization</span></span>

<span data-ttu-id="ad575-133">La página de administración es actualmente accesible para cualquier persona que visite el sitio.</span><span class="sxs-lookup"><span data-stu-id="ad575-133">The Admin page is currently accessible to anyone visiting the site.</span></span> <span data-ttu-id="ad575-134">Vamos a cambiar esto para restringir el permiso a los administradores.</span><span class="sxs-lookup"><span data-stu-id="ad575-134">Let's change this to restrict permission to administrators.</span></span>

<span data-ttu-id="ad575-135">Comience agregando un rol de administrador y un usuario administrador.</span><span class="sxs-lookup"><span data-stu-id="ad575-135">Start by adding an "Administrator" role and an administrator user.</span></span> <span data-ttu-id="ad575-136">En Explorador de soluciones, expanda la carpeta filters y abra el archivo denominado InitializeSimpleMembershipAttribute.cs.</span><span class="sxs-lookup"><span data-stu-id="ad575-136">In Solution Explorer, expand the Filters folder and open the file named InitializeSimpleMembershipAttribute.cs.</span></span> <span data-ttu-id="ad575-137">Busque el constructor de `SimpleMembershipInitializer`.</span><span class="sxs-lookup"><span data-stu-id="ad575-137">Locate the `SimpleMembershipInitializer` constructor.</span></span> <span data-ttu-id="ad575-138">Después de la llamada a **WebSecurity. InitializeDatabaseConnection**, agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="ad575-138">After the call to **WebSecurity.InitializeDatabaseConnection**, add the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

<span data-ttu-id="ad575-139">Se trata de una manera rápida y desfasada de agregar el rol "administrador" y crear un usuario para el rol.</span><span class="sxs-lookup"><span data-stu-id="ad575-139">This is a quick-and-dirty way to add the "Administrator" role and create a user for the role.</span></span>

<span data-ttu-id="ad575-140">En Explorador de soluciones, expanda la carpeta Controllers y abra el archivo HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="ad575-140">In Solution Explorer, expand the Controllers folder and open the HomeController.cs file.</span></span> <span data-ttu-id="ad575-141">Agregue el atributo **Authorize** al método `Admin`.</span><span class="sxs-lookup"><span data-stu-id="ad575-141">Add the **Authorize** attribute to the `Admin` method.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

<span data-ttu-id="ad575-142">Abra el archivo AdminController.cs y agregue el atributo **Authorize** a toda la clase `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="ad575-142">Open the AdminController.cs file and add the **Authorize** attribute to the entire `AdminController` class.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> <span data-ttu-id="ad575-143">Tanto MVC como la API Web definen ambos atributos de **autorización** , en diferentes espacios de nombres.</span><span class="sxs-lookup"><span data-stu-id="ad575-143">MVC and Web API both define **Authorize** attributes, in different namespaces.</span></span> <span data-ttu-id="ad575-144">MVC usa **System. Web. Mvc. AuthorizeAttribute**, mientras que la API Web usa **System. Web. http. AuthorizeAttribute**.</span><span class="sxs-lookup"><span data-stu-id="ad575-144">MVC uses **System.Web.Mvc.AuthorizeAttribute**, while Web API uses **System.Web.Http.AuthorizeAttribute**.</span></span>

<span data-ttu-id="ad575-145">Ahora solo los administradores pueden ver la página de administración.</span><span class="sxs-lookup"><span data-stu-id="ad575-145">Now only administrators can view the Admin page.</span></span> <span data-ttu-id="ad575-146">Además, si envía una solicitud HTTP al controlador de administración, la solicitud debe contener una cookie de autenticación.</span><span class="sxs-lookup"><span data-stu-id="ad575-146">Also, if you send an HTTP request to the Admin controller, the request must contain an authentication cookie.</span></span> <span data-ttu-id="ad575-147">De lo contrario, el servidor envía una respuesta HTTP 401 (no autorizado).</span><span class="sxs-lookup"><span data-stu-id="ad575-147">If not, the server sends an HTTP 401 (Unauthorized) response.</span></span> <span data-ttu-id="ad575-148">Puede verlo en Fiddler mediante el envío de una solicitud GET a `http://localhost:*port*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="ad575-148">You can see this in Fiddler by sending a GET request to `http://localhost:*port*/api/admin`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ad575-149">[Anterior](using-web-api-with-entity-framework-part-3.md)
> [Siguiente](using-web-api-with-entity-framework-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="ad575-149">[Previous](using-web-api-with-entity-framework-part-3.md)
[Next](using-web-api-with-entity-framework-part-5.md)</span></span>
