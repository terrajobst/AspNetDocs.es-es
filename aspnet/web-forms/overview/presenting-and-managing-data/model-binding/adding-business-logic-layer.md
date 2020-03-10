---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Agregar capa de lógica de negocios a un proyecto que utiliza el enlace de modelos y formularios Web Forms | Microsoft Docs
author: Rick-Anderson
description: En esta serie de tutoriales se muestran los aspectos básicos del uso del enlace de modelos con un proyecto de formularios Web Forms ASP.NET. El enlace de modelos hace que la interacción de datos sea más recta-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: a824d06d3781e11706f2a48d44ea3ad89bdb7c8b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78515431"
---
# <a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a>Agregar capa de lógica de negocios a un proyecto que usa el enlace de modelos y formularios Web Forms

por [Tom FitzMacken](https://github.com/tfitzmac)

> En esta serie de tutoriales se muestran los aspectos básicos del uso del enlace de modelos con un proyecto de formularios Web Forms ASP.NET. El enlace de modelos hace que la interacción de datos sea más directa que tratar con objetos de origen de datos (como ObjectDataSource o SqlDataSource). Esta serie comienza con material introductorio y se traslada a conceptos más avanzados en los tutoriales posteriores.
> 
> En este tutorial se muestra cómo usar el enlace de modelos con una capa de lógica de negocios. Establecerá el miembro OnCallingDataMethods para especificar que un objeto distinto de la página actual se usa para llamar a los métodos de datos.
> 
> Este tutorial se basa en el proyecto creado en las partes [anteriores](retrieving-data.md) de la serie.
> 
> Puede [Descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o VB. El código descargable funciona con Visual Studio 2012 o Visual Studio 2013. Usa la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2013 que se muestra en este tutorial.

## <a name="what-youll-build"></a>Lo que va a compilar

El enlace de modelos permite colocar el código de interacción de datos en el archivo de código subyacente de una página web o en una clase de lógica de negocios independiente. En los tutoriales anteriores se ha mostrado cómo usar los archivos de código subyacente para el código de interacción de datos. Este enfoque funciona para sitios pequeños, pero puede dar lugar a repeticiones de código y mayor dificultad al mantener un sitio de gran tamaño. También puede ser muy difícil probar mediante programación el código que reside en los archivos de código subyacente, ya que no hay ninguna capa de abstracción.

Para centralizar el código de interacción de datos, puede crear una capa de lógica de negocios que contenga toda la lógica para interactuar con los datos. A continuación, llame a la capa de lógica de negocios desde las páginas Web. En este tutorial se muestra cómo trasladar todo el código que ha escrito en los tutoriales anteriores a una capa de lógica de negocios y, a continuación, usar ese código desde las páginas.

En este tutorial, hará lo siguiente:

1. Trasladar el código de los archivos de código subyacente a una capa de lógica de negocios
2. Cambiar los controles enlazados a datos para llamar a los métodos en el nivel de lógica de negocios

## <a name="create-business-logic-layer"></a>Crear capa de lógica de negocios

Ahora, creará la clase a la que se llama desde las páginas Web. Los métodos de esta clase tienen un aspecto similar al de los métodos usados en los tutoriales anteriores e incluyen los atributos de proveedor de valores.

En primer lugar, agregue una nueva carpeta denominada **BLL**.

![Agregar carpeta](adding-business-logic-layer/_static/image1.png)

En la carpeta BLL, cree una nueva clase denominada **SchoolBL.CS**. Contendrá todas las operaciones de datos que residían originalmente en los archivos de código subyacente. Los métodos son prácticamente los mismos que los métodos del archivo de código subyacente, pero incluirán algunos cambios.

El cambio más importante que hay que tener en cuenta es que ya no se está ejecutando el código desde una instancia de la clase **Page** . La clase Page contiene el método **TryUpdateModel** y la propiedad **ModelState** . Cuando este código se mueve a una capa de lógica de negocios, ya no tiene una instancia de la clase Page para llamar a estos miembros. Para solucionar este problema, debe agregar un parámetro **ModelMethodContext** a cualquier método que tenga acceso a TryUpdateModel o ModelState. Este parámetro ModelMethodContext se usa para llamar a TryUpdateModel o recuperar ModelState. No es necesario cambiar nada en la página web para tener en cuenta este nuevo parámetro.

Reemplace el código de SchoolBL.cs por el código siguiente.

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a>Revisar las páginas existentes para recuperar datos de la capa de lógica de negocios

Por último, convertirá las páginas Students. aspx, AddStudent. aspx y Courses. aspx del uso de las consultas del archivo de código subyacente en el uso del nivel de lógica de negocios.

En los archivos de código subyacente para estudiantes, AddStudent y cursos, elimine o comente los siguientes métodos de consulta:

- studentsGrid\_GetData
- studentsGrid\_UpdateItem
- studentsGrid\_DeleteItem
- addStudentForm\_InsertItem
- coursesGrid\_GetData

Ahora no debe tener ningún código en el archivo de código subyacente que pertenezca a las operaciones de datos.

El controlador de eventos **OnCallingDataMethods** le permite especificar un objeto que se usará para los métodos de datos. En Students. aspx, agregue un valor para ese controlador de eventos y cambie los nombres de los métodos de datos a los nombres de los métodos de la clase de lógica de negocios.

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

En el archivo de código subyacente para Students. aspx, defina el controlador de eventos para el evento CallingDataMethods. En este controlador de eventos, se especifica la clase de lógica de negocios para las operaciones de datos.

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

En AddStudent. aspx, realice cambios similares.

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

En Courses. aspx, realice cambios similares.

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

Ejecute la aplicación y observe que todas las páginas funcionan como tenían anteriormente. La lógica de validación también funciona correctamente.

## <a name="conclusion"></a>Conclusión

En este tutorial, reestructure la aplicación para usar una capa de acceso a datos y una capa de lógica de negocios. Especificó que los controles de datos usan un objeto que no es la página actual para las operaciones de datos.

> [!div class="step-by-step"]
> [Anterior](using-query-string-values-to-retrieve-data.md)
