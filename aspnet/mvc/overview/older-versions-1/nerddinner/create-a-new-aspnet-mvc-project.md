---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Crear un nuevo proyecto de ASP.NET MVC | Microsoft Docs
author: microsoft
description: En el paso 1 se muestra cómo poner en marcha la estructura de la aplicación NerdDinner básica.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 189ddc187fc83db14106b2da199ba12a70a32b45
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78469243"
---
# <a name="create-a-new-aspnet-mvc-project"></a>Crear un proyecto de ASP.NET MVC

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 1 de un [tutorial de aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito en el que se explica cómo crear una aplicación web pequeña, pero completa, con ASP.NET MVC 1.
> 
> En el paso 1 se muestra cómo poner en marcha la estructura de la aplicación NerdDinner básica.
> 
> Si usa ASP.NET MVC 3, se recomienda seguir los tutoriales [de la introducción con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o MVC [Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-1-file-gtnew-project"></a>NerdDinner paso 1: archivo-&gt;nuevo proyecto

Comenzaremos nuestra aplicación NerdDinner seleccionando el elemento **de menú Archivo-&gt;nuevo proyecto** en visual Studio 2008 o gratis Visual Web Developer 2008 Express.

Se abrirá el cuadro de diálogo "nuevo proyecto". Para crear una nueva aplicación ASP.NET MVC, seleccionaremos el nodo "Web" en el lado izquierdo del cuadro de diálogo y, a continuación, elegiremos la plantilla de proyecto "ASP.NET MVC Web Application" a la derecha:

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*Importante: Asegúrese de que ha descargado e instalado ASP.NET MVC; de lo contrario, no se mostrará en el cuadro de diálogo nuevo proyecto. Puede usar la versión 2 de la [instalador de plataforma web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx) si aún no la ha instalado (ASP.NET MVC está disponible en la sección "plataforma web: entornos de&gt;y tiempos de ejecución").*

Asignaremos el nombre "NerdDinner" al nuevo proyecto y, a continuación, haremos clic en el botón "Aceptar" para crearlo.

Al hacer clic en "Aceptar", Visual Studio abrirá un cuadro de diálogo adicional en el que se le pregunta si quiere crear también un proyecto de prueba unitaria para la nueva aplicación. Este proyecto de prueba unitaria nos permite crear pruebas automatizadas que comprueban la funcionalidad y el comportamiento de la aplicación (algo que veremos cómo hacerlo más adelante en este tutorial).

![](create-a-new-aspnet-mvc-project/_static/image2.png)

La lista desplegable "marco de prueba" del cuadro de diálogo anterior se rellena con todas las plantillas de proyecto de prueba unitaria de ASP.NET MVC disponibles en la máquina. Las versiones se pueden descargar para NUnit, MBUnit y XUnit. También se admite el marco de pruebas unitarias de Visual Studio integrado.

*Nota: el marco de pruebas unitarias de Visual Studio solo está disponible con Visual Studio 2008 Professional y versiones posteriores. Si usa VS 2008 Standard Edition o Visual Web Developer 2008 Express, tendrá que descargar e instalar las extensiones NUnit, MBUnit o XUnit para ASP.NET MVC para que se muestre este cuadro de diálogo. No se mostrará el cuadro de diálogo si no hay ningún marco de pruebas instalado.*

Usaremos el nombre predeterminado "NerdDinner. tests" para el proyecto de prueba que creamos y usaremos la opción de marco "prueba unitaria de Visual Studio". Cuando hacemos clic en el botón "Aceptar", Visual Studio creará una solución para nosotros con dos proyectos en él: uno para nuestra aplicación web y otro para nuestras pruebas unitarias:

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>Examinar la estructura de directorios de NerdDinner

Al crear una nueva aplicación ASP.NET MVC con Visual Studio, se agrega automáticamente un número de archivos y directorios al proyecto:

![](create-a-new-aspnet-mvc-project/_static/image4.png)

Los proyectos de MVC de ASP.NET de forma predeterminada tienen seis directorios de nivel superior:

| **Directorio** | **Propósito** |
| --- | --- |
| **/Controllers** | Dónde colocar las clases de controlador que controlan las solicitudes de dirección URL |
| **/Models** | Dónde colocar las clases que representan y manipulan los datos |
| **/Views** | Dónde colocar los archivos de plantilla de interfaz de usuario que son responsables de la representación de la salida |
| **/Scripts** | Dónde colocar los archivos y scripts de la biblioteca de JavaScript (. js) |
| **/Content** | Dónde se colocan los archivos de imagen y CSS, y otros contenidos no dinámicos o que no son de JavaScript |
| **/App\_datos** | Donde se almacenan los archivos de datos que se van a leer y escribir. |

ASP.NET MVC no requiere esta estructura. De hecho, los desarrolladores que trabajan en aplicaciones de gran tamaño suelen particionar la aplicación en varios proyectos para que sea más fácil de administrar (por ejemplo: las clases del modelo de datos van a menudo en un proyecto de biblioteca de clases independiente de la aplicación web). La estructura predeterminada del proyecto, sin embargo, proporciona una buena Convención de directorio predeterminada que se puede usar para mantener la preocupación en la aplicación.

Al expandir el directorio/Controllers, veremos que Visual Studio agregó dos clases de controlador: HomeController y AccountController, de forma predeterminada al proyecto:

![](create-a-new-aspnet-mvc-project/_static/image5.png)

Al expandir el directorio/views, encontrará tres subdirectorios:/Home,/Account y/Shared, así como varios archivos de plantilla dentro de ellos también se agregaron al proyecto de forma predeterminada:

![](create-a-new-aspnet-mvc-project/_static/image6.png)

Cuando se expandan los directorios/Content y/scripts, encontrará un archivo site. CSS que se usa para aplicar estilo a todo el código HTML del sitio, así como a las bibliotecas de JavaScript que pueden habilitar la compatibilidad con ASP.NET AJAX y jQuery en la aplicación:

![](create-a-new-aspnet-mvc-project/_static/image7.png)

Al expandir el proyecto NerdDinner. tests, encontrará dos clases que contienen pruebas unitarias para las clases de controlador:

![](create-a-new-aspnet-mvc-project/_static/image8.png)

Estos archivos predeterminados agregados por Visual Studio nos proporcionan una estructura básica para una aplicación de trabajo: completa con Página principal, acerca de la página, Inicio de sesión de cuenta, cierre de sesión y páginas de registro, y una página de error no controlada (todos los cables conectados y listos para usar).

### <a name="running-the-nerddinner-application"></a>Ejecución de la aplicación NerdDinner

Para ejecutar el proyecto, puede elegir los elementos de menú Depurar **-&gt;iniciar depuración** o depurar **&gt;iniciar sin depurar** :

![](create-a-new-aspnet-mvc-project/_static/image9.png)

Se iniciará el ASP.NET Web-Server integrado que se incluye con Visual Studio y se ejecutará la aplicación:

![](create-a-new-aspnet-mvc-project/_static/image10.png)

A continuación se muestra la Página principal de nuestro nuevo proyecto (URL: "/") cuando se ejecuta:

![](create-a-new-aspnet-mvc-project/_static/image11.png)

Al hacer clic en la pestaña "acerca de", se muestra una página acerca de (URL: "/Home/About"):

![](create-a-new-aspnet-mvc-project/_static/image12.png)

Al hacer clic en el vínculo "iniciar sesión" en la parte superior derecha nos remite a una página de inicio de sesión (URL: "/Account/LogOn")

![](create-a-new-aspnet-mvc-project/_static/image13.png)

Si no tenemos una cuenta de inicio de sesión, podemos hacer clic en el vínculo Register (URL: "/Account/Register") para crear uno:

![](create-a-new-aspnet-mvc-project/_static/image14.png)

El código para implementar la funcionalidad anterior de inicio, acerca de y cierre de sesión/Registro se agregó de forma predeterminada al crear el nuevo proyecto. La usaremos como punto de partida de nuestra aplicación.

### <a name="testing-the-nerddinner-application"></a>Prueba de la aplicación NerdDinner

Si usamos la edición Professional o una versión posterior de Visual Studio 2008, podemos usar la compatibilidad integrada de IDE de pruebas unitarias en Visual Studio para probar el proyecto:

![](create-a-new-aspnet-mvc-project/_static/image15.png)

Al elegir una de las opciones anteriores, se abrirá el panel "Resultados de pruebas" en el IDE y nos proporcionará el estado Pass/Fail en las 27 pruebas unitarias incluidas en nuestro nuevo proyecto que cubren la funcionalidad integrada:

![](create-a-new-aspnet-mvc-project/_static/image16.png)

Más adelante en este tutorial hablaremos sobre las pruebas automatizadas y agregaremos pruebas unitarias adicionales que cubran la funcionalidad de la aplicación que implementamos.

### <a name="next-step"></a>Paso siguiente

Ahora tenemos una estructura de aplicación básica en su lugar. Ahora vamos [a crear una base de datos para almacenar los datos de la aplicación](create-a-database.md).

> [!div class="step-by-step"]
> [Anterior](introducing-the-nerddinner-tutorial.md)
> [Siguiente](create-a-database.md)
