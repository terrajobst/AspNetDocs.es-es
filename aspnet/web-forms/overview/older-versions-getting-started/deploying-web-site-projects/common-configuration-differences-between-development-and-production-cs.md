---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs
title: Diferencias de configuración comunes entre el desarrollo yC#la producción () | Microsoft Docs
author: rick-anderson
description: En los tutoriales anteriores hemos implementado nuestro sitio web mediante la copia de todos los archivos pertinentes desde el entorno de desarrollo en el entorno de producción. Pero...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 721a5c37-7e21-48e0-832e-535c6351dcae
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs
msc.type: authoredcontent
ms.openlocfilehash: 60379c87a8cf58b89066a6070ac659e65930b4fa
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78439015"
---
# <a name="common-configuration-differences-between-development-and-production-c"></a>Diferencias de configuración comunes entre el desarrollo y la producción (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial05_ConfigDifferences_cs.pdf)

> En los tutoriales anteriores hemos implementado nuestro sitio web mediante la copia de todos los archivos pertinentes desde el entorno de desarrollo en el entorno de producción. Sin embargo, no es raro que haya diferencias de configuración entre entornos, lo que requiere que cada entorno tenga un único archivo Web. config. En este tutorial se examinan las diferencias de configuración típicas y se examinan las estrategias para mantener información de configuración independiente.

## <a name="introduction"></a>Introducción

Los dos últimos tutoriales se han examinado a través de la implementación de una aplicación web simple. En el tutorial [*implementación del sitio mediante un cliente FTP*](deploying-your-site-using-an-ftp-client-cs.md) se ha mostrado cómo usar un cliente FTP independiente para copiar los archivos necesarios desde el entorno de desarrollo hasta la producción. En el tutorial anterior, [*implementación del sitio con Visual Studio*](deploying-your-site-using-visual-studio-cs.md), se examinó la implementación con la herramienta Copiar sitio web de Visual Studio y la opción publicar. En ambos tutoriales, cada archivo del entorno de producción era una copia de un archivo en el entorno de desarrollo. Sin embargo, no es raro que los archivos de configuración del entorno de producción difieran de los del entorno de desarrollo. La configuración de una aplicación web se almacena en el archivo `Web.config` y normalmente incluye información sobre los recursos externos, como los servidores de base de datos, Web y de correo electrónico. También deletrea el comportamiento de la aplicación en determinadas situaciones, como el curso de acción que se debe realizar cuando se produce una excepción no controlada.

Al implementar una aplicación web es importante que la información de configuración correcta termine en el entorno de producción. En la mayoría de los casos, el archivo de `Web.config` en el entorno de desarrollo no se puede copiar en el entorno de producción tal cual. En su lugar, es necesario cargar una versión personalizada de `Web.config` en producción. En este tutorial se revisan brevemente algunas de las diferencias de configuración más comunes. también se resumen algunas técnicas para mantener información de configuración diferente entre los entornos.

## <a name="typical-configuration-differences-between-the-development-and-production-environments"></a>Diferencias de configuración típicas entre los entornos de desarrollo y producción

El archivo de `Web.config` incluye una serie de información de configuración para una aplicación ASP.NET. Parte de esta información de configuración es la misma independientemente del entorno. Por ejemplo, la configuración de autenticación y las reglas de autorización de URL que se han escrito en los elementos `<authentication>` y `<authorization>` del archivo de `Web.config` suelen ser las mismas independientemente del entorno. Pero otra información de configuración, como información sobre los recursos externos, normalmente difiere en función del entorno.

