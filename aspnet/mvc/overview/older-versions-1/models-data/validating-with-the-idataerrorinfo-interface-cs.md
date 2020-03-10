---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
title: Validar con la interfaz IDataErrorInfo (C#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther muestra cómo mostrar mensajes de error de validación personalizados implementando la interfaz IDataErrorInfo en una clase de modelo.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 4733b9f1-9999-48fb-8b73-6038fbcc5ecb
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 938b180da02b1963acffd021d18621d75d1d0447
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78436351"
---
# <a name="validating-with-the-idataerrorinfo-interface-c"></a>Validación de la interfaz IDataErrorInfo (C#)

por [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther muestra cómo mostrar mensajes de error de validación personalizados implementando la interfaz IDataErrorInfo en una clase de modelo.

El objetivo de este tutorial es explicar un enfoque para realizar la validación en una aplicación ASP.NET MVC. Aprenderá a impedir que alguien envíe un formulario HTML sin proporcionar valores para los campos de formulario requeridos. En este tutorial, aprenderá a realizar la validación mediante la interfaz IErrorDataInfo.

## <a name="assumptions"></a>Suposiciones

En este tutorial, usaremos la base de datos MoviesDB y la tabla de base de datos movies. Esta tabla tiene las siguientes columnas:

<a id="0.5_table01"></a>

| **Nombre de la columna** | **Tipo de datos** | **Permitir valores NULL** |
| --- | --- | --- |
| Id. | Valor int. | False |
| Título | Nvarchar(100) | False |
| Director | Nvarchar(100) | False |
| DateReleased | DateTime | False |

En este tutorial, se usa Microsoft Entity Framework para generar las clases del modelo de base de datos. La clase de película generada por la Entity Framework se muestra en la figura 1.

[![la entidad Movie](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)

**Figura 01**: la entidad Movie ([haga clic para ver la imagen de tamaño completo](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))

> [!NOTE] 
> 
> Para obtener más información sobre el uso de la Entity Framework para generar las clases de modelo de base de datos, vea mi tutorial titulado crear clases de modelo con el Entity Framework.

## <a name="the-controller-class"></a>Clase Controller

Usamos el controlador Home para mostrar películas y crear nuevas películas. El código de esta clase se incluye en la lista 1.

**Lista 1-Controllers\HomeController.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample1.cs)]

La clase de controlador de inicio de la lista 1 contiene dos acciones Create (). La primera acción muestra el formulario HTML para crear una nueva película. La segunda acción Create () realiza la inserción real de la nueva película en la base de datos. La segunda acción Create () se invoca cuando el formulario mostrado por la primera acción Create () se envía al servidor.

Observe que la segunda acción Create () contiene las siguientes líneas de código:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample2.cs)]

La propiedad IsValid devuelve false cuando se produce un error de validación. En ese caso, se vuelve a mostrar la vista crear que contiene el formulario HTML para crear una película.

## <a name="creating-a-partial-class"></a>Crear una clase parcial

La clase de película se genera mediante el Entity Framework. Puede ver el código de la clase Movie si expande el archivo MoviesDBModel. edmx en la ventana Explorador de soluciones y abre el archivo MoviesDBModel.Designer.cs en el editor de código (vea la figura 2).

[![el código para la entidad Movie](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)

**Figura 02**: el código de la entidad Movie ([haga clic para ver la imagen de tamaño completo](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))

La clase Movie es una clase parcial. Esto significa que se puede agregar otra clase parcial con el mismo nombre para extender la funcionalidad de la clase Movie. Agregaremos la lógica de validación a la nueva clase parcial.

Agregue la clase en la lista 2 a la carpeta models.

**Lista 2-Models\Movie.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample3.cs)]

Observe que la clase de la lista 2 incluye el modificador *parcial* . Los métodos o propiedades que se agregan a esta clase se convierten en parte de la clase de película generada por el Entity Framework.

## <a name="adding-onchanging-and-onchanged-partial-methods"></a>Agregar métodos parciales Changing y onchange

Cuando el Entity Framework genera una clase de entidad, el Entity Framework agrega automáticamente métodos parciales a la clase. En el Entity Framework se generan métodos parciales Changing y onchange que corresponden a cada propiedad de la clase.

En el caso de la clase Movie, el Entity Framework crea los siguientes métodos:

- OnIdChanging
- OnIdChanged
- OnTitleChanging
- OnTitleChanged
- OnDirectorChanging
- OnDirectorChanged
- OnDateReleasedChanging
- OnDateReleasedChanged

