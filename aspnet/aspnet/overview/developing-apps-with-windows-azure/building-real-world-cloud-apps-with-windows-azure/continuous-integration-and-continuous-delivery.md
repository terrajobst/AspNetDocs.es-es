---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
title: Integración continua y entrega continua (creación de aplicaciones en la nube reales con Azure) | Microsoft Docs
author: MikeWasson
description: La creación de aplicaciones reales en la nube con el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: eaece9f5-f80c-428b-b771-5db66d275b7d
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
msc.type: authoredcontent
ms.openlocfilehash: cf3c65ef95528173eed3fb08984035b2512861c4
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457042"
---
# <a name="continuous-integration-and-continuous-delivery-building-real-world-cloud-apps-with-azure"></a>Integración continua y entrega continua (creación de aplicaciones en la nube reales con Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de corrección de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar el libro electrónico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **creación de aplicaciones reales en la nube con** el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden ayudarle a desarrollar correctamente aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).

Los dos primeros patrones de proceso de desarrollo recomendados fueron [automatizados todo](automate-everything.md) y el [control de código fuente](source-control.md), y el tercer patrón de proceso los combina. La integración continua (CI) significa que cada vez que un desarrollador protege el código en el repositorio de origen, se desencadena automáticamente una compilación. La entrega continua (CD) lleva un paso más allá: después de que una compilación y las pruebas unitarias automatizadas se realizan correctamente, implementa automáticamente la aplicación en un entorno donde puede realizar pruebas más exhaustivas.

La nube le permite minimizar el costo de mantenimiento de un entorno de prueba, ya que solo paga por los recursos del entorno, siempre y cuando los esté usando. El proceso de CD puede configurar el entorno de prueba cuando lo necesite, y puede deshacerlo cuando haya terminado las pruebas.

## <a name="continuous-integration-and-continuous-delivery-workflow"></a>Flujo de trabajo de integración continua y entrega continua

Por lo general, se recomienda realizar la entrega continua en los entornos de desarrollo y ensayo. La mayoría de los equipos, incluso en Microsoft, requieren un proceso de revisión y aprobación manual para la implementación de producción. En el caso de una implementación de producción, es posible que desee asegurarse de que se produce cuando las personas clave del equipo de desarrollo están disponibles para soporte técnico o durante períodos de tráfico bajo. Sin embargo, no hay nada que le impida automatizar completamente los entornos de desarrollo y pruebas para que todos los desarrolladores tengan que proteger un cambio y se configure un entorno para las pruebas de aceptación.

