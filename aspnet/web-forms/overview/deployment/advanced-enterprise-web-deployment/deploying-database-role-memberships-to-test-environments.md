---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: Implementar pertenencias a roles de base de datos en entornos de prueba | Microsoft Docs
author: jrjlee
description: En este tema se describe cómo agregar cuentas de usuario a roles de base de datos como parte de la implementación de una solución en un entorno de prueba. Al implementar una solución que contiene...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: a15f5bf5f659d151e91ef9e53c5ad55bcd8e2b01
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78474949"
---
# <a name="deploying-database-role-memberships-to-test-environments"></a>Implementar las pertenencias a roles de base de datos en entornos de prueba

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo agregar cuentas de usuario a roles de base de datos como parte de la implementación de una solución en un entorno de prueba.
> 
> Cuando implementa una solución que contiene un proyecto de base de datos en un entorno de ensayo o de producción, normalmente no desea que el desarrollador automatice la adición de cuentas de usuario a roles de base de datos. En la mayoría de los casos, el desarrollador no sabrá qué cuentas de usuario se deben agregar a los roles de base de datos, y estos requisitos podrían cambiar en cualquier momento. Sin embargo, cuando se implementa una solución que contiene un proyecto de base de datos en un entorno de desarrollo o de prueba, la situación suele ser bastante diferente:
> 
> - Normalmente, el desarrollador vuelve a implementar la solución de forma periódica, a menudo varias veces al día.
> - La base de datos se vuelve a crear normalmente en cada implementación, lo que significa que los usuarios de la base de datos deben crearse y agregarse a los roles después de cada implementación.
> - Normalmente, el desarrollador tiene control total sobre el entorno de desarrollo o de prueba de destino.
> 
> En este escenario, a menudo resulta útil crear automáticamente usuarios de base de datos y asignar pertenencias a roles de base de datos como parte del proceso de implementación.
> 
> El factor clave es que esta operación debe ser condicional según el entorno de destino. Si va a realizar la implementación en un entorno de ensayo o de producción, quiere omitir la operación. Si va a implementar en un entorno de desarrollo o de prueba, querrá implementar la pertenencia a roles sin necesidad de más intervención. En este tema se describe un enfoque que puede usar para abordar este desafío.

Este tema forma parte de una serie de tutoriales basados en los requisitos de implementación empresarial de una empresa ficticia denominada Fabrikam, Inc. En esta serie de tutoriales se&#x2014;usa una solución de ejemplo de la&#x2014; [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar una aplicación web con un nivel de complejidad realista, como una aplicación ASP.NET MVC 3, un servicio Windows Communication Foundation (WCF) y un proyecto de base de datos.

