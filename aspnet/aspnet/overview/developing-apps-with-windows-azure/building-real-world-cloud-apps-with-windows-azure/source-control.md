---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
title: Control de código fuente (creación de aplicaciones en la nube reales con Azure) | Microsoft Docs
author: MikeWasson
description: La creación de aplicaciones reales en la nube con el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden...
ms.author: riande
ms.date: 06/23/2015
ms.assetid: 2a0370d3-c2fb-4bf3-88b8-aad5a736c793
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
msc.type: authoredcontent
ms.openlocfilehash: 5a1e0d7cd3c396d4be79c8958422602055eb3db1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78500533"
---
# <a name="source-control-building-real-world-cloud-apps-with-azure"></a>Control de código fuente (creación de aplicaciones en la nube reales con Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de corrección de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar el libro electrónico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **creación de aplicaciones reales en la nube con** el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden ayudarle a desarrollar correctamente aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).

El control de código fuente es esencial para todos los proyectos de desarrollo en la nube, no solo para los entornos de equipo. No podría pensar en editar código fuente o incluso en un documento de Word sin una función de deshacer y copias de seguridad automáticas, y el control de código fuente le proporciona esas funciones en un nivel de proyecto donde pueden ahorrar incluso más tiempo cuando algo sale mal. Con servicios de control de código fuente en la nube, ya no tiene que preocuparse de una configuración complicada y puede usar Azure Repos control de código fuente de forma gratuita para un máximo de 5 usuarios.

La primera parte de este capítulo explica tres prácticas recomendadas clave que se deben tener en cuenta:

