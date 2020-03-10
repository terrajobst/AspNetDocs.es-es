---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Uso de Web API 2 con Entity Framework 6 | Microsoft Docs
author: MikeWasson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web con un ASP.NET Web API back-end. En el tutorial se usa Entity Framework 6 para los datos...
ms.author: riande
ms.date: 01/17/2019
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 0f5dc960f494af5bd4ce87863a510d1892319908
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504841"
---
# <a name="using-web-api-2-with-entity-framework-6"></a>Usar Web API 2 con Entity Framework 6

[Descargar proyecto completado](https://github.com/MikeWasson/BookService)

> Este tutorial le enseña los aspectos básicos de la creación de una aplicación web con un back-end ASP.NET Web API. En el tutorial se usa Entity Framework 6 para la capa de datos y Knockout. js para la aplicación JavaScript del lado cliente. En el tutorial también se muestra cómo implementar la aplicación en Azure App Service Web Apps.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
>
> - API Web 2,1
> - Visual Studio 2017 (Descargue Visual Studio 2017 [aquí](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - Entity Framework 6
> - .NET 4.7
> - [Knockout. js](http://knockoutjs.com/) 3,1

En este tutorial se usa ASP.NET Web API 2 con Entity Framework 6 para crear una aplicación web que manipula una base de datos back-end. Esta es una captura de pantalla de la aplicación que creará.

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

La aplicación usa un diseño de aplicación de una sola página (SPA). "Aplicación de una sola página" es el término general para una aplicación web que carga una sola página HTML y, a continuación, actualiza la página dinámicamente, en lugar de cargar nuevas páginas. Después de la carga de página inicial, la aplicación se comunica con el servidor a través de solicitudes AJAX. Las solicitudes AJAX devuelven datos JSON, que la aplicación usa para actualizar la interfaz de usuario.

AJAX no es nuevo, pero hoy en día hay marcos de JavaScript que facilitan la compilación y el mantenimiento de una aplicación de SPA sofisticada. En este tutorial se usa [Knockout. js](http://knockoutjs.com/), pero puede usar cualquier marco de cliente de JavaScript.

Estos son los principales bloques de creación para esta aplicación:

- ASP.NET MVC crea la página HTML.
- ASP.NET Web API controla las solicitudes AJAX y devuelve datos JSON.
- Datos de Knockout. js: enlaza los elementos HTML a los datos JSON.
- Entity Framework se comunica con la base de datos.

## <a name="see-this-app-running-on-azure"></a>Vea esta aplicación en ejecución en Azure

¿Desea ver el sitio terminado que se ejecuta como una aplicación Web activa? Puede implementar una versión completa de la aplicación en su cuenta de Azure; para ello, seleccione el botón siguiente.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

Necesita una cuenta de Azure para implementar esta solución en Azure. Si aún no tiene una cuenta, tiene las siguientes opciones:

- [Abra una cuenta de Azure gratis](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) : obtiene créditos que puede usar para probar los servicios de Azure de pago y, incluso después de que se agoten, puede mantener la cuenta y usar los servicios gratuitos de Azure.
- [Activar las ventajas de suscriptor de MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) : su suscripción a MSDN le proporciona créditos todos los meses que puede usar para los servicios de Azure de pago.

## <a name="create-the-project"></a>Crear el proyecto

Abra Visual Studio. En el menú **archivo** , seleccione **nuevo**y seleccione **proyecto**. (O seleccione **nuevo proyecto** en la página de inicio).

En el cuadro de diálogo **nuevo proyecto** , seleccione **Web** en el panel izquierdo y **ASP.NET Web Application (.NET Framework)** en el panel central. Asigne al proyecto el nombre **BookService** y seleccione **Aceptar**.

[![](part-1/_static/image11.png)](part-1/_static/image11.png)

En el cuadro de diálogo **nuevo proyecto ASP.net** , seleccione la plantilla **API Web** .

[![](part-1/_static/image12.png)](part-1/_static/image12.png)

Seleccione **Aceptar** para crear el proyecto.

## <a name="configure-azure-settings-optional"></a>Configuración de Azure (opcional)

Después de crear el proyecto, puede optar por implementarlo en Azure App Service Web Apps en cualquier momento. 

1. En Explorador de soluciones, haga clic con el botón derecho en el proyecto y seleccione **publicar**.

2. En la ventana que aparece, seleccione **iniciar**. Aparece el cuadro de diálogo **seleccionar un destino de publicación** .

   [![](part-1/_static/image14.png)](part-1/_static/image14.png)

3. Seleccione **Crear perfil**. Aparecerá el cuadro de diálogo **Crear servicio de aplicaciones**.

   [![](part-1/_static/image15.png)](part-1/_static/image15.png)

   Acepte los valores predeterminados o escriba valores diferentes para el nombre de la aplicación, el grupo de recursos, el plan de hospedaje, la suscripción de Azure y la región geográfica. 

4. Seleccione **crear una base de datos SQL**. Aparece el cuadro de diálogo **configurar SQL Server** . 

   [![](part-1/_static/image16.png)](part-1/_static/image16.png)

   Acepte los valores predeterminados o escriba valores distintos. Escriba un **nombre de usuario de administrador** y una **contraseña de administrador** para la nueva base de datos. Seleccione **Aceptar** cuando haya terminado. Vuelve a aparecer la página **crear App Service** .

5. Seleccione **crear** para crear el perfil. Aparece un mensaje en la esquina inferior derecha que indica que la implementación está en curso. Tras unos instantes, la ventana de **publicación** vuelve a aparecer.

    [![](part-1/_static/image17.png)](part-1/_static/image17.png)
   
    El perfil que ha creado para implementar la aplicación ya está disponible. 

> [!div class="step-by-step"]
> [Siguiente](part-2.md)
