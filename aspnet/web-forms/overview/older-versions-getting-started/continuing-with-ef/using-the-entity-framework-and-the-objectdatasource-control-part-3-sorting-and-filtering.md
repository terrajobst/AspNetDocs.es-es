---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Usar el Entity Framework 4,0 y el control ObjectDataSource, parte 3: ordenación y filtrado | Microsoft Docs'
author: tdykstra
description: Esta serie de tutoriales se basa en la aplicación web contoso University que crea el Introducción con la serie de tutoriales Entity Framework 4,0. I...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 603120864528b9a5ff81214270eb9a7f1b68b347
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78512719"
---
# <a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a>Usar el Entity Framework 4,0 y el control ObjectDataSource, parte 3: ordenación y filtrado

por [Tom Dykstra](https://github.com/tdykstra)

> Esta serie de tutoriales se basa en la aplicación web contoso University que crea el [Introducción con la](https://asp.net/entity-framework/tutorials#Getting%20Started) serie de tutoriales Entity Framework 4,0. Si no completó los tutoriales anteriores, como punto de partida para este tutorial, puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que habría creado. También puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) que se crea en la serie completa de tutoriales. Si tiene alguna pregunta sobre los tutoriales, puede publicarlos en el [Foro de Entity Framework de ASP.net](https://forums.asp.net/1227.aspx).

En el tutorial anterior, implementó el patrón del repositorio en una aplicación Web de n niveles que utiliza el Entity Framework y el control `ObjectDataSource`. En este tutorial se muestra cómo realizar tareas de ordenación y filtrado y cómo controlar los escenarios de detalles principales. Agregará las siguientes mejoras a la página *departments. aspx* :

- Un cuadro de texto para permitir que los usuarios seleccionen departamentos por nombre.
- Una lista de cursos para cada departamento que se muestra en la cuadrícula.
- La capacidad de ordenar haciendo clic en los encabezados de columna.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)

## <a name="adding-the-ability-to-sort-gridview-columns"></a>Agregar la capacidad de ordenar las columnas de GridView

Abra la página *departments. aspx* y agregue un atributo `SortParameterName="sortExpression"` al control `ObjectDataSource` denominado `DepartmentsObjectDataSource`. (Más adelante creará un método `GetDepartments` que toma un parámetro denominado `sortExpression`). El marcado para la etiqueta de apertura del control ahora es similar al ejemplo siguiente.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

Agregue el atributo `AllowSorting="true"` a la etiqueta de apertura del control `GridView`. El marcado para la etiqueta de apertura del control ahora es similar al ejemplo siguiente.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

En *departments.aspx.CS*, establezca el criterio de ordenación predeterminado llamando al método `Sort` del control `GridView` desde el método `Page_Load`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

Puede agregar código que ordena o filtra en la clase de lógica de negocios o en la clase de repositorio. Si lo hace en la clase de lógica de negocios, el trabajo de ordenación o filtrado se realizará después de que se recuperen los datos de la base de datos, porque la clase de lógica de negocios está trabajando con un `IEnumerable` objeto devuelto por el repositorio. Si agrega código de ordenación y filtrado en la clase de repositorio y lo hace antes de que una expresión LINQ o una consulta de objeto se haya convertido en un objeto `IEnumerable`, los comandos se pasarán a la base de datos para su procesamiento, que suele ser más eficaz. En este tutorial, va a implementar la ordenación y el filtrado de una manera que hace que la base de datos realice el procesamiento, es decir, en el repositorio.

Para agregar capacidad de ordenación, debe agregar un nuevo método a la interfaz del repositorio y a las clases de repositorio, así como a la clase de lógica de negocios. En el archivo *ISchoolRepository.CS* , agregue un nuevo método de `GetDepartments` que toma un parámetro `sortExpression` que se utilizará para ordenar la lista de departamentos devueltos:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

El parámetro `sortExpression` especificará la columna por la que se va a ordenar y la dirección de ordenación.

Agregue el código para el nuevo método al archivo *SchoolRepository.CS* :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

Cambie el método de `GetDepartments` sin parámetros existente para llamar al nuevo método:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

En el proyecto de prueba, agregue el siguiente método nuevo a *MockSchoolRepository.CS*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

Si va a crear cualquier prueba unitaria que dependa de este método para devolver una lista ordenada, deberá ordenar la lista antes de devolverla. No creará pruebas como esta en este tutorial, por lo que el método solo puede devolver la lista sin ordenar de departamentos.

En el archivo *SchoolBL.CS* , agregue el siguiente método nuevo a la clase de lógica de negocios:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

Este código pasa el parámetro de ordenación al método de repositorio.

Ejecute la página *departments. aspx* .

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)

