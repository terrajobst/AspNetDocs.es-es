---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'Tutorial: personalización de la vista para EF Database First con la aplicación ASP.NET MVC'
description: Este tutorial se centra en cambiar las vistas generadas automáticamente para mejorar la presentación.
author: Rick-Anderson
ms.author: riande
ms.date: 01/24/2019
ms.topic: tutorial
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 89b8a0eb84b6e287c45bc141c68a2c76e63b0e41
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78471523"
---
# <a name="tutorial-customize-view-for-ef-database-first-with-aspnet-mvc-app"></a>Tutorial: personalización de la vista para EF Database First con la aplicación ASP.NET MVC

Con el scaffolding de MVC, Entity Framework y ASP.NET, puede crear una aplicación web que proporcione una interfaz a una base de datos existente. En esta serie de tutoriales se muestra cómo generar automáticamente código que permite a los usuarios mostrar, editar, crear y eliminar datos que residen en una tabla de base de datos. El código generado corresponde a las columnas de la tabla de base de datos.

Este tutorial se centra en cambiar las vistas generadas automáticamente para mejorar la presentación.

En este tutorial va a:

> [!div class="checklist"]
> * Agregar cursos a la página de detalles de estudiante
> * Confirmar que los cursos se agregan a la página

## <a name="prerequisites"></a>Requisitos previos

* [Cambio de la base de datos](changing-the-database.md)

## <a name="add-courses-to-student-detail"></a>Agregar cursos a los detalles de estudiante

El código generado proporciona un buen punto de partida para la aplicación, pero no proporciona necesariamente toda la funcionalidad que necesita en la aplicación. Puede personalizar el código para satisfacer los requisitos específicos de la aplicación. Actualmente, la aplicación no muestra los cursos inscritos del estudiante seleccionado. En esta sección, agregará los cursos inscritos de cada estudiante a la vista de **detalles** del alumno.

Abra **vistas** > **estudiantes** > *details. cshtml*. Debajo de la última &lt;etiqueta de&gt;/DL, pero antes de la etiqueta de cierre &lt;/div&gt;, agregue el código siguiente.

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

Este código crea una tabla que muestra una fila para cada registro de la tabla de inscripción del alumno seleccionado. El método de **visualización** representa el código HTML para el objeto (modelItem) que representa la expresión. Use el método display (en lugar de simplemente incrustar el valor de propiedad en el código) para asegurarse de que el valor tiene el formato correcto según su tipo y la plantilla de ese tipo. En este ejemplo, cada expresión devuelve una única propiedad del registro actual del bucle y los valores son tipos primitivos que se representan como texto.

## <a name="confirm-courses-are-added"></a>Confirmar que se han agregado cursos

Ejecute la solución. Haga clic en **lista de estudiantes** y seleccione **detalles** para uno de los alumnos. Verá que los cursos inscritos se han incluido en la vista.

![estudiante con inscripción](customizing-a-view/_static/image1.png)

## <a name="next-steps"></a>Pasos siguientes
En este tutorial va a:

> [!div class="checklist"]
> * Cursos agregados a la página de detalles de estudiante
> * Confirmado que los cursos se han agregado a la página

Avance al siguiente tutorial para aprender a agregar anotaciones de datos para especificar los requisitos de validación y el formato de presentación.
> [!div class="nextstepaction"]
> [Mejorar la validación de datos](enhancing-data-validation.md)
