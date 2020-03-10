---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Configuración de la solución Contact Manager | Microsoft Docs
author: jrjlee
description: En este tema se describe cómo descargar y configurar la solución Contact Manager para ejecutarse localmente en una estación de trabajo de desarrollador.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: d9774ee01cb0515d7e733b24baa661f2648bd7c4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78511165"
---
# <a name="setting-up-the-contact-manager-solution"></a>Configurar la solución Contact Manager

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo descargar y configurar la solución Contact Manager para ejecutarse localmente en una estación de trabajo de desarrollador.

## <a name="system-requirements"></a>Requisitos del sistema

Para ejecutar la solución Contact Manager localmente y realizar las otras tareas descritas en este tutorial, deberá instalar este software en la estación de trabajo de Desarrollador:

- Visual Studio 2010 Service Pack 1, Premium o Ultimate Edition
- Internet Information Services (IIS) 7.5 Express
- SQL Server Express 2008 R2
- Herramienta de implementación web de IIS (Web Deploy) 2,1 o posterior
- ASP.NET 4,0
- ASP.NET MVC 3
- .NET Framework 4
- .NET Framework 3.5 SP1

A excepción de Visual Studio 2010, puede descargar e instalar las versiones más recientes de todos estos productos y componentes a través del [instalador de plataforma web](https://go.microsoft.com/?linkid=9805118).

## <a name="download-and-extract-the-solution"></a>Descarga y extracción de la solución

Puede descargar la aplicación de ejemplo Contact Manager desde la galería de código de MSDN [aquí](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).

## <a name="configure-and-run-the-solution"></a>Configuración y ejecución de la solución

Para configurar y ejecutar la solución Contact Manager en el equipo local, deberá realizar estos pasos de alto nivel:

1. Si aún no tiene una, cree una base de datos de servicios de aplicación de ASP.NET local con las características de pertenencia y administración de roles habilitadas.
2. Edite las cadenas de conexión en los archivos *Web. config* para que apunten a la instancia de SQL Server Express local.
3. Ejecute la solución desde Visual Studio 2010.

En el resto de esta sección se proporcionan más instrucciones sobre cómo realizar cada una de estas tareas.

**Para crear la base de datos de servicios de aplicación**

1. Abra un símbolo del sistema de Visual Studio 2010. Para ello, en el menú **Inicio** , seleccione **todos los programas**, haga clic en **Microsoft Visual Studio 2010**, haga clic en **Visual Studio Tools**y, a continuación, haga clic en **símbolo del sistema de Visual Studio (2010)** .
2. En el símbolo del sistema, escriba este comando y, a continuación, presione ENTRAR:

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. Use el modificador **– C** para especificar la cadena de conexión para el servidor de base de datos.
    2. Utilice el modificador **– a** para especificar las características de servicios de aplicaciones que desea agregar a la base de datos. En este caso, **m** indica que desea agregar compatibilidad para el proveedor de pertenencia y **r** indica que desea agregar compatibilidad para el administrador de roles.
    3. Use el modificador **-d** para especificar un nombre para la base de datos de servicios de aplicación. Si omite este modificador, la utilidad creará una base de datos con el nombre predeterminado **Aspnetdb**.
3. Cuando la base de datos se haya creado correctamente, el símbolo del sistema mostrará una confirmación.

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> Para obtener más información sobre la utilidad ASPNET\_regsql, vea [ASP.NET SQL Server registration Tool (aspnet\_regsql. exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).

El siguiente paso consiste en asegurarse de que las cadenas de conexión de la solución Contact Manager apuntan a la instancia local de SQL Server Express.

**Para actualizar las cadenas de conexión**

1. Abra la solución Contact Manager en Visual Studio 2010.
2. En la ventana de **Explorador de soluciones** , expanda el proyecto **ContactManager. Mvc** y, a continuación, haga doble clic en el nodo **Web. config** .

    > [!NOTE]
    > El proyecto ContactManager. MVC incluye dos archivos *Web. config* . Debe editar el archivo de nivel de proyecto.

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. En el elemento **connectionStrings** , compruebe que la cadena de conexión denominada **ApplicationServices** apunta a la base de datos local de servicios de aplicación de ASP.net.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. En la ventana de **Explorador de soluciones** , expanda el proyecto **ContactManager. Service** y, a continuación, haga doble clic en el nodo **Web. config** .

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. En el elemento **connectionStrings** , en la cadena de conexión denominada **ContactManagerContext**, compruebe que la propiedad **origen de datos** está establecida en la instancia local de SQL Server Express. No es necesario cambiar nada más en la cadena de conexión.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. Guarde todos los archivos abiertos.

Ahora debería estar listo para ejecutar la solución Contact Manager en el equipo local.

> [!NOTE]
> Si sigue estos pasos sin crear primero una base de datos de servicios de aplicación, ASP.NET creará la base de datos la primera vez que intente crear un usuario. Sin embargo, la creación manual de la base de datos le proporciona mucho más control sobre el conjunto de características de servicios de aplicaciones que desea admitir.

**Para ejecutar la solución Contact Manager**

1. En Visual Studio 2010, presione F5.
2. Internet Explorer se inicia y solicita la dirección URL de la aplicación Contact Manager ASP.NET MVC 3. De forma predeterminada, la aplicación muestra la página **todos los contactos** .

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. Agregue unos cuantos contactos y, a continuación, compruebe que la aplicación funciona según lo previsto.

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. Vaya a `http://localhost:50114/Account/Register` (ajuste la dirección URL si está hospedando la aplicación en un puerto diferente). Agregue un nombre de usuario, una dirección de correo electrónico y una contraseña, y compruebe que es capaz de registrar una cuenta correctamente.

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. Vaya a `http://localhost:50114/Account/LogOn` (ajuste la dirección URL si está hospedando la aplicación en un puerto diferente). Compruebe que puede iniciar sesión con la cuenta que acaba de crear.

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. Cierre Internet Explorer para detener la depuración.

## <a name="conclusion"></a>Conclusión

En este punto, la solución Contact Manager debe estar completamente configurada para ejecutarse en el equipo local. Puede usar la solución como referencia al trabajar en otros temas de este tutorial.

En el tema siguiente, [Descripción del archivo de proyecto](understanding-the-project-file.md), se explica cómo puede usar los archivos de proyecto de Microsoft Build Engine personalizado (MSBuild) dentro de la solución Contact Manager para controlar el proceso de implementación.

> [!div class="step-by-step"]
> [Anterior](the-contact-manager-solution.md)
> [Siguiente](understanding-the-project-file.md)