Ahora puede hacer clic en cualquier encabezado de columna para ordenar por esa columna. Si la columna ya está ordenada, al hacer clic en el encabezado se invierte la dirección de ordenación.

## <a name="adding-a-search-box"></a>Agregar un cuadro de búsqueda

En esta sección, agregará un cuadro de texto de búsqueda, lo vinculará al control `ObjectDataSource` mediante un parámetro de control y agregará un método a la clase de lógica de negocios para admitir el filtrado.

Abra la página *departments. aspx* y agregue el siguiente marcado entre el encabezado y el primer control `ObjectDataSource`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

En el control `ObjectDataSource` denominado `DepartmentsObjectDataSource`, haga lo siguiente:

- Agregue un elemento `SelectParameters` para un parámetro denominado `nameSearchString` que obtiene el valor especificado en el control `SearchTextBox`.
- Cambie el valor del atributo `SelectMethod` a `GetDepartmentsByName`. (Creará este método más adelante).

El marcado del control `ObjectDataSource` ahora es similar al ejemplo siguiente:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

En *ISchoolRepository.CS*, agregue un método `GetDepartmentsByName` que toma los parámetros `sortExpression` y `nameSearchString`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

En *SchoolRepository.CS*, agregue el siguiente método nuevo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

Este código usa un método `Where` para seleccionar los elementos que contienen la cadena de búsqueda. Si la cadena de búsqueda está vacía, se seleccionarán todos los registros. Tenga en cuenta que cuando se especifican llamadas a métodos juntas en una instrucción como esta (`Include`, a continuación, se `OrderBy``Where`), el método de `Where` siempre debe ser el último.

Cambie el método de `GetDepartments` existente que toma un parámetro `sortExpression` para llamar al nuevo método:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

En *MockSchoolRepository.CS* en el proyecto de prueba, agregue el siguiente método nuevo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

En *SchoolBL.CS*, agregue el siguiente método nuevo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

Ejecute la página *departments. aspx* y escriba una cadena de búsqueda para asegurarse de que la lógica de selección funciona. Deje vacío el cuadro de texto e intente realizar una búsqueda para asegurarse de que se devuelven todos los registros.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)

## <a name="adding-a-details-column-for-each-grid-row"></a>Agregar una columna de detalles para cada fila de la cuadrícula

A continuación, desea ver todos los cursos de cada departamento mostrados en la celda derecha de la cuadrícula. Para ello, utilizará un control `GridView` anidado y lo enlazará a los datos de la propiedad de navegación `Courses` de la entidad `Department`.

Abra *departments. aspx* y, en el marcado del control `GridView`, especifique un controlador para el evento `RowDataBound`. El marcado para la etiqueta de apertura del control ahora es similar al ejemplo siguiente.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

Agregue un nuevo elemento `TemplateField` después del campo de plantilla `Administrator`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

Este marcado crea un control `GridView` anidado que muestra el número de curso y el título de una lista de cursos. No especifica ningún origen de datos porque lo enlazará en el código del controlador de `RowDataBound`.

Abra *departments.aspx.CS* y agregue el siguiente controlador para el evento `RowDataBound`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

Este código obtiene la entidad `Department` de los argumentos del evento, convierte la propiedad de navegación `Courses` en una colección `List` y enlaza el `GridView` anidado a la colección.

Abra el archivo *SchoolRepository.CS* y especifique la carga diligente para la propiedad de navegación `Courses` llamando al método `Include` en la consulta de objeto que cree en el método `GetDepartmentsByName`. La instrucción `return` del método `GetDepartmentsByName` ahora es similar al ejemplo siguiente.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

Ejecute la página. Además de la funcionalidad de ordenación y filtrado que agregó anteriormente, el control GridView ahora muestra detalles de los cursos anidados para cada departamento.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)

Esto completa la introducción a los escenarios de ordenación, filtrado y detalle principal. En el siguiente tutorial, verá cómo controlar la simultaneidad.

> [!div class="step-by-step"]
> [Anterior](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [Siguiente](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
