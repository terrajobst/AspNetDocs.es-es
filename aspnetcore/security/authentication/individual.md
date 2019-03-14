---
title: Artículos basados en los proyectos de ASP.NET Core creados con cuentas de usuario individuales
author: rick-anderson
description: Descubra artículos basados en los proyectos de ASP.NET Core creados con cuentas de usuario individuales.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: c73365eafaf2c38ef02c3c83ccf5ced4264f7dc0
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57033842"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="04c53-103">Artículos basados en los proyectos de ASP.NET Core creados con cuentas de usuario individuales</span><span class="sxs-lookup"><span data-stu-id="04c53-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="04c53-104">ASP.NET Core Identity se incluye en las plantillas de proyecto en Visual Studio con la opción "Cuentas de usuario individuales".</span><span class="sxs-lookup"><span data-stu-id="04c53-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="04c53-105">Las plantillas de autenticación están disponibles en la CLI de .NET Core con `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="04c53-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new razor -au Individual
```

::: moniker-end

<span data-ttu-id="04c53-106">Consulte [este problema de GitHub](https://github.com/aspnet/AspNetCore/issues/5833) para la autenticación de API web.</span><span class="sxs-lookup"><span data-stu-id="04c53-106">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5833) for web API authentication.</span></span>

<a name="no"></a>
## <a name="no-authentication"></a><span data-ttu-id="04c53-107">Sin autenticación</span><span class="sxs-lookup"><span data-stu-id="04c53-107">No Authentication</span></span>

<span data-ttu-id="04c53-108">La autenticación se especifica en la CLI de .NET Core con el `-au` opción.</span><span class="sxs-lookup"><span data-stu-id="04c53-108">Authentication is specified in the .NET Core CLI with the `-au` option.</span></span> <span data-ttu-id="04c53-109">En Visual Studio, el **Cambiar autenticación** cuadro de diálogo está disponible para nuevas aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="04c53-109">In Visual Studio, the **Change Authentication** dialog is available for new web applications.</span></span> <span data-ttu-id="04c53-110">El valor predeterminado para nuevas aplicaciones web en Visual Studio es **sin autenticación**.</span><span class="sxs-lookup"><span data-stu-id="04c53-110">The default for new web apps in Visual Studio is **No Authentication**.</span></span>

<span data-ttu-id="04c53-111">Proyectos creados con ninguna autenticación:</span><span class="sxs-lookup"><span data-stu-id="04c53-111">Projects created with no authentication:</span></span>

* <span data-ttu-id="04c53-112">No contienen páginas web y la interfaz de usuario para iniciar sesión y cierre la sesión.</span><span class="sxs-lookup"><span data-stu-id="04c53-112">Don't contain web pages and UI to sign in and sign out.</span></span>
* <span data-ttu-id="04c53-113">No contienen código de autenticación.</span><span class="sxs-lookup"><span data-stu-id="04c53-113">Don't contain authentication code.</span></span>

<a name="win"></a>
## <a name="windows-authentication"></a><span data-ttu-id="04c53-114">Autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="04c53-114">Windows Authentication</span></span>

<span data-ttu-id="04c53-115">Se especifica la autenticación de Windows para las nuevas aplicaciones web en la CLI de .NET Core con el `-au Windows` opción.</span><span class="sxs-lookup"><span data-stu-id="04c53-115">Windows Authentication is specified for new web apps in the .NET Core CLI with the `-au Windows` option.</span></span> <span data-ttu-id="04c53-116">En Visual Studio, el **Cambiar autenticación** cuadro de diálogo proporciona el **Windows autenticación** opciones.</span><span class="sxs-lookup"><span data-stu-id="04c53-116">In Visual Studio, the **Change Authentication** dialog provides the **Windows Authentication** options.</span></span>

<span data-ttu-id="04c53-117">Si selecciona la autenticación de Windows, la aplicación está configurada para usar el [módulo de autenticación de Windows IIS](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="04c53-117">If Windows Authentication is selected, the app is configured to use the [Windows Authentication IIS module](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="04c53-118">Autenticación de Windows está diseñada para sitios web de Intranet.</span><span class="sxs-lookup"><span data-stu-id="04c53-118">Windows Authentication is intended for Intranet web sites.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="04c53-119">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="04c53-119">Additional resources</span></span>

<span data-ttu-id="04c53-120">Los artículos siguientes muestran cómo usar el código generado en las plantillas de ASP.NET Core que usan cuentas de usuario individuales:</span><span class="sxs-lookup"><span data-stu-id="04c53-120">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="04c53-121">Autenticación en dos fases con SMS</span><span class="sxs-lookup"><span data-stu-id="04c53-121">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="04c53-122">Confirmación de las cuentas y recuperación de contraseñas en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="04c53-122">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="04c53-123">Crear una aplicación ASP.NET Core con datos de usuario protegidos por autorización</span><span class="sxs-lookup"><span data-stu-id="04c53-123">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
