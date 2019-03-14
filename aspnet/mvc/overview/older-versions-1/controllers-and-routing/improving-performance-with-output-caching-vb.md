---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-vb
title: Mejora del rendimiento con salida de almacenamiento en caché (VB) | Microsoft Docs
author: microsoft
description: En este tutorial, obtendrá información sobre cómo puede mejorar considerablemente el rendimiento de las aplicaciones web de ASP.NET MVC aprovechando las ventajas de la caché de resultados. Que...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 0e7b4d85-2c46-4eaf-b6a8-6cd566a67334
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-vb
msc.type: authoredcontent
ms.openlocfilehash: 9402d7e053ef11eeefa92d112b05ec255d5ec6f7
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57033672"
---
<a name="improving-performance-with-output-caching-vb"></a><span data-ttu-id="ee433-104">Mejorar el rendimiento con el almacenamiento en caché de resultados (VB)</span><span class="sxs-lookup"><span data-stu-id="ee433-104">Improving Performance with Output Caching (VB)</span></span>
====================
<span data-ttu-id="ee433-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="ee433-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="ee433-106">En este tutorial, obtendrá información sobre cómo puede mejorar considerablemente el rendimiento de las aplicaciones web de ASP.NET MVC aprovechando las ventajas de la caché de resultados.</span><span class="sxs-lookup"><span data-stu-id="ee433-106">In this tutorial, you learn how you can dramatically improve the performance of your ASP.NET MVC web applications by taking advantage of output caching.</span></span> <span data-ttu-id="ee433-107">Aprenda a almacenar en caché el resultado devuelto de una acción de controlador para que no tenga el mismo contenido a crearse cada vez que un nuevo usuario invoca la acción.</span><span class="sxs-lookup"><span data-stu-id="ee433-107">You learn how to cache the result returned from a controller action so that the same content does not need to be created each and every time a new user invokes the action.</span></span>


<span data-ttu-id="ee433-108">El objetivo de este tutorial es explicar cómo puede mejorar considerablemente el rendimiento de una aplicación ASP.NET MVC aprovechando las ventajas de la caché de resultados.</span><span class="sxs-lookup"><span data-stu-id="ee433-108">The goal of this tutorial is to explain how you can dramatically improve the performance of an ASP.NET MVC application by taking advantage of the output cache.</span></span> <span data-ttu-id="ee433-109">La caché de resultados le permite almacenar en caché el contenido devuelto por una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="ee433-109">The output cache enables you to cache the content returned by a controller action.</span></span> <span data-ttu-id="ee433-110">De este modo, no necesita el mismo contenido va a generar cada vez que se invoca la misma acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="ee433-110">That way, the same content does not need to be generated each and every time the same controller action is invoked.</span></span>

<span data-ttu-id="ee433-111">Por ejemplo, imagine que su aplicación ASP.NET MVC muestra una lista de registros de base de datos en una vista llamada Index.</span><span class="sxs-lookup"><span data-stu-id="ee433-111">Imagine, for example, that your ASP.NET MVC application displays a list of database records in a view named Index.</span></span> <span data-ttu-id="ee433-112">Normalmente, cada vez que un usuario invoca la acción del controlador que devuelve la vista de índice, el conjunto de registros de base de datos se debe recuperar de la base de datos mediante la ejecución de una consulta de base de datos.</span><span class="sxs-lookup"><span data-stu-id="ee433-112">Normally, each and every time that a user invokes the controller action that returns the Index view, the set of database records must be retrieved from the database by executing a database query.</span></span>

<span data-ttu-id="ee433-113">Si, por otro lado, aprovechar las ventajas de la caché de resultados, a continuación, no puede ejecutar una consulta de base de datos cada vez que cualquier usuario invoca la misma acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="ee433-113">If, on the other hand, you take advantage of the output cache then you can avoid executing a database query every time any user invokes the same controller action.</span></span> <span data-ttu-id="ee433-114">La vista se puede recuperar de la memoria caché en lugar de regeneración de la acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="ee433-114">The view can be retrieved from the cache instead of being regenerated from the controller action.</span></span> <span data-ttu-id="ee433-115">Almacenamiento en caché permite evitar la ejecución redundante trabaja en el servidor.</span><span class="sxs-lookup"><span data-stu-id="ee433-115">Caching enables you to avoid performing redundant work on the server.</span></span>

