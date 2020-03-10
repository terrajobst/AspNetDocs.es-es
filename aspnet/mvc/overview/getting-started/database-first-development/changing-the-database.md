---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'Tutorial: cambio de la base de datos de EF Database First con la aplicación ASP.NET MVC'
description: Este tutorial se centra en hacer una actualización de la estructura de la base de datos y propagar ese cambio en toda la aplicación Web.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 52cad1120908cf0d4f85770f8e2690f9415c5f56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78499525"
---
# <a name="tutorial-change-the-database-for-ef-database-first-with-aspnet-mvc-app"></a>Tutorial: cambio de la base de datos de EF Database First con la aplicación ASP.NET MVC

Con el scaffolding de MVC, Entity Framework y ASP.NET, puede crear una aplicación web que proporcione una interfaz a una base de datos existente. En esta serie de tutoriales se muestra cómo generar automáticamente código que permite a los usuarios mostrar, editar, crear y eliminar datos que residen en una tabla de base de datos. El código generado corresponde a las columnas de la tabla de base de datos.

Este tutorial se centra en hacer una actualización de la estructura de la base de datos y propagar ese cambio en toda la aplicación Web.

En este tutorial va a:

> [!div class="checklist"]
> * Agregar una columna
> * Agregar la propiedad a las vistas

## <a name="prerequisites"></a>Requisitos previos

* [Generar vistas](generating-views.md)

## <a name="add-a-column"></a>Agregar una columna

Si actualiza la estructura de una tabla en la base de datos, debe asegurarse de que el cambio se propague al modelo de datos, las vistas y el controlador.

En este tutorial, agregará una nueva columna a la tabla Student para registrar el segundo nombre del estudiante. Para agregar esta columna, abra el proyecto de base de datos y abra el archivo Student. SQL. A través del diseñador o del código T-SQL, agregue una columna denominada **MiddleName** que sea un nvarchar (50) y admita valores NULL.

Implemente este cambio en la base de datos local. para ello, inicie el proyecto de base de datos (o F5). El nuevo campo se agrega a la tabla. Si no lo ve en el Explorador de objetos de SQL Server, haga clic en el botón actualizar en el panel.

![Mostrar nueva columna](changing-the-database/_static/image2.png)

La nueva columna existe en la tabla de base de datos, pero no existe actualmente en la clase de modelo de datos. Debe actualizar el modelo para incluir la nueva columna. En la carpeta **Models** , abra el archivo **ContosoModel. edmx** para mostrar el diagrama del modelo. Observe que el modelo Student no contiene la propiedad MiddleName. Haga clic con el botón secundario en cualquier parte de la superficie de diseño y seleccione **Actualizar modelo desde base de datos**.

En el Asistente para actualización, seleccione la pestaña **Actualizar** y, a continuación, seleccione **tablas** > **DBO** > **Student**. Haga clic en **Finalizar**.

Una vez finalizado el proceso de actualización, el diagrama de base de datos incluye la nueva propiedad **MiddleName** . Guarde el archivo **ContosoModel. edmx** . Debe guardar este archivo para que la nueva propiedad se propague a la clase **Student.CS** . Ahora ha actualizado la base de datos y el modelo.

Compile la solución.

## <a name="add-the-property-to-the-views"></a>Agregar la propiedad a las vistas

Desafortunadamente, las vistas todavía no contienen la nueva propiedad. Para actualizar las vistas tiene dos opciones: puede volver a generar las vistas mediante la adición de scaffolding para la clase Student, o bien puede Agregar manualmente la nueva propiedad a las vistas existentes. En este tutorial, agregará el scaffolding de nuevo porque no ha realizado ningún cambio personalizado en las vistas generadas automáticamente. Puede considerar la posibilidad de agregar manualmente la propiedad cuando haya realizado cambios en las vistas y no desee perder los cambios.

Para asegurarse de que se vuelven a crear las vistas, elimine la carpeta **Students** en **views**y elimine **StudentsController**. A continuación, haga clic con el botón secundario en la carpeta **Controllers** y agregue scaffolding para el modelo **Student** . De nuevo, asigne al controlador el nombre **StudentsController**. Seleccione **Agregar**.

Vuelva a compilar la solución. Las vistas contienen ahora la propiedad MiddleName.

![Mostrar segundo nombre](changing-the-database/_static/image5.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial va a:

> [!div class="checklist"]
> * Se ha agregado una columna
> * Se ha agregado la propiedad a las vistas

Avance al siguiente tutorial para aprender a personalizar la vista para mostrar los detalles de un registro de estudiante.
> [!div class="nextstepaction"]
> [Personalización de una vista](customizing-a-view.md)