---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'Parte 7: pertenencia y autorización | Microsoft Docs'
author: jongalloway
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 7 cubre la pertenencia y la autorización.
ms.author: riande
ms.date: 10/13/2010
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: a6a1a936e0ea29ea36721ba78f35845401f74c01
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433471"
---
# <a name="part-7-membership-and-authorization"></a><span data-ttu-id="63474-104">Parte 7: pertenencia y autorización</span><span class="sxs-lookup"><span data-stu-id="63474-104">Part 7: Membership and Authorization</span></span>

<span data-ttu-id="63474-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="63474-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="63474-106">El almacén de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.</span><span class="sxs-lookup"><span data-stu-id="63474-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="63474-107">MVC Music Store es una implementación ligera de almacén de ejemplo que vende álbumes musicales en línea e implementa la funcionalidad básica de administración de sitios, Inicio de sesión de usuario y carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="63474-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="63474-108">En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="63474-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="63474-109">La parte 7 cubre la pertenencia y la autorización.</span><span class="sxs-lookup"><span data-stu-id="63474-109">Part 7 covers Membership and Authorization.</span></span>

<span data-ttu-id="63474-110">Nuestro controlador del administrador de la tienda es actualmente accesible para cualquier persona que visite nuestro sitio.</span><span class="sxs-lookup"><span data-stu-id="63474-110">Our Store Manager controller is currently accessible to anyone visiting our site.</span></span> <span data-ttu-id="63474-111">Vamos a cambiar esto para restringir el permiso a los administradores del sitio.</span><span class="sxs-lookup"><span data-stu-id="63474-111">Let's change this to restrict permission to site administrators.</span></span>

## <a name="adding-the-accountcontroller-and-views"></a><span data-ttu-id="63474-112">Agregar AccountController y vistas</span><span class="sxs-lookup"><span data-stu-id="63474-112">Adding the AccountController and Views</span></span>

<span data-ttu-id="63474-113">Una diferencia entre la plantilla de aplicación Web ASP.NET MVC 3 completa y la plantilla de aplicación Web vacía de ASP.NET MVC 3 es que la plantilla vacía no incluye un controlador de cuentas.</span><span class="sxs-lookup"><span data-stu-id="63474-113">One difference between the full ASP.NET MVC 3 Web Application template and the ASP.NET MVC 3 Empty Web Application template is that the empty template doesn't include an Account Controller.</span></span> <span data-ttu-id="63474-114">Vamos a agregar un controlador de cuenta mediante la copia de algunos archivos de una nueva aplicación de ASP.NET MVC creada a partir de la plantilla de aplicación web completa ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="63474-114">We'll add an Account Controller by copying a few files from a new ASP.NET MVC application created from the full ASP.NET MVC 3 Web Application template.</span></span>

<span data-ttu-id="63474-115">Cree una nueva aplicación de ASP.NET MVC con la plantilla de aplicación Web ASP.NET MVC 3 completa y copie los siguientes archivos en los mismos directorios de nuestro proyecto:</span><span class="sxs-lookup"><span data-stu-id="63474-115">Create a new ASP.NET MVC application using the full ASP.NET MVC 3 Web Application template and copy the following files into the same directories in our project:</span></span>

1. <span data-ttu-id="63474-116">Copiar AccountController.cs en el directorio Controllers</span><span class="sxs-lookup"><span data-stu-id="63474-116">Copy AccountController.cs in the Controllers directory</span></span>
2. <span data-ttu-id="63474-117">Copiar AccountModels en el directorio Models</span><span class="sxs-lookup"><span data-stu-id="63474-117">Copy AccountModels in the Models directory</span></span>
3. <span data-ttu-id="63474-118">Cree un directorio de cuentas dentro del directorio views y copie las cuatro vistas en</span><span class="sxs-lookup"><span data-stu-id="63474-118">Create an Account directory inside the Views directory and copy all four views in</span></span>

<span data-ttu-id="63474-119">Cambie el espacio de nombres de las clases del controlador y del modelo para que comiencen con MvcMusicStore.</span><span class="sxs-lookup"><span data-stu-id="63474-119">Change the namespace for the Controller and Model classes so they begin with MvcMusicStore.</span></span> <span data-ttu-id="63474-120">La clase AccountController debe usar el espacio de nombres MvcMusicStore. Controllers y la clase AccountModels debe usar el espacio de nombres MvcMusicStore. Models.</span><span class="sxs-lookup"><span data-stu-id="63474-120">The AccountController class should use the MvcMusicStore.Controllers namespace, and the AccountModels class should use the MvcMusicStore.Models namespace.</span></span>