#### <a name="enabling-output-caching"></a><span data-ttu-id="ee433-116">Habilitación de la caché de resultados</span><span class="sxs-lookup"><span data-stu-id="ee433-116">Enabling Output Caching</span></span>

<span data-ttu-id="ee433-117">Habilitar almacenamiento en caché de salida mediante la adición de un &lt;OutputCache&gt; atributo a una acción de controlador individuales o una clase de controlador completo.</span><span class="sxs-lookup"><span data-stu-id="ee433-117">You enable output caching by adding an &lt;OutputCache&gt; attribute to either an individual controller action or an entire controller class.</span></span> <span data-ttu-id="ee433-118">Por ejemplo, el controlador en el listado 1 expone una acción denominada Index().</span><span class="sxs-lookup"><span data-stu-id="ee433-118">For example, the controller in Listing 1 exposes an action named Index().</span></span> <span data-ttu-id="ee433-119">El resultado de la acción de Index() se almacena en caché durante 10 segundos.</span><span class="sxs-lookup"><span data-stu-id="ee433-119">The output of the Index() action is cached for 10 seconds.</span></span>

<span data-ttu-id="ee433-120">**Listing 1 – Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="ee433-120">**Listing 1 – Controllers\HomeController.vb**</span></span>

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample1.vb)]


<span data-ttu-id="ee433-121">En las versiones Beta de ASP.NET MVC, la caché de resultados no funciona para una dirección URL como [ http://www.MySite.com/ ](http://www.mysite.com/).</span><span class="sxs-lookup"><span data-stu-id="ee433-121">In the Beta versions of ASP.NET MVC, output caching does not work for a URL like [http://www.MySite.com/](http://www.mysite.com/).</span></span> <span data-ttu-id="ee433-122">En su lugar, debe especificar una dirección URL como [ http://www.MySite.com/Home/Index ](http://www.mysite.com/Home/Index).</span><span class="sxs-lookup"><span data-stu-id="ee433-122">Instead, you must enter a URL like [http://www.MySite.com/Home/Index](http://www.mysite.com/Home/Index).</span></span>


<span data-ttu-id="ee433-123">En el listado 1, se almacena en caché el resultado de la acción de Index() durante 10 segundos.</span><span class="sxs-lookup"><span data-stu-id="ee433-123">In Listing 1, the output of the Index() action is cached for 10 seconds.</span></span> <span data-ttu-id="ee433-124">Si lo prefiere, puede especificar una duración mucho mayor de la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="ee433-124">If you prefer, you can specify a much longer cache duration.</span></span> <span data-ttu-id="ee433-125">Por ejemplo, si desea almacenar en caché el resultado de una acción del controlador durante un día, a continuación, puede especificar una duración de caché de 86400 segundos (60 segundos \* 60 minutos \* 24 horas).</span><span class="sxs-lookup"><span data-stu-id="ee433-125">For example, if you want to cache the output of a controller action for one day then you can specify a cache duration of 86400 seconds (60 seconds \* 60 minutes \* 24 hours).</span></span>

<span data-ttu-id="ee433-126">No hay ninguna garantía de que el contenido se almacenará en caché para la cantidad de tiempo que especifique.</span><span class="sxs-lookup"><span data-stu-id="ee433-126">There is no guarantee that content will be cached for the amount of time that you specify.</span></span> <span data-ttu-id="ee433-127">Cuando los recursos de memoria son bajos, la memoria caché inicia la expulsar contenido automáticamente.</span><span class="sxs-lookup"><span data-stu-id="ee433-127">When memory resources become low, the cache starts evicting content automatically.</span></span>

<span data-ttu-id="ee433-128">El controlador Home en el listado 1 devuelve la vista de índice en el listado 2.</span><span class="sxs-lookup"><span data-stu-id="ee433-128">The Home controller in Listing 1 returns the Index view in Listing 2.</span></span> <span data-ttu-id="ee433-129">No hay nada especial acerca de esta vista.</span><span class="sxs-lookup"><span data-stu-id="ee433-129">There is nothing special about this view.</span></span> <span data-ttu-id="ee433-130">La vista de índice simplemente muestra la hora actual (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="ee433-130">The Index view simply displays the current time (see Figure 1).</span></span>

<span data-ttu-id="ee433-131">**Listing 2 – Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="ee433-131">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](improving-performance-with-output-caching-vb/samples/sample2.aspx)]

