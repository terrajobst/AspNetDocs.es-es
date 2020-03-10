---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
title: Validar con la interfaz IDataErrorInfo (C#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther muestra cómo mostrar mensajes de error de validación personalizados implementando la interfaz IDataErrorInfo en una clase de modelo.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 4733b9f1-9999-48fb-8b73-6038fbcc5ecb
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 938b180da02b1963acffd021d18621d75d1d0447
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78436351"
---
# <a name="validating-with-the-idataerrorinfo-interface-c"></a><span data-ttu-id="37cb2-103">Validación de la interfaz IDataErrorInfo (C#)</span><span class="sxs-lookup"><span data-stu-id="37cb2-103">Validating with the IDataErrorInfo Interface (C#)</span></span>

<span data-ttu-id="37cb2-104">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="37cb2-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="37cb2-105">Stephen Walther muestra cómo mostrar mensajes de error de validación personalizados implementando la interfaz IDataErrorInfo en una clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="37cb2-105">Stephen Walther shows you how to display custom validation error messages by implementing the IDataErrorInfo interface in a model class.</span></span>

<span data-ttu-id="37cb2-106">El objetivo de este tutorial es explicar un enfoque para realizar la validación en una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="37cb2-106">The goal of this tutorial is to explain one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="37cb2-107">Aprenderá a impedir que alguien envíe un formulario HTML sin proporcionar valores para los campos de formulario requeridos.</span><span class="sxs-lookup"><span data-stu-id="37cb2-107">You learn how to prevent someone from submitting an HTML form without providing values for required form fields.</span></span> <span data-ttu-id="37cb2-108">En este tutorial, aprenderá a realizar la validación mediante la interfaz IErrorDataInfo.</span><span class="sxs-lookup"><span data-stu-id="37cb2-108">In this tutorial, you learn how to perform validation by using the IErrorDataInfo interface.</span></span>

## <a name="assumptions"></a><span data-ttu-id="37cb2-109">Suposiciones</span><span class="sxs-lookup"><span data-stu-id="37cb2-109">Assumptions</span></span>

<span data-ttu-id="37cb2-110">En este tutorial, usaremos la base de datos MoviesDB y la tabla de base de datos movies.</span><span class="sxs-lookup"><span data-stu-id="37cb2-110">In this tutorial, I'll use the MoviesDB database and the Movies database table.</span></span> <span data-ttu-id="37cb2-111">Esta tabla tiene las siguientes columnas:</span><span class="sxs-lookup"><span data-stu-id="37cb2-111">This table has the following columns:</span></span>

<a id="0.5_table01"></a>

| <span data-ttu-id="37cb2-112">**Nombre de la columna**</span><span class="sxs-lookup"><span data-stu-id="37cb2-112">**Column Name**</span></span> | <span data-ttu-id="37cb2-113">**Tipo de datos**</span><span class="sxs-lookup"><span data-stu-id="37cb2-113">**Data Type**</span></span> | <span data-ttu-id="37cb2-114">**Permitir valores NULL**</span><span class="sxs-lookup"><span data-stu-id="37cb2-114">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="37cb2-115">Id.</span><span class="sxs-lookup"><span data-stu-id="37cb2-115">Id</span></span> | <span data-ttu-id="37cb2-116">Valor int.</span><span class="sxs-lookup"><span data-stu-id="37cb2-116">Int</span></span> | <span data-ttu-id="37cb2-117">False</span><span class="sxs-lookup"><span data-stu-id="37cb2-117">False</span></span> |
| <span data-ttu-id="37cb2-118">Título</span><span class="sxs-lookup"><span data-stu-id="37cb2-118">Title</span></span> | <span data-ttu-id="37cb2-119">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="37cb2-119">Nvarchar(100)</span></span> | <span data-ttu-id="37cb2-120">False</span><span class="sxs-lookup"><span data-stu-id="37cb2-120">False</span></span> |
| <span data-ttu-id="37cb2-121">Director</span><span class="sxs-lookup"><span data-stu-id="37cb2-121">Director</span></span> | <span data-ttu-id="37cb2-122">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="37cb2-122">Nvarchar(100)</span></span> | <span data-ttu-id="37cb2-123">False</span><span class="sxs-lookup"><span data-stu-id="37cb2-123">False</span></span> |
| <span data-ttu-id="37cb2-124">DateReleased</span><span class="sxs-lookup"><span data-stu-id="37cb2-124">DateReleased</span></span> | <span data-ttu-id="37cb2-125">DateTime</span><span class="sxs-lookup"><span data-stu-id="37cb2-125">DateTime</span></span> | <span data-ttu-id="37cb2-126">False</span><span class="sxs-lookup"><span data-stu-id="37cb2-126">False</span></span> |

<span data-ttu-id="37cb2-127">En este tutorial, se usa Microsoft Entity Framework para generar las clases del modelo de base de datos.</span><span class="sxs-lookup"><span data-stu-id="37cb2-127">In this tutorial, I use the Microsoft Entity Framework to generate my database model classes.</span></span> <span data-ttu-id="37cb2-128">La clase de película generada por la Entity Framework se muestra en la figura 1.</span><span class="sxs-lookup"><span data-stu-id="37cb2-128">The Movie class generated by the Entity Framework is displayed in Figure 1.</span></span>

<span data-ttu-id="37cb2-129">[![la entidad Movie](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="37cb2-129">[![The Movie entity](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)</span></span>

<span data-ttu-id="37cb2-130">**Figura 01**: la entidad Movie ([haga clic para ver la imagen de tamaño completo](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="37cb2-130">**Figure 01**: The Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="37cb2-131">Para obtener más información sobre el uso de la Entity Framework para generar las clases de modelo de base de datos, vea mi tutorial titulado crear clases de modelo con el Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="37cb2-131">To learn more about using the Entity Framework to generate your database model classes, see my tutorial entitled Creating Model Classes with the Entity Framework.</span></span>

## <a name="the-controller-class"></a><span data-ttu-id="37cb2-132">Clase Controller</span><span class="sxs-lookup"><span data-stu-id="37cb2-132">The Controller Class</span></span>

<span data-ttu-id="37cb2-133">Usamos el controlador Home para mostrar películas y crear nuevas películas.</span><span class="sxs-lookup"><span data-stu-id="37cb2-133">We use the Home controller to list movies and create new movies.</span></span> <span data-ttu-id="37cb2-134">El código de esta clase se incluye en la lista 1.</span><span class="sxs-lookup"><span data-stu-id="37cb2-134">The code for this class is contained in Listing 1.</span></span>

<span data-ttu-id="37cb2-135">**Lista 1-Controllers\HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="37cb2-135">**Listing 1 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample1.cs)]

<span data-ttu-id="37cb2-136">La clase de controlador de inicio de la lista 1 contiene dos acciones Create ().</span><span class="sxs-lookup"><span data-stu-id="37cb2-136">The Home controller class in Listing 1 contains two Create() actions.</span></span> <span data-ttu-id="37cb2-137">La primera acción muestra el formulario HTML para crear una nueva película.</span><span class="sxs-lookup"><span data-stu-id="37cb2-137">The first action displays the HTML form for creating a new movie.</span></span> <span data-ttu-id="37cb2-138">La segunda acción Create () realiza la inserción real de la nueva película en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="37cb2-138">The second Create() action performs the actual insert of the new movie into the database.</span></span> <span data-ttu-id="37cb2-139">La segunda acción Create () se invoca cuando el formulario mostrado por la primera acción Create () se envía al servidor.</span><span class="sxs-lookup"><span data-stu-id="37cb2-139">The second Create() action is invoked when the form displayed by the first Create() action is submitted to the server.</span></span>

<span data-ttu-id="37cb2-140">Observe que la segunda acción Create () contiene las siguientes líneas de código:</span><span class="sxs-lookup"><span data-stu-id="37cb2-140">Notice that the second Create() action contains the following lines of code:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample2.cs)]

<span data-ttu-id="37cb2-141">La propiedad IsValid devuelve false cuando se produce un error de validación.</span><span class="sxs-lookup"><span data-stu-id="37cb2-141">The IsValid property returns false when there is a validation error.</span></span> <span data-ttu-id="37cb2-142">En ese caso, se vuelve a mostrar la vista crear que contiene el formulario HTML para crear una película.</span><span class="sxs-lookup"><span data-stu-id="37cb2-142">In that case, the Create view that contains the HTML form for creating a movie is redisplayed.</span></span>

## <a name="creating-a-partial-class"></a><span data-ttu-id="37cb2-143">Crear una clase parcial</span><span class="sxs-lookup"><span data-stu-id="37cb2-143">Creating a Partial Class</span></span>

<span data-ttu-id="37cb2-144">La clase de película se genera mediante el Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="37cb2-144">The Movie class is generated by the Entity Framework.</span></span> <span data-ttu-id="37cb2-145">Puede ver el código de la clase Movie si expande el archivo MoviesDBModel. edmx en la ventana Explorador de soluciones y abre el archivo MoviesDBModel.Designer.cs en el editor de código (vea la figura 2).</span><span class="sxs-lookup"><span data-stu-id="37cb2-145">You can see the code for the Movie class if you expand the MoviesDBModel.edmx file in the Solution Explorer window and open the MoviesDBModel.Designer.cs file in the Code Editor (see Figure 2).</span></span>

<span data-ttu-id="37cb2-146">[![el código para la entidad Movie](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="37cb2-146">[![The code for the Movie entity](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)</span></span>

<span data-ttu-id="37cb2-147">**Figura 02**: el código de la entidad Movie ([haga clic para ver la imagen de tamaño completo](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="37cb2-147">**Figure 02**: The code for the Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))</span></span>

<span data-ttu-id="37cb2-148">La clase Movie es una clase parcial.</span><span class="sxs-lookup"><span data-stu-id="37cb2-148">The Movie class is a partial class.</span></span> <span data-ttu-id="37cb2-149">Esto significa que se puede agregar otra clase parcial con el mismo nombre para extender la funcionalidad de la clase Movie.</span><span class="sxs-lookup"><span data-stu-id="37cb2-149">That means that we can add another partial class with the same name to extend the functionality of the Movie class.</span></span> <span data-ttu-id="37cb2-150">Agregaremos la lógica de validación a la nueva clase parcial.</span><span class="sxs-lookup"><span data-stu-id="37cb2-150">We'll add our validation logic to the new partial class.</span></span>

<span data-ttu-id="37cb2-151">Agregue la clase en la lista 2 a la carpeta models.</span><span class="sxs-lookup"><span data-stu-id="37cb2-151">Add the class in Listing 2 to the Models folder.</span></span>

<span data-ttu-id="37cb2-152">**Lista 2-Models\Movie.cs**</span><span class="sxs-lookup"><span data-stu-id="37cb2-152">**Listing 2 - Models\Movie.cs**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample3.cs)]

<span data-ttu-id="37cb2-153">Observe que la clase de la lista 2 incluye el modificador *parcial* .</span><span class="sxs-lookup"><span data-stu-id="37cb2-153">Notice that the class in Listing 2 includes the *partial* modifier.</span></span> <span data-ttu-id="37cb2-154">Los métodos o propiedades que se agregan a esta clase se convierten en parte de la clase de película generada por el Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="37cb2-154">Any methods or properties that you add to this class become part of the Movie class generated by the Entity Framework.</span></span>

## <a name="adding-onchanging-and-onchanged-partial-methods"></a><span data-ttu-id="37cb2-155">Agregar métodos parciales Changing y onchange</span><span class="sxs-lookup"><span data-stu-id="37cb2-155">Adding OnChanging and OnChanged Partial Methods</span></span>

<span data-ttu-id="37cb2-156">Cuando el Entity Framework genera una clase de entidad, el Entity Framework agrega automáticamente métodos parciales a la clase.</span><span class="sxs-lookup"><span data-stu-id="37cb2-156">When the Entity Framework generates an entity class, the Entity Framework adds partial methods to the class automatically.</span></span> <span data-ttu-id="37cb2-157">En el Entity Framework se generan métodos parciales Changing y onchange que corresponden a cada propiedad de la clase.</span><span class="sxs-lookup"><span data-stu-id="37cb2-157">The Entity Framework generates OnChanging and OnChanged partial methods that correspond to each property of the class.</span></span>

<span data-ttu-id="37cb2-158">En el caso de la clase Movie, el Entity Framework crea los siguientes métodos:</span><span class="sxs-lookup"><span data-stu-id="37cb2-158">In the case of the Movie class, the Entity Framework creates the following methods:</span></span>

- <span data-ttu-id="37cb2-159">OnIdChanging</span><span class="sxs-lookup"><span data-stu-id="37cb2-159">OnIdChanging</span></span>
- <span data-ttu-id="37cb2-160">OnIdChanged</span><span class="sxs-lookup"><span data-stu-id="37cb2-160">OnIdChanged</span></span>
- <span data-ttu-id="37cb2-161">OnTitleChanging</span><span class="sxs-lookup"><span data-stu-id="37cb2-161">OnTitleChanging</span></span>
- <span data-ttu-id="37cb2-162">OnTitleChanged</span><span class="sxs-lookup"><span data-stu-id="37cb2-162">OnTitleChanged</span></span>
- <span data-ttu-id="37cb2-163">OnDirectorChanging</span><span class="sxs-lookup"><span data-stu-id="37cb2-163">OnDirectorChanging</span></span>
- <span data-ttu-id="37cb2-164">OnDirectorChanged</span><span class="sxs-lookup"><span data-stu-id="37cb2-164">OnDirectorChanged</span></span>
- <span data-ttu-id="37cb2-165">OnDateReleasedChanging</span><span class="sxs-lookup"><span data-stu-id="37cb2-165">OnDateReleasedChanging</span></span>
- <span data-ttu-id="37cb2-166">OnDateReleasedChanged</span><span class="sxs-lookup"><span data-stu-id="37cb2-166">OnDateReleasedChanged</span></span>

<span data-ttu-id="37cb2-167">El método Changing se llama justo antes de que se cambie la propiedad correspondiente.</span><span class="sxs-lookup"><span data-stu-id="37cb2-167">The OnChanging method is called right before the corresponding property is changed.</span></span> <span data-ttu-id="37cb2-168">El método onchange se llama justo después de cambiar la propiedad.</span><span class="sxs-lookup"><span data-stu-id="37cb2-168">The OnChanged method is called right after the property is changed.</span></span>

<span data-ttu-id="37cb2-169">Puede aprovechar estos métodos parciales para agregar lógica de validación a la clase Movie.</span><span class="sxs-lookup"><span data-stu-id="37cb2-169">You can take advantage of these partial methods to add validation logic to the Movie class.</span></span> <span data-ttu-id="37cb2-170">La clase Update Movie de la lista 3 comprueba que las propiedades title y director tienen asignados valores no vacíos.</span><span class="sxs-lookup"><span data-stu-id="37cb2-170">The update Movie class in Listing 3 verifies that the Title and Director properties are assigned nonempty values.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="37cb2-171">Un método parcial es un método definido en una clase que no es necesario implementar.</span><span class="sxs-lookup"><span data-stu-id="37cb2-171">A partial method is a method defined in a class that you are not required to implement.</span></span> <span data-ttu-id="37cb2-172">Si no implementa un método parcial, el compilador quita la firma del método y todas las llamadas al método, por lo que no hay costos en tiempo de ejecución asociados con el método parcial.</span><span class="sxs-lookup"><span data-stu-id="37cb2-172">If you don't implement a partial method then the compiler removes the method signature and all calls to the method so there are no run-time costs associated with the partial method.</span></span> <span data-ttu-id="37cb2-173">En el editor de Visual Studio Code, puede Agregar un método parcial escribiendo la palabra clave *partial* seguida de un espacio para ver una lista de las particiones que se van a implementar.</span><span class="sxs-lookup"><span data-stu-id="37cb2-173">In the Visual Studio Code Editor, you can add a partial method by typing the keyword *partial* followed by a space to view a list of partials to implement.</span></span>

<span data-ttu-id="37cb2-174">**Lista 3: Models\Movie.cs**</span><span class="sxs-lookup"><span data-stu-id="37cb2-174">**Listing 3 - Models\Movie.cs**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample4.cs)]

<span data-ttu-id="37cb2-175">Por ejemplo, si intenta asignar una cadena vacía a la propiedad title, se asigna un mensaje de error a un diccionario denominado \_errores.</span><span class="sxs-lookup"><span data-stu-id="37cb2-175">For example, if you attempt to assign an empty string to the Title property, then an error message is assigned to a Dictionary named \_errors.</span></span>

<span data-ttu-id="37cb2-176">En este momento, no ocurre nada realmente cuando se asigna una cadena vacía a la propiedad title y se agrega un error al campo Private \_Errors.</span><span class="sxs-lookup"><span data-stu-id="37cb2-176">At this point, nothing actually happens when you assign an empty string to the Title property and an error is added to the private \_errors field.</span></span> <span data-ttu-id="37cb2-177">Necesitamos implementar la interfaz IDataErrorInfo para exponer estos errores de validación en el marco de MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="37cb2-177">We need to implement the IDataErrorInfo interface to expose these validation errors to the ASP.NET MVC framework.</span></span>

## <a name="implementing-the-idataerrorinfo-interface"></a><span data-ttu-id="37cb2-178">Implementar la interfaz IDataErrorInfo</span><span class="sxs-lookup"><span data-stu-id="37cb2-178">Implementing the IDataErrorInfo Interface</span></span>

<span data-ttu-id="37cb2-179">La interfaz IDataErrorInfo forma parte de .NET Framework desde la primera versión.</span><span class="sxs-lookup"><span data-stu-id="37cb2-179">The IDataErrorInfo interface has been part of the .NET framework since the first version.</span></span> <span data-ttu-id="37cb2-180">Esta interfaz es una interfaz muy sencilla:</span><span class="sxs-lookup"><span data-stu-id="37cb2-180">This interface is a very simple interface:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample5.cs)]

