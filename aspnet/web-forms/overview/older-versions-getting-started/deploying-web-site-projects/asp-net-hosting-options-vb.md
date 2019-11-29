---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-vb
title: Opciones de hospedaje de ASP.NET (VB) | Microsoft Docs
author: rick-anderson
description: Las aplicaciones Web de ASP.NET suelen diseñarse, crearse y probarse en un entorno de desarrollo local y deben implementarse en un entorno de producción o...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 492f5ae2-bad7-4107-89a9-f04a9525dee7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-vb
msc.type: authoredcontent
ms.openlocfilehash: 4293114002aee15ddace1a3f19c240f35e6065f5
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74620755"
---
# <a name="aspnet-hosting-options-vb"></a>Opciones de hospedaje de ASP.NET (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial01_Basics_vb.pdf)

> Las aplicaciones Web de ASP.NET suelen diseñarse, crearse y probarse en un entorno de desarrollo local y deben implementarse en un entorno de producción una vez que esté listo para su lanzamiento. En este tutorial se proporciona información general de alto nivel sobre el proceso de implementación y sirve como introducción a esta serie de tutoriales.

## <a name="introduction"></a>Introducción

Las aplicaciones web suelen diseñarse, crearse y probarse en un entorno de desarrollo al que solo pueden acceder los programadores que trabajan en el sitio. Una vez que la aplicación está lista para su lanzamiento, se mueve a un entorno de producción donde cualquier usuario de Internet puede tener acceso al sitio. Este proceso de implementación presenta una serie de desafíos:

- Debe existir un entorno de producción y estar configurado correctamente antes de que se pueda implementar una aplicación ASP.NET. Además, el entorno de producción debe mantenerse actualizado con las revisiones de seguridad más recientes.
- El conjunto correcto de archivos de marcado, archivos de código y archivos auxiliares debe copiarse desde el entorno de desarrollo al entorno de producción. En el caso de las aplicaciones controladas por datos, esto podría requerir también copiar el esquema y los datos de la base de datos.
- Puede haber diferencias de configuración entre los dos entornos. La cadena de conexión de base de datos o el servidor de correo electrónico usado en el entorno de desarrollo probablemente serán diferentes del entorno de producción. Lo que es más, el comportamiento de la aplicación puede depender del entorno. Por ejemplo, cuando se produce un error en el desarrollo, los detalles del error se pueden mostrar en la pantalla, pero cuando se produce un error en producción, se debería mostrar una página de error fácil de usar en su lugar y los detalles del error se enviarán por correo electrónico a los desarrolladores.

Para obviar el primer desafío: la configuración y el mantenimiento de un entorno de producción, muchas personas y empresas externalizan sus entornos de producción a *proveedores de hospedaje web*. Un proveedor de hospedaje web es una empresa que administra el entorno de producción en su nombre. Hay innumerables proveedores de hosts Web, cada uno con distintos precios y niveles de servicio. Vea la sección "buscar un proveedor de hosts Web" para obtener sugerencias sobre cómo buscar un proveedor de servicios de este tipo.

Este es el primero de una serie de tutoriales que examinan los pasos implicados en la implementación de una aplicación Web ASP.NET en un entorno de producción administrado por un proveedor de host Web. En el transcurso de estos tutoriales, examinaremos:

- Qué archivos se deben implementar en el proveedor de host Web.
- Herramientas para simplificar el proceso de implementación.
- Cómo implementar una base de datos.
- Sugerencias para implementar una base de datos que usa [el proveedor de pertenencia y roles basado en SQL](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md), además de maneras de imitar la herramienta de administración de sitios web en un entorno de producción.
- Estrategias para actualizar sin problemas la base de datos de producción con los cambios realizados durante el desarrollo.
- Técnicas para registrar los errores que se producen en producción y formas de notificar a los desarrolladores cuando se produce un error.

Estos tutoriales están orientados a ser concisos y proporcionan instrucciones paso a paso con muchas capturas de pantalla para guiarle en el proceso visualmente. En este tutorial de inaugural se proporciona información general sobre el proceso de implementación de ASP.NET y consejos sobre cómo buscar un proveedor de hospedaje Web. Comencemos.

## <a name="an-overview-of-the-aspnet-deployment-process"></a>Información general sobre el proceso de implementación de ASP.NET

En Resumen, la implementación de una aplicación de ASP.NET consta de los tres pasos siguientes:

1. Configurar la aplicación Web, el servidor Web y la base de datos en el entorno de producción.
2. Sincronice las páginas ASP.NET, los archivos de código, los ensamblados de la carpeta `Bin` y los archivos de compatibilidad relacionados con HTML, como los archivos CSS y JavaScript.
3. Sincronizar el esquema de la base de datos o los datos.

La información de configuración de una aplicación web se encuentra normalmente en el archivo `Web.config` e incluye cadenas de conexión de base de datos, criterios de control de errores, reglas de reescritura de direcciones URL e información del servidor de correo electrónico. A menudo, esta información es diferente para una aplicación en desarrollo frente a la misma aplicación en producción. Por ejemplo, al desarrollar una aplicación, es mejor usar una base de datos de desarrollo para que no realice pruebas en la base de datos de producción. Como resultado, las cadenas de conexión de base de datos suelen diferir entre las aplicaciones de desarrollo y de producción. Debido a estas diferencias, parte de la implementación implica realizar cambios en la información de configuración de la aplicación Web.

Además de los cambios de configuración de la aplicación Web, el paso 1 también puede implicar la configuración para el servidor Web y la base de datos. Por ejemplo, si una página de ASP.NET crea o elimina archivos de un directorio en el servidor Web, el servidor Web debe configurarse para permitir las modificaciones del sistema de archivos. Del mismo modo, es posible que haya configuraciones de permisos o de autenticación que deban realizarse en la base de datos.

El paso 2 implica la sincronización del conjunto de páginas esenciales de ASP.NET y los archivos auxiliares entre los entornos de desarrollo y de producción. El conjunto determinado de archivos relacionados con ASP.NET que se deben sincronizar entre los dos entornos depende del tipo de proyecto creado en Visual Studio, y es la discusión en el siguiente tutorial, <em>[que determina qué archivos se deben implementar](determining-what-files-need-to-be-deployed-vb.md)</em>. El tercer y cuarto tutoriales: <em>[implementar el sitio mediante FTP](deploying-your-site-using-an-ftp-client-vb.md)</em>e <em>[implementar el sitio con Visual Studio](deploying-your-site-using-visual-studio-vb.md)</em> : Examine las distintas herramientas y técnicas para sincronizar estos archivos.

Al compilar aplicaciones controladas por datos, normalmente se usan dos bases de datos: una para el desarrollo y otra en producción. Durante el desarrollo, el esquema de la base de datos de desarrollo se puede modificar para incluir nuevas tablas, columnas, procedimientos almacenados y desencadenadores, o bien se puede modificar para quitar o cambiar el nombre de los objetos de base de datos existentes. Entre el momento en que se realizan estos cambios y el momento en que se implementa la aplicación en producción, las bases de datos de desarrollo y producción no están sincronizadas. Este asincronía debe corregirse durante el proceso de implementación. Estos desafíos se examinarán en futuros tutoriales.

## <a name="finding-a-web-host-provider"></a>Buscar un proveedor de hosts Web

Las aplicaciones de ASP.NET se pueden implementar en cualquier servidor Web que tenga instalado el .NET Framework y Internet Information Services (IIS). Podría hospedar un sitio de su equipo personal, suponiendo que tuviera una conexión de banda ancha a Internet y que sepa cómo configurar el enrutador para permitir las solicitudes Web entrantes. También puede hospedar un sitio de un equipo en una intranet, tantas empresas como hagan. Sin embargo, el enfoque de estos tutoriales es hospedar el sitio web con un proveedor de host Web.

