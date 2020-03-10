---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
title: Realizar una validación simpleC#() | Microsoft Docs
author: StephenWalther
description: Obtenga información sobre cómo realizar la validación en una aplicación ASP.NET MVC. En este tutorial, Stephen Walther le presenta el estado del modelo y la aplicación auxiliar HTML de validación...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 21383c9d-6aea-4bad-a99b-b5f2c9d6503f
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: e33f522af74efe97b5a245e956bc0b918ea769af
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78436723"
---
# <a name="performing-simple-validation-c"></a>Realizar una validación simple (C#)

por [Stephen Walther](https://github.com/StephenWalther)

> Obtenga información sobre cómo realizar la validación en una aplicación ASP.NET MVC. En este tutorial, Stephen Walther le presenta el modelo de estado y las aplicaciones auxiliares HTML de validación.

El objetivo de este tutorial es explicar cómo puede realizar la validación en una aplicación ASP.NET MVC. Por ejemplo, aprenderá a impedir que alguien envíe un formulario que no contenga un valor para un campo obligatorio. Aprenderá a usar la aplicación auxiliar HTML de validación y el estado del modelo.

## <a name="understanding-model-state"></a>Descripción del estado del modelo

Puede usar el estado del modelo, o con más precisión, el Diccionario de estado del modelo para representar los errores de validación. Por ejemplo, la acción Create () de la lista 1 valida las propiedades de una clase Product antes de agregar la clase Product a una base de datos.

No recomiendo que agregue la lógica de validación o de base de datos a un controlador. Un controlador solo debe contener lógica relacionada con el control de flujo de la aplicación. Estamos tomando un método abreviado para simplificar las cosas.

**Lista 1-Controllers\ProductController.cs**

[!code-csharp[Main](performing-simple-validation-cs/samples/sample1.cs)]

En la lista 1, se validan las propiedades name, Description y UnidadesEnExistencias de la clase product. Si alguna de estas propiedades no supera una prueba de validación, se agrega un error al Diccionario de estado del modelo (representado por la propiedad ModelState de la clase de controlador).

Si hay algún error en el estado del modelo, la propiedad ModelState. IsValid devuelve false. En ese caso, se vuelve a mostrar el formulario HTML para crear un nuevo producto. De lo contrario, si no hay ningún error de validación, el nuevo producto se agrega a la base de datos.

## <a name="using-the-validation-helpers"></a>Uso de las aplicaciones auxiliares de validación

El marco de MVC de ASP.NET incluye dos aplicaciones auxiliares de validación: la aplicación auxiliar HTML. ValidationMessage () y la aplicación auxiliar HTML. ValidationSummary (). Use estas dos aplicaciones auxiliares en una vista para mostrar mensajes de error de validación.

Las aplicaciones auxiliares de HTML. ValidationMessage () y HTML. ValidationSummary () se usan en las vistas de creación y edición que genera automáticamente el scaffolding de ASP.NET MVC. Siga estos pasos para generar la vista de creación:

1. Haga clic con el botón secundario en la acción crear () en el controlador del producto y seleccione la opción de menú **Agregar vista** (vea la figura 1).
2. En el cuadro de diálogo **Agregar vista** , active la casilla **crear una vista fuertemente tipada** (consulte la figura 2).
3. En la lista desplegable **Ver clase de datos** , seleccione la clase product.
4. En la lista desplegable **ver contenido** , seleccione crear.
5. Haga clic en el botón **Agregar**.

Asegúrese de compilar la aplicación antes de agregar una vista. De lo contrario, la lista de clases no aparecerá en la lista desplegable **Ver clase de datos** .

[![el cuadro de diálogo nuevo proyecto](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)

**Figura 01**: agregar una vista ([haga clic para ver la imagen de tamaño completo](performing-simple-validation-cs/_static/image2.png))

[![el cuadro de diálogo nuevo proyecto](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)

**Figura 02**: creación de una vista fuertemente tipada ([haga clic para ver la imagen de tamaño completo](performing-simple-validation-cs/_static/image4.png))

Después de completar estos pasos, obtendrá la vista de creación en la lista 2.

**Lista 2-Views\Product\Create.aspx**

[!code-aspx[Main](performing-simple-validation-cs/samples/sample2.aspx)]

En la lista 2, se llama a la aplicación auxiliar HTML. ValidationSummary () inmediatamente encima del formulario HTML. Esta aplicación auxiliar se usa para mostrar una lista de mensajes de errores de validación. La aplicación auxiliar HTML. ValidationSummary () representa los errores en una lista con viñetas.

Se llama a la aplicación auxiliar HTML. ValidationMessage () junto a cada uno de los campos del formulario HTML. Esta aplicación auxiliar se usa para mostrar un mensaje de error justo junto a un campo de formulario. En el caso de la lista 2, la aplicación auxiliar HTML. ValidationMessage () muestra un asterisco cuando se produce un error.

En la página de la figura 3 se muestran los mensajes de error representados por las aplicaciones auxiliares de validación cuando se envía el formulario con campos que faltan y valores no válidos.

[![el cuadro de diálogo nuevo proyecto](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)

**Figura 03**: la vista de creación enviada con problemas ([haga clic para ver la imagen de tamaño completo](performing-simple-validation-cs/_static/image6.png))

Observe que la apariencia de los campos de entrada HTML también se modifica cuando se produce un error de validación. La aplicación auxiliar HTML. TextBox () representa un atributo *Class = "Input-Validation-error"* cuando hay un error de validación asociado a la propiedad representada por la aplicación auxiliar HTML. TextBox ().

Hay tres clases de hoja de estilos en cascada que se usan para controlar la apariencia de los errores de validación:

- Input-Validation-error: se aplica a la &lt;entrada&gt; la etiqueta presentada por la aplicación auxiliar HTML. TextBox ().
- error de validación de campo: se aplica al &lt;intervalo&gt; la etiqueta presentada por la aplicación auxiliar HTML. ValidationMessage ().
- Validation-Summary-Errors: se aplica a la etiqueta &lt;UL&gt; representada por la aplicación auxiliar HTML. ValidationSummary ().

Puede modificar estas clases de hojas de estilos en cascada y, por tanto, modificar la apariencia de los errores de validación mediante la modificación del archivo site. CSS que se encuentra en la carpeta de contenido.

> [!NOTE] 
> 
> La clase HtmlHelper incluye propiedades estáticas de solo lectura para recuperar los nombres de las clases CSS relacionadas con la validación. Estas propiedades estáticas se denominan ValidationInputCssClassName, ValidationFieldCssClassName y ValidationSummaryCssClassName.

## <a name="prebinding-validation-and-postbinding-validation"></a>Validación de enlace y validación de postenlace

Si envía el formulario HTML para crear un producto y especifica un valor no válido para el campo precio y ningún valor para el campo UnidadesEnExistencias, recibirá los mensajes de validación que se muestran en la figura 4. ¿De dónde proceden estos mensajes de error de validación?

[![el cuadro de diálogo nuevo proyecto](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)

**Figura 04**: enlace de errores de validación ([haga clic para ver la imagen de tamaño completo](performing-simple-validation-cs/_static/image8.png))

En realidad, hay dos tipos de mensajes de error de validación: los generados antes de que los campos de formulario HTML se enlacen a una clase y los generados después de que los campos de formulario se enlacen a la clase. En otras palabras, hay errores de validación de enlace y errores de validación de enlace.

La acción Create () expuesta por el controlador del producto en la enumeración 1 acepta una instancia de la clase product. La firma del método Create tiene el siguiente aspecto:

[!code-csharp[Main](performing-simple-validation-cs/samples/sample3.cs)]

Los valores de los campos de formulario HTML del formulario de creación se enlazan a la clase productToCreate por algo denominado enlazador de modelos. El enlazador de modelos predeterminado agrega automáticamente un mensaje de error al estado del modelo cuando no puede enlazar un campo de formulario a una propiedad de formulario.

El enlazador de modelos predeterminado no puede enlazar la cadena "Apple" a la propiedad Price de la clase product. No se puede asignar una cadena a una propiedad decimal. Por lo tanto, el enlazador de modelos agrega un error al estado del modelo.

El enlazador de modelos predeterminado tampoco puede asignar un valor null a una propiedad que no acepta valores NULL. En concreto, el enlazador de modelos no puede asignar un valor null a la propiedad UnidadesEnExistencias. Una vez más, el enlazador de modelos se pone al día y agrega un mensaje de error al estado del modelo.

Si desea personalizar la apariencia de estos mensajes de error de enlace, debe crear cadenas de recursos para estos mensajes.

## <a name="summary"></a>Resumen

El objetivo de este tutorial era describir la mecánica básica de la validación en el marco de MVC de ASP.NET. Ha aprendido a usar el estado del modelo y las aplicaciones auxiliares HTML de validación. También se ha analizado la distinción entre la validación del enlace y el enlace. En otros tutoriales, trataremos varias estrategias para mover el código de validación de los controladores y a las clases de modelo.

> [!div class="step-by-step"]
> [Anterior](displaying-a-table-of-database-data-cs.md)
> [Siguiente](validating-with-the-idataerrorinfo-interface-cs.md)