<span data-ttu-id="37cb2-181">Si una clase implementa la interfaz IDataErrorInfo, el marco de MVC de ASP.NET usará esta interfaz al crear una instancia de la clase.</span><span class="sxs-lookup"><span data-stu-id="37cb2-181">If a class implements the IDataErrorInfo interface, the ASP.NET MVC framework will use this interface when creating an instance of the class.</span></span> <span data-ttu-id="37cb2-182">Por ejemplo, la acción crear () del controlador Home acepta una instancia de la clase Movie:</span><span class="sxs-lookup"><span data-stu-id="37cb2-182">For example, the Home controller Create() action accepts an instance of the Movie class:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample6.cs)]

<span data-ttu-id="37cb2-183">El marco de MVC de ASP.NET crea la instancia de la película pasada a la acción Create () mediante un enlazador de modelos (DefaultModelBinder).</span><span class="sxs-lookup"><span data-stu-id="37cb2-183">The ASP.NET MVC framework creates the instance of the Movie passed to the Create() action by using a model binder (the DefaultModelBinder).</span></span> <span data-ttu-id="37cb2-184">El enlazador de modelos es responsable de crear una instancia del objeto Movie enlazando los campos de formulario HTML a una instancia del objeto Movie.</span><span class="sxs-lookup"><span data-stu-id="37cb2-184">The model binder is responsible for creating an instance of the Movie object by binding the HTML form fields to an instance of the Movie object.</span></span>