<span data-ttu-id="ee433-132">**Ilustración 1: vista de índice de almacenamiento en caché**</span><span class="sxs-lookup"><span data-stu-id="ee433-132">**Figure 1 – Cached Index view**</span></span>

![clip_image002](improving-performance-with-output-caching-vb/_static/image1.jpg)

<span data-ttu-id="ee433-134">Si se invoca la acción de Index() varias veces, escriba/Home de dirección URL/índice en la barra de direcciones del explorador y presionar el botón de actualización y volver a cargar en el explorador varias veces y, después, la hora mostrada por la vista de índice no cambiará durante 10 segundos.</span><span class="sxs-lookup"><span data-stu-id="ee433-134">If you invoke the Index() action multiple times by entering the URL /Home/Index in the address bar of your browser and hitting the Refresh/Reload button in your browser repeatedly, then the time displayed by the Index view won't change for 10 seconds.</span></span> <span data-ttu-id="ee433-135">Se muestra al mismo tiempo porque la vista se almacena en caché.</span><span class="sxs-lookup"><span data-stu-id="ee433-135">The same time is displayed because the view is cached.</span></span>

<span data-ttu-id="ee433-136">Es importante comprender que la misma vista se almacena en caché para los usuarios que visitan su aplicación.</span><span class="sxs-lookup"><span data-stu-id="ee433-136">It is important to understand that the same view is cached for everyone who visits your application.</span></span> <span data-ttu-id="ee433-137">Cualquier persona que invoca la acción de Index() obtendrá la misma versión en caché de la vista de índice.</span><span class="sxs-lookup"><span data-stu-id="ee433-137">Anyone who invokes the Index() action will get the same cached version of the Index view.</span></span> <span data-ttu-id="ee433-138">Esto significa que la cantidad de trabajo que el servidor web debe realizar para dar servicio a la vista de índice se reduce drásticamente.</span><span class="sxs-lookup"><span data-stu-id="ee433-138">This means that the amount of work that the web server must perform to serve the Index view is dramatically reduced.</span></span>

<span data-ttu-id="ee433-139">La vista en el listado 2 ocurre a hacer algo realmente sencillo.</span><span class="sxs-lookup"><span data-stu-id="ee433-139">The view in Listing 2 happens to be doing something really simple.</span></span> <span data-ttu-id="ee433-140">La vista solo muestra la hora actual.</span><span class="sxs-lookup"><span data-stu-id="ee433-140">The view just displays the current time.</span></span> <span data-ttu-id="ee433-141">Sin embargo, podría simplemente como caché fácilmente una vista que muestra un conjunto de registros de base de datos.</span><span class="sxs-lookup"><span data-stu-id="ee433-141">However, you could just as easily cache a view that displays a set of database records.</span></span> <span data-ttu-id="ee433-142">En ese caso, el conjunto de registros de base de datos no necesitaría que deben recuperarse de la base de datos cada vez que se invoca la acción del controlador que devuelve la vista.</span><span class="sxs-lookup"><span data-stu-id="ee433-142">In that case, the set of database records would not need to be retrieved from the database each and every time the controller action that returns the view is invoked.</span></span> <span data-ttu-id="ee433-143">Almacenamiento en caché puede reducir la cantidad de trabajo que deben llevar a cabo el servidor web y el servidor de base de datos.</span><span class="sxs-lookup"><span data-stu-id="ee433-143">Caching can reduce the amount of work that both your web server and database server must perform.</span></span>

<span data-ttu-id="ee433-144">No use la página &lt;% @ OutputCache %&gt; la directiva en una vista de MVC.</span><span class="sxs-lookup"><span data-stu-id="ee433-144">Don't use the page &lt;%@ OutputCache %&gt; directive in an MVC view.</span></span> <span data-ttu-id="ee433-145">Esta directiva es hemorragia a través del mundo de formularios Web Forms y no debe usarse en una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ee433-145">This directive is bleeding over from the Web Forms world and should not be used in an ASP.NET MVC application.</span></span> 

#### <a name="where-content-is-cached"></a><span data-ttu-id="ee433-146">Donde se almacena en caché de contenido</span><span class="sxs-lookup"><span data-stu-id="ee433-146">Where Content is Cached</span></span>

