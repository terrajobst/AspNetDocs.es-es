---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Publicar la aplicación en Azure Azure App Service | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: a9a7b74a07c71b47253c0af304c7a26ffa4eaae5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504763"
---
# <a name="publish-the-app-to-azure-azure-app-service"></a>Publicación de la aplicación en Azure Azure App Service

por [Mike Wasson](https://github.com/MikeWasson)

[Descargar proyecto completado](https://github.com/MikeWasson/BookService)

Como último paso, publicará la aplicación en Azure. En Explorador de soluciones, haga clic con el botón derecho en el proyecto y seleccione **publicar**.

![](part-10/_static/image1.png)

Al hacer clic en **publicar** se invoca el cuadro de diálogo **publicación web** . Si seleccionó **hospedar en la nube** cuando creó el proyecto por primera vez, la conexión y la configuración ya están configuradas. En ese caso, solo tiene que hacer clic en la pestaña **configuración** y comprobar &quot;ejecutar migraciones de Code First&quot;. (Si no seleccionó **host en la nube** al principio, siga los pasos descritos en la [sección siguiente](#new-website)).

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

Para implementar la aplicación, haga clic en **publicar**. Puede ver el progreso de la publicación en la ventana **actividad de publicación web** . (En el menú **Ver** , seleccione **otras ventanas**y, a continuación, seleccione **actividad de publicación web**).

![](part-10/_static/image4.png)

Cuando Visual Studio finaliza la implementación de la aplicación, el explorador predeterminado se abre automáticamente en la dirección URL del sitio Web implementado y la aplicación que ha creado se ejecuta ahora en la nube. La dirección URL de la barra de direcciones del explorador muestra que el sitio se está cargando desde Internet.

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a>Implementación en un sitio web nuevo

Si no ha activado **host en la nube** cuando creó el proyecto por primera vez, puede configurar ahora una nueva aplicación Web. En Explorador de soluciones, haga clic con el botón derecho en el proyecto y seleccione **publicar**. Seleccione la pestaña **perfil** y haga clic en **Microsoft Azure websites**. Si no ha iniciado sesión actualmente en Azure, se le pedirá que inicie sesión.

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

En el cuadro de diálogo **sitios Web existentes** , haga clic en **nuevo**.

![](part-10/_static/image9.png)

Escriba un nombre de sitio. Seleccione su suscripción de Azure y la región. En **servidor de base de datos**, seleccione **crear nuevo servidor**o seleccione un servidor existente. Haga clic en **Crear**.

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

Haga clic en la pestaña **configuración** y compruebe &quot;ejecutar migraciones de Code First&quot;. A continuación, haga clic en **Publicar**.

> [!div class="step-by-step"]
> [Anterior](part-9.md)
