---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Usar valores de cadena de consulta para filtrar datos con el enlace de modelos y formularios Web Forms | Microsoft Docs
author: Rick-Anderson
description: En esta serie de tutoriales se muestran los aspectos básicos del uso del enlace de modelos con un proyecto de formularios Web Forms ASP.NET. El enlace de modelos hace que la interacción de datos sea más recta-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 143ddcb40b576a3129e659b90bfc8321c061a547
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78519085"
---
# <a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a>Usar valores de cadena de consulta para filtrar datos con el enlace de modelos y formularios Web Forms

por [Tom FitzMacken](https://github.com/tfitzmac)

> En esta serie de tutoriales se muestran los aspectos básicos del uso del enlace de modelos con un proyecto de formularios Web Forms ASP.NET. El enlace de modelos hace que la interacción de datos sea más directa que tratar con objetos de origen de datos (como ObjectDataSource o SqlDataSource). Esta serie comienza con material introductorio y se traslada a conceptos más avanzados en los tutoriales posteriores.
> 
> En este tutorial se muestra cómo pasar un valor en la cadena de consulta y usar ese valor para recuperar datos a través del enlace de modelos.
> 
> Este tutorial se basa en el proyecto creado en las partes [anteriores](retrieving-data.md) de la serie.
> 
> Puede [Descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o VB. El código descargable funciona con Visual Studio 2012 o Visual Studio 2013. Usa la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2013 que se muestra en este tutorial.

## <a name="what-youll-build"></a>Lo que va a compilar

En este tutorial, hará lo siguiente:

1. Agregar una nueva página para mostrar los cursos inscritos de un estudiante
2. Recuperar los cursos inscritos del estudiante seleccionado en función de un valor de la cadena de consulta
3. Agregar un hipervínculo con un valor de cadena de consulta de la vista de cuadrícula a la nueva página

Los pasos de este tutorial son bastante similares a lo que hizo en el [tutorial](sorting-paging-and-filtering-data.md) anterior para filtrar los estudiantes mostrados en función de la selección del usuario en una lista desplegable. En ese tutorial, ha usado el atributo **control** en el método SELECT para especificar que el valor del parámetro procede de un control. En este tutorial, usará el atributo **QueryString** en el método SELECT para especificar que el valor del parámetro procede de la cadena de consulta.

## <a name="add-new-page-for-displaying-a-students-courses"></a>Agregar una nueva página para mostrar los cursos de un estudiante

Agregue un nuevo formulario Web Forms que use la página maestra site. Master y asigne el nombre **cursos**a la página.

En el archivo **Courses. aspx** , agregue una vista de cuadrícula para mostrar los cursos del alumno seleccionado.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a>Definir el método Select

En **Courses.aspx.CS**, agregará el método Select con el nombre especificado en la propiedad **SelectMethod** de la vista de cuadrícula. En ese método, definirá la consulta para recuperar los cursos de un estudiante y especificará que el parámetro proviene de un valor de cadena de consulta con el mismo nombre que el parámetro.

En primer lugar, debe agregar las siguientes instrucciones **using** .

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

A continuación, agregue el código siguiente a Courses.aspx.cs:

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

El atributo QueryString significa que un valor de cadena de consulta denominado StudentID se asigna automáticamente al parámetro de este método.

## <a name="add-hyperlink-with-query-string-value"></a>Agregar hipervínculo con valor de cadena de consulta

En la vista de cuadrícula de Students. aspx, agregará un campo de hipervínculo que se vincula a la página nuevos cursos. El hipervínculo incluirá un valor de cadena de consulta con el identificador del alumno.

En Students. aspx, agregue el siguiente campo a las columnas de la vista de cuadrícula justo debajo del campo para el total de créditos.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

Ejecute la aplicación y observe que la vista de cuadrícula incluye ahora el vínculo cursos.

![Agregar hipervínculo](using-query-string-values-to-retrieve-data/_static/image1.png)

Al hacer clic en uno de los vínculos, verá los cursos inscritos de los estudiantes.

![Mostrar cursos](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a>Conclusión

En este tutorial, ha agregado un vínculo con un valor de cadena de consulta. Usó ese valor de cadena de consulta para el valor del parámetro en el método Select.

En el siguiente [tutorial](adding-business-logic-layer.md), moverá el código de los archivos de código subyacente a una capa de lógica de negocios y una capa de acceso a datos.

> [!div class="step-by-step"]
> [Anterior](integrating-jquery-ui.md)
> [Siguiente](adding-business-logic-layer.md)
