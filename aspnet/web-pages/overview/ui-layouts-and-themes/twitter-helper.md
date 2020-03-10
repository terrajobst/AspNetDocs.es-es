---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Aplicación auxiliar de Twitter con ASP.NET Web Pages | Microsoft Docs
author: Rick-Anderson
description: En este tema y aplicación se muestra cómo agregar una aplicación auxiliar de Twitter a su proyecto WebMatrix 3. Contiene el código auxiliar de Twitter y muestra cómo llamar a la aplicación auxiliar...
ms.author: riande
ms.date: 11/26/2018
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 76e32b7c808467a9a87c70017dac02bdb895e1df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78518623"
---
# <a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="91f31-104">Asistente de Twitter con ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="91f31-104">Twitter Helper with ASP.NET Web Pages</span></span>

<span data-ttu-id="91f31-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="91f31-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="91f31-106">Las aplicaciones auxiliares de Twitter están obsoletas.</span><span class="sxs-lookup"><span data-stu-id="91f31-106">Twitter Helpers are obsolete.</span></span> <span data-ttu-id="91f31-107">Para ver las últimas herramientas de Engagement de Twitter para sitios web, consulte [información general de Twitter para sitios web](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span><span class="sxs-lookup"><span data-stu-id="91f31-107">For Twitter's latest engagement tools for websites, see [Twitter for Websites Overview](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span></span>

> <span data-ttu-id="91f31-108">En este tema y aplicación se muestra cómo agregar una aplicación auxiliar de Twitter a su proyecto WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="91f31-108">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="91f31-109">Contiene el código auxiliar de Twitter y muestra cómo llamar a los métodos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="91f31-109">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="91f31-110">Este código para el archivo Twitter. cshtml fue desarrollado por **Tian pan** de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="91f31-110">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="91f31-111">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="91f31-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="91f31-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="91f31-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="91f31-113">Este tutorial también funciona con ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="91f31-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>

## <a name="introduction"></a><span data-ttu-id="91f31-114">Introducción</span><span class="sxs-lookup"><span data-stu-id="91f31-114">Introduction</span></span>

<span data-ttu-id="91f31-115">En este tema se muestra cómo agregar una aplicación auxiliar de Twitter a la aplicación y usar sintaxis Razor para llamar a los métodos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="91f31-115">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="91f31-116">El ayudante de Twitter facilita la incorporación de los botones y widgets de Twitter en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="91f31-116">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="91f31-117">Para usar un widget de Twitter, como la escala de tiempo de un usuario o los resultados de búsqueda de un hashtag, primero debe crear el [Widget en Twitter](https://twitter.com/settings/widgets).</span><span class="sxs-lookup"><span data-stu-id="91f31-117">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="91f31-118">Después de crear el widget, recibirá un identificador de widget. Pasa este identificador de widget como parámetro al llamar a los métodos auxiliares que muestran widget.</span><span class="sxs-lookup"><span data-stu-id="91f31-118">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="91f31-119">Este tema se ha escrito para la versión 1,1 de la API de Twitter.</span><span class="sxs-lookup"><span data-stu-id="91f31-119">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="91f31-120">Al agregar directamente el código auxiliar de Twitter al proyecto, puede actualizar el código auxiliar si cambia la API de Twitter.</span><span class="sxs-lookup"><span data-stu-id="91f31-120">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="91f31-121">Para obtener información sobre cómo instalar WebMatrix, consulte [Introducción a ASP.NET Web Pages 2-Introducción](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="91f31-121">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="91f31-122">Incorporación de la aplicación auxiliar de Twitter al proyecto</span><span class="sxs-lookup"><span data-stu-id="91f31-122">Add Twitter Helper to your project</span></span>

<span data-ttu-id="91f31-123">Para agregar la aplicación auxiliar de Twitter, en primer lugar, agregue una carpeta denominada **App\_código** al proyecto.</span><span class="sxs-lookup"><span data-stu-id="91f31-123">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="91f31-124">A continuación, cree un archivo denominado **Twitter. cshtml**.</span><span class="sxs-lookup"><span data-stu-id="91f31-124">Then, create a file named **Twitter.cshtml**.</span></span>

![Carpeta App_Code](twitter-helper/_static/image1.png)

<span data-ttu-id="91f31-126">Reemplace el código predeterminado de Twitter. cshtml por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="91f31-126">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="91f31-127">Llamar a métodos de Twitter desde las páginas web</span><span class="sxs-lookup"><span data-stu-id="91f31-127">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="91f31-128">En el ejemplo siguiente se muestra cómo usar los métodos auxiliares de Twitter desde una página del proyecto.</span><span class="sxs-lookup"><span data-stu-id="91f31-128">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="91f31-129">En el proyecto, querrá reemplazar los valores de parámetro por valores que sean relevantes para sus necesidades.</span><span class="sxs-lookup"><span data-stu-id="91f31-129">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="91f31-130">Puede usar los identificadores de widget proporcionados para explorar cómo funcionan los métodos, pero querrá generar sus propios widgets para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="91f31-130">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="91f31-131">No todos los parámetros que se muestran a continuación son obligatorios.</span><span class="sxs-lookup"><span data-stu-id="91f31-131">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="91f31-132">Los parámetros opcionales se usan para personalizar cómo se muestra el botón o el widget.</span><span class="sxs-lookup"><span data-stu-id="91f31-132">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="91f31-133">Por ejemplo, el botón seguir solo requiere que el nombre de usuario siga, pero en el ejemplo se muestra cómo incluir el número de seguidores y cómo especificar el tamaño del botón y el idioma.</span><span class="sxs-lookup"><span data-stu-id="91f31-133">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="91f31-134">Ver los resultados</span><span class="sxs-lookup"><span data-stu-id="91f31-134">See the results</span></span>

<span data-ttu-id="91f31-135">El código anterior genera los siguientes botones y widgets.</span><span class="sxs-lookup"><span data-stu-id="91f31-135">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="91f31-136">Estos botones y widgets son totalmente funcionales, no capturas de pantallas.</span><span class="sxs-lookup"><span data-stu-id="91f31-136">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="91f31-137">El botón siguiente se muestra en Español porque el parámetro de idioma se estableció en **es**.</span><span class="sxs-lookup"><span data-stu-id="91f31-137">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="91f31-138">Botón seguir</span><span class="sxs-lookup"><span data-stu-id="91f31-138">Follow Button</span></span>

<span data-ttu-id="91f31-139">[Siga @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="91f31-139">[Follow @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="tweet-button"></a><span data-ttu-id="91f31-140">Botón Tweet</span><span class="sxs-lookup"><span data-stu-id="91f31-140">Tweet Button</span></span>

<span data-ttu-id="91f31-141">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="91f31-141">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="user-timeline-profile"></a><span data-ttu-id="91f31-142">Escala de tiempo de usuario (perfil)</span><span class="sxs-lookup"><span data-stu-id="91f31-142">User Timeline (Profile)</span></span>

<span data-ttu-id="91f31-143">[Tweets por @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="91f31-143">[Tweets by @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="favorites"></a><span data-ttu-id="91f31-144">Favoritos</span><span class="sxs-lookup"><span data-stu-id="91f31-144">Favorites</span></span>

<span data-ttu-id="91f31-145">[Tweets favoritos por @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="91f31-145">[Favorite Tweets by @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="list"></a><span data-ttu-id="91f31-146">Lista</span><span class="sxs-lookup"><span data-stu-id="91f31-146">List</span></span>

<span data-ttu-id="91f31-147">[Tweets de @Microsoft/MS\_bandas\_consumidor](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="91f31-147">[Tweets from @Microsoft/MS\_Consumer\_Bands](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="search"></a><span data-ttu-id="91f31-148">Buscar</span><span class="sxs-lookup"><span data-stu-id="91f31-148">Search</span></span>

<span data-ttu-id="91f31-149">[Tweets sobre &quot;#asp .net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="91f31-149">[Tweets about &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>
