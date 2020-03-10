---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET acceso denegado a directorios IIS | Microsoft Docs
author: rick-anderson
description: En estas notas del producto se describe lo que debe hacer si una solicitud a la aplicación ASP.NET devuelve el error "denegado el acceso al directorio DirectoryName. No se pudo...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: a3a53aa88abbe1bcaaea7d691406800c8f9b988b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78518575"
---
# <a name="aspnet-denied-access-to-iis-directories"></a><span data-ttu-id="bde67-104">Acceso denegado de ASP.NET a los directorios de IIS</span><span class="sxs-lookup"><span data-stu-id="bde67-104">ASP.NET Denied Access to IIS Directories</span></span>

> <span data-ttu-id="bde67-105">En estas notas del producto se describe lo que debe hacer si una solicitud a la aplicación ASP.NET devuelve el error "denegado el acceso al directorio *directoryname* .</span><span class="sxs-lookup"><span data-stu-id="bde67-105">This whitepaper describes what you must do if a request to your ASP.NET application returns the error, "Denied Access to *DirectoryName* directory.</span></span> <span data-ttu-id="bde67-106">No se pudo iniciar la supervisión de los cambios de directorio. "</span><span class="sxs-lookup"><span data-stu-id="bde67-106">Failed to start monitoring directory changes."</span></span>
> 
> <span data-ttu-id="bde67-107">Se aplica a ASP.NET 1,0 y ASP.NET 1,1.</span><span class="sxs-lookup"><span data-stu-id="bde67-107">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>

<span data-ttu-id="bde67-108">ASP.NET v1 RTM ahora se ejecuta con una cuenta de Windows con menos privilegios registrada como la cuenta "ASPNET" en un equipo local.</span><span class="sxs-lookup"><span data-stu-id="bde67-108">ASP.NET V1 RTM now runs using a less privileged windows account - registered as the "ASPNET" account on a local machine.</span></span>

<span data-ttu-id="bde67-109">En algunos sistemas bloqueados, es posible que esta cuenta no tenga de forma predeterminada acceso de lectura a los directorios de contenido de un sitio web, el directorio raíz de la aplicación o el directorio raíz del sitio Web.</span><span class="sxs-lookup"><span data-stu-id="bde67-109">On some locked down systems, this account may not by default have read security access to a website's content directories, the application root directory, or the web site root directory.</span></span> <span data-ttu-id="bde67-110">En este caso, recibirá el siguiente error al solicitar páginas de una aplicación web determinada:</span><span class="sxs-lookup"><span data-stu-id="bde67-110">In this case you will receive the following error when requesting pages from a given web application:</span></span>

![](denied-access-to-iis-directories/_static/image1.jpg)

<span data-ttu-id="bde67-111">Para solucionarlo, deberá cambiar los permisos de seguridad en los directorios correspondientes.</span><span class="sxs-lookup"><span data-stu-id="bde67-111">To fix this you will need to change the security permissions on the appropriate directories.</span></span>

<span data-ttu-id="bde67-112">En concreto, ASP.NET requiere acceso de lectura, ejecución y lista para la cuenta de ASPNET para la raíz del sitio web (por ejemplo: c:\Inetpub\wwwroot o cualquier directorio de sitio alternativo que haya configurado en IIS), el directorio de contenido y el directorio raíz de la aplicación. para supervisar los cambios del archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="bde67-112">Specifically, ASP.NET requires read, execute, and list access for the ASPNET account for the web site root (for example: c:\inetpub\wwwroot or any alternative site directory you may have configured in IIS), the content directory and the application root directory in order to monitor for configuration file changes.</span></span> <span data-ttu-id="bde67-113">La raíz de la aplicación corresponde a la ruta de acceso de la carpeta asociada al directorio virtual de la aplicación en la herramienta de administración de IIS (inetmgr).</span><span class="sxs-lookup"><span data-stu-id="bde67-113">The application root corresponds to the folder path associated with the application virtual directory in the IIS Administration tool (inetmgr).</span></span>

<span data-ttu-id="bde67-114">Por ejemplo, considere la siguiente jerarquía de aplicación en la carpeta wwwroot.</span><span class="sxs-lookup"><span data-stu-id="bde67-114">For example, consider the following application hierarchy under the wwwroot folder.</span></span>

`C:\inetpub\wwwroot\myapp\default.aspx`

