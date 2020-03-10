---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Creación de una aplicación de MVC 3 con JavaScript discreto y discreto | Microsoft Docs
author: microsoft
description: La aplicación Web de ejemplo de lista de usuarios muestra lo sencillo que es crear aplicaciones ASP.NET MVC 3 mediante el motor de vistas de Razor. La aplicación de ejemplo s...
ms.author: riande
ms.date: 11/01/2010
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: fb63493ff22c9261fc5746a998a32f2511141f87
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78435001"
---
# <a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a><span data-ttu-id="c96cc-104">Crear una aplicación de MVC 3 con Razor y JavaScript discreto</span><span class="sxs-lookup"><span data-stu-id="c96cc-104">Creating a MVC 3 Application with Razor and Unobtrusive JavaScript</span></span>

<span data-ttu-id="c96cc-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c96cc-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="c96cc-106">La aplicación Web de ejemplo de lista de usuarios muestra lo sencillo que es crear aplicaciones ASP.NET MVC 3 mediante el motor de vistas de Razor.</span><span class="sxs-lookup"><span data-stu-id="c96cc-106">The User List sample web application demonstrates how simple it is to create ASP.NET MVC 3 applications using the Razor view engine.</span></span> <span data-ttu-id="c96cc-107">La aplicación de ejemplo muestra cómo usar el nuevo motor de vista de Razor con ASP.NET MVC versión 3 y Visual Studio 2010 para crear un sitio web de lista de usuarios ficticios que incluye funcionalidad como la creación, visualización, edición y eliminación de usuarios.</span><span class="sxs-lookup"><span data-stu-id="c96cc-107">The sample application shows how to use the new Razor view engine with ASP.NET MVC version 3 and Visual Studio 2010 to create a fictional User List website that includes functionality such as creating, displaying, editing, and deleting users.</span></span>
> 
> <span data-ttu-id="c96cc-108">En este tutorial se describen los pasos que se realizaron para compilar la aplicación de ejemplo de lista de usuarios ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="c96cc-108">This tutorial describes the steps that were taken in order to build the User List sample ASP.NET MVC 3 application.</span></span> <span data-ttu-id="c96cc-109">Hay disponible un proyecto de C# Visual Studio con y código fuente de VB para acompañar este tema: [Descargar](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span><span class="sxs-lookup"><span data-stu-id="c96cc-109">A Visual Studio project with C# and VB source code is available to accompany this topic: [Download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span></span> <span data-ttu-id="c96cc-110">Si tiene alguna pregunta sobre este tutorial, publíquelos en el foro de [MVC](https://forums.asp.net/1146.aspx).</span><span class="sxs-lookup"><span data-stu-id="c96cc-110">If you have questions about this tutorial, please post them to the [MVC forum](https://forums.asp.net/1146.aspx).</span></span>

## <a name="overview"></a><span data-ttu-id="c96cc-111">Información general</span><span class="sxs-lookup"><span data-stu-id="c96cc-111">Overview</span></span>

<span data-ttu-id="c96cc-112">La aplicación que va a compilar es un sitio web de lista de usuarios simple.</span><span class="sxs-lookup"><span data-stu-id="c96cc-112">The application you'll be building is a simple user list website.</span></span> <span data-ttu-id="c96cc-113">Los usuarios pueden escribir, ver y actualizar la información del usuario.</span><span class="sxs-lookup"><span data-stu-id="c96cc-113">Users can enter, view, and update user information.</span></span>

