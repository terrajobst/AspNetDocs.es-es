---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
title: '#3 de iteración: agregar validaciónC#de formulario () | Microsoft Docs'
author: microsoft
description: En la tercera iteración, agregamos la validación de formulario básica. Impedimos que los usuarios envíen un formulario sin completar los campos de formulario necesarios. También validamos emai...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 51a0d175-913b-43d8-95e3-840fb96ad1a9
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: af2e86e820f60f0a3d8e3db8f78eba67ef63579a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437989"
---
# <a name="iteration-3--add-form-validation-c"></a>#3 de iteración: agregar validaciónC#de formulario ()

por [Microsoft](https://github.com/microsoft)

[Descargar código](iteration-3-add-form-validation-cs/_static/contactmanager_3_cs1.zip)

> En la tercera iteración, agregamos la validación de formulario básica. Impedimos que los usuarios envíen un formulario sin completar los campos de formulario necesarios. También validamos las direcciones de correo electrónico y los números de teléfono.

## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Creación de una aplicación ASP.NET MVC de administraciónC#de contactos ()

En esta serie de tutoriales, creamos una aplicación de administración de contactos completa de principio a fin. La aplicación Contact Manager le permite almacenar información de contacto: nombres, números de teléfono y direcciones de correo electrónico, para obtener una lista de personas.

Compilamos la aplicación en varias iteraciones. Con cada iteración, mejoramos gradualmente la aplicación. El objetivo de este enfoque de varias iteraciones es permitirle comprender la razón de cada cambio.

- #1 de iteración: cree la aplicación. En la primera iteración, se crea el administrador de contactos de la manera más sencilla posible. Agregamos compatibilidad con las operaciones básicas de base de datos: crear, leer, actualizar y eliminar (CRUD).

- #2 de iteración: haga que la aplicación tenga un buen aspecto. En esta iteración, mejoramos la apariencia de la aplicación modificando la página maestra de vista de MVC predeterminada de ASP.NET y la hoja de estilos en cascada.

- #3 de iteración: agregar validación de formulario. En la tercera iteración, agregamos la validación de formulario básica. Impedimos que los usuarios envíen un formulario sin completar los campos de formulario necesarios. También validamos las direcciones de correo electrónico y los números de teléfono.

- #4 de iteración: hacer que la aplicación esté acoplada de forma flexible. En esta cuarta iteración, se aprovechan varios patrones de diseño de software para facilitar el mantenimiento y la modificación de la aplicación Contact Manager. Por ejemplo, se refactoriza nuestra aplicación para usar el patrón de repositorio y el patrón de inserción de dependencias.

- #5 de iteración: crear pruebas unitarias. En la quinta iteración, se facilita el mantenimiento y la modificación de la aplicación mediante la adición de pruebas unitarias. Hemos simulado nuestras clases de modelo de datos y compilamos pruebas unitarias para nuestros controladores y la lógica de validación.

- #6 de iteración: Use el desarrollo controlado por pruebas. En esta sexta iteración, agregamos una nueva funcionalidad a nuestra aplicación escribiendo primero las pruebas unitarias y escribiendo código en las pruebas unitarias. En esta iteración, se agregan grupos de contactos.

- #7 de iteración: agregar funcionalidad de Ajax. En la séptima iteración, se mejora la capacidad de respuesta y el rendimiento de la aplicación mediante la adición de compatibilidad con Ajax.

## <a name="this-iteration"></a>Esta iteración

En esta segunda iteración de la aplicación Contact Manager, agregamos la validación de formularios básicos. Impedimos que los usuarios envíen un contacto sin escribir valores para los campos de formulario obligatorios. También Validamos los números de teléfono y las direcciones de correo electrónico (consulte la figura 1).

[![el cuadro de diálogo nuevo proyecto](iteration-3-add-form-validation-cs/_static/image1.jpg)](iteration-3-add-form-validation-cs/_static/image1.png)

**Figura 01**: un formulario con validación ([haga clic para ver la imagen de tamaño completo](iteration-3-add-form-validation-cs/_static/image2.png))

En esta iteración, agregamos la lógica de validación directamente a las acciones del controlador. En general, esta no es la manera recomendada de agregar validación a una aplicación de ASP.NET MVC. Un mejor enfoque consiste en colocar una lógica de validación de la aplicación en un [nivel de servicio](http://martinfowler.com/eaaCatalog/serviceLayer.html)independiente. En la iteración siguiente, se refactoriza la aplicación Contact Manager para que la aplicación sea más fácil de mantener.

En esta iteración, para simplificar las cosas, escribimos todo el código de validación a mano. En lugar de escribir el código de validación, podríamos aprovechar el marco de validación. Por ejemplo, puede usar el bloque de aplicaciones de validación de Microsoft Enterprise Library (VAB) para implementar la lógica de validación de la aplicación ASP.NET MVC. Para obtener más información sobre el bloque de aplicación de validación, consulte:

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Agregar validación a la vista de creación

Permita que s Comience agregando la lógica de validación a la vista de creación. Afortunadamente, dado que generamos la vista de creación con Visual Studio, la vista de creación ya contiene toda la lógica de la interfaz de usuario necesaria para mostrar los mensajes de validación. La vista crear se incluye en la lista 1.

**Lista 1-\Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-cs/samples/sample1.aspx)]

Observe la llamada al método auxiliar HTML. ValidationSummary () que aparece justo encima del formulario HTML. Si hay mensajes de error de validación, este método muestra los mensajes de validación en una lista con viñetas.

