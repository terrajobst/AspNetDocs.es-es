---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
title: Probar la seguridad de una contraseña (VB) | Microsoft Docs
author: wenz
description: Las contraseñas son necesarias casi en cualquier lugar, por lo que los usuarios diferidos tienden a elegir contraseñas sencillas que son fáciles de interrumpir. Control PasswordStrength en el ASP. N...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 9215a37f-3133-4887-8ed2-3689f3a53551
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
msc.type: authoredcontent
ms.openlocfilehash: b614e1788eeafc175dd792ec6d3e4619f9ea2b7a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78508975"
---
# <a name="testing-the-strength-of-a-password-vb"></a><span data-ttu-id="29bb3-104">Probar la seguridad de una contraseña (VB)</span><span class="sxs-lookup"><span data-stu-id="29bb3-104">Testing the Strength of a Password (VB)</span></span>

<span data-ttu-id="29bb3-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="29bb3-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="29bb3-106">[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="29bb3-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span></span>

> <span data-ttu-id="29bb3-107">Las contraseñas son necesarias casi en cualquier lugar, por lo que los usuarios diferidos tienden a elegir contraseñas sencillas que son fáciles de interrumpir.</span><span class="sxs-lookup"><span data-stu-id="29bb3-107">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="29bb3-108">El control PasswordStrength en el kit de herramientas de control de AJAX de ASP.NET puede comprobar la calidad de una contraseña.</span><span class="sxs-lookup"><span data-stu-id="29bb3-108">The PasswordStrength control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="overview"></a><span data-ttu-id="29bb3-109">Información general</span><span class="sxs-lookup"><span data-stu-id="29bb3-109">Overview</span></span>

<span data-ttu-id="29bb3-110">Las contraseñas son necesarias casi en cualquier lugar, por lo que los usuarios diferidos tienden a elegir contraseñas sencillas que son fáciles de interrumpir.</span><span class="sxs-lookup"><span data-stu-id="29bb3-110">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="29bb3-111">El control `PasswordStrength` en el kit de herramientas de control de ASP.NET AJAX puede comprobar la calidad de una contraseña.</span><span class="sxs-lookup"><span data-stu-id="29bb3-111">The `PasswordStrength` control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="steps"></a><span data-ttu-id="29bb3-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="29bb3-112">Steps</span></span>

<span data-ttu-id="29bb3-113">El control `PasswordStrength` extiende un cuadro de texto y comprueba si la contraseña es suficientemente adecuada.</span><span class="sxs-lookup"><span data-stu-id="29bb3-113">The `PasswordStrength` control extends a text box and checks whether the password in it is good enough.</span></span> <span data-ttu-id="29bb3-114">Ofrece una gran cantidad de opciones a través de atributos. Estos son solo algunos de ellos:</span><span class="sxs-lookup"><span data-stu-id="29bb3-114">It offers a wealth of options via attributes; here are just some of them:</span></span>

- <span data-ttu-id="29bb3-115">`MinimumNumericCharacters` número mínimo de caracteres numéricos necesarios en la contraseña</span><span class="sxs-lookup"><span data-stu-id="29bb3-115">`MinimumNumericCharacters` minimum number of numeric characters required in the password</span></span>
- <span data-ttu-id="29bb3-116">`MinimumSymbolCharacters` número mínimo de caracteres de símbolos (no Letras y dígitos) necesarios en la contraseña</span><span class="sxs-lookup"><span data-stu-id="29bb3-116">`MinimumSymbolCharacters` minimum number of symbol characters (not letters and digits) required in the password</span></span>
- <span data-ttu-id="29bb3-117">`PreferredPasswordLength` longitud mínima de la contraseña</span><span class="sxs-lookup"><span data-stu-id="29bb3-117">`PreferredPasswordLength` minimum length of the password</span></span>
- <span data-ttu-id="29bb3-118">`RequiresUpperAndLowerCaseCharacters` si la contraseña necesita usar caracteres en mayúsculas y minúsculas</span><span class="sxs-lookup"><span data-stu-id="29bb3-118">`RequiresUpperAndLowerCaseCharacters` whether the password needs to use both uppercase and lowercase characters</span></span>

<span data-ttu-id="29bb3-119">El `StrengthIndicatorType` proporciona la información sobre cómo presentar la fuerza de la contraseña, como texto (valor `"Text"`) o como un tipo de barra de progreso (valor `"BarIndicator"`).</span><span class="sxs-lookup"><span data-stu-id="29bb3-119">The `StrengthIndicatorType` provides the information how to present the strength of the password, as text (value `"Text"`) or as a kind of progress bar (value `"BarIndicator"`).</span></span> <span data-ttu-id="29bb3-120">En el atributo `DisplayPosition`, configure dónde aparece la información.</span><span class="sxs-lookup"><span data-stu-id="29bb3-120">In the `DisplayPosition` attribute, you configure where the information appears.</span></span> <span data-ttu-id="29bb3-121">Este es un ejemplo completo, incluido el control de `ScriptManager` de ASP.NET AJAX, el `PasswordStrength` control y, por supuesto, un cuadro de texto en el que el usuario puede escribir una contraseña.</span><span class="sxs-lookup"><span data-stu-id="29bb3-121">Here is a complete example, including the ASP.NET AJAX `ScriptManager` control, the `PasswordStrength` control and of course a text box where the user may enter a password.</span></span> <span data-ttu-id="29bb3-122">Por motivos de demostración, el último campo de formulario es un campo de texto normal y no un campo de contraseña para que pueda ver durante el desarrollo lo que está escribiendo.</span><span class="sxs-lookup"><span data-stu-id="29bb3-122">For the sake of demonstration, the latter form field is a regular text field and not a password field so that you can see during development what you are typing.</span></span>

[!code-aspx[Main](testing-the-strength-of-a-password-vb/samples/sample1.aspx)]

<span data-ttu-id="29bb3-123">Ejecute la página y escriba fuera: solo después de escribir letras minúsculas, letras mayúsculas, dígitos y símbolos, se considera que la contraseña es ininterrumpida.</span><span class="sxs-lookup"><span data-stu-id="29bb3-123">Run the page and type away: Only after you have entered lowercase letters, uppercase letters, digits and symbols, the password is deemed as unbreakable .</span></span>

<span data-ttu-id="29bb3-124">[![ahora la contraseña es (bastante) buena](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="29bb3-124">[![Now the password is (quite) good](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span></span>

<span data-ttu-id="29bb3-125">Ahora la contraseña es (bastante) buena ([haga clic para ver la imagen de tamaño completo](testing-the-strength-of-a-password-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="29bb3-125">Now the password is (quite) good ([Click to view full-size image](testing-the-strength-of-a-password-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="29bb3-126">Anterior</span><span class="sxs-lookup"><span data-stu-id="29bb3-126">Previous</span></span>](testing-the-strength-of-a-password-cs.md)