En el diagrama siguiente de [un libro electrónico de patrones y prácticas de Microsoft sobre la entrega continua](https://aka.ms/ReleasePipeline) se muestra un flujo de trabajo típico. Haga clic en la imagen para ver su tamaño completo en su contexto original.

[flujo de trabajo de entrega continua ![](continuous-integration-and-continuous-delivery/_static/image1.png)](https://msdn.microsoft.com/library/dn449955.aspx)

## <a name="how-the-cloud-enables-cost-effective-ci-and-cd"></a>Cómo la nube habilita los CI y el CD rentables

Automatizar estos procesos en Azure es fácil. Dado que está ejecutando todo en la nube, no tiene que comprar ni administrar servidores para sus compilaciones o entornos de prueba. Y no tiene que esperar a que un servidor esté disponible para realizar las pruebas. Con cada compilación que haga, puede poner en marcha un entorno de prueba en Azure con el script de automatización, ejecutar pruebas de aceptación o realizar pruebas más exhaustivas en él y, a continuación, cuando acabe de cortarlo. Y si solo ejecuta ese servidor durante 2 horas o 8 horas o un día, la cantidad de dinero que tiene que pagar es mínima, ya que solo paga por el tiempo en el que se está ejecutando realmente una máquina. Por ejemplo, el entorno necesario para la aplicación de corrección de ti básicamente cuesta aproximadamente un ciento por hora si se desplaza un nivel hacia arriba desde el nivel gratis. En el transcurso de un mes, si solo ejecutó el entorno una hora cada vez, el entorno de pruebas podría costar menos que un latte que compre en Starbucks.

## <a name="azure-devops-services"></a>Azure DevOps Services 

Azure DevOps Services proporciona varias características para ayudarle en el desarrollo de aplicaciones desde la planeación de la implementación.

- Admite el control de código fuente git (distribuido) y TFVC (centralizado).
- Ofrece un servicio de compilación elástica, lo que significa que crea dinámicamente servidores de compilación cuando se necesitan y los pone al día cuando finalizan. Puede iniciar automáticamente una compilación cuando alguien protege los cambios del código fuente y no tiene que asignar y pagar sus propios servidores de compilación que se encuentran inactivos la mayor parte del tiempo. El servicio de compilación es gratuito siempre que no supere un número determinado de compilaciones. Si espera realizar un gran volumen de compilaciones, puede pagar un poco más para los servidores de compilación reservados.
- Admite la entrega continua a Azure.
- Admite la prueba de carga automatizada. La prueba de carga es fundamental para una aplicación en la nube, pero a menudo no se tiene cuidado hasta que es demasiado tarde. La prueba de carga simula un uso intensivo de una aplicación por miles de usuarios, lo que le permite encontrar cuellos de botella y mejorar el rendimiento, antes de publicar la aplicación en producción.
- Admite la colaboración en el salón del equipo, lo que facilita la comunicación en tiempo real y la colaboración para equipos ágiles pequeños.
- Admite la administración de proyectos ágiles.

Para obtener más información sobre las características de integración y entrega continuas de Azure DevOps Services, consulte [la documentación de Azure DevOps](/azure/devops/index).

Si está buscando una solución de administración de proyectos, colaboración en equipo y control de código fuente, consulte Azure DevOps Services. Regístrese en [Azure DevOps Services](https://dev.azure.com/).

## <a name="summary"></a>Resumen

Los tres primeros patrones de desarrollo en la nube han estado sobre cómo implementar un proceso de desarrollo repetible, confiable y predecible con un tiempo de ciclo inferior. En el [siguiente capítulo](web-development-best-practices.md) comenzamos a examinar los patrones de arquitectura y codificación.

## <a name="resources"></a>Recursos

Para obtener más información, consulte [implementación de una aplicación web en Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-deploy/).

Consulte también los siguientes recursos:

- [Creación de una canalización de versión con Team Foundation Server 2012](https://aka.ms/ReleasePipeline). El libro electrónico, los laboratorios prácticos y el código de ejemplo de los patrones y prácticas de Microsoft proporcionan una introducción detallada sobre la entrega continua. Trata el uso de Visual Studio Lab Management y Release Management de Visual Studio.
- [Instrucciones y herramientas de DevOps rangeers](https://aka.ms/vsarsolutions/). Los rangos de ALM introdujeron la solución complementaria de ejemplo de DevOps Workbench y una guía práctica en colaboración con el libro Patterns &amp; Practices que *crean una canalización de versiones con TFS 2012*, como una excelente manera de empezar a aprender los conceptos de DevOps &amp; Release Management para TFS 2012 y para iniciar los neumáticos. En la guía se muestra cómo crear una vez e implementar en varios entornos.
- [Pruebas para la entrega continua con Visual Studio 2012](https://msdn.microsoft.com/library/jj159345.aspx). Libro electrónico de Microsoft Patterns and Practices, se explica cómo integrar las pruebas automatizadas con la entrega continua.
- [WindowsAzureDeploymentTracker](https://github.com/RyanTBerry/WindowsAzureDeploymentTracker). Código fuente de una herramienta diseñada para capturar una compilación de TFS (basada en una etiqueta), compilarla, empaquetarla, permitir que alguien en el rol DevOps configure aspectos específicos del mismo e insertarla en Azure. La herramienta realiza un seguimiento del proceso de implementación para habilitar las operaciones de "reversión" a una versión implementada previamente. La herramienta no tiene dependencias externas y puede funcionar de forma independiente mediante las API de TFS y el SDK de Azure.
- [Entrega continua: versiones de software confiables a través de la automatización de compilación, pruebas e implementación](https://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912/ref=sr_1_1?s=books&amp;ie=UTF8&amp;qid=1377126361). Libro de Jez Humble.
- La [versión de lanzamiento diseña e implementa software listo para la producción](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213). Libro de Michael T. Nygard.

> [!div class="step-by-step"]
> [Anterior](source-control.md)
> [Siguiente](web-development-best-practices.md)
