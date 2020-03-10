---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Crear el cliente de JavaScript | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: 74f2cc4e5e401d690042b05b028dfc0c46ae282a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504727"
---
# <a name="create-the-javascript-client"></a><span data-ttu-id="4b2f4-102">Crear el cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="4b2f4-102">Create the JavaScript Client</span></span>

<span data-ttu-id="4b2f4-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4b2f4-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="4b2f4-104">Descargar proyecto completado</span><span class="sxs-lookup"><span data-stu-id="4b2f4-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="4b2f4-105">En esta sección, creará el cliente para la aplicación mediante HTML, JavaScript y la biblioteca [Knockout. js](http://knockoutjs.com/) .</span><span class="sxs-lookup"><span data-stu-id="4b2f4-105">In this section, you will create the client for the application, using HTML, JavaScript, and the [Knockout.js](http://knockoutjs.com/) library.</span></span> <span data-ttu-id="4b2f4-106">Compilaremos la aplicación cliente en etapas:</span><span class="sxs-lookup"><span data-stu-id="4b2f4-106">We'll build the client app in stages:</span></span>

- <span data-ttu-id="4b2f4-107">Mostrar una lista de libros.</span><span class="sxs-lookup"><span data-stu-id="4b2f4-107">Showing a list of books.</span></span>
- <span data-ttu-id="4b2f4-108">Mostrando un detalle del libro.</span><span class="sxs-lookup"><span data-stu-id="4b2f4-108">Showing a book detail.</span></span>
- <span data-ttu-id="4b2f4-109">Agregar un nuevo libro.</span><span class="sxs-lookup"><span data-stu-id="4b2f4-109">Adding a new book.</span></span>

<span data-ttu-id="4b2f4-110">La biblioteca de cobertura usa el patrón Model-View-ViewModel (MVVM):</span><span class="sxs-lookup"><span data-stu-id="4b2f4-110">The Knockout library uses the Model-View-ViewModel (MVVM) pattern:</span></span>

- <span data-ttu-id="4b2f4-111">El **modelo** es la representación del lado servidor de los datos en el dominio empresarial (en nuestro caso, libros y autores).</span><span class="sxs-lookup"><span data-stu-id="4b2f4-111">The **model** is the server-side representation of the data in the business domain (in our case, books and authors).</span></span>
- <span data-ttu-id="4b2f4-112">La **vista** es el nivel de presentación (html).</span><span class="sxs-lookup"><span data-stu-id="4b2f4-112">The **view** is the presentation layer (HTML).</span></span>
- <span data-ttu-id="4b2f4-113">El **modelo de vista** es un objeto de JavaScript que contiene los modelos.</span><span class="sxs-lookup"><span data-stu-id="4b2f4-113">The **view model** is a JavaScript object that holds the models.</span></span> <span data-ttu-id="4b2f4-114">El modelo de vista es una abstracción de código de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="4b2f4-114">The view model is a code abstraction of the UI.</span></span> <span data-ttu-id="4b2f4-115">No tiene ningún conocimiento de la representación en HTML.</span><span class="sxs-lookup"><span data-stu-id="4b2f4-115">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="4b2f4-116">En su lugar, representa características abstractas de la vista, como &quot;una lista de libros&quot;.</span><span class="sxs-lookup"><span data-stu-id="4b2f4-116">Instead, it represents abstract features of the view, such as &quot;a list of books&quot;.</span></span>

<span data-ttu-id="4b2f4-117">La vista está enlazada a datos con el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="4b2f4-117">The view is data-bound to the view model.</span></span> <span data-ttu-id="4b2f4-118">Las actualizaciones del modelo de vista se reflejan automáticamente en la vista.</span><span class="sxs-lookup"><span data-stu-id="4b2f4-118">Updates to the view model are automatically reflected in the view.</span></span> <span data-ttu-id="4b2f4-119">El modelo de vista también obtiene los eventos de la vista, como los clics de botón.</span><span class="sxs-lookup"><span data-stu-id="4b2f4-119">The view model also gets events from the view, such as button clicks.</span></span>

![](part-6/_static/image1.png)

<span data-ttu-id="4b2f4-120">Este enfoque facilita el cambio del diseño y la interfaz de usuario de la aplicación, ya que puede cambiar los enlaces sin necesidad de volver a escribir ningún código.</span><span class="sxs-lookup"><span data-stu-id="4b2f4-120">This approach makes it easy to change the layout and UI of your app, because you can change the bindings, without rewriting any code.</span></span> <span data-ttu-id="4b2f4-121">Por ejemplo, puede mostrar una lista de elementos como `<ul>`y después cambiarlo más adelante a una tabla.</span><span class="sxs-lookup"><span data-stu-id="4b2f4-121">For example, you might show a list of items as a `<ul>`, then change it later to a table.</span></span>

## <a name="add-the-knockout-library"></a><span data-ttu-id="4b2f4-122">Agregar la biblioteca de cobertura</span><span class="sxs-lookup"><span data-stu-id="4b2f4-122">Add the Knockout Library</span></span>

<span data-ttu-id="4b2f4-123">En Visual Studio, en el menú **herramientas** , seleccione **Administrador de paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="4b2f4-123">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**.</span></span> <span data-ttu-id="4b2f4-124">Seleccione **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="4b2f4-124">Then select **Package Manager Console**.</span></span> <span data-ttu-id="4b2f4-125">En la ventana Package Manager Console, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="4b2f4-125">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-6/samples/sample1.cmd)]

