---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Uso de un CAPTCHA para impedir que los bots usen el sitio de ASP.NET Web Razor) | Microsoft Docs
author: microsoft
description: En este artículo se explica cómo usar ReCaptcha (una medida de seguridad) para evitar que programas automatizados (bots) realicen tareas en una ASP.NET Web Pages (Razor)...
ms.author: riande
ms.date: 05/21/2012
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: 2647a3155893a3dfb3214795a5f9cf1e8931fa91
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78440197"
---
# <a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a><span data-ttu-id="96eee-103">Uso de un CAPTCHA para impedir que los bots usen el sitio de ASP.NET Web Razor)</span><span class="sxs-lookup"><span data-stu-id="96eee-103">Using a CAPTCHA to Prevent Bots from Using Your ASP.NET Web Razor) Site</span></span>

<span data-ttu-id="96eee-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="96eee-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="96eee-105">En este artículo se explica cómo usar ReCaptcha (una medida de seguridad) para evitar que programas automatizados (bots) realicen tareas en un sitio web de ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="96eee-105">This article explains how to use ReCaptcha (a security measure) to prevent automated programs (bots) from performing tasks in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="96eee-106">**Lo que aprenderá:**</span><span class="sxs-lookup"><span data-stu-id="96eee-106">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="96eee-107">Cómo agregar una prueba de CAPTCHA a su sitio.</span><span class="sxs-lookup"><span data-stu-id="96eee-107">How to add a CAPTCHA test to your site.</span></span>
> 
> <span data-ttu-id="96eee-108">Estas son las características de ASP.NET presentadas en el artículo:</span><span class="sxs-lookup"><span data-stu-id="96eee-108">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="96eee-109">Aplicación auxiliar de `ReCaptcha`.</span><span class="sxs-lookup"><span data-stu-id="96eee-109">The `ReCaptcha` helper.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="96eee-110">La información de este artículo se aplica a ASP.NET Web Pages 1,0 y páginas web 2.</span><span class="sxs-lookup"><span data-stu-id="96eee-110">The information in this article applies to ASP.NET Web Pages 1.0 and Web Pages 2.</span></span>

## <a name="about-captchas"></a><span data-ttu-id="96eee-111">Acerca de CAPTCHAs</span><span class="sxs-lookup"><span data-stu-id="96eee-111">About CAPTCHAs</span></span>

<span data-ttu-id="96eee-112">Cada vez que permita que los usuarios se registren en su sitio, o que solo escriban un nombre y una dirección URL (por ejemplo, para un Comentario de blog), podría obtener una avalancha de nombres falsos.</span><span class="sxs-lookup"><span data-stu-id="96eee-112">Any time you let people register in your site, or even just enter a name and URL (like for a blog comment), you might get a flood of fake names.</span></span> <span data-ttu-id="96eee-113">A menudo se les dejan programas automatizados (bots) que intentan dejar las direcciones URL en todos los sitios web que pueden encontrar.</span><span class="sxs-lookup"><span data-stu-id="96eee-113">These are often left by automated programs (bots) that try to leave URLs in every website they can find.</span></span> <span data-ttu-id="96eee-114">(Una motivación común es publicar las direcciones URL de los productos para su venta).</span><span class="sxs-lookup"><span data-stu-id="96eee-114">(A common motivation is to post the URLs of products for sale.)</span></span>

<span data-ttu-id="96eee-115">Puede ayudar a asegurarse de que un usuario es una persona real y no un programa informático mediante el uso de un *CAPTCHA* para validar a los usuarios cuando se registran o escriben su nombre y sitio.</span><span class="sxs-lookup"><span data-stu-id="96eee-115">You can help make sure that a user is real person and not a computer program by using a *CAPTCHA* to validate users when they register or otherwise enter their name and site.</span></span> <span data-ttu-id="96eee-116">CAPTCHA es una prueba de convertir pública completamente automatizada para indicar a los equipos y a los seres humanos que están separados.</span><span class="sxs-lookup"><span data-stu-id="96eee-116">CAPTCHA stands for Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="96eee-117">Un CAPTCHA es una prueba de *desafío-respuesta* en la que se pide al usuario que haga algo que sea fácil de realizar para una persona, pero que sea difícil de hacer con un programa automatizado.</span><span class="sxs-lookup"><span data-stu-id="96eee-117">A CAPTCHA is a *challenge-response* test in which the user is asked to do something that is easy for a person to do but hard for an automated program to do.</span></span> <span data-ttu-id="96eee-118">El tipo más común de CAPTCHA es aquel en el que se ven algunas letras distorsionadas y se les pide que las escriba.</span><span class="sxs-lookup"><span data-stu-id="96eee-118">The most common type of CAPTCHA is one where you see some distorted letters and are asked to type them.</span></span> <span data-ttu-id="96eee-119">(Se supone que la distorsión dificulta que los bots descifran las letras).</span><span class="sxs-lookup"><span data-stu-id="96eee-119">(The distortion is supposed to make it hard for bots to decipher the letters.)</span></span>

## <a name="adding-a-recaptcha-test"></a><span data-ttu-id="96eee-120">Agregar una prueba de ReCaptcha</span><span class="sxs-lookup"><span data-stu-id="96eee-120">Adding a ReCaptcha Test</span></span>