<span data-ttu-id="ee433-147">De forma predeterminada, cuando se usa el &lt;OutputCache&gt; atributo, el contenido se almacena en caché en tres ubicaciones: el servidor web, los servidores proxy y el explorador web.</span><span class="sxs-lookup"><span data-stu-id="ee433-147">By default, when you use the &lt;OutputCache&gt; attribute, content is cached in three locations: the web server, any proxy servers, and the web browser.</span></span> <span data-ttu-id="ee433-148">Puede controlar exactamente dónde haya almacenado en caché mediante la modificación de la propiedad Location de la &lt;OutputCache&gt; atributo.</span><span class="sxs-lookup"><span data-stu-id="ee433-148">You can control exactly where content is cached by modifying the Location property of the &lt;OutputCache&gt; attribute.</span></span>

<span data-ttu-id="ee433-149">Puede establecer la propiedad Location en cualquiera de los siguientes valores:</span><span class="sxs-lookup"><span data-stu-id="ee433-149">You can set the Location property to any one of the following values:</span></span>

> <span data-ttu-id="ee433-150">· Cualquier</span><span class="sxs-lookup"><span data-stu-id="ee433-150">· Any</span></span>
> 
> <span data-ttu-id="ee433-151">· Cliente</span><span class="sxs-lookup"><span data-stu-id="ee433-151">· Client</span></span>
> 
> <span data-ttu-id="ee433-152">· Nivel inferior</span><span class="sxs-lookup"><span data-stu-id="ee433-152">· Downstream</span></span>
> 
> <span data-ttu-id="ee433-153">· Servidor</span><span class="sxs-lookup"><span data-stu-id="ee433-153">· Server</span></span>
> 
> <span data-ttu-id="ee433-154">· Ninguno</span><span class="sxs-lookup"><span data-stu-id="ee433-154">· None</span></span>
> 
> <span data-ttu-id="ee433-155">· ServerAndClient</span><span class="sxs-lookup"><span data-stu-id="ee433-155">· ServerAndClient</span></span>


<span data-ttu-id="ee433-156">De forma predeterminada, la propiedad de ubicación tiene el valor Any.</span><span class="sxs-lookup"><span data-stu-id="ee433-156">By default, the Location property has the value Any.</span></span> <span data-ttu-id="ee433-157">Sin embargo, hay situaciones en que desee solo en el explorador o solo en el servidor de caché.</span><span class="sxs-lookup"><span data-stu-id="ee433-157">However, there are situations in which you might want to cache only on the browser or only on the server.</span></span> <span data-ttu-id="ee433-158">Por ejemplo, si se almacenan en caché información personalizada para cada usuario, a continuación, se debe no almacenar en caché la información en el servidor.</span><span class="sxs-lookup"><span data-stu-id="ee433-158">For example, if you are caching information that is personalized for each user then you should not cache the information on the server.</span></span> <span data-ttu-id="ee433-159">Si va a mostrar información diferente a distintos usuarios, a continuación, se debe almacenar en caché la información únicamente en el cliente.</span><span class="sxs-lookup"><span data-stu-id="ee433-159">If you are displaying different information to different users then you should cache the information only on the client.</span></span>

<span data-ttu-id="ee433-160">Por ejemplo, el controlador en el listado 3 expone una acción denominada GetName() que devuelve el nombre de usuario actual.</span><span class="sxs-lookup"><span data-stu-id="ee433-160">For example, the controller in Listing 3 exposes an action named GetName() that returns the current user name.</span></span> <span data-ttu-id="ee433-161">Si Jack inicia sesión en el sitio Web e invoca la acción GetName(), a continuación, la acción devuelve la cadena "Jack Hi".</span><span class="sxs-lookup"><span data-stu-id="ee433-161">If Jack logs into the website and invokes the GetName() action then the action returns the string "Hi Jack".</span></span> <span data-ttu-id="ee433-162">Si, posteriormente, Jill inicia sesión en el sitio Web e invoca la acción GetName(), a continuación, también obtendrá la cadena "Jack Hi".</span><span class="sxs-lookup"><span data-stu-id="ee433-162">If, subsequently, Jill logs into the website and invokes the GetName() action then she also will get the string "Hi Jack".</span></span> <span data-ttu-id="ee433-163">La cadena se almacena en caché en el servidor web para todos los usuarios después de Jack inicialmente invoca la acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="ee433-163">The string is cached on the web server for all users after Jack initially invokes the controller action.</span></span>

