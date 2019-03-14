---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-vb
title: 'Iteración #2: que la aplicación parezca interesante (VB) | Microsoft Docs'
author: microsoft
description: En esta iteración, mejorar la apariencia de la aplicación mediante la modificación de la página principal de la vista de MVC de ASP.NET predeterminada y en cascada de hoja de estilos.
ms.author: riande
ms.date: 02/20/2009
ms.assetid: f65cb436-e493-46fd-9608-384b27385aa1
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-vb
msc.type: authoredcontent
ms.openlocfilehash: 7f27cbab17effc3b44649e06409893e6be09b011
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57050922"
---
<a name="iteration-2--make-the-application-look-nice-vb"></a><span data-ttu-id="bf9c8-103">Iteración #2: que la aplicación parezca interesante (VB)</span><span class="sxs-lookup"><span data-stu-id="bf9c8-103">Iteration #2 – Make the application look nice (VB)</span></span>
====================
<span data-ttu-id="bf9c8-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="bf9c8-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="bf9c8-105">Descargue el código</span><span class="sxs-lookup"><span data-stu-id="bf9c8-105">Download Code</span></span>](iteration-2-make-the-application-look-nice-vb/_static/contactmanager_2_vb1.zip)

> <span data-ttu-id="bf9c8-106">En esta iteración, mejorar la apariencia de la aplicación mediante la modificación de la página principal de la vista de MVC de ASP.NET predeterminada y en cascada de hoja de estilos.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-106">In this iteration, we improve the appearance of the application by modifying the default ASP.NET MVC view master page and cascading style sheet.</span></span>


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a><span data-ttu-id="bf9c8-107">Creación de una aplicación de ASP.NET MVC (VB) de administración de contactos</span><span class="sxs-lookup"><span data-stu-id="bf9c8-107">Building a Contact Management ASP.NET MVC Application (VB)</span></span>
  

<span data-ttu-id="bf9c8-108">En esta serie de tutoriales, creamos una aplicación de administración de contactos completa desde el principio para finalizar.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-108">In this series of tutorials, we build an entire Contact Management application from start to finish.</span></span> <span data-ttu-id="bf9c8-109">La aplicación de Contact Manager le permite almacenar información de contacto: los nombres, direcciones de correo electrónico y números de teléfono: para obtener una lista de personas.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-109">The Contact Manager application enables you to store contact information – names, phone numbers and email addresses – for a list of people.</span></span>

<span data-ttu-id="bf9c8-110">Creamos la aplicación a través de varias iteraciones.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-110">We build the application over multiple iterations.</span></span> <span data-ttu-id="bf9c8-111">Con cada iteración, mejorar gradualmente la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-111">With each iteration, we gradually improve the application.</span></span> <span data-ttu-id="bf9c8-112">Es el objetivo de este enfoque de varias iteraciones para que pueda entender el motivo de cada cambio.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-112">The goal of this multiple iteration approach is to enable you to understand the reason for each change.</span></span>

- <span data-ttu-id="bf9c8-113">Iteración #1: crear la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-113">Iteration #1 – Create the application.</span></span> <span data-ttu-id="bf9c8-114">En la primera iteración, se creará el Administrador de contactos de la manera más sencilla posible.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-114">In the first iteration, we create the Contact Manager in the simplest way possible.</span></span> <span data-ttu-id="bf9c8-115">Se agrega compatibilidad para las operaciones de base de datos básica: Crear, leer, actualizar y eliminar (CRUD).</span><span class="sxs-lookup"><span data-stu-id="bf9c8-115">We add support for basic database operations: Create, Read, Update, and Delete (CRUD).</span></span>

- <span data-ttu-id="bf9c8-116">Iteración #2: hacer que la aplicación parezca interesante.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-116">Iteration #2 – Make the application look nice.</span></span> <span data-ttu-id="bf9c8-117">En esta iteración, mejorar la apariencia de la aplicación mediante la modificación de la página principal de la vista de MVC de ASP.NET predeterminada y en cascada de hoja de estilos.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-117">In this iteration, we improve the appearance of the application by modifying the default ASP.NET MVC view master page and cascading style sheet.</span></span>

