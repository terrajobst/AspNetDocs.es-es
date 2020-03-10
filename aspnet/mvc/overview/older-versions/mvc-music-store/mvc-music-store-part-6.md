---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Parte 6: uso de anotaciones de datos para la validación del modelo | Microsoft Docs'
author: jongalloway
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 6 cubre el uso de anotaciones de datos para el modelo V...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: bc031dd5be61cc6707c522f85f6af77a420c8b31
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433537"
---
# <a name="part-6-using-data-annotations-for-model-validation"></a><span data-ttu-id="a002b-104">Parte 6: uso de anotaciones de datos para la validación del modelo</span><span class="sxs-lookup"><span data-stu-id="a002b-104">Part 6: Using Data Annotations for Model Validation</span></span>

<span data-ttu-id="a002b-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="a002b-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="a002b-106">El almacén de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.</span><span class="sxs-lookup"><span data-stu-id="a002b-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="a002b-107">MVC Music Store es una implementación ligera de almacén de ejemplo que vende álbumes musicales en línea e implementa la funcionalidad básica de administración de sitios, Inicio de sesión de usuario y carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="a002b-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="a002b-108">En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="a002b-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="a002b-109">La parte 6 cubre el uso de anotaciones de datos para la validación del modelo.</span><span class="sxs-lookup"><span data-stu-id="a002b-109">Part 6 covers Using Data Annotations for Model Validation.</span></span>

<span data-ttu-id="a002b-110">Tenemos un problema importante con nuestros formularios de creación y edición: no están realizando ninguna validación.</span><span class="sxs-lookup"><span data-stu-id="a002b-110">We have a major issue with our Create and Edit forms: they're not doing any validation.</span></span> <span data-ttu-id="a002b-111">Podemos hacer cosas como dejar en blanco los campos obligatorios o escribir letras en el campo Precio, y el primer error que veremos en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a002b-111">We can do things like leave required fields blank or type letters in the Price field, and the first error we'll see is from the database.</span></span>

<span data-ttu-id="a002b-112">Podemos agregar fácilmente la validación a nuestra aplicación agregando anotaciones de datos a nuestras clases de modelo.</span><span class="sxs-lookup"><span data-stu-id="a002b-112">We can easily add validation to our application by adding Data Annotations to our model classes.</span></span> <span data-ttu-id="a002b-113">Las anotaciones de datos nos permiten describir las reglas que queremos aplicar a nuestras propiedades del modelo, y ASP.NET MVC se encargará de aplicarlas y mostrar los mensajes adecuados a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="a002b-113">Data Annotations allow us to describe the rules we want applied to our model properties, and ASP.NET MVC will take care of enforcing them and displaying appropriate messages to our users.</span></span>

## <a name="adding-validation-to-our-album-forms"></a><span data-ttu-id="a002b-114">Agregar validación a los formularios de álbumes</span><span class="sxs-lookup"><span data-stu-id="a002b-114">Adding Validation to our Album Forms</span></span>

<span data-ttu-id="a002b-115">Usaremos los siguientes atributos de anotación de datos:</span><span class="sxs-lookup"><span data-stu-id="a002b-115">We'll use the following Data Annotation attributes:</span></span>

- <span data-ttu-id="a002b-116">**Requerido** : indica que la propiedad es un campo obligatorio.</span><span class="sxs-lookup"><span data-stu-id="a002b-116">**Required** – Indicates that the property is a required field</span></span>
- <span data-ttu-id="a002b-117">**DisplayName** : define el texto que se desea usar en los campos de formulario y en los mensajes de validación.</span><span class="sxs-lookup"><span data-stu-id="a002b-117">**DisplayName** – Defines the text we want used on form fields and validation messages</span></span>
- <span data-ttu-id="a002b-118">**StringLength** : define una longitud máxima para un campo de cadena.</span><span class="sxs-lookup"><span data-stu-id="a002b-118">**StringLength** – Defines a maximum length for a string field</span></span>
- <span data-ttu-id="a002b-119">**Intervalo** : proporciona un valor máximo y mínimo para un campo numérico.</span><span class="sxs-lookup"><span data-stu-id="a002b-119">**Range** – Gives a maximum and minimum value for a numeric field</span></span>
- <span data-ttu-id="a002b-120">**BIND** : enumera los campos que se van a excluir o incluir al enlazar valores de parámetro o formulario a las propiedades del modelo</span><span class="sxs-lookup"><span data-stu-id="a002b-120">**Bind** – Lists fields to exclude or include when binding parameter or form values to model properties</span></span>
- <span data-ttu-id="a002b-121">**ScaffoldColumn** : permite ocultar campos de los formularios del editor</span><span class="sxs-lookup"><span data-stu-id="a002b-121">**ScaffoldColumn** – Allows hiding fields from editor forms</span></span>

