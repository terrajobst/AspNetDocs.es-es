---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/more-patterns-and-guidance
title: Más patrones e instrucciones (creación de aplicaciones en la nube reales con Azure) | Microsoft Docs
author: MikeWasson
description: La creación de aplicaciones reales en la nube con el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 7e97cfc3-d830-4002-8ff7-5790d1ff49e6
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/more-patterns-and-guidance
msc.type: authoredcontent
ms.openlocfilehash: afade34477d1136883e7543d09e73dfbe435690e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74585361"
---
# <a name="more-patterns-and-guidance-building-real-world-cloud-apps-with-azure"></a>Más patrones e instrucciones (creación de aplicaciones en la nube reales con Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de corrección de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar el libro electrónico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **creación de aplicaciones reales en la nube con** el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden ayudarle a desarrollar correctamente aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).

Ahora ha visto 13 patrones que proporcionan instrucciones sobre cómo hacerlo correctamente en la informática en la nube. Estos son solo algunos de los patrones que se aplican a las aplicaciones en la nube. Estos son algunos temas más sobre informática en la nube y recursos para ayudarle con ellos:

- Migrar aplicaciones locales existentes a la nube. 

    - [Mover aplicaciones a la nube](https://msdn.microsoft.com/library/ff728592.aspx). Libro electrónico de Microsoft Patterns and Practices. También está disponible como un [bolsillo de copia rígida](https://www.amazon.com/dp/1621140202).
    - [Migración de ASP.net y IIS.net de Microsoft](https://go.microsoft.com/fwlink/?LinkId=400656). Caso práctico de Robert McMurray.
    - [Mover 4th &amp; mayor a sitios web de Azure](http://www.jeff.wilcox.name/2013/04/4thandmayor-azure-websites/). Publicación de blog de Jeff Wilcox crónicamente su experiencia en el movimiento de una aplicación web desde Amazon Web Services a Web Apps en Azure App Service.
    - [Mover aplicaciones a Azure: ¿Qué cambios hay?](https://azure.microsoft.com/documentation/videos/web-sites-internals-and-the-file-system/) Short video de Stefan Schackow, se explica el acceso al sistema de archivos en Web Apps en Azure App Service.
    - [Nube híbrida de Azure](https://www.amazon.com/dp/B00EOP4UQW). Libro impreso o libro electrónico de Danny Garber, Cristóbal Malik y Adam Fazio.
- Problemas de seguridad, autenticación y autorización exclusivos de las aplicaciones en la nube

    - [Guía de seguridad de Azure](https://azure.microsoft.com/blog/2014/02/10/best-practices-windows-azure-websites-waws/)
    - [Patrones y prácticas de Microsoft: Guía de Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte patrón de equipo selector, patrón de identidad federada.
    - [Seguridad de red de Azure](https://download.microsoft.com/download/4/3/9/43902EC9-410E-4875-8800-0788BE146A3D/Windows%20Azure%20Network%20Security%20Whitepaper%20-%20FINAL.docx). Notas del producto de Ashin aparte.

Vea también patrones e instrucciones de informática en la nube adicionales en [patrones y prácticas de Microsoft: instrucciones de Azure](https://msdn.microsoft.com/library/dn568099.aspx).

<a id="resources"></a>
## <a name="resources"></a>Recursos

En cada uno de los capítulos de este libro electrónico se proporcionan vínculos a recursos para obtener más información sobre ese tema específico. En la lista siguiente se proporcionan vínculos a información general de los procedimientos recomendados y patrones recomendados para el desarrollo en la nube con Azure.

Documentation

- [Prácticas recomendadas para el diseño de servicios a gran escala en Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). En las notas del producto, Mark SIMM y Michael Thomassy.
- [Failsafe: Guía para arquitecturas de nube resistentes](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Notas del producto de Marc Mercuri, Ulrich Homann y Andrew Townhill. Versión de la Página Web de la serie de vídeos FailSafe.
- [Guía de Azure](https://azure.microsoft.com/develop/net/guidance/) Página del portal de documentación oficial relacionada con el desarrollo de aplicaciones para Azure.

Vídeos

- [Creación de aplicaciones reales en la nube con Azure, parte 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324) y [parte 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325). Vídeo de la presentación de Scott Guthrie en la que se basa este libro electrónico. Presente en Tech Ed Australia en septiembre de 2013. Se ha entregado una versión anterior de la misma presentación en noruego Developers Conference (NDC) en junio de 2013: [NDC parte 1](http://vimeo.com/68215538), [NDC parte 2](http://vimeo.com/68215602).
- [Failsafe: creación de Cloud Services escalables y resistentes](https://channel9.msdn.com/Series/FailSafe). Series de vídeos de nueve partes de Ulrich Homann, Marc Mercuri y Mark SIMM. Presenta una vista de nivel 400 de cómo diseñar aplicaciones en la nube. Esta serie se centra en la teoría y los motivos de los patrones recomendados; para obtener más información sobre cómo hacerlo, consulte la creación de grandes series mediante Mark SIMM.
- [Building Big: lecciones aprendidas de clientes de Azure, parte 1](https://channel9.msdn.com/Events/Build/2012/3-029) y [parte 2](https://channel9.msdn.com/Events/Build/2012/3-030). Series de vídeos de dos partes de Simon Davies y Mark SIMM, similar a la serie FailSafe, pero orientadas más hacia la implementación práctica.

Ejemplo de código

- [La aplicación Fix it que acompaña a este libro electrónico](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4?cdn_id=2013-12-03-002).
- [Aspectos básicos del servicio en la nube C# en Azure en para Visual Studio 2012](https://aka.ms/csf). Proyecto descargable en el sitio de la galería de código de Microsoft, que incluye código y documentación desarrollados por el equipo de asesoramiento al cliente (CAT) de Microsoft. Muestra muchas de las prácticas recomendadas que se recomiendan en la serie de vídeos inseguros y la creación de grandes series de vídeos y las notas del producto de FailSafe. La página de la galería de código también incluye vínculos a la documentación ampliada por los autores del proyecto; vea especialmente el vínculo de la [colección wiki de aspectos básicos del servicio](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx) en la nube en el cuadro azul situado cerca de la parte superior de la descripción del proyecto. Este proyecto y la documentación del mismo se desarrollan activamente, lo que lo convierte en una mejor opción para la información sobre muchos temas que notas del producto similares pero anteriores.

Libros de copias impresas

- [Informática en la nube envenenada](https://www.amazon.com/dp/0470903562). Por Barrie Sosinsky.
- La [versión de lanzamiento diseña e implementa software listo para la producción](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213). Por Michael T. Nygard.
- [Patrones de arquitectura en la nube: uso de Microsoft Azure](http://shop.oreilly.com/product/0636920023777.do). Por Bill Wilder.
- [Plataforma de Windows Azure](https://www.amazon.com/dp/1430235632). Por Tejaswi Redkar.
- [Patrones de programación de Windows Azure para inicios](https://www.amazon.com/dp/1849685606). Por Riccardo Muti Becker.
- [Guía de desarrollo de Microsoft Windows Azure](https://www.amazon.com/dp/1849682224). De Neil Mackenzie.

Por último, cuando empiece a crear aplicaciones reales y a ejecutarlas en Azure, es probable que necesite ayuda de expertos. Puede formular preguntas en sitios de la comunidad, como [foros de Azure o stackoverflow](https://azure.microsoft.com/support/forums/), o puede ponerse en contacto directamente con Microsoft para obtener soporte técnico de Azure. Microsoft ofrece varios niveles de soporte técnico de Azure: para obtener un resumen y una comparación de las opciones, consulte [soporte técnico de Azure](https://azure.microsoft.com/support/plans/).

<a id="acknowledgments"></a>
## <a name="acknowledgments"></a>Agradecimientos

Este contenido fue escrito por Tom Dykstra, Rick Anderson y Mike Wasson. La mayor parte del contenido original procede de [Scott Guthrie](https://weblogs.asp.net/scottgu/)y, a su vez, se ha dibujado sobre el material de Mark SIMM y el equipo de asesoramiento al cliente (CAT) de Microsoft.

Muchos otros compañeros de Microsoft revisaron y comentaron los borradores y el código:

- Tim Ammann: revisó el capítulo de Automation.
- Christopher Bennage: revisar y probar el código de corrección.
- Ryan Berry: revisar el capítulo de CD/CI.
- Vittorio BERTOCCI: Revise el capítulo de SSO.
- Chris Ceballos: ayudó a resolver problemas técnicos en los scripts de PowerShell.
- Conor Cunningham: Revise el capítulo opciones de almacenamiento de datos.
- Carlos Farre: revisar y probar el código de corrección de problemas de seguridad.
- Larry Franks: revisó el capítulo sobre telemetría y supervisión.
- Jonathan Gao: revisó las secciones de Hadoop y MapReduce del capítulo opciones de almacenamiento de datos.
- Sidney Higa: revisar todos los capítulos.
- Gordon Hogenson: revisó el capítulo de control de código fuente.
- Tamra Myers: capítulos sobre opciones de almacenamiento de datos, blobs y colas.
- Pranav Rastogi: Revise el capítulo de SSO.
- Junio de Blendr González: se ha agregado el control de errores y la ayuda a los scripts de automatización de PowerShell.
- Mani Subramanian: revisar todos los capítulos y lideró el proceso de revisión y pruebas del código para el código de corrección de ti.
- Shaun Tinline-Jones: revisar el capítulo de particionamiento de datos.
- Selcin Tukarslan: capítulos revisados que cubren SQL Database y SQL Server.
- Edward Wu: código de ejemplo proporcionado para el capítulo SSO.
- Guang Yang: escribió los scripts de automatización de PowerShell.

Los miembros del [Consejo Asesor de instrucciones para desarrolladores de Microsoft](https://aka.ms/DGAC) (DGAC) también revisan y comentan los borradores:

- Jean-Luc Boucho
- Catalin Gheorghiu
- Wouter de Kort
- Carlos dos Santos
- Neil Mackenzie
- Dennis Persson
- Sunil Sabat
- [Aleksey Sinyagin](http://www.linkedin.com/in/sinyagin)
- Bill Wagner
- Michael madera

Otros miembros de DGAC revisaron y comentaron en el esquema preliminar:

- Damir Arh
- Edward Bakker
- Srdjan Bozovic
- Cam
- Gianni Rosa Gallina
- Paulo Morgado
- Jason Oliveira
- Alberto poblacion
- Ryan Riley
- Perez Jones Tsisah
- Roger Whitehead
- Pawel Wilkosz

> [!div class="step-by-step"]
> [Anterior](queue-centric-work-pattern.md)
> [Siguiente](the-fix-it-sample-application.md)
