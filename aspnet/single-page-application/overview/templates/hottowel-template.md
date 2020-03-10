---
uid: single-page-application/overview/templates/hottowel-template
title: Plantilla de paño de acceso frecuente | Microsoft Docs
author: madskristensen
description: Plantilla HotTowel
ms.author: riande
ms.date: 02/09/2013
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: eeab69e75546791978bb09d7823d95caf9dca1a0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467143"
---
# <a name="hot-towel-template"></a>Plantilla de Hot Towel

por [Mads Kristensen](https://github.com/madskristensen)

> La plantilla MVC de webtoallas está escrita por John Papa
> 
> Elija la versión que desea descargar:
> 
> [Plantilla MVC de webtoallas para Visual Studio 2012](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [Plantilla MVC de webtoallas para Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> El paño activo: porque no desea ir al SPA sin ninguno.

¿Quiere crear un SPA pero no puede decidir por dónde empezar? Use el paño activo y, en segundos, tendrá un SPA y todas las herramientas que necesita para compilar.

El paño candente crea un excelente punto de partida para crear una aplicación de una sola página (SPA) con ASP.NET. De forma avanzada, proporciona una estructura modular para el código, ver la navegación, el enlace de datos, la administración de datos enriquecida y el estilo sencillo pero elegante. El paño de acceso frecuente proporciona todo lo que necesita para crear un SPA, de modo que pueda centrarse en la aplicación, no en el establecimiento.

> Obtenga más información sobre la creación de un SPA a partir de [vídeos, tutoriales y cursos de Pluralsight de John Papa](http://johnpapa.net/spa?vsix).

## <a name="application-structure"></a>Estructura de aplicación

El formato de WebPhone SPA proporciona una carpeta de aplicación que contiene los archivos JavaScript y HTML que definen su aplicación.

Dentro de la carpeta de la aplicación:

- Durandal
- servicios
- ViewModels
- vistas

La carpeta de la aplicación contiene una colección de módulos. Estos módulos encapsulan la funcionalidad y declaran las dependencias en otros módulos. La carpeta views contiene el código HTML de la aplicación y la carpeta ViewModels contiene la lógica de presentación para las vistas (un patrón MVVM común). La carpeta servicios es ideal para alojar cualquier servicio común que la aplicación pueda necesitar, como la recuperación de datos HTTP o la interacción del almacenamiento local. Es común que varios ViewModels vuelvan a usar el código de los módulos de servicio.

## <a name="aspnet-mvc-server-side-application-structure"></a>Estructura de la aplicación del lado servidor ASP.NET MVC

El paño activo se basa en la conocida y eficaz estructura de ASP.NET MVC.

- Inicio de\_de la aplicación
- Contenido
- Controladores
- Modelos
- Scripts
- Vistas

## <a name="featured-libraries"></a>Bibliotecas destacadas

- ASP.NET MVC
- ASP.NET Web API
- Optimización web de ASP.NET: Unión y minificación
- [Multiplataforma: administración de datos](http://Breezejs.com) enriquecida
- [Durandal. js](http://Durandaljs.com) : navegación y visualización de la composición
- [Knockout. js](http://Knockoutjs.com) : enlaces de datos
- Requerir la modularidad de [. js](http://requirejs.org) con AMD y optimización
- [Tostador. js](http://jpapa.me/c7toastr) : mensajes emergentes
- [Bootstrap de Twitter](https://twitter.github.com/bootstrap/) : estilo de CSS sólido

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a>Instalación a través de la plantilla de Visual Studio 2012 Hot paño SPA

El paño activo se puede instalar como una plantilla de Visual Studio 2012. Simplemente haga clic en `File` | `New Project` y elija `ASP.NET MVC 4 Web Application`. A continuación, seleccione la plantilla "aplicación de una sola página de webtoallas" y ejecute.

## <a name="installing-via-the-nuget-package"></a>Instalación mediante el paquete NuGet

El paño de acceso frecuente es también un paquete de NuGet que aumenta un proyecto de MVC de ASP.NET vacío existente. Solo tiene que instalar con Nuget y, después, ejecutar.

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a>¿Cómo puedo compilar en el paño activo?

Simplemente comience a agregar código.

1. Agregue su propio código del lado servidor, preferiblemente Entity Framework y WebAPI (que realmente destaca con el. js)
2. Agregar vistas a la carpeta `App/views`
3. Agregar ViewModels a la carpeta `App/viewmodels`
4. Agregar enlaces de datos HTML y knockout a las nuevas vistas
5. Actualice las rutas de navegación en `shell.js`

## <a name="walkthrough-of-the-htmljavascript"></a>Tutorial de HTML/JavaScript

### <a name="viewshottowelindexcshtml"></a>Views/HotTowel/index.cshtml

index. cshtml es la ruta de inicio y la vista de la aplicación MVC. Contiene todas las etiquetas meta estándar, vínculos CSS y referencias de JavaScript que cabría esperar. El cuerpo contiene una sola `<div>` que es donde se colocará todo el contenido (las vistas) cuando se soliciten. En el `@Scripts.Render` se usa require. js para ejecutar el punto de entrada del código de la aplicación, que se encuentra en el archivo de `main.js`. Se proporciona una pantalla de presentación para mostrar cómo crear una pantalla de presentación mientras se carga la aplicación.

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a>App/Main. js

El archivo de `main.js` contiene el código que se ejecutará en cuanto se cargue la aplicación. Aquí es donde desea definir las rutas de navegación, establecer las vistas de inicio y realizar cualquier instalación o arranque, como desbloquear los datos de la aplicación.

El archivo `main.js` define algunos de los módulos de Durandal para ayudar a iniciar la aplicación. La instrucción define ayuda a resolver las dependencias de los módulos para que estén disponibles para la función. En primer lugar, los mensajes de depuración están habilitados, que envían mensajes sobre qué eventos realiza la aplicación en la ventana de la consola. El código app. Start indica a Durandal Framework que inicie la aplicación. Las convenciones se establecen para que Durandal sepa todas las vistas y ViewModels se encuentran en las mismas carpetas con nombre, respectivamente. Por último, el `app.setRoot` inicia la carga del `shell` mediante una animación de `entrance` predefinida.

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a>Vistas

Las vistas se encuentran en la carpeta `App/views`.

### <a name="shellhtml"></a>shell.html

El `shell.html` contiene el diseño maestro para el código HTML. Todas las demás vistas se crearán en algún lugar de la vista de `shell`. El paño de acceso frecuente proporciona una `shell` con tres regiones de este tipo: un encabezado, un área de contenido y un pie de página. Cada una de estas regiones se carga con contenido que forma otras vistas cuando se solicita.

Los enlaces de `compose` para el encabezado y el pie de página están codificados de forma rígida en el paño activo para que apunten a las vistas `nav` y `footer`, respectivamente. El enlace de redacción de la sección `#content` se enlaza al elemento activo del módulo de `router`. En otras palabras, al hacer clic en un vínculo de navegación, su vista correspondiente se carga en esta área.

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a>nav.html

El `nav.html` contiene los vínculos de navegación para el SPA. Aquí es donde se puede colocar la estructura de menú, por ejemplo. A menudo, esto está enlazado a datos (mediante Knockout) al módulo `router` para mostrar la navegación que ha definido en el `shell.js`. El knockout busca los atributos de enlace de datos y los enlaza al `shell` ViewModel para mostrar las rutas de navegación y mostrar un componente ProgressBar (mediante Twitter bootstrap) si el módulo de `router` está en medio de navegar de una vista a otra (vea `router.isNavigating`).

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a>Home. html y details. html

Estas vistas contienen HTML para las vistas personalizadas. Cuando se hace clic en el vínculo `home` del menú de la vista de `nav`, la vista de `home` se colocará en el área de contenido de la vista de `shell`. Estas vistas se pueden aumentar o reemplazar por sus propias vistas personalizadas.

### <a name="footerhtml"></a>footer.html

El `footer.html` contiene el código HTML que aparece en el pie de página, en la parte inferior de la vista de `shell`.

## <a name="viewmodels"></a>ViewModels

ViewModels se encuentran en la carpeta `App/viewmodels`.

### <a name="shelljs"></a>shell.js

El `shell` ViewModel contiene propiedades y funciones que se enlazan a la vista de `shell`. A menudo, aquí es donde se encuentran los enlaces de navegación del menú (vea la lógica de `router.mapNav`).

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a>Home. js y details. js

Estos ViewModels contienen las propiedades y las funciones que se enlazan a la vista `home`. también contiene la lógica de presentación de la vista y es el pegado entre los datos y la vista.

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a>Servicios

Los servicios se encuentran en la carpeta app/Services. Idealmente, los servicios futuros, como un módulo de servicio de datos, que es responsable de la obtención y publicación de datos remotos, podrían colocarse.

### <a name="loggerjs"></a>logger.js

El paño de acceso frecuente proporciona un módulo de `logger` en la carpeta servicios. El módulo `logger` es ideal para registrar mensajes en la consola y en el usuario en elementos emergentes emergentes.