- [Tratar los scripts de automatización como código fuente](#scripts) y la versión junto con el código de la aplicación.
- No [proteja nunca los secretos](#secrets) (datos confidenciales, como las credenciales) en un repositorio de código fuente.
- [Configure bifurcaciones de origen](#devops) para habilitar el flujo de trabajo de DevOps.

En el resto del capítulo se proporcionan algunas implementaciones de ejemplo de estos patrones en Visual Studio, Azure y Azure Repos:

- [Agregar scripts al control de código fuente en Visual Studio](#vsscripts)
- [Almacenar datos confidenciales en Azure](#appsettings)
- [Usar git en Visual Studio y Azure Repos](#gittfs)

<a id="scripts"></a>
## <a name="treat-automation-scripts-as-source-code"></a>Tratar scripts de automatización como código fuente

Cuando trabaje en un proyecto en la nube, cambiará con frecuencia y querrá poder reaccionar rápidamente a los problemas que informan los clientes. Responder rápidamente implica el uso de scripts de automatización, como se explica en el capítulo [automatizar todo](automate-everything.md) . Todos los scripts que se usan para crear el entorno, implementar en él, escalar, etc., deben estar sincronizados con el código fuente de la aplicación.

Para mantener los scripts sincronizados con el código, almacénelos en el sistema de control de código fuente. Después, si alguna vez necesita revertir los cambios o realizar una corrección rápida del código de producción, que es diferente del código de desarrollo, no tiene que perder tiempo intentando realizar un seguimiento de los valores de configuración que han cambiado o los miembros del equipo que tienen copias de la versión que necesita. Está seguro de que los scripts que necesita están sincronizados con el código base que necesita y está seguro de que todos los miembros del equipo están trabajando con los mismos scripts. A continuación, si necesita automatizar las pruebas y la implementación de una corrección activa en producción o en el nuevo desarrollo de características, tendrá el script adecuado para el código que debe actualizarse.

<a id="secrets"></a>
## <a name="dont-check-in-secrets"></a>No proteger secretos

Normalmente, se puede tener acceso a un repositorio de código fuente a demasiadas personas para que sea un lugar seguro adecuado para datos confidenciales, como contraseñas. Si los scripts se basan en secretos como contraseñas, parametrizar esas opciones para que no se guarden en el código fuente y almacene los secretos en otro lugar.

Por ejemplo, Azure le permite descargar archivos que contienen la configuración de publicación para automatizar la creación de perfiles de publicación. Estos archivos incluyen nombres de usuario y contraseñas que están autorizados a administrar los servicios de Azure. Si usa este método para crear perfiles de publicación, y si protege estos archivos en el control de código fuente, cualquier usuario con acceso al repositorio podrá ver esos nombres de usuario y contraseñas. Puede almacenar la contraseña de forma segura en el perfil de publicación en sí porque está cifrada y se encuentra en un archivo *. pubxml. User* que, de forma predeterminada, no se incluye en el control de código fuente.

<a id="devops"></a>
## <a name="structure-source-branches-to-facilitate-devops-workflow"></a>Bifurcaciones de origen de estructura para facilitar el flujo de trabajo de DevOps

La forma de implementar las bifurcaciones en el repositorio afecta a la capacidad de desarrollar nuevas características y solucionar problemas en producción. Este es un patrón que usan muchos equipos de tamaño medio:

![Estructura de bifurcación de origen](source-control/_static/image1.png)

La rama maestra siempre coincide con el código que está en producción. Las ramas situadas debajo de Master se corresponden con distintas fases del ciclo de vida de desarrollo. La rama desarrollo es donde se implementan las nuevas características. Para un equipo pequeño, es posible que solo tenga la principal y la de desarrollo, pero a menudo se recomienda que la gente tenga una rama de ensayo entre el desarrollo y la maestra. Puede usar el almacenamiento provisional para las pruebas de integración finales antes de que se mueva una actualización a producción.

En el caso de los equipos grandes, puede haber ramas independientes para cada característica nueva; en el caso de equipos más pequeños, puede que todos los usuarios protejan la rama de desarrollo.

Si tiene una rama para cada característica, cuando la característica A esté lista, combine los cambios del código fuente en la rama de desarrollo y baje en las otras ramas de características. Este proceso de combinación de código fuente puede llevar mucho tiempo y para evitar que el trabajo siga manteniendo las características independientes, algunos equipos implementan una alternativa denominada *[alternancia](http://en.wikipedia.org/wiki/Feature_toggle)* de características (también conocidas como *marcas de características*). Esto significa que todo el código de todas las características está en la misma rama, pero habilita o deshabilita cada característica mediante modificadores en el código. Por ejemplo, supongamos que la característica A es un campo nuevo para corregir las tareas de la aplicación de ti y la característica B agrega funcionalidad de almacenamiento en caché. El código de ambas características puede estar en la rama desarrollo, pero la aplicación solo mostrará el campo nuevo cuando una variable se establezca en true y solo usará el almacenamiento en caché cuando una variable diferente esté establecida en true. Si la característica A no está lista para promocionarse pero la característica B está lista, puede promocionar todo el código a producción con la característica un conmutador desactivado y el conmutador de la característica B. Después, puede finalizar la característica A y promoverla más adelante, todo sin combinación de código fuente.

Tanto si usa ramas como si no para características, una estructura de bifurcación como esta le permite transmitir el código desde el desarrollo hasta el entorno de producción de forma ágil y repetible.

Esta estructura también le permite reaccionar rápidamente a los comentarios de los clientes. Si necesita realizar una corrección rápida de la producción, también puede hacerlo eficazmente de forma ágil. Puede crear una bifurcación fuera de la principal o del almacenamiento provisional y, cuando esté listo, combinarla en maestra y en las ramas de características y desarrollo.

![rama de revisiones](source-control/_static/image2.png)

Sin una estructura de bifurcación como esta con la separación de las ramas de producción y desarrollo, un problema de producción podría ponerle en la posición de tener que promover el nuevo código de características junto con la corrección de producción. Es posible que el nuevo código de característica no esté totalmente probado y listo para la producción, y es posible que tenga que realizar una gran cantidad de trabajo de copia de seguridad de los cambios que no están listos. O bien, es posible que tenga que retrasar la corrección para probar los cambios y preparar la implementación.

A continuación, verá ejemplos de cómo implementar estos tres patrones en Visual Studio, Azure y Azure Repos. Estos son ejemplos en lugar de instrucciones de procedimientos paso a paso detalladas. para obtener instrucciones detalladas que proporcionen todo el contexto necesario, consulte la sección de [recursos](#resources) al final del capítulo.

<a id="vsscripts"></a>
## <a name="add-scripts-to-source-control-in-visual-studio"></a>Agregar scripts al control de código fuente en Visual Studio

Puede agregar scripts al control de código fuente en Visual Studio incluyéndolos en una carpeta de soluciones de Visual Studio (suponiendo que el proyecto está en el control de código fuente). Esta es una forma de hacerlo.

Cree una carpeta para los scripts en la carpeta de la solución (la misma carpeta que contiene el archivo *. sln* ).

![Carpeta Automation](source-control/_static/image3.png)

Copie los archivos de script en la carpeta.

![Contenido de la carpeta Automation](source-control/_static/image4.png)

En Visual Studio, agregue una carpeta de soluciones al proyecto.

![Selección del menú Nueva carpeta de soluciones](source-control/_static/image5.png)

Y agregue los archivos de script a la carpeta de la solución.

![Selección del menú Agregar elemento existente](source-control/_static/image6.png)

![Cuadro de diálogo Agregar elemento existente](source-control/_static/image7.png)

Los archivos de script se incluyen ahora en el proyecto y el control de código fuente está realizando el seguimiento de los cambios de versión junto con los cambios correspondientes del código fuente.

<a id="appsettings"></a>
## <a name="store-sensitive-data-in-azure"></a>Almacenar datos confidenciales en Azure

Si ejecuta la aplicación en un sitio web de Azure, una manera de evitar el almacenamiento de credenciales en el control de código fuente es almacenarlas en Azure en su lugar.

Por ejemplo, la aplicación Fix it almacena en su archivo Web. config dos cadenas de conexión que tendrán contraseñas en producción y una clave que proporcione acceso a su cuenta de almacenamiento de Azure.

[!code-xml[Main](source-control/samples/sample1.xml?highlight=2-3,11)]

Si coloca los valores de producción reales para esta configuración en el archivo *Web. config* o si los coloca en el archivo *Web. Release. config* para configurar una transformación de Web. config para insertarlos durante la implementación, se almacenarán en el repositorio de origen. Si especifica las cadenas de conexión de base de datos en el perfil de publicación de producción, la contraseña se encontrará en el archivo *. pubxml* . (Puede excluir el archivo *. pubxml* del control de código fuente, pero, a continuación, perderá la ventaja de compartir el resto de la configuración de implementación).

Azure le ofrece una alternativa para las secciones **appSettings** y cadenas de conexión del archivo *Web. config* . Esta es la parte relevante de la pestaña **configuración** de un sitio web en el portal de administración de Azure:

![appSettings y connectionStrings en el portal](source-control/_static/image8.png)

Cuando implemente un proyecto en este sitio web y se ejecute la aplicación, los valores que haya almacenado en Azure invalidarán los valores que se encuentren en el archivo Web. config.

Puede establecer estos valores en Azure mediante el portal de administración o scripts. El script de automatización de creación de entorno que vio en el capítulo [automatizar todo](automate-everything.md) crea una Azure SQL Database, obtiene las cadenas de conexión de almacenamiento y SQL Database y almacena estos secretos en la configuración del sitio Web.

[!code-powershell[Main](source-control/samples/sample2.ps1)]

[!code-powershell[Main](source-control/samples/sample3.ps1)]

Tenga en cuenta que los scripts están parametrizados para que los valores reales no se conserven en el repositorio de origen.

Cuando se ejecuta localmente en el entorno de desarrollo, la aplicación lee el archivo Web. config local y la cadena de conexión apunta a una base de datos de LocalDB SQL Server en la carpeta *app\_Data* del proyecto Web. Al ejecutar la aplicación en Azure y la aplicación intenta leer estos valores desde el archivo Web. config, lo que se devuelve y usa son los valores almacenados para el sitio web, no lo que se encuentra realmente en el archivo Web. config.

<a id="gittfs"></a>
## <a name="use-git-in-visual-studio-and-azure-devops"></a>Uso de Git en Visual Studio y Azure DevOps

Puede usar cualquier entorno de control de código fuente para implementar la estructura de bifurcación DevOps presentada anteriormente. En el caso de los equipos distribuidos, un [sistema de control de versiones distribuido](http://en.wikipedia.org/wiki/Distributed_revision_control) (DVCS) podría funcionar mejor. en el caso de otros equipos, es posible que un [sistema centralizado](http://en.wikipedia.org/wiki/Revision_control) funcione mejor.

[Git](http://git-scm.com/) es un popular sistema de control de versiones distribuido. Si usa git para el control de código fuente, tendrá una copia completa del repositorio con todo su historial en el equipo local. Muchas personas prefieren que, dado que es más fácil seguir trabajando cuando no está conectado a la red, puede seguir realizando confirmaciones y reversiones, crear y cambiar ramas, etc. Incluso cuando está conectado a la red, es más fácil y rápido crear bifurcaciones y cambiar de rama cuando todo es local. También puede realizar confirmaciones y reversiones locales sin que afecte a otros desarrolladores. Y puede procesar por lotes las confirmaciones antes de enviarlas al servidor.

[Azure Repos](/azure/devops/repos/index?view=vsts) ofrece [git](/azure/devops/repos/git/?view=vsts) y [control de versiones de Team Foundation](/azure/devops/repos/tfvc/index?view=vsts) (TFVC; control de código fuente centralizado). Comience a usar Azure DevOps [aquí](https://app.vsaex.visualstudio.com/signup).

Visual Studio 2017 incluye [compatibilidad con git](https://msdn.microsoft.com/library/hh850437.aspx)integrada de primera clase. Esta es una demostración rápida de cómo funciona.

Con un proyecto abierto en Visual Studio, haga clic con el botón derecho en la solución en **Explorador de soluciones**y, a continuación, elija **Agregar solución al control de código fuente**.

![Agregar solución al control de código fuente](source-control/_static/image9.png)

Visual Studio le pregunta si desea usar TFVC (control de versiones centralizado) o git.

![Elegir control de código fuente](source-control/_static/image10.png)

Al seleccionar git y hacer clic en **Aceptar**, Visual Studio crea un nuevo repositorio de Git local en la carpeta de la solución. El nuevo repositorio todavía no tiene archivos; tendrá que agregarlos al repositorio mediante una confirmación de Git. Haga clic con el botón secundario en la solución en **Explorador de soluciones**y, a continuación, haga clic en **confirmar**.

![Confirmación](source-control/_static/image11.png)

Visual Studio organiza automáticamente en fases todos los archivos de proyecto de la confirmación y los enumera en **Team Explorer** en el panel de **cambios incluidos** . (Si hubiera algunos que no deseaba incluir en la confirmación, puede seleccionarlos, hacer clic con el botón secundario y hacer clic en **excluir**).

![Team Explorer](source-control/_static/image12.png)

Escriba un Comentario de confirmación y haga clic en **confirmar**, y Visual Studio ejecutará la confirmación y mostrará el ID. de confirmación.

![Team Explorer cambios](source-control/_static/image13.png)

Ahora, si cambia parte del código para que sea diferente de lo que hay en el repositorio, puede ver fácilmente las diferencias. Haga clic con el botón derecho en un archivo que haya cambiado, seleccione **Comparar con sin modificar**y obtenga una pantalla de comparación que muestre el cambio no confirmado.

![Comparar con sin modificar](source-control/_static/image14.png)

![Diferencias que muestran los cambios](source-control/_static/image15.png)

Puede ver fácilmente qué cambios está realizando y protegerlos.

Supongamos que necesita crear una rama: puede hacerlo también en Visual Studio. En **Team Explorer**, haga clic en **nueva rama**.

![Team Explorer nueva rama](source-control/_static/image16.png)

Escriba un nombre de rama, haga clic en **crear rama**y, si seleccionó **rama de desprotección**, Visual Studio desprotegerá automáticamente la nueva rama.

![Team Explorer nueva rama](source-control/_static/image17.png)

Ahora puede realizar cambios en los archivos y protegerlos en esta rama. Además, puede cambiar fácilmente entre ramas y Visual Studio sincroniza automáticamente los archivos con cualquier rama que haya desprotegido. En este ejemplo, el título de la página web en *\_layout. cshtml* se ha cambiado a "Hot Fix 1" en la rama HotFix1.

![Rama Hotfix1](source-control/_static/image18.png)

Si vuelve a la rama Master, el contenido del archivo *\_layout. cshtml* se revierte automáticamente a lo que están en la rama Master.

![Rama maestra](source-control/_static/image19.png)

Este es un ejemplo sencillo de cómo puede crear rápidamente una bifurcación y avanzar y retroceder entre ramas. Esta característica permite un flujo de trabajo muy ágil mediante la estructura de bifurcación y los scripts de automatización presentados en el capítulo [automatizar todo](automate-everything.md) . Por ejemplo, puede trabajar en la rama desarrollo, crear una rama de corrección activa fuera de la maestra, cambiar a la nueva rama, realizar los cambios allí y confirmarlos, y luego volver a la rama de desarrollo y continuar lo que estaba haciendo.

Lo que ha visto aquí es cómo trabaja con un repositorio de Git local en Visual Studio. En un entorno de equipo, normalmente también se envían cambios a un repositorio común. Las herramientas de Visual Studio también permiten apuntar a un repositorio git remoto. Puede usar GitHub.com para ese propósito, o bien puede usar [git y Azure Repos](/azure/devops/repos/git/overview?view=vsts) integrado con las demás funcionalidades de DevOps de Azure, como el seguimiento de errores y elementos de trabajo.

Esta es la única forma de implementar una estrategia de bifurcación ágil, por supuesto. Puede habilitar el mismo flujo de trabajo ágil mediante un repositorio de control de código fuente centralizado.

## <a name="summary"></a>Resumen

Mida el éxito del sistema de control de código fuente en función de la rapidez con la que puede realizar un cambio y obtenerlo de una manera segura y predecible. Si se mezclarlas para realizar un cambio, ya que tiene que realizar un día o dos pruebas manuales en él, puede preguntarse lo que tiene que hacer en proceso o de prueba para que pueda realizar ese cambio en minutos o en peor tiempo de una hora. Una estrategia para hacerlo es implementar la integración continua y la entrega continua, que trataremos en el [siguiente capítulo](continuous-integration-and-continuous-delivery.md).

<a id="resources"></a>
## <a name="resources"></a>Recursos

Para obtener más información sobre las estrategias de bifurcación, vea los siguientes recursos:

- [Creación de una canalización de versión con Team Foundation Server 2012](https://msdn.microsoft.com/library/dn449957.aspx). Documentación sobre patrones y prácticas de Microsoft. Consulte el capítulo 6 para obtener una explicación de las estrategias de bifurcación. La característica aboga a través de las ramas de características y, si se usan ramas para las características, aboga por su mantenimiento a corto plazo (horas o días como máximo).
- [Guía de control de versiones](https://aka.ms/vsarsolutions). Guía de las estrategias de bifurcación de los rangos de ALM. Vea Branching Strategies. pdf en la pestaña descargas.
- [Desarrollo de software con alternancia de características](https://msdn.microsoft.com/magazine/dn683796.aspx). Artículo de MSDN Magazine.
- [Alternancia de características](http://martinfowler.com/bliki/FeatureToggle.html). Introducción a las marcas de características/alternancia de características en el blog de Martin Fowler.
- [Característica alterna las ramas de características de vs](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx). Otra entrada de blog acerca de los alternancias de características, por Dylan Smith.

Para obtener más información sobre cómo administrar la información confidencial que no debe mantenerse en los repositorios de control de código fuente, consulte los siguientes recursos:

- [Prácticas recomendadas para implementar contraseñas y otros datos confidenciales en ASP.net y Azure App Service](../../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
- [Sitios web de Azure: Cómo funcionan las cadenas de aplicación y las cadenas de conexión](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Explica la característica de Azure que invalida `appSettings` y `connectionStrings` datos en el archivo *Web. config* .
- Configuración [personalizada y configuración de la aplicación en sitios web de Azure: con Stefan Schackow](https://azure.microsoft.com/documentation/videos/configuration-and-app-settings-of-azure-web-sites/).

Para obtener información sobre otros métodos para mantener la información confidencial fuera del control de código fuente, vea [ASP.NET MVC: mantener la configuración privada fuera del control de código fuente](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).

> [!div class="step-by-step"]
> [Anterior](automate-everything.md)
> [Siguiente](continuous-integration-and-continuous-delivery.md)