<span data-ttu-id="37cb2-185">DefaultModelBinder detecta si una clase implementa la interfaz IDataErrorInfo.</span><span class="sxs-lookup"><span data-stu-id="37cb2-185">The DefaultModelBinder detects whether or not a class implements the IDataErrorInfo interface.</span></span> <span data-ttu-id="37cb2-186">Si una clase implementa esta interfaz, el enlazador de modelos invoca a IDataErrorInfo. este indizador para cada propiedad de la clase.</span><span class="sxs-lookup"><span data-stu-id="37cb2-186">If a class implements this interface then the model binder invokes the IDataErrorInfo.this indexer for each property of the class.</span></span> <span data-ttu-id="37cb2-187">Si el indexador devuelve un mensaje de error, el enlazador de modelos agrega este mensaje de error al estado del modelo automáticamente.</span><span class="sxs-lookup"><span data-stu-id="37cb2-187">If the indexer returns an error message then the model binder adds this error message to model state automatically.</span></span>

<span data-ttu-id="37cb2-188">DefaultModelBinder también comprueba la propiedad IDataErrorInfo. error.</span><span class="sxs-lookup"><span data-stu-id="37cb2-188">The DefaultModelBinder also checks the IDataErrorInfo.Error property.</span></span> <span data-ttu-id="37cb2-189">Esta propiedad está pensada para representar errores de validación específicos de la propiedad que están asociados a la clase.</span><span class="sxs-lookup"><span data-stu-id="37cb2-189">This property is intended to represent non-property specific validation errors associated with the class.</span></span> <span data-ttu-id="37cb2-190">Por ejemplo, es posible que desee aplicar una regla de validación que dependa de los valores de varias propiedades de la clase Movie.</span><span class="sxs-lookup"><span data-stu-id="37cb2-190">For example, you might want to enforce a validation rule that depends on the values of multiple properties of the Movie class.</span></span> <span data-ttu-id="37cb2-191">En ese caso, se devolvería un error de validación de la propiedad error.</span><span class="sxs-lookup"><span data-stu-id="37cb2-191">In that case, you would return a validation error from the Error property.</span></span>