<span data-ttu-id="bde67-115">En este ejemplo, la cuenta ASPNET necesita los permisos de lectura definidos anteriormente para el contenido en el directorio MyApp y wwwroot.</span><span class="sxs-lookup"><span data-stu-id="bde67-115">For this example, the ASPNET account needs the read permissions defined above for content in both the myapp and the wwwroot directory.</span></span> <span data-ttu-id="bde67-116">También se puede usar una sola ACL heredada en la carpeta raíz para ambos directorios, si están anidados.</span><span class="sxs-lookup"><span data-stu-id="bde67-116">A single inherited ACL on the root folder can also be optionally used for both directories if they're nested.</span></span>

<span data-ttu-id="bde67-117">Para agregar permisos a un directorio, realice los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="bde67-117">To add permissions to a directory, perform the following steps:</span></span>

- <span data-ttu-id="bde67-118">Mediante el explorador de Windows, navegue hasta el directorio</span><span class="sxs-lookup"><span data-stu-id="bde67-118">Using the Windows explorer, navigate to the directory</span></span>
- <span data-ttu-id="bde67-119">Haga clic con el botón derecho en la carpeta del directorio y elija "propiedades".</span><span class="sxs-lookup"><span data-stu-id="bde67-119">Right click on the directory folder and choose "Properties"</span></span>
- <span data-ttu-id="bde67-120">Vaya a la pestaña "seguridad" en el cuadro de diálogo de propiedades.</span><span class="sxs-lookup"><span data-stu-id="bde67-120">Navigate to the "Security" tab on the property dialog</span></span>
- <span data-ttu-id="bde67-121">Haga clic en el botón "agregar" y escriba el nombre de la máquina seguido del nombre de la cuenta ASPNET.</span><span class="sxs-lookup"><span data-stu-id="bde67-121">Click the "Add" button and enter the machine name followed by the ASPNET account name.</span></span> <span data-ttu-id="bde67-122">Por ejemplo, en un equipo denominado "WebDev", escribiría webdev\ASPNET y aceptaría "OK".</span><span class="sxs-lookup"><span data-stu-id="bde67-122">For example, on a machine named "webdev", you would enter webdev\ASPNET and hit "OK".</span></span>
- <span data-ttu-id="bde67-123">Asegúrese de que la cuenta de ASPNET tiene activadas las casillas de verificación "leer &amp; ejecutar", "Mostrar el contenido de la carpeta" y "leer".</span><span class="sxs-lookup"><span data-stu-id="bde67-123">Ensure that the ASPNET account has the "Read &amp; Execute", "List Folder Contents", and "Read" checkboxes checked.</span></span>
- <span data-ttu-id="bde67-124">Presione Aceptar para descartar el cuadro de diálogo y guardar los cambios.</span><span class="sxs-lookup"><span data-stu-id="bde67-124">Hit OK to dismiss the dialog and save the changes.</span></span>

![](denied-access-to-iis-directories/_static/image2.jpg)

<span data-ttu-id="bde67-125">Si lo desea, estos cambios se pueden automatizar mediante scripts o la herramienta "cacls. exe" que se incluye con Windows.</span><span class="sxs-lookup"><span data-stu-id="bde67-125">If desired, these changes can be automated using scripts or the "cacls.exe" tool that ships with Windows.</span></span> <span data-ttu-id="bde67-126">Para obtener más información sobre la cuenta de ASPNET, consulte el [documento de preguntas más frecuentes](https://go.microsoft.com/fwlink/?LinkId=5828).</span><span class="sxs-lookup"><span data-stu-id="bde67-126">For more information on the ASPNET account, please see the [FAQ document](https://go.microsoft.com/fwlink/?LinkId=5828).</span></span>

<span data-ttu-id="bde67-127">Si una aplicación web determinada se basa en tener permisos de escritura o modificación en una carpeta o un archivo determinados, esto se puede conceder siguiendo el mismo procedimiento y comprobando las casillas "escribir" y/o "modificar".</span><span class="sxs-lookup"><span data-stu-id="bde67-127">If a given web application relies on having write or modify permissions to a particular folder or file, this can be granted by following the same procedure and checking the "Write" and/or "Modify" checkboxes.</span></span>

<span data-ttu-id="bde67-128">En los equipos que permiten a todos los usuarios o al grupo de usuarios tener acceso de lectura a estos directorios (que es la configuración predeterminada), no se producirán problemas y no se requerirán los pasos anteriores.</span><span class="sxs-lookup"><span data-stu-id="bde67-128">On machines that allow Everyone or the Users group read access to these directories (which is the default configuration), no issues will be encountered and the above steps will not be required.</span></span>
