---
uid: web-api/overview/security/external-authentication-services
title: Servicios de autenticación externos con ASP.NET Web APIC#() | Microsoft Docs
author: rmcmurray
description: Describe el uso de servicios de autenticación externos en ASP.NET Web API.
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: b2571552a3f8040ff42bfa0a9fa48981f71a1e4b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447421"
---
# <a name="external-authentication-services-with-aspnet-web-api-c"></a><span data-ttu-id="3a793-103">Servicios de autenticación externos con ASP.NET Web APIC#()</span><span class="sxs-lookup"><span data-stu-id="3a793-103">External Authentication Services with ASP.NET Web API (C#)</span></span>

<span data-ttu-id="3a793-104">Visual Studio 2017 y ASP.NET 4.7.2 expanden las opciones de seguridad para [aplicaciones de una sola página](../../../single-page-application/index.md) (Spa) y servicios de [API Web](../../index.md) para la integración con servicios de autenticación externos, entre los que se incluyen varios servicios de autenticación de redes sociales y de OAuth/OpenID: Microsoft Accounts, Twitter, Facebook y Google.</span><span class="sxs-lookup"><span data-stu-id="3a793-104">Visual Studio 2017 and ASP.NET 4.7.2 expand the security options for [Single Page Applications](../../../single-page-application/index.md) (SPA) and [Web API](../../index.md) services to integrate with external authentication services, which include several OAuth/OpenID and social media authentication services: Microsoft Accounts, Twitter, Facebook, and Google.</span></span>  

### <a name="in-this-walkthrough"></a><span data-ttu-id="3a793-105">En este tutorial</span><span class="sxs-lookup"><span data-stu-id="3a793-105">In this Walkthrough</span></span>

