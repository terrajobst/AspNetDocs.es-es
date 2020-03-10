---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Detección de la clase de inicio OWIN | Microsoft Docs
author: Praburaj
description: En este tutorial se muestra cómo configurar la clase de inicio OWIN cargada. Para obtener más información sobre OWIN, consulte información general de Project Katana. Este tutorial fue...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: e1670c32049ec33dd4d1941a091a429d3929c452
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78500185"
---
# <a name="owin-startup-class-detection"></a><span data-ttu-id="925f3-105">Detección de la clase de inicio OWIN</span><span class="sxs-lookup"><span data-stu-id="925f3-105">OWIN Startup Class Detection</span></span>

> <span data-ttu-id="925f3-106">En este tutorial se muestra cómo configurar la clase de inicio OWIN cargada.</span><span class="sxs-lookup"><span data-stu-id="925f3-106">This tutorial shows how to configure which OWIN startup class is loaded.</span></span> <span data-ttu-id="925f3-107">Para obtener más información sobre OWIN, consulte [información general de Project Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="925f3-107">For more information on OWIN, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="925f3-108">Este tutorial fue escrito por Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan y Howard Dierking ( [@howard\_Dierking](https://twitter.com/howard_dierking) ).</span><span class="sxs-lookup"><span data-stu-id="925f3-108">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan, and Howard Dierking ( [@howard\_dierking](https://twitter.com/howard_dierking) ).</span></span>
>
> ## <a name="prerequisites"></a><span data-ttu-id="925f3-109">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="925f3-109">Prerequisites</span></span>
>
> [<span data-ttu-id="925f3-110">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="925f3-110">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)

## <a name="owin-startup-class-detection"></a><span data-ttu-id="925f3-111">Detección de la clase de inicio OWIN</span><span class="sxs-lookup"><span data-stu-id="925f3-111">OWIN Startup Class Detection</span></span>

 <span data-ttu-id="925f3-112">Cada aplicación OWIN tiene una clase de inicio donde se especifican los componentes de la canalización de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="925f3-112">Every OWIN Application has a startup class where you specify components for the application pipeline.</span></span> <span data-ttu-id="925f3-113">Hay diferentes formas de conectar la clase startup con el tiempo de ejecución, en función del modelo de hospedaje que elija (OwinHost, IIS e IIS-Express).</span><span class="sxs-lookup"><span data-stu-id="925f3-113">There are different ways you can connect your startup class with the runtime, depending on the hosting model you choose (OwinHost, IIS, and IIS-Express).</span></span> <span data-ttu-id="925f3-114">La clase de inicio que se muestra en este tutorial se puede usar en todas las aplicaciones de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="925f3-114">The startup class shown in this tutorial can be used in every hosting application.</span></span> <span data-ttu-id="925f3-115">La clase startup se conecta con el tiempo de ejecución de hospedaje mediante uno de estos métodos:</span><span class="sxs-lookup"><span data-stu-id="925f3-115">You connect the startup class with the hosting runtime using one of the these approaches:</span></span>

1. <span data-ttu-id="925f3-116">**Convención de nomenclatura**: Katana busca una clase denominada `Startup` en el espacio de nombres que coincide con el nombre de ensamblado o el espacio de nombres global.</span><span class="sxs-lookup"><span data-stu-id="925f3-116">**Naming Convention**: Katana looks for a class named `Startup` in namespace matching the assembly name or the global namespace.</span></span>
2. <span data-ttu-id="925f3-117">**Atributo OwinStartup**: este es el método que la mayoría de los desarrolladores tomarán para especificar la clase startup.</span><span class="sxs-lookup"><span data-stu-id="925f3-117">**OwinStartup Attribute**: This is the approach most developers will take to specify the startup class.</span></span> <span data-ttu-id="925f3-118">El atributo siguiente establecerá la clase startup en la clase `TestStartup` en el espacio de nombres `StartupDemo`.</span><span class="sxs-lookup"><span data-stu-id="925f3-118">The following attribute will set the startup class to the `TestStartup` class in the `StartupDemo` namespace.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   <span data-ttu-id="925f3-119">El atributo `OwinStartup` invalida la Convención de nomenclatura.</span><span class="sxs-lookup"><span data-stu-id="925f3-119">The `OwinStartup` attribute overrides the naming convention.</span></span> <span data-ttu-id="925f3-120">También puede especificar un nombre descriptivo con este atributo; sin embargo, si usa un nombre descriptivo, también debe usar el elemento `appSetting` del archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="925f3-120">You can also specify a friendly name with this attribute, however, using a friendly name requires you to also use the `appSetting` element in the configuration file.</span></span>
3. <span data-ttu-id="925f3-121">**El elemento appSetting del archivo de configuración**: el elemento `appSetting` invalida el atributo `OwinStartup` y la Convención de nomenclatura.</span><span class="sxs-lookup"><span data-stu-id="925f3-121">**The appSetting element in the Configuration file**: The `appSetting` element overrides the `OwinStartup` attribute and naming convention.</span></span> <span data-ttu-id="925f3-122">Puede tener varias clases de inicio (cada una con un atributo `OwinStartup`) y configurar la clase de inicio que se cargará en un archivo de configuración con un marcado similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="925f3-122">You can have multiple startup classes (each using an `OwinStartup` attribute) and configure which startup class will be loaded in a configuration file using markup similar to the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   <span data-ttu-id="925f3-123">También se puede usar la clave siguiente, que especifica explícitamente la clase de inicio y el ensamblado:</span><span class="sxs-lookup"><span data-stu-id="925f3-123">The following key, which explicitly specifies the startup class and assembly can also be used:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   <span data-ttu-id="925f3-124">El siguiente XML del archivo de configuración especifica un nombre de clase de inicio descriptivo de `ProductionConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="925f3-124">The following XML in the configuration file specifies a friendly startup class name of `ProductionConfiguration`.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   <span data-ttu-id="925f3-125">El marcado anterior se debe usar con el siguiente `OwinStartup` atributo, que especifica un nombre descriptivo y hace que se ejecute la clase `ProductionStartup2`.</span><span class="sxs-lookup"><span data-stu-id="925f3-125">The above markup must be used with the following `OwinStartup` attribute which specifies a friendly name and causes the `ProductionStartup2` class to run.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. <span data-ttu-id="925f3-126">Para deshabilitar la detección de inicio de OWIN, agregue el `appSetting owin:AutomaticAppStartup` con un valor de `"false"` en el archivo Web. config.</span><span class="sxs-lookup"><span data-stu-id="925f3-126">To disable OWIN startup discovery add the `appSetting owin:AutomaticAppStartup` with a value of `"false"` in the web.config file.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a><span data-ttu-id="925f3-127">Creación de una aplicación Web de ASP.NET con el inicio de OWIN</span><span class="sxs-lookup"><span data-stu-id="925f3-127">Create an ASP.NET Web App using OWIN Startup</span></span>

1. <span data-ttu-id="925f3-128">Cree una aplicación Web de Asp.Net vacía y asígnele el nombre **StartupDemo**.</span><span class="sxs-lookup"><span data-stu-id="925f3-128">Create an empty Asp.Net web application and name it **StartupDemo**.</span></span> <span data-ttu-id="925f3-129">-Instalar `Microsoft.Owin.Host.SystemWeb` mediante el administrador de paquetes NuGet.</span><span class="sxs-lookup"><span data-stu-id="925f3-129">- Install `Microsoft.Owin.Host.SystemWeb` using the NuGet package manager.</span></span> <span data-ttu-id="925f3-130">En el menú **herramientas** , seleccione **Administrador de paquetes NuGet**y, a continuación, **consola del administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="925f3-130">From the **Tools** menu, select **NuGet Package Manager**, and then **Package Manager Console**.</span></span> <span data-ttu-id="925f3-131">Escriba el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="925f3-131">Enter the following command:</span></span>

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. <span data-ttu-id="925f3-132">Agregue una clase de inicio OWIN.</span><span class="sxs-lookup"><span data-stu-id="925f3-132">Add an OWIN startup class.</span></span> <span data-ttu-id="925f3-133">En Visual Studio 2017, haga clic con el botón derecho en el proyecto y seleccione **Agregar clase**.-en el cuadro de diálogo **Agregar nuevo elemento** , escriba *OWIN* en el campo de búsqueda y cambie el nombre a startup.CS y, a continuación, seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="925f3-133">In Visual Studio 2017 right-click the project and select **Add Class**.- In the **Add New Item** dialog box, enter *OWIN* in the search field, and change the name to Startup.cs, and then select **Add**.</span></span>

     ![](owin-startup-class-detection/_static/image1.png)

   <span data-ttu-id="925f3-134">La próxima vez que desee agregar una *clase de inicio Owin*, estará disponible en el menú **Agregar** .</span><span class="sxs-lookup"><span data-stu-id="925f3-134">The next time you want to add an *Owin Startup class*, it will be in available from the **Add** menu.</span></span>

     ![](owin-startup-class-detection/_static/image2.png)

   <span data-ttu-id="925f3-135">Como alternativa, puede hacer clic con el botón derecho en el proyecto, seleccionar **Agregar**, seleccionar **nuevo elemento**y, a continuación, seleccionar la **clase de inicio Owin**.</span><span class="sxs-lookup"><span data-stu-id="925f3-135">Alternatively, you can right-click the project and select **Add**, then select **New Item**, and then select the **Owin Startup class**.</span></span>

     ![](owin-startup-class-detection/_static/image3.png)

- <span data-ttu-id="925f3-136">Reemplace el código generado en el archivo *Startup.CS* por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="925f3-136">Replace the generated code in the *Startup.cs* file with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]

  <span data-ttu-id="925f3-137">El `app.Use` expresión lambda se usa para registrar el componente de middleware especificado en la canalización OWIN.</span><span class="sxs-lookup"><span data-stu-id="925f3-137">The `app.Use` lambda expression is used to register the specified middleware component to the OWIN pipeline.</span></span> <span data-ttu-id="925f3-138">En este caso, estamos configurando el registro de las solicitudes entrantes antes de responder a la solicitud entrante.</span><span class="sxs-lookup"><span data-stu-id="925f3-138">In this case we are setting up logging of incoming requests before responding to the incoming request.</span></span> <span data-ttu-id="925f3-139">El parámetro `next` es el delegado ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt;) al siguiente componente de la canalización.</span><span class="sxs-lookup"><span data-stu-id="925f3-139">The `next` parameter is the delegate ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) to the next component in the pipeline.</span></span> <span data-ttu-id="925f3-140">La expresión lambda `app.Run` enlaza la canalización a las solicitudes entrantes y proporciona el mecanismo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="925f3-140">The `app.Run` lambda expression hooks up the pipeline to incoming requests and provides the response mechanism.</span></span>
     > [!NOTE]
     > <span data-ttu-id="925f3-141">En el código anterior hemos comentado el atributo `OwinStartup` y estamos confiando en la Convención de ejecución de la clase denominada `Startup`.-Presione ***F5*** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="925f3-141">In the code above we have commented out the `OwinStartup` attribute and we're relying on the convention of running the class named `Startup` .- Press ***F5*** to run the application.</span></span> <span data-ttu-id="925f3-142">Actualización de aciertos varias veces.</span><span class="sxs-lookup"><span data-stu-id="925f3-142">Hit refresh a few times.</span></span>

    <span data-ttu-id="925f3-143">![](owin-startup-class-detection/_static/image4.png) Nota: el número que se muestra en las imágenes de este tutorial no coincidirá con el número que vea.</span><span class="sxs-lookup"><span data-stu-id="925f3-143">![](owin-startup-class-detection/_static/image4.png) Note: The number shown in the images in this tutorial will not match the number you see.</span></span> <span data-ttu-id="925f3-144">La cadena de milisegundos se usa para mostrar una nueva respuesta al actualizar la página.</span><span class="sxs-lookup"><span data-stu-id="925f3-144">The millisecond string is used to show a new response when you refresh the page.</span></span>
  <span data-ttu-id="925f3-145">Puede ver la información de seguimiento en la ventana de **salida** .</span><span class="sxs-lookup"><span data-stu-id="925f3-145">You can see the trace information in the **Output** window.</span></span>

    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a><span data-ttu-id="925f3-146">Agregar más clases de inicio</span><span class="sxs-lookup"><span data-stu-id="925f3-146">Add More Startup Classes</span></span>

<span data-ttu-id="925f3-147">En esta sección, vamos a agregar otra clase de inicio.</span><span class="sxs-lookup"><span data-stu-id="925f3-147">In this section we'll add another Startup class.</span></span> <span data-ttu-id="925f3-148">Puede agregar varias clases de inicio de OWIN a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="925f3-148">You can add multiple OWIN startup class to your application.</span></span> <span data-ttu-id="925f3-149">Por ejemplo, puede que desee crear clases de inicio para desarrollo, pruebas y producción.</span><span class="sxs-lookup"><span data-stu-id="925f3-149">For example, you might want to create startup classes for development, testing and production.</span></span>

1. <span data-ttu-id="925f3-150">Cree una nueva clase de inicio OWIN y asígnele el nombre `ProductionStartup`.</span><span class="sxs-lookup"><span data-stu-id="925f3-150">Create a new OWIN Startup class and name it `ProductionStartup`.</span></span>
2. <span data-ttu-id="925f3-151">Reemplace el código generado con el siguiente:</span><span class="sxs-lookup"><span data-stu-id="925f3-151">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. <span data-ttu-id="925f3-152">Presione Ctrl F5 para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="925f3-152">Press Control F5 to run the app.</span></span> <span data-ttu-id="925f3-153">El atributo `OwinStartup` especifica que se ejecuta la clase de inicio de producción.</span><span class="sxs-lookup"><span data-stu-id="925f3-153">The `OwinStartup` attribute specifies the production startup class is run.</span></span>

    ![](owin-startup-class-detection/_static/image6.png)
4. <span data-ttu-id="925f3-154">Cree otra clase de inicio de OWIN y asígnele el nombre `TestStartup`.</span><span class="sxs-lookup"><span data-stu-id="925f3-154">Create another OWIN Startup class and name it `TestStartup`.</span></span>
5. <span data-ttu-id="925f3-155">Reemplace el código generado con el siguiente:</span><span class="sxs-lookup"><span data-stu-id="925f3-155">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   <span data-ttu-id="925f3-156">La sobrecarga de `OwinStartup` atributo anterior especifica `TestingConfiguration` como el nombre *descriptivo* de la clase startup.</span><span class="sxs-lookup"><span data-stu-id="925f3-156">The `OwinStartup` attribute overload above specifies `TestingConfiguration` as the *friendly* name of the Startup class.</span></span>
6. <span data-ttu-id="925f3-157">Abra el archivo *Web. config* y agregue la clave de inicio de la aplicación OWIN que especifica el nombre descriptivo de la clase startup:</span><span class="sxs-lookup"><span data-stu-id="925f3-157">Open the *web.config* file and add the OWIN App startup key which specifies the friendly name of the Startup class:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. <span data-ttu-id="925f3-158">Presione Ctrl F5 para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="925f3-158">Press Control F5 to run the app.</span></span> <span data-ttu-id="925f3-159">El elemento de configuración de la aplicación toma el precedente y se ejecuta la configuración de prueba.</span><span class="sxs-lookup"><span data-stu-id="925f3-159">The app settings element takes precedent, and the test configuration is run.</span></span>

    ![](owin-startup-class-detection/_static/image7.png)
8. <span data-ttu-id="925f3-160">Quite el nombre *descriptivo* del atributo `OwinStartup` de la clase `TestStartup`.</span><span class="sxs-lookup"><span data-stu-id="925f3-160">Remove the *friendly* name from the `OwinStartup` attribute in the `TestStartup` class.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. <span data-ttu-id="925f3-161">Reemplace la clave de inicio de la aplicación OWIN en el archivo *Web. config* por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="925f3-161">Replace the OWIN App startup key in the *web.config* file with the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. <span data-ttu-id="925f3-162">Revierta el atributo `OwinStartup` de cada clase en el código de atributo predeterminado generado por Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="925f3-162">Revert the `OwinStartup` attribute in each class to the default attribute code generated by Visual Studio:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    <span data-ttu-id="925f3-163">Cada una de las claves de inicio de la aplicación OWIN siguiente hará que se ejecute la clase Production.</span><span class="sxs-lookup"><span data-stu-id="925f3-163">Each of the OWIN App startup keys below will cause the production class to run.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    <span data-ttu-id="925f3-164">La última clave de inicio especifica el método de configuración de inicio.</span><span class="sxs-lookup"><span data-stu-id="925f3-164">The last startup key specifies the startup configuration method.</span></span> <span data-ttu-id="925f3-165">La siguiente clave de inicio de la aplicación OWIN permite cambiar el nombre de la clase de configuración a `MyConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="925f3-165">The following OWIN App startup key allows you to change the name of the configuration class to `MyConfiguration` .</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a><span data-ttu-id="925f3-166">Usar Owinhost. exe</span><span class="sxs-lookup"><span data-stu-id="925f3-166">Using Owinhost.exe</span></span>

1. <span data-ttu-id="925f3-167">Reemplace el archivo Web. config por el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="925f3-167">Replace the Web.config file with the following markup:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   <span data-ttu-id="925f3-168">La última clave gana, por lo que en este caso se especifica `TestStartup`.</span><span class="sxs-lookup"><span data-stu-id="925f3-168">The last key wins, so in this case `TestStartup` is specified.</span></span>
2. <span data-ttu-id="925f3-169">Instale Owinhost desde el PMC:</span><span class="sxs-lookup"><span data-stu-id="925f3-169">Install Owinhost from the PMC:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. <span data-ttu-id="925f3-170">Navegue hasta la carpeta de la aplicación (la carpeta que contiene el archivo *Web. config* ) y en un símbolo del sistema y escriba:</span><span class="sxs-lookup"><span data-stu-id="925f3-170">Navigate to the application folder (the folder containing the *Web.config* file) and in a command prompt and type:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   <span data-ttu-id="925f3-171">La ventana comandos mostrará:</span><span class="sxs-lookup"><span data-stu-id="925f3-171">The command window will show:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. <span data-ttu-id="925f3-172">Inicie un explorador con la dirección URL `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="925f3-172">Launch a browser with the URL `http://localhost:5000/`.</span></span>

    ![](owin-startup-class-detection/_static/image8.png)

   <span data-ttu-id="925f3-173">OwinHost respeta las convenciones de inicio mencionadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="925f3-173">OwinHost honored the startup conventions listed above.</span></span>
5. <span data-ttu-id="925f3-174">En la ventana de comandos, presione Entrar para salir de OwinHost.</span><span class="sxs-lookup"><span data-stu-id="925f3-174">In the command window, press Enter to exit OwinHost.</span></span>
6. <span data-ttu-id="925f3-175">En la clase `ProductionStartup`, agregue el siguiente atributo OwinStartup que especifica un nombre descriptivo de *ProductionConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="925f3-175">In the `ProductionStartup` class, add the following OwinStartup attribute which specifies a friendly name of *ProductionConfiguration*.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. <span data-ttu-id="925f3-176">En el símbolo del sistema y escriba:</span><span class="sxs-lookup"><span data-stu-id="925f3-176">In the command prompt and type:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   <span data-ttu-id="925f3-177">Se carga la clase de inicio de producción.</span><span class="sxs-lookup"><span data-stu-id="925f3-177">The Production startup class is loaded.</span></span>
    ![](owin-startup-class-detection/_static/image9.png)

   <span data-ttu-id="925f3-178">Nuestra aplicación tiene varias clases de inicio y en este ejemplo hemos diferido qué clase de inicio se va a cargar hasta el tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="925f3-178">Our application has multiple startup classes, and in this example we have deferred which startup class to load until runtime.</span></span>
8. <span data-ttu-id="925f3-179">Pruebe las siguientes opciones de inicio en tiempo de ejecución:</span><span class="sxs-lookup"><span data-stu-id="925f3-179">Test the following runtime startup options:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
