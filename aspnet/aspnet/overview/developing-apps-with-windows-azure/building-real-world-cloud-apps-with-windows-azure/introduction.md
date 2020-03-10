---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: Creación de aplicaciones en la nube del mundo real con Azure | Microsoft Docs
author: MikeWasson
description: Este libro electrónico le guía a través de un enfoque basado en patrones para crear soluciones en la nube del mundo real. Los patrones se aplican al proceso de desarrollo, así como a...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: b88573b3702b755b155e8da35f5f8a67931bafc6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78500653"
---
# <a name="building-real-world-cloud-apps-with-azure"></a>Creación de aplicaciones en la nube del mundo real con Azure

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de corrección de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar el libro electrónico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Este libro electrónico le guía a través de un enfoque basado en patrones para crear soluciones en la nube del mundo real. Los patrones se aplican al proceso de desarrollo, así como a las prácticas de arquitectura y codificación.
> 
> El contenido se basa en una presentación desarrollada por Scott Guthrie y se entrega en la Conferencia de desarrolladores noruegos (NDC) en junio de 2013 ([parte 1](http://vimeo.com/68215538), [parte 2](http://vimeo.com/68215602)) y en Microsoft Tech Ed Australia en septiembre de 2013 (parte[1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324), parte [2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325)). [Muchos otros](more-patterns-and-guidance.md#acknowledgments) han actualizado y ampliado el contenido al pasarlo de vídeo a formato escrito.

## <a name="intended-audience"></a>Destinatarios

Los desarrolladores que están familiarizados con el desarrollo en la nube, la consideración de un cambio a la nube o son nuevos en el desarrollo en la nube encontrarán aquí una breve descripción de los conceptos y prácticas más importantes que necesitan conocer. Los conceptos se ilustran con ejemplos concretos, y cada capítulo se vincula a otros recursos para obtener información más detallada. Los ejemplos y los vínculos a recursos adicionales son para los marcos y servicios de Microsoft, pero los principios que se ilustran se aplican también a otros marcos de desarrollo web y entornos en la nube.

Los desarrolladores que ya están desarrollando para la nube pueden encontrar ideas que le ayuden a mejorar su éxito. Cada capítulo de la serie se puede leer de forma independiente, por lo que puede elegir y elegir los temas que le interesen.

Cualquier persona que haya visto la *creación de aplicaciones en la nube reales* de Scott Guthrie con Azure Presentation y quiere obtener más detalles e información actualizada lo encontrará aquí.

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a>Patrones de desarrollo en la nube

En este libro electrónico se explican trece patrones recomendados para el desarrollo en la nube. "Pattern" se usa aquí en un sentido general para hacer cosas recomendadas: la mejor manera de desarrollar, diseñar y codificar aplicaciones en la nube. Estos son patrones clave que le ayudarán a "entrar en el Pit de éxito" si los sigue.

- [Automatizar todo](automate-everything.md).

    - Use scripts para maximizar la eficacia y minimizar los errores en procesos repetitivos.
    - Demo: scripts de administración de Azure.
- [Control de código fuente](source-control.md). 

    - Configure la estructura de bifurcación en el control de código fuente para facilitar el flujo de trabajo de DevOps.
    - Demo: agregar scripts al control de código fuente.
    - Demo: Mantenga los datos confidenciales fuera del control de código fuente.
    - Demo: usar git en Visual Studio.
- [Integración y entrega continuas](continuous-integration-and-continuous-delivery.md). 

    - Automatizar la compilación y la implementación con cada inserción en el repositorio del control de código fuente.
- [Prácticas recomendadas para el desarrollo web](web-development-best-practices.md). 

    - Mantener el nivel de Web sin estado.
    - Demo: escalado y escalado automático en Web Apps en Azure App Service.
    - Evite el estado de sesión.
    - Usar una red CDN con una reserva cuando la red CDN no está disponible.
    - Use el modelo de programación asincrónica.
    - Demo: Async en ASP.NET MVC y Entity Framework.
- [Inicio de sesión único](single-sign-on.md). 

    - Introducción a la Azure Active Directory.
    - Demo: cree una aplicación de ASP.NET que use Azure Active Directory.
- [Opciones de almacenamiento de datos](data-storage-options.md). 

    - Tipos de almacenes de datos.
    - Cómo elegir el almacén de datos correcto.
    - Demo: Azure SQL Database.
- [Estrategias de particionamiento de datos](data-partitioning-strategies.md). 

    - Divida los datos en vertical, horizontalmente o ambos para facilitar el escalado de una base de datos relacional.
- [Almacenamiento de blobs no estructurado](unstructured-blob-storage.md). 

    - Almacene los archivos en la nube mediante el servicio BLOB.
    - Demo: usar el almacenamiento de blobs en la aplicación Fix it.
- [Diseño para sobrevivir a errores](design-to-survive-failures.md). 

    - Tipos de errores.
    - Ámbito del error.
    - Descripción de los SLA.
- [Supervisión y telemetría](monitoring-and-telemetry.md). 

    - Por qué debe comprar una aplicación de telemetría y escribir su propio código para instrumentar la aplicación.
    - Demo: New Relic para Azure
    - Demo: registro de código en la aplicación Fix it.
    - Demo: inserción de dependencias en la aplicación Fix it.
    - Demo: compatibilidad de registro integrada en Azure.
- [Control de errores transitorios](transient-fault-handling.md). 

    - Use la lógica de reintento/interrupción inteligente para mitigar el efecto de los errores transitorios.
    - Demo: reintentos o devoluciones en Entity Framework 6.
- [Almacenamiento en caché distribuido](distributed-caching.md). 

    - Mejorar la escalabilidad y reducir los costos de transacciones de bases de datos mediante el uso de almacenamiento en caché distribuido.
- [Patrón de trabajo centrado en cola](queue-centric-work-pattern.md). 

    - Habilite la alta disponibilidad y mejore la escalabilidad mediante el acoplamiento flexible de niveles web y de trabajo.
    - Demo: colas de Azure Storage en la aplicación Fix it.
- [Más pautas y patrones de aplicaciones en la nube](more-patterns-and-guidance.md).
- [Apéndice: Aplicación de ejemplo Reparar](the-fix-it-sample-application.md)

    - Problemas conocidos
    - Procedimientos recomendados
    - Cómo descargar, compilar, ejecutar e implementar.

Estos patrones se aplican a todos los entornos de nube, pero se mostrarán mediante ejemplos basados en tecnologías y servicios de Microsoft, como Visual Studio, Team Foundation Service, ASP.NET y Azure.

En este resto de este capítulo se presenta la aplicación de ejemplo de corrección y el Web Apps en Azure App Service entorno de nube en el que se ejecuta la aplicación de corrección de ti.

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a>Aplicación de ejemplo Fix it

La mayoría de los ejemplos de código y capturas de pantalla que se muestran en este libro electrónico se basan en la aplicación Fix it desarrollada originalmente por [Scott Guthrie](https://weblogs.asp.net/scottgu/) para demostrar los patrones y procedimientos recomendados de desarrollo de aplicaciones en la nube.

![Corregir Página principal de la aplicación](introduction/_static/image1.png)

La aplicación de ejemplo es un sistema simple de vales de elementos de trabajo. Si necesita algo corregido, puede crear un vale y asignarlo a alguien, y otros usuarios pueden iniciar sesión y ver los vales asignados a ellos y marcar las entradas como completadas cuando el trabajo ha terminado.

Es un proyecto Web de Visual Studio estándar. Se basa en ASP.NET MVC y utiliza una base de datos SQL Server. Se puede ejecutar localmente en IIS Express y se puede implementar en un sitio web de Azure para ejecutarse en la nube. Puede iniciar sesión mediante la autenticación de formularios y una base de datos local o mediante un proveedor de redes sociales como Google. (Más adelante también se mostrará cómo iniciar sesión con una cuenta de la organización Active Directory).

![Página de inicio de sesión](introduction/_static/image2.png)

Una vez que haya iniciado sesión, puede crear un vale, asignarlo a alguien y cargar una imagen de lo que desea corregir.

![Crear una tarea corregir ti](introduction/_static/image3.png)

![Corregir la tarea creada](introduction/_static/image4.png)

Puede realizar un seguimiento del progreso de los elementos de trabajo que ha creado, ver los vales asignados a usted, ver los detalles de los vales y marcar los elementos como completados.

Se trata de una aplicación muy sencilla desde una perspectiva de características, pero verá cómo compilarla para que se pueda escalar a millones de usuarios y que sea resistente a cosas como errores de base de datos y finalizaciones de conexión. También aprenderá a crear un flujo de trabajo de desarrollo ágil y automatizado, lo que le permite empezar a usar de forma sencilla y mejorar la aplicación mediante la iteración del ciclo de desarrollo de manera eficaz y rápida.

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a>Web Apps en Azure App Service

El entorno de nube que se usa para la aplicación Fix it es un servicio de Azure al que llamamos sitios Web. Este servicio es una forma de hospedar su propia aplicación web en Azure sin tener que crear máquinas virtuales y mantenerlas actualizadas, instalar y configurar IIS, etc. Hospedamos su sitio en nuestras máquinas virtuales y proporcionamos automáticamente copias de seguridad y recuperación y otros servicios. El servicio sitios web funciona con ASP.NET, node. js, PHP y Python. Permite realizar una implementación muy rápida con Visual Studio, Web Deploy, FTP, Git o TFS. Normalmente es solo unos pocos segundos entre el momento en que se inicia una implementación y el momento en que la actualización está disponible a través de Internet. Es todo lo que puede comenzar, y puede escalar verticalmente a medida que crezca el tráfico.

En segundo plano, Web Apps en Azure App Service proporciona una gran cantidad de componentes y características de arquitectura que tendría que crear usted mismo si fuera a hospedar un sitio web mediante IIS en sus propias máquinas virtuales. Un componente es un punto de conexión de implementación que configura automáticamente IIS e instala la aplicación en tantas máquinas virtuales como desee para ejecutar el sitio.

![Servicio de implementación](introduction/_static/image5.png)

Cuando un usuario visita el sitio web, no alcanza las máquinas virtuales de IIS directamente, pasan por los equilibradores de carga de [enrutamiento de solicitud de aplicaciones (arr)](https://www.iis.net/downloads/microsoft/application-request-routing) . Puede usarlos con sus propios servidores, pero la ventaja es que están configurados automáticamente. Usan una heurística inteligente que tiene en cuenta factores como la afinidad de la sesión, la profundidad de la cola en IIS y el uso de CPU en cada equipo para dirigir el tráfico a las máquinas virtuales que hospedan el sitio Web.

![Equilibrador de carga ARR](introduction/_static/image6.png)

Si un equipo deja de funcionar, Azure lo extrae automáticamente de la rotación, pone en marcha una nueva instancia de máquina virtual y comienza a dirigir el tráfico a la nueva instancia, todo ello sin tiempo de inactividad para la aplicación.

![Recuperación automática de errores de máquina](introduction/_static/image7.png)

Todo esto se realiza automáticamente. Lo único que debe hacer es crear un sitio web e implementar la aplicación en él, mediante Windows PowerShell, Visual Studio o el portal de administración de Azure.

Para ver un tutorial paso a paso rápido y sencillo que muestra cómo crear una aplicación web en Visual Studio e implementarla en un sitio web de Azure, consulte Introducción a [Azure y ASP.net](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

<a id="summary"></a>
## <a name="summary"></a>Resumen

Esta introducción ha proporcionado una lista de temas que el libro cubrirá, capturas de pantallas de la aplicación de ejemplo y una breve descripción general del Web Apps en Azure App Service entorno de nube. Una de las grandes ventajas de desarrollar aplicaciones en y en la nube es que es fácil automatizar las tareas de desarrollo repetitivas, como crear un entorno de prueba e implementar el código en ella. Cómo hacerlo es el tema del [siguiente capítulo](automate-everything.md).

## <a name="resources"></a>Recursos

Para obtener más información acerca de los temas que se tratan en este capítulo, vea los siguientes recursos.

Documentación:

- [Web Apps en Azure App Service](https://azure.microsoft.com/services/app-service/web/). Página del portal para la documentación de Azure sobre Web Apps.
- [Web Apps, Cloud Services y máquinas virtuales: ¿Cuándo se debe usar?](https://azure.microsoft.com/documentation/articles/choose-web-site-cloud-service-vm/) WAWS, tal como se muestra en este capítulo, es solo una de las tres formas en que puede ejecutar aplicaciones web en Azure. En este artículo se explican las diferencias entre las tres maneras y se proporcionan instrucciones sobre cómo elegir cuál es la adecuada para su escenario. Al igual que los sitios web, Cloud Services es una característica de PaaS de Azure. Las máquinas virtuales son una característica de IaaS. Para obtener una explicación de PaaS frente a IaaS, consulte el capítulo [Opciones de datos](data-storage-options.md#paasiaas) .

Vídeos:

- [Scott Guthrie comienza en el paso 0: ¿Qué es Azure Cloud OS?](https://azure.microsoft.com/documentation/videos/what-is-the-cloud-os-scottgu/)
- [Arquitectura de sitios web: con Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/).
- [Sitios web de Azure Internals con NIR Mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski).

> [!div class="step-by-step"]
> [Siguiente](automate-everything.md)