<span data-ttu-id="a002b-122">*Nota: para obtener más información sobre la validación de modelos mediante atributos de anotación de datos, consulte la documentación de MSDN en* [`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span><span class="sxs-lookup"><span data-stu-id="a002b-122">*Note: For more information on Model Validation using Data Annotation attributes, see the MSDN documentation at*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span></span>

<span data-ttu-id="a002b-123">Abra la clase album y agregue las siguientes instrucciones *using* a la parte superior.</span><span class="sxs-lookup"><span data-stu-id="a002b-123">Open the Album class and add the following *using* statements to the top.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

<span data-ttu-id="a002b-124">A continuación, actualice las propiedades para agregar atributos de presentación y validación como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="a002b-124">Next, update the properties to add display and validation attributes as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

<span data-ttu-id="a002b-125">Aunque estamos ahí, también hemos cambiado el género y el artista a propiedades virtuales.</span><span class="sxs-lookup"><span data-stu-id="a002b-125">While we're there, we've also changed the Genre and Artist to virtual properties.</span></span> <span data-ttu-id="a002b-126">Esto permite Entity Framework para cargarlos de forma diferida según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="a002b-126">This allows Entity Framework to lazy-load them as necessary.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

<span data-ttu-id="a002b-127">Después de agregar estos atributos a nuestro modelo de álbum, nuestra pantalla de creación y edición comienza inmediatamente a validar los campos y usando los nombres para mostrar que hemos elegido (por ejemplo, dirección URL de carátula de álbum en lugar de AlbumArtUrl).</span><span class="sxs-lookup"><span data-stu-id="a002b-127">After having added these attributes to our Album model, our Create and Edit screen immediately begin validating fields and using the Display Names we've chosen (e.g. Album Art Url instead of AlbumArtUrl).</span></span> <span data-ttu-id="a002b-128">Ejecute la aplicación y vaya a/StoreManager/Create.</span><span class="sxs-lookup"><span data-stu-id="a002b-128">Run the application and browse to /StoreManager/Create.</span></span>

![](mvc-music-store-part-6/_static/image1.png)

<span data-ttu-id="a002b-129">A continuación, se interrumpirán algunas reglas de validación.</span><span class="sxs-lookup"><span data-stu-id="a002b-129">Next, we'll break some validation rules.</span></span> <span data-ttu-id="a002b-130">Escriba un precio de 0 y deje el título en blanco.</span><span class="sxs-lookup"><span data-stu-id="a002b-130">Enter a price of 0 and leave the Title blank.</span></span> <span data-ttu-id="a002b-131">Cuando hacemos clic en el botón crear, veremos que el formulario se muestra con mensajes de error de validación que muestran qué campos no cumplían las reglas de validación que hemos definido.</span><span class="sxs-lookup"><span data-stu-id="a002b-131">When we click on the Create button, we will see the form displayed with validation error messages showing which fields did not meet the validation rules we have defined.</span></span>

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a><span data-ttu-id="a002b-132">Probar la validación del lado cliente</span><span class="sxs-lookup"><span data-stu-id="a002b-132">Testing the Client-Side Validation</span></span>

<span data-ttu-id="a002b-133">La validación del lado servidor es muy importante desde el punto de vista de la aplicación, ya que los usuarios pueden eludir la validación del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="a002b-133">Server-side validation is very important from an application perspective, because users can circumvent client-side validation.</span></span> <span data-ttu-id="a002b-134">Sin embargo, los formularios de página web que solo implementan la validación del servidor presentan tres problemas importantes.</span><span class="sxs-lookup"><span data-stu-id="a002b-134">However, webpage forms which only implement server-side validation exhibit three significant problems.</span></span>

1. <span data-ttu-id="a002b-135">El usuario tiene que esperar a que el formulario se exponga, se valide en el servidor y para que la respuesta se envíe a su explorador.</span><span class="sxs-lookup"><span data-stu-id="a002b-135">The user has to wait for the form to be posted, validated on the server, and for the response to be sent to their browser.</span></span>
2. <span data-ttu-id="a002b-136">El usuario no recibe comentarios inmediatos cuando corrige un campo para que ahora pase las reglas de validación.</span><span class="sxs-lookup"><span data-stu-id="a002b-136">The user doesn't get immediate feedback when they correct a field so that it now passes the validation rules.</span></span>
3. <span data-ttu-id="a002b-137">Estamos desperdiciando recursos del servidor para realizar la lógica de validación en lugar de aprovechar el explorador del usuario.</span><span class="sxs-lookup"><span data-stu-id="a002b-137">We are wasting server resources to perform validation logic instead of leveraging the user's browser.</span></span>

<span data-ttu-id="a002b-138">Afortunadamente, las plantillas scaffolding de ASP.NET MVC 3 tienen la validación del lado cliente integrada y no requieren ningún trabajo adicional.</span><span class="sxs-lookup"><span data-stu-id="a002b-138">Fortunately, the ASP.NET MVC 3 scaffold templates have client-side validation built in, requiring no additional work whatsoever.</span></span>

<span data-ttu-id="a002b-139">Al escribir una sola letra en el campo título, se cumplen los requisitos de validación, por lo que el mensaje de validación se quita inmediatamente.</span><span class="sxs-lookup"><span data-stu-id="a002b-139">Typing a single letter in the Title field satisfies the validation requirements, so the validation message is immediately removed.</span></span>

![](mvc-music-store-part-6/_static/image3.png)

> [!div class="step-by-step"]
> <span data-ttu-id="a002b-140">[Anterior](mvc-music-store-part-5.md)
> [Siguiente](mvc-music-store-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="a002b-140">[Previous](mvc-music-store-part-5.md)
[Next](mvc-music-store-part-7.md)</span></span>