El método Changing se llama justo antes de que se cambie la propiedad correspondiente. El método onchange se llama justo después de cambiar la propiedad.

Puede aprovechar estos métodos parciales para agregar lógica de validación a la clase Movie. La clase Update Movie de la lista 3 comprueba que las propiedades title y director tienen asignados valores no vacíos.

> [!NOTE] 
> 
> Un método parcial es un método definido en una clase que no es necesario implementar. Si no implementa un método parcial, el compilador quita la firma del método y todas las llamadas al método, por lo que no hay costos en tiempo de ejecución asociados con el método parcial. En el editor de Visual Studio Code, puede Agregar un método parcial escribiendo la palabra clave *partial* seguida de un espacio para ver una lista de las particiones que se van a implementar.

**Lista 3: Models\Movie.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample4.cs)]

Por ejemplo, si intenta asignar una cadena vacía a la propiedad title, se asigna un mensaje de error a un diccionario denominado \_errores.

En este momento, no ocurre nada realmente cuando se asigna una cadena vacía a la propiedad title y se agrega un error al campo Private \_Errors. Necesitamos implementar la interfaz IDataErrorInfo para exponer estos errores de validación en el marco de MVC de ASP.NET.

## <a name="implementing-the-idataerrorinfo-interface"></a>Implementar la interfaz IDataErrorInfo

La interfaz IDataErrorInfo forma parte de .NET Framework desde la primera versión. Esta interfaz es una interfaz muy sencilla:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample5.cs)]

Si una clase implementa la interfaz IDataErrorInfo, el marco de MVC de ASP.NET usará esta interfaz al crear una instancia de la clase. Por ejemplo, la acción crear () del controlador Home acepta una instancia de la clase Movie:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample6.cs)]

El marco de MVC de ASP.NET crea la instancia de la película pasada a la acción Create () mediante un enlazador de modelos (DefaultModelBinder). El enlazador de modelos es responsable de crear una instancia del objeto Movie enlazando los campos de formulario HTML a una instancia del objeto Movie.

DefaultModelBinder detecta si una clase implementa la interfaz IDataErrorInfo. Si una clase implementa esta interfaz, el enlazador de modelos invoca a IDataErrorInfo. este indizador para cada propiedad de la clase. Si el indexador devuelve un mensaje de error, el enlazador de modelos agrega este mensaje de error al estado del modelo automáticamente.

DefaultModelBinder también comprueba la propiedad IDataErrorInfo. error. Esta propiedad está pensada para representar errores de validación específicos de la propiedad que están asociados a la clase. Por ejemplo, es posible que desee aplicar una regla de validación que dependa de los valores de varias propiedades de la clase Movie. En ese caso, se devolvería un error de validación de la propiedad error.

La clase de película actualizada en la lista 4 implementa la interfaz IDataErrorInfo.

**Lista 4-Models\Movie.cs (implementa IDataErrorInfo)**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample7.cs)]

En la lista 4, la propiedad del indexador comprueba la colección de errores \_para ver si contiene una clave que corresponde al nombre de la propiedad que se ha pasado al indizador. Si no hay ningún error de validación asociado a la propiedad, se devuelve una cadena vacía.

No es necesario modificar el controlador Home de ninguna manera para usar la clase de película modificada. En la página que se muestra en la figura 3 se muestra lo que ocurre cuando no se especifica ningún valor para los campos de formulario de título o de director.

[![crear métodos de acción automáticamente](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)

**Figura 03**: formulario con valores que faltan ([haga clic para ver la imagen de tamaño completo](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))

Observe que el valor DateReleased se valida automáticamente. Dado que la propiedad DateReleased no acepta valores NULL, DefaultModelBinder genera un error de validación para esta propiedad automáticamente cuando no tiene un valor. Si desea modificar el mensaje de error para la propiedad DateReleased, debe crear un enlazador de modelos personalizado.

## <a name="summary"></a>Resumen

En este tutorial, aprendió a usar la interfaz IDataErrorInfo para generar mensajes de error de validación. En primer lugar, creamos una clase de película parcial que extiende la funcionalidad de la clase de película parcial generada por el Entity Framework. A continuación, agregamos la lógica de validación a los métodos parciales de la clase Movie OnTitleChanging () y OnDirectorChanging (). Por último, hemos implementado la interfaz IDataErrorInfo para exponer estos mensajes de validación en el marco de MVC de ASP.NET.

> [!div class="step-by-step"]
> [Anterior](performing-simple-validation-cs.md)
> [Siguiente](validating-with-a-service-layer-cs.md)
