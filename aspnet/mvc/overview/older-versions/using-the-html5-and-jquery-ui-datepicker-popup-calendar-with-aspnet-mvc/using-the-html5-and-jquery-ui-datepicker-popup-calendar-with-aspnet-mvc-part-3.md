---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Usar el calendario emergente del DatePicker de la interfaz de usuario de HTML5 y jQuery con ASP.NET MVC, parte 3 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, plantillas de pantalla y el calendario emergente de DatePicker de la interfaz de usuario de jQuery en un ASP.NET...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: b3249397e54e64538c4dc78e5fe8b94656e8962b
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457900"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a><span data-ttu-id="10242-103">Usar el calendario emergente del DatePicker de la interfaz de usuario de HTML5 y jQuery con ASP.NET MVC, parte 3</span><span class="sxs-lookup"><span data-stu-id="10242-103">Using the HTML5 and jQuery UI Datepicker Popup Calendar with ASP.NET MVC - Part 3</span></span>

<span data-ttu-id="10242-104">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="10242-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="10242-105">Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, plantillas de pantalla y el calendario emergente de DatePicker de la interfaz de usuario de jQuery en una aplicación web MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="10242-105">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery UI datepicker popup calendar in an ASP.NET MVC Web application.</span></span>

## <a name="working-with-complex-types"></a><span data-ttu-id="10242-106">Trabajar con tipos complejos</span><span class="sxs-lookup"><span data-stu-id="10242-106">Working with Complex Types</span></span>

<span data-ttu-id="10242-107">En esta sección, creará una clase de dirección y aprenderá a crear una plantilla para mostrarla.</span><span class="sxs-lookup"><span data-stu-id="10242-107">In this section you'll create an address class and learn how to create a template to display it.</span></span>

<span data-ttu-id="10242-108">En la carpeta *Models* , cree un nuevo archivo de clase denominado *Person.CS* donde colocará dos tipos: una clase `Person` y una clase `Address`.</span><span class="sxs-lookup"><span data-stu-id="10242-108">In the *Models* folder, create a new class file named *Person.cs* where you'll put two types: a `Person` class and an `Address` class.</span></span> <span data-ttu-id="10242-109">La clase `Person` contendrá una propiedad con el tipo `Address`.</span><span class="sxs-lookup"><span data-stu-id="10242-109">The `Person` class will contain a property that's typed as `Address`.</span></span> <span data-ttu-id="10242-110">El tipo de `Address` es un tipo complejo, lo que significa que no es uno de los tipos integrados como `int`, `string`o `double`.</span><span class="sxs-lookup"><span data-stu-id="10242-110">The `Address` type is a complex type, meaning it's not one of the built-in types like `int`, `string`, or `double`.</span></span> <span data-ttu-id="10242-111">En su lugar, tiene varias propiedades.</span><span class="sxs-lookup"><span data-stu-id="10242-111">Instead, it has several properties.</span></span> <span data-ttu-id="10242-112">El código de las nuevas clases tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="10242-112">The code for the new classes looks like this:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

<span data-ttu-id="10242-113">En el controlador de `Movie`, agregue la siguiente acción de `PersonDetail` para mostrar una instancia de Person:</span><span class="sxs-lookup"><span data-stu-id="10242-113">In the `Movie` controller, add the following `PersonDetail` action to display a person instance:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

<span data-ttu-id="10242-114">Después, agregue el código siguiente al controlador de `Movie` para rellenar el modelo de `Person` con algunos datos de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="10242-114">Then add the following code to the `Movie` controller to populate the `Person` model with some sample data:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

<span data-ttu-id="10242-115">Abra el archivo *Views\Movies\PersonDetail.cshtml* y agregue el marcado siguiente para la vista `PersonDetail`.</span><span class="sxs-lookup"><span data-stu-id="10242-115">Open the *Views\Movies\PersonDetail.cshtml* file and add the following markup for the `PersonDetail` view.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

<span data-ttu-id="10242-116">Presione Ctrl + F5 para ejecutar la aplicación y vaya a *Movies/PersonDetail*.</span><span class="sxs-lookup"><span data-stu-id="10242-116">Press Ctrl+F5 to run the application and navigate to *Movies/PersonDetail*.</span></span>

