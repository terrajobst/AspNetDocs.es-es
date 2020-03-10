---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Parte 1: información general y archivo: > nuevo proyecto | Microsoft Docs'
author: jongalloway
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 1 cubre información general y > nuevo proyecto.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 48428ff4ab5888253ed93ac41e79006eec823ad2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78451285"
---
# <a name="part-1-overview-and-file-new-project"></a>Parte 1: información general y archivo > nuevo proyecto

por [Jon Galloway](https://github.com/jongalloway)

> El almacén de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> MVC Music Store es una implementación ligera de almacén de ejemplo que vende álbumes musicales en línea e implementa la funcionalidad básica de administración de sitios, Inicio de sesión de usuario y carro de la compra.  
>   
> En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 1 cubre información general y&gt;nuevo proyecto.

## <a name="overview"></a>Información general

MVC Music Store es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Web Developer para el desarrollo web. Comenzaremos lentamente, por lo que la experiencia de desarrollo web de nivel principiante es correcta.

La aplicación que se va a crear es una tienda de música simple. Hay tres partes principales en la aplicación: compras, desprotección y administración.

![](mvc-music-store-part-1/_static/image1.jpg)

Los visitantes pueden examinar álbumes por género:

![](mvc-music-store-part-1/_static/image2.jpg)

Pueden ver un solo álbum y agregarlo a su carro:

![](mvc-music-store-part-1/_static/image3.jpg)

Pueden revisar el carro y quitar los elementos que ya no deseen:

![](mvc-music-store-part-1/_static/image4.jpg)

Al continuar con la desprotección, se les pedirá que inicien sesión o se registren para una cuenta de usuario.

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

Después de crear una cuenta, puede completar el pedido rellenando la información de envío y pago. Para simplificar las cosas, vamos a ejecutar una fantástica promoción: todo está disponible si escribe el código de promoción "gratis".

![](mvc-music-store-part-1/_static/image5.jpg)

Después de la ordenación, verán una pantalla de confirmación simple:

![](mvc-music-store-part-1/_static/image6.jpg)

Además de las páginas orientadas al cliente, también se creará una sección de administrador en la que se muestra una lista de álbumes en los que los administradores pueden crear, editar y eliminar álbumes:

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a>1. archivo:&gt; nuevo proyecto

### <a name="installing-the-software"></a>Instalación del software

Este tutorial comenzará por la creación de un nuevo proyecto de ASP.NET MVC 3 con Visual Web Developer 2010 Express gratuito (que es gratuito) y, a continuación, agregaremos características para crear una aplicación que funcione completamente. A lo largo del proceso, trataremos el acceso a bases de datos, los escenarios de publicación de formularios, la validación de datos, el uso de páginas maestras para un diseño de página coherente, el uso de AJAX para la validación y actualización de páginas, el inicio de sesión de usuario

Puede seguir paso a paso, o bien puede descargar la aplicación completada desde [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).

Puede usar Visual Studio 2010 SP1 o Visual Web Developer 2010 Express SP1 (una versión gratuita de Visual Studio 2010) para compilar la aplicación. Usaremos el SQL Server Compact (también gratuito) para hospedar la base de datos. Antes de empezar, asegúrese de que ha instalado los requisitos previos que se enumeran a continuación.

- [Requisitos previos de Visual Studio Web Developer Express SP1]
- [ASP.NET MVC 3 Tools Update]
- [SQL Server Compact 4,0]: compatibilidad con el motor de tiempo de ejecución y las herramientas

### <a name="creating-a-new-aspnet-mvc-3-project"></a>Creación de un nuevo proyecto de ASP.NET MVC 3

Comenzaremos seleccionando "nuevo proyecto" en el menú Archivo de Visual Web Developer. Se abrirá el cuadro de diálogo nuevo proyecto.

![](mvc-music-store-part-1/_static/image5.png)

Seleccionaremos el grupo plantillas C# Web de Visual&gt; a la izquierda y, a continuación, elegiremos la plantilla "aplicación web MVC 3 ASP.net" en la columna central. Asigne al proyecto el nombre MvcMusicStore y presione el botón Aceptar.

![](mvc-music-store-part-1/_static/image8.jpg)

Se mostrará un cuadro de diálogo secundario que nos permite realizar algunos valores específicos de MVC para el proyecto. Seleccione lo siguiente:

Plantilla de proyecto: seleccionar vacío

Motor de vista: seleccione Razor

Usar marcado semántico de HTML5: comprobado

Compruebe que la configuración sea la que se muestra a continuación y, a continuación, presione el botón Aceptar.

![](mvc-music-store-part-1/_static/image9.jpg)

Se creará el proyecto. Echemos un vistazo a las carpetas que se han agregado a nuestra aplicación en el Explorador de soluciones del lado derecho.

![](mvc-music-store-part-1/_static/image10.jpg)

La plantilla de MVC 3 vacía no está completamente vacía: agrega una estructura de carpetas básica:

![](mvc-music-store-part-1/_static/image6.png)

ASP.NET MVC usa algunas convenciones de nomenclatura básicas para los nombres de carpeta:

| **Carpeta** | **Propósito** |
| --- | --- |
| **/Controllers** | Los controladores responden a la entrada desde el explorador, deciden qué hacer con él y devuelven respuesta al usuario. |
| **/Views** | Vistas que contienen nuestras plantillas de interfaz de usuario |
| **/Models** | Los modelos contienen y manipulan datos |
| **/Content** | Esta carpeta contiene nuestras imágenes, CSS y cualquier otro contenido estático. |
| **/Scripts** | Esta carpeta contiene los archivos de JavaScript |

Estas carpetas se incluyen incluso en una aplicación ASP.NET MVC vacía porque el marco ASP.NET MVC usa de forma predeterminada un enfoque de "Convención de configuración" y realiza algunas suposiciones predeterminadas en función de las convenciones de nomenclatura de carpetas. Por ejemplo, los controladores buscan vistas en la carpeta views de forma predeterminada sin tener que especificar explícitamente esto en el código. El uso de las convenciones predeterminadas reduce la cantidad de código que necesita escribir y también puede facilitar a otros desarrolladores el conocimiento del proyecto. Explicaremos estas convenciones más a medida que compilamos nuestra aplicación.

> [!div class="step-by-step"]
> [Siguiente](mvc-music-store-part-2.md)