<span data-ttu-id="63474-121">*Nota: estos archivos también están disponibles en la descarga de MvcMusicStore-Assets. zip desde la que se copiaron los archivos de diseño del sitio al principio del tutorial. Los archivos de pertenencia se encuentran en el directorio de código.*</span><span class="sxs-lookup"><span data-stu-id="63474-121">*Note: These files are also available in the MvcMusicStore-Assets.zip download from which we copied our site design files at the beginning of the tutorial. The Membership files are located in the Code directory.*</span></span>

<span data-ttu-id="63474-122">La solución actualizada debe ser similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="63474-122">The updated solution should look like the following:</span></span>

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a><span data-ttu-id="63474-123">Agregar un usuario administrativo con el sitio de configuración de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="63474-123">Adding an Administrative User with the ASP.NET Configuration site</span></span>

<span data-ttu-id="63474-124">Antes de que se requiera la autorización en nuestro sitio web, deberá crear un usuario con acceso.</span><span class="sxs-lookup"><span data-stu-id="63474-124">Before we require Authorization in our website, we'll need to create a user with access.</span></span> <span data-ttu-id="63474-125">La forma más fácil de crear un usuario es usar el sitio web de configuración de ASP.NET integrado.</span><span class="sxs-lookup"><span data-stu-id="63474-125">The easiest way to create a user is to use the built-in ASP.NET Configuration website.</span></span>

<span data-ttu-id="63474-126">Para iniciar el sitio web de configuración de ASP.NET, haga clic en el icono del Explorador de soluciones.</span><span class="sxs-lookup"><span data-stu-id="63474-126">Launch the ASP.NET Configuration website by clicking following the icon in the Solution Explorer.</span></span>

![](mvc-music-store-part-7/_static/image2.png)

<span data-ttu-id="63474-127">Esto inicia un sitio web de configuración.</span><span class="sxs-lookup"><span data-stu-id="63474-127">This launches a configuration website.</span></span> <span data-ttu-id="63474-128">Haga clic en la pestaña seguridad de la pantalla de inicio y, a continuación, haga clic en el vínculo "habilitar roles" en el centro de la pantalla.</span><span class="sxs-lookup"><span data-stu-id="63474-128">Click on the Security tab on the home screen, then click the "Enable roles" link in the center of the screen.</span></span>

![](mvc-music-store-part-7/_static/image3.png)

<span data-ttu-id="63474-129">Haga clic en el vínculo "crear o administrar roles".</span><span class="sxs-lookup"><span data-stu-id="63474-129">Click the "Create or Manage roles" link.</span></span>

![](mvc-music-store-part-7/_static/image4.png)

<span data-ttu-id="63474-130">Escriba "administrador" como nombre del rol y presione el botón Agregar rol.</span><span class="sxs-lookup"><span data-stu-id="63474-130">Enter "Administrator" as the role name and press the Add Role button.</span></span>

![](mvc-music-store-part-7/_static/image5.png)

<span data-ttu-id="63474-131">Haga clic en el botón atrás y, a continuación, haga clic en el vínculo crear usuario en el lado izquierdo.</span><span class="sxs-lookup"><span data-stu-id="63474-131">Click the Back button, then click on the Create user link on the left side.</span></span>

![](mvc-music-store-part-7/_static/image6.png)

<span data-ttu-id="63474-132">Rellene los campos de información de usuario de la izquierda con la siguiente información:</span><span class="sxs-lookup"><span data-stu-id="63474-132">Fill in the user information fields on the left using the following information:</span></span>

