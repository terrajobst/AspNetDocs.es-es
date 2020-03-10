---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Parte 1: archivo: > nuevo proyecto | Microsoft Docs'
author: JoeStagner
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks. En la parte 1 se tratan información general y archivo o proyecto nuevo.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: 05a3ace3d8fef9c1f3593f7948e42b4725d70134
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78516541"
---
# <a name="part-1-file--new-project"></a>Parte 1: archivo: > nuevo proyecto

por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks muestra cómo es extraordinariamente simple la creación de aplicaciones eficaces y escalables para la plataforma .NET. Muestra cómo usar las excelentes características nuevas de ASP.NET 4 para crear una tienda en línea, como compras, desprotección y administración.
> 
> En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks. En la parte 1 se tratan información general y archivo o proyecto nuevo.

## <a id="_Toc260221666"></a>Visión

Este tutorial es una introducción a los WebForms de ASP.NET. Comenzaremos lentamente, por lo que la experiencia de desarrollo web de nivel principiante es correcta.

La aplicación que se va a crear es una tienda en línea sencilla.

![](tailspin-spyworks-part-1/_static/image1.jpg)

Los visitantes pueden examinar productos por Categoría:

![](tailspin-spyworks-part-1/_static/image2.jpg)

Pueden ver un solo producto y agregarlo a su carro:

![](tailspin-spyworks-part-1/_static/image3.jpg)

Pueden revisar el carro y quitar los elementos que ya no deseen:

![](tailspin-spyworks-part-1/_static/image4.jpg)

Al continuar con la desprotección, se les pedirá que

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

Después de la ordenación, verán una pantalla de confirmación simple:

![](tailspin-spyworks-part-1/_static/image7.jpg)

Comenzaremos por crear un nuevo proyecto de WebForms de ASP.NET en Visual Studio 2010 y agregaremos características de forma incremental para crear una aplicación totalmente operativa. A lo largo del proceso, veremos el acceso a la base de datos, las vistas de lista y cuadrícula, las páginas de actualización de datos, la validación de datos, el uso de páginas maestras para un diseño de página coherente, AJAX, validación, pertenencia a usuarios, etc.

Puede seguir paso a paso, o puede descargar la aplicación completada desde [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)

Puede usar Visual Studio 2010 o la versión gratuita de Visual Web Developer 2010 desde [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/). Para compilar la aplicación, puede usar SQL Server o el SQL Server Express gratuito para hospedar la base de datos.

## <a id="_Toc260221667"></a>Archivo/nuevo proyecto

Comenzaremos seleccionando el nuevo proyecto en el menú Archivo de Visual Studio. Se abrirá el cuadro de diálogo nuevo proyecto.

![](tailspin-spyworks-part-1/_static/image8.jpg)

Seleccionaremos el grupo de C# plantillas de Visual/Web a la izquierda y, a continuación, elegiremos la plantilla "aplicación Web de ASP.net" en la columna central. Asigne al proyecto el nombre TailspinSpyworks y presione el botón Aceptar.

![](tailspin-spyworks-part-1/_static/image9.jpg)

Se creará el proyecto. Echemos un vistazo a las carpetas que se incluyen en nuestra aplicación en el Explorador de soluciones del lado derecho.

![](tailspin-spyworks-part-1/_static/image10.jpg)

La solución vacía no está completamente vacía: agrega una estructura de carpetas básica:

![](tailspin-spyworks-part-1/_static/image1.png)

Tenga en cuenta las convenciones implementadas por la plantilla de proyecto predeterminada de ASP.NET 4.

- La carpeta "cuenta" implementa una interfaz de usuario básica para ASP. Subsistema de pertenencia de la red.
- La carpeta "scripts" actúa como repositorio para los archivos JavaScript del lado cliente y los archivos principales de jQuery. js están disponibles de forma predeterminada.
- La carpeta "Styles" se usa para organizar los objetos visuales del sitio web (hojas de estilos CSS)

Cuando se presiona F5 para ejecutar la aplicación y se representa la página default. aspx, vemos lo siguiente.

![](tailspin-spyworks-part-1/_static/image11.jpg)

Nuestra primera mejora de la aplicación será reemplazar el archivo Style. CSS de la plantilla WebForms predeterminada por las clases CSS y los archivos de imagen asociados que van a representar el asthetics visual que queremos para nuestra aplicación Tailspin Spyworks.

Después de hacerlo, la página default. aspx se representa como esta.

![](tailspin-spyworks-part-1/_static/image12.jpg)

Observe los vínculos de imagen en la parte superior derecha de la página y los elementos de menú que se han agregado a la página maestra. Solo los vínculos "Inicio de sesión" y "cuenta" apuntan a las páginas que existen (generadas por la plantilla predeterminada) y el resto de las páginas que se van a implementar a medida que se compila la aplicación.

También vamos a reubicar la página maestra en el directorio de estilos. Aunque esto es solo una preferencia, puede simplificar las cosas si decidimos que la aplicación "se pueda enmascarar" en el futuro.

Después de hacerlo, deberá cambiar las referencias de la página maestra en todos los archivos. aspx generados por las páginas de WebForms predeterminadas de ASP.NET.

> [!div class="step-by-step"]
> [Siguiente](tailspin-spyworks-part-2.md)