![Sitio de ejemplo](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

<span data-ttu-id="c96cc-115">Puede descargar el proyecto de VB C# y completado [aquí](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span><span class="sxs-lookup"><span data-stu-id="c96cc-115">You can download the VB and C# completed project [here](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span></span>

## <a name="creating-the-web-application"></a><span data-ttu-id="c96cc-116">Creación de la aplicación Web</span><span class="sxs-lookup"><span data-stu-id="c96cc-116">Creating the Web Application</span></span>

<span data-ttu-id="c96cc-117">Para iniciar el tutorial, abra Visual Studio 2010 y cree un nuevo proyecto con la plantilla *aplicación web MVC 3 ASP.net* .</span><span class="sxs-lookup"><span data-stu-id="c96cc-117">To start the tutorial, open Visual Studio 2010 and create a new project using the *ASP.NET MVC 3 Web Application* template.</span></span> <span data-ttu-id="c96cc-118">Asigne a la aplicación el nombre &quot;Mvc3Razor&quot;.</span><span class="sxs-lookup"><span data-stu-id="c96cc-118">Name the application &quot;Mvc3Razor&quot;.</span></span>

<span data-ttu-id="c96cc-119">[![nuevo proyecto de MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="c96cc-119">[![New MVC 3 project](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span></span>

<span data-ttu-id="c96cc-120">En el cuadro de diálogo **nuevo ASP.NET MVC 3 Project** , seleccione **aplicación de Internet**, seleccione el motor de vistas de Razor y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="c96cc-120">In the **New ASP.NET MVC 3 Project** dialog, select **Internet Application**, select the Razor view engine, and then click **OK**.</span></span>

![Cuadro de diálogo nuevo proyecto de ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

<span data-ttu-id="c96cc-122">En este tutorial no se usará el proveedor de pertenencia a ASP.NET, por lo que puede eliminar todos los archivos asociados con el inicio de sesión y la pertenencia.</span><span class="sxs-lookup"><span data-stu-id="c96cc-122">In this tutorial you will not be using the ASP.NET membership provider, so you can delete all the files associated with logon and membership.</span></span> <span data-ttu-id="c96cc-123">En **Explorador de soluciones**, quite los siguientes archivos y directorios:</span><span class="sxs-lookup"><span data-stu-id="c96cc-123">In **Solution Explorer**, remove the following files and directories:</span></span>

- <span data-ttu-id="c96cc-124">*Controllers\AccountController*</span><span class="sxs-lookup"><span data-stu-id="c96cc-124">*Controllers\AccountController*</span></span>
- <span data-ttu-id="c96cc-125">*Models\AccountModels*</span><span class="sxs-lookup"><span data-stu-id="c96cc-125">*Models\AccountModels*</span></span>
- <span data-ttu-id="c96cc-126">*\\Views\Shared _LogOnPartial*</span><span class="sxs-lookup"><span data-stu-id="c96cc-126">*Views\Shared\\_LogOnPartial*</span></span>
- <span data-ttu-id="c96cc-127">*Views\Account* (y todos los archivos de este directorio)</span><span class="sxs-lookup"><span data-stu-id="c96cc-127">*Views\Account* (and all the files in this directory)</span></span>

![SOLN exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

<span data-ttu-id="c96cc-129">Edite el archivo <em>\_layout. cshtml</em> y reemplace el marcado dentro del elemento `<div>` denominado `logindisplay` con el mensaje <em>&quot;</em>inicio de sesión deshabilitado&quot;.</span><span class="sxs-lookup"><span data-stu-id="c96cc-129">Edit the <em>\_Layout.cshtml</em> file and replace the markup inside the `<div>` element named `logindisplay` with the message <em>&quot;</em>Login Disabled&quot;.</span></span> <span data-ttu-id="c96cc-130">En el ejemplo siguiente se muestra el nuevo marcado:</span><span class="sxs-lookup"><span data-stu-id="c96cc-130">The following example shows the new markup:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a><span data-ttu-id="c96cc-131">Agregar el modelo</span><span class="sxs-lookup"><span data-stu-id="c96cc-131">Adding the Model</span></span>

<span data-ttu-id="c96cc-132">En **Explorador de soluciones**, haga clic con el botón secundario en la carpeta *modelos* , seleccione **Agregar**y, a continuación, haga clic en **clase**.</span><span class="sxs-lookup"><span data-stu-id="c96cc-132">In **Solution Explorer**, right-click the *Models* folder, select **Add**, and then click **Class**.</span></span>

![Nueva clase MDL de usuario](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

<span data-ttu-id="c96cc-134">Asigne a la clase el nombre `UserModel`.</span><span class="sxs-lookup"><span data-stu-id="c96cc-134">Name the class `UserModel`.</span></span> <span data-ttu-id="c96cc-135">Reemplace el contenido del archivo *UserModel* por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="c96cc-135">Replace the contents of the *UserModel* file with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

<span data-ttu-id="c96cc-136">La clase `UserModel` representa a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="c96cc-136">The `UserModel` class represents users.</span></span> <span data-ttu-id="c96cc-137">Cada miembro de la clase se anota con el atributo [required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) del espacio de nombres [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) .</span><span class="sxs-lookup"><span data-stu-id="c96cc-137">Each member of the class is annotated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute from the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace.</span></span> <span data-ttu-id="c96cc-138">Los atributos del espacio de nombres [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) proporcionan validación automática del lado cliente y del servidor para las aplicaciones Web.</span><span class="sxs-lookup"><span data-stu-id="c96cc-138">The attributes in the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace provide automatic client- and server-side validation for web applications.</span></span>

<span data-ttu-id="c96cc-139">Abra la clase `HomeController` y agregue una directiva `using` para poder tener acceso a las clases `UserModel` y `Users`:</span><span class="sxs-lookup"><span data-stu-id="c96cc-139">Open the `HomeController` class and add a `using` directive so that you can access the `UserModel` and `Users` classes:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

<span data-ttu-id="c96cc-140">Justo después de la declaración de `HomeController`, agregue el siguiente comentario y la referencia a una clase de `Users`:</span><span class="sxs-lookup"><span data-stu-id="c96cc-140">Just after the `HomeController` declaration, add the following comment and the reference to a `Users` class:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

<span data-ttu-id="c96cc-141">La clase `Users` es un almacén de datos simplificado y en memoria que usará en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="c96cc-141">The `Users` class is a simplified, in-memory data store that you'll use in this tutorial.</span></span> <span data-ttu-id="c96cc-142">En una aplicación real usaría una base de datos para almacenar información de usuario.</span><span class="sxs-lookup"><span data-stu-id="c96cc-142">In a real application you would use a database to store user information.</span></span> <span data-ttu-id="c96cc-143">Las primeras líneas del archivo `HomeController` se muestran en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c96cc-143">The first few lines of the `HomeController` file are shown in the following example:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

<span data-ttu-id="c96cc-144">Compile la aplicación para que el modelo de usuario estará disponible para el Asistente para scaffolding en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="c96cc-144">Build the application so that the user model will be available to the scaffolding wizard in the next step.</span></span>

## <a name="creating-the-default-view"></a><span data-ttu-id="c96cc-145">Crear la vista predeterminada</span><span class="sxs-lookup"><span data-stu-id="c96cc-145">Creating the Default View</span></span>

<span data-ttu-id="c96cc-146">El siguiente paso consiste en agregar un método de acción y una vista para mostrar los usuarios.</span><span class="sxs-lookup"><span data-stu-id="c96cc-146">The next step is to add an action method and view to display the users.</span></span>

<span data-ttu-id="c96cc-147">Elimine el archivo *Views\Home\Index* existente.</span><span class="sxs-lookup"><span data-stu-id="c96cc-147">Delete the existing *Views\Home\Index* file.</span></span> <span data-ttu-id="c96cc-148">Creará un nuevo archivo de *Índice* para mostrar los usuarios.</span><span class="sxs-lookup"><span data-stu-id="c96cc-148">You will create a new *Index* file to display the users.</span></span>

<span data-ttu-id="c96cc-149">En la clase `HomeController`, reemplace el contenido del método `Index` por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="c96cc-149">In the `HomeController` class, replace the contents of the `Index` method with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

<span data-ttu-id="c96cc-150">Haga clic con el botón secundario dentro del método `Index` y, a continuación, haga clic en **Agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="c96cc-150">Right-click inside the `Index` method and then click **Add View**.</span></span>

![Agregar vista](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

<span data-ttu-id="c96cc-152">Seleccione la opción **crear una vista fuertemente tipada** .</span><span class="sxs-lookup"><span data-stu-id="c96cc-152">Select the **Create a strongly-typed view** option.</span></span> <span data-ttu-id="c96cc-153">En **clase de datos de vista**, seleccione **Mvc3Razor. Models. UserModel**.</span><span class="sxs-lookup"><span data-stu-id="c96cc-153">For **View data class**, select **Mvc3Razor.Models.UserModel**.</span></span> <span data-ttu-id="c96cc-154">(Si no ve **Mvc3Razor. Models. UserModel** en el cuadro **Ver clase de datos** , debe compilar el proyecto). Asegúrese de que el motor de vista está establecido en **Razor**.</span><span class="sxs-lookup"><span data-stu-id="c96cc-154">(If you don't see **Mvc3Razor.Models.UserModel** in the **View data class** box, you need to build the project.) Make sure that the view engine is set to **Razor**.</span></span> <span data-ttu-id="c96cc-155">Establezca **ver contenido** en **lista** y, a continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="c96cc-155">Set **View content** to **List** and then click **Add**.</span></span>

![Agregar vista de índice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

<span data-ttu-id="c96cc-157">La nueva vista scaffolding automáticamente los datos de usuario que se pasan a la vista `Index`.</span><span class="sxs-lookup"><span data-stu-id="c96cc-157">The new view automatically scaffolds the user data that's passed to the `Index` view.</span></span> <span data-ttu-id="c96cc-158">Examine el archivo *Views\Home\Index* recién generado.</span><span class="sxs-lookup"><span data-stu-id="c96cc-158">Examine the newly generated *Views\Home\Index* file.</span></span> <span data-ttu-id="c96cc-159">Los vínculos **crear nuevos**, **Editar**, **detalles**y **eliminar** no funcionan, pero el resto de la página es funcional.</span><span class="sxs-lookup"><span data-stu-id="c96cc-159">The **Create New**, **Edit**, **Details**, and **Delete** links don't work, but the rest of the page is functional.</span></span> <span data-ttu-id="c96cc-160">Ejecute la página.</span><span class="sxs-lookup"><span data-stu-id="c96cc-160">Run the page.</span></span> <span data-ttu-id="c96cc-161">Verá una lista de usuarios.</span><span class="sxs-lookup"><span data-stu-id="c96cc-161">You see a list of users.</span></span>

![Página de índice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

<span data-ttu-id="c96cc-163">Abra el archivo *index. cshtml* y reemplace el marcado `ActionLink` de **Edit**, **Details**y **Delete** con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="c96cc-163">Open the *Index.cshtml* file and replace the `ActionLink` markup for **Edit**, **Details**, and **Delete** with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

<span data-ttu-id="c96cc-164">El nombre de usuario se utiliza como identificador para buscar el registro seleccionado en los vínculos de **edición**, **detalles**y **eliminación** .</span><span class="sxs-lookup"><span data-stu-id="c96cc-164">The user name is used as the ID to find the selected record in the **Edit**, **Details**, and **Delete** links.</span></span>

## <a name="creating-the-details-view"></a><span data-ttu-id="c96cc-165">Crear la vista de detalles</span><span class="sxs-lookup"><span data-stu-id="c96cc-165">Creating the Details View</span></span>

<span data-ttu-id="c96cc-166">El siguiente paso consiste en agregar un método de acción de `Details` y una vista para mostrar los detalles del usuario.</span><span class="sxs-lookup"><span data-stu-id="c96cc-166">The next step is to add a `Details` action method and view in order to display user details.</span></span>

![Detalles](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

<span data-ttu-id="c96cc-168">Agregue el siguiente método de `Details` al controlador Home:</span><span class="sxs-lookup"><span data-stu-id="c96cc-168">Add the following `Details` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

<span data-ttu-id="c96cc-169">Haga clic con el botón derecho en el método `Details` y seleccione <strong>Agregar vista</strong>.</span><span class="sxs-lookup"><span data-stu-id="c96cc-169">Right-click inside the `Details` method and then select <strong>Add View</strong>.</span></span> <span data-ttu-id="c96cc-170">Compruebe que el cuadro <strong>Ver clase de datos</strong> contiene <strong>Mvc3Razor. Models. UserModel</strong><em>.</em></span><span class="sxs-lookup"><span data-stu-id="c96cc-170">Verify that the <strong>View data class</strong> box contains <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span></span> <span data-ttu-id="c96cc-171">Establezca <strong>ver contenido</strong> en <strong>detalles</strong> y, a continuación, haga clic en <strong>Agregar</strong>.</span><span class="sxs-lookup"><span data-stu-id="c96cc-171">Set <strong>View content</strong> to <strong>Details</strong> and then click <strong>Add</strong>.</span></span>

![Agregar vista de detalles](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

<span data-ttu-id="c96cc-173">Ejecute la aplicación y seleccione un vínculo de detalles.</span><span class="sxs-lookup"><span data-stu-id="c96cc-173">Run the application and select a details link.</span></span> <span data-ttu-id="c96cc-174">El scaffolding automático muestra cada propiedad del modelo.</span><span class="sxs-lookup"><span data-stu-id="c96cc-174">The automatic scaffolding shows each property in the model.</span></span>

![Detalles](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a><span data-ttu-id="c96cc-176">Crear la vista de edición</span><span class="sxs-lookup"><span data-stu-id="c96cc-176">Creating the Edit View</span></span>

<span data-ttu-id="c96cc-177">Agregue el siguiente método de `Edit` al controlador Home.</span><span class="sxs-lookup"><span data-stu-id="c96cc-177">Add the following `Edit` method to the home controller.</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

<span data-ttu-id="c96cc-178">Agregue una vista como en los pasos anteriores, pero establezca **ver contenido** en **Editar**.</span><span class="sxs-lookup"><span data-stu-id="c96cc-178">Add a view as in the previous steps, but set **View content** to **Edit**.</span></span>

![Agregar vista de edición](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

<span data-ttu-id="c96cc-180">Ejecute la aplicación y edite el nombre y el apellido de uno de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="c96cc-180">Run the application and edit the first and last name of one of the users.</span></span> <span data-ttu-id="c96cc-181">Si infringe cualquier restricción `DataAnnotation` que se haya aplicado a la clase `UserModel`, al enviar el formulario, verá errores de validación generados por el código del servidor.</span><span class="sxs-lookup"><span data-stu-id="c96cc-181">If you violate any `DataAnnotation` constraints that have been applied to the `UserModel` class, when you submit the form, you will see validation errors that are produced by server code.</span></span> <span data-ttu-id="c96cc-182">Por ejemplo, si cambia el nombre &quot;Ana&quot; a &quot;un&quot;, al enviar el formulario, se muestra el siguiente error en el formulario:</span><span class="sxs-lookup"><span data-stu-id="c96cc-182">For example, if you change the first name &quot;Ann&quot; to &quot;A&quot;, when you submit the form, the following error is displayed on the form:</span></span>

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

<span data-ttu-id="c96cc-183">En este tutorial, va a tratar el nombre de usuario como clave principal.</span><span class="sxs-lookup"><span data-stu-id="c96cc-183">In this tutorial, you're treating the user name as the primary key.</span></span> <span data-ttu-id="c96cc-184">Por lo tanto, no se puede cambiar la propiedad del nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="c96cc-184">Therefore, the user name property cannot be changed.</span></span> <span data-ttu-id="c96cc-185">En el archivo *Edit. cshtml* , justo después de la instrucción `Html.BeginForm`, establezca el nombre de usuario en un campo oculto.</span><span class="sxs-lookup"><span data-stu-id="c96cc-185">In the *Edit.cshtml* file, just after the `Html.BeginForm` statement, set the user name to be a hidden field.</span></span> <span data-ttu-id="c96cc-186">Esto hace que la propiedad se pase en el modelo.</span><span class="sxs-lookup"><span data-stu-id="c96cc-186">This causes the property to be passed in the model.</span></span> <span data-ttu-id="c96cc-187">En el fragmento de código siguiente se muestra la ubicación de la instrucción `Hidden`:</span><span class="sxs-lookup"><span data-stu-id="c96cc-187">The following code fragment shows the placement of the `Hidden` statement:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

<span data-ttu-id="c96cc-188">Reemplace el `TextBoxFor` y `ValidationMessageFor` marcado para el nombre de usuario por una llamada a `DisplayFor`.</span><span class="sxs-lookup"><span data-stu-id="c96cc-188">Replace the `TextBoxFor` and `ValidationMessageFor` markup for the user name with a `DisplayFor` call.</span></span> <span data-ttu-id="c96cc-189">El método `DisplayFor` muestra la propiedad como un elemento de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="c96cc-189">The `DisplayFor` method displays the property as a read-only element.</span></span> <span data-ttu-id="c96cc-190">En el siguiente ejemplo se muestra el marcado completado.</span><span class="sxs-lookup"><span data-stu-id="c96cc-190">The following example shows the completed markup.</span></span> <span data-ttu-id="c96cc-191">Las llamadas `TextBoxFor` y `ValidationMessageFor` originales se comentan con los caracteres de comentario de inicio y de finalización de Razor (`@* *@`)</span><span class="sxs-lookup"><span data-stu-id="c96cc-191">The original `TextBoxFor` and `ValidationMessageFor` calls are commented out with the Razor begin-comment and end-comment characters (`@* *@`)</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a><span data-ttu-id="c96cc-192">Habilitar la validación del lado cliente</span><span class="sxs-lookup"><span data-stu-id="c96cc-192">Enabling Client-Side Validation</span></span>

<span data-ttu-id="c96cc-193">Para habilitar la validación del lado cliente en ASP.NET MVC 3, debe establecer dos marcas y debe incluir tres archivos JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c96cc-193">To enable client-side validation in ASP.NET MVC 3, you must set two flags and you must include three JavaScript files.</span></span>

<span data-ttu-id="c96cc-194">Abra el archivo *Web. config* de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c96cc-194">Open the application's *Web.config* file.</span></span> <span data-ttu-id="c96cc-195">Compruebe `that ClientValidationEnabled` y `UnobtrusiveJavaScriptEnabled` se establecen en true en la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c96cc-195">Verify `that ClientValidationEnabled` and `UnobtrusiveJavaScriptEnabled` are set to true in the application settings.</span></span> <span data-ttu-id="c96cc-196">El siguiente fragmento del archivo *Web. config* raíz muestra la configuración correcta:</span><span class="sxs-lookup"><span data-stu-id="c96cc-196">The following fragment from the root *Web.config* file shows the correct settings:</span></span>

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

<span data-ttu-id="c96cc-197">Si se establece `UnobtrusiveJavaScriptEnabled` en true, se habilita la validación de cliente discreta y discreta de Ajax.</span><span class="sxs-lookup"><span data-stu-id="c96cc-197">Setting `UnobtrusiveJavaScriptEnabled` to true enables unobtrusive Ajax and unobtrusive client validation.</span></span> <span data-ttu-id="c96cc-198">Cuando se usa la validación discreta, las reglas de validación se convierten en atributos HTML5.</span><span class="sxs-lookup"><span data-stu-id="c96cc-198">When you use unobtrusive validation, the validation rules are turned into HTML5 attributes.</span></span> <span data-ttu-id="c96cc-199">Los nombres de atributo HTML5 solo pueden contener letras minúsculas, números y guiones.</span><span class="sxs-lookup"><span data-stu-id="c96cc-199">HTML5 attribute names can consist of only lowercase letters, numbers, and dashes.</span></span>

<span data-ttu-id="c96cc-200">Si se establece `ClientValidationEnabled` en true, se habilita la validación del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="c96cc-200">Setting `ClientValidationEnabled` to true enables client-side validation.</span></span> <span data-ttu-id="c96cc-201">Al establecer estas claves en el archivo *Web. config* de la aplicación, se habilita la validación del cliente y el JavaScript discreto para toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c96cc-201">By setting these keys in the application *Web.config* file, you enable client validation and unobtrusive JavaScript for the entire application.</span></span> <span data-ttu-id="c96cc-202">También puede habilitar o deshabilitar esta configuración en vistas individuales o en métodos de controlador mediante el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="c96cc-202">You can also enable or disable these settings in individual views or in controller methods using the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

<span data-ttu-id="c96cc-203">También debe incluir varios archivos JavaScript en la vista representada.</span><span class="sxs-lookup"><span data-stu-id="c96cc-203">You also need to include several JavaScript files in the rendered view.</span></span> <span data-ttu-id="c96cc-204">Una manera fácil de incluir JavaScript en todas las vistas es agregarlos al archivo *Views\Shared\\_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="c96cc-204">An easy way to include the JavaScript in all views is to add them to the *Views\Shared\\_Layout.cshtml* file.</span></span> <span data-ttu-id="c96cc-205">Reemplace el elemento `<head>` del archivo *\_layout. cshtml* por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="c96cc-205">Replace the `<head>` element of the *\_Layout.cshtml* file with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

<span data-ttu-id="c96cc-206">Los dos primeros scripts de jQuery se hospedan en Microsoft Ajax Content Delivery Network (CDN).</span><span class="sxs-lookup"><span data-stu-id="c96cc-206">The first two jQuery scripts are hosted by the Microsoft Ajax Content Delivery Network (CDN).</span></span> <span data-ttu-id="c96cc-207">Al aprovechar la red CDN de Microsoft Ajax, puede mejorar significativamente el rendimiento del primer acierto de las aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="c96cc-207">By taking advantage of the Microsoft Ajax CDN, you can significantly improve the first-hit performance of your applications.</span></span>

<span data-ttu-id="c96cc-208">Ejecute la aplicación y haga clic en un vínculo de edición.</span><span class="sxs-lookup"><span data-stu-id="c96cc-208">Run the application and click an edit link.</span></span> <span data-ttu-id="c96cc-209">Vea el origen de la página en el explorador.</span><span class="sxs-lookup"><span data-stu-id="c96cc-209">View the page's source in the browser.</span></span> <span data-ttu-id="c96cc-210">El origen del explorador muestra muchos atributos con el formato `data-val` (para la validación de datos).</span><span class="sxs-lookup"><span data-stu-id="c96cc-210">The browser source shows many attributes of the form `data-val` (for data validation).</span></span> <span data-ttu-id="c96cc-211">Cuando está habilitada la validación de cliente y el JavaScript discreto, los campos de entrada con una regla de validación de cliente contienen el `data-val="true"` atributo para desencadenar la validación de cliente discreta.</span><span class="sxs-lookup"><span data-stu-id="c96cc-211">When client validation and unobtrusive JavaScript is enabled, input fields with a client-validation rule contain the `data-val="true"` attribute to trigger unobtrusive client validation.</span></span> <span data-ttu-id="c96cc-212">Por ejemplo, el campo `City` del modelo se redecoraba con el atributo [required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) , lo que da como resultado el código HTML que se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c96cc-212">For example, the `City` field in the model was decorated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute, which results in the HTML shown in the following example:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

<span data-ttu-id="c96cc-213">Para cada regla de validación de cliente, se agrega un atributo que tiene el formulario `data-val-rulename="message"`.</span><span class="sxs-lookup"><span data-stu-id="c96cc-213">For each client-validation rule, an attribute is added that has the form `data-val-rulename="message"`.</span></span> <span data-ttu-id="c96cc-214">Con el ejemplo de campo `City` mostrado anteriormente, la regla de validación de cliente necesaria genera el `data-val-required` atributo y el mensaje &quot;el campo City es necesario&quot;.</span><span class="sxs-lookup"><span data-stu-id="c96cc-214">Using the `City` field example shown earlier, the required client-validation rule generates the `data-val-required` attribute and the message &quot;The City field is required&quot;.</span></span> <span data-ttu-id="c96cc-215">Ejecute la aplicación, edite uno de los usuarios y desactive el campo `City`.</span><span class="sxs-lookup"><span data-stu-id="c96cc-215">Run the application, edit one of the users, and clear the `City` field.</span></span> <span data-ttu-id="c96cc-216">Al salir del campo, verá un mensaje de error de validación del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="c96cc-216">When you tab out of the field, you see a client-side validation error message.</span></span>

![Ciudad necesaria](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

<span data-ttu-id="c96cc-218">Del mismo modo, para cada parámetro de la regla de validación de cliente, se agrega un atributo que tiene el formato `data-val-rulename-paramname=paramvalue`.</span><span class="sxs-lookup"><span data-stu-id="c96cc-218">Similarly, for each parameter in the client-validation rule, an attribute is added that has the form `data-val-rulename-paramname=paramvalue`.</span></span> <span data-ttu-id="c96cc-219">Por ejemplo, la propiedad `FirstName` se anota con el atributo [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) y especifica una longitud mínima de 3 y una longitud máxima de 8.</span><span class="sxs-lookup"><span data-stu-id="c96cc-219">For example, the `FirstName` property is annotated with the [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attribute and specifies a minimum length of 3 and a maximum length of 8.</span></span> <span data-ttu-id="c96cc-220">La regla de validación de datos denominada `length` tiene el nombre de parámetro `max` y el valor de parámetro 8.</span><span class="sxs-lookup"><span data-stu-id="c96cc-220">The data validation rule named `length` has the parameter name `max` and the parameter value 8.</span></span> <span data-ttu-id="c96cc-221">A continuación se muestra el código HTML que se genera para el campo `FirstName` cuando se edita uno de los usuarios:</span><span class="sxs-lookup"><span data-stu-id="c96cc-221">The following shows the HTML that is generated for the `FirstName` field when you edit one of the users:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

<span data-ttu-id="c96cc-222">Para obtener más información sobre la validación discreta del cliente, consulte la entrada sobre la [validación de cliente discreta en ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) en el blog de Brad Wilson.</span><span class="sxs-lookup"><span data-stu-id="c96cc-222">For more information about unobtrusive client validation, see the entry [Unobtrusive Client Validation in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) in Brad Wilson's blog.</span></span>

> [!NOTE]
> <span data-ttu-id="c96cc-223">En ASP.NET MVC 3 beta, a veces es necesario enviar el formulario para iniciar la validación del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="c96cc-223">In ASP.NET MVC 3 Beta, you sometimes need to submit the form in order to start client-side validation.</span></span> <span data-ttu-id="c96cc-224">Esto puede cambiarse para la versión final.</span><span class="sxs-lookup"><span data-stu-id="c96cc-224">This might be changed for the final release.</span></span>

## <a name="creating-the-create-view"></a><span data-ttu-id="c96cc-225">Crear la vista</span><span class="sxs-lookup"><span data-stu-id="c96cc-225">Creating the Create View</span></span>

<span data-ttu-id="c96cc-226">El siguiente paso consiste en agregar un método de acción de `Create` y una vista para permitir al usuario crear un nuevo usuario.</span><span class="sxs-lookup"><span data-stu-id="c96cc-226">The next step is to add a `Create` action method and view in order to enable the user to create a new user.</span></span> <span data-ttu-id="c96cc-227">Agregue el siguiente método de `Create` al controlador Home:</span><span class="sxs-lookup"><span data-stu-id="c96cc-227">Add the following `Create` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

<span data-ttu-id="c96cc-228">Agregue una vista como en los pasos anteriores, pero establezca **ver contenido** en **crear**.</span><span class="sxs-lookup"><span data-stu-id="c96cc-228">Add a view as in the previous steps, but set **View content** to **Create**.</span></span>

![Crear vista](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

<span data-ttu-id="c96cc-230">Ejecute la aplicación, seleccione el vínculo **crear** y agregue un nuevo usuario.</span><span class="sxs-lookup"><span data-stu-id="c96cc-230">Run the application, select the **Create** link, and add a new user.</span></span> <span data-ttu-id="c96cc-231">El método `Create` aprovecha automáticamente la validación del lado cliente y del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="c96cc-231">The `Create` method automatically takes advantage of client-side and server-side validation.</span></span> <span data-ttu-id="c96cc-232">Intente escribir un nombre de usuario que contenga espacios en blanco, como &quot;Ben X&quot;.</span><span class="sxs-lookup"><span data-stu-id="c96cc-232">Try to enter a user name that contains white space, such as &quot;Ben X&quot;.</span></span> <span data-ttu-id="c96cc-233">Al salir del campo Nombre de usuario, se muestra un error de validación del lado cliente (`White space is not allowed`).</span><span class="sxs-lookup"><span data-stu-id="c96cc-233">When you tab out of the user name field, a client-side validation error (`White space is not allowed`) is displayed.</span></span>

## <a name="add-the-delete-method"></a><span data-ttu-id="c96cc-234">Agregar el método Delete</span><span class="sxs-lookup"><span data-stu-id="c96cc-234">Add the Delete method</span></span>

<span data-ttu-id="c96cc-235">Para completar el tutorial, agregue el siguiente método de `Delete` al controlador Home:</span><span class="sxs-lookup"><span data-stu-id="c96cc-235">To complete the tutorial, add the following `Delete` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

<span data-ttu-id="c96cc-236">Agregue una vista `Delete` como en los pasos anteriores, estableciendo **ver contenido** en **eliminar**.</span><span class="sxs-lookup"><span data-stu-id="c96cc-236">Add a `Delete` view as in the previous steps, setting **View content** to **Delete**.</span></span>

![Eliminar vista](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

<span data-ttu-id="c96cc-238">Ahora tiene una aplicación ASP.NET MVC 3 sencilla pero totalmente funcional con validación.</span><span class="sxs-lookup"><span data-stu-id="c96cc-238">You now have a simple but fully functional ASP.NET MVC 3 application with validation.</span></span>
