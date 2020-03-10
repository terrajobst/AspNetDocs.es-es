---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'Tutorial: generación de vistas para EF Database First con la aplicación ASP.NET MVC'
description: Este tutorial se centra en el uso de la técnica scaffolding de ASP.NET para generar los controladores y las vistas.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: e71e13e22d8a72e1699cfc70d4d93af603edba5b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78499477"
---
# <a name="tutorial-generate-views-for-ef-database-first-with-aspnet-mvc-app"></a>Tutorial: generación de vistas para EF Database First con la aplicación ASP.NET MVC

Con el scaffolding de MVC, Entity Framework y ASP.NET, puede crear una aplicación web que proporcione una interfaz a una base de datos existente. En esta serie de tutoriales se muestra cómo generar automáticamente código que permite a los usuarios mostrar, editar, crear y eliminar datos que residen en una tabla de base de datos. El código generado corresponde a las columnas de la tabla de base de datos.

Este tutorial se centra en el uso de la técnica scaffolding de ASP.NET para generar los controladores y las vistas.

En este tutorial va a:

> [!div class="checklist"]
> * Agregar scaffold
> * Agregar vínculos a nuevas vistas
> * Mostrar vistas de estudiante
> * Mostrar vistas de inscripción

## <a name="prerequisite"></a>Requisito previo

* [Crear la aplicación web y los modelos de datos](creating-the-web-application.md)

## <a name="add-scaffold"></a>Agregar scaffold

Está listo para generar código que proporcionará las operaciones de datos estándar para las clases de modelo. Agregue el código agregando un elemento scaffold. Hay muchas opciones para el tipo de scaffolding que puede Agregar; en este tutorial, el scaffolding incluirá un controlador y vistas que corresponden a los modelos de estudiantes e inscripción que creó en la sección anterior.

Para mantener la coherencia en el proyecto, agregará el nuevo controlador a la carpeta de **Controladores** existentes. Haga clic con el botón derecho en la carpeta **Controllers** y seleccione **Agregar** > **nuevo elemento con scaffolding**.

Seleccione el **controlador de MVC 5 con vistas con Entity Framework** opción. Esta opción generará el controlador y las vistas para actualizar, eliminar, crear y mostrar los datos en el modelo.

![Agregar controlador de MVC](generating-views/_static/image2.png)

Seleccione **Student (ContosoSite. Models)** para la clase Model y seleccione **ContosoUniversityDataEntities (ContosoSite. Models)** para la clase context. Mantenga el nombre del controlador como **StudentsController**.

Haga clic en **Agregar**.

Si recibe un error, puede deberse a que no ha compilado el proyecto en la sección anterior. Si es así, intente compilar el proyecto y, a continuación, vuelva a agregar el elemento con scaffolding.

Una vez completado el proceso de generación de código, verá un nuevo controlador y vistas en los **Controladores** y **vistas** de proyecto > carpetas de **estudiantes** .

Vuelva a realizar los mismos pasos, pero agregue un scaffolding a la clase **Enrollment** . Cuando termine, tendrá un archivo **EnrollmentsController.CS** y una carpeta en **views** denominado **inscripciones** con las vistas crear, eliminar, detalles, editar e índice.

## <a name="add-links-to-new-views"></a>Agregar vínculos a nuevas vistas

Para facilitar la navegación a las nuevas vistas, puede Agregar un par de hipervínculos a las vistas de índice para estudiantes e inscripciones. Abra el archivo en **views** > **Home** > *index. cshtml*, que es la Página principal de su sitio. Agregue el código siguiente debajo de Jumbotron.

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

En el caso del método ActionLink, el primer parámetro es el texto que se va a mostrar en el vínculo. El segundo parámetro es la acción y el tercer parámetro es el nombre del controlador. Por ejemplo, el primer vínculo apunta a la acción de índice en StudentsController. El hipervínculo real se construye a partir de estos valores. En última instancia, el primer vínculo lleva a los usuarios al archivo **index. cshtml** dentro de la carpeta **views/Students** .

## <a name="display-student-views"></a>Mostrar vistas de estudiante

Comprobará que el código agregado al proyecto muestra correctamente una lista de los alumnos y permite a los usuarios editar, crear o eliminar los registros de estudiante en la base de datos.

Haga clic con el botón derecho en el archivo **views** > **Home** > *index. cshtml* y seleccione **ver en el explorador**. En la Página principal de la aplicación, seleccione **lista de alumnos**.

![](generating-views/_static/image6.png)

En la página **Índice** , observe la lista de estudiantes y vínculos para modificar estos datos. Seleccione el vínculo **crear nuevo** y proporcione algunos valores para un estudiante nuevo. Haga clic en **crear**y observe que el nuevo estudiante se agrega a la lista.

De nuevo en la página de **Índice** , seleccione el vínculo **Editar** y cambie algunos de los valores de un estudiante. Haga clic en **Guardar**y observe que se ha cambiado el registro del alumno.

Por último, seleccione el vínculo **eliminar** y confirme que desea eliminar el registro haciendo clic en el botón **eliminar** .

Sin escribir código, se han agregado vistas que realizan operaciones comunes en los datos de la tabla Student.

Es posible que haya observado que la etiqueta de texto de un campo se basa en la propiedad de base de datos (como **LastName**), que no es necesariamente lo que desea mostrar en la Página Web. Por ejemplo, puede que prefiera que la etiqueta sea **Last Name**. Corregirá este problema de presentación más adelante en el tutorial.

## <a name="display-enrollment-views"></a>Mostrar vistas de inscripción

La base de datos incluye una relación de uno a varios entre las tablas Student e Enrollment y una relación uno a varios entre las tablas Course e Enrollment. Las vistas para la inscripción controlan correctamente estas relaciones. Vaya a la Página principal del sitio y seleccione el vínculo **lista de inscripciones** y, a continuación, el vínculo **crear nuevo** .

La vista muestra un formulario para crear un nuevo registro de inscripción. En concreto, tenga en cuenta que el formulario contiene una lista desplegable **CourseID** y una lista desplegable **StudentID** . Ambos se rellenan con valores de las tablas relacionadas.

Además, la validación de los valores proporcionados se aplica automáticamente en función del tipo de datos del campo. La **calificación** requiere un número, por lo que se muestra un mensaje de error si se intenta proporcionar un valor incompatible: *el campo grado debe ser un número.*

Ha comprobado que las vistas generadas automáticamente permiten a los usuarios trabajar con los datos de la base de datos. En el siguiente tutorial de esta serie, actualizará la base de datos y realizará los cambios correspondientes en la aplicación Web.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial va a:

> [!div class="checklist"]
> * Scaffolding agregado
> * Vínculos agregados a nuevas vistas
> * Vistas de estudiante mostradas
> * Vistas de inscripción mostradas

Avance al siguiente tutorial para obtener información sobre cómo cambiar la base de datos.
> [!div class="nextstepaction"]
> [Cambio de la base de datos](changing-the-database.md)