<span data-ttu-id="37cb2-192">La clase de película actualizada en la lista 4 implementa la interfaz IDataErrorInfo.</span><span class="sxs-lookup"><span data-stu-id="37cb2-192">The updated Movie class in Listing 4 implements the IDataErrorInfo interface.</span></span>

<span data-ttu-id="37cb2-193">**Lista 4-Models\Movie.cs (implementa IDataErrorInfo)**</span><span class="sxs-lookup"><span data-stu-id="37cb2-193">**Listing 4 - Models\Movie.cs (implements IDataErrorInfo)**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample7.cs)]

<span data-ttu-id="37cb2-194">En la lista 4, la propiedad del indexador comprueba la colección de errores \_para ver si contiene una clave que corresponde al nombre de la propiedad que se ha pasado al indizador.</span><span class="sxs-lookup"><span data-stu-id="37cb2-194">In Listing 4, the indexer property checks the \_errors collection to see if it contains a key that corresponds to the property name passed to the indexer.</span></span> <span data-ttu-id="37cb2-195">Si no hay ningún error de validación asociado a la propiedad, se devuelve una cadena vacía.</span><span class="sxs-lookup"><span data-stu-id="37cb2-195">If there is no validation error associated with the property then an empty string is returned.</span></span>

<span data-ttu-id="37cb2-196">No es necesario modificar el controlador Home de ninguna manera para usar la clase de película modificada.</span><span class="sxs-lookup"><span data-stu-id="37cb2-196">You don't need to modify the Home controller in any way to use the modified Movie class.</span></span> <span data-ttu-id="37cb2-197">En la página que se muestra en la figura 3 se muestra lo que ocurre cuando no se especifica ningún valor para los campos de formulario de título o de director.</span><span class="sxs-lookup"><span data-stu-id="37cb2-197">The page displayed in Figure 3 illustrates what happens when no value is entered for the Title or Director form fields.</span></span>