<span data-ttu-id="96eee-121">En las páginas de ASP.NET, puede usar la aplicación auxiliar de `ReCaptcha` para representar una prueba de CAPTCHA basada en el servicio ReCaptcha ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="96eee-121">In ASP.NET pages, you can use the `ReCaptcha` helper to render a CAPTCHA test that is based on the ReCaptcha service ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="96eee-122">La aplicación auxiliar de `ReCaptcha` muestra una imagen de dos palabras distorsionadas que los usuarios tienen que escribir correctamente antes de que se valide la página.</span><span class="sxs-lookup"><span data-stu-id="96eee-122">The `ReCaptcha` helper displays an image of two distorted words that users have to enter correctly before the page is validated.</span></span> <span data-ttu-id="96eee-123">El servicio ReCaptcha.Net valida la respuesta del usuario.</span><span class="sxs-lookup"><span data-stu-id="96eee-123">The user response is validated by the ReCaptcha.Net service.</span></span>

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. <span data-ttu-id="96eee-124">Registre el sitio web en ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="96eee-124">Register your website at ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="96eee-125">Cuando haya completado el registro, obtendrá una clave pública y una clave privada.</span><span class="sxs-lookup"><span data-stu-id="96eee-125">When you've completed registration, you'll get a public key and a private key.</span></span>
2. <span data-ttu-id="96eee-126">Agregue la biblioteca de aplicaciones auxiliares Web de ASP.NET a su sitio web, tal como se describe en [instalación de aplicaciones auxiliares en un sitio de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.</span><span class="sxs-lookup"><span data-stu-id="96eee-126">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
3. <span data-ttu-id="96eee-127">Si aún no tiene un archivo *\_AppStart. cshtml* , en la carpeta raíz de un sitio web, cree un archivo denominado *\_AppStart. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="96eee-127">If you don't already have a *\_AppStart.cshtml* file, in the root folder of a website create a file named *\_AppStart.cshtml*.</span></span>
4. <span data-ttu-id="96eee-128">Agregue la siguiente configuración auxiliar de `Recaptcha` en el archivo *\_AppStart. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="96eee-128">Add the following `Recaptcha` helper settings in the *\_AppStart.cshtml* file:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. <span data-ttu-id="96eee-129">Establezca las propiedades `PublicKey` y `PrivateKey` con sus propias claves públicas y privadas.</span><span class="sxs-lookup"><span data-stu-id="96eee-129">Set the `PublicKey` and `PrivateKey` properties using your own public and private keys.</span></span>
6. <span data-ttu-id="96eee-130">Guarde el archivo *\_AppStart. cshtml* y ciérrelo.</span><span class="sxs-lookup"><span data-stu-id="96eee-130">Save the *\_AppStart.cshtml* file and close it.</span></span>
7. <span data-ttu-id="96eee-131">En la carpeta raíz de un sitio web, cree una nueva página denominada *reCAPTCHA. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="96eee-131">In the root folder of a website, create new page named *Recaptcha.cshtml*.</span></span>
8. <span data-ttu-id="96eee-132">Reemplace el contenido existente por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="96eee-132">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. <span data-ttu-id="96eee-133">Ejecute la página *reCAPTCHA. cshtml* en un explorador.</span><span class="sxs-lookup"><span data-stu-id="96eee-133">Run the *Recaptcha.cshtml* page in a browser.</span></span> <span data-ttu-id="96eee-134">Si el valor de `PrivateKey` es válido, la página muestra el control ReCaptcha y un botón.</span><span class="sxs-lookup"><span data-stu-id="96eee-134">If the `PrivateKey` value is valid, the page displays the ReCaptcha control and a button.</span></span> <span data-ttu-id="96eee-135">Si no ha establecido las claves globalmente en *\_AppStart. html*, la página mostrará un error.</span><span class="sxs-lookup"><span data-stu-id="96eee-135">If you had not set the keys globally in *\_AppStart.html*, the page would display an error.</span></span> 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. <span data-ttu-id="96eee-136">Escriba las palabras de la prueba.</span><span class="sxs-lookup"><span data-stu-id="96eee-136">Enter the words for the test.</span></span> <span data-ttu-id="96eee-137">Si pasa la prueba de ReCaptcha, verá un mensaje para ese efecto.</span><span class="sxs-lookup"><span data-stu-id="96eee-137">If you pass the ReCaptcha test, you see a message to that effect.</span></span> <span data-ttu-id="96eee-138">De lo contrario, verá un mensaje de error y se vuelve a mostrar el control ReCaptcha.</span><span class="sxs-lookup"><span data-stu-id="96eee-138">Otherwise you see an error message and the ReCaptcha control is redisplayed.</span></span>

> [!NOTE]
> <span data-ttu-id="96eee-139">Si el equipo se encuentra en un dominio que utiliza el servidor proxy, es posible que deba configurar el elemento `defaultproxy` del archivo *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="96eee-139">If your computer is on a domain that uses proxy server, you might need to configure the `defaultproxy` element of the *Web.config* file.</span></span> <span data-ttu-id="96eee-140">En el ejemplo siguiente se muestra un archivo *Web. config* con el elemento `defaultproxy` configurado para permitir que el servicio ReCaptcha funcione.</span><span class="sxs-lookup"><span data-stu-id="96eee-140">The following example shows a *Web.config* file with the `defaultproxy` element configured to enable the ReCaptcha service to work.</span></span>
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="96eee-141">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="96eee-141">Additional Resources</span></span>

- [<span data-ttu-id="96eee-142">Personalizar el comportamiento de todo el sitio para sitios de ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="96eee-142">Customizing Site-Wide Behavior for ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="96eee-143">Sitio de ReCaptcha</span><span class="sxs-lookup"><span data-stu-id="96eee-143">ReCaptcha site</span></span>](https://www.google.com/recaptcha)