<span data-ttu-id="10242-117">La vista `PersonDetail` no contiene el `Address` tipo complejo, como puede ver en esta captura de pantalla.</span><span class="sxs-lookup"><span data-stu-id="10242-117">The `PersonDetail` view doesn't contain the `Address` complex type, as you can see in this screenshot.</span></span> <span data-ttu-id="10242-118">(No se muestra ninguna dirección).</span><span class="sxs-lookup"><span data-stu-id="10242-118">(No address is shown.)</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

<span data-ttu-id="10242-119">No se muestran los datos del modelo de `Address` porque es un tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="10242-119">The `Address` model data is not displayed because it's a complex type.</span></span> <span data-ttu-id="10242-120">Para mostrar la información de la dirección, vuelva a abrir el archivo *Views\Movies\PersonDetail.cshtml* y agregue el marcado siguiente.</span><span class="sxs-lookup"><span data-stu-id="10242-120">To display the address information, open the *Views\Movies\PersonDetail.cshtml* file again and add the following markup.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

<span data-ttu-id="10242-121">El marcado completo de la vista `PersonDetail` Now tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="10242-121">The complete markup for the `PersonDetail` now view looks like this:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

<span data-ttu-id="10242-122">Vuelva a ejecutar la aplicación y muestre la vista `PersonDetail`.</span><span class="sxs-lookup"><span data-stu-id="10242-122">Run the application again and display the `PersonDetail` view.</span></span> <span data-ttu-id="10242-123">Ahora se muestra la información de la dirección:</span><span class="sxs-lookup"><span data-stu-id="10242-123">The address information is now displayed:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a><span data-ttu-id="10242-124">Crear una plantilla para un tipo complejo</span><span class="sxs-lookup"><span data-stu-id="10242-124">Creating a Template for a Complex Type</span></span>

<span data-ttu-id="10242-125">En esta sección, creará una plantilla que se usará para representar el `Address` tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="10242-125">In this section you'll create a template that will be used to render the `Address` complex type.</span></span> <span data-ttu-id="10242-126">Al crear una plantilla para el tipo de `Address`, ASP.NET MVC puede usarla automáticamente para dar formato a un modelo de dirección en cualquier parte de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="10242-126">When you create a template for the `Address` type, ASP.NET MVC can automatically use it to format an address model anywhere in the application.</span></span> <span data-ttu-id="10242-127">Esto le ofrece una manera de controlar la representación del tipo de `Address` desde un solo lugar de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="10242-127">This gives you a way to control the rendering of the `Address` type from just one place in the application.</span></span>

<span data-ttu-id="10242-128">En la carpeta *Views\Shared\DisplayTemplates.* , cree una vista parcial fuertemente tipada denominada **Address**:</span><span class="sxs-lookup"><span data-stu-id="10242-128">In the *Views\Shared\DisplayTemplates* folder, create a strongly typed partial view named **Address**:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

<span data-ttu-id="10242-129">Haga clic en **Agregar**y, a continuación, abra el nuevo archivo *Views\Shared\DisplayTemplates\Address.cshtml* .</span><span class="sxs-lookup"><span data-stu-id="10242-129">Click **Add**, and then open the new *Views\Shared\DisplayTemplates\Address.cshtml* file.</span></span> <span data-ttu-id="10242-130">La nueva vista contiene el siguiente marcado generado:</span><span class="sxs-lookup"><span data-stu-id="10242-130">The new view contains the following generated markup:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

<span data-ttu-id="10242-131">Ejecute la aplicación y muestre la vista `PersonDetail`.</span><span class="sxs-lookup"><span data-stu-id="10242-131">Run the application and display the `PersonDetail` view.</span></span> <span data-ttu-id="10242-132">Esta vez, la plantilla de `Address` que acaba de crear se usa para mostrar el `Address` tipo complejo, por lo que la presentación tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="10242-132">This time, the `Address` template that you just created is used to display the `Address` complex type, so the display looks like the following:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a><span data-ttu-id="10242-133">Resumen: formas de especificar el formato de presentación del modelo y la plantilla</span><span class="sxs-lookup"><span data-stu-id="10242-133">Summary: Ways to Specify the Model Display Format and Template</span></span>

<span data-ttu-id="10242-134">Ha visto que puede especificar el formato o la plantilla para una propiedad del modelo mediante los siguientes enfoques:</span><span class="sxs-lookup"><span data-stu-id="10242-134">You've seen that you can specify the format or template for a model property by using the following approaches:</span></span>

