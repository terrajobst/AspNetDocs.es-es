---
ms.openlocfilehash: eb2f83504129ae2b19ff5d85a2bc7d90293b43d6
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57043402"
---
* Startup.cs: [Clase de inicio](xref:fundamentals/startup) -clase configura la canalización de solicitudes que controla todas las solicitudes realizadas a la aplicación.
* Program.cs: [Clase de programa](xref:fundamentals/index) que contiene el punto de entrada principal de la aplicación.
* firstapp.csproj : [Archivo de proyecto](/dotnet/articles/core/preview3/tools/csproj) formato de archivo de proyecto de MSBuild para aplicaciones de ASP.NET Core. Contiene referencias entre proyectos, referencias de NuGet y otros elementos relacionados del proyecto.
* appSettings.JSON / appsettings. Development.JSON: Archivo de configuración de configuración de aplicaciones de base de entorno. [Ver configuración](xref:fundamentals/configuration/index).
* bower.JSON: Dependencias de paquetes de bower para el proyecto.
* .bowerrc : Archivo de configuración de bower que define dónde instalar los componentes cuando los recursos de descarga de Bower.
* bundleconfig.JSON: archivos de configuración para agrupar y minificar recursos front-end de JavaScript y CSS.
* Vistas: Contiene las vistas de Razor. Las vistas son los componentes que muestran la interfaz de usuario (IU) de la aplicación. Por lo general, esta interfaz de usuario muestra los datos del modelo.
* Controladores: Contiene los controladores de MVC, inicialmente *HomeController.cs*. Los controladores son clases que controlan las solicitudes del explorador.
* wwwroot : Carpeta de raíz de la aplicación Web.

Para obtener más información, consulte [el modelo MVC](xref:mvc/overview).
