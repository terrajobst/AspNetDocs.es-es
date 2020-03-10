---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Introducción a ASP.NET MVC | Microsoft Docs
author: shanselman
description: Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Cree una aplicación web simple que lea y escriba en una base de datos.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: f8f0014776ba1313119e8c39c63a216b0fc864e7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78469801"
---
# <a name="intro-to-aspnet-mvc"></a>Introducción a ASP.NET MVC

por [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Una versión actualizada si este tutorial está disponible [aquí](../../getting-started/introduction/getting-started.md) mediante [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013). En el nuevo tutorial se usa ASP.NET MVC 5, que proporciona muchas mejoras en este tutorial.
>
>
> Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Creará una aplicación web simple que lee y escribe en una base de datos. Visite el [centro de aprendizaje de MVC de ASP.net](../../../index.md) para buscar otros tutoriales y ejemplos de ASP.NET MVC.

Vamos a crear nuestra primera aplicación web MVC de ASP.NET con [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/). Crearemos una pequeña aplicación de lista de películas que nos permitirá crear y enumerar películas.

## <a name="what-youll-build"></a>Lo que creará

A continuación se muestran dos capturas de pantallas de la aplicación que va a compilar. Tendrá una tabla sencilla de películas con varias columnas.

[![lista de películas: Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

Y tendrá un formulario de creación para que podamos agregar películas a la lista.

[![crear una película: Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a>Habilidades que aprenderá

Este tutorial le enseñará los conceptos básicos de la creación de una aplicación web MVC de ASP.NET con Visual Studio. Aprenderá lo siguiente:

- Cómo crear un nuevo proyecto de ASP.NET MVC
- Cómo crear una nueva base de datos con SQL Server
- Cómo crear controladores y vistas de ASP.NET MVC
- Cómo recuperar y Mostrar datos
- Cómo editar datos y habilitar la validación de datos
- Cómo actualizar el esquema de la base de datos

## <a name="get-started"></a>Primeros pasos

Para empezar, ejecute Visual Web Developer 2010 Express (lo llamaremos "vWD existente" desde ahora) y seleccione nuevo proyecto en la pantalla Inicio.

Visual Web Developer es un IDE o un entorno de desarrollo integrado. Del mismo modo que usa Microsoft Word para escribir documentos, usará un IDE para crear aplicaciones. Hay una barra de herramientas en la parte superior que muestra varias opciones disponibles, así como el menú que también podría haber usado para seleccionar archivo | Nuevo proyecto.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a>Crear la primera aplicación

Puede crear aplicaciones mediante Visual Basic o visual C#. Por ahora, seleccione visual C# a la izquierda y, a continuación, elija "aplicación web MVC 2 de ASP.net". Asigne el nombre "películas" al proyecto y haga clic en Aceptar.

[![nuevo proyecto](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

En el lado derecho está el Explorador de soluciones que muestra todos los archivos y carpetas de la aplicación. La ventana grande del centro es donde se edita el código y se dedica la mayor parte del tiempo. Visual Studio usó una plantilla predeterminada para el proyecto ASP.NET MVC que acaba de crear, por lo que tiene una aplicación en funcionamiento en este momento sin hacer nada. Este es un sencillo "Hola mundo! Project, y es un buen punto de partida para la aplicación.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

Seleccione el botón "reproducir" en la barra de herramientas.

![Iniciar depuración](getting-started-with-mvc-part1/_static/image11.png)

Es una flecha verde que apunta a la derecha que compilará el programa e iniciará la aplicación en un explorador Web.

*Nota: en su lugar, puede presionar F5 en el teclado o seleccionar depurar-&gt;iniciar depuración desde el menú "depurar".*

Esto hará que Visual Web Developer inicie un servidor Web de desarrollo y ejecute la aplicación web (no es necesario realizar ninguna configuración o pasos manuales para habilitarlo). A continuación, iniciará un explorador y lo configurará para examinar la Página principal de la aplicación. Observe que la barra de direcciones del explorador indica "localhost", y no algo como example.com. Esto se debe a que localhost siempre apunta a su propio equipo local, que en este caso está ejecutando la aplicación que acabamos de crear.

[Página principal de ![](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

De forma predeterminada, esta plantilla predeterminada proporciona dos páginas para visitar y una página de inicio de sesión básica. Vamos a cambiar el funcionamiento de esta aplicación y aprender un poco sobre ASP.NET MVC en el proceso. Cierre el explorador y permita cambiar parte del código.

> [!div class="step-by-step"]
> [Siguiente](getting-started-with-mvc-part2.md)
