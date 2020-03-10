---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'Tutorial: mejora de la validación de datos para EF Database First con la aplicación ASP.NET MVC'
description: Este tutorial se centra en agregar anotaciones de datos al modelo de datos para especificar los requisitos de validación y el formato de presentación.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 897cd7c6a40445e2a4abede50d81e101372d3233
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78499537"
---
# <a name="tutorial-enhance-data-validation-for-ef-database-first-with-aspnet-mvc-app"></a>Tutorial: mejora de la validación de datos para EF Database First con la aplicación ASP.NET MVC

Con el scaffolding de MVC, Entity Framework y ASP.NET, puede crear una aplicación web que proporcione una interfaz a una base de datos existente. En esta serie de tutoriales se muestra cómo generar automáticamente código que permite a los usuarios mostrar, editar, crear y eliminar datos que residen en una tabla de base de datos. El código generado corresponde a las columnas de la tabla de base de datos.

Este tutorial se centra en agregar anotaciones de datos al modelo de datos para especificar los requisitos de validación y el formato de presentación. Se mejoró en función de los comentarios de los usuarios en la sección de comentarios.

En este tutorial va a:

> [!div class="checklist"]
> * Agregar anotaciones de datos
> * Agregar clases de metadatos

## <a name="prerequisites"></a>Requisitos previos

* [Personalización de una vista](customizing-a-view.md)

## <a name="add-data-annotations"></a>Agregar anotaciones de datos

Como vimos en un tema anterior, algunas reglas de validación de datos se aplican automáticamente a los datos proporcionados por el usuario. Por ejemplo, solo puede proporcionar un número para la propiedad grade. Para especificar más reglas de validación de datos, puede agregar anotaciones de datos a la clase de modelo. Estas anotaciones se aplican en toda la aplicación web para la propiedad especificada. También puede aplicar atributos de formato que cambian el modo en que se muestran las propiedades; por ejemplo, cambiar el valor utilizado para las etiquetas de texto.

En este tutorial, agregará anotaciones de datos para restringir la longitud de los valores proporcionados para las propiedades FirstName, LastName y MiddleName. En la base de datos, estos valores se limitan a 50 caracteres; sin embargo, en la aplicación web no se aplica actualmente el límite de caracteres. Si un usuario proporciona más de 50 caracteres para uno de estos valores, la página se bloqueará al intentar guardar el valor en la base de datos. También se puede restringir el nivel a los valores entre 0 y 4.

Seleccione **modelos** > **ContosoModel. edmx** > **ContosoModel.tt** y abra el archivo *Student.CS* . Agregue el siguiente código resaltado a la clase.

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

Abra *Enrollment.CS* y agregue el siguiente código resaltado.

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

Compile la solución.

Haga clic en **lista de estudiantes** y seleccione **Editar**. Si intenta escribir más de 50 caracteres, se muestra un mensaje de error.

![Mostrar mensaje de error](enhancing-data-validation/_static/image1.png)

Vuelva a la Página principal. Haga clic en la **lista de inscripciones** y seleccione **Editar**. Intente proporcionar una calificación superior a 4. Recibirá este error: *el campo grado debe estar entre 0 y 4.*

## <a name="add-metadata-classes"></a>Agregar clases de metadatos

La adición de atributos de validación directamente a la clase de modelo funciona cuando no se espera que la base de datos cambie; sin embargo, si la base de datos cambia y necesita volver a generar la clase de modelo, perderá todos los atributos que se han aplicado a la clase de modelo. Este enfoque puede ser muy ineficaz y propenso a perder reglas de validación importantes.

Para evitar este problema, puede Agregar una clase de metadatos que contenga los atributos. Al asociar la clase de modelo a la clase de metadatos, esos atributos se aplican al modelo. En este enfoque, la clase de modelo se puede volver a generar sin perder todos los atributos que se han aplicado a la clase de metadatos.

En la carpeta **Models** , agregue una clase denominada *Metadata.CS*.

Reemplace el código de *Metadata.CS* por el código siguiente.

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

Estas clases de metadatos contienen todos los atributos de validación que se aplicaron previamente a las clases de modelo. El atributo **Display** se usa para cambiar el valor que se usa para las etiquetas de texto.

Ahora, debe asociar las clases de modelo a las clases de metadatos.

En la carpeta **Models** , agregue una clase denominada *PartialClasses.CS*.

Reemplace el contenido del archivo por el código siguiente.

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

Tenga en cuenta que cada clase se marca como una clase `partial`, y cada una coincide con el nombre y el espacio de nombres como la clase que se genera automáticamente. Al aplicar el atributo de metadatos a la clase parcial, se asegura de que los atributos de validación de datos se aplicarán a la clase generada automáticamente. Estos atributos no se perderán al volver a generar las clases de modelo porque el atributo de metadatos se aplica en clases parciales que no se vuelven a generar.

Para volver a generar las clases generadas automáticamente, abra el archivo *ContosoModel. edmx* . Una vez más, haga clic con el botón secundario en la superficie de diseño y seleccione **Actualizar modelo desde base de datos**. Aunque no haya cambiado la base de datos, este proceso volverá a generar las clases. En la pestaña **Actualizar** , seleccione **tablas** y **Finalizar**.

Guarde el archivo *ContosoModel. edmx* para aplicar los cambios.

Abra el archivo *Student.CS* o el archivo *Enrollment.CS* y observe que los atributos de validación de datos que aplicó anteriormente ya no están en el archivo. Sin embargo, ejecute la aplicación y observe que las reglas de validación se siguen aplicando al escribir los datos.

## <a name="conclusion"></a>Conclusión

Esta serie proporciona un ejemplo sencillo de cómo generar código a partir de una base de datos existente que permite a los usuarios editar, actualizar, crear y eliminar datos. Usó ASP.NET MVC 5, Entity Framework y ASP.NET scaffolding para crear el proyecto. 

Para obtener un ejemplo introductorio del desarrollo de Code First, consulte [Introducción con ASP.NET MVC 5](../introduction/getting-started.md). 

Para obtener un ejemplo más avanzado, vea [creación de un modelo de datos de Entity Framework para una aplicación ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Tenga en cuenta que la API DbContext que se usa para trabajar con datos en Database First es la misma que la API que se usa para trabajar con datos en Code First. Incluso si piensa usar Database First, puede obtener información sobre cómo controlar escenarios más complejos, como la lectura y la actualización de datos relacionados, el control de conflictos de simultaneidad, etc., desde un tutorial de Code First. La única diferencia radica en cómo se crean la base de datos, la clase de contexto y las clases de entidad.

## <a name="additional-resources"></a>Recursos adicionales

Para obtener una lista completa de las anotaciones de validación de datos que se pueden aplicar a las propiedades y las clases, vea [System. ComponentModel. DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial va a:

> [!div class="checklist"]
> * Anotaciones de datos agregadas
> * Clases de metadatos agregadas

Para obtener información sobre cómo implementar una aplicación web y SQL Database en Azure App Service, consulte este tutorial:
> [!div class="nextstepaction"]
> [Implementación de una aplicación .NET en Azure App Service](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase/)
