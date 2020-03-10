---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'Tutorial: creación de la aplicación web y los modelos de datos para EF Database First con ASP.NET MVC'
description: Este tutorial se centra en la creación de la aplicación web y la generación de los modelos de datos basados en las tablas de base de datos.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 30fd42be5677df6fa6ee0630914098c30d21385b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78499531"
---
# <a name="tutorial-create-the-web-application-and-data-models-for-ef-database-first-with-aspnet-mvc"></a>Tutorial: creación de la aplicación web y los modelos de datos para EF Database First con ASP.NET MVC

 Con el scaffolding de MVC, Entity Framework y ASP.NET, puede crear una aplicación web que proporcione una interfaz a una base de datos existente. En esta serie de tutoriales se muestra cómo generar automáticamente código que permite a los usuarios mostrar, editar, crear y eliminar datos que residen en una tabla de base de datos. El código generado corresponde a las columnas de la tabla de base de datos.

Este tutorial se centra en la creación de la aplicación web y la generación de los modelos de datos basados en las tablas de base de datos.

En este tutorial va a:

> [!div class="checklist"]
> * Crear una aplicación web ASP.NET
> * Generar los modelos

## <a name="prerequisites"></a>Requisitos previos

* [Introducción a Entity Framework 6 Database First con MVC 5](setting-up-database.md)

## <a name="create-an-aspnet-web-app"></a>Crear una aplicación web ASP.NET

En una solución nueva o en la misma solución que el proyecto de base de datos, cree un nuevo proyecto en Visual Studio y seleccione la plantilla **aplicación Web ASP.net** . Asigne al proyecto el nombre **ContosoSite**.

![creación de proyecto](creating-the-web-application/_static/image1.png)

Haga clic en **Aceptar**.

En la ventana nuevo proyecto de ASP.NET, seleccione la plantilla **MVC** . Puede borrar la opción **host en la nube** por ahora porque implementará la aplicación en la nube más adelante. Haga clic en **Aceptar** para crear la aplicación.

El proyecto se crea con los archivos y carpetas predeterminados.

En este tutorial, usará Entity Framework 6. Puede comprobar la versión de Entity Framework del proyecto a través de la ventana administrar paquetes NuGet. Si es necesario, actualice su versión de Entity Framework.

![Mostrar versión](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>Generar los modelos

Ahora creará Entity Framework modelos a partir de las tablas de base de datos. Estos modelos son clases que usará para trabajar con los datos. Cada modelo refleja una tabla en la base de datos y contiene propiedades que corresponden a las columnas de la tabla.

Haga clic con el botón secundario en la carpeta **modelos** y seleccione **Agregar** y **nuevo elemento**.

En la ventana Agregar nuevo elemento, seleccione **datos** en el panel izquierdo y **ADO.NET Entity Data Model** en las opciones del panel central. Asigne al nuevo archivo de modelo el nombre **ContosoModel**.

Haga clic en **Agregar**.

En el Asistente para Entity Data Model, seleccione **EF Designer en la base de datos**.

Haga clic en **Siguiente**.

Si tiene conexiones de base de datos definidas en el entorno de desarrollo, es posible que vea una de estas conexiones previamente seleccionada. Sin embargo, desea crear una nueva conexión a la base de datos creada en la primera parte de este tutorial. Haga clic en el botón **nueva conexión** .

En el ventana Propiedades de conexión, proporcione el nombre del servidor local en el que se creó la base de datos (en este caso, **LocalDB) \ProjectsV13**). Después de proporcionar el nombre del servidor, seleccione el ContosoUniversityData de las bases de datos disponibles.

![establecer propiedades de conexión](creating-the-web-application/_static/image8.png)

Haga clic en **Aceptar**.

Ahora se muestran las propiedades de conexión correctas. Puede usar el nombre predeterminado para la conexión en el archivo Web. config.

Haga clic en **Siguiente**.

Seleccione la versión más reciente de Entity Framework.

Haga clic en **Siguiente**.

Seleccione **tablas** para generar modelos para las tres tablas.

Haga clic en **Finalizar**.

Si recibe una advertencia de seguridad, seleccione **Aceptar** para continuar con la ejecución de la plantilla.

Los modelos se generan a partir de las tablas de base de datos y se muestra un diagrama que muestra las propiedades y las relaciones entre las tablas.

![diagrama del modelo](creating-the-web-application/_static/image11.png)

La carpeta modelos ahora incluye muchos archivos nuevos relacionados con los modelos generados a partir de la base de datos.

El archivo **ContosoModel.Context.CS** contiene una clase que se deriva de la clase **DbContext** y proporciona una propiedad para cada clase de modelo que corresponde a una tabla de base de datos. Los archivos **Course.CS**, **Enrollment.CS**y **Student.CS** contienen las clases de modelo que representan las tablas de bases de datos. Usará tanto la clase de contexto como las clases de modelo al trabajar con la técnica scaffolding.

Antes de continuar con este tutorial, compile el proyecto. En la siguiente sección, generará código basado en los modelos de datos, pero esa sección no funcionará si el proyecto no se ha compilado.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial va a:

> [!div class="checklist"]
> * Se creó una aplicación Web de ASP.NET
> * Modelos generados

Avance al siguiente tutorial para aprender a crear código generado basado en los modelos de datos.
> [!div class="nextstepaction"]
> [Generar vistas](generating-views.md)
