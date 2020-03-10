---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
title: Validación con los validadores de anotación de datosC#() | Microsoft Docs
author: microsoft
description: Aproveche el enlazador de modelos de anotación de datos para realizar la validación en una aplicación ASP.NET MVC. Obtenga información sobre cómo usar los distintos tipos de validador...
ms.author: riande
ms.date: 05/29/2009
ms.assetid: 7ca8013e-9dfc-4e33-8336-cdccfd5f9414
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
msc.type: authoredcontent
ms.openlocfilehash: e154384c08adf0c14920afff85e983a67b41707c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78435985"
---
# <a name="validation-with-the-data-annotation-validators-c"></a>Validación con los validadores de anotación de datos (C#)

por [Microsoft](https://github.com/microsoft)

> Aproveche el enlazador de modelos de anotación de datos para realizar la validación en una aplicación ASP.NET MVC. Aprenda a usar los distintos tipos de atributos de validador y a trabajar con ellos en Microsoft Entity Framework.

En este tutorial, aprenderá a usar los validadores de anotación de datos para realizar la validación en una aplicación ASP.NET MVC. La ventaja de usar los validadores de anotación de datos es que permiten realizar la validación simplemente agregando uno o más atributos, como el atributo required o StringLength, a una propiedad de clase.

Antes de poder usar los validadores de anotación de datos, debe descargar el enlazador de modelos de anotaciones de datos. Puede descargar el ejemplo de enlazador de modelos de anotaciones de datos del sitio web de CodePlex; para ello, haga clic [aquí](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).

Es importante comprender que el enlazador de modelos de anotaciones de datos no es una parte oficial del marco Microsoft ASP.NET MVC. Aunque el equipo de Microsoft ASP.NET MVC creó el enlazador de modelos de anotaciones de datos, Microsoft no ofrece soporte técnico oficial para el enlazador de modelos de anotaciones de datos que se describe y se usa en este tutorial.

## <a name="using-the-data-annotation-model-binder"></a>Usar el enlazador de modelos de anotación de datos

Para poder usar el enlazador de modelos de anotaciones de datos en una aplicación ASP.NET MVC, primero debe agregar una referencia al ensamblado Microsoft. Web. Mvc. DataAnnotations. dll y al ensamblado System. ComponentModel. DataAnnotations. dll. Seleccione la opción de menú **proyecto, agregar referencia**. A continuación, haga clic en la pestaña **examinar** y vaya a la ubicación donde descargó (y descomprime) el ejemplo de enlazador de modelos de anotaciones de datos (vea la **Ilustración 1**).

[![](validation-with-the-data-annotation-validators-cs/_static/image2.png)](validation-with-the-data-annotation-validators-cs/_static/image1.png)

**Figura 1**: agregar una referencia al enlazador de modelos de anotaciones de datos ([haga clic para ver la imagen de tamaño completo](validation-with-the-data-annotation-validators-cs/_static/image3.png))

Seleccione el ensamblado Microsoft. Web. Mvc. DataAnnotations. dll y el ensamblado System. ComponentModel. DataAnnotations. dll y haga clic en el botón **Aceptar** .

No se puede usar el ensamblado System. ComponentModel. DataAnnotations. dll incluido con .NET Framework Service Pack 1 con el enlazador de modelos de anotaciones de datos. Debe usar la versión del ensamblado System. ComponentModel. DataAnnotations. dll que se incluye con la descarga de ejemplo del enlazador de modelos de anotaciones de datos.

Por último, debe registrar el enlazador de modelos DataAnnotations en el archivo global. asax. Agregue la siguiente línea de código al controlador de eventos de inicio () de la aplicación\_para que el método Start () de la aplicación\_tenga el siguiente aspecto:

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample1.cs)]

Esta línea de código registra ataAnnotationsModelBinder como el enlazador de modelos predeterminado para toda la aplicación ASP.NET MVC.

## <a name="using-the-data-annotation-validator-attributes"></a>Usar los atributos de validador de anotación de datos

Cuando se usa el enlazador de modelos de anotaciones de datos, se usan atributos de validador para realizar la validación. El espacio de nombres System. ComponentModel. DataAnnotations incluye los siguientes atributos de validador:

- Range: permite validar si el valor de una propiedad se encuentra entre un intervalo de valores especificado.
- RegularExpression: permite validar si el valor de una propiedad coincide con un patrón de expresión regular especificado.
- Requerido: permite marcar una propiedad según sea necesario.
- StringLength: permite especificar una longitud máxima para una propiedad de cadena.
- Validación: la clase base para todos los atributos de validador.

> [!NOTE] 
> 
> Si los validadores estándar no satisfacen sus necesidades de validación, siempre tiene la opción de crear un atributo de validador personalizado heredando un nuevo atributo de validador del atributo de validación base.

La clase Product de la **lista 1** muestra cómo utilizar estos atributos de validador. Las propiedades name, Description y UnitPrice se marcan como required. La propiedad Name debe tener una longitud de cadena inferior a 10 caracteres. Por último, la propiedad UnitPrice debe coincidir con un patrón de expresión regular que representa una cantidad de moneda.

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample2.cs)]

**Lista 1**: Models\Product.CS

La clase Product muestra cómo usar un atributo adicional: el atributo DisplayName. El atributo DisplayName le permite modificar el nombre de la propiedad cuando la propiedad se muestra en un mensaje de error. En lugar de mostrar el mensaje de error "el campo UnitPrice es obligatorio", puede mostrar el mensaje de error "el campo Price Is Required".

