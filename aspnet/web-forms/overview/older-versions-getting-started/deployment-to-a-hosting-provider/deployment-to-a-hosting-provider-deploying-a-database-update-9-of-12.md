---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 'Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementar una base de datos Update-9 de 12 | Microsoft Docs'
author: tdykstra
description: En esta serie de tutoriales se muestra cómo implementar (publicar) un proyecto de aplicación Web de ASP.NET que incluye una base de datos de SQL Server Compact mediante Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 3385e1019d9e7a9263fd9513c39b25eb85febbb5
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74582020"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a>Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementar una base de datos Update-9 de 12

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> En esta serie de tutoriales se muestra cómo implementar (publicar) un proyecto de aplicación Web ASP.NET que incluye una base de datos SQL Server Compact mediante Visual Studio 2012 RC o Visual Studio Express 2012 RC para Web. También puede usar Visual Studio 2010 si instala la actualización de publicación Web. Para obtener una introducción a la serie, vea [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver un tutorial que muestra las características de implementación que se introdujeron después de la versión RC de Visual Studio 2012, muestra cómo implementar SQL Server ediciones distintas de SQL Server Compact y muestra cómo realizar la implementación en Azure App Service Web Apps, vea [implementación web de ASP.net con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Información general del

En este tutorial, realizará un cambio en la base de datos y los cambios en el código relacionado, probará los cambios en Visual Studio y, a continuación, implementará la actualización en los entornos de prueba y producción.

Aviso: Si recibe un mensaje de error o algo no funciona a medida que avanza en el tutorial, asegúrese de consultar la [Página de solución de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Agregar una nueva columna a una tabla

En esta sección, agregará una columna de fecha de nacimiento a la clase base `Person` para las entidades `Student` y `Instructor`. A continuación, actualice la página que muestra los datos del instructor para que muestren la nueva columna.

En el proyecto *ContosoUniversity. Dal* , Abra *Person.CS* y agregue la siguiente propiedad al final de la clase `Person` (debe haber dos llaves de cierre después):

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

A continuación, actualice el método de inicialización para que proporcione un valor para la nueva columna. Abra *Migrations\Configuration.CS* y reemplace el bloque de código que comienza `var instructors = new List<Instructor>` por el siguiente bloque de código que incluye la información de fecha de nacimiento:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

En el proyecto ContosoUniversity, Abra *instructors. aspx* y agregue un nuevo campo plantilla para mostrar la fecha de nacimiento. Agréguelo entre los de la fecha de contratación y la asignación de la oficina:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

(Si la sangría de código no está sincronizada, puede presionar CTRL-K y, a continuación, CTRL + D para cambiar el formato del archivo automáticamente).

Compile la solución y, a continuación, abra la ventana **consola del administrador de paquetes** . Asegúrese de que ContosoUniversity. DAL sigue seleccionado como **proyecto predeterminado**.

En la ventana de la **consola del administrador de paquetes** , seleccione **ContosoUniversity. Dal** como **proyecto predeterminado**y, a continuación, escriba el siguiente comando:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

Cuando finaliza este comando, Visual Studio abre el archivo de clase que define la nueva clase de `DbMigration` y, en el método `Up`, puede ver el código que crea la nueva columna.

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

Compile la solución y, a continuación, escriba el siguiente comando en la ventana de la **consola del administrador de paquetes** (Asegúrese de que el proyecto CONTOSOUNIVERSITY. Dal todavía está seleccionado):

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

Cuando finalice el comando, ejecute la aplicación y seleccione la página instructors. Cuando se cargue la página, verá que tiene el nuevo campo de fecha de nacimiento.

[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Implementar la actualización de la base de datos en el entorno de prueba

En **Explorador de soluciones** Seleccione el proyecto ContosoUniversity.

En la barra de herramientas de **publicación de un solo clic** , seleccione el perfil de publicación de **prueba** y, a continuación, haga clic en **publicar web**. (Si la barra de herramientas está deshabilitada, seleccione el proyecto ContosoUniversity en **Explorador de soluciones**).

Visual Studio implementa la aplicación actualizada y el explorador se abre en la Página principal. Ejecute la página instructores para comprobar que la actualización se ha implementado correctamente. Cuando la aplicación intenta obtener acceso a la base de datos de esta página, Code First actualiza el esquema de la base de datos y ejecuta el método `Seed`. Cuando se muestre la página, verá la columna **fecha de nacimiento** esperada con fechas.

[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Implementación de la actualización de la base de datos en el entorno de producción

Ahora puede realizar la implementación en producción. La única diferencia es que usará *app\_. htm sin conexión* para evitar que los usuarios accedan al sitio y, por tanto, actualicen la base de datos mientras se implementan los cambios. Para la implementación de producción, realice los pasos siguientes:

- Cargue la *aplicación\_archivo. htm sin conexión* en el sitio de producción.
- En Visual Studio, elija el perfil de producción en la barra de herramientas de **publicación en Web** y haga clic en **publicar web**.
- Elimine la *aplicación\_archivo. htm sin conexión* del sitio de producción.

> [!NOTE]
> Mientras la aplicación está en uso en el entorno de producción, debe implementar un plan de copia de seguridad. Es decir, debe copiar periódicamente los archivos *School-Prod. sdf* y *ASPNET-Prod. sdf* desde el sitio de producción a una ubicación de almacenamiento segura, y debe mantener varias generaciones de dichas copias de seguridad. Al actualizar la base de datos, debe realizar una copia de seguridad inmediatamente antes del cambio. Después, si comete un error y no lo detecta hasta después de implementarlo en producción, podrá recuperar la base de datos al estado en que se encontraba antes de que se dañara.

Cuando Visual Studio abre la dirección URL de la Página principal en el explorador, se muestra la página *\_la aplicación offline. htm* . Después de eliminar la *aplicación\_archivo. htm sin conexión* , puede ir a la Página principal de nuevo para comprobar que la actualización se ha implementado correctamente.

[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)

Ahora ha implementado una actualización de la aplicación que incluía un cambio en la base de datos de prueba y producción. En el siguiente tutorial se muestra cómo migrar la base de datos de SQL Server Compact a SQL Server Express y SQL Server.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [Siguiente](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