- <span data-ttu-id="10242-135">Aplicar el atributo `DisplayFormat` a una propiedad del modelo.</span><span class="sxs-lookup"><span data-stu-id="10242-135">Applying the `DisplayFormat` attribute to a property in the model.</span></span> <span data-ttu-id="10242-136">Por ejemplo, el código siguiente hace que se muestre la fecha sin la hora:</span><span class="sxs-lookup"><span data-stu-id="10242-136">For example, the following code causes the date to be displayed without the time:</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- <span data-ttu-id="10242-137">Aplicar un atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) a una propiedad en el modelo y especificar el tipo de datos.</span><span class="sxs-lookup"><span data-stu-id="10242-137">Applying a [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribute to a property in the model and specifying the data type.</span></span> <span data-ttu-id="10242-138">Por ejemplo, el código siguiente hace que se muestre la fecha sin la hora.</span><span class="sxs-lookup"><span data-stu-id="10242-138">For example, the following code causes the date to be displayed without the time.</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    <span data-ttu-id="10242-139">Si la aplicación contiene una plantilla *Date. cshtml* en la carpeta *Views\Shared\DisplayTemplates.* o en la carpeta *Views\Movies\DisplayTemplates* , esa plantilla se utilizará para representar la propiedad `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="10242-139">If the application contains a *date.cshtml* template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder, that template will be used to render the `DateTime` property.</span></span> <span data-ttu-id="10242-140">De lo contrario, el sistema integrado de plantillas de ASP.NET muestra la propiedad como una fecha.</span><span class="sxs-lookup"><span data-stu-id="10242-140">Otherwise the built-in ASP.NET templating system displays the property as a date.</span></span>
- <span data-ttu-id="10242-141">Crear una plantilla de presentación en la carpeta *Views\Shared\DisplayTemplates.* o en la carpeta *Views\Movies\DisplayTemplates* cuyo nombre coincida con el tipo de datos al que desea dar formato.</span><span class="sxs-lookup"><span data-stu-id="10242-141">Creating a display template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder whose name matches the data type that you want to format.</span></span> <span data-ttu-id="10242-142">Por ejemplo, vio que el *Views\Shared\DisplayTemplates\DateTime.cshtml* se usó para representar las propiedades de `DateTime` en un modelo, sin agregar un atributo al modelo y sin agregar ningún marcado a las vistas.</span><span class="sxs-lookup"><span data-stu-id="10242-142">For example, you saw that the *Views\Shared\DisplayTemplates\DateTime.cshtml* was used to render `DateTime` properties in a model, without adding an attribute to the model and without adding any markup to views.</span></span>
- <span data-ttu-id="10242-143">Usar el atributo [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) en el modelo para especificar la plantilla para mostrar la propiedad del modelo.</span><span class="sxs-lookup"><span data-stu-id="10242-143">Using the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute on the model to specify the template to display the model property.</span></span>
- <span data-ttu-id="10242-144">Agregar explícitamente el nombre de la plantilla para mostrar a la llamada [HTML. DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) en una vista.</span><span class="sxs-lookup"><span data-stu-id="10242-144">Explicitly adding the display template name to the [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) call in a view.</span></span>

<span data-ttu-id="10242-145">El método que use dependerá de lo que necesite hacer en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="10242-145">The approach you use depends on what you need to do in your application.</span></span> <span data-ttu-id="10242-146">No es raro mezclar estos enfoques para obtener exactamente el tipo de formato que necesita.</span><span class="sxs-lookup"><span data-stu-id="10242-146">It's not uncommon to mix these approaches to get exactly the kind of formatting that you need.</span></span>

<span data-ttu-id="10242-147">En la siguiente sección, cambiará los engranajes un poco y pasará de la personalización de la forma en que se muestran los datos para personalizar cómo se escriben.</span><span class="sxs-lookup"><span data-stu-id="10242-147">In the next section, you'll switch gears a bit and move from customizing how data is displayed to customizing how it's entered.</span></span> <span data-ttu-id="10242-148">Enlazará el DatePicker de jQuery a las vistas de edición de la aplicación para proporcionar una manera sencilla de especificar fechas.</span><span class="sxs-lookup"><span data-stu-id="10242-148">You'll hook up the jQuery datepicker to the edit views in the application in order to provide a slick way to specify dates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="10242-149">[Anterior](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [Siguiente](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="10242-149">[Previous](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span></span>