El método de implementación que se encuentra en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto dividido que se describe en [Descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en el&#x2014;que el proceso de compilación se controla mediante dos archivos de proyecto que contiene instrucciones de compilación que se aplican a cada entorno de destino y uno que contiene la configuración de compilación e implementación específica del entorno. En tiempo de compilación, el archivo de proyecto específico del entorno se combina en el archivo de proyecto independiente del entorno para formar un conjunto completo de instrucciones de compilación.

## <a name="task-overview"></a>Información general sobre tareas

En este tema se supone que:

- El enfoque de archivo de proyecto dividido se usa para la implementación de soluciones, como se describe en [Descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Llame a VSDBCMD desde el archivo de proyecto para implementar el proyecto de base de datos, como se describe en [Descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Para crear usuarios de base de datos y asignar pertenencias a roles al implementar un proyecto de base de datos en un entorno de prueba, necesitará:

- Cree un script de Transact Lenguaje de consulta estructurado (Transact-SQL) que realice los cambios necesarios en la base de datos.
- Cree un destino de Microsoft Build Engine (MSBuild) que use la utilidad sqlcmd. exe para ejecutar el script SQL.
- Configure los archivos de proyecto para que invoquen el destino al implementar la solución en un entorno de prueba.

En este tema se muestra cómo realizar cada uno de estos procedimientos.

## <a name="scripting-the-database-role-memberships"></a>Crear scripts de pertenencias a roles de base de datos

Puede crear un script Transact-SQL de muchas maneras diferentes y en cualquier ubicación que elija. El enfoque más sencillo consiste en crear el script dentro de la solución en Visual Studio 2010.

**Para crear un script SQL**

1. En la ventana de **Explorador de soluciones** , expanda el nodo del proyecto de base de datos.
2. Haga clic con el botón secundario en la carpeta **scripts** , elija **Agregar**y, a continuación, haga clic en **nueva carpeta**.
3. Escriba **Test** como el nombre de la carpeta y, a continuación, presione Entrar.
4. Haga clic con el botón secundario en la carpeta **prueba** , seleccione **Agregar**y, a continuación, haga clic en **script**.
5. En el cuadro de diálogo **Agregar nuevo elemento** , asigne un nombre descriptivo a la secuencia de comandos (por ejemplo, **AddRoleMemberships. SQL**) y, a continuación, haga clic en **Agregar**.

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. En el archivo *AddRoleMemberships. SQL* , agregue instrucciones TRANSACT-SQL que:

    1. Cree un usuario de base de datos para el inicio de sesión de SQL Server que tendrá acceso a la base de datos.
    2. Agregue el usuario de base de datos a los roles de base de datos necesarios.
7. El archivo debe ser similar al siguiente:

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. Guarde el archivo.

## <a name="executing-the-script-on-the-target-database"></a>Ejecutar el script en la base de datos de destino

Idealmente, ejecutaría cualquier script de Transact-SQL necesario como parte de un script posterior a la implementación al implementar el proyecto de base de datos. Sin embargo, los scripts posteriores a la implementación no permiten ejecutar la lógica de forma condicional en función de las configuraciones de la solución o las propiedades de compilación. La alternativa es ejecutar los scripts SQL directamente desde el archivo de proyecto de MSBuild mediante la creación de un elemento de **destino** que ejecute un comando sqlcmd. exe. Puede usar este comando para ejecutar el script en la base de datos de destino:

[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]

> [!NOTE]
> Para obtener más información sobre las opciones de línea de comandos de SQLCMD, vea [sqlcmd (utilidad](https://msdn.microsoft.com/library/ms162773.aspx)).

Antes de insertar este comando en un destino de MSBuild, debe tener en cuenta en qué condiciones desea que se ejecute el script:

- La base de datos de destino debe existir antes de cambiar sus pertenencias a roles. Como tal, debe ejecutar este script *después* de la implementación de la base de datos.
- Debe incluir una condición para que el script solo se ejecute para entornos de prueba.
- Si está ejecutando una implementación&#x2014;"What if" en otras palabras, si va a generar scripts de implementación pero no los ejecuta&#x2014;realmente, no debe ejecutar el script SQL.

Si utiliza el enfoque de archivo de proyecto dividido que se describe en [Descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), como se muestra en la solución de ejemplo Contact Manager, puede dividir las instrucciones de compilación del script SQL de la siguiente manera:

- Cualquier propiedad necesaria específica del entorno, junto con la propiedad que determina si se deben implementar permisos, debe ir en el archivo de proyecto específico del entorno (por ejemplo, *env-dev. proj*).
- El destino de MSBuild, junto con cualquier propiedad que no cambie entre los entornos de destino, debe ir en el archivo de proyecto universal (por ejemplo, *Publish. proj*).

En el archivo de proyecto específico del entorno, debe definir el nombre del servidor de base de datos, el nombre de la base de datos de destino y una propiedad booleana que permite al usuario especificar si desea implementar la pertenencia a roles.

[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]

En el archivo de proyecto universal, debe proporcionar la ubicación del archivo ejecutable SQLCMD y la ubicación del script SQL que desea ejecutar. Estas propiedades seguirán siendo las mismas independientemente del entorno de destino. También debe crear un destino de MSBuild para ejecutar el comando sqlcmd.

[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]

Observe que se agrega la ubicación del ejecutable de SQLCMD como una propiedad estática, ya que esto podría ser útil para otros destinos. Por el contrario, se define la ubicación del script de SQL y la sintaxis del comando SQLCMD como propiedades dinámicas dentro del destino, ya que no serán necesarios antes de que se ejecute el destino. En este caso, el destino **DeployTestDBPermissions** solo se ejecutará si se cumplen estas condiciones:

- La propiedad **DeployTestDBRoleMemberships** se establece en **true**.
- El usuario no ha especificado una marca **Whatif = true** .

Por último, no se olvide de invocar el destino. En el archivo *Publish. proj* , puede hacerlo agregando el destino a la lista de dependencias para el destino predeterminado **FullPublish** . Debe asegurarse de que el destino **DeployTestDBPermissions** no se ejecute hasta que se haya ejecutado el destino **PublishDbPackages** .

[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]

## <a name="conclusion"></a>Conclusión

En este tema se describe una manera de agregar usuarios y pertenencias a roles de base de datos como una acción posterior a la implementación al implementar un proyecto de base de datos. Esto suele ser útil cuando se vuelve a crear una base de datos con regularidad en un entorno de prueba, pero normalmente se debe evitar cuando se implementan bases de datos en entornos de ensayo o de producción. Por lo tanto, debe asegurarse de que usa la lógica condicional necesaria para que los usuarios de la base de datos y las pertenencias a roles solo se creen cuando sea adecuado hacerlo.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre el uso de VSDBCMD para implementar proyectos de base de datos, vea [implementar proyectos de base de datos](../web-deployment-in-the-enterprise/deploying-database-projects.md). Para obtener instrucciones sobre cómo personalizar las implementaciones de base de datos para distintos entornos de destino, consulte [Personalización de implementaciones de base de datos para varios entornos](customizing-database-deployments-for-multiple-environments.md). Para obtener más información sobre el uso de archivos de proyecto de MSBuild personalizados para controlar el proceso de implementación, vea [Descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) y [Descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Para obtener más información sobre las opciones de línea de comandos de SQLCMD, vea [sqlcmd (utilidad](https://msdn.microsoft.com/library/ms162773.aspx)).

> [!div class="step-by-step"]
> [Anterior](customizing-database-deployments-for-multiple-environments.md)
> [Siguiente](deploying-membership-databases-to-enterprise-environments.md)