Las cadenas de conexiones de base de datos son un ejemplo primo de información de configuración que difiere en función del entorno. Cuando una aplicación web se comunica con un servidor de base de datos, primero debe establecer una conexión y esto se consigue a través de una [cadena de conexión](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string). Aunque es posible codificar la cadena de conexión de base de datos directamente en las páginas web o en el código que se conecta a la base de datos, es mejor colocarla `Web.config`[elemento`<connectionStrings>`](https://msdn.microsoft.com/library/bf7sd233.aspx) para que la información de la cadena de conexión esté en una única ubicación centralizada. A menudo, se utiliza una base de datos diferente durante el desarrollo que se utiliza en producción. por consiguiente, la información de la cadena de conexión debe ser única para cada entorno.

> [!NOTE]
> Los futuros tutoriales exploran la implementación de aplicaciones controladas por datos, en cuyo momento profundizaremos en los detalles sobre cómo se almacenan las cadenas de conexión de base de datos en el archivo de configuración.

El comportamiento previsto de los entornos de desarrollo y producción difiere considerablemente. Un pequeño grupo de desarrolladores está creando, probando y depurando una aplicación web en el entorno de desarrollo. En el entorno de producción, la misma aplicación la están visitando muchos usuarios simultáneos diferentes. ASP.NET incluye una serie de características que ayudan a los desarrolladores a probar y depurar una aplicación, pero estas características deben deshabilitarse por motivos de seguridad y rendimiento en el entorno de producción. Echemos un vistazo a algunas de estas opciones de configuración.

### <a name="configuration-settings-that-impact-performance"></a>Opciones de configuración que afectan al rendimiento

Cuando se visita una página ASP.NET por primera vez (o la primera vez que ha cambiado), su marcado declarativo se debe convertir en una clase y se debe compilar esta clase. Si la aplicación Web usa la compilación automática, la clase de código subyacente de la página también debe compilarse. Puede configurar una serie de opciones de compilación a través del [elemento`<compilation>`](https://msdn.microsoft.com/library/s10awwz0.aspx)del archivo de `Web.config`.

El atributo Debug es uno de los atributos más importantes en el elemento `<compilation>`. Si el atributo `debug` está establecido en "true", los ensamblados compilados incluyen símbolos de depuración, que son necesarios al depurar una aplicación en Visual Studio. Sin embargo, los símbolos de depuración aumentan el tamaño del ensamblado e imponen requisitos de memoria adicionales al ejecutar el código. Además, cuando el atributo `debug` se establece en "true", el contenido devuelto por `WebResource.axd` no se almacena en caché, lo que significa que cada vez que un usuario visita una página, necesitará volver a descargar el contenido estático que devuelve `WebResource.axd`.

> [!NOTE]
> `WebResource.axd` es un controlador HTTP integrado incluido en ASP.NET 2,0 que los controles de servidor usan para recuperar recursos incrustados, como archivos de scripts, imágenes, archivos CSS y otro tipo de contenido. Para obtener más información sobre cómo funciona `WebResource.axd` y cómo puede usarlo para tener acceso a los recursos insertados desde los controles de servidor personalizados, consulte [acceso a los recursos insertados a través de una dirección URL mediante `WebResource.axd`](http://aspnet.4guysfromrolla.com/articles/080906-1.aspx).

Normalmente, el atributo `debug` del elemento `<compilation>` se establece en "true" en el entorno de desarrollo. De hecho, este atributo debe establecerse en "true" para depurar una aplicación Web; Si intenta depurar una aplicación de ASP.NET desde Visual Studio y el atributo `debug` está establecido en "false", Visual Studio mostrará un mensaje que explica que no se puede depurar la aplicación hasta que el atributo de `debug` se establezca en "true" y le proporcionará el cambio.

**Nunca** debe tener el atributo `debug` establecido en "true" en un entorno de producción debido a su impacto en el rendimiento. Para obtener una explicación más detallada sobre este tema, consulte la entrada de blog de [Scott Guthrie](https://weblogs.asp.net/scottgu/), [no ejecute aplicaciones de ASP.net de producción con `debug="true"` habilitado](https://weblogs.asp.net/scottgu/442448).

### <a name="custom-errors-and-tracing"></a>Errores y seguimiento personalizados

Cuando se produce una excepción no controlada en una aplicación ASP.NET, se propaga hasta el tiempo de ejecución en el que se produce una de tres cosas:

- Se muestra un mensaje de error genérico en tiempo de ejecución. Esta página informa al usuario de que se ha producido un error de tiempo de ejecución, pero no proporciona detalles sobre el error.
- Se muestra un mensaje de detalles de excepción, que incluye información sobre la excepción que se acaba de iniciar.
- Se muestra una página de error personalizada, que es una página ASP.NET que se crea y que muestra cualquier mensaje que se desee.

Lo que sucede en el caso de una excepción no controlada depende de la [sección`<customErrors>`](https://msdn.microsoft.com/library/h0hfz6fc.aspx)del archivo de `Web.config`.

Al desarrollar y probar una aplicación, ayuda a ver los detalles de cualquier excepción en el explorador. Sin embargo, mostrar los detalles de la excepción en una aplicación en producción es un riesgo de seguridad potencial. Además, es inestable y hace que su sitio web parezca no profesional. Idealmente, en el caso de una excepción no controlada, una aplicación web en el entorno de desarrollo mostrará los detalles de la excepción, mientras que la misma aplicación en producción mostrará una página de error personalizada.

> [!NOTE]
> La configuración de la sección `<customErrors>` predeterminada muestra el mensaje de detalles de la excepción solo cuando la página se está visitando a través de localhost y muestra la página de error de tiempo de ejecución genérica en caso contrario. Esto no es lo ideal, pero es seguro saber que el comportamiento predeterminado no revela los detalles de la excepción a los visitantes no locales. En un futuro tutorial se examina la sección `<customErrors>` con más detalle y se muestra cómo se muestra una página de error personalizada cuando se produce un error en producción.

Otra característica de ASP.NET que resulta útil durante el desarrollo es el seguimiento. La traza, si está habilitada, registra información sobre cada solicitud entrante y proporciona una página web especial, `Trace.axd`, para ver los detalles de las solicitudes recientes. Puede activar y configurar el seguimiento a través del [elemento`<trace>`](https://msdn.microsoft.com/library/6915t83k.aspx) en `Web.config`.

Si habilita el seguimiento, asegúrese de que está deshabilitado en producción. Dado que la información de seguimiento incluye cookies, datos de sesión y otra información potencialmente confidencial, es importante deshabilitar el seguimiento en producción. La buena noticia es que, de forma predeterminada, el seguimiento está deshabilitado y el `Trace.axd` archivo solo es accesible a través de localhost. Si cambia estos valores predeterminados en desarrollo, asegúrese de que se desactivan en producción.

## <a name="techniques-for-maintaining-separate-configuration-information"></a>Técnicas para mantener información de configuración independiente

Tener valores de configuración diferentes en los entornos de desarrollo y producción complica el proceso de implementación. En los dos tutoriales anteriores, el proceso de implementación implicaba copiar todos los archivos necesarios desde el desarrollo hasta el entorno de producción, pero ese enfoque solo funciona si la información de configuración es la misma en ambos entornos. Hay varias técnicas para implementar una aplicación con información de configuración variable. Vamos a catalogar algunas de estas opciones para las aplicaciones web hospedadas.

### <a name="manually-deploying-the-production-environment-configuration-file"></a>Implementar manualmente el archivo de configuración del entorno de producción

El enfoque más sencillo consiste en mantener dos versiones del archivo `Web.config`: una para el entorno de desarrollo y otra para el entorno de producción. La implementación de un sitio en producción implica copiar todos los archivos en el servidor de producción en el entorno de desarrollo, *excepto* el archivo de `Web.config`. En su lugar, el archivo de `Web.config` específico del entorno de producción se copiará en producción.

Este enfoque no es muy sofisticado, pero es fácil de implementar porque la información de configuración cambia con poca frecuencia. Funciona mejor para las aplicaciones con un pequeño equipo de desarrollo que se hospedan en un único servidor Web y cuya información de configuración se modifica con poca frecuencia. Es más tenable cuando se implementan manualmente los archivos de aplicación mediante un cliente FTP independiente. Al usar la herramienta Copiar sitio web de Visual Studio o la opción publicar, deberá cambiar primero el archivo de `Web.config` específico de la implementación con el de la producción específica de la plataforma antes de la implementación y, a continuación, cambiarlos de nuevo una vez completada la implementación.

### <a name="change-the-configuration-during-the-build-or-deployment-process"></a>Cambiar la configuración durante el proceso de compilación o implementación

Hasta ahora, las discusiones han asumido un proceso de compilación e implementación ad hoc. Muchos proyectos de software de mayor tamaño tienen procesos más formalizados que hacen uso de herramientas de código abierto, de crecimiento doméstico o de terceros. Para estos proyectos, es probable que pueda personalizar el proceso de compilación o implementación para modificar correctamente la información de configuración antes de que se inserte en producción. Si compila la aplicación web con [msbuild](http://en.wikipedia.org/wiki/MSBuild), [NAnt](http://nant.sourceforge.net/)o alguna otra herramienta de compilación, es probable que agregue un paso de compilación para modificar el archivo de `Web.config` para que incluya la configuración específica de producción. O bien, el flujo de trabajo de implementación podría conectarse mediante programación al servidor de control de código fuente y recuperar el archivo de `Web.config` adecuado.

El enfoque real para obtener la información de configuración adecuada en producción varía considerablemente en función de las herramientas y el flujo de trabajo. Como tal, no profundizaremos más en este tema. Si usa una herramienta de compilación conocida como MSBuild o NAnt, puede encontrar artículos y tutoriales de implementación específicos de estas herramientas a través de una búsqueda web.

### <a name="managing-configuration-differences-via-the-web-deployment-project-add-in"></a>Administrar las diferencias de configuración a través del complemento de proyecto de implementación web

En 2006 Microsoft lanzó el complemento de proyecto de desarrollo web para Visual Studio 2005. Se publicó un complemento para Visual Studio 2008 en 2008. Este complemento permite a los desarrolladores de ASP.NET crear un proyecto de implementación web independiente junto con su proyecto de aplicación web que, cuando se compila, compila explícitamente la aplicación web y copia los archivos para implementarlos en un directorio de salida local. El proyecto de aplicación Web usa MSBuild en segundo plano.

De forma predeterminada, el archivo de `Web.config` del entorno de desarrollo se copia en el directorio de salida, pero puede configurar el proyecto de implementación web para personalizar el

información de configuración que se copia en este directorio de las maneras siguientes:

- A través de `Web.config` sustitución de la sección de archivo, en la que se especifica la sección que se va a reemplazar y un archivo XML que contiene el texto de reemplazo.
- Al proporcionar una ruta de acceso a un archivo de origen de configuración externo. Con esta opción seleccionada, el proyecto de implementación web copia un archivo de `Web.config` determinado en el directorio de salida (en lugar del archivo `Web.config` que se usa en el entorno de desarrollo).
- Agregando reglas personalizadas al archivo de MSBuild usado por el proyecto de implementación web.

Para implementar la aplicación Web, compile el proyecto de implementación web y, a continuación, copie los archivos de la carpeta de salida del proyecto en el entorno de producción.

Para obtener más información sobre el uso del proyecto de implementación web, consulte [este artículo de proyectos de implementación web](https://msdn.microsoft.com/magazine/cc163448.aspx) del problema de abril de 2007 de [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx), o consulte los vínculos en la sección lecturas adicionales al final de este tutorial.

> [!NOTE]
> No se puede usar el proyecto de implementación web con Visual Web Developer porque el proyecto de implementación web se implementa como un complemento de Visual Studio y las ediciones de Visual Studio Express (incluido Visual Web Developer) no admiten complementos.

## <a name="summary"></a>Resumen

Los recursos externos y el comportamiento de una aplicación web en desarrollo suelen ser diferentes de lo que ocurre cuando la misma aplicación está en producción. Por ejemplo, las cadenas de conexión a bases de datos, las opciones de compilación y el comportamiento cuando se produce una excepción no controlada normalmente difieren entre los entornos. El proceso de implementación debe dar cabida a estas diferencias. Como se explicó en este tutorial, el enfoque más sencillo consiste en copiar manualmente un archivo de configuración alternativo en el entorno de producción. Son posibles soluciones más elegantes cuando se usa el complemento de proyecto de implementación web o con un proceso de compilación o implementación más formalizado que pueda acomodar dichas personalizaciones.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Explicación de las cadenas de conexión](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)
- [Cadenas de conexión de base de datos @ ConnectionStrings.com](http://www.connectionstrings.com/)
- [No ejecute aplicaciones de ASP.NET de producción con `debug="true"` habilitado](https://weblogs.asp.net/scottgu/Don_1920_t-run-production-ASP.NET-Applications-with-debug_3D001D20_true_1D20_-enabled)
- [Responder correctamente a las excepciones no controladas: mostrar páginas de errores descriptivos](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Cómo: usar un proyecto de implementación web de Visual Studio 2008](../../../videos/how-do-i/how-do-i-use-a-visual-studio-2008-web-deployment-project.md)
- [Opciones de configuración de claves al implementar una base de datos](http://aspnet.4guysfromrolla.com/articles/121008-1.aspx)
- [Proyectos de implementación web de Visual studio 2008 descargar](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) | [Visual Studio 2005 proyectos de implementación web descargar](https://download.microsoft.com/download/9/4/9/9496adc4-574e-4043-bb70-bc841e27f13c/WebDeploymentSetup.msi)
- [Vs 2008 proyectos de implementación web](https://weblogs.asp.net/scottgu/archive/2005/11/06/429723.aspx) | se ha [publicado compatibilidad con proyectos de implementación web de vs 2008](https://weblogs.asp.net/scottgu/archive/2008/01/28/vs-2008-web-deployment-project-support-released.aspx)
- [Proyectos de implementación web](https://msdn.microsoft.com/magazine/cc163448.aspx)

> [!div class="step-by-step"]
> [Anterior](deploying-your-site-using-visual-studio-cs.md)
> [Siguiente](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
