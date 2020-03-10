---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
title: Implementar bases de datos de pertenencia en entornos empresariales | Microsoft Docs
author: jrjlee
description: En este tema se explican las consideraciones y los desafíos clave que debe superar al aprovisionar las bases de datos de servicios de aplicaciones de ASP.NET (más comunes...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 3cf765df-d311-4f68-a295-c9685ceea830
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
msc.type: authoredcontent
ms.openlocfilehash: 50f49af502b75aa5ad52756a76a5e7340aca53f7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78422161"
---
# <a name="deploying-membership-databases-to-enterprise-environments"></a>Implementar bases de datos de pertenencia en entornos empresariales

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se explican las consideraciones y los desafíos clave que debe superar al aprovisionar las bases de datos de servicios de aplicaciones de ASP.NET (más conocidas como bases de datos de pertenencia) en entornos de prueba, ensayo o producción. También se describen los métodos que puede usar para cumplir estos desafíos.

Este tema forma parte de una serie de tutoriales basados en los requisitos de implementación empresarial de una empresa ficticia denominada Fabrikam, Inc. En esta serie de tutoriales se&#x2014;usa una solución de ejemplo de la&#x2014; [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar una aplicación web con un nivel de complejidad realista, como una aplicación ASP.NET MVC 3, un servicio Windows Communication Foundation (WCF) y un proyecto de base de datos.

El método de implementación que se encuentra en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto dividido que se describe en [Descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en el&#x2014;que el proceso de compilación se controla mediante dos archivos de proyecto que contiene instrucciones de compilación que se aplican a cada entorno de destino y uno que contiene la configuración de compilación e implementación específica del entorno. En tiempo de compilación, el archivo de proyecto específico del entorno se combina en el archivo de proyecto independiente del entorno para formar un conjunto completo de instrucciones de compilación.

## <a name="what-are-the-issues-when-you-deploy-a-membership-database"></a>¿Cuáles son los problemas que se producen al implementar una base de datos de pertenencia?

En la mayoría de los casos, al diseñar una estrategia de implementación para una base de datos, lo primero que debe tener en cuenta es qué datos desea implementar. En un entorno de desarrollo o de prueba, es posible que desee implementar datos de cuentas de usuario para facilitar las pruebas rápidas y sencillas. En un entorno de ensayo o de producción, es muy poco probable que desee implementar datos de cuentas de usuario.

Desafortunadamente, las bases de datos de pertenencia a ASP.NET presentan algunos desafíos específicos que hacen que esta decisión sea mucho más compleja:

- Una implementación de solo esquema dejará la base de datos de pertenencia en un estado no operativo. Esto se debe a que la base de datos de pertenencia incluye algunos datos de configuración (en la tabla **SchemaVersions de aspnet\_** ) que la base de datos necesita para funcionar. Como tal, si realiza una implementación solo de esquema de la base de datos de pertenencia para excluir los datos de la cuenta de usuario, deberá ejecutar un script posterior a la implementación para agregar los datos de configuración esenciales.
- En función de cómo esté configurada la base de datos de pertenencia, el proveedor de pertenencia puede usar la clave de equipo para cifrar las contraseñas y almacenarlas en la base de datos. En este caso, cualquier dato de cuenta de usuario que implemente con la base de datos dejará de ser utilizable en el servidor de destino. Por esta razón, la implementación de datos de cuentas de usuario no es un escenario admitido.

## <a name="choosing-a-membership-database-strategy"></a>Elegir una estrategia de base de datos de pertenencia

Siga estas instrucciones cuando elija cómo aprovisionar una base de datos de pertenencia en un entorno de servidor empresarial:

- Siempre que sea posible, no implemente las bases de datos de pertenencia. En su lugar, cree la base de datos de pertenencia manualmente en el servidor de base de datos de destino. Si no ha personalizado el esquema de la base de datos de pertenencia, puede crear simplemente uno en situ en el destino mediante la [herramienta de registro de SQL Server ASP.net (aspnet\_regsql. exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).
- Si no tiene ninguna opción, pero para implementar una base&#x2014;de datos de pertenencia por ejemplo, si ha realizado modificaciones extensas en el esquema&#x2014;de la base de datos, debe realizar una implementación solo de esquema de la base de datos de pertenencia, excluir los datos de la cuenta de usuario y, a continuación, ejecutar un script posterior a la implementación para agregar los datos de configuración necesarios. Puede encontrar una amplia orientación sobre estos enfoques en [Cómo: implementar la base de datos de pertenencia de ASP.net sin incluir cuentas de usuario](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

Es importante recordar que *es probable que el esquema de la base de datos de pertenencia sea bastante estático*. Incluso si ha personalizado la base de datos de pertenencia, no es probable que necesite actualizar el esquema de forma periódica&#x2014;, por lo que no va a cambiar con la misma frecuencia que el código en una aplicación web o un proyecto de base de datos. Como tal, no es necesario incluir la base de datos de pertenencia en ningún proceso de implementación automatizado o de un solo paso.

## <a name="using-vsdbcmd-to-update-a-membership-database-schema"></a>Usar VSDBCMD para actualizar un esquema de la base de datos de pertenencia

Si modifica la estructura de la base de datos de pertenencia después de la primera implementación, es posible que no desee usar la herramienta de implementación web (Web Deploy) de Internet Information Services (IIS) para volver a implementar la base de datos. La funcionalidad de implementación de base de datos de Web Deploy no incluye la capacidad de realizar actualizaciones diferenciales en una base de datos&#x2014;de destino, Web deploy debe quitar y volver a crear la base de datos. Esto significa que se pierden los datos de cuentas de usuario existentes, lo que normalmente no es deseable en entornos de ensayo o de producción.

La alternativa es usar la utilidad VSDBCMD para actualizar el esquema de la base de datos de destino. VSDBCMD incluye dos capacidades importantes. En primer lugar, puede importar el esquema de una base de datos existente en un archivo. dbschema. En segundo lugar, puede implementar un archivo. dbschema en una base de datos existente como una actualización diferencial, lo que significa que solo realiza los cambios necesarios para actualizar la base de datos de destino y no pierde ningún dato.

Puede usar estos pasos de alto nivel para actualizar un esquema de la base de datos de pertenencia:

1. Use la acción de **importación** de VSDBCMD para generar un archivo. dbschema para la base de datos de pertenencia de origen. Este procedimiento se describe en [Cómo: importar un esquema desde un símbolo del sistema](https://msdn.microsoft.com/library/dd172135.aspx).
2. Use la acción de **implementación** de VSDBCMD para implementar el archivo. dbschema en la base de datos de pertenencia de destino. Este procedimiento se describe en [referencia de línea de comandos para VSDBCMD. EXE (implementación y importación de esquema)](https://msdn.microsoft.com/library/dd193283.aspx).

## <a name="conclusion"></a>Conclusión

En este tema se describen algunos de los desafíos a los que puede enfrentarse cuando necesita aprovisionar bases de datos de pertenencia a ASP.NET en varios entornos de destino. En concreto, se explica por qué las implementaciones solo de esquema dejarán la base de datos de pertenencia en un estado no operativo y por qué no se admite la implementación de datos de cuentas de usuario. En el tema también se presentan instrucciones para aprovisionar, implementar y actualizar bases de datos de pertenencia en diferentes escenarios.

## <a name="further-reading"></a>Información adicional

Para obtener más instrucciones y ejemplos de cómo usar VSDBCMD, vea [referencia de la línea de comandos para VSDBCMD. EXE (implementación y importación de esquema)](https://msdn.microsoft.com/library/dd193283.aspx) y [Cómo: importar un esquema desde un símbolo del sistema](https://msdn.microsoft.com/library/dd172135.aspx). Para obtener más información sobre el uso de ASPNET\_regsql. exe para crear bases de datos de pertenencia, vea [ASP.NET SQL Server registration Tool (aspnet\_regsql. exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx). Para obtener instrucciones generales sobre la implementación de bases de datos de pertenencia, consulte [Cómo: implementar la base de datos de pertenencia de ASP.net sin incluir cuentas de usuario](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Anterior](deploying-database-role-memberships-to-test-environments.md)
> [Siguiente](excluding-files-and-folders-from-deployment.md)