<span data-ttu-id="4b2f4-126">Este comando agrega los archivos de cobertura a la carpeta scripts.</span><span class="sxs-lookup"><span data-stu-id="4b2f4-126">This command adds the Knockout files to the Scripts folder.</span></span>

## <a name="create-the-view-model"></a><span data-ttu-id="4b2f4-127">Crear el modelo de vista</span><span class="sxs-lookup"><span data-stu-id="4b2f4-127">Create the View Model</span></span>

<span data-ttu-id="4b2f4-128">Agregue un archivo de JavaScript denominado app. js a la carpeta scripts.</span><span class="sxs-lookup"><span data-stu-id="4b2f4-128">Add a JavaScript file named app.js to the Scripts folder.</span></span> <span data-ttu-id="4b2f4-129">(En Explorador de soluciones, haga clic con el botón derecho en la carpeta scripts, seleccione **Agregar**y, a continuación, **archivo JavaScript**). Pegue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="4b2f4-129">(In Solution Explorer, right-click the Scripts folder, select **Add**, then select **JavaScript File**.) Paste in the following code:</span></span>

[!code-javascript[Main](part-6/samples/sample2.js)]

<span data-ttu-id="4b2f4-130">En la cobertura, la clase `observable` habilita el enlace de datos.</span><span class="sxs-lookup"><span data-stu-id="4b2f4-130">In Knockout, the `observable` class enables data-binding.</span></span> <span data-ttu-id="4b2f4-131">Cuando el contenido de un objeto observable cambia, el objeto observable notifica a todos los controles enlazados a datos para que se puedan actualizar por sí mismos.</span><span class="sxs-lookup"><span data-stu-id="4b2f4-131">When the contents of an observable change, the observable notifies all of the data-bound controls, so they can update themselves.</span></span> <span data-ttu-id="4b2f4-132">(La clase `observableArray` es la versión de la matriz de *observable*). Para empezar, nuestro modelo de vista tiene dos observables:</span><span class="sxs-lookup"><span data-stu-id="4b2f4-132">(The `observableArray` class is the array version of *observable*.) To start with, our view model has two observables:</span></span>

- <span data-ttu-id="4b2f4-133">`books` contiene la lista de libros.</span><span class="sxs-lookup"><span data-stu-id="4b2f4-133">`books` holds the list of books.</span></span>
- <span data-ttu-id="4b2f4-134">`error` contiene un mensaje de error si se produce un error en una llamada AJAX.</span><span class="sxs-lookup"><span data-stu-id="4b2f4-134">`error` contains an error message if an AJAX call fails.</span></span>

<span data-ttu-id="4b2f4-135">El método `getAllBooks` realiza una llamada AJAX para obtener la lista de libros.</span><span class="sxs-lookup"><span data-stu-id="4b2f4-135">The `getAllBooks` method makes an AJAX call to get the list of books.</span></span> <span data-ttu-id="4b2f4-136">Después, envía el resultado a la matriz de `books`.</span><span class="sxs-lookup"><span data-stu-id="4b2f4-136">Then it pushes the result onto the `books` array.</span></span>

<span data-ttu-id="4b2f4-137">El método `ko.applyBindings` forma parte de la biblioteca de cobertura.</span><span class="sxs-lookup"><span data-stu-id="4b2f4-137">The `ko.applyBindings` method is part of the Knockout library.</span></span> <span data-ttu-id="4b2f4-138">Toma el modelo de vista como parámetro y configura el enlace de datos.</span><span class="sxs-lookup"><span data-stu-id="4b2f4-138">It takes the view model as a parameter and sets up the data binding.</span></span>

## <a name="add-a-script-bundle"></a><span data-ttu-id="4b2f4-139">Agregar un paquete de scripts</span><span class="sxs-lookup"><span data-stu-id="4b2f4-139">Add a Script Bundle</span></span>

<span data-ttu-id="4b2f4-140">La Unión es una característica de ASP.NET 4,5 que facilita la combinación o agrupación de varios archivos en un único archivo.</span><span class="sxs-lookup"><span data-stu-id="4b2f4-140">Bundling is a feature in ASP.NET 4.5 that makes it easy to combine or bundle multiple files into a single file.</span></span> <span data-ttu-id="4b2f4-141">La agrupación reduce el número de solicitudes al servidor, lo que puede mejorar el tiempo de carga de la página.</span><span class="sxs-lookup"><span data-stu-id="4b2f4-141">Bundling reduces the number of requests to the server, which can improve page load time.</span></span>

<span data-ttu-id="4b2f4-142">Abra la aplicación de archivo\_Start/BundleConfig. cs.</span><span class="sxs-lookup"><span data-stu-id="4b2f4-142">Open the file App\_Start/BundleConfig.cs.</span></span> <span data-ttu-id="4b2f4-143">Agregue el código siguiente al método RegisterBundles.</span><span class="sxs-lookup"><span data-stu-id="4b2f4-143">Add the following code to the RegisterBundles method.</span></span>

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> <span data-ttu-id="4b2f4-144">[Anterior](part-5.md)
> [Siguiente](part-7.md)</span><span class="sxs-lookup"><span data-stu-id="4b2f4-144">[Previous](part-5.md)
[Next](part-7.md)</span></span>