> [!NOTE] 
> 
> Si desea personalizar completamente el mensaje de error que muestra un validador, puede asignar un mensaje de error personalizado a la propiedad ErrorMessage del validador de la siguiente manera: `<Required(ErrorMessage:="This field needs a value!")>`

Puede usar la clase Product de la **lista 1** con la acción de controlador Create () en la **lista 2**. Esta acción del controlador vuelve a mostrar la vista de creación cuando el estado del modelo contiene errores.

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample3.cs)]

**Lista 2**: Controllers\ProductController.VB

Por último, puede crear la vista en la **lista 3** haciendo clic con el botón secundario en la acción crear () y seleccionando la opción de menú **Agregar vista**. Cree una vista fuertemente tipada con la clase Product como clase de modelo. Seleccione **crear** en la lista desplegable ver contenido (vea la **figura 2**).

[![](validation-with-the-data-annotation-validators-cs/_static/image5.png)](validation-with-the-data-annotation-validators-cs/_static/image4.png)

**Figura 2**: adición de la vista de creación

[!code-aspx[Main](validation-with-the-data-annotation-validators-cs/samples/sample4.aspx)]

**Lista 3**: Views\Product\Create.aspx

> [!NOTE] 
> 
> Quite el campo ID. del formulario de creación generado por la opción de menú **Agregar vista** . Dado que el campo ID. corresponde a una columna de identidad, no desea permitir que los usuarios escriban un valor para este campo.

Si envía el formulario para crear un producto y no especifica valores para los campos obligatorios, se mostrarán los mensajes de error de validación de la **figura 3** .

[![](validation-with-the-data-annotation-validators-cs/_static/image7.png)](validation-with-the-data-annotation-validators-cs/_static/image6.png)

**Figura 3**: faltan campos obligatorios

Si escribe una cantidad de moneda no válida, se muestra el mensaje de error de la **figura 4** .

[![](validation-with-the-data-annotation-validators-cs/_static/image9.png)](validation-with-the-data-annotation-validators-cs/_static/image8.png)

**Figura 4**: importe de moneda no válido

## <a name="using-data-annotation-validators-with-the-entity-framework"></a>Usar validadores de anotación de datos con el Entity Framework

Si usa Microsoft Entity Framework para generar las clases del modelo de datos, no podrá aplicar los atributos del validador directamente a las clases. Dado que el Entity Framework Designer genera las clases del modelo, los cambios que realice en las clases del modelo se sobrescribirán la próxima vez que realice cambios en el diseñador.

Si desea utilizar los validadores con las clases generadas por el Entity Framework, debe crear clases de metadatos. Los validadores se aplican a la clase meta data en lugar de aplicar los validadores a la clase real.

Por ejemplo, Imagine que ha creado una clase de película mediante el Entity Framework (consulte la **figura 5**). Imagínese, además, que desea que las propiedades requeridas de las propiedades del título y el director de la película. En ese caso, puede crear la clase parcial y la clase de metadatos en la **lista 4**.

[![](validation-with-the-data-annotation-validators-cs/_static/image11.png)](validation-with-the-data-annotation-validators-cs/_static/image10.png)

**Figura 5**: clase de película generada por Entity Framework

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample5.cs)]

**Lista 4**: Models\Movie.CS

El archivo de la **lista 4** contiene dos clases denominadas Movie y MovieMetaData. La clase Movie es una clase parcial. Corresponde a la clase parcial generada por el Entity Framework que se encuentra en el archivo Model. Designer. VB.

Actualmente, .NET Framework no admite propiedades parciales. Por lo tanto, no hay ninguna manera de aplicar los atributos del validador a las propiedades de la clase Movie definida en el archivo Model. Designer. VB aplicando los atributos de validador a las propiedades de la clase Movie definida en el archivo en la **lista 4**.

Observe que la clase parcial de película está decorada con un atributo MetadataType que apunta a la clase MovieMetaData. La clase MovieMetaData contiene propiedades de proxy para las propiedades de la clase Movie.

Los atributos del validador se aplican a las propiedades de la clase MovieMetaData. Las propiedades title, director y DateReleased se marcan como propiedades requeridas. Se debe asignar a la propiedad Director una cadena que contenga menos de 5 caracteres. Por último, el atributo DisplayName se aplica a la propiedad DateReleased para mostrar un mensaje de error como "el campo Date released es obligatorio". en lugar del error "el campo DateReleased es obligatorio".

> [!NOTE] 
> 
> Tenga en cuenta que las propiedades de proxy de la clase MovieMetaData no necesitan representar los mismos tipos que las propiedades correspondientes de la clase Movie. Por ejemplo, la propiedad director es una propiedad de cadena de la clase Movie y una propiedad de objeto de la clase MovieMetaData.

En la página de la **figura 6** se muestran los mensajes de error devueltos al escribir valores no válidos para las propiedades de la película.

[![](validation-with-the-data-annotation-validators-cs/_static/image13.png)](validation-with-the-data-annotation-validators-cs/_static/image12.png)

**Figura 6**: uso de validadores con la Entity Framework ([haga clic para ver la imagen de tamaño completo](validation-with-the-data-annotation-validators-cs/_static/image14.png))

## <a name="summary"></a>Resumen

En este tutorial, aprendió a aprovechar el enlazador de modelos de anotación de datos para realizar la validación en una aplicación ASP.NET MVC. Aprendió a usar los distintos tipos de atributos de validador, como los atributos requeridos y StringLength. También aprendió a utilizar estos atributos al trabajar con Microsoft Entity Framework.

> [!div class="step-by-step"]
> [Anterior](validating-with-a-service-layer-cs.md)
> [Siguiente](creating-model-classes-with-the-entity-framework-vb.md)
