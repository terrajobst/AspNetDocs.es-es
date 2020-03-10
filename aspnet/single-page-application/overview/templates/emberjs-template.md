---
uid: single-page-application/overview/templates/emberjs-template
title: Plantilla EmberJS | Microsoft Docs
author: xqiu
description: Plantilla de EmberJS
ms.author: riande
ms.date: 01/30/2013
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1aefa46dd0841b1b06675409cc8a09f9a218d7ac
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467161"
---
# <a name="emberjs-template"></a><span data-ttu-id="23b3d-103">Plantilla de EmberJS</span><span class="sxs-lookup"><span data-stu-id="23b3d-103">EmberJS template</span></span>

<span data-ttu-id="23b3d-104">por [Xinyang Qiu](https://github.com/xqiu)</span><span class="sxs-lookup"><span data-stu-id="23b3d-104">by [Xinyang Qiu](https://github.com/xqiu)</span></span>

> <span data-ttu-id="23b3d-105">La plantilla EmberJS MVC está escrita por Nathan Totten, Thiago Santos y Xinyang Qiu.</span><span class="sxs-lookup"><span data-stu-id="23b3d-105">The EmberJS MVC Template is written by Nathan Totten, Thiago Santos, and Xinyang Qiu.</span></span>
> 
> [<span data-ttu-id="23b3d-106">Descarga de la plantilla EmberJS MVC</span><span class="sxs-lookup"><span data-stu-id="23b3d-106">Download the EmberJS MVC Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=282647)

<span data-ttu-id="23b3d-107">La plantilla EmberJS SPA está diseñada para empezar a crear rápidamente aplicaciones Web interactivas del lado cliente con EmberJS.</span><span class="sxs-lookup"><span data-stu-id="23b3d-107">The EmberJS SPA template is designed to get you started quickly building interactive client-side web apps using EmberJS.</span></span>

<span data-ttu-id="23b3d-108">"Aplicación de una sola página" (SPA) es el término general para una aplicación web que carga una sola página HTML y, a continuación, actualiza la página dinámicamente, en lugar de cargar nuevas páginas.</span><span class="sxs-lookup"><span data-stu-id="23b3d-108">"Single-page application" (SPA) is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="23b3d-109">Después de la carga inicial de la página, el SPA se comunica con el servidor a través de solicitudes AJAX.</span><span class="sxs-lookup"><span data-stu-id="23b3d-109">After the initial page load, the SPA talks with the server through AJAX requests.</span></span>

![](emberjs-template/_static/image1.png)

<span data-ttu-id="23b3d-110">AJAX no es nuevo, pero hoy en día hay marcos de JavaScript que facilitan la compilación y el mantenimiento de una aplicación de SPA sofisticada.</span><span class="sxs-lookup"><span data-stu-id="23b3d-110">AJAX is nothing new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="23b3d-111">Además, HTML 5 y CSS3 facilitan la creación de interfaces de IU enriquecidas.</span><span class="sxs-lookup"><span data-stu-id="23b3d-111">Also, HTML 5 and CSS3 are making it easier to create rich UIs.</span></span>

<span data-ttu-id="23b3d-112">La plantilla EmberJS SPA usa la biblioteca de JavaScript para [Ember](http://emberjs.com/) para controlar las actualizaciones de páginas de las solicitudes Ajax.</span><span class="sxs-lookup"><span data-stu-id="23b3d-112">The EmberJS SPA Template uses the [Ember](http://emberjs.com/) JavaScript library to handle page updates from AJAX requests.</span></span> <span data-ttu-id="23b3d-113">Ember. js usa el enlace de datos para sincronizar la página con los datos más recientes.</span><span class="sxs-lookup"><span data-stu-id="23b3d-113">Ember.js uses data binding to synchronize the page with the latest data.</span></span> <span data-ttu-id="23b3d-114">De este modo, no tiene que escribir ningún código que recorra los datos JSON y actualice el DOM.</span><span class="sxs-lookup"><span data-stu-id="23b3d-114">That way, you don't have to write any of the code that walks through the JSON data and updates the DOM.</span></span> <span data-ttu-id="23b3d-115">En su lugar, se colocan atributos declarativos en el código HTML que indican a Ember. js cómo presentar los datos.</span><span class="sxs-lookup"><span data-stu-id="23b3d-115">Instead, you put declarative attributes in the HTML that tell Ember.js how to present the data.</span></span>

<span data-ttu-id="23b3d-116">En el lado del servidor, la plantilla EmberJS es casi idéntica a la [plantilla KNOCKOUTJS Spa](../introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="23b3d-116">On the server side, the EmberJS template is almost identical to the [KnockoutJS SPA template](../introduction/knockoutjs-template.md).</span></span> <span data-ttu-id="23b3d-117">Usa ASP.NET MVC para servir documentos HTML y ASP.NET Web API para controlar las solicitudes AJAX desde el cliente.</span><span class="sxs-lookup"><span data-stu-id="23b3d-117">It uses ASP.NET MVC to serve HTML documents, and ASP.NET Web API to handle AJAX requests from the client.</span></span> <span data-ttu-id="23b3d-118">Para obtener más información sobre esos aspectos de la plantilla, consulte la documentación de la [plantilla KnockoutJS](../introduction/knockoutjs-template.md) .</span><span class="sxs-lookup"><span data-stu-id="23b3d-118">For more information about those aspects of the template, refer to the [KnockoutJS template](../introduction/knockoutjs-template.md) documentation.</span></span> <span data-ttu-id="23b3d-119">Este tema se centra en las diferencias entre la plantilla de cobertura y la plantilla EmberJS.</span><span class="sxs-lookup"><span data-stu-id="23b3d-119">This topic focuses on the differences between the Knockout template and the EmberJS template.</span></span>

## <a name="create-an-emberjs-spa-template-project"></a><span data-ttu-id="23b3d-120">Creación de un proyecto de plantilla de EmberJS SPA</span><span class="sxs-lookup"><span data-stu-id="23b3d-120">Create an EmberJS SPA Template Project</span></span>

<span data-ttu-id="23b3d-121">Descargue e instale la plantilla haciendo clic en el botón Descargar anterior.</span><span class="sxs-lookup"><span data-stu-id="23b3d-121">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="23b3d-122">Es posible que tenga que reiniciar Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="23b3d-122">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="23b3d-123">En el panel **plantillas** , seleccione **plantillas instaladas** y expanda el nodo **Visual C#**  .</span><span class="sxs-lookup"><span data-stu-id="23b3d-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="23b3d-124">En **Visual C#** , seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="23b3d-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="23b3d-125">En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="23b3d-125">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="23b3d-126">Proporcione un nombre al proyecto y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="23b3d-126">Name the project and click **OK**.</span></span>

![](emberjs-template/_static/image2.png)

<span data-ttu-id="23b3d-127">En el Asistente para **nuevo proyecto** , seleccione **proyecto de Ember. js Spa**.</span><span class="sxs-lookup"><span data-stu-id="23b3d-127">In the **New Project** wizard, select **Ember.js SPA Project**.</span></span>

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a><span data-ttu-id="23b3d-128">Información general de la plantilla de EmberJS SPA</span><span class="sxs-lookup"><span data-stu-id="23b3d-128">EmberJS SPA Template Overview</span></span>

<span data-ttu-id="23b3d-129">La plantilla EmberJS usa una combinación de jQuery, Ember. js y Manillars. js para crear una interfaz de usuario fluida e interactiva.</span><span class="sxs-lookup"><span data-stu-id="23b3d-129">The EmberJS template uses a combination of jQuery, Ember.js, Handlebars.js to create a smooth, interactive UI.</span></span>

<span data-ttu-id="23b3d-130">Ember. js es una biblioteca de JavaScript que usa un patrón MVC del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="23b3d-130">Ember.js is a JavaScript library that uses a client-side MVC pattern.</span></span>

- <span data-ttu-id="23b3d-131">Una *plantilla*, que se escribe en el lenguaje de plantillas del manillar, describe la interfaz de usuario de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="23b3d-131">A *template*, written in the Handlebars templating language, describes the application user interface.</span></span> <span data-ttu-id="23b3d-132">En el modo de lanzamiento, el [compilador del manillar](https://github.com/Myslik/csharp-ember-handlebars) se utiliza para agrupar y compilar la plantilla del manillar.</span><span class="sxs-lookup"><span data-stu-id="23b3d-132">In release mode, the [Handlebars compiler](https://github.com/Myslik/csharp-ember-handlebars) is used to bundle and compile the handlebars template.</span></span>
- <span data-ttu-id="23b3d-133">Un *modelo* almacena los datos de aplicación que obtiene del servidor (listas de tareas pendientes y elementos de la lista de tareas).</span><span class="sxs-lookup"><span data-stu-id="23b3d-133">A *model* stores the application data that it gets from the server (ToDo lists and ToDo items).</span></span>
- <span data-ttu-id="23b3d-134">Un *controlador* almacena el estado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="23b3d-134">A *controller* stores application state.</span></span> <span data-ttu-id="23b3d-135">Los controladores suelen presentar los datos del modelo a las plantillas correspondientes.</span><span class="sxs-lookup"><span data-stu-id="23b3d-135">Controllers often present model data to the corresponding templates.</span></span>
- <span data-ttu-id="23b3d-136">Una *vista* traduce los eventos primitivos de la aplicación y los pasa al controlador.</span><span class="sxs-lookup"><span data-stu-id="23b3d-136">A *view* translates primitive events from the application and passes these to the controller.</span></span>
- <span data-ttu-id="23b3d-137">Un *enrutador* administra el estado de la aplicación, manteniendo las direcciones URL y las plantillas sincronizadas.</span><span class="sxs-lookup"><span data-stu-id="23b3d-137">A *router* manages application state, keeping URLs and templates in sync.</span></span>

<span data-ttu-id="23b3d-138">Además, la biblioteca de datos Ember se puede usar para sincronizar objetos JSON (obtenidos del servidor a través de una API de RESTful) y los modelos de cliente.</span><span class="sxs-lookup"><span data-stu-id="23b3d-138">In addition, the Ember Data library can be used to synchronize JSON objects (obtained from the server through a RESTful API) and the client models.</span></span>

<span data-ttu-id="23b3d-139">La plantilla EmberJS SPA organiza los scripts en ocho capas:</span><span class="sxs-lookup"><span data-stu-id="23b3d-139">The EmberJS SPA template organizes the scripts into eight layers:</span></span>

- <span data-ttu-id="23b3d-140">WebAPI\_Adapter. js, WebAPI\_serializador. js: extiende la biblioteca de datos Ember para trabajar con ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="23b3d-140">webapi\_adapter.js, webapi\_serializer.js: Extends the Ember Data library to work with ASP.NET Web API.</span></span>
- <span data-ttu-id="23b3d-141">Scripts/helpers. js: define las nuevas aplicaciones auxiliares de Ember manillar.</span><span class="sxs-lookup"><span data-stu-id="23b3d-141">Scripts/helpers.js: Defines new Ember Handlebars helpers.</span></span>
- <span data-ttu-id="23b3d-142">Scripts/app. js: crea la aplicación y configura el adaptador y el serializador.</span><span class="sxs-lookup"><span data-stu-id="23b3d-142">Scripts/app.js: Creates the app and configures the adapter and serializer.</span></span>
- <span data-ttu-id="23b3d-143">Scripts/App/Models/\*. js: define los modelos.</span><span class="sxs-lookup"><span data-stu-id="23b3d-143">Scripts/app/models/\*.js: Defines the models.</span></span>
- <span data-ttu-id="23b3d-144">Scripts/App/views/\*. js: define las vistas.</span><span class="sxs-lookup"><span data-stu-id="23b3d-144">Scripts/app/views/\*.js: Defines the views.</span></span>
- <span data-ttu-id="23b3d-145">Scripts/App/Controllers/\*. js: define los controladores.</span><span class="sxs-lookup"><span data-stu-id="23b3d-145">Scripts/app/controllers/\*.js: Defines the controllers.</span></span>
- <span data-ttu-id="23b3d-146">Scripts/aplicación/rutas, scripts/App/router. js: define las rutas.</span><span class="sxs-lookup"><span data-stu-id="23b3d-146">Scripts/app/routes, Scripts/app/router.js: Defines the routes.</span></span>
- <span data-ttu-id="23b3d-147">Templates/\*. HBS: define las plantillas del manillar.</span><span class="sxs-lookup"><span data-stu-id="23b3d-147">Templates/\*.hbs: Defines the handlebars templates.</span></span>

<span data-ttu-id="23b3d-148">Echemos un vistazo a algunos de estos scripts con más detalle.</span><span class="sxs-lookup"><span data-stu-id="23b3d-148">Let's look at some of these scripts in more detail.</span></span>

## <a name="models"></a><span data-ttu-id="23b3d-149">Modelos</span><span class="sxs-lookup"><span data-stu-id="23b3d-149">Models</span></span>

<span data-ttu-id="23b3d-150">Los modelos se definen en la carpeta scripts/aplicación/modelos.</span><span class="sxs-lookup"><span data-stu-id="23b3d-150">The models are defined in the Scripts/app/models folder.</span></span> <span data-ttu-id="23b3d-151">Hay dos archivos de modelo: todoItem. js y todoList. js.</span><span class="sxs-lookup"><span data-stu-id="23b3d-151">There are two model files: todoItem.js and todoList.js.</span></span>

<span data-ttu-id="23b3d-152">**todo. Model. js** define los modelos del lado cliente (explorador) de las listas de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="23b3d-152">**todo.model.js** defines the client-side (browser) models for the to-do lists.</span></span> <span data-ttu-id="23b3d-153">Hay dos clases de modelo: todoItem y todoList.</span><span class="sxs-lookup"><span data-stu-id="23b3d-153">There are two model classes: todoItem and todoList.</span></span> <span data-ttu-id="23b3d-154">En Ember, los modelos son subclases de DS. Modela.</span><span class="sxs-lookup"><span data-stu-id="23b3d-154">In Ember, models are subclasses of DS.Model.</span></span> <span data-ttu-id="23b3d-155">Un modelo puede tener propiedades con atributos:</span><span class="sxs-lookup"><span data-stu-id="23b3d-155">A model can have properties with attributes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

<span data-ttu-id="23b3d-156">Los modelos pueden definir relaciones con otros modelos:</span><span class="sxs-lookup"><span data-stu-id="23b3d-156">Models can define relationships to other models:</span></span>

[!code-css[Main](emberjs-template/samples/sample2.css)]

<span data-ttu-id="23b3d-157">Los modelos pueden tener propiedades calculadas que se enlazan a otras propiedades:</span><span class="sxs-lookup"><span data-stu-id="23b3d-157">Models can have computed properties that bind to other properties:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

<span data-ttu-id="23b3d-158">Los modelos pueden tener funciones de observador, que se invocan cuando cambia una propiedad observada:</span><span class="sxs-lookup"><span data-stu-id="23b3d-158">Models can have observer functions, which are invoked when an observed property changes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a><span data-ttu-id="23b3d-159">Vistas</span><span class="sxs-lookup"><span data-stu-id="23b3d-159">Views</span></span>

<span data-ttu-id="23b3d-160">Las vistas se definen en la carpeta scripts/aplicación/vistas.</span><span class="sxs-lookup"><span data-stu-id="23b3d-160">The views are defined in the Scripts/app/views folder.</span></span> <span data-ttu-id="23b3d-161">Una vista traduce los eventos de la interfaz de usuario de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="23b3d-161">A view translates events from the application UI.</span></span> <span data-ttu-id="23b3d-162">Un controlador de eventos puede devolver la llamada a las funciones de controlador o simplemente llamar directamente al contexto de datos.</span><span class="sxs-lookup"><span data-stu-id="23b3d-162">An event handler can call back to controller functions, or simply call the data context directly.</span></span>

<span data-ttu-id="23b3d-163">Por ejemplo, el código siguiente procede de views/TodoItemEditView. js.</span><span class="sxs-lookup"><span data-stu-id="23b3d-163">For example, the following code is from views/TodoItemEditView.js.</span></span> <span data-ttu-id="23b3d-164">Define el control de eventos para un campo de texto de entrada.</span><span class="sxs-lookup"><span data-stu-id="23b3d-164">It defines the event handling for an input text field.</span></span>

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a><span data-ttu-id="23b3d-165">Controlador</span><span class="sxs-lookup"><span data-stu-id="23b3d-165">Controller</span></span>

<span data-ttu-id="23b3d-166">Los controladores se definen en la carpeta scripts/aplicación/controladores.</span><span class="sxs-lookup"><span data-stu-id="23b3d-166">The controllers are defined in the Scripts/app/controllers folder.</span></span> <span data-ttu-id="23b3d-167">Para representar un único modelo, extienda `Ember.ObjectController`:</span><span class="sxs-lookup"><span data-stu-id="23b3d-167">To represent a single model, extend `Ember.ObjectController`:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

<span data-ttu-id="23b3d-168">Un controlador también puede representar una colección de modelos extendiendo `Ember.ArrayController`.</span><span class="sxs-lookup"><span data-stu-id="23b3d-168">A controller can also represent a collection of models by extending `Ember.ArrayController`.</span></span> <span data-ttu-id="23b3d-169">Por ejemplo, TodoListController representa una matriz de objetos `todoList`.</span><span class="sxs-lookup"><span data-stu-id="23b3d-169">For example, the TodoListController represents an array of `todoList` objects.</span></span> <span data-ttu-id="23b3d-170">El controlador ordena por el identificador de todoList, en orden descendente:</span><span class="sxs-lookup"><span data-stu-id="23b3d-170">The controller sorts by todoList ID, in descending order:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

<span data-ttu-id="23b3d-171">El controlador define una función denominada `addTodoList`, que crea un nuevo todoList y lo agrega a la matriz.</span><span class="sxs-lookup"><span data-stu-id="23b3d-171">The controller defines a function named `addTodoList`, which creates a new todoList and adds it to the array.</span></span> <span data-ttu-id="23b3d-172">Para ver cómo se llama a esta función, abra el archivo de plantilla denominado todoListTemplate. html en la carpeta de plantillas.</span><span class="sxs-lookup"><span data-stu-id="23b3d-172">To see how this function gets called, open the template file named todoListTemplate.html, in the Templates folder.</span></span> <span data-ttu-id="23b3d-173">El código de plantilla siguiente enlaza un botón a la función `addTodoList`:</span><span class="sxs-lookup"><span data-stu-id="23b3d-173">The following template code binds a button to the `addTodoList` function:</span></span>

[!code-html[Main](emberjs-template/samples/sample8.html)]

<span data-ttu-id="23b3d-174">El controlador también contiene una propiedad `error`, que contiene un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="23b3d-174">The controller also contains an `error` property, which holds an error message.</span></span> <span data-ttu-id="23b3d-175">Este es el código de plantilla para mostrar el mensaje de error (también en todoListTemplate. html):</span><span class="sxs-lookup"><span data-stu-id="23b3d-175">Here is the template code to display the error message (also in todoListTemplate.html):</span></span>

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a><span data-ttu-id="23b3d-176">Rutas</span><span class="sxs-lookup"><span data-stu-id="23b3d-176">Routes</span></span>

<span data-ttu-id="23b3d-177">Enrutador. js define las rutas y la plantilla predeterminada que se va a mostrar, configura el estado de la aplicación y coincide con las direcciones URL de las rutas:</span><span class="sxs-lookup"><span data-stu-id="23b3d-177">Router.js defines the routes and the default template to display, sets up application state, and matches URLs to routes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

<span data-ttu-id="23b3d-178">TodoListRoute. js carga los datos para TodoListRoute invalidando la función setupController:</span><span class="sxs-lookup"><span data-stu-id="23b3d-178">TodoListRoute.js loads data for the TodoListRoute by overriding the setupController function:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

<span data-ttu-id="23b3d-179">Ember usa convenciones de nomenclatura para hacer coincidir las direcciones URL, los nombres de ruta, los controladores y las plantillas.</span><span class="sxs-lookup"><span data-stu-id="23b3d-179">Ember uses naming conventions to match URLs, route names, controllers, and templates.</span></span> <span data-ttu-id="23b3d-180">Para obtener más información, consulte [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) en la documentación de EmberJS.</span><span class="sxs-lookup"><span data-stu-id="23b3d-180">For more information, see [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) at the EmberJS documentation.</span></span>

## <a name="templates"></a><span data-ttu-id="23b3d-181">Plantillas</span><span class="sxs-lookup"><span data-stu-id="23b3d-181">Templates</span></span>

<span data-ttu-id="23b3d-182">La carpeta Templates contiene cuatro plantillas:</span><span class="sxs-lookup"><span data-stu-id="23b3d-182">The Templates folder contains four templates:</span></span>

- <span data-ttu-id="23b3d-183">Application. HBS: plantilla predeterminada que se representa cuando se inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="23b3d-183">application.hbs: The default template that is rendered when the application is started.</span></span>
- <span data-ttu-id="23b3d-184">about. HBS: plantilla para la ruta "/About".</span><span class="sxs-lookup"><span data-stu-id="23b3d-184">about.hbs: The template for the "/about" route.</span></span>
- <span data-ttu-id="23b3d-185">index. HBS: la plantilla para la ruta de la raíz "/".</span><span class="sxs-lookup"><span data-stu-id="23b3d-185">index.hbs: The template for the root "/" route.</span></span>
- <span data-ttu-id="23b3d-186">todoList. HBS: plantilla para la ruta "/todo".</span><span class="sxs-lookup"><span data-stu-id="23b3d-186">todoList.hbs: The template for the "/todo" route.</span></span>
- <span data-ttu-id="23b3d-187">\_barra de navegación. HBS: la plantilla define el menú de navegación.</span><span class="sxs-lookup"><span data-stu-id="23b3d-187">\_navbar.hbs: The template defines the navigation menu.</span></span>

<span data-ttu-id="23b3d-188">La plantilla de aplicación actúa como una página maestra.</span><span class="sxs-lookup"><span data-stu-id="23b3d-188">The application template acts like a master page.</span></span> <span data-ttu-id="23b3d-189">Contiene un encabezado, un pie de página y un "{{Outlet}}" para insertar otras plantillas en función de la ruta.</span><span class="sxs-lookup"><span data-stu-id="23b3d-189">It contains a header, a footer, and an "{{outlet}}" to insert other templates in depending on the route.</span></span> <span data-ttu-id="23b3d-190">Para obtener más información acerca de las plantillas de aplicación en Ember, consulte [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span><span class="sxs-lookup"><span data-stu-id="23b3d-190">For more information about application templates in Ember, see [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span></span>

<span data-ttu-id="23b3d-191">La plantilla "/todoList" contiene dos expresiones de bucle.</span><span class="sxs-lookup"><span data-stu-id="23b3d-191">The "/todoList" template contains two loop expressions.</span></span> <span data-ttu-id="23b3d-192">El bucle exterior es `{{#each controller}}`y el bucle interior se `{{#each todos}}`.</span><span class="sxs-lookup"><span data-stu-id="23b3d-192">The outside loop is `{{#each controller}}`, and the inside loop is `{{#each todos}}`.</span></span> <span data-ttu-id="23b3d-193">En el código siguiente se muestra una vista `Ember.Checkbox` integrada, una `App.TodoItemEditView`personalizada y un vínculo con una acción `deleteTodo`.</span><span class="sxs-lookup"><span data-stu-id="23b3d-193">The following code shows a built-in `Ember.Checkbox` view, a customized `App.TodoItemEditView`, and a link with a `deleteTodo` action.</span></span>

[!code-html[Main](emberjs-template/samples/sample12.html)]

<span data-ttu-id="23b3d-194">La clase `HtmlHelperExtensions`, que se define en Controllers/HtmlHelperExtensions. CS, define una función auxiliar para almacenar en memoria caché e insertar archivos de plantilla cuando **Debug** está establecido en **true** en el archivo Web. config.</span><span class="sxs-lookup"><span data-stu-id="23b3d-194">The `HtmlHelperExtensions` class, defined in Controllers/HtmlHelperExtensions.cs, defines a helper function to cache and insert template files when **debug** is set to **true** in the Web.config file.</span></span> <span data-ttu-id="23b3d-195">Esta función se llama desde el archivo de vista de ASP.NET MVC definido en views/Home/app. cshtml:</span><span class="sxs-lookup"><span data-stu-id="23b3d-195">This function is called from the ASP.NET MVC view file defined in Views/Home/App.cshtml:</span></span>

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

<span data-ttu-id="23b3d-196">Se llama sin argumentos, la función representa todos los archivos de plantilla en la carpeta de plantillas.</span><span class="sxs-lookup"><span data-stu-id="23b3d-196">Called with no arguments, the function renders all of the template files in the Templates folder.</span></span> <span data-ttu-id="23b3d-197">También puede especificar una subcarpeta o un archivo de plantilla específico.</span><span class="sxs-lookup"><span data-stu-id="23b3d-197">You can also specify a subfolder or a specific template file.</span></span>

<span data-ttu-id="23b3d-198">Cuando **Debug** es **false** en Web. config, la aplicación incluye el elemento de agrupación "~/bundles/templates".</span><span class="sxs-lookup"><span data-stu-id="23b3d-198">When **debug** is **false** in Web.config, the application includes the bundle item "~/bundles/templates".</span></span> <span data-ttu-id="23b3d-199">Este elemento de paquete se agrega en BundleConfig.cs, mediante la biblioteca del compilador del manillar:</span><span class="sxs-lookup"><span data-stu-id="23b3d-199">This bundle item is added in BundleConfig.cs, using the Handlebars compiler library:</span></span>

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