<span data-ttu-id="37cb2-198">[![crear métodos de acción automáticamente](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="37cb2-198">[![Creating action methods automatically](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)</span></span>

<span data-ttu-id="37cb2-199">**Figura 03**: formulario con valores que faltan ([haga clic para ver la imagen de tamaño completo](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="37cb2-199">**Figure 03**: A form with missing values ([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))</span></span>

<span data-ttu-id="37cb2-200">Observe que el valor DateReleased se valida automáticamente.</span><span class="sxs-lookup"><span data-stu-id="37cb2-200">Notice that the DateReleased value is validated automatically.</span></span> <span data-ttu-id="37cb2-201">Dado que la propiedad DateReleased no acepta valores NULL, DefaultModelBinder genera un error de validación para esta propiedad automáticamente cuando no tiene un valor.</span><span class="sxs-lookup"><span data-stu-id="37cb2-201">Because the DateReleased property does not accept NULL values, the DefaultModelBinder generates a validation error for this property automatically when it does not have a value.</span></span> <span data-ttu-id="37cb2-202">Si desea modificar el mensaje de error para la propiedad DateReleased, debe crear un enlazador de modelos personalizado.</span><span class="sxs-lookup"><span data-stu-id="37cb2-202">If you want to modify the error message for the DateReleased property then you need to create a custom model binder.</span></span>

## <a name="summary"></a><span data-ttu-id="37cb2-203">Resumen</span><span class="sxs-lookup"><span data-stu-id="37cb2-203">Summary</span></span>

<span data-ttu-id="37cb2-204">En este tutorial, aprendió a usar la interfaz IDataErrorInfo para generar mensajes de error de validación.</span><span class="sxs-lookup"><span data-stu-id="37cb2-204">In this tutorial, you learned how to use the IDataErrorInfo interface to generate validation error messages.</span></span> <span data-ttu-id="37cb2-205">En primer lugar, creamos una clase de película parcial que extiende la funcionalidad de la clase de película parcial generada por el Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="37cb2-205">First, we created a partial Movie class that extends the functionality of the partial Movie class generated by the Entity Framework.</span></span> <span data-ttu-id="37cb2-206">A continuación, agregamos la lógica de validación a los métodos parciales de la clase Movie OnTitleChanging () y OnDirectorChanging ().</span><span class="sxs-lookup"><span data-stu-id="37cb2-206">Next, we added validation logic to the Movie class OnTitleChanging() and OnDirectorChanging() partial methods.</span></span> <span data-ttu-id="37cb2-207">Por último, hemos implementado la interfaz IDataErrorInfo para exponer estos mensajes de validación en el marco de MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="37cb2-207">Finally, we implemented the IDataErrorInfo interface in order to expose these validation messages to the ASP.NET MVC framework.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="37cb2-208">[Anterior](performing-simple-validation-cs.md)
> [Siguiente](validating-with-a-service-layer-cs.md)</span><span class="sxs-lookup"><span data-stu-id="37cb2-208">[Previous](performing-simple-validation-cs.md)
[Next](validating-with-a-service-layer-cs.md)</span></span>
