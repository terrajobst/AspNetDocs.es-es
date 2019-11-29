---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Introducción con formularios Web Forms de ASP.NET 4,7 y Visual Studio 2017 | Microsoft Docs
author: Erikre
description: Esta serie de tutoriales paso a paso le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4,7 y Microsoft Visual Studio
ms.author: riande
ms.date: 01/09/2019
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 52d5eb7abe4520ebdf6667d299d055fc7619a635
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74615455"
---
# <a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2017"></a>Introducción con formularios Web Forms de ASP.NET 4,5 y Visual Studio 2017

[Descargar el proyecto de ejemplo deC#Wingtip Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar el libro electrónico (pdf)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

En esta serie de tutoriales se muestra cómo crear una aplicación de formularios Web Forms ASP.NET con ASP.NET 4,5 y Microsoft Visual Studio 2017. 

## <a name="introduction"></a>Introducción

Esta serie de tutoriales le guía en la creación de una aplicación de formularios Web Forms ASP.NET con Visual Studio 2017 y ASP.NET 4,5. Creará una aplicación llamada **Wingtip Toys** -un sitio web de escaparate simplificado que vende artículos en línea. Durante la serie, se resaltan las nuevas características de ASP.NET 4,5.

### <a name="target-audience"></a>Audiencia de destino

Los desarrolladores nuevos en ASP.NET Web Forms son la audiencia de destino de esta serie de tutoriales.

Debería tener algún conocimiento en las siguientes áreas:

- Programación orientada a objetos (OOP) e idiomas
- Desarrollo web (HTML, CSS, JavaScript)
- Bases de datos relacionales
- Arquitectura de n niveles

Para revisar estas áreas, considere la posibilidad de estudiar el siguiente contenido:

- [Introducción a Visual C#](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Desarrollo web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, jQuery](http://w3schools.com/)
- [Base de datos relacional](http://en.wikipedia.org/wiki/Relational_database)
- [Arquitectura de varios niveles](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a>Características de la aplicación

Las características de formularios Web Forms de ASP.NET presentadas en esta serie incluyen:

- Proyecto de aplicación web (no proyecto de sitio web)
- formularios Web Forms
- Páginas maestras, configuración
- Bootstrap
- Entity Framework Code First, LocalDB
- Validación de solicitudes
- Controles de datos fuertemente tipados
- Enlace de modelos
- Anotaciones de datos
- Proveedores de valores
- SSL y OAuth
- ASP.NET Identity, configuración y autorización
- Validación discreta
- Enrutamiento
- Control de errores de ASP.NET

### <a name="application-scenarios-and-tasks"></a>Escenarios y tareas de la aplicación

Las tareas de la serie de tutoriales incluyen:

- Crear, revisar y ejecutar un proyecto nuevo
- Crear una estructura de base de datos
- Inicializar y propagar una base de datos
- Personalización de la interfaz de usuario con estilos, gráficos y una página maestra
- Agregar páginas y navegación
- Mostrar detalles del menú y datos del producto
- Creación de un carro de la compra
- Adición de compatibilidad con SSL y OAuth
- Agregar un método de pago
- Incluir un rol de administrador y un usuario en la aplicación
- Restringir el acceso a páginas y carpetas específicas
- Carga de un archivo en la aplicación Web
- Implementar la validación de entrada
- Registro de rutas para la aplicación Web
- Implementar el control de errores y el registro de errores

## <a name="overview"></a>Información general del

Esta serie de tutoriales está destinada a usuarios familiarizados con los conceptos de programación, pero que son nuevos en ASP.NET Web Forms. Si ya está familiarizado con los formularios Web Forms de ASP.NET, esta serie puede ayudarle a conocer las nuevas características de ASP.NET 4,5. Para los lectores que no estén familiarizados con los conceptos de programación y los formularios Web Forms de ASP.NET, consulte los tutoriales de formularios Web Forms adicionales que se proporcionan en la sección [Introducción](../../../index.md) del sitio Web de ASP.net.

El ASP.NET 4,5 proporcionado en esta serie de tutoriales incluye las siguientes características:

- Una interfaz de usuario sencilla para crear proyectos que ofrece [compatibilidad con muchos marcos de trabajo de ASP.net](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC y Web API).
- [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), un diseño, la configuración y el marco de diseño con capacidad de respuesta.
- [ASP.net Identity](../../../../identity/index.md), un nuevo sistema de pertenencia a ASP.net que funciona igual en todos los marcos de ASP.net y funciona con software de hospedaje web que no sea IIS.
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx)

  Una actualización del Entity Framework le permite:
  - Recuperar y manipular datos como objetos fuertemente tipados
  - Acceder a los datos de forma asincrónica
  - Control de errores de conexión transitorios
  - Instrucciones SQL de registro

Para obtener una lista completa de características de ASP.NET 4,5, consulte [ASP.net and Web Tools para obtener Visual Studio 2013 notas de la versión](../../../../visual-studio/overview/2013/release-notes.md).

### <a name="the-wingtip-toys-sample-application"></a>Aplicación de ejemplo Wingtip Toys

Las capturas de pantallas siguientes proceden de la aplicación de formularios Web Forms ASP.NET que se crea en esta serie de tutoriales. Al ejecutar la aplicación en Visual Studio, aparece la siguiente página principal Web.

![Wingtip Toys: página predeterminada](introduction-and-overview/_static/image1.png)

Puede registrarse como usuario nuevo o iniciar sesión como un usuario existente. La navegación superior tiene vínculos a las categorías de producto y sus productos de la base de datos.

Si selecciona **productos**, se muestran todos los productos disponibles. 

![Wingtip Toys-productos](introduction-and-overview/_static/image2.png)

Si selecciona un producto específico, se mostrarán los detalles del producto.

![Wingtip Toys: detalles del producto](introduction-and-overview/_static/image3.png)

Como usuario, puede registrarse e iniciar sesión con la funcionalidad predeterminada de la plantilla de formularios Web Forms. En este tutorial también se explica cómo iniciar sesión con una cuenta existente de gmail. Además, puede iniciar sesión como administrador para agregar y quitar productos de la base de datos.

![Wingtip Toys: Inicio de sesión](introduction-and-overview/_static/image4.png)

Una vez que haya iniciado sesión como usuario, puede Agregar productos al carro de la compra y desprotegerlos con PayPal. La aplicación de ejemplo está diseñada para trabajar en el espacio aislado del desarrollador de PayPal. No tiene lugar ninguna transacción Money real.

![Wingtip Toys: carro de la compra](introduction-and-overview/_static/image5.png)

PayPal confirma la información de la cuenta, el pedido y el pago.

![Wingtip Toys-PayPal](introduction-and-overview/_static/image6.png)

Después de volver de PayPal, puede revisar y completar el pedido.

![Wingtip Toys: revisión de pedidos](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, asegúrese de que está instalado el siguiente software en el equipo:

- [Microsoft Visual Studio 2017 o Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).

El .NET Framework se instala automáticamente.

En esta serie de tutoriales se usa Microsoft Visual Studio Community 2017. Puede usar o Microsoft Visual Studio 2017 para completar esta serie de tutoriales.

Tenga en cuenta lo siguiente sobre Visual Studio:

* Microsoft Visual Studio 2017 y Microsoft Visual Studio Community 2017 se conocen como *Visual Studio* a lo largo de esta serie de tutoriales.

* Visual Studio 2017 se instala junto a las versiones anteriores ya instaladas. Los sitios creados en versiones anteriores se pueden abrir en Visual Studio 2017 y seguir abriendo en versiones anteriores.

* La primera vez que inicia Visual Studio, se supone que ha seleccionado la configuración de *desarrollo web* . Para obtener más información, consulte [Cómo: seleccionar la configuración del entorno de desarrollo web](https://msdn.microsoft.com/library/ff521558.aspx).

Después de instalar los requisitos previos, está listo para empezar a crear el proyecto web presentado en esta serie de tutoriales.

## <a name="download-the-sample-application"></a>Descarga de la aplicación de ejemplo

 Puede descargar la aplicación de ejemplo completada en cualquier momento desde el sitio de ejemplos de MSDN:

[Introducción con formularios Web Forms de ASP.NET 4,5 y Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 

 Esta descarga tiene los siguientes elementos:

- La aplicación de ejemplo en la carpeta *WingtipToys* .
- Los recursos usados para crear la aplicación de ejemplo en la carpeta *WingtipToys-assets* de la carpeta *WingtipToys* .

La descarga es un archivo *. zip* . Para ver el proyecto completado que crea esta serie de tutoriales, busque y *C#* seleccione la carpeta en el archivo. zip. Guarde la C# carpeta en la carpeta que utiliza para trabajar con proyectos de Visual Studio. De forma predeterminada, la carpeta proyectos de Visual Studio 2017 es:

<strong>C:\Users&#92;</strong>  <strong><em>&lt;nombredeusuario&gt;</em></strong> <strong>\source\repos</strong>

Cambie el nombre ***C#*** de la carpeta a ***WingtipToys***.

> [!NOTE]
> Si ya tiene una carpeta llamada *WingtipToys* en la carpeta de proyectos, cambie temporalmente el nombre de la carpeta existente antes de *C#* cambiar el nombre de la carpeta a *WingtipToys*.

Para ejecutar el proyecto completado, abra la carpeta *WingtipToys* y haga doble clic en el archivo *WingtipToys. sln* . Visual Studio 2017 abre el proyecto. A continuación, haga clic con el botón secundario en el archivo *default. aspx* en **Explorador de soluciones** y seleccione **ver en el explorador**.

## <a name="take-a-aspnet-web-forms-quiz-to-review-content"></a>Tome una prueba de formularios Web Forms de ASP.NET para revisar el contenido

Después de completar la serie de tutoriales, realice una prueba para probar sus conocimientos y reforzar los conceptos clave. Cada pregunta proporciona una explicación y vínculos a instrucciones adicionales.

* [Cuestionario de formularios Web Forms ASP.NET](https://blogs.msdn.microsoft.com/erikreitan/2016/01/08/asp-net-web-forms-quiz/) 

## <a name="tutorial-support-and-comments"></a>Comentarios y soporte técnico del tutorial

Si tiene preguntas y comentarios, use las secciones p y a que se incluyen en la página de ejemplo [Introducción con ASP.net 4,5 Web Forms y Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#).

Los comentarios de esta serie de tutoriales son bienvenidos. Cuando se actualiza esta serie de tutoriales, se hace todo lo posible para tener en cuenta las correcciones o sugerencias para mejorar.

Si se produce un error, los mensajes de error correspondientes podrían ser confusos, sin ninguna explicación adecuada sobre cómo corregirlo. Para obtener ayuda, puede consultar los [foros de ASP.net](https://forums.asp.net/). Otra buena fuente es la sección preguntas y respuestas de la página de ejemplo [Introducción con los formularios Web de ASP.net 4,5 y Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#). 

> [!div class="step-by-step"]
> [Siguiente](create-the-project.md)
