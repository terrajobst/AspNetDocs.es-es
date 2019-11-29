---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Implementación web de ASP.NET con Visual Studio: implementación de una actualización de código | Microsoft Docs'
author: tdykstra
description: En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros, por usa...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: 3881833bfe2a50a38a357614f92f434a04a8ab08
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74626777"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a><span data-ttu-id="1bb18-103">Implementación web de ASP.NET con Visual Studio: implementación de una actualización de código</span><span class="sxs-lookup"><span data-stu-id="1bb18-103">ASP.NET Web Deployment using Visual Studio: Deploying a Code Update</span></span>

<span data-ttu-id="1bb18-104">por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="1bb18-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="1bb18-105">Descargar el proyecto de inicio</span><span class="sxs-lookup"><span data-stu-id="1bb18-105">Download Starter Project</span></span>](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="1bb18-106">En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros mediante Visual Studio 2012 o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="1bb18-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="1bb18-107">Para obtener información sobre la serie, vea [el primer tutorial de la serie](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1bb18-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>

## <a name="overview"></a><span data-ttu-id="1bb18-108">Información general del</span><span class="sxs-lookup"><span data-stu-id="1bb18-108">Overview</span></span>

<span data-ttu-id="1bb18-109">Después de la implementación inicial, el trabajo de mantenimiento y desarrollo del sitio web continúa y antes de que se desee implementar una actualización.</span><span class="sxs-lookup"><span data-stu-id="1bb18-109">After the initial deployment, your work of maintaining and developing your web site continues, and before long you will want to deploy an update.</span></span> <span data-ttu-id="1bb18-110">Este tutorial le guiará por el proceso de implementación de una actualización en el código de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1bb18-110">This tutorial takes you through the process of deploying an update to your application code.</span></span> <span data-ttu-id="1bb18-111">La actualización que implementa e implementa en este tutorial no implica un cambio en la base de datos; en el siguiente tutorial, verá qué es diferente de la implementación de un cambio en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1bb18-111">The update that you implement and deploy in this tutorial does not involve a database change; you'll see what's different about deploying a database change in the next tutorial.</span></span>

<span data-ttu-id="1bb18-112">Aviso: Si recibe un mensaje de error o algo no funciona a medida que avanza en el tutorial, asegúrese de consultar la [Página de solución de problemas](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="1bb18-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="make-a-code-change"></a><span data-ttu-id="1bb18-113">Hacer un cambio de código</span><span class="sxs-lookup"><span data-stu-id="1bb18-113">Make a code change</span></span>

<span data-ttu-id="1bb18-114">Como ejemplo sencillo de una actualización de la aplicación, agregará a la página de **instructores** una lista de cursos impartidos por el instructor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="1bb18-114">As a simple example of an update to your application, you'll add to the **Instructors** page a list of courses taught by the selected instructor.</span></span>

<span data-ttu-id="1bb18-115">Si ejecuta la página **instructores** , observará que hay vínculos **seleccionados** en la cuadrícula, pero que no hacen nada más que hacer que el fondo de la fila se convierta en gris.</span><span class="sxs-lookup"><span data-stu-id="1bb18-115">If you run the **Instructors** page, you'll notice that there are **Select** links in the grid, but they don't do anything other than make the row background turn gray.</span></span>

![Página de instructores con selección](deploying-a-code-update/_static/image1.png)

<span data-ttu-id="1bb18-117">Ahora agregará el código que se ejecuta cuando se hace clic en el vínculo **seleccionar** y muestra una lista de los cursos impartidos por el instructor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="1bb18-117">Now you'll add code that runs when the **Select** link is clicked and displays a list of courses taught by the selected instructor .</span></span>

1. <span data-ttu-id="1bb18-118">En *instructors. aspx*, agregue el siguiente marcado inmediatamente después del control **ErrorMessageLabel** `Label`:</span><span class="sxs-lookup"><span data-stu-id="1bb18-118">In *Instructors.aspx*, add the following markup immediately after the **ErrorMessageLabel** `Label` control:</span></span>

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. <span data-ttu-id="1bb18-119">Ejecute la página y seleccione un instructor.</span><span class="sxs-lookup"><span data-stu-id="1bb18-119">Run the page and select an instructor.</span></span> <span data-ttu-id="1bb18-120">Verá una lista de los cursos impartidos por ese instructor.</span><span class="sxs-lookup"><span data-stu-id="1bb18-120">You see a list of courses taught by that instructor.</span></span>

    ![Página de instructores con cursos impartidos](deploying-a-code-update/_static/image2.png)
3. <span data-ttu-id="1bb18-122">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="1bb18-122">Close the browser.</span></span>

## <a name="deploy-the-code-update-to-the-test-environment"></a><span data-ttu-id="1bb18-123">Implementar la actualización del código en el entorno de prueba</span><span class="sxs-lookup"><span data-stu-id="1bb18-123">Deploy the code update to the test environment</span></span>

<span data-ttu-id="1bb18-124">Antes de poder usar los perfiles de publicación para implementar en pruebas, ensayo y producción, debe cambiar las opciones de publicación de bases de datos.</span><span class="sxs-lookup"><span data-stu-id="1bb18-124">Before you can use your publish profiles to deploy to test, staging, and production, you need to change database publishing options.</span></span> <span data-ttu-id="1bb18-125">Ya no es necesario ejecutar los scripts de implementación de datos y de concesión para la base de datos de pertenencia.</span><span class="sxs-lookup"><span data-stu-id="1bb18-125">You no longer need to run the grant and data deployment scripts for the membership database.</span></span>

1. <span data-ttu-id="1bb18-126">Para abrir el Asistente para **publicación web** , haga clic con el botón secundario en el proyecto ContosoUniversity y haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="1bb18-126">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="1bb18-127">Haga clic en el perfil de **prueba** en la lista desplegable **perfil** .</span><span class="sxs-lookup"><span data-stu-id="1bb18-127">Click the **Test** profile in the **Profile** drop-down list.</span></span>
3. <span data-ttu-id="1bb18-128">Haga clic en la pestaña **configuración** .</span><span class="sxs-lookup"><span data-stu-id="1bb18-128">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="1bb18-129">En **DefaultConnection** en la sección **bases** de datos, desactive la casilla **Actualizar base de datos** .</span><span class="sxs-lookup"><span data-stu-id="1bb18-129">Under **DefaultConnection** in the **Databases** section, clear the **Update database** check box.</span></span>
5. <span data-ttu-id="1bb18-130">Haga clic en la pestaña **perfil** y, a continuación, haga clic en el perfil de **almacenamiento provisional** en la lista desplegable **perfil** .</span><span class="sxs-lookup"><span data-stu-id="1bb18-130">Click the **Profile** tab, and then click the **Staging** profile in the **Profile** drop-down list.</span></span>
6. <span data-ttu-id="1bb18-131">Cuando se le pregunte si desea guardar los cambios realizados en el perfil de **prueba** , haga clic en **sí**.</span><span class="sxs-lookup"><span data-stu-id="1bb18-131">When you are asked if you want to save the changes made to the **Test** profile, click **Yes**.</span></span>
7. <span data-ttu-id="1bb18-132">Realice el mismo cambio en el perfil de almacenamiento provisional.</span><span class="sxs-lookup"><span data-stu-id="1bb18-132">Make the same change in the Staging profile.</span></span>
8. <span data-ttu-id="1bb18-133">Repita el proceso para hacer el mismo cambio en el perfil de producción.</span><span class="sxs-lookup"><span data-stu-id="1bb18-133">Repeat the process to make the same change in the Production profile.</span></span>
9. <span data-ttu-id="1bb18-134">Cierre el Asistente para **publicación web** .</span><span class="sxs-lookup"><span data-stu-id="1bb18-134">Close the **Publish Web** wizard.</span></span>

<span data-ttu-id="1bb18-135">La implementación en el entorno de prueba es ahora una cuestión sencilla para ejecutar de nuevo la publicación con un solo clic.</span><span class="sxs-lookup"><span data-stu-id="1bb18-135">Deploying to the test environment is now a simple matter of running one-click publish again.</span></span> <span data-ttu-id="1bb18-136">Para que este proceso sea más rápido, puede usar la barra de herramientas de **publicación de un solo clic** .</span><span class="sxs-lookup"><span data-stu-id="1bb18-136">To make this process quicker, you can use the **Web One Click Publish** toolbar.</span></span>

1. <span data-ttu-id="1bb18-137">En el menú **Ver** , elija **barras de herramientas** y, después, **haga clic en publicación web**.</span><span class="sxs-lookup"><span data-stu-id="1bb18-137">In the **View** menu, choose **Toolbars** and then select **Web One Click Publish**.</span></span>

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. <span data-ttu-id="1bb18-139">En **Explorador de soluciones**, seleccione el proyecto ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="1bb18-139">In **Solution Explorer**, select the ContosoUniversity project.</span></span>
3. <span data-ttu-id="1bb18-140">en la barra de herramientas de **publicación de un solo clic** , elija el perfil de publicación de **prueba** y, a continuación, haga clic en **publicar web** (el icono con flechas que apuntan a la izquierda y a la derecha)</span><span class="sxs-lookup"><span data-stu-id="1bb18-140">the **Web One Click Publish** toolbar, choose the **Test** publish profile and then click **Publish Web** (the icon with arrows pointing left and right).</span></span>

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. <span data-ttu-id="1bb18-142">Visual Studio implementa la aplicación actualizada y el explorador se abre automáticamente en la Página principal.</span><span class="sxs-lookup"><span data-stu-id="1bb18-142">Visual Studio deploys the updated application, and the browser automatically opens to the home page.</span></span>
5. <span data-ttu-id="1bb18-143">Ejecute la página Instructors y seleccione un instructor para comprobar que la actualización se ha implementado correctamente.</span><span class="sxs-lookup"><span data-stu-id="1bb18-143">Run the Instructors page and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="1bb18-144">Normalmente también realizaría pruebas de regresión (es decir, probar el resto del sitio para asegurarse de que el nuevo cambio no interrumpió ninguna funcionalidad existente).</span><span class="sxs-lookup"><span data-stu-id="1bb18-144">You would normally also do regression testing (that is, test the rest of the site to make sure that the new change didn't break any existing functionality).</span></span> <span data-ttu-id="1bb18-145">Pero para este tutorial omitirá ese paso y continuará con la implementación de la actualización en el entorno de ensayo y la producción.</span><span class="sxs-lookup"><span data-stu-id="1bb18-145">But for this tutorial you'll skip that step and proceed to deploy the update to staging and production.</span></span>

<span data-ttu-id="1bb18-146">Al volver a implementar, Web Deploy determina automáticamente qué archivos han cambiado y solo copia los archivos modificados en el servidor.</span><span class="sxs-lookup"><span data-stu-id="1bb18-146">When you redeploy, Web Deploy automatically determines which files have changed and only copies changed files to the server.</span></span> <span data-ttu-id="1bb18-147">De forma predeterminada, Web Deploy utiliza fechas de última modificación en los archivos para determinar cuáles han cambiado.</span><span class="sxs-lookup"><span data-stu-id="1bb18-147">By default, Web Deploy uses last-changed dates on files to determine which ones have changed.</span></span> <span data-ttu-id="1bb18-148">Algunos sistemas de control de código fuente cambian las fechas del archivo incluso cuando no se cambia el contenido del archivo.</span><span class="sxs-lookup"><span data-stu-id="1bb18-148">Some source control systems change file dates even when you don't change the file contents.</span></span> <span data-ttu-id="1bb18-149">En ese caso, es posible que desee configurar Web Deploy para usar sumas de comprobación de archivos con el fin de determinar qué archivos han cambiado.</span><span class="sxs-lookup"><span data-stu-id="1bb18-149">In that case, you might want to configure Web Deploy to use file checksums to determine which files have changed.</span></span> <span data-ttu-id="1bb18-150">Para obtener más información, consulte [¿por qué se vuelve a implementar todos los archivos, aunque no los he cambiado?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) en las preguntas más frecuentes sobre la implementación de ASP.net.</span><span class="sxs-lookup"><span data-stu-id="1bb18-150">For more information, see [Why do all of my files get redeployed although I didn't change them?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) in the ASP.NET Deployment FAQ.</span></span>

## <a name="take-the-application-offline-during-deployment"></a><span data-ttu-id="1bb18-151">Poner la aplicación sin conexión durante la implementación</span><span class="sxs-lookup"><span data-stu-id="1bb18-151">Take the application offline during deployment</span></span>

<span data-ttu-id="1bb18-152">El cambio que está implementando ahora es un cambio sencillo en una sola página.</span><span class="sxs-lookup"><span data-stu-id="1bb18-152">The change you're deploying now is a simple change to a single page.</span></span> <span data-ttu-id="1bb18-153">Pero a veces se implementan cambios mayores, o se implementan cambios en el código y en la base de datos, y es posible que el sitio se comporte de forma incorrecta si un usuario solicita una página antes de que finalice la implementación.</span><span class="sxs-lookup"><span data-stu-id="1bb18-153">But sometimes you deploy larger changes, or you deploy both code and database changes, and the site might behave incorrectly if a user requests a page before deployment is finished.</span></span> <span data-ttu-id="1bb18-154">Para evitar que los usuarios accedan al sitio mientras la implementación está en curso, puede usar una *aplicación\_archivo. htm sin conexión* .</span><span class="sxs-lookup"><span data-stu-id="1bb18-154">To prevent users from accessing the site while deployment is in progress, you can use an *app\_offline.htm* file.</span></span> <span data-ttu-id="1bb18-155">Cuando se coloca un archivo con el nombre *app\_offline. htm* en la carpeta raíz de la aplicación, IIS muestra automáticamente ese archivo en lugar de ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1bb18-155">When you put a file named *app\_offline.htm* in the root folder of your application, IIS automatically displays that file instead of running your application.</span></span> <span data-ttu-id="1bb18-156">Por lo tanto, para evitar el acceso durante la implementación, coloque *app\_offline. htm* en la carpeta raíz, ejecute el proceso de implementación y, a continuación, quite *App\_offline. htm* después de la implementación correcta.</span><span class="sxs-lookup"><span data-stu-id="1bb18-156">So to prevent access during deployment, you put *app\_offline.htm* in the root folder, run the deployment process, and then remove *app\_offline.htm* after successful deployment.</span></span>

<span data-ttu-id="1bb18-157">Puede configurar Web Deploy para colocar automáticamente una aplicación predeterminada *\_archivo. htm sin conexión* en el servidor cuando comience a implementarse y quitarla cuando finalice.</span><span class="sxs-lookup"><span data-stu-id="1bb18-157">You can configure Web Deploy to automatically put a default *app\_offline.htm* file on the server when it starts deploying and remove it when it finishes.</span></span> <span data-ttu-id="1bb18-158">Para ello, agregue el siguiente elemento XML al archivo de Perfil de publicación (. pubxml):</span><span class="sxs-lookup"><span data-stu-id="1bb18-158">To do that all you have to do is add the following XML element to your publish profile (.pubxml) file:</span></span>

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

<span data-ttu-id="1bb18-159">En este tutorial, verá cómo crear y usar una *aplicación personalizada\_archivo. htm sin conexión* .</span><span class="sxs-lookup"><span data-stu-id="1bb18-159">For this tutorial you'll see how to create and use a custom *app\_offline.htm* file.</span></span>

<span data-ttu-id="1bb18-160">No es necesario usar *app\_offline. htm* en el sitio de ensayo, ya que no tiene usuarios que accedan al sitio de ensayo.</span><span class="sxs-lookup"><span data-stu-id="1bb18-160">Using *app\_offline.htm* in the staging site isn't required, because you don't have users accessing the staging site.</span></span> <span data-ttu-id="1bb18-161">Pero es recomendable usar el almacenamiento provisional para probar todo el modo en que planea implementarlo en producción.</span><span class="sxs-lookup"><span data-stu-id="1bb18-161">But it's a good practice to use staging to test everything the way you plan to deploy in production.</span></span>

### <a name="create-app_offlinehtm"></a><span data-ttu-id="1bb18-162">Crear aplicación\_sin conexión. htm</span><span class="sxs-lookup"><span data-stu-id="1bb18-162">Create app\_offline.htm</span></span>

1. <span data-ttu-id="1bb18-163">En **Explorador de soluciones**, haga clic con el botón secundario en la solución, haga clic en **Agregar**y, a continuación, haga clic en **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="1bb18-163">In **Solution Explorer**, right-click the solution and click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="1bb18-164">Cree una **página HTML** denominada *App\_offline. htm* (elimine la "l" final en la extensión *. html* que Visual Studio crea de forma predeterminada).</span><span class="sxs-lookup"><span data-stu-id="1bb18-164">Create an **HTML Page** named *app\_offline.htm* (delete the final "l" in the *.html* extension that Visual Studio creates by default).</span></span>
3. <span data-ttu-id="1bb18-165">Reemplace el marcado de la plantilla por el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="1bb18-165">Replace the template markup with the following markup:</span></span>

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. <span data-ttu-id="1bb18-166">Guarde y cierre el archivo.</span><span class="sxs-lookup"><span data-stu-id="1bb18-166">Save and close the file.</span></span>

### <a name="copy-app_offlinehtm-to-the-root-folder-of-the-web-site"></a><span data-ttu-id="1bb18-167">Copie la aplicación\_offline. htm en la carpeta raíz del sitio Web.</span><span class="sxs-lookup"><span data-stu-id="1bb18-167">Copy app\_offline.htm to the root folder of the web site</span></span>

<span data-ttu-id="1bb18-168">Puede usar cualquier herramienta FTP para copiar archivos en el sitio Web.</span><span class="sxs-lookup"><span data-stu-id="1bb18-168">You can use any FTP tool to copy files to the web site.</span></span> <span data-ttu-id="1bb18-169">[FileZilla](http://filezilla-project.org/) es una conocida herramienta de FTP y se muestra en las capturas de pantalla.</span><span class="sxs-lookup"><span data-stu-id="1bb18-169">[FileZilla](http://filezilla-project.org/) is a popular FTP tool and is shown in the screen shots.</span></span>

<span data-ttu-id="1bb18-170">Para usar una herramienta de FTP, necesita tres cosas: la dirección URL de FTP, el nombre de usuario y la contraseña.</span><span class="sxs-lookup"><span data-stu-id="1bb18-170">To use an FTP tool, you need three things: the FTP URL, the user name, and the password.</span></span>

<span data-ttu-id="1bb18-171">La dirección URL se muestra en la página panel del sitio web de Azure Portal de administración, y el nombre de usuario y la contraseña para FTP se pueden encontrar en el archivo *. publishsettings* que descargó anteriormente.</span><span class="sxs-lookup"><span data-stu-id="1bb18-171">The URL is shown on the web site's dashboard page in the Azure Management Portal, and the user name and password for FTP can be found in the *.publishsettings* file that you downloaded earlier.</span></span> <span data-ttu-id="1bb18-172">En los pasos siguientes se muestra cómo obtener estos valores.</span><span class="sxs-lookup"><span data-stu-id="1bb18-172">The following steps show how to get these values.</span></span>

1. <span data-ttu-id="1bb18-173">En el Portal de administración de Azure, haga clic en la pestaña **sitios web** y, a continuación, haga clic en el sitio web de ensayo.</span><span class="sxs-lookup"><span data-stu-id="1bb18-173">In the Azure Management Portal, click **Web Sites** tab and then click the staging web site.</span></span>
2. <span data-ttu-id="1bb18-174">En la página **Panel** , desplácese hacia abajo para buscar el nombre de host FTP en la sección **vista rápida** .</span><span class="sxs-lookup"><span data-stu-id="1bb18-174">On the **Dashboard** page, scroll down to find the FTP host name in the **Quick Glance** section.</span></span>

    ![Nombre de host FTP](deploying-a-code-update/_static/image5.png)
3. <span data-ttu-id="1bb18-176">Abra el archivo staging *. publishsettings* en el Bloc de notas o en otro editor de texto.</span><span class="sxs-lookup"><span data-stu-id="1bb18-176">Open the staging *.publishsettings* file in Notepad or another text editor.</span></span>
4. <span data-ttu-id="1bb18-177">Busque el elemento `publishProfile` del perfil FTP.</span><span class="sxs-lookup"><span data-stu-id="1bb18-177">Find the `publishProfile` element for the FTP profile.</span></span>
5. <span data-ttu-id="1bb18-178">Copie los valores de `userName` y `userPWD`.</span><span class="sxs-lookup"><span data-stu-id="1bb18-178">Copy the `userName` and `userPWD` values.</span></span>

    ![Credenciales de FTP](deploying-a-code-update/_static/image6.png)
6. <span data-ttu-id="1bb18-180">Abra la herramienta FTP e inicie sesión en la dirección URL de FTP.</span><span class="sxs-lookup"><span data-stu-id="1bb18-180">Open your FTP tool and log on to the FTP URL.</span></span>
7. <span data-ttu-id="1bb18-181">Copie *app\_offline. htm* de la carpeta de la solución en la carpeta */site/wwwroot* del sitio de ensayo.</span><span class="sxs-lookup"><span data-stu-id="1bb18-181">Copy *app\_offline.htm* from the solution folder to the */site/wwwroot* folder in the staging site.</span></span>

    ![Copiar app_offline](deploying-a-code-update/_static/image7.png)
8. <span data-ttu-id="1bb18-183">Vaya a la dirección URL del sitio de ensayo.</span><span class="sxs-lookup"><span data-stu-id="1bb18-183">Browse to your staging site's URL.</span></span> <span data-ttu-id="1bb18-184">Verá que la *aplicación\_página offline. htm* aparece ahora en lugar de la Página principal.</span><span class="sxs-lookup"><span data-stu-id="1bb18-184">You see that the *app\_offline.htm* page is now displayed instead of your home page.</span></span>

    ![app_offline. htm en la ventana del explorador](deploying-a-code-update/_static/image8.png)

<span data-ttu-id="1bb18-186">Ahora está listo para realizar la implementación en el almacenamiento provisional.</span><span class="sxs-lookup"><span data-stu-id="1bb18-186">You are now ready to deploy to staging.</span></span>

## <a name="deploy-the-code-update-to-staging-and-production"></a><span data-ttu-id="1bb18-187">Implementación de la actualización del código en ensayo y producción</span><span class="sxs-lookup"><span data-stu-id="1bb18-187">Deploy the code update to staging and production</span></span>

1. <span data-ttu-id="1bb18-188">En la barra de herramientas de **publicación en Web** , elija el perfil de publicación de **ensayo** y, a continuación, haga clic en **publicar web**.</span><span class="sxs-lookup"><span data-stu-id="1bb18-188">In the **Web One Click Publish** toolbar, choose the **Staging** publish profile and then click **Publish Web**.</span></span>

    <span data-ttu-id="1bb18-189">Visual Studio implementa la aplicación actualizada y abre el explorador en la Página principal del sitio.</span><span class="sxs-lookup"><span data-stu-id="1bb18-189">Visual Studio deploys the updated application and opens the browser to the site's home page.</span></span> <span data-ttu-id="1bb18-190">Se muestra la *aplicación\_archivo. htm sin conexión* .</span><span class="sxs-lookup"><span data-stu-id="1bb18-190">The *app\_offline.htm* file is displayed.</span></span> <span data-ttu-id="1bb18-191">Antes de que pueda realizar pruebas para comprobar que la implementación se ha realizado correctamente, debe quitar la *aplicación\_archivo. htm sin conexión* .</span><span class="sxs-lookup"><span data-stu-id="1bb18-191">Before you can test to verify successful deployment, you must remove the *app\_offline.htm* file.</span></span>
2. <span data-ttu-id="1bb18-192">Vuelva a la herramienta FTP y elimine la **aplicación\_offline. htm** del sitio de ensayo.</span><span class="sxs-lookup"><span data-stu-id="1bb18-192">Return to your FTP tool, and delete **app\_offline.htm** from the staging site.</span></span>
3. <span data-ttu-id="1bb18-193">En el explorador, abra la página de instructores en el sitio de ensayo y seleccione un instructor para comprobar que la actualización se ha implementado correctamente.</span><span class="sxs-lookup"><span data-stu-id="1bb18-193">In the browser, open the Instructors page in the staging site, and select an instructor to verify that the update was successfully deployed.</span></span>
4. <span data-ttu-id="1bb18-194">Siga el mismo procedimiento para producción que para el almacenamiento provisional.</span><span class="sxs-lookup"><span data-stu-id="1bb18-194">Follow the same procedure for production as you did for staging.</span></span>

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a><span data-ttu-id="1bb18-195">Revisar los cambios e implementar archivos específicos</span><span class="sxs-lookup"><span data-stu-id="1bb18-195">Reviewing Changes and Deploying Specific Files</span></span>

<span data-ttu-id="1bb18-196">Visual Studio 2012 también ofrece la posibilidad de implementar archivos individuales.</span><span class="sxs-lookup"><span data-stu-id="1bb18-196">Visual Studio 2012 also gives you the ability to deploy individual files.</span></span> <span data-ttu-id="1bb18-197">En el caso de un archivo seleccionado, puede ver las diferencias entre la versión local y la versión implementada, implementar el archivo en el entorno de destino o copiar el archivo del entorno de destino en el proyecto local.</span><span class="sxs-lookup"><span data-stu-id="1bb18-197">For a selected file you can view differences between the local version and the deployed version, deploy the file to the destination environment, or copy the file from the destination environment to the local project.</span></span> <span data-ttu-id="1bb18-198">En esta sección del tutorial, verá cómo usar estas características.</span><span class="sxs-lookup"><span data-stu-id="1bb18-198">In this section of the tutorial you see how to use these features.</span></span>

### <a name="make-a-change-to-deploy"></a><span data-ttu-id="1bb18-199">Realizar un cambio en la implementación</span><span class="sxs-lookup"><span data-stu-id="1bb18-199">Make a change to deploy</span></span>

1. <span data-ttu-id="1bb18-200">Abra *Content/site. CSS*y busque el bloque de la etiqueta `body`.</span><span class="sxs-lookup"><span data-stu-id="1bb18-200">Open *Content/Site.css*, and find the block for the `body` tag.</span></span>
2. <span data-ttu-id="1bb18-201">Cambie el valor de `background-color` de `#fff` a `darkblue`.</span><span class="sxs-lookup"><span data-stu-id="1bb18-201">Change the value for `background-color` from `#fff` to `darkblue`.</span></span>

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a><span data-ttu-id="1bb18-202">Ver el cambio en la ventana de vista previa de publicación</span><span class="sxs-lookup"><span data-stu-id="1bb18-202">View the change in the Publish Preview window</span></span>

<span data-ttu-id="1bb18-203">Al usar el Asistente para **publicación web** para publicar el proyecto, puede ver los cambios que se van a publicar haciendo doble clic en el archivo en la ventana de **vista previa** .</span><span class="sxs-lookup"><span data-stu-id="1bb18-203">When you use the **Publish Web** wizard to publish the project, you can see what changes are going to be published by double-clicking the file in the **Preview** window.</span></span>

1. <span data-ttu-id="1bb18-204">Haga clic con el botón derecho en el proyecto ContosoUniversity y haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="1bb18-204">Right-click the ContosoUniversity project and click **Publish**.</span></span>
2. <span data-ttu-id="1bb18-205">En la lista desplegable **perfil** , seleccione el perfil de publicación de **prueba** .</span><span class="sxs-lookup"><span data-stu-id="1bb18-205">From the **Profile** drop-down list, select the **Test** publish profile.</span></span>
3. <span data-ttu-id="1bb18-206">Haga clic en **vista previa**y, a continuación, en **iniciar vista previa**.</span><span class="sxs-lookup"><span data-stu-id="1bb18-206">Click **Preview**, and then click **Start Preview**.</span></span>
4. <span data-ttu-id="1bb18-207">En el panel de **vista previa** , haga doble clic en **site. CSS**.</span><span class="sxs-lookup"><span data-stu-id="1bb18-207">In the **Preview** pane, double-click **Site.css**.</span></span>

    ![Haga doble clic en site. CSS](deploying-a-code-update/_static/image9.png)

    <span data-ttu-id="1bb18-209">El cuadro de diálogo **vista previa** de los cambios muestra una vista previa de los cambios que se van a implementar.</span><span class="sxs-lookup"><span data-stu-id="1bb18-209">The **Preview changes** dialog shows a preview of the changes that will be deployed.</span></span>

    ![Vista previa de los cambios en site. CSS](deploying-a-code-update/_static/image10.png)

    <span data-ttu-id="1bb18-211">Si hace doble clic en el archivo *Web. config* , el cuadro de diálogo **vista previa de los cambios** muestra el efecto de las transformaciones de configuración de compilación y las transformaciones de Perfil de publicación.</span><span class="sxs-lookup"><span data-stu-id="1bb18-211">If you double-click the *Web.config* file, the **Preview changes** dialog shows the effect of your build configuration transformations and publish profile transformations.</span></span> <span data-ttu-id="1bb18-212">En este momento, no ha hecho nada que haría que el archivo *Web. config* en el servidor cambiara, por lo que espera no ver ningún cambio.</span><span class="sxs-lookup"><span data-stu-id="1bb18-212">At this point you have not done anything that would cause the *Web.config* file on the server to change, so you expect to see no changes.</span></span> <span data-ttu-id="1bb18-213">Sin embargo, la ventana **vista previa de cambios** muestra incorrectamente dos cambios.</span><span class="sxs-lookup"><span data-stu-id="1bb18-213">However, the **Preview changes** window incorrectly shows two changes.</span></span> <span data-ttu-id="1bb18-214">Parece que se quitarán dos elementos XML.</span><span class="sxs-lookup"><span data-stu-id="1bb18-214">It looks like two XML elements will be removed.</span></span> <span data-ttu-id="1bb18-215">Estos elementos se agregan mediante el proceso de publicación cuando se selecciona **ejecutar migraciones de Code First en el inicio** de la aplicación para una clase de contexto Code First.</span><span class="sxs-lookup"><span data-stu-id="1bb18-215">These elements are added by the publish process when you select **Execute Code First Migrations on application start** for a Code First context class.</span></span> <span data-ttu-id="1bb18-216">La comparación se realiza antes de que el proceso de publicación Agregue esos elementos, por lo que parece que se quitan, aunque no se quitarán.</span><span class="sxs-lookup"><span data-stu-id="1bb18-216">The comparison is done before the publish process adds those elements, so it looks like they are being removed although they will not be removed.</span></span> <span data-ttu-id="1bb18-217">Este error se corregirá en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="1bb18-217">This error will be corrected in a future release.</span></span>
5. <span data-ttu-id="1bb18-218">Haga clic en **Cerrar**.</span><span class="sxs-lookup"><span data-stu-id="1bb18-218">Click **Close**.</span></span>
6. <span data-ttu-id="1bb18-219">Haga clic en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="1bb18-219">Click **Publish**.</span></span>
7. <span data-ttu-id="1bb18-220">Cuando el explorador se abra en la Página principal del sitio de prueba, presione CTRL + F5 para que se produzca una actualización de hardware con el fin de ver el efecto del cambio de CSS.</span><span class="sxs-lookup"><span data-stu-id="1bb18-220">When the browser opens to the home page of the Test site, press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Efecto del cambio de CSS](deploying-a-code-update/_static/image11.png)
8. <span data-ttu-id="1bb18-222">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="1bb18-222">Close the browser.</span></span>

### <a name="publish-specific-files-from-solution-explorer"></a><span data-ttu-id="1bb18-223">Publicar archivos específicos desde Explorador de soluciones</span><span class="sxs-lookup"><span data-stu-id="1bb18-223">Publish specific files from Solution Explorer</span></span>

<span data-ttu-id="1bb18-224">Supongamos que no le gusta el fondo azul y desea revertir al color original.</span><span class="sxs-lookup"><span data-stu-id="1bb18-224">Suppose you don't like the blue background and want to revert to the original color.</span></span> <span data-ttu-id="1bb18-225">En esta sección se restaurará la configuración original mediante la publicación de un archivo específico directamente desde **Explorador de soluciones**.</span><span class="sxs-lookup"><span data-stu-id="1bb18-225">In this section you'll restore the original settings by publishing a specific file directly from **Solution Explorer**.</span></span>

1. <span data-ttu-id="1bb18-226">Abra *contenido/sitio. CSS* y restaure el valor de `background-color` en `#fff`.</span><span class="sxs-lookup"><span data-stu-id="1bb18-226">Open *Content/Site.css* and restore the `background-color` setting to `#fff`.</span></span>
2. <span data-ttu-id="1bb18-227">En **Explorador de soluciones**, haga clic con el botón secundario en el archivo *Content/site. CSS* .</span><span class="sxs-lookup"><span data-stu-id="1bb18-227">In **Solution Explorer**, right-click the *Content/Site.css* file.</span></span>

    <span data-ttu-id="1bb18-228">En el menú contextual se muestran tres opciones de publicación.</span><span class="sxs-lookup"><span data-stu-id="1bb18-228">The context menu shows three publish options.</span></span>

    ![Opciones de publicación de Explorador de soluciones](deploying-a-code-update/_static/image12.png)
3. <span data-ttu-id="1bb18-230">Haga clic en **vista previa de cambios en site. CSS**.</span><span class="sxs-lookup"><span data-stu-id="1bb18-230">Click **Preview changes to Site.css**.</span></span>

    <span data-ttu-id="1bb18-231">Se abre una ventana para mostrar las diferencias entre el archivo local y la versión del mismo en el entorno de destino.</span><span class="sxs-lookup"><span data-stu-id="1bb18-231">A window opens to show the differences between the local file and the version of it in the destination environment.</span></span>

    ![Diff-Content/site. CSS](deploying-a-code-update/_static/image13.png)
4. <span data-ttu-id="1bb18-233">En **Explorador de soluciones**, haga clic con el botón derecho en **site. CSS** de nuevo y haga clic en **Publicar sitio. CSS**.</span><span class="sxs-lookup"><span data-stu-id="1bb18-233">In **Solution Explorer**, right-click **Site.css** again and click **Publish Site.css**.</span></span>

    <span data-ttu-id="1bb18-234">La ventana **actividad de publicación web** muestra que el archivo se ha publicado.</span><span class="sxs-lookup"><span data-stu-id="1bb18-234">The **Web Publish Activity** window shows that the file has been published.</span></span>

    ![Ventana actividad de publicación Web](deploying-a-code-update/_static/image14.png)
5. <span data-ttu-id="1bb18-236">Abra un explorador en la dirección URL de `http://localhost/contosouniversity` y, a continuación, presione CTRL + F5 para producir una actualización de hardware con el fin de ver el efecto del cambio de CSS.</span><span class="sxs-lookup"><span data-stu-id="1bb18-236">Open a browser to the `http://localhost/contosouniversity` URL, and then press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Página principal con CSS normal](deploying-a-code-update/_static/image15.png)
6. <span data-ttu-id="1bb18-238">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="1bb18-238">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="1bb18-239">Resumen</span><span class="sxs-lookup"><span data-stu-id="1bb18-239">Summary</span></span>

<span data-ttu-id="1bb18-240">Ahora ha visto varias maneras de implementar una actualización de la aplicación que no implica un cambio en la base de datos y ha visto cómo obtener una vista previa de los cambios para comprobar que lo que se va a actualizar es lo que espera.</span><span class="sxs-lookup"><span data-stu-id="1bb18-240">You've now seen several ways to deploy an application update that does not involve a database change, and you've seen how to preview the changes to verify that what will be updated is what you expect.</span></span> <span data-ttu-id="1bb18-241">La página Instructors tiene ahora una sección de **cursos impartidos** .</span><span class="sxs-lookup"><span data-stu-id="1bb18-241">The Instructors page now has a **Courses Taught** section.</span></span>

![Página de instructores con cursos impartidos](deploying-a-code-update/_static/image16.png)

<span data-ttu-id="1bb18-243">En el siguiente tutorial se muestra cómo implementar un cambio en la base de datos: agregará un campo BIRTHDATE a la base de datos y a la página instructores.</span><span class="sxs-lookup"><span data-stu-id="1bb18-243">The next tutorial shows you how to deploy a database change: you'll add a birthdate field to the database and to the Instructors page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1bb18-244">[Anterior](deploying-to-production.md)
> [Siguiente](deploying-a-database-update.md)</span><span class="sxs-lookup"><span data-stu-id="1bb18-244">[Previous](deploying-to-production.md)
[Next](deploying-a-database-update.md)</span></span>