Tenga en cuenta, además, las llamadas a HTML. ValidationMessage () que aparecen junto a cada campo de formulario. La aplicación auxiliar ValidationMessage () muestra un mensaje de error de validación individual. En el caso de la lista 1, se muestra un asterisco cuando se produce un error de validación.

Por último, la aplicación auxiliar HTML. TextBox () representa automáticamente una clase de hoja de estilos en cascada cuando hay un error de validación asociado a la propiedad que muestra la aplicación auxiliar. La aplicación auxiliar HTML. TextBox () representa una clase denominada **Input-Validation-error**.

Al crear una nueva aplicación ASP.NET MVC, se crea automáticamente una hoja de estilos denominada site. CSS en la carpeta de contenido. Esta hoja de estilos contiene las siguientes definiciones de clases CSS relacionadas con la apariencia de los mensajes de error de validación:

[!code-css[Main](iteration-3-add-form-validation-cs/samples/sample2.css)]

La clase Field-Validation-error se usa para aplicar estilo a la salida representada por la aplicación auxiliar HTML. ValidationMessage (). La clase de error de validación de entrada se usa para aplicar estilo al cuadro de texto (entrada) representado por la aplicación auxiliar HTML. TextBox (). La clase Validation-Summary-Errors se usa para aplicar estilo a la lista sin ordenar que representa la aplicación auxiliar HTML. ValidationSummary ().

> [!NOTE] 
> 
> Puede modificar las clases de hoja de estilos descritas en esta sección para personalizar la apariencia de los mensajes de error de validación.

## <a name="adding-validation-logic-to-the-create-action"></a>Agregar lógica de validación a la acción Create

En este momento, la creación de vista nunca muestra mensajes de error de validación porque no hemos escrito la lógica para generar ningún mensaje. Para mostrar los mensajes de error de validación, debe agregar los mensajes de error a ModelState.

> [!NOTE] 
> 
> El método UpdateModel () agrega mensajes de error a ModelState automáticamente cuando se produce un error al asignar el valor de un campo de formulario a una propiedad. Por ejemplo, si intenta asignar la cadena "Apple" a una propiedad BirthDate que acepta valores DateTime, el método UpdateModel () agrega un error a ModelState.

El método Create () modificado de la lista 2 contiene una nueva sección que valida las propiedades de la clase Contact antes de que se inserte el nuevo contacto en la base de datos.

**Lista 2-Controllers\ContactController.cs (crear con validación)**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample3.cs)]

La sección Validate exige cuatro reglas de validación distintas:

- La propiedad FirstName debe tener una longitud mayor que cero (y no puede constar solo de espacios)
- La propiedad LastName debe tener una longitud mayor que cero (y no puede constar solo de espacios)
- Si la propiedad Phone tiene un valor (tiene una longitud mayor que 0), la propiedad Phone debe coincidir con una expresión regular.
- Si la propiedad email tiene un valor (tiene una longitud mayor que 0), la propiedad email debe coincidir con una expresión regular.

Cuando hay una infracción de la regla de validación, se agrega un mensaje de error a ModelState con la ayuda del método AddModelError (). Cuando se agrega un mensaje a ModelState, se proporciona el nombre de una propiedad y el texto de un mensaje de error de validación. Este mensaje de error se muestra en la vista mediante los métodos auxiliares HTML. ValidationSummary () y HTML. ValidationMessage ().

Una vez ejecutadas las reglas de validación, se comprueba la propiedad IsValid de ModelState. La propiedad IsValid devuelve false cuando se han agregado mensajes de error de validación a ModelState. Si se produce un error en la validación, se vuelve a mostrar el formulario de creación con los mensajes de error.

> [!NOTE] 
> 
> Obtuve las expresiones regulares para validar el número de teléfono y la dirección de correo electrónico del repositorio de expresiones regulares en [ *http://regexlib.com* ](http://regexlib.com)

## <a name="adding-validation-logic-to-the-edit-action"></a>Agregar lógica de validación a la acción de edición

La acción Editar () actualiza un contacto. La acción Edit () debe realizar exactamente la misma validación que la acción Create (). En lugar de duplicar el mismo código de validación, debemos refactorizar el controlador de contactos para que las acciones Create () y Edit () llamen al mismo método de validación.

La clase de controlador de contacto modificada se incluye en la lista 3. Esta clase tiene un nuevo método ValidateContact () al que se llama en las acciones Create () y Edit ().

**Lista 3: Controllers\ContactController.cs**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample4.cs)]

## <a name="summary"></a>Resumen

En esta iteración, hemos agregado la validación de formularios básicos a nuestra aplicación de Contact Manager. Nuestra lógica de validación impide que los usuarios envíen un nuevo contacto o editen un contacto existente sin proporcionar valores para las propiedades FirstName y LastName. Además, los usuarios deben proporcionar números de teléfono y direcciones de correo electrónico válidos.

En esta iteración, se ha agregado la lógica de validación a nuestra aplicación de Contact Manager de la manera más sencilla posible. Sin embargo, la combinación de la lógica de validación en nuestra lógica del controlador creará problemas a largo plazo. Nuestra aplicación será más difícil de mantener y modificar con el tiempo.

En la siguiente iteración, refactorizaremos la lógica de validación y la lógica de acceso a la base de datos fuera de nuestros controladores. Nos beneficiaremos de varios principios de diseño de software que nos permitan crear una aplicación más acoplada y más flexible.

> [!div class="step-by-step"]
> [Anterior](iteration-2-make-the-application-look-nice-cs.md)
> [Siguiente](iteration-4-make-the-application-loosely-coupled-cs.md)
