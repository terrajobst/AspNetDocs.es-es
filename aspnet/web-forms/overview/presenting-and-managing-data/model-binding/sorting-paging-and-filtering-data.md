---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Ordenar, paginar y filtrar datos con el enlace de modelos y formularios Web Forms | Microsoft Docs
author: Rick-Anderson
description: En esta serie de tutoriales se muestran los aspectos básicos del uso del enlace de modelos con un proyecto de formularios Web Forms ASP.NET. El enlace de modelos hace que la interacción de datos sea más recta-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: f8e64392af6110f36c6af98c4e4e9481c94a0d82
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78441067"
---
# <a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a>Ordenar, paginar y filtrar datos con el enlace de modelos y formularios Web Forms

por [Tom FitzMacken](https://github.com/tfitzmac)

> En esta serie de tutoriales se muestran los aspectos básicos del uso del enlace de modelos con un proyecto de formularios Web Forms ASP.NET. El enlace de modelos hace que la interacción de datos sea más directa que tratar con objetos de origen de datos (como ObjectDataSource o SqlDataSource). Esta serie comienza con material introductorio y se traslada a conceptos más avanzados en los tutoriales posteriores.
> 
> En este tutorial se muestra cómo agregar ordenación, paginación y filtrado de los datos a través del enlace de modelos.
> 
> Este tutorial se basa en el proyecto creado en la primera [parte](retrieving-data.md) de la serie.
> 
> Puede [Descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o VB. El código descargable funciona con Visual Studio 2012 o Visual Studio 2013. Usa la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2013 que se muestra en este tutorial.

## <a name="what-youll-build"></a>Lo que va a compilar

En este tutorial, hará lo siguiente:

1. Habilitar la ordenación y paginación de los datos
2. Habilitar el filtrado de los datos en función de una selección por parte del usuario

## <a name="add-sorting"></a>Adición de ordenación

Habilitar la ordenación en GridView es muy fácil. En el archivo Student. aspx, simplemente establezca **AllowSorting** en **true** en GridView. No es necesario establecer un valor de **SortExpression** para cada columna, ya que el campo de campos se usa automáticamente. GridView modifica la consulta para incluir el orden de los datos por el valor seleccionado. En el código resaltado siguiente se muestra la adición que debe hacer para habilitar la ordenación.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

Ejecutar la aplicación web y probar la ordenación de los registros de estudiante por los valores de columnas diferentes.

![ordenar estudiantes](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a>Adición de paginación

También es muy fácil habilitar la paginación. En GridView, establezca la propiedad **AllowPaging** en **true** y establezca la propiedad **pageSize** en el número de registros que desea mostrar en cada página. En este tutorial, puede establecerlo en 4.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

Ejecute la aplicación web y observe que ahora los registros se dividen en varias páginas con un máximo de 4 registros en una sola página.

![Agregar paginación](sorting-paging-and-filtering-data/_static/image4.png)

La ejecución diferida de consultas mejora la eficacia de la aplicación. En lugar de recuperar todo el conjunto de datos, GridView modifica la consulta para recuperar solo los registros de la página actual.

## <a name="filter-records-by-user-selection"></a>Filtrar registros por selección de usuario

El enlace de modelos agrega varios atributos que le permiten designar cómo establecer el valor de un parámetro en un método de enlace de modelos. Estos atributos están en el espacio de nombres **System. Web. ModelBinding** . Son los siguientes:

- Control
- Cookie
- Form
- Perfil
- QueryString
- RouteData
- Sesión
- Perfil
- ViewState

En este tutorial, usará el valor de un control para filtrar los registros que se muestran en GridView. Agregará el atributo de **control** al método de consulta que creó anteriormente. En un tutorial [posterior](using-query-string-values-to-retrieve-data.md) , se aplicará el atributo **QueryString** a un parámetro para especificar que el valor del parámetro proviene de un valor de cadena de consulta.

En primer lugar, sobre ValidationSummary, agregue una lista desplegable para filtrar los estudiantes que se muestran.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

En el archivo de código subyacente, modifique el método SELECT para recibir un valor del control y establezca el nombre del parámetro en el nombre del control que proporciona el valor.

Debe agregar una instrucción **using** para el espacio de nombres **System. Web. ModelBinding** para resolver el atributo de control.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

En el código siguiente se muestra que el método Select ha vuelto a trabajar para filtrar los datos devueltos en función del valor de la lista desplegable. Al agregar un atributo de control antes de un parámetro, se especifica que el valor de este parámetro proviene de un control con el mismo nombre.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

Ejecute la aplicación web y seleccione valores diferentes en la lista desplegable para filtrar la lista de alumnos.

![filtrar estudiantes](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a>Conclusión

En este tutorial, ha habilitado la ordenación y la paginación de los datos. También ha habilitado el filtrado de los datos por el valor de un control.

En el siguiente [tutorial](integrating-jquery-ui.md) , mejorará la interfaz de usuario mediante la integración de un widget de jQuery UI en la plantilla de datos dinámicos.

> [!div class="step-by-step"]
> [Anterior](updating-deleting-and-creating-data.md)
> [Siguiente](integrating-jquery-ui.md)