- <span data-ttu-id="bf9c8-118">Iteración #3: agregar una validación de formulario.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-118">Iteration #3 – Add form validation.</span></span> <span data-ttu-id="bf9c8-119">En la tercera iteración, se agrega una validación de formulario básico.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-119">In the third iteration, we add basic form validation.</span></span> <span data-ttu-id="bf9c8-120">Evitamos personas desde el envío de un formulario sin completar los campos obligatorios.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-120">We prevent people from submitting a form without completing required form fields.</span></span> <span data-ttu-id="bf9c8-121">También hemos validar direcciones de correo electrónico y números de teléfono.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-121">We also validate email addresses and phone numbers.</span></span>

- <span data-ttu-id="bf9c8-122">Iteración #4: hacer que la aplicación tenga un acoplamiento.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-122">Iteration #4 – Make the application loosely coupled.</span></span> <span data-ttu-id="bf9c8-123">En este cuarta iteración, aprovechamos de varios patrones de diseño de software para que sea más fácil de mantener y modificar la aplicación de Contact Manager.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-123">In this fourth iteration, we take advantage of several software design patterns to make it easier to maintain and modify the Contact Manager application.</span></span> <span data-ttu-id="bf9c8-124">Por ejemplo, se refactoriza nuestra aplicación para usar el patrón de repositorio y la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-124">For example, we refactor our application to use the Repository pattern and the Dependency Injection pattern.</span></span>

- <span data-ttu-id="bf9c8-125">Iteración #5: crear pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-125">Iteration #5 – Create unit tests.</span></span> <span data-ttu-id="bf9c8-126">En la iteración quinta, realizamos nuestra aplicación más fácil de mantener y modificar mediante la adición de las pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-126">In the fifth iteration, we make our application easier to maintain and modify by adding unit tests.</span></span> <span data-ttu-id="bf9c8-127">Hemos simular nuestras clases de modelo de datos y generar pruebas unitarias para nuestros controladores y la lógica de validación.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-127">We mock our data model classes and build unit tests for our controllers and validation logic.</span></span>

- <span data-ttu-id="bf9c8-128">Iteración #6: usar el desarrollo controlado por pruebas.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-128">Iteration #6 – Use test-driven development.</span></span> <span data-ttu-id="bf9c8-129">En esta iteración sexta, se agregan nuevas funciones a nuestra aplicación escribir pruebas unitarias en primer lugar y escribir código en las pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-129">In this sixth iteration, we add new functionality to our application by writing unit tests first and writing code against the unit tests.</span></span> <span data-ttu-id="bf9c8-130">En esta iteración, se agrega grupos de contactos.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-130">In this iteration, we add contact groups.</span></span>

- <span data-ttu-id="bf9c8-131">Iteración #7: agregar funcionalidad de Ajax.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-131">Iteration #7 – Add Ajax functionality.</span></span> <span data-ttu-id="bf9c8-132">En la iteración séptima, mejorar la capacidad de respuesta y el rendimiento de nuestra aplicación agregando compatibilidad con Ajax.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-132">In the seventh iteration, we improve the responsiveness and performance of our application by adding support for Ajax.</span></span>

## <a name="this-iteration"></a><span data-ttu-id="bf9c8-133">Esta iteración</span><span class="sxs-lookup"><span data-stu-id="bf9c8-133">This Iteration</span></span>