| <span data-ttu-id="63474-133">**Campo**</span><span class="sxs-lookup"><span data-stu-id="63474-133">**Field**</span></span> | <span data-ttu-id="63474-134">**Valor**</span><span class="sxs-lookup"><span data-stu-id="63474-134">**Value**</span></span> |
| --- | --- |
| <span data-ttu-id="63474-135">**Nombre de usuario**</span><span class="sxs-lookup"><span data-stu-id="63474-135">**User Name**</span></span> | <span data-ttu-id="63474-136">Administrador</span><span class="sxs-lookup"><span data-stu-id="63474-136">Administrator</span></span> |
| <span data-ttu-id="63474-137">**Contraseña**</span><span class="sxs-lookup"><span data-stu-id="63474-137">**Password**</span></span> | <span data-ttu-id="63474-138">password123!</span><span class="sxs-lookup"><span data-stu-id="63474-138">password123!</span></span> |
| <span data-ttu-id="63474-139">**Confirmar contraseña**</span><span class="sxs-lookup"><span data-stu-id="63474-139">**Confirm Password**</span></span> | <span data-ttu-id="63474-140">password123!</span><span class="sxs-lookup"><span data-stu-id="63474-140">password123!</span></span> |
| <span data-ttu-id="63474-141">**Correo electrónico**</span><span class="sxs-lookup"><span data-stu-id="63474-141">**E-mail**</span></span> | <span data-ttu-id="63474-142">(cualquier dirección de correo electrónico funcionará)</span><span class="sxs-lookup"><span data-stu-id="63474-142">(any email address will work)</span></span> |
| <span data-ttu-id="63474-143">**Pregunta de seguridad**</span><span class="sxs-lookup"><span data-stu-id="63474-143">**Security Question**</span></span> | <span data-ttu-id="63474-144">(lo que quiera)</span><span class="sxs-lookup"><span data-stu-id="63474-144">(whatever you like)</span></span> |
| <span data-ttu-id="63474-145">**Respuesta de seguridad**</span><span class="sxs-lookup"><span data-stu-id="63474-145">**Security Answer**</span></span> | <span data-ttu-id="63474-146">(lo que quiera)</span><span class="sxs-lookup"><span data-stu-id="63474-146">(whatever you like)</span></span> |

<span data-ttu-id="63474-147">*Nota: puede usar cualquier contraseña que quiera. La configuración de seguridad de contraseña predeterminada requiere una contraseña que tenga una longitud de 7 caracteres y que contenga un carácter no alfanumérico.*</span><span class="sxs-lookup"><span data-stu-id="63474-147">*Note: You can of course use any password you'd like. The default password security settings require a password that is 7 characters long and contains one non-alphanumeric character.*</span></span>

<span data-ttu-id="63474-148">Seleccione el rol de administrador para este usuario y haga clic en el botón crear usuario.</span><span class="sxs-lookup"><span data-stu-id="63474-148">Select the Administrator role for this user, and click the Create User button.</span></span>

![](mvc-music-store-part-7/_static/image7.png)

<span data-ttu-id="63474-149">En este punto, debería ver un mensaje que indica que el usuario se ha creado correctamente.</span><span class="sxs-lookup"><span data-stu-id="63474-149">At this point, you should see a message indicating that the user was created successfully.</span></span>

![](mvc-music-store-part-7/_static/image8.png)

<span data-ttu-id="63474-150">Ahora puede cerrar la ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="63474-150">You can now close the browser window.</span></span>

## <a name="role-based-authorization"></a><span data-ttu-id="63474-151">Autorización basada en roles</span><span class="sxs-lookup"><span data-stu-id="63474-151">Role-based Authorization</span></span>

<span data-ttu-id="63474-152">Ahora se puede restringir el acceso a StoreManagerController mediante el atributo [Authorize], especificando que el usuario debe tener el rol de administrador para tener acceso a cualquier acción del controlador en la clase.</span><span class="sxs-lookup"><span data-stu-id="63474-152">Now we can restrict access to the StoreManagerController using the [Authorize] attribute, specifying that the user must be in the Administrator role to access any controller action in the class.</span></span>

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

<span data-ttu-id="63474-153">*Nota: el atributo [Authorize] se puede colocar en métodos de acción específicos, así como en el nivel de clase del controlador.*</span><span class="sxs-lookup"><span data-stu-id="63474-153">*Note: The [Authorize] attribute can be placed on specific action methods as well as at the Controller class level.*</span></span>

<span data-ttu-id="63474-154">Ahora, al ir a/StoreManager, se abre un cuadro de diálogo de inicio de sesión:</span><span class="sxs-lookup"><span data-stu-id="63474-154">Now browsing to /StoreManager brings up a Log On dialog:</span></span>

![](mvc-music-store-part-7/_static/image9.png)

<span data-ttu-id="63474-155">Después de iniciar sesión con la nueva cuenta de administrador, podemos ir a la pantalla de edición del álbum como antes.</span><span class="sxs-lookup"><span data-stu-id="63474-155">After logging on with our new Administrator account, we're able to go to the Album Edit screen as before.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="63474-156">[Anterior](mvc-music-store-part-6.md)
> [Siguiente](mvc-music-store-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="63474-156">[Previous](mvc-music-store-part-6.md)
[Next](mvc-music-store-part-8.md)</span></span>