<span data-ttu-id="ee433-164">**Listing 3 – Controllers\BadUserController.vb**</span><span class="sxs-lookup"><span data-stu-id="ee433-164">**Listing 3 – Controllers\BadUserController.vb**</span></span>

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample3.vb)]

<span data-ttu-id="ee433-165">Probablemente, el controlador en el listado 3 no funciona la forma que desee.</span><span class="sxs-lookup"><span data-stu-id="ee433-165">Most likely, the controller in Listing 3 does not work the way that you want.</span></span> <span data-ttu-id="ee433-166">No desea que aparezca el mensaje "¡Hi Jack" a Jill.</span><span class="sxs-lookup"><span data-stu-id="ee433-166">You don't want to display the message "Hi Jack" to Jill.</span></span>

<span data-ttu-id="ee433-167">Nunca se debe almacenar en caché contenido personalizado en la caché del servidor.</span><span class="sxs-lookup"><span data-stu-id="ee433-167">You should never cache personalized content in the server cache.</span></span> <span data-ttu-id="ee433-168">Sin embargo, es posible que desee almacenar en caché el contenido personalizado en la caché del explorador para mejorar el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="ee433-168">However, you might want to cache the personalized content in the browser cache to improve performance.</span></span> <span data-ttu-id="ee433-169">Si almacenar en caché contenido en el explorador y un usuario invoca la acción del controlador mismo varias veces, se puede recuperar el contenido desde la caché del explorador en lugar del servidor.</span><span class="sxs-lookup"><span data-stu-id="ee433-169">If you cache content in the browser, and a user invokes the same controller action multiple times, then the content can be retrieved from the browser cache instead of the server.</span></span>

<span data-ttu-id="ee433-170">El controlador modificado en el listado 4 se almacena en caché el resultado de la acción GetName().</span><span class="sxs-lookup"><span data-stu-id="ee433-170">The modified controller in Listing 4 caches the output of the GetName() action.</span></span> <span data-ttu-id="ee433-171">Sin embargo, el contenido se almacena en caché solo en el explorador y no en el servidor.</span><span class="sxs-lookup"><span data-stu-id="ee433-171">However, the content is cached only on the browser and not on the server.</span></span> <span data-ttu-id="ee433-172">De este modo, cuando varios usuarios invocan el método GetName(), cada persona obtiene su propio nombre de usuario y no otro nombre del usuario.</span><span class="sxs-lookup"><span data-stu-id="ee433-172">That way, when multiple users invoke the GetName() method, each person gets their own user name and not another person's user name.</span></span>

<span data-ttu-id="ee433-173">**Listing 4 – Controllers\UserController.vb**</span><span class="sxs-lookup"><span data-stu-id="ee433-173">**Listing 4 – Controllers\UserController.vb**</span></span>

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample4.vb)]

<span data-ttu-id="ee433-174">Tenga en cuenta que el &lt;OutputCache&gt; atributo en el listado 4 incluye una propiedad de ubicación establecida en el valor OutputCacheLocation.Client.</span><span class="sxs-lookup"><span data-stu-id="ee433-174">Notice that the &lt;OutputCache&gt; attribute in Listing 4 includes a Location property set to the value OutputCacheLocation.Client.</span></span> <span data-ttu-id="ee433-175">El &lt;OutputCache&gt; atributo también incluye una propiedad NoStore.</span><span class="sxs-lookup"><span data-stu-id="ee433-175">The &lt;OutputCache&gt; attribute also includes a NoStore property.</span></span> <span data-ttu-id="ee433-176">La propiedad NoStore se usa para informar a los exploradores y servidores proxy que no debe almacenar una copia permanente del contenido almacenado en caché.</span><span class="sxs-lookup"><span data-stu-id="ee433-176">The NoStore property is used to inform proxy servers and browsers that they should not store a permanent copy of the cached content.</span></span>

#### <a name="varying-the-output-cache"></a><span data-ttu-id="ee433-177">Variar la memoria caché de salida</span><span class="sxs-lookup"><span data-stu-id="ee433-177">Varying the Output Cache</span></span>

