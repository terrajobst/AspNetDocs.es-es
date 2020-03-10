---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'Implementación web de ASP.NET con Visual Studio: implementación de la línea de comandos | Microsoft Docs'
author: tdykstra
description: En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros, por usa...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: 13cfe4492398b59f2c80394689cc113ccb218c60
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78512077"
---
# <a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a><span data-ttu-id="63dc4-103">Implementación web de ASP.NET con Visual Studio: implementación desde la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="63dc4-103">ASP.NET Web Deployment using Visual Studio: Command Line Deployment</span></span>

<span data-ttu-id="63dc4-104">por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="63dc4-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="63dc4-105">Descargar el proyecto de inicio</span><span class="sxs-lookup"><span data-stu-id="63dc4-105">Download Starter Project</span></span>](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="63dc4-106">En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros mediante Visual Studio 2012 o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="63dc4-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="63dc4-107">Para obtener información sobre la serie, vea [el primer tutorial de la serie](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="63dc4-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>

## <a name="overview"></a><span data-ttu-id="63dc4-108">Información general</span><span class="sxs-lookup"><span data-stu-id="63dc4-108">Overview</span></span>

<span data-ttu-id="63dc4-109">En este tutorial se muestra cómo invocar la canalización de publicación Web de Visual Studio desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="63dc4-109">This tutorial shows you how to invoke the Visual Studio web publish pipeline from the command line.</span></span> <span data-ttu-id="63dc4-110">Esto resulta útil en escenarios en los que desea [automatizar el proceso de implementación](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) en lugar de hacerlo manualmente en Visual Studio, normalmente mediante un [sistema de control de versiones de código fuente](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span><span class="sxs-lookup"><span data-stu-id="63dc4-110">This is useful for scenarios where you want to [automate the deployment process](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) instead of doing it manually in Visual Studio, typically by using a [source code version control system](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span></span>

## <a name="make-a-change-to-deploy"></a><span data-ttu-id="63dc4-111">Realizar un cambio en la implementación</span><span class="sxs-lookup"><span data-stu-id="63dc4-111">Make a change to deploy</span></span>

<span data-ttu-id="63dc4-112">Actualmente en la página acerca de se muestra el código de plantilla.</span><span class="sxs-lookup"><span data-stu-id="63dc4-112">Currently the About page displays the template code.</span></span>

![Página acerca de con código de plantilla](command-line-deployment/_static/image1.png)

<span data-ttu-id="63dc4-114">Lo reemplazará por código que muestra un resumen de la inscripción de estudiantes.</span><span class="sxs-lookup"><span data-stu-id="63dc4-114">You'll replace that with code that displays a summary of student enrollment.</span></span>

<span data-ttu-id="63dc4-115">Abra la página *About. aspx* , elimine todo el marcado dentro del elemento `MainContent` `Content` e inserte el marcado siguiente en su lugar:</span><span class="sxs-lookup"><span data-stu-id="63dc4-115">Open the *About.aspx* page, delete all of the markup inside the `MainContent` `Content` element, and insert the following markup in its place:</span></span>

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

<span data-ttu-id="63dc4-116">Ejecute el proyecto y seleccione la página **About** .</span><span class="sxs-lookup"><span data-stu-id="63dc4-116">Run the project and select the **About** page.</span></span>

![Página About](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a><span data-ttu-id="63dc4-118">Implementar para probar mediante la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="63dc4-118">Deploy to Test by using the command line</span></span>

<span data-ttu-id="63dc4-119">No va a implementar otro cambio en la base de datos, por lo que debe deshabilitar la implementación de la base de datos dbDacFx para la base de datos ASPNET-ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="63dc4-119">You won't be deploying another database change, so disable dbDacFx database deployment for the aspnet-ContosoUniversity database.</span></span> <span data-ttu-id="63dc4-120">Abra el Asistente para **publicación web** y, en cada uno de los tres perfiles de publicación, desactive la casilla **Actualizar base de datos** en la pestaña **configuración** .</span><span class="sxs-lookup"><span data-stu-id="63dc4-120">Open the **Publish Web** wizard, and in each of the three publish profiles, clear the **Update Database** check box on the **Settings** tab.</span></span>

<span data-ttu-id="63dc4-121">En la página de inicio de Windows 8, busque **símbolo del sistema para desarrolladores para VS2012**.</span><span class="sxs-lookup"><span data-stu-id="63dc4-121">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**.</span></span>

<span data-ttu-id="63dc4-122">Haga clic con el botón derecho en el icono de **símbolo del sistema para desarrolladores de VS2012** y haga clic en **Ejecutar como administrador**.</span><span class="sxs-lookup"><span data-stu-id="63dc4-122">Right-click the icon for **Developer Command Prompt for VS2012** and click **Run as administrator**.</span></span>

<span data-ttu-id="63dc4-123">Escriba el siguiente comando en el símbolo del sistema y reemplace la ruta de acceso al archivo de solución por la ruta de acceso al archivo de solución:</span><span class="sxs-lookup"><span data-stu-id="63dc4-123">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file:</span></span>

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

<span data-ttu-id="63dc4-124">MSBuild compila la solución y la implementa en el entorno de prueba.</span><span class="sxs-lookup"><span data-stu-id="63dc4-124">MSBuild builds the solution and deploys it to the test environment.</span></span>

![Salida de la línea de comandos](command-line-deployment/_static/image3.png)

<span data-ttu-id="63dc4-126">Abra un explorador y vaya a `http://localhost/ContosoUniversity`y, a continuación, haga clic en la página **About** para comprobar que la implementación se realizó correctamente.</span><span class="sxs-lookup"><span data-stu-id="63dc4-126">Open a browser and go to `http://localhost/ContosoUniversity`, then click the **About** page to verify that the deployment was successful.</span></span>

<span data-ttu-id="63dc4-127">Si no ha creado ningún estudiante en la prueba, verá una página vacía en el encabezado de **estadísticas del cuerpo del alumno** .</span><span class="sxs-lookup"><span data-stu-id="63dc4-127">If you haven't created any students in test, you'll see an empty page under the **Student Body Statistics** heading.</span></span> <span data-ttu-id="63dc4-128">Vaya a la página **Students** , haga clic en **Agregar Student**y agregue algunos estudiantes y, a continuación, vuelva a la página **About** para ver las estadísticas de los estudiantes.</span><span class="sxs-lookup"><span data-stu-id="63dc4-128">Go to the **Students** page, click **Add Student**, and add some students, and then return to the **About** page to see student statistics.</span></span>

![Página acerca de en el entorno de prueba](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a><span data-ttu-id="63dc4-130">Opciones de la línea de comandos de clave</span><span class="sxs-lookup"><span data-stu-id="63dc4-130">Key command line options</span></span>

<span data-ttu-id="63dc4-131">El comando que ha especificado ha pasado la ruta de acceso del archivo de solución y dos propiedades a MSBuild:</span><span class="sxs-lookup"><span data-stu-id="63dc4-131">The command that you entered passed the solution file path and two properties to MSBuild:</span></span>

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a><span data-ttu-id="63dc4-132">Implementación de la solución en comparación con la implementación de proyectos individuales</span><span class="sxs-lookup"><span data-stu-id="63dc4-132">Deploying the solution versus deploying individual projects</span></span>

<span data-ttu-id="63dc4-133">Al especificar el archivo de solución, se generan todos los proyectos de la solución.</span><span class="sxs-lookup"><span data-stu-id="63dc4-133">Specifying the solution file causes all projects in the solution to be built.</span></span> <span data-ttu-id="63dc4-134">Si tiene varios proyectos web en la solución, se aplica el siguiente comportamiento de MSBuild:</span><span class="sxs-lookup"><span data-stu-id="63dc4-134">If you have multiple web projects in the solution, the following MSBuild behavior applies:</span></span>

- <span data-ttu-id="63dc4-135">Las propiedades que se especifican en la línea de comandos se pasan a todos los proyectos.</span><span class="sxs-lookup"><span data-stu-id="63dc4-135">The properties that you specify on the command line are passed to every project.</span></span> <span data-ttu-id="63dc4-136">Por lo tanto, cada proyecto web debe tener un perfil de publicación con el nombre que especifique.</span><span class="sxs-lookup"><span data-stu-id="63dc4-136">Therefore, each web project must have a publish profile with the name that you specify.</span></span> <span data-ttu-id="63dc4-137">Si especifica `/p:PublishProfile=Test`, cada proyecto web debe tener un perfil de publicación denominado *Test*.</span><span class="sxs-lookup"><span data-stu-id="63dc4-137">If you specify `/p:PublishProfile=Test`, each web project must have a publish profile named *Test*.</span></span>
- <span data-ttu-id="63dc4-138">Es posible que publique correctamente un proyecto cuando no se haya compilando otro.</span><span class="sxs-lookup"><span data-stu-id="63dc4-138">You might successfully publish one project when another one doesn't even build.</span></span> <span data-ttu-id="63dc4-139">Para obtener más información, vea el subproceso de stackoverflow [msbuild genera un error con dos paquetes](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span><span class="sxs-lookup"><span data-stu-id="63dc4-139">For more information, see the stackoverflow thread [MSBuild fails with two packages](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span></span>

<span data-ttu-id="63dc4-140">Si especifica un proyecto individual en lugar de una solución, tendrá que agregar un parámetro que especifique la versión de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="63dc4-140">If you specify an individual project instead of a solution, you have to add a parameter that specifies the Visual Studio version.</span></span> <span data-ttu-id="63dc4-141">Si usa Visual Studio 2012, la línea de comandos sería similar al ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="63dc4-141">If you are using Visual Studio 2012 the command line would be similar to the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

<span data-ttu-id="63dc4-142">El número de versión de Visual Studio 2010 es 10,0.</span><span class="sxs-lookup"><span data-stu-id="63dc4-142">The version number for Visual Studio 2010 is 10.0.</span></span> <span data-ttu-id="63dc4-143">Para obtener más información, vea [compatibilidad de proyectos de Visual Studio y VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) en el blog de Sayed Hashimi.</span><span class="sxs-lookup"><span data-stu-id="63dc4-143">For more information, see [Visual Studio project compatibility and VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) on Sayed Hashimi's blog.</span></span>

### <a name="specifying-the-publish-profile"></a><span data-ttu-id="63dc4-144">Especificar el perfil de publicación</span><span class="sxs-lookup"><span data-stu-id="63dc4-144">Specifying the publish profile</span></span>

<span data-ttu-id="63dc4-145">Puede especificar el perfil de publicación por nombre o la ruta de acceso completa al archivo *. pubxml* , como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="63dc4-145">You can specify the publish profile by name or by the full path to the *.pubxml* file, as shown in the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a><span data-ttu-id="63dc4-146">Métodos de publicación Web compatibles con la publicación en la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="63dc4-146">Web publish methods supported for command-line publishing</span></span>

<span data-ttu-id="63dc4-147">Se admiten tres métodos de publicación para la publicación de la línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="63dc4-147">Three publish methods are supported for command line publishing:</span></span>

- <span data-ttu-id="63dc4-148">`MSDeploy`-publicar mediante Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="63dc4-148">`MSDeploy` - Publish by using Web Deploy.</span></span>
- <span data-ttu-id="63dc4-149">`Package`-publicar creando un paquete de Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="63dc4-149">`Package` - Publish by creating a Web Deploy Package.</span></span> <span data-ttu-id="63dc4-150">Tiene que instalar el paquete por separado desde el comando de MSBuild que lo crea.</span><span class="sxs-lookup"><span data-stu-id="63dc4-150">You have to install the package separately from the MSBuild command that creates it.</span></span>
- <span data-ttu-id="63dc4-151">`FileSystem`: publicar mediante la copia de archivos en una carpeta especificada.</span><span class="sxs-lookup"><span data-stu-id="63dc4-151">`FileSystem` - Publish by copying files to a specified folder.</span></span>

### <a name="specifying-the-build-configuration-and-platform"></a><span data-ttu-id="63dc4-152">Especificar la configuración de compilación y la plataforma</span><span class="sxs-lookup"><span data-stu-id="63dc4-152">Specifying the build configuration and platform</span></span>

<span data-ttu-id="63dc4-153">La configuración de compilación y la plataforma deben establecerse en Visual Studio o en la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="63dc4-153">The build configuration and platform must be set in Visual Studio or on the command line.</span></span> <span data-ttu-id="63dc4-154">Los perfiles de publicación incluyen propiedades que se denominan `LastUsedBuildConfiguration` y `LastUsedPlatform`, pero no puede establecer estas propiedades para determinar cómo se compila el proyecto.</span><span class="sxs-lookup"><span data-stu-id="63dc4-154">The publish profiles include properties that are named `LastUsedBuildConfiguration` and `LastUsedPlatform`, but you can't set these properties in order to determine how the project is built.</span></span> <span data-ttu-id="63dc4-155">Para obtener más información, vea [msbuild: How to Set the Configuration Property en el](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) blog de Sayed Hashimi.</span><span class="sxs-lookup"><span data-stu-id="63dc4-155">For more information, see [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) on Sayed Hashimi's blog.</span></span>

## <a name="deploy-to-staging"></a><span data-ttu-id="63dc4-156">Implementación en entorno de ensayo</span><span class="sxs-lookup"><span data-stu-id="63dc4-156">Deploy to staging</span></span>

<span data-ttu-id="63dc4-157">Para implementar en Azure, debe agregar la contraseña a la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="63dc4-157">To deploy to Azure, you must add the password to the command line.</span></span> <span data-ttu-id="63dc4-158">Si ha guardado la contraseña en el perfil de publicación en Visual Studio, se almacenará en formato cifrado en el archivo. *pubxml. User* .</span><span class="sxs-lookup"><span data-stu-id="63dc4-158">If you saved the password in the publish profile in Visual Studio, it was stored in encrypted form in the your *.pubxml.user* file.</span></span> <span data-ttu-id="63dc4-159">MSBuild no tiene acceso a ese archivo al realizar una implementación de línea de comandos, por lo que tiene que pasar la contraseña en un parámetro de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="63dc4-159">That file is not accessed by MSBuild when you do a command line deployment, so you have to pass in the password in a command line parameter.</span></span>

1. <span data-ttu-id="63dc4-160">Copie la contraseña que necesita del archivo *. publishsettings* que descargó anteriormente para el sitio web de ensayo.</span><span class="sxs-lookup"><span data-stu-id="63dc4-160">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the staging web site.</span></span> <span data-ttu-id="63dc4-161">La contraseña es el valor del atributo `userPWD` del elemento `publishProfile` Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="63dc4-161">The password is the value of the `userPWD` attribute for the Web Deploy `publishProfile` element.</span></span>

    ![Web Deploy contraseña](command-line-deployment/_static/image5.png)
2. <span data-ttu-id="63dc4-163">En la página de inicio de Windows 8, busque **símbolo del sistema para desarrolladores de VS2012**y haga clic en el icono para abrir el símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="63dc4-163">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**, and click the icon to open the command prompt.</span></span> <span data-ttu-id="63dc4-164">(No tiene que abrirlo como administrador esta vez porque no está implementando en IIS en el equipo local).</span><span class="sxs-lookup"><span data-stu-id="63dc4-164">(You don't have to open it as administrator this time because you aren't deploying to IIS on the local computer.)</span></span>
3. <span data-ttu-id="63dc4-165">Escriba el siguiente comando en el símbolo del sistema y reemplace la ruta de acceso al archivo de solución por la ruta de acceso al archivo de solución y la contraseña por la contraseña:</span><span class="sxs-lookup"><span data-stu-id="63dc4-165">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    <span data-ttu-id="63dc4-166">Tenga en cuenta que esta línea de comandos incluye un parámetro adicional: `/p:AllowUntrustedCertificate=true`.</span><span class="sxs-lookup"><span data-stu-id="63dc4-166">Notice that this command line includes an extra parameter: `/p:AllowUntrustedCertificate=true`.</span></span> <span data-ttu-id="63dc4-167">Cuando se escribe este tutorial, se debe establecer la propiedad `AllowUntrustedCertificate` al publicar en Azure desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="63dc4-167">As this tutorial is being written, the `AllowUntrustedCertificate` property must be set when you publish to Azure from the command line.</span></span> <span data-ttu-id="63dc4-168">Cuando se lance la corrección para este error, no necesitará ese parámetro.</span><span class="sxs-lookup"><span data-stu-id="63dc4-168">When the fix for this bug is released, you won't need that parameter.</span></span>
4. <span data-ttu-id="63dc4-169">Abra un explorador y vaya a la dirección URL del sitio de ensayo y, a continuación, haga clic en la página **acerca** de para comprobar que la implementación se realizó correctamente.</span><span class="sxs-lookup"><span data-stu-id="63dc4-169">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

    <span data-ttu-id="63dc4-170">Como vimos anteriormente para el entorno de prueba, es posible que tenga que crear algunos estudiantes para ver las estadísticas en la página **acerca** de.</span><span class="sxs-lookup"><span data-stu-id="63dc4-170">As you saw earlier for the test environment, you might have to create some students to see statistics on the **About** page.</span></span>

## <a name="deploy-to-production"></a><span data-ttu-id="63dc4-171">Implementar en producción</span><span class="sxs-lookup"><span data-stu-id="63dc4-171">Deploy to production</span></span>

<span data-ttu-id="63dc4-172">El proceso de implementación en producción es similar al proceso de ensayo.</span><span class="sxs-lookup"><span data-stu-id="63dc4-172">The process for deploying to production is similar to the process for staging.</span></span>

1. <span data-ttu-id="63dc4-173">Copie la contraseña que necesita del archivo *. publishsettings* que descargó anteriormente para el sitio web de producción.</span><span class="sxs-lookup"><span data-stu-id="63dc4-173">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the production web site.</span></span>
2. <span data-ttu-id="63dc4-174">Abra **símbolo del sistema para desarrolladores para VS2012**.</span><span class="sxs-lookup"><span data-stu-id="63dc4-174">Open **Developer Command Prompt for VS2012**.</span></span>
3. <span data-ttu-id="63dc4-175">Escriba el siguiente comando en el símbolo del sistema y reemplace la ruta de acceso al archivo de solución por la ruta de acceso al archivo de solución y la contraseña por la contraseña:</span><span class="sxs-lookup"><span data-stu-id="63dc4-175">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    <span data-ttu-id="63dc4-176">Para un sitio de producción real, si también se ha producido un cambio en la base de datos, normalmente se copiará la *aplicación\_archivo. htm sin conexión* en el sitio antes de la implementación y se eliminará después de la implementación correcta.</span><span class="sxs-lookup"><span data-stu-id="63dc4-176">For a real production site, if there was also a database change, you would typically copy the *app\_offline.htm* file to the site before deployment and delete it after successful deployment.</span></span>
4. <span data-ttu-id="63dc4-177">Abra un explorador y vaya a la dirección URL del sitio de ensayo y, a continuación, haga clic en la página **acerca** de para comprobar que la implementación se realizó correctamente.</span><span class="sxs-lookup"><span data-stu-id="63dc4-177">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

## <a name="summary"></a><span data-ttu-id="63dc4-178">Resumen</span><span class="sxs-lookup"><span data-stu-id="63dc4-178">Summary</span></span>

<span data-ttu-id="63dc4-179">Ahora ha implementado una actualización de la aplicación mediante la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="63dc4-179">You have now deployed an application update by using the command line.</span></span>

![Página acerca de en el entorno de prueba](command-line-deployment/_static/image6.png)

<span data-ttu-id="63dc4-181">En el siguiente tutorial, verá un ejemplo de cómo extender la canalización de publicación Web.</span><span class="sxs-lookup"><span data-stu-id="63dc4-181">In the next tutorial, you will see an example of how to extend the web publish pipeline.</span></span> <span data-ttu-id="63dc4-182">En el ejemplo se muestra cómo implementar archivos que no están incluidos en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="63dc4-182">The example will show you how to deploy files that are not included in the project.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="63dc4-183">[Anterior](deploying-a-database-update.md)
> [Siguiente](deploying-extra-files.md)</span><span class="sxs-lookup"><span data-stu-id="63dc4-183">[Previous](deploying-a-database-update.md)
[Next](deploying-extra-files.md)</span></span>