<span data-ttu-id="bf9c8-134">El objetivo de esta iteración es mejorar la apariencia de la aplicación de Contact Manager.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-134">The goal of this iteration is to improve the appearance of the Contact Manager application.</span></span> <span data-ttu-id="bf9c8-135">Actualmente, el Administrador de contactos usa la página principal de la vista de MVC de ASP.NET de forma predeterminada y la hoja de estilos en cascada (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="bf9c8-135">Currently, the Contact Manager uses the default ASP.NET MVC view master page and cascading style sheet (see Figure 1).</span></span> <span data-ttu-id="bf9c8-136">Estos don t aspecto negativo, pero que no necesita el Administrador de contacto para el aspecto de cada otro sitio Web de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-136">These don t look bad, but I don t want the Contact Manager to look just like every other ASP.NET MVC website.</span></span> <span data-ttu-id="bf9c8-137">Quiero reemplazar estos archivos con los archivos personalizados.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-137">I want to replace these files with custom files.</span></span>


<span data-ttu-id="bf9c8-138">[![El cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image1.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bf9c8-138">[![The New Project dialog box](iteration-2-make-the-application-look-nice-vb/_static/image1.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image1.png)</span></span>

<span data-ttu-id="bf9c8-139">**Figura 01**: La apariencia predeterminada de una aplicación ASP.NET MVC ([haga clic aquí para ver imagen en tamaño completo](iteration-2-make-the-application-look-nice-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="bf9c8-139">**Figure 01**: The default appearance of an ASP.NET MVC Application ([Click to view full-size image](iteration-2-make-the-application-look-nice-vb/_static/image2.png))</span></span>


<span data-ttu-id="bf9c8-140">En esta iteración, describen dos enfoques para mejorar el diseño visual de nuestra aplicación.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-140">In this iteration, I discuss two approaches to improving the visual design of our application.</span></span> <span data-ttu-id="bf9c8-141">En primer lugar, mostraré cómo aprovechar las ventajas de la Galería de diseño de MVC de ASP.NET para descargar una plantilla de diseño de MVC de ASP.NET gratuita.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-141">First, I show you how to take advantage of the ASP.NET MVC Design gallery to download a free ASP.NET MVC design template.</span></span> <span data-ttu-id="bf9c8-142">La Galería de diseño de MVC de ASP.NET le permite crear una aplicación web profesionales sin realizar ningún trabajo.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-142">The ASP.NET MVC Design gallery enables you to create a professional web application without doing any work.</span></span>

<span data-ttu-id="bf9c8-143">Decidido no usar una plantilla desde la Galería de diseño de MVC de ASP.NET para la aplicación de Contact Manager.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-143">I decided to not use a template from the ASP.NET MVC Design gallery for the Contact Manager application.</span></span> <span data-ttu-id="bf9c8-144">En su lugar, tuve un diseño personalizado creado por una empresa de diseño profesional.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-144">Instead, I had a custom design created by a professional design firm.</span></span> <span data-ttu-id="bf9c8-145">En la segunda parte de este tutorial, explica cómo trabajé con una empresa de diseño profesional para crear el diseño final de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-145">In the second part of this tutorial, I explain how I worked with a professional design company to create the final ASP.NET MVC design.</span></span>

## <a name="the-aspnet-mvc-design-gallery"></a><span data-ttu-id="bf9c8-146">La Galería de diseño de MVC de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="bf9c8-146">The ASP.NET MVC Design Gallery</span></span>

<span data-ttu-id="bf9c8-147">La Galería de diseño de MVC de ASP.NET es un recurso gratuito proporcionado por Microsoft.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-147">The ASP.NET MVC Design Gallery is a free resource provided by Microsoft.</span></span> <span data-ttu-id="bf9c8-148">La Galería de MVC de ASP.NET se encuentra en la siguiente dirección:</span><span class="sxs-lookup"><span data-stu-id="bf9c8-148">The ASP.NET MVC Gallery is located at the following address:</span></span>

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

<span data-ttu-id="bf9c8-149">La Galería de diseño de MVC de ASP.NET hospeda una colección de diseños de sitio Web gratuito que se crearon específicamente para usar en un proyecto de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-149">The ASP.NET MVC Design Gallery hosts a collection of free website designs that were created specifically for using in an ASP.NET MVC project.</span></span> <span data-ttu-id="bf9c8-150">Diseños se cargan los miembros de la Comunidad.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-150">Designs are uploaded by members of the community.</span></span> <span data-ttu-id="bf9c8-151">Los visitantes de la Galería pueden votar por sus diseños favoritos (consulte la figura 2).</span><span class="sxs-lookup"><span data-stu-id="bf9c8-151">Visitors to the Gallery can vote for their favorite designs (see Figure 2).</span></span>


<span data-ttu-id="bf9c8-152">[![El cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image2.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="bf9c8-152">[![The New Project dialog box](iteration-2-make-the-application-look-nice-vb/_static/image2.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image3.png)</span></span>

<span data-ttu-id="bf9c8-153">**Figura 02**: La Galería de diseño de MVC de ASP.NET ([haga clic aquí para ver imagen en tamaño completo](iteration-2-make-the-application-look-nice-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="bf9c8-153">**Figure 02**: The ASP.NET MVC Design Gallery ([Click to view full-size image](iteration-2-make-the-application-look-nice-vb/_static/image4.png))</span></span>


<span data-ttu-id="bf9c8-154">Mientras escribo este tutorial, el diseño más popular en la galería es un diseño denominado octubre por David Hauser.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-154">As I write this tutorial, the most popular design in the gallery is a design named October by David Hauser.</span></span> <span data-ttu-id="bf9c8-155">Puede usar este diseño para un proyecto ASP.NET MVC mediante los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="bf9c8-155">You can use this design for an ASP.NET MVC project by completing the following steps:</span></span>

1. <span data-ttu-id="bf9c8-156">Haga clic en el **descargar** botón para descargar el archivo October.zip en el equipo.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-156">Click the **Download** button to download the October.zip file to your computer.</span></span>
2. <span data-ttu-id="bf9c8-157">Haga clic en el archivo October.zip descargado y haga clic en el **desbloquear** (consulte la figura 3).</span><span class="sxs-lookup"><span data-stu-id="bf9c8-157">Right-click the downloaded October.zip file and click the **Unblock** button (see Figure 3).</span></span>
3. <span data-ttu-id="bf9c8-158">Descomprima el archivo en una carpeta denominada octubre.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-158">Unzip the file to a folder named October.</span></span>
4. <span data-ttu-id="bf9c8-159">Seleccione todos los archivos de la carpeta DesignTemplate contenida en la carpeta de octubre, haga clic en los archivos y seleccione la opción de menú **copia**.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-159">Select all of the files from the DesignTemplate folder contained in the October folder, right-click the files, and select the menu option **Copy**.</span></span>
5. <span data-ttu-id="bf9c8-160">Haga clic en el nodo del proyecto ContactManager en la ventana Explorador de soluciones de Visual Studio y seleccione la opción de menú **pegar** (consulte la figura 4).</span><span class="sxs-lookup"><span data-stu-id="bf9c8-160">Right-click the ContactManager project node in the Visual Studio Solution Explorer window and select the menu option **Paste** (see Figure 4).</span></span>
6. <span data-ttu-id="bf9c8-161">Seleccione la opción de menú de Visual Studio **editar, buscar y reemplazar, reemplazo rápido** y reemplace *[MyProjectName]* con *ContactManager* (consulte la figura 5).</span><span class="sxs-lookup"><span data-stu-id="bf9c8-161">Select the Visual Studio menu option **Edit, Find and Replace, Quick Replace** and replace *[MyProjectName]* with *ContactManager* (see Figure 5).</span></span>


<span data-ttu-id="bf9c8-162">[![El cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image3.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="bf9c8-162">[![The New Project dialog box](iteration-2-make-the-application-look-nice-vb/_static/image3.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image5.png)</span></span>

<span data-ttu-id="bf9c8-163">**Figura 03**: Desbloquear un archivo descargado de Internet ([haga clic aquí para ver imagen en tamaño completo](iteration-2-make-the-application-look-nice-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="bf9c8-163">**Figure 03**: Unblocking a file downloaded from the web ([Click to view full-size image](iteration-2-make-the-application-look-nice-vb/_static/image6.png))</span></span>


<span data-ttu-id="bf9c8-164">[![El cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image4.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="bf9c8-164">[![The New Project dialog box](iteration-2-make-the-application-look-nice-vb/_static/image4.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image7.png)</span></span>

<span data-ttu-id="bf9c8-165">**Figura 04**: Sobrescribiendo los archivos en el Explorador de soluciones ([haga clic aquí para ver imagen en tamaño completo](iteration-2-make-the-application-look-nice-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="bf9c8-165">**Figure 04**: Overwriting files in the Solution Explorer ([Click to view full-size image](iteration-2-make-the-application-look-nice-vb/_static/image8.png))</span></span>


<span data-ttu-id="bf9c8-166">[![El cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image5.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="bf9c8-166">[![The New Project dialog box](iteration-2-make-the-application-look-nice-vb/_static/image5.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image9.png)</span></span>

<span data-ttu-id="bf9c8-167">**Figura 05**: Sustituyendo [ProjectName] ContactManager ([haga clic aquí para ver imagen en tamaño completo](iteration-2-make-the-application-look-nice-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="bf9c8-167">**Figure 05**: Replacing [ProjectName] with ContactManager ([Click to view full-size image](iteration-2-make-the-application-look-nice-vb/_static/image10.png))</span></span>


<span data-ttu-id="bf9c8-168">Después de completar estos pasos, la aplicación web usará el nuevo diseño.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-168">After you complete these steps, your web application will use the new design.</span></span> <span data-ttu-id="bf9c8-169">La página en la figura 6 muestra la apariencia de la aplicación de Contact Manager con el diseño de octubre.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-169">The page in Figure 6 illustrates the appearance of the Contact Manager application with the October design.</span></span>


<span data-ttu-id="bf9c8-170">[![El cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image6.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="bf9c8-170">[![The New Project dialog box](iteration-2-make-the-application-look-nice-vb/_static/image6.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image11.png)</span></span>

<span data-ttu-id="bf9c8-171">**Figura 06**: ContactManager con la plantilla de octubre ([haga clic aquí para ver imagen en tamaño completo](iteration-2-make-the-application-look-nice-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="bf9c8-171">**Figure 06**: ContactManager with the October template ([Click to view full-size image](iteration-2-make-the-application-look-nice-vb/_static/image12.png))</span></span>


## <a name="creating-a-custom-aspnet-mvc-design"></a><span data-ttu-id="bf9c8-172">Crear un diseño MVC de ASP.NET personalizados</span><span class="sxs-lookup"><span data-stu-id="bf9c8-172">Creating a Custom ASP.NET MVC Design</span></span>

<span data-ttu-id="bf9c8-173">La Galería de diseño de MVC de ASP.NET tiene una buena selección de los estilos de diseño diferentes.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-173">The ASP.NET MVC Design Gallery has a good selection of different design styles.</span></span> <span data-ttu-id="bf9c8-174">La Galería proporciona una manera sencilla para personalizar la apariencia de las aplicaciones de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-174">The Gallery provides you with a painless way to customize the appearance of your ASP.NET MVC applications.</span></span> <span data-ttu-id="bf9c8-175">Y, por supuesto, la Galería tiene la gran ventaja de estar totalmente gratuita.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-175">And, of course, the Gallery has the big advantage of being completely free.</span></span>

<span data-ttu-id="bf9c8-176">Sin embargo, es posible que deba crear un diseño completamente único para el sitio Web.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-176">However, you might need to create a completely unique design for your website.</span></span> <span data-ttu-id="bf9c8-177">En ese caso, tiene sentido para trabajar con una empresa de diseño del sitio Web.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-177">In that case, it makes sense to work with a website design company.</span></span> <span data-ttu-id="bf9c8-178">Decidido adoptar este enfoque para el diseño de la aplicación de Contact Manager.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-178">I decided to take this approach for the design for the Contact Manager application.</span></span>

<span data-ttu-id="bf9c8-179">De Contact Manager de iteración 1 y lo envíe el proyecto a la empresa de diseño.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-179">I zipped up the Contact Manager from Iteration #1 and sent the project to the design company.</span></span> <span data-ttu-id="bf9c8-180">No poseen Visual Studio (es lamentable en ellas!), pero que no ha t presentan un problema.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-180">They did not own Visual Studio (shame on them!), but that didn t present a problem.</span></span> <span data-ttu-id="bf9c8-181">Podían descargar Microsoft Visual Web Developer de forma gratuita desde el [ https://www.asp.net ](https://www.asp.net) sitio Web y abra la aplicación de Contact Manager en Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-181">They were able to download Microsoft Visual Web Developer for free from the [https://www.asp.net](https://www.asp.net) website and open the Contact Manager application in Visual Web Developer.</span></span> <span data-ttu-id="bf9c8-182">En un par de días, se hubiera producido el diseño en la figura 7.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-182">In a couple of days, they had produced the design in Figure 7.</span></span>


<span data-ttu-id="bf9c8-183">[![El cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image7.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="bf9c8-183">[![The New Project dialog box](iteration-2-make-the-application-look-nice-vb/_static/image7.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image13.png)</span></span>

<span data-ttu-id="bf9c8-184">**Figura 07**: El diseño de ASP.NET MVC Contact Manager ([haga clic aquí para ver imagen en tamaño completo](iteration-2-make-the-application-look-nice-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="bf9c8-184">**Figure 07**: The ASP.NET MVC Contact Manager Design ([Click to view full-size image](iteration-2-make-the-application-look-nice-vb/_static/image14.png))</span></span>


<span data-ttu-id="bf9c8-185">El nuevo diseño del consistió en dos archivos principales: un nuevo archivo de hoja de estilos en cascada y un nuevo archivo de página de vista maestra.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-185">The new design consisted of two main files: a new cascading style sheet file and a new view master page file.</span></span> <span data-ttu-id="bf9c8-186">Una página maestra de la vista contiene el diseño y el contenido compartido de las vistas en una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-186">A view master page contains the layout and shared content for views in an ASP.NET MVC application.</span></span> <span data-ttu-id="bf9c8-187">Por ejemplo, la página principal de la vista incluye el encabezado, las pestañas de navegación y pie de página que aparecen en la figura 7.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-187">For example, the view master page includes the header, navigation tabs, and footer that appear in Figure 7.</span></span> <span data-ttu-id="bf9c8-188">Sobrescribió el Site.Master Vista página principal existente en la carpeta Views\Shared con el nuevo archivo Site.Master de la compañía de diseño</span><span class="sxs-lookup"><span data-stu-id="bf9c8-188">I overwrote the existing Site.Master view master page in the Views\Shared folder with the new Site.Master file from the design company,</span></span>

<span data-ttu-id="bf9c8-189">La compañía de diseño también se crea una nueva hoja de estilos en cascada y un conjunto de imágenes.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-189">The design company also created a new cascading style sheet and set of images.</span></span> <span data-ttu-id="bf9c8-190">Puedo colocar estos nuevos archivos en la carpeta de contenido y sobrescribió el archivo Site.css existente.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-190">I placed these new files in the Content folder and overwrote the existing Site.css file.</span></span> <span data-ttu-id="bf9c8-191">Debe colocar todo el contenido estático en la carpeta de contenido.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-191">You should place all static content in the Content folder.</span></span>

<span data-ttu-id="bf9c8-192">Observe que el nuevo diseño de Contact Manager incluye imágenes para modificar y eliminar contactos.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-192">Notice that the new design for the Contact Manager includes images for editing and deleting contacts.</span></span> <span data-ttu-id="bf9c8-193">Una imagen de edición y eliminación aparecen al lado de cada contacto de la tabla HTML de contactos.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-193">An Edit and Delete image appear next to each contact in the HTML table of contacts.</span></span>

<span data-ttu-id="bf9c8-194">Originalmente, estos vínculos se representaran con el código HTML. Aplicación auxiliar de ActionLink() similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="bf9c8-194">Originally, these links that were rendered with the HTML.ActionLink() helper like this:</span></span>

[!code-aspx[Main](iteration-2-make-the-application-look-nice-vb/samples/sample1.aspx)]

<span data-ttu-id="bf9c8-195">El método Html.ActionLink() no admite imágenes (el método HTML codifica el texto del vínculo por motivos de seguridad).</span><span class="sxs-lookup"><span data-stu-id="bf9c8-195">The Html.ActionLink() method does not support images (the method HTML encodes the link text for security reasons).</span></span> <span data-ttu-id="bf9c8-196">Por lo tanto, reemplazar las llamadas a Html.ActionLink() con llamadas a Url.Action() similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="bf9c8-196">Therefore, I replaced the calls to Html.ActionLink() with calls to Url.Action() like this:</span></span>

[!code-aspx[Main](iteration-2-make-the-application-look-nice-vb/samples/sample2.aspx)]

<span data-ttu-id="bf9c8-197">El método de Html.ActionLink() representa un hipervínculo HTML completo.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-197">The Html.ActionLink() method renders an entire HTML hyperlink.</span></span> <span data-ttu-id="bf9c8-198">El método Url.Action(), por otro lado, representa la dirección URL sin el &lt;un&gt; etiqueta.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-198">The Url.Action() method, on the other hand, renders just the URL without the &lt;a&gt; tag.</span></span>

<span data-ttu-id="bf9c8-199">Además, tenga en cuenta que el nuevo diseño incluye pestañas seleccionadas o sin seleccionadas.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-199">Notice, furthermore, that the new design includes both selected and unselected tabs.</span></span> <span data-ttu-id="bf9c8-200">Por ejemplo, en la figura 8, el **crear nuevo contacto** pestaña está seleccionada y el **Mis contactos** ficha no está seleccionada.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-200">For example, in Figure 8, the **Create New Contact** tab is selected and the **My Contacts** tab is not selected.</span></span>


<span data-ttu-id="bf9c8-201">[![El cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image8.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="bf9c8-201">[![The New Project dialog box](iteration-2-make-the-application-look-nice-vb/_static/image8.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image15.png)</span></span>

<span data-ttu-id="bf9c8-202">**Figura 08**: Activada y desactivada pestañas ([haga clic aquí para ver imagen en tamaño completo](iteration-2-make-the-application-look-nice-vb/_static/image16.png))</span><span class="sxs-lookup"><span data-stu-id="bf9c8-202">**Figure 08**: Selected and unselected tabs([Click to view full-size image](iteration-2-make-the-application-look-nice-vb/_static/image16.png))</span></span>


<span data-ttu-id="bf9c8-203">Para poder representar pestañas seleccionadas o sin seleccionadas, he creado una aplicación auxiliar HTML personalizada denominada el MenuItemHelper.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-203">To support rendering both selected and unselected tabs, I created a custom HTML helper named the MenuItemHelper.</span></span> <span data-ttu-id="bf9c8-204">Este método auxiliar representa cualquiera un &lt;li&gt; etiqueta o una &lt;clase li = "selected"&gt; etiqueta en función de si el controlador actual y la acción se corresponde con el nombre de acción y controlador pasado a la aplicación auxiliar.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-204">This helper method renders either a &lt;li&gt; tag or a &lt;li class="selected"&gt; tag depending on whether the current controller and action corresponds to the controller and action name passed to the helper.</span></span> <span data-ttu-id="bf9c8-205">El código para el MenuItemHelper está contenido en el listado 1.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-205">The code for the MenuItemHelper is contained in Listing 1.</span></span>

<span data-ttu-id="bf9c8-206">**Listing 1 – Helpers\MenuItemHelper.vb**</span><span class="sxs-lookup"><span data-stu-id="bf9c8-206">**Listing 1 – Helpers\MenuItemHelper.vb**</span></span>

[!code-vb[Main](iteration-2-make-the-application-look-nice-vb/samples/sample3.vb)]

<span data-ttu-id="bf9c8-207">El MenuItemHelper usa la clase TagBuilder internamente para crear el &lt;li&gt; etiqueta HTML.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-207">The MenuItemHelper uses the TagBuilder class internally to build the &lt;li&gt; HTML tag.</span></span> <span data-ttu-id="bf9c8-208">La clase TagBuilder es una clase de utilidad muy útil que puede usar siempre que tenga que crear una nueva etiqueta HTML.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-208">The TagBuilder class is a very useful utility class that you can use whenever you need to build up a new HTML tag.</span></span> <span data-ttu-id="bf9c8-209">Incluye métodos para agregar atributos, agregar clases CSS, generación de identificadores y modificación de la etiqueta a la s HTML interno.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-209">It includes methods for adding attributes, adding CSS classes, generating Ids, and modifying the tag s inner HTML.</span></span>

## <a name="summary"></a><span data-ttu-id="bf9c8-210">Resumen</span><span class="sxs-lookup"><span data-stu-id="bf9c8-210">Summary</span></span>

<span data-ttu-id="bf9c8-211">En esta iteración, se ha mejorado el diseño visual de nuestra aplicación de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-211">In this iteration, we improved the visual design of our ASP.NET MVC application.</span></span> <span data-ttu-id="bf9c8-212">En primer lugar, se introdujeron en la Galería de diseño de MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-212">First, you were introduced to the ASP.NET MVC Design Gallery.</span></span> <span data-ttu-id="bf9c8-213">Ha aprendido cómo descargar las plantillas de diseño gratuita desde la Galería de diseño de MVC de ASP.NET que puede usar en sus aplicaciones de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-213">You learned how to download free design templates from the ASP.NET MVC Design Gallery that you can use in your ASP.NET MVC applications.</span></span>

<span data-ttu-id="bf9c8-214">A continuación, explicamos cómo puede crear un diseño personalizado modificando el archivo de hoja de estilos en cascada de forma predeterminada y el archivo de página de vista maestra.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-214">Next, we discussed how you can create a custom design by modifying the default cascading style sheet file and master view page file.</span></span> <span data-ttu-id="bf9c8-215">Para admitir el nuevo diseño, se tenía que realizar algunos cambios menores a nuestra aplicación de Contact Manager.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-215">In order to support the new design, we had to make some minor changes to our Contact Manager application.</span></span> <span data-ttu-id="bf9c8-216">Por ejemplo, hemos agregado una nueva aplicación auxiliar HTML denominado el MenuItemHelper que muestra pestañas seleccionadas o sin seleccionadas.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-216">For example, we added a new HTML helper named the MenuItemHelper that displays selected and unselected tabs.</span></span>

<span data-ttu-id="bf9c8-217">En la siguiente iteración, abordamos al asunto muy importante de validación.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-217">In the next iteration, we tackle the very important subject of validation.</span></span> <span data-ttu-id="bf9c8-218">Agregamos el código de validación a nuestra aplicación para que un usuario no puede crear un nuevo contacto sin proporcionar valores necesarios, como una persona s primero y el apellido.</span><span class="sxs-lookup"><span data-stu-id="bf9c8-218">We add validation code to our application so that a user cannot create a new contact without supplying required values such as a person s first and last name.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bf9c8-219">[Anterior](iteration-1-create-the-application-vb.md)
> [Siguiente](iteration-3-add-form-validation-vb.md)</span><span class="sxs-lookup"><span data-stu-id="bf9c8-219">[Previous](iteration-1-create-the-application-vb.md)
[Next](iteration-3-add-form-validation-vb.md)</span></span>