<span data-ttu-id="ee433-178">En algunas situaciones, es posible que desee distintas versiones en caché del mismo contenido.</span><span class="sxs-lookup"><span data-stu-id="ee433-178">In some situations, you might want different cached versions of the very same content.</span></span> <span data-ttu-id="ee433-179">Por ejemplo, imagine que va a crear una página de maestro y detalles.</span><span class="sxs-lookup"><span data-stu-id="ee433-179">Imagine, for example, that you are creating a master/detail page.</span></span> <span data-ttu-id="ee433-180">La página principal muestra una lista de títulos de películas.</span><span class="sxs-lookup"><span data-stu-id="ee433-180">The master page displays a list of movie titles.</span></span> <span data-ttu-id="ee433-181">Al hacer clic en un título, obtener detalles de la película seleccionada.</span><span class="sxs-lookup"><span data-stu-id="ee433-181">When you click a title, you get details for the selected movie.</span></span>

<span data-ttu-id="ee433-182">Si almacena en caché la página de detalles, a continuación, los detalles de la misma película aparecerá con independencia de qué película haga clic en.</span><span class="sxs-lookup"><span data-stu-id="ee433-182">If you cache the details page, then the details for the same movie will be displayed no matter which movie you click.</span></span> <span data-ttu-id="ee433-183">La primera película seleccionada por el primer usuario se mostrará para todos los usuarios futuros.</span><span class="sxs-lookup"><span data-stu-id="ee433-183">The first movie selected by the first user will be displayed to all future users.</span></span>

<span data-ttu-id="ee433-184">Puede corregir este problema aprovechando las ventajas de la propiedad VaryByParam de la &lt;OutputCache&gt; atributo.</span><span class="sxs-lookup"><span data-stu-id="ee433-184">You can fix this problem by taking advantage of the VaryByParam property of the &lt;OutputCache&gt; attribute.</span></span> <span data-ttu-id="ee433-185">Esta propiedad le permite crear diferentes versiones en caché del mismo contenido cuando un parámetro de formulario o parámetro de cadena de consulta varía.</span><span class="sxs-lookup"><span data-stu-id="ee433-185">This property enables you to create different cached versions of the very same content when a form parameter or query string parameter varies.</span></span>

<span data-ttu-id="ee433-186">Por ejemplo, el controlador en el listado 5 expone dos acciones denominadas Master() y Details().</span><span class="sxs-lookup"><span data-stu-id="ee433-186">For example, the controller in Listing 5 exposes two actions named Master() and Details().</span></span> <span data-ttu-id="ee433-187">La acción Master() devuelve una lista de títulos de películas y la acción Details() devuelve los detalles de la película seleccionada.</span><span class="sxs-lookup"><span data-stu-id="ee433-187">The Master() action returns a list of movie titles and the Details() action returns the details for the selected movie.</span></span>

<span data-ttu-id="ee433-188">**Listing 5 – Controllers\MoviesController.vb**</span><span class="sxs-lookup"><span data-stu-id="ee433-188">**Listing 5 – Controllers\MoviesController.vb**</span></span>

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample5.vb)]

<span data-ttu-id="ee433-189">La acción Master() incluye una propiedad VaryByParam con el valor "none".</span><span class="sxs-lookup"><span data-stu-id="ee433-189">The Master() action includes a VaryByParam property with the value "none".</span></span> <span data-ttu-id="ee433-190">Se devuelve cuando se visualiza el Master() se invoca la acción, la misma versión en caché del patrón.</span><span class="sxs-lookup"><span data-stu-id="ee433-190">When the Master() action is invoked, the same cached version of the Master view is returned.</span></span> <span data-ttu-id="ee433-191">Los parámetros de formulario o la cadena de consulta son parámetros omite (consulte la figura 2).</span><span class="sxs-lookup"><span data-stu-id="ee433-191">Any form parameters or query string parameters are ignored (see Figure 2).</span></span>

<span data-ttu-id="ee433-192">**Ilustración 2: la vista /Movies/Master**</span><span class="sxs-lookup"><span data-stu-id="ee433-192">**Figure 2 – The /Movies/Master view**</span></span>

![clip_image004](improving-performance-with-output-caching-vb/_static/image2.jpg)