> [!NOTE]
> [IIS](https://www.iis.net/) es el servidor Web de clase empresarial de Microsoft. Se incluye con las ediciones no domésticas de Windows, como Windows Server 2008 y ciertas ediciones de Windows Vista. No es necesario instalar IIS para dar servicio a las aplicaciones de ASP.NET en un entorno de desarrollo, ya que Visual Studio incluye el servidor Web de desarrollo de ASP.NET. Sin embargo, el servidor Web de desarrollo de ASP.NET solo acepta conexiones locales y, por lo tanto, no se puede usar en un entorno de producción.

Antes de implementar el sitio en un proveedor de hosts Web, primero debe decidir con qué empresa hacer negocios. Hay innumerables empresas de hospedaje web en Marketplace; una búsqueda de "empresa de hospedaje web" devuelve más de 5 millones resultados. ¿Cómo se encuentra el más adecuado para usted? Su motor de búsqueda favorito es un buen punto de partida, como los sitios web como los [hosts](http://www.tophosts.com/) y [HostCritique](http://www.hostcritique.net/), que comparan y contrastan los distintos servicios de hospedaje. También aconseja preguntar a sus colegas y compañeros de trabajo para conocer las recomendaciones; también puede solicitar recomendaciones en el foro de [hospedaje abierto](https://forums.asp.net/158.aspx) aquí, en los [foros de ASP.net](https://forums.asp.net/).

Las compañías de hospedaje web suelen ofrecer planes de hospedaje compartidos y planes de hospedaje dedicados. Con el hospedaje compartido, un servidor Web único hospeda docenas si no tiene cientos de sitios web diferentes. Con el hospedaje dedicado, conceda un equipo de la compañía que sirva a su sitio y a su sitio. Un plan de hospedaje compartido podría incluir compatibilidad con páginas de ASP.NET, la capacidad de trabajar con bases de datos de Microsoft Access, 5 GB de espacio en disco y 100 GB de tráfico de ancho de banda mensual durante $9,95 al mes. Otro plan de hospedaje compartido podría incluir la compatibilidad con páginas de ASP.NET, el acceso al servidor de base de datos Microsoft SQL Server 2008, 10 GB de espacio en disco y 250 GB de tráfico mensual de ancho de banda durante $19,95 al mes. Los planes de hospedaje dedicados suelen ser mucho más caros, cuestan varios cientos de dólares al mes, pero ofrecen un mejor rendimiento y un mayor control que las opciones de hospedaje compartido. El plan que elija dependerá de su presupuesto, de cuánto tráfico recibe el sitio web y de las características que espera que necesite.

Dos consideraciones importantes a la hora de elegir un proveedor de host web son el servicio de atención al cliente y la calidad del servicio. Si tiene alguna pregunta o un problema de configuración, ¿cuánto tiempo se tarda en enviar el problema al Departamento de soporte técnico del host web hasta que reciba una respuesta? ¿Qué fiabilidad tienen los servicios de la compañía? ¿Suelen tener interrupciones en la base de datos? ¿Con qué frecuencia se desconecta su servidor de correo electrónico? Siempre puede solicitar a una empresa que proporcione detalles sobre su tiempo de actividad e informará sobre su política de atención al cliente, pero una manera más SureFire es solicitar los comentarios de los clientes actuales y anteriores, que puede hacer a través de los foros en línea, los grupos de noticias y el correo electrónico listservs .

> [!NOTE]
> Algunas empresas de hospedaje web centran su negocio en una determinada pila tecnológica, como .NET o [LAMP](http://en.wikipedia.org/wiki/LAMP_stack) (**L** INUX **, Pache** , **M** ySQL y **P** HP), por lo que debe asegurarse de que la empresa que seleccione hospede aplicaciones ASP.net. Compruebe también para asegurarse de que admiten la versión de ASP.NET que está usando para compilar la aplicación. Y si va a compilar una aplicación controlada por datos, asegúrese de que el host web ofrece el mismo servidor de base de datos y la misma versión que está usando.

## <a name="summary"></a>Resumen

Las aplicaciones Web de ASP.NET suelen diseñarse, crearse y probarse en un entorno de desarrollo local. Una vez que una versión está lista para su lanzamiento, se mueve a un entorno de producción. Aunque es posible hospedar sitios web de ASP.NET en su equipo personal o en servidores de la empresa, muchas empresas y usuarios optan por externalizar su hospedaje a un proveedor de host Web.

En esta serie de tutoriales se examinan los pasos para implementar una aplicación de ASP.NET en un proveedor de host web y se exploran los desafíos más comunes. En este tutorial se ofrece información general de alto nivel sobre el proceso de implementación de ASP.NET y se ofrecen sugerencias para buscar un proveedor de hosts web adecuado. En el siguiente tutorial se examina qué archivos relacionados con ASP.NET deben copiarse en el entorno de producción al implementar el sitio Web.

¡ Feliz programación!

### <a name="special-thanks-to"></a>Agradecimiento especial...

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor responsable de este tutorial era Teresa Murphy. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](users-and-roles-on-the-production-website-cs.md)
> [Siguiente](determining-what-files-need-to-be-deployed-vb.md)