- [<span data-ttu-id="3a793-106">Usar servicios de autenticación externos</span><span class="sxs-lookup"><span data-stu-id="3a793-106">Using External Authentication Services</span></span>](#USING)
- [<span data-ttu-id="3a793-107">Crear la aplicación Web de ejemplo</span><span class="sxs-lookup"><span data-stu-id="3a793-107">Creating the Sample Web Application</span></span>](#SAMPLE)
- [<span data-ttu-id="3a793-108">Habilitación de la autenticación de Facebook</span><span class="sxs-lookup"><span data-stu-id="3a793-108">Enabling Facebook Authentication</span></span>](#FACEBOOK)
- [<span data-ttu-id="3a793-109">Habilitar la autenticación de Google</span><span class="sxs-lookup"><span data-stu-id="3a793-109">Enabling Google Authentication</span></span>](#GOOGLE)
- [<span data-ttu-id="3a793-110">Habilitar la autenticación de Microsoft</span><span class="sxs-lookup"><span data-stu-id="3a793-110">Enabling Microsoft Authentication</span></span>](#MICROSOFT)
- [<span data-ttu-id="3a793-111">Habilitación de la autenticación de Twitter</span><span class="sxs-lookup"><span data-stu-id="3a793-111">Enabling Twitter Authentication</span></span>](#TWITTER)
- [<span data-ttu-id="3a793-112">Información adicional</span><span class="sxs-lookup"><span data-stu-id="3a793-112">Additional Information</span></span>](#MOREINFO)

    - [<span data-ttu-id="3a793-113">Combinación de servicios de autenticación externos</span><span class="sxs-lookup"><span data-stu-id="3a793-113">Combining External Authentication Services</span></span>](#COMBINE)
    - [<span data-ttu-id="3a793-114">Configuración de IIS Express para utilizar un nombre de dominio completo</span><span class="sxs-lookup"><span data-stu-id="3a793-114">Configuring IIS Express to use a Fully Qualified Domain Name</span></span>](#FQDN)
    - [<span data-ttu-id="3a793-115">Cómo obtener la configuración de la aplicación para la autenticación de Microsoft</span><span class="sxs-lookup"><span data-stu-id="3a793-115">How to Obtain your Application Settings for Microsoft Authentication</span></span>](#OBTAIN)
    - [<span data-ttu-id="3a793-116">Opcional: deshabilitar el registro local</span><span class="sxs-lookup"><span data-stu-id="3a793-116">Optional: Disable Local Registration</span></span>](#DISABLE)

### <a name="prerequisites"></a><span data-ttu-id="3a793-117">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="3a793-117">Prerequisites</span></span>

<span data-ttu-id="3a793-118">Para seguir los ejemplos de este tutorial, debe disponer de lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="3a793-118">To follow the examples in this walkthrough, you need to have the following:</span></span>

- <span data-ttu-id="3a793-119">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="3a793-119">Visual Studio 2017</span></span>
- <span data-ttu-id="3a793-120">Una cuenta de desarrollador con el identificador de la aplicación y la clave secreta para uno de los siguientes servicios de autenticación de medios sociales:</span><span class="sxs-lookup"><span data-stu-id="3a793-120">A developer account with the application identifier and secret key for one of the following social media authentication services:</span></span>

  - <span data-ttu-id="3a793-121">Cuentas de Microsoft ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))</span><span class="sxs-lookup"><span data-stu-id="3a793-121">Microsoft Accounts ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))</span></span>
  - <span data-ttu-id="3a793-122">Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))</span><span class="sxs-lookup"><span data-stu-id="3a793-122">Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))</span></span>
  - <span data-ttu-id="3a793-123">Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))</span><span class="sxs-lookup"><span data-stu-id="3a793-123">Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))</span></span>
  - <span data-ttu-id="3a793-124">Google ([https://developers.google.com/](https://developers.google.com))</span><span class="sxs-lookup"><span data-stu-id="3a793-124">Google ([https://developers.google.com/](https://developers.google.com))</span></span>

<a id="USING"></a>
## <a name="using-external-authentication-services"></a><span data-ttu-id="3a793-125">Usar servicios de autenticación externos</span><span class="sxs-lookup"><span data-stu-id="3a793-125">Using External Authentication Services</span></span>

<span data-ttu-id="3a793-126">La abundancia de servicios de autenticación externos que actualmente están disponibles para los desarrolladores web ayuda a reducir el tiempo de desarrollo al crear nuevas aplicaciones Web.</span><span class="sxs-lookup"><span data-stu-id="3a793-126">The abundance of external authentication services that are currently available to web developers help to reduce development time when creating new web applications.</span></span> <span data-ttu-id="3a793-127">Los usuarios Web suelen tener varias cuentas existentes para los sitios web de servicios web y medios sociales más conocidos, por lo que cuando una aplicación web implementa los servicios de autenticación desde un servicio Web externo o un sitio web de redes sociales, ahorra el tiempo de desarrollo que se habría dedicado a crear una implementación de autenticación.</span><span class="sxs-lookup"><span data-stu-id="3a793-127">Web users typically have several existing accounts for popular web services and social media websites, therefore when a web application implements the authentication services from an external web service or social media website, it saves the development time that would have been spent creating an authentication implementation.</span></span> <span data-ttu-id="3a793-128">El uso de un servicio de autenticación externo evita que los usuarios finales tengan que crear otra cuenta para la aplicación web y que también no tengan que recordar otro nombre de usuario y contraseña.</span><span class="sxs-lookup"><span data-stu-id="3a793-128">Using an external authentication service saves end users from having to create another account for your web application, and also from having to remember another username and password.</span></span>

<span data-ttu-id="3a793-129">En el pasado, los desarrolladores tenían dos opciones: crear su propia implementación de autenticación u obtener información sobre cómo integrar un servicio de autenticación externo en sus aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="3a793-129">In the past, developers have had two choices: create their own authentication implementation, or learn how to integrate an external authentication service into their applications.</span></span> <span data-ttu-id="3a793-130">En el nivel más básico, el siguiente diagrama muestra un flujo de solicitud simple para un agente de usuario (explorador Web) que solicita información de una aplicación web que está configurada para usar un servicio de autenticación externo:</span><span class="sxs-lookup"><span data-stu-id="3a793-130">At the most basic level, the following diagram illustrates a simple request flow for a user agent (web browser) that is requesting information from a web application that is configured to use an external authentication service:</span></span>

[![](external-authentication-services/_static/image2.png "Click to Expand the Image")](external-authentication-services/_static/image1.png)

<span data-ttu-id="3a793-131">En el diagrama anterior, el agente de usuario (o el explorador Web en este ejemplo) realiza una solicitud a una aplicación Web, que redirige el explorador Web a un servicio de autenticación externo.</span><span class="sxs-lookup"><span data-stu-id="3a793-131">In the preceding diagram, the user agent (or web browser in this example) makes a request to a web application, which redirects the web browser to an external authentication service.</span></span> <span data-ttu-id="3a793-132">El agente de usuario envía sus credenciales al servicio de autenticación externo y, si el agente de usuario se ha autenticado correctamente, el servicio de autenticación externo redirigirá al agente de usuario a la aplicación web original con algún tipo de token que el el agente de usuario enviará a la aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="3a793-132">The user agent sends its credentials to the external authentication service, and if the user agent has successfully authenticated, the external authentication service will redirect the user agent to the original web application with some form of token which the user agent will send to the web application.</span></span> <span data-ttu-id="3a793-133">La aplicación web usará el token para comprobar que el agente de usuario se ha autenticado correctamente mediante el servicio de autenticación externo, y la aplicación web puede usar el token para recopilar más información sobre el agente de usuario.</span><span class="sxs-lookup"><span data-stu-id="3a793-133">The web application will use the token to verify that the user agent has been successfully authenticated by the external authentication service, and the web application may use the token to gather more information about the user agent.</span></span> <span data-ttu-id="3a793-134">Una vez que la aplicación termina de procesar la información del agente de usuario, la aplicación web devolverá la respuesta adecuada al agente de usuario en función de su configuración de autorización.</span><span class="sxs-lookup"><span data-stu-id="3a793-134">Once the application is done processing the user agent's information, the web application will return the appropriate response to the user agent based on its authorization settings.</span></span>

<span data-ttu-id="3a793-135">En este segundo ejemplo, el agente de usuario negocia con la aplicación web y el servidor de autorización externo, y la aplicación web realiza una comunicación adicional con el servidor de autorización externo para recuperar información adicional sobre el usuario. directivas</span><span class="sxs-lookup"><span data-stu-id="3a793-135">In this second example, the user agent negotiates with the web application and external authorization server, and the web application performs additional communication with the external authorization server to retrieve additional information about the user agent:</span></span>

[![](external-authentication-services/_static/image4.png "Click to Expand the Image")](external-authentication-services/_static/image3.png)

<span data-ttu-id="3a793-136">Visual Studio 2017 y ASP.NET 4.7.2 facilitan la integración con servicios de autenticación externos para los desarrolladores, ya que proporcionan una integración integrada para los siguientes servicios de autenticación:</span><span class="sxs-lookup"><span data-stu-id="3a793-136">Visual Studio 2017 and ASP.NET 4.7.2 make integration with external authentication services easier for developers by providing built-in integration for the following authentication services:</span></span>

- <span data-ttu-id="3a793-137">Facebook</span><span class="sxs-lookup"><span data-stu-id="3a793-137">Facebook</span></span>
- <span data-ttu-id="3a793-138">Google</span><span class="sxs-lookup"><span data-stu-id="3a793-138">Google</span></span>
- <span data-ttu-id="3a793-139">Cuentas de Microsoft (cuentas de Windows Live ID)</span><span class="sxs-lookup"><span data-stu-id="3a793-139">Microsoft Accounts (Windows Live ID accounts)</span></span>
- <span data-ttu-id="3a793-140">Twitter</span><span class="sxs-lookup"><span data-stu-id="3a793-140">Twitter</span></span>

<span data-ttu-id="3a793-141">En los ejemplos de este tutorial se muestra cómo configurar cada uno de los servicios de autenticación externa admitidos mediante la nueva plantilla de aplicación Web de ASP.NET que se incluye con Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="3a793-141">The examples in this walkthrough will demonstrate how to configure each of the supported external authentication services by using the new ASP.NET Web Application template that ships with Visual Studio 2017.</span></span>

> [!NOTE]
> <span data-ttu-id="3a793-142">Si es necesario, puede que tenga que agregar su FQDN a la configuración del servicio de autenticación externo.</span><span class="sxs-lookup"><span data-stu-id="3a793-142">If necessary, you may need to add your FQDN to the settings for your external authentication service.</span></span> <span data-ttu-id="3a793-143">Este requisito se basa en las restricciones de seguridad de algunos servicios de autenticación externos que requieren que el FQDN de la configuración de la aplicación coincida con el FQDN que usan los clientes.</span><span class="sxs-lookup"><span data-stu-id="3a793-143">This requirement is based on security constraints for some external authentication services which require the FQDN in your application settings to match the FQDN that is used by your clients.</span></span> <span data-ttu-id="3a793-144">(Los pasos para ello variarán en gran medida para cada servicio de autenticación externo; tendrá que consultar la documentación de cada servicio de autenticación externo para ver si es necesario y cómo configurar estos valores). Si necesita configurar IIS Express para usar un FQDN para probar este entorno, vea la sección [configuración de IIS Express para usar un nombre de dominio completo](#FQDN) más adelante en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="3a793-144">(The steps for this will vary greatly for each external authentication service; you will need to consult the documentation for each external authentication service to see if this is required and how to configure these settings.) If you need to configure IIS Express to use an FQDN for testing this environment, see the [Configuring IIS Express to use a Fully Qualified Domain Name](#FQDN) section later in this walkthrough.</span></span>

<a id="SAMPLE"></a>
## <a name="create-a-sample-web-application"></a><span data-ttu-id="3a793-145">Crear una aplicación Web de ejemplo</span><span class="sxs-lookup"><span data-stu-id="3a793-145">Create a Sample Web Application</span></span>

<span data-ttu-id="3a793-146">Los pasos siguientes le guiarán en el proceso de creación de una aplicación de ejemplo con la plantilla de aplicación Web ASP.NET y la utilizará para cada uno de los servicios de autenticación externos más adelante en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="3a793-146">The following steps will lead you through creating a sample application by using the ASP.NET Web Application template, and you will use this sample application for each of the external authentication services later in this walkthrough.</span></span>

<span data-ttu-id="3a793-147">Inicie Visual Studio 2017 y seleccione **nuevo proyecto** en la página de inicio.</span><span class="sxs-lookup"><span data-stu-id="3a793-147">Start Visual Studio 2017 and select **New Project** from the Start page.</span></span> <span data-ttu-id="3a793-148">O bien, en el menú **archivo** , seleccione **nuevo** y luego **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="3a793-148">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<!-- [![](external-authentication-services/_static/image6.png "Click to Expand the Image")](external-authentication-services/_static/image5.png) -->

<span data-ttu-id="3a793-149">Cuando se muestre el cuadro de diálogo **nuevo proyecto** , seleccione **instalado** y expanda **Visual C#** .</span><span class="sxs-lookup"><span data-stu-id="3a793-149">When the **New Project** dialog box is displayed, select **Installed** and expand **Visual C#**.</span></span> <span data-ttu-id="3a793-150">En **Visual C#** , seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="3a793-150">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="3a793-151">En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.net (.NET Framework)** .</span><span class="sxs-lookup"><span data-stu-id="3a793-151">In the list of project templates, select **ASP.NET Web Application (.Net Framework)**.</span></span> <span data-ttu-id="3a793-152">Escriba un nombre para el proyecto y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="3a793-152">Enter a name for your project and click **OK**.</span></span>

[![](external-authentication-services/_static/image71.png "Click to Expand the Image")](external-authentication-services/_static/image71.png)

<span data-ttu-id="3a793-153">Cuando se muestre el **nuevo proyecto ASP.net** , seleccione la plantilla **aplicación de una sola página** y haga clic en **crear proyecto**.</span><span class="sxs-lookup"><span data-stu-id="3a793-153">When the **New ASP.NET Project** is displayed, select the **Single Page Application** template and click **Create Project**.</span></span>

[![](external-authentication-services/_static/image72.png "Click to Expand the Image")](external-authentication-services/_static/image72.png)

<span data-ttu-id="3a793-154">Espere mientras Visual Studio 2017 crea el proyecto.</span><span class="sxs-lookup"><span data-stu-id="3a793-154">Wait as Visual Studio 2017 creates your project.</span></span>

<!-- [![](external-authentication-services/_static/image12.png "Click to Expand the Image")](external-authentication-services/_static/image11.png) -->

<span data-ttu-id="3a793-155">Cuando Visual Studio 2017 haya terminado de crear el proyecto, abra el archivo *Startup.auth.CS* que se encuentra en la **aplicación\_** carpeta de inicio.</span><span class="sxs-lookup"><span data-stu-id="3a793-155">When Visual Studio 2017 has finished creating your project, open the *Startup.Auth.cs* file that is located in the **App\_Start** folder.</span></span>

<span data-ttu-id="3a793-156">La primera vez que se crea el proyecto, ninguno de los servicios de autenticación externos está habilitado en el archivo *Startup.auth.CS* ; a continuación se muestra el aspecto que podría tener el código, con las secciones resaltadas para donde habilitaría un servicio de autenticación externo y cualquier configuración relevante para usar cuentas Microsoft, Twitter, Facebook o la autenticación de Google con la aplicación ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="3a793-156">When you first create the project, none of the external authentication services are enabled in *Startup.Auth.cs* file; the following illustrates what your code might resemble, with the sections highlighted for where you would enable an external authentication service and any relevant settings in order to use Microsoft Accounts, Twitter, Facebook, or Google authentication with your ASP.NET application:</span></span>

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

<span data-ttu-id="3a793-157">Al presionar F5 para compilar y depurar la aplicación Web, se mostrará una pantalla de inicio de sesión donde verá que no se ha definido ningún servicio de autenticación externo.</span><span class="sxs-lookup"><span data-stu-id="3a793-157">When you press F5 to build and debug your web application, it will display a login screen where you will see that no external authentication services have been defined.</span></span>

[![](external-authentication-services/_static/image73.png "Click to Expand the Image")](external-authentication-services/_static/image73.png)

<span data-ttu-id="3a793-158">En las secciones siguientes, obtendrá información sobre cómo habilitar cada uno de los servicios de autenticación externos que se proporcionan con ASP.NET en Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="3a793-158">In the following sections, you will learn how to enable each of the external authentication services that are provided with ASP.NET in Visual Studio 2017.</span></span>

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a><span data-ttu-id="3a793-159">Habilitación de la autenticación de Facebook</span><span class="sxs-lookup"><span data-stu-id="3a793-159">Enabling Facebook authentication</span></span>

<span data-ttu-id="3a793-160">El uso de la autenticación de Facebook requiere la creación de una cuenta de desarrollador de Facebook y el proyecto requerirá un identificador de aplicación y una clave secreta de Facebook para funcionar.</span><span class="sxs-lookup"><span data-stu-id="3a793-160">Using Facebook authentication requires you to create a Facebook developer account, and your project will require an application ID and secret key from Facebook in order to function.</span></span> <span data-ttu-id="3a793-161">Para obtener información sobre cómo crear una cuenta de desarrollador de Facebook y obtener el identificador de la aplicación y la clave secreta, consulte [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span><span class="sxs-lookup"><span data-stu-id="3a793-161">For information about creating a Facebook developer account and obtaining your application ID and secret key, see [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span></span>

<span data-ttu-id="3a793-162">Una vez que haya obtenido el identificador y la clave secreta de la aplicación, siga estos pasos para habilitar la autenticación de Facebook para la aplicación web:</span><span class="sxs-lookup"><span data-stu-id="3a793-162">Once you have obtained your application ID and secret key, use the following steps to enable Facebook authentication for your web application:</span></span>

1. <span data-ttu-id="3a793-163">Cuando el proyecto esté abierto en Visual Studio 2017, abra el archivo *Startup.auth.CS* .</span><span class="sxs-lookup"><span data-stu-id="3a793-163">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="3a793-164">Busque la sección de autenticación de Facebook en el código:</span><span class="sxs-lookup"><span data-stu-id="3a793-164">Locate the Facebook authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. <span data-ttu-id="3a793-165">Quite el &quot;//&quot; caracteres para quitar las marcas de comentario de las líneas de código resaltadas y, a continuación, agregue el identificador de aplicación y la clave secreta.</span><span class="sxs-lookup"><span data-stu-id="3a793-165">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your application ID and secret key.</span></span> <span data-ttu-id="3a793-166">Una vez que haya agregado esos parámetros, puede volver a compilar el proyecto:</span><span class="sxs-lookup"><span data-stu-id="3a793-166">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. <span data-ttu-id="3a793-167">Al presionar F5 para abrir la aplicación web en el explorador Web, verá que Facebook se ha definido como un servicio de autenticación externo:</span><span class="sxs-lookup"><span data-stu-id="3a793-167">When you press F5 to open your web application in your web browser, you will see that Facebook has been defined as an external authentication service:</span></span>

    [![](external-authentication-services/_static/image74.png "Click to Expand the Image")](external-authentication-services/_static/image74.png)
5. <span data-ttu-id="3a793-168">Al hacer clic en el botón de **Facebook** , el explorador se redirigirá a la página de inicio de sesión de Facebook:</span><span class="sxs-lookup"><span data-stu-id="3a793-168">When you click the **Facebook** button, your browser will be redirected to the Facebook login page:</span></span>

    [![](external-authentication-services/_static/image22.png "Click to Expand the Image")](external-authentication-services/_static/image21.png)
6. <span data-ttu-id="3a793-169">Después de escribir sus credenciales de Facebook y hacer clic en **iniciar sesión**, el explorador Web se redirigirá de nuevo a la aplicación Web, lo que le pedirá el **nombre de usuario** que desea asociar a su cuenta de Facebook:</span><span class="sxs-lookup"><span data-stu-id="3a793-169">After you enter your Facebook credentials and click **Log in**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Facebook account:</span></span>

    [![](external-authentication-services/_static/image24.png "Click to Expand the Image")](external-authentication-services/_static/image23.png)
7. <span data-ttu-id="3a793-170">Después de escribir el nombre de usuario y hacer clic en el botón de **registro** , la aplicación web mostrará la **Página principal** predeterminada de la cuenta de Facebook:</span><span class="sxs-lookup"><span data-stu-id="3a793-170">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Facebook account:</span></span>

    [![](external-authentication-services/_static/image26.png "Click to Expand the Image")](external-authentication-services/_static/image25.png)

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a><span data-ttu-id="3a793-171">Habilitar la autenticación de Google</span><span class="sxs-lookup"><span data-stu-id="3a793-171">Enabling Google Authentication</span></span>

<span data-ttu-id="3a793-172">El uso de la autenticación de Google requiere la creación de una cuenta de desarrollador de Google, y el proyecto requerirá un identificador de aplicación y una clave secreta de Google para poder funcionar.</span><span class="sxs-lookup"><span data-stu-id="3a793-172">Using Google authentication requires you to create a Google developer account, and your project will require an application ID and secret key from Google in order to function.</span></span> <span data-ttu-id="3a793-173">Para obtener información sobre cómo crear una cuenta de desarrollador de Google y obtener el identificador de la aplicación y la clave secreta, consulte [https://developers.google.com](https://developers.google.com).</span><span class="sxs-lookup"><span data-stu-id="3a793-173">For information about creating a Google developer account and obtaining your application ID and secret key, see [https://developers.google.com](https://developers.google.com).</span></span>

<span data-ttu-id="3a793-174">Para habilitar la autenticación de Google para la aplicación Web, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="3a793-174">To enable Google authentication for your web application, use the following steps:</span></span>

1. <span data-ttu-id="3a793-175">Cuando el proyecto esté abierto en Visual Studio 2017, abra el archivo *Startup.auth.CS* .</span><span class="sxs-lookup"><span data-stu-id="3a793-175">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="3a793-176">Busque la sección de código de la autenticación de Google:</span><span class="sxs-lookup"><span data-stu-id="3a793-176">Locate the Google authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. <span data-ttu-id="3a793-177">Quite el &quot;//&quot; caracteres para quitar las marcas de comentario de las líneas de código resaltadas y, a continuación, agregue el identificador de aplicación y la clave secreta.</span><span class="sxs-lookup"><span data-stu-id="3a793-177">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your application ID and secret key.</span></span> <span data-ttu-id="3a793-178">Una vez que haya agregado esos parámetros, puede volver a compilar el proyecto:</span><span class="sxs-lookup"><span data-stu-id="3a793-178">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. <span data-ttu-id="3a793-179">Al presionar F5 para abrir la aplicación web en el explorador Web, verá que Google se ha definido como un servicio de autenticación externo:</span><span class="sxs-lookup"><span data-stu-id="3a793-179">When you press F5 to open your web application in your web browser, you will see that Google has been defined as an external authentication service:</span></span>

    [![](external-authentication-services/_static/image75.png "Click to Expand the Image")](external-authentication-services/_static/image75.png)
5. <span data-ttu-id="3a793-180">Al hacer clic en el botón de **Google** , el explorador se redirigirá a la página de inicio de sesión de Google:</span><span class="sxs-lookup"><span data-stu-id="3a793-180">When you click the **Google** button, your browser will be redirected to the Google login page:</span></span>

    [![](external-authentication-services/_static/image32.png "Click to Expand the Image")](external-authentication-services/_static/image31.png)
6. <span data-ttu-id="3a793-181">Después de escribir sus credenciales de Google y hacer clic en **iniciar sesión**, Google le pedirá que compruebe que la aplicación web tiene permisos para acceder a su cuenta de Google:</span><span class="sxs-lookup"><span data-stu-id="3a793-181">After you enter your Google credentials and click **Sign in**, Google will prompt you to verify that your web application has permissions to access your Google account:</span></span>

    [![](external-authentication-services/_static/image34.png "Click to Expand the Image")](external-authentication-services/_static/image33.png)
7. <span data-ttu-id="3a793-182">Al hacer clic en **Aceptar**, el explorador Web se redirigirá de nuevo a la aplicación Web, lo que le pedirá el **nombre de usuario** que desea asociar a su cuenta de Google:</span><span class="sxs-lookup"><span data-stu-id="3a793-182">When you click **Accept**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Google account:</span></span>

    [![](external-authentication-services/_static/image36.png "Click to Expand the Image")](external-authentication-services/_static/image35.png)
8. <span data-ttu-id="3a793-183">Después de escribir el nombre de usuario y hacer clic en el botón de **registro** , la aplicación web mostrará la **Página principal** predeterminada de la cuenta de Google:</span><span class="sxs-lookup"><span data-stu-id="3a793-183">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Google account:</span></span>

    [![](external-authentication-services/_static/image38.png "Click to Expand the Image")](external-authentication-services/_static/image37.png)

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a><span data-ttu-id="3a793-184">Habilitar la autenticación de Microsoft</span><span class="sxs-lookup"><span data-stu-id="3a793-184">Enabling Microsoft Authentication</span></span>

<span data-ttu-id="3a793-185">La autenticación de Microsoft requiere la creación de una cuenta de desarrollador y requiere un identificador de cliente y un secreto de cliente para poder funcionar.</span><span class="sxs-lookup"><span data-stu-id="3a793-185">Microsoft authentication requires you to create a developer account, and it requires a client ID and client secret in order to function.</span></span> <span data-ttu-id="3a793-186">Para obtener información sobre cómo crear una cuenta de desarrollador de Microsoft y obtener el identificador de cliente y el secreto de cliente, consulte [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070).</span><span class="sxs-lookup"><span data-stu-id="3a793-186">For information about creating a Microsoft developer account and obtaining your client ID and client secret, see [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070).</span></span>

<span data-ttu-id="3a793-187">Una vez que haya obtenido la clave de consumidor y el secreto de consumidor, siga estos pasos para habilitar la autenticación de Microsoft para su aplicación web:</span><span class="sxs-lookup"><span data-stu-id="3a793-187">Once you have obtained your consumer key and consumer secret, use the following steps to enable Microsoft authentication for your web application:</span></span>

1. <span data-ttu-id="3a793-188">Cuando el proyecto esté abierto en Visual Studio 2017, abra el archivo *Startup.auth.CS* .</span><span class="sxs-lookup"><span data-stu-id="3a793-188">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="3a793-189">Busque la sección de código de autenticación de Microsoft:</span><span class="sxs-lookup"><span data-stu-id="3a793-189">Locate the Microsoft authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. <span data-ttu-id="3a793-190">Quite el &quot;//&quot; caracteres para quitar las marcas de comentario de las líneas de código resaltadas y, a continuación, agregue el identificador de cliente y el secreto de cliente.</span><span class="sxs-lookup"><span data-stu-id="3a793-190">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your client ID and client secret.</span></span> <span data-ttu-id="3a793-191">Una vez que haya agregado esos parámetros, puede volver a compilar el proyecto:</span><span class="sxs-lookup"><span data-stu-id="3a793-191">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. <span data-ttu-id="3a793-192">Al presionar F5 para abrir la aplicación web en el explorador Web, verá que Microsoft se ha definido como un servicio de autenticación externo:</span><span class="sxs-lookup"><span data-stu-id="3a793-192">When you press F5 to open your web application in your web browser, you will see that Microsoft has been defined as an external authentication service:</span></span>

    [![](external-authentication-services/_static/image42.png "Click to Expand the Image")](external-authentication-services/_static/image41.png)
5. <span data-ttu-id="3a793-193">Al hacer clic en el botón **Microsoft** , el explorador se redirigirá a la página de inicio de sesión de Microsoft:</span><span class="sxs-lookup"><span data-stu-id="3a793-193">When you click the **Microsoft** button, your browser will be redirected to the Microsoft login page:</span></span>

    [![](external-authentication-services/_static/image44.png "Click to Expand the Image")](external-authentication-services/_static/image43.png)
6. <span data-ttu-id="3a793-194">Después de escribir sus credenciales de Microsoft y hacer clic en **iniciar sesión**, se le pedirá que compruebe que la aplicación web tiene permisos de acceso a la cuenta de Microsoft:</span><span class="sxs-lookup"><span data-stu-id="3a793-194">After you enter your Microsoft credentials and click **Sign in**, you will be prompted to verify that your web application has permissions to access your Microsoft account:</span></span>

    [![](external-authentication-services/_static/image46.png "Click to Expand the Image")](external-authentication-services/_static/image45.png)
7. <span data-ttu-id="3a793-195">Al hacer clic en **sí**, el explorador Web se redirigirá de nuevo a la aplicación Web, lo que le pedirá el **nombre de usuario** que desea asociar a su cuenta de Microsoft:</span><span class="sxs-lookup"><span data-stu-id="3a793-195">When you click **Yes**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Microsoft account:</span></span>

    [![](external-authentication-services/_static/image48.png "Click to Expand the Image")](external-authentication-services/_static/image47.png)
8. <span data-ttu-id="3a793-196">Después de escribir el nombre de usuario y hacer clic en el botón de **registro** , la aplicación web mostrará la **Página principal** predeterminada del cuenta de Microsoft:</span><span class="sxs-lookup"><span data-stu-id="3a793-196">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Microsoft account:</span></span>

    [![](external-authentication-services/_static/image50.png "Click to Expand the Image")](external-authentication-services/_static/image49.png)

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a><span data-ttu-id="3a793-197">Habilitación de la autenticación de Twitter</span><span class="sxs-lookup"><span data-stu-id="3a793-197">Enabling Twitter Authentication</span></span>

<span data-ttu-id="3a793-198">La autenticación de Twitter requiere la creación de una cuenta de desarrollador y requiere una clave de consumidor y un secreto de consumidor para poder funcionar.</span><span class="sxs-lookup"><span data-stu-id="3a793-198">Twitter authentication requires you to create a developer account, and it requires a consumer key and consumer secret in order to function.</span></span> <span data-ttu-id="3a793-199">Para obtener información sobre cómo crear una cuenta de desarrollador de Twitter y obtener la clave de consumidor y el secreto de consumidor, consulte [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span><span class="sxs-lookup"><span data-stu-id="3a793-199">For information about creating a Twitter developer account and obtaining your consumer key and consumer secret, see [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span></span>

<span data-ttu-id="3a793-200">Una vez que haya obtenido la clave de consumidor y el secreto de consumidor, siga estos pasos para habilitar la autenticación de Twitter para la aplicación web:</span><span class="sxs-lookup"><span data-stu-id="3a793-200">Once you have obtained your consumer key and consumer secret, use the following steps to enable Twitter authentication for your web application:</span></span>

1. <span data-ttu-id="3a793-201">Cuando el proyecto esté abierto en Visual Studio 2017, abra el archivo *Startup.auth.CS* .</span><span class="sxs-lookup"><span data-stu-id="3a793-201">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="3a793-202">Busque la sección autenticación de Twitter del código:</span><span class="sxs-lookup"><span data-stu-id="3a793-202">Locate the Twitter authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. <span data-ttu-id="3a793-203">Quite el &quot;//&quot; caracteres para quitar la marca de comentario de las líneas de código resaltadas y, a continuación, agregue la clave de consumidor y el secreto de consumidor.</span><span class="sxs-lookup"><span data-stu-id="3a793-203">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your consumer key and consumer secret.</span></span> <span data-ttu-id="3a793-204">Una vez que haya agregado esos parámetros, puede volver a compilar el proyecto:</span><span class="sxs-lookup"><span data-stu-id="3a793-204">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. <span data-ttu-id="3a793-205">Al presionar F5 para abrir la aplicación web en el explorador Web, verá que Twitter se ha definido como un servicio de autenticación externo:</span><span class="sxs-lookup"><span data-stu-id="3a793-205">When you press F5 to open your web application in your web browser, you will see that Twitter has been defined as an external authentication service:</span></span>

    [![](external-authentication-services/_static/image54.png "Click to Expand the Image")](external-authentication-services/_static/image53.png)
5. <span data-ttu-id="3a793-206">Al hacer clic en el botón **Twitter** , el explorador se redirigirá a la página de inicio de sesión de Twitter:</span><span class="sxs-lookup"><span data-stu-id="3a793-206">When you click the **Twitter** button, your browser will be redirected to the Twitter login page:</span></span>

    [![](external-authentication-services/_static/image56.png "Click to Expand the Image")](external-authentication-services/_static/image55.png)
6. <span data-ttu-id="3a793-207">Después de escribir sus credenciales de Twitter y hacer clic en **autorizar aplicación**, el explorador Web se redirigirá de nuevo a la aplicación Web, lo que le pedirá el **nombre de usuario** que quiere asociar a su cuenta de Twitter:</span><span class="sxs-lookup"><span data-stu-id="3a793-207">After you enter your Twitter credentials and click **Authorize app**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Twitter account:</span></span>

    [![](external-authentication-services/_static/image58.png "Click to Expand the Image")](external-authentication-services/_static/image57.png)
7. <span data-ttu-id="3a793-208">Después de escribir el nombre de usuario y hacer clic en el botón de **registro** , la aplicación web mostrará la **Página principal** predeterminada de la cuenta de Twitter:</span><span class="sxs-lookup"><span data-stu-id="3a793-208">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Twitter account:</span></span>

    [![](external-authentication-services/_static/image60.png "Click to Expand the Image")](external-authentication-services/_static/image59.png)

<a id="MOREINFO"></a>
## <a name="additional-information"></a><span data-ttu-id="3a793-209">Información adicional</span><span class="sxs-lookup"><span data-stu-id="3a793-209">Additional Information</span></span>

<span data-ttu-id="3a793-210">Para obtener información adicional sobre la creación de aplicaciones que usan OAuth y OpenID, consulte las direcciones URL siguientes:</span><span class="sxs-lookup"><span data-stu-id="3a793-210">For additional information about creating applications that use OAuth and OpenID, see the following URLs:</span></span>

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a><span data-ttu-id="3a793-211">Combinación de servicios de autenticación externos</span><span class="sxs-lookup"><span data-stu-id="3a793-211">Combining External Authentication Services</span></span>

<span data-ttu-id="3a793-212">Para mayor flexibilidad, puede definir varios servicios de autenticación externos al mismo tiempo, lo que permite a los usuarios de la aplicación web usar una cuenta de cualquiera de los servicios de autenticación externa habilitados:</span><span class="sxs-lookup"><span data-stu-id="3a793-212">For greater flexibility, you can define multiple external authentication services at the same time - this allows your web application's users to use an account from any of the enabled external authentication services:</span></span>

[![](external-authentication-services/_static/image62.png "Click to Expand the Image")](external-authentication-services/_static/image61.png)

<a id="FQDN"></a>
### <a name="configure-iis-express-to-use-a-fully-qualified-domain-name"></a><span data-ttu-id="3a793-213">Configurar IIS Express para usar un nombre de dominio completo</span><span class="sxs-lookup"><span data-stu-id="3a793-213">Configure IIS Express to use a fully qualified domain name</span></span>

<span data-ttu-id="3a793-214">Algunos proveedores de autenticación externos no admiten la prueba de la aplicación mediante una dirección HTTP como `http://localhost:port/`.</span><span class="sxs-lookup"><span data-stu-id="3a793-214">Some external authentication providers do not support testing your application by using an HTTP address like `http://localhost:port/`.</span></span> <span data-ttu-id="3a793-215">Para solucionar este problema, puede Agregar una asignación de nombre de dominio completo (FQDN) estática al archivo de hosts y configurar las opciones del proyecto en Visual Studio 2017 para usar el FQDN para pruebas o depuración.</span><span class="sxs-lookup"><span data-stu-id="3a793-215">To work around this issue, you can add a static Fully Qualified Domain Name (FQDN) mapping to your HOSTS file and configure your project options in Visual Studio 2017 to use the FQDN for testing/debugging.</span></span> <span data-ttu-id="3a793-216">Para ello, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="3a793-216">To do so, use the following steps:</span></span>

- <span data-ttu-id="3a793-217">Agregue un FQDN estático que asigne el archivo de hosts:</span><span class="sxs-lookup"><span data-stu-id="3a793-217">Add a static FQDN mapping your HOSTS file:</span></span>

  1. <span data-ttu-id="3a793-218">Abra un símbolo del sistema con privilegios elevados en Windows.</span><span class="sxs-lookup"><span data-stu-id="3a793-218">Open an elevated command prompt in Windows.</span></span>
  2. <span data-ttu-id="3a793-219">Escriba el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="3a793-219">Type the following command:</span></span>

      <span data-ttu-id="3a793-220"><kbd>Bloc de notas%WinDir%\system32\drivers\etc\hosts</kbd></span><span class="sxs-lookup"><span data-stu-id="3a793-220"><kbd>notepad %WinDir%\system32\drivers\etc\hosts</kbd></span></span>
  3. <span data-ttu-id="3a793-221">Agregue una entrada como la siguiente al archivo de hosts:</span><span class="sxs-lookup"><span data-stu-id="3a793-221">Add an entry like the following to the HOSTS file:</span></span>

      <span data-ttu-id="3a793-222"><kbd>127.0.0.1 www.wingtiptoys.com</kbd></span><span class="sxs-lookup"><span data-stu-id="3a793-222"><kbd>127.0.0.1 www.wingtiptoys.com</kbd></span></span>
  4. <span data-ttu-id="3a793-223">Guarde y cierre el archivo de hosts.</span><span class="sxs-lookup"><span data-stu-id="3a793-223">Save and close your HOSTS file.</span></span>

- <span data-ttu-id="3a793-224">Configure el proyecto de Visual Studio para usar el FQDN:</span><span class="sxs-lookup"><span data-stu-id="3a793-224">Configure your Visual Studio project to use the FQDN:</span></span>

  1. <span data-ttu-id="3a793-225">Cuando el proyecto esté abierto en Visual Studio 2017, haga clic en el menú **proyecto** y, a continuación, seleccione las propiedades del proyecto.</span><span class="sxs-lookup"><span data-stu-id="3a793-225">When your project is open in Visual Studio 2017, click the **Project** menu, and then select your project's properties.</span></span> <span data-ttu-id="3a793-226">Por ejemplo, puede seleccionar **propiedades de WebApplication1**.</span><span class="sxs-lookup"><span data-stu-id="3a793-226">For example, you might select **WebApplication1 Properties**.</span></span>
  2. <span data-ttu-id="3a793-227">Seleccione la pestaña **Web** .</span><span class="sxs-lookup"><span data-stu-id="3a793-227">Select the **Web** tab.</span></span>
  3. <span data-ttu-id="3a793-228">Escriba el FQDN de la <strong>dirección URL del proyecto</strong>.</span><span class="sxs-lookup"><span data-stu-id="3a793-228">Enter your FQDN for the <strong>Project Url</strong>.</span></span> <span data-ttu-id="3a793-229">Por ejemplo, debe especificar <kbd><http://www.wingtiptoys.com></kbd> si se trata de la asignación de FQDN que ha agregado al archivo de hosts.</span><span class="sxs-lookup"><span data-stu-id="3a793-229">For example, you would enter <kbd><http://www.wingtiptoys.com></kbd> if that was the FQDN mapping that you added to your HOSTS file.</span></span>

- <span data-ttu-id="3a793-230">Configure IIS Express para usar el FQDN de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="3a793-230">Configure IIS Express to use the FQDN for your application:</span></span>

    1. <span data-ttu-id="3a793-231">Abra un símbolo del sistema con privilegios elevados en Windows.</span><span class="sxs-lookup"><span data-stu-id="3a793-231">Open an elevated command prompt in Windows.</span></span>
    2. <span data-ttu-id="3a793-232">Escriba el siguiente comando para cambiar a la carpeta IIS Express:</span><span class="sxs-lookup"><span data-stu-id="3a793-232">Type the following command to change to your IIS Express folder:</span></span>

        <span data-ttu-id="3a793-233"><kbd>CD/d &quot;%ProgramFiles%\IIS Express&quot;</kbd></span><span class="sxs-lookup"><span data-stu-id="3a793-233"><kbd>cd /d &quot;%ProgramFiles%\IIS Express&quot;</kbd></span></span>
    3. <span data-ttu-id="3a793-234">Escriba el siguiente comando para agregar el FQDN a la aplicación:</span><span class="sxs-lookup"><span data-stu-id="3a793-234">Type the following command to add the FQDN to your application:</span></span>

        <span data-ttu-id="3a793-235"><kbd>appcmd. exe set config-Section: System. applicationHost/sites/+&quot;[name = ' WebApplication1 ']. bindings. [Protocol = ' http ', bindingInformation = ' \*: 80: www. wingtiptoys. com ']&quot;/commit: apphost</kbd></span><span class="sxs-lookup"><span data-stu-id="3a793-235"><kbd>appcmd.exe set config -section:system.applicationHost/sites /+&quot;[name='WebApplication1'].bindings.[protocol='http',bindingInformation='\*:80:www.wingtiptoys.com']&quot; /commit:apphost</kbd></span></span>

  <span data-ttu-id="3a793-236">Donde **WebApplication1** es el nombre del proyecto y **bindingInformation** contiene el número de puerto y el FQDN que desea usar para las pruebas.</span><span class="sxs-lookup"><span data-stu-id="3a793-236">Where **WebApplication1** is the name of your project and **bindingInformation** contains the port number and FQDN that you want to use for your testing.</span></span>

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a><span data-ttu-id="3a793-237">Cómo obtener la configuración de la aplicación para la autenticación de Microsoft</span><span class="sxs-lookup"><span data-stu-id="3a793-237">How to Obtain your Application Settings for Microsoft Authentication</span></span>

<span data-ttu-id="3a793-238">La vinculación de una aplicación a Windows Live para la autenticación de Microsoft es un proceso sencillo.</span><span class="sxs-lookup"><span data-stu-id="3a793-238">Linking an application to Windows Live for Microsoft Authentication is a simple process.</span></span> <span data-ttu-id="3a793-239">Si aún no ha vinculado una aplicación a Windows Live, puede seguir estos pasos:</span><span class="sxs-lookup"><span data-stu-id="3a793-239">If you have not already linked an application to Windows Live, you can use the following steps:</span></span>

1. <span data-ttu-id="3a793-240">Vaya a [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070) y escriba el nombre de cuenta de Microsoft y la contraseña cuando se le solicite y, a continuación, haga clic en **iniciar sesión**:</span><span class="sxs-lookup"><span data-stu-id="3a793-240">Browse to [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070) and enter your Microsoft account name and password when prompted, then click **Sign in**:</span></span>

   <!--  [![](external-authentication-services/_static/image64.png "Click to Expand the Image")](external-authentication-services/_static/image63.png) -->
2. <span data-ttu-id="3a793-241">Seleccione **Agregar una aplicación** , escriba el nombre de la aplicación cuando se le solicite y, a continuación, haga clic en **crear**:</span><span class="sxs-lookup"><span data-stu-id="3a793-241">Select **Add an app** and enter the name of your application when prompted, and then click **Create**:</span></span>

    [![](external-authentication-services/_static/image79.png "Click to Expand the Image")](external-authentication-services/_static/image79.png)
3. <span data-ttu-id="3a793-242">Seleccione la aplicación en **nombre** y aparece la página Propiedades de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3a793-242">Select your app under **Name** and its application properties page appears.</span></span>

4. <span data-ttu-id="3a793-243">Escriba el dominio de redirección de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3a793-243">Enter the redirect domain for your application.</span></span> <span data-ttu-id="3a793-244">Copie el **identificador** de la aplicación y, en **secretos de aplicación**, seleccione **generar contraseña**.</span><span class="sxs-lookup"><span data-stu-id="3a793-244">Copy the **Application ID** and, under **Application Secrets**, select **Generate Password**.</span></span> <span data-ttu-id="3a793-245">Copie la contraseña que aparece.</span><span class="sxs-lookup"><span data-stu-id="3a793-245">Copy the password that appears.</span></span> <span data-ttu-id="3a793-246">El identificador y la contraseña de la aplicación son el identificador de cliente y el secreto de cliente.</span><span class="sxs-lookup"><span data-stu-id="3a793-246">The application ID and password are your client ID and client secret.</span></span> <span data-ttu-id="3a793-247">Seleccione **Aceptar** y, a continuación, **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="3a793-247">Select **Ok** and then **Save**.</span></span>

    [![](external-authentication-services/_static/image77.png "Click to Expand the Image")](external-authentication-services/_static/image77.png)

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a><span data-ttu-id="3a793-248">Opcional: deshabilitar el registro local</span><span class="sxs-lookup"><span data-stu-id="3a793-248">Optional: Disable Local Registration</span></span>

<span data-ttu-id="3a793-249">La funcionalidad de registro local de ASP.NET actual no impide que los programas automáticos (bots) creen cuentas de miembro; por ejemplo, mediante el uso de una tecnología de prevención y validación de bot, como [CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md).</span><span class="sxs-lookup"><span data-stu-id="3a793-249">The current ASP.NET local registration functionality does not prevent automated programs (bots) from creating member accounts; for example, by using a bot-prevention and validation technology like [CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md).</span></span> <span data-ttu-id="3a793-250">Por ello, debe quitar el formulario de inicio de sesión local y el vínculo de registro en la página de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="3a793-250">Because of this, you should remove the local login form and registration link on the login page.</span></span> <span data-ttu-id="3a793-251">Para ello, abra la página *\_login. cshtml* del proyecto y, a continuación, comente las líneas del panel de inicio de sesión local y el vínculo de registro.</span><span class="sxs-lookup"><span data-stu-id="3a793-251">To do so, open the *\_Login.cshtml* page in your project, and then comment out the lines for the local login panel and the registration link.</span></span> <span data-ttu-id="3a793-252">La página resultante debe ser similar a la del ejemplo de código siguiente:</span><span class="sxs-lookup"><span data-stu-id="3a793-252">The resulting page should look like the following code sample:</span></span>

[!code-html[Main](external-authentication-services/samples/sample10.html)]

<span data-ttu-id="3a793-253">Una vez deshabilitado el panel de inicio de sesión local y el vínculo de registro, la página de inicio de sesión solo mostrará los proveedores de autenticación externos que ha habilitado:</span><span class="sxs-lookup"><span data-stu-id="3a793-253">Once the local login panel and the registration link have been disabled, your login page will only display the external authentication providers that you have enabled:</span></span>

[![](external-authentication-services/_static/image70.png "Click to Expand the Image")](external-authentication-services/_static/image69.png)