<span data-ttu-id="ee433-194">**Ilustración 3: la vista de detalles/Movies /**</span><span class="sxs-lookup"><span data-stu-id="ee433-194">**Figure 3 – The /Movies/Details view**</span></span>

![clip_image006](improving-performance-with-output-caching-vb/_static/image3.jpg)

<span data-ttu-id="ee433-196">La acción Details() incluye una propiedad VaryByParam con el valor "Id".</span><span class="sxs-lookup"><span data-stu-id="ee433-196">The Details() action includes a VaryByParam property with the value "Id".</span></span> <span data-ttu-id="ee433-197">Cuando los distintos valores del parámetro del identificador se pasan a la acción del controlador, se generan diferentes versiones en caché de la vista de detalles.</span><span class="sxs-lookup"><span data-stu-id="ee433-197">When different values of the Id parameter are passed to the controller action, different cached versions of the Details view are generated.</span></span>

<span data-ttu-id="ee433-198">Es importante entender que utilizando los resultados de la propiedad VaryByParam en más de almacenamiento en caché y no inferior.</span><span class="sxs-lookup"><span data-stu-id="ee433-198">It is important to understand that using the VaryByParam property results in more caching and not less.</span></span> <span data-ttu-id="ee433-199">Se crea una versión almacenada en caché diferente de la vista de detalles para cada versión del parámetro del identificador.</span><span class="sxs-lookup"><span data-stu-id="ee433-199">A different cached version of the Details view is created for each different version of the Id parameter.</span></span>

<span data-ttu-id="ee433-200">Puede establecer la propiedad VaryByParam en los siguientes valores:</span><span class="sxs-lookup"><span data-stu-id="ee433-200">You can set the VaryByParam property to the following values:</span></span>

> <span data-ttu-id="ee433-201">\* = Crear una versión almacenada en caché diferente cada vez que varía de un parámetro de cadena de consulta o formulario.</span><span class="sxs-lookup"><span data-stu-id="ee433-201">\* = Create a different cached version whenever a form or query string parameter varies.</span></span>
> 
> <span data-ttu-id="ee433-202">None = nunca crear diferentes versiones en caché</span><span class="sxs-lookup"><span data-stu-id="ee433-202">none = Never create different cached versions</span></span>
> 
> <span data-ttu-id="ee433-203">Lista de puntos y comas de parámetros = crear diferentes versiones en caché siempre que cualquiera de los parámetros de cadena de consulta o formulario en la lista varía</span><span class="sxs-lookup"><span data-stu-id="ee433-203">Semicolon list of parameters = Create different cached versions whenever any of the form or query string parameters in the list varies</span></span>


#### <a name="creating-a-cache-profile"></a><span data-ttu-id="ee433-204">Crear un perfil de caché</span><span class="sxs-lookup"><span data-stu-id="ee433-204">Creating a Cache Profile</span></span>

<span data-ttu-id="ee433-205">Como alternativa a la configuración de las propiedades de la caché de salida modificando las propiedades de la &lt;OutputCache&gt; atributo, puede crear un perfil de caché en el archivo de configuración (web.config) de la web.</span><span class="sxs-lookup"><span data-stu-id="ee433-205">As an alternative to configuring output cache properties by modifying properties of the &lt;OutputCache&gt; attribute, you can create a cache profile in the web configuration (web.config) file.</span></span> <span data-ttu-id="ee433-206">Crear un perfil de caché en el archivo de configuración web ofrece dos ventajas importantes.</span><span class="sxs-lookup"><span data-stu-id="ee433-206">Creating a cache profile in the web configuration file offers a couple of important advantages.</span></span>

<span data-ttu-id="ee433-207">En primer lugar, mediante la configuración de caché de resultados en el archivo de configuración web, puede controlar cómo almacenar las acciones de controlador en caché contenido en una ubicación central.</span><span class="sxs-lookup"><span data-stu-id="ee433-207">First, by configuring output caching in the web configuration file, you can control how controller actions cache content in one central location.</span></span> <span data-ttu-id="ee433-208">Puede crear un perfil de caché y aplicar el perfil a varios controladores o acciones de controlador.</span><span class="sxs-lookup"><span data-stu-id="ee433-208">You can create one cache profile and apply the profile to several controllers or controller actions.</span></span>

<span data-ttu-id="ee433-209">En segundo lugar, puede modificar el archivo de configuración web sin volver a compilar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ee433-209">Second, you can modify the web configuration file without recompiling your application.</span></span> <span data-ttu-id="ee433-210">Si tiene que deshabilitar el almacenamiento en caché para una aplicación que ya se ha implementado en producción, simplemente puede modificar los perfiles de caché definidos en el archivo de configuración web.</span><span class="sxs-lookup"><span data-stu-id="ee433-210">If you need to disable caching for an application that has already been deployed to production, then you can simply modify the cache profiles defined in the web configuration file.</span></span> <span data-ttu-id="ee433-211">Se detectarán automáticamente y aplica los cambios realizados en el archivo de configuración web.</span><span class="sxs-lookup"><span data-stu-id="ee433-211">Any changes to the web configuration file will be detected automatically and applied.</span></span>

<span data-ttu-id="ee433-212">Por ejemplo, el &lt;almacenamiento en caché&gt; sección de configuración web en el listado 6 define un perfil de caché denominado Cache1Hour.</span><span class="sxs-lookup"><span data-stu-id="ee433-212">For example, the &lt;caching&gt; web configuration section in Listing 6 defines a cache profile named Cache1Hour.</span></span> <span data-ttu-id="ee433-213">El &lt;almacenamiento en caché&gt; sección debe aparecer dentro de la &lt;system.web&gt; sección de un archivo de configuración web.</span><span class="sxs-lookup"><span data-stu-id="ee433-213">The &lt;caching&gt; section must appear within the &lt;system.web&gt; section of a web configuration file.</span></span>

<span data-ttu-id="ee433-214">**Listado 6: almacenamiento en caché de sección de web.config**</span><span class="sxs-lookup"><span data-stu-id="ee433-214">**Listing 6 – Caching section for web.config**</span></span>

[!code-xml[Main](improving-performance-with-output-caching-vb/samples/sample6.xml)]

<span data-ttu-id="ee433-215">El controlador en la lista 7 muestra cómo puede aplicar el perfil Cache1Hour a una acción de controlador con el &lt;OutputCache&gt; atributo.</span><span class="sxs-lookup"><span data-stu-id="ee433-215">The controller in Listing 7 illustrates how you can apply the Cache1Hour profile to a controller action with the &lt;OutputCache&gt; attribute.</span></span>

<span data-ttu-id="ee433-216">**Listing 7 – Controllers\ProfileController.vb**</span><span class="sxs-lookup"><span data-stu-id="ee433-216">**Listing 7 – Controllers\ProfileController.vb**</span></span>

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample7.vb)]

<span data-ttu-id="ee433-217">Si se invoca la acción de Index() expuesta por el controlador en la lista 7 se devolverá el mismo tiempo durante una hora.</span><span class="sxs-lookup"><span data-stu-id="ee433-217">If you invoke the Index() action exposed by the controller in Listing 7 then the same time will be returned for 1 hour.</span></span>

#### <a name="summary"></a><span data-ttu-id="ee433-218">Resumen</span><span class="sxs-lookup"><span data-stu-id="ee433-218">Summary</span></span>

<span data-ttu-id="ee433-219">Almacenamiento en caché de salida proporciona un método muy fácil de lo que mejora notablemente el rendimiento de las aplicaciones de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ee433-219">Output caching provides you with a very easy method of dramatically improving the performance of your ASP.NET MVC applications.</span></span> <span data-ttu-id="ee433-220">En este tutorial, ha aprendido a usar el &lt;OutputCache&gt; atributo para almacenar en caché el resultado de las acciones de controlador.</span><span class="sxs-lookup"><span data-stu-id="ee433-220">In this tutorial, you learned how to use the &lt;OutputCache&gt; attribute to cache the output of controller actions.</span></span> <span data-ttu-id="ee433-221">También ha aprendido cómo modificar las propiedades de la &lt;OutputCache&gt; atributo como las propiedades de duración y VaryByParam para modificar cómo se almacena en caché de contenido.</span><span class="sxs-lookup"><span data-stu-id="ee433-221">You also learned how to modify properties of the &lt;OutputCache&gt; attribute such as the Duration and VaryByParam properties to modify how content gets cached.</span></span> <span data-ttu-id="ee433-222">Por último, ha aprendido a definir perfiles de caché en el archivo de configuración web.</span><span class="sxs-lookup"><span data-stu-id="ee433-222">Finally, you learned how to define cache profiles in the web configuration file.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ee433-223">[Anterior](understanding-action-filters-vb.md)
> [Siguiente](adding-dynamic-content-to-a-cached-page-vb.md)</span><span class="sxs-lookup"><span data-stu-id="ee433-223">[Previous](understanding-action-filters-vb.md)
[Next](adding-dynamic-content-to-a-cached-page-vb.md)</span></span>