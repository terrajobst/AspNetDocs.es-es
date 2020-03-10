---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Actualizar, eliminar y crear datos con el enlace de modelos y formularios Web Forms | Microsoft Docs
author: Rick-Anderson
description: En esta serie de tutoriales se muestran los aspectos básicos del uso del enlace de modelos con un proyecto de formularios Web Forms ASP.NET. El enlace de modelos hace que la interacción de datos sea más recta-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 11dc52ec79ca91119b37ea60e08164eb1b1d0e2b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78474139"
---
# <a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a>Actualizar, eliminar y crear datos con enlace de modelos y formularios Web Forms

por [Tom FitzMacken](https://github.com/tfitzmac)

> En esta serie de tutoriales se muestran los aspectos básicos del uso del enlace de modelos con un proyecto de formularios Web Forms ASP.NET. El enlace de modelos hace que la interacción de datos sea más directa que tratar con objetos de origen de datos (como ObjectDataSource o SqlDataSource). Esta serie comienza con material introductorio y se traslada a conceptos más avanzados en los tutoriales posteriores.
> 
> En este tutorial se muestra cómo crear, actualizar y eliminar datos con el enlace de modelos. Establecerá las siguientes propiedades:
> 
> - DeleteMethod
> - InsertMethod
> - UpdateMethod
> 
> Estas propiedades reciben el nombre del método que controla la operación correspondiente. Dentro de ese método, se proporciona la lógica para interactuar con los datos.
> 
> Este tutorial se basa en el proyecto creado en la primera [parte](retrieving-data.md) de la serie.
> 
> Puede [Descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o VB. El código descargable funciona con Visual Studio 2012 o Visual Studio 2013. Usa la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2013 que se muestra en este tutorial.

## <a name="what-youll-build"></a>Lo que va a compilar

En este tutorial, hará lo siguiente:

1. Agregar plantillas de datos dinámicos
2. Habilitar la actualización y eliminación de datos mediante los métodos de enlace de modelos
3. Aplicar reglas de validación de datos: habilitar la creación de un nuevo registro en la base de datos

## <a name="add-dynamic-data-templates"></a>Agregar plantillas de datos dinámicos

Para proporcionar la mejor experiencia de usuario y minimizar la repetición de código, utilizará plantillas de datos dinámicos. Puede integrar fácilmente plantillas de datos dinámicos predefinidas en el sitio existente mediante la instalación de un paquete de NuGet.

En **administrar paquetes NuGet**, instale **DynamicDataTemplatesCS**.

![plantillas de datos dinámicos](updating-deleting-and-creating-data/_static/image1.png)

Observe que el proyecto incluye ahora una carpeta denominada **DynamicData**. En esa carpeta, encontrará las plantillas que se aplican automáticamente a los controles dinámicos de los formularios Web Forms.

![carpeta de datos dinámicos](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a>Habilitar la actualización y eliminación

Permitir a los usuarios actualizar y eliminar registros en la base de datos es muy similar al proceso de recuperación de datos. En las propiedades **UpdateMethod** y **DeleteMethod** , especifique los nombres de los métodos que realizan esas operaciones. Con un control GridView, también puede especificar la generación automática de botones de edición y eliminación. En el código resaltado siguiente se muestran las adiciones al código GridView.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

En el archivo de código subyacente, agregue una instrucción using para **System. Data. Entity. Infrastructure**.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

A continuación, agregue los siguientes métodos de actualización y eliminación.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

El método **TryUpdateModel** aplica los valores enlazados a datos coincidentes del formulario web al elemento de datos. El elemento de datos se recupera según el valor del parámetro id.

## <a name="enforce-validation-requirements"></a>Exigir requisitos de validación

Los atributos de validación que se aplican a las propiedades FirstName, LastName y Year de la clase Student se aplican automáticamente al actualizar los datos. Los controles DynamicField agregan validadores de cliente y servidor basados en los atributos de validación. Las propiedades FirstName y LastName son obligatorias. FirstName no puede tener más de 20 caracteres de longitud y LastName no puede superar los 40 caracteres. Year debe ser un valor válido para la enumeración AcademicYear.

Si el usuario infringe uno de los requisitos de validación, la actualización no continúa. Para ver el mensaje de error, agregue un control ValidationSummary sobre GridView. Para mostrar los errores de validación del enlace de modelos, establezca la propiedad **ShowModelStateErrors** establecida en **true**. 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

Ejecute la aplicación web y actualice y elimine cualquiera de los registros.

![actualizar datos](updating-deleting-and-creating-data/_static/image3.png)

Observe que, cuando en el modo de edición, el valor de la propiedad Year se representa automáticamente como una lista desplegable. La propiedad Year es un valor de enumeración y la plantilla de datos dinámicos para un valor de enumeración especifica una lista desplegable para su edición. Para encontrar esa plantilla, abra la **enumeración\_archivo Edit. ascx** en la carpeta **DynamicData**/**FieldTemplates**

Si proporciona valores válidos, la actualización se completará correctamente. Si infringe uno de los requisitos de validación, la actualización no continúa y se muestra un mensaje de error sobre la cuadrícula.

![mensaje de error](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a>Agregar nuevos registros

El control GridView no incluye la propiedad **InsertMethod** y, por tanto, no se puede usar para agregar un nuevo registro con el enlace de modelos. Puede encontrar la propiedad InsertMethod en los controles **FormView**, **DetailsView**o **ListView** . En este tutorial, usará un control FormView para agregar un nuevo registro.

En primer lugar, agregue un vínculo a la nueva página que creará para agregar un nuevo registro. Encima de ValidationSummary, agregue:

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

El nuevo vínculo aparecerá en la parte superior del contenido de la página Students.

![nuevo vínculo](updating-deleting-and-creating-data/_static/image5.png)

Después, agregue un nuevo formulario Web Forms mediante una página maestra y asígnele el nombre **AddStudent**. Seleccione site. Master como la página maestra.

Se representarán los campos para agregar un estudiante nuevo mediante un control **DynamicEntity** . El control DynamicEntity representa las propiedades editables de la clase especificada en la propiedad ItemType. La propiedad StudentID se marcó con el atributo **[ScaffoldColumn (false)]** para que no se represente. En el marcador de posición MainContent de la página AddStudent, agregue el siguiente código.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

En el archivo de código subyacente (AddStudent.aspx.cs), agregue una instrucción **using** para el espacio de nombres **ContosoUniversityModelBinding. Models** .

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

A continuación, agregue los métodos siguientes para especificar cómo se inserta un nuevo registro y un controlador de eventos para el botón Cancelar.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

Guarde todos los cambios.

Ejecute la aplicación web y cree un estudiante nuevo.

![Agregar nuevo estudiante](updating-deleting-and-creating-data/_static/image6.png)

Haga clic en **Insertar** y observe que se ha creado el estudiante nuevo.

![Mostrar nuevo estudiante](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a>Conclusión

En este tutorial, ha habilitado la actualización, la eliminación y la creación de datos. Las reglas de validación garantizadas se aplican al interactuar con los datos.

En el siguiente [tutorial](sorting-paging-and-filtering-data.md) de esta serie, podrá habilitar la ordenación, la paginación y el filtrado de datos.

> [!div class="step-by-step"]
> [Anterior](retrieving-data.md)
> [Siguiente](sorting-paging-and-filtering-data.md)
