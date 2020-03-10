---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-vb
title: '#2 de iteración: hacer que la aplicación tenga un buen aspecto (VB) | Microsoft Docs'
author: microsoft
description: En esta iteración, mejoramos la apariencia de la aplicación modificando la página maestra de vista de MVC predeterminada de ASP.NET y la hoja de estilos en cascada.
ms.author: riande
ms.date: 02/20/2009
ms.assetid: f65cb436-e493-46fd-9608-384b27385aa1
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-vb
msc.type: authoredcontent
ms.openlocfilehash: cd392baaefcfc9eef3551bc534e0b912ccd349cc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78487285"
---
# <a name="iteration-2--make-the-application-look-nice-vb"></a>#2 de iteración: hacer que la aplicación tenga un buen aspecto (VB)

por [Microsoft](https://github.com/microsoft)

[Descargar código](iteration-2-make-the-application-look-nice-vb/_static/contactmanager_2_vb1.zip)

> En esta iteración, mejoramos la apariencia de la aplicación modificando la página maestra de vista de MVC predeterminada de ASP.NET y la hoja de estilos en cascada.

## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Creación de una aplicación ASP.NET MVC de administración de contactos (VB)

En esta serie de tutoriales, creamos una aplicación de administración de contactos completa de principio a fin. La aplicación Contact Manager le permite almacenar información de contacto (nombres, números de teléfono y direcciones de correo electrónico) para obtener una lista de personas.

Compilamos la aplicación en varias iteraciones. Con cada iteración, mejoramos gradualmente la aplicación. El objetivo de este enfoque de varias iteraciones es permitirle comprender la razón de cada cambio.

- #1 de iteración: cree la aplicación. En la primera iteración, se crea el administrador de contactos de la manera más sencilla posible. Agregamos compatibilidad con las operaciones básicas de base de datos: crear, leer, actualizar y eliminar (CRUD).

- #2 de iteración: haga que la aplicación tenga un buen aspecto. En esta iteración, mejoramos la apariencia de la aplicación modificando la página maestra de vista de MVC predeterminada de ASP.NET y la hoja de estilos en cascada.

- #3 de iteración: agregar validación de formulario. En la tercera iteración, agregamos la validación de formulario básica. Impedimos que los usuarios envíen un formulario sin completar los campos de formulario necesarios. También validamos las direcciones de correo electrónico y los números de teléfono.

- #4 de iteración: hacer que la aplicación esté acoplada de forma flexible. En esta cuarta iteración, se aprovechan varios patrones de diseño de software para facilitar el mantenimiento y la modificación de la aplicación Contact Manager. Por ejemplo, se refactoriza nuestra aplicación para usar el patrón de repositorio y el patrón de inserción de dependencias.

- #5 de iteración: crear pruebas unitarias. En la quinta iteración, se facilita el mantenimiento y la modificación de la aplicación mediante la adición de pruebas unitarias. Hemos simulado nuestras clases de modelo de datos y compilamos pruebas unitarias para nuestros controladores y la lógica de validación.

- #6 de iteración: Use el desarrollo controlado por pruebas. En esta sexta iteración, agregamos una nueva funcionalidad a nuestra aplicación escribiendo primero las pruebas unitarias y escribiendo código en las pruebas unitarias. En esta iteración, se agregan grupos de contactos.

- #7 de iteración: agregar funcionalidad de Ajax. En la séptima iteración, se mejora la capacidad de respuesta y el rendimiento de la aplicación mediante la adición de compatibilidad con Ajax.

## <a name="this-iteration"></a>Esta iteración

El objetivo de esta iteración es mejorar la apariencia de la aplicación Contact Manager. Actualmente, el administrador de contactos usa la página maestra de vista de ASP.NET MVC predeterminada y la hoja de estilos en cascada (vea la ilustración 1). Estos no parecen correctos, pero no quiero que el administrador de contactos tenga el aspecto de todos los demás sitios Web ASP.NET MVC. Deseo reemplazar estos archivos por archivos personalizados.

[![el cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image1.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image1.png)

**Figura 01**: la apariencia predeterminada de una aplicación ASP.NET MVC ([haga clic para ver la imagen de tamaño completo](iteration-2-make-the-application-look-nice-vb/_static/image2.png))

En esta iteración, se describen dos métodos para mejorar el diseño visual de la aplicación. En primer lugar, le mostramos cómo sacar partido de la galería de diseño de MVC de ASP.NET para descargar una plantilla de diseño ASP.NET MVC gratuita. La galería de diseño de ASP.NET MVC le permite crear una aplicación web profesional sin realizar ningún trabajo.

Decidí no usar una plantilla de la galería de diseño de MVC de ASP.NET para la aplicación Contact Manager. En su lugar, tenía un diseño personalizado creado por una empresa de diseño profesional. En la segunda parte de este tutorial, se explica cómo trabajo con una empresa de diseño profesional para crear el diseño de MVC de ASP.NET final.

## <a name="the-aspnet-mvc-design-gallery"></a>La galería de diseño de ASP.NET MVC

La galería de diseño de MVC de ASP.NET es un recurso gratuito proporcionado por Microsoft. La galería de MVC de ASP.NET se encuentra en la siguiente dirección:

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

La galería de diseño de MVC de ASP.NET hospeda una colección de diseños de sitios web gratuitos que se crearon específicamente para usar en un proyecto de ASP.NET MVC. Los miembros de la comunidad cargan los diseños. Los visitantes de la Galería pueden votar por sus diseños favoritos (vea la figura 2).

[![el cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image2.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image3.png)

**Figura 02**: la galería de diseño de MVC de ASP.net ([haga clic para ver la imagen de tamaño completo](iteration-2-make-the-application-look-nice-vb/_static/image4.png))

A medida que escribo este tutorial, el diseño más popular en la Galería es un diseño denominado octubre de David Hauser. Puede usar este diseño para un proyecto de ASP.NET MVC completando los pasos siguientes:

1. Haga clic en el botón **Descargar** para descargar el archivo. zip de octubre en el equipo.
2. Haga clic con el botón derecho en el archivo. zip de octubre descargado y haga clic en el botón **desbloquear** (vea la figura 3).
3. Descomprima el archivo en una carpeta denominada octubre.
4. Seleccione todos los archivos de la carpeta DesignTemplate que se encuentra en la carpeta de octubre, haga clic con el botón secundario en los archivos y seleccione la opción de menú **copiar**.
5. Haga clic con el botón secundario en el nodo del proyecto ContactManager en la ventana Explorador de soluciones de Visual Studio y seleccione la opción de menú **pegar** (vea la figura 4).
6. Seleccione la opción de menú de Visual Studio **Editar, buscar y reemplazar, reemplazo rápido** y reemplazar *[MyProjectName]* con *ContactManager* (vea la figura 5).

[![el cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image3.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image5.png)

**Figura 03**: desbloqueo de un archivo descargado de la web ([haga clic para ver la imagen de tamaño completo](iteration-2-make-the-application-look-nice-vb/_static/image6.png))

[![el cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image4.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image7.png)

**Figura 04**: sobrescritura de archivos en el explorador de soluciones ([haga clic para ver la imagen de tamaño completo](iteration-2-make-the-application-look-nice-vb/_static/image8.png))

[![el cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image5.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image9.png)

**Figura 05**: reemplazo de [ProjectName] por ContactManager ([haga clic para ver la imagen de tamaño completo](iteration-2-make-the-application-look-nice-vb/_static/image10.png))

Después de completar estos pasos, la aplicación web usará el nuevo diseño. La página de la figura 6 muestra el aspecto de la aplicación Contact Manager con el diseño de octubre.

[![el cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image6.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image11.png)

**Figura 06**: ContactManager con la plantilla de octubre ([haga clic para ver la imagen de tamaño completo](iteration-2-make-the-application-look-nice-vb/_static/image12.png))

## <a name="creating-a-custom-aspnet-mvc-design"></a>Creación de un diseño de MVC de ASP.NET personalizado

La galería de diseño de ASP.NET MVC tiene una buena selección de estilos de diseño diferentes. La galería de le proporciona una manera sencilla de personalizar la apariencia de las aplicaciones de ASP.NET MVC. Y, por supuesto, la galería tiene la gran ventaja de ser completamente gratuita.

Sin embargo, es posible que tenga que crear un diseño completamente único para su sitio Web. En ese caso, tiene sentido trabajar con una empresa de diseño de sitios Web. Decidí adoptar este enfoque para el diseño de la aplicación Contact Manager.

Se ha comprimido el administrador de contactos de la iteración #1 y se ha enviado el proyecto a la empresa de diseño. No tenían Visual Studio (Shame), pero eso no suponía ningún problema. Pudieron Descargar Microsoft Visual Web Developer de forma gratuita desde el sitio web de [https://www.asp.net](https://www.asp.net) y abrir la aplicación Contact Manager en Visual Web Developer. En un par de días, han generado el diseño en la figura 7.

[![el cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image7.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image13.png)

**Figura 07**: diseño de ASP.NET MVC Contact Manager ([haga clic para ver la imagen de tamaño completo](iteration-2-make-the-application-look-nice-vb/_static/image14.png))

El nuevo diseño se compone de dos archivos principales: un nuevo archivo de hoja de estilos en cascada y un nuevo archivo de página maestra de vista. Una página maestra de vista contiene el diseño y el contenido compartido de las vistas en una aplicación ASP.NET MVC. Por ejemplo, la página ver maestro incluye el encabezado, las pestañas de navegación y el pie de página que aparecen en la figura 7. Sobrescribió la página maestra de la vista site. Master existente en la carpeta Views\Shared con el nuevo archivo site. Master de la empresa de diseño.

La empresa de diseño también ha creado una nueva hoja de estilos en cascada y un conjunto de imágenes. Colocamos estos nuevos archivos en la carpeta de contenido y sobrescribió el archivo site. CSS existente. Debe colocar todo el contenido estático en la carpeta de contenido.

Tenga en cuenta que el nuevo diseño del administrador de contactos incluye imágenes para editar y eliminar contactos. Una imagen de edición y eliminación aparece junto a cada contacto en la tabla HTML de contactos.

Originalmente, estos vínculos se representaban con el código HTML. La aplicación auxiliar ActionLink () es similar a la siguiente:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-vb/samples/sample1.aspx)]

El método html. ActionLink () no admite imágenes (el método HTML codifica el texto del vínculo por motivos de seguridad). Por lo tanto, Reemplazamos las llamadas a HTML. ActionLink () con las llamadas a URL. Action () de la siguiente manera:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-vb/samples/sample2.aspx)]

El método html. ActionLink () representa un hipervínculo HTML completo. Por otro lado, el método URL. Action () representa solo la dirección URL sin el &lt;una etiqueta&gt;.

Tenga en cuenta, además, que el nuevo diseño incluye pestañas seleccionadas y no seleccionadas. Por ejemplo, en la figura 8, la pestaña **crear nuevo contacto** está seleccionada y la pestaña **mis contactos** no está seleccionada.

[![el cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image8.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image15.png)

**Figura 08**: pestañas seleccionadas y no seleccionadas ([haga clic para ver la imagen a tamaño completo](iteration-2-make-the-application-look-nice-vb/_static/image16.png))

Para admitir la representación de pestañas seleccionadas y no seleccionadas, he creado una aplicación auxiliar HTML personalizada denominada MenuItemHelper. Este método auxiliar representa una etiqueta de &lt;Li&gt; o una etiqueta de&gt; de &lt;Li = "Selected", en función de si el controlador y la acción actuales se corresponden con el controlador y el nombre de acción pasados al ayudante. El código de MenuItemHelper se encuentra en la lista 1.

**Lista 1: Helpers\MenuItemHelper.vb**

[!code-vb[Main](iteration-2-make-the-application-look-nice-vb/samples/sample3.vb)]

MenuItemHelper usa la clase TagBuilder internamente para compilar la etiqueta HTML &lt;Li&gt;. La clase TagBuilder es una clase de utilidad muy útil que puede usar siempre que necesite crear una nueva etiqueta HTML. Incluye métodos para agregar atributos, agregar clases CSS, generar identificadores y modificar la etiqueta HTML interno.

## <a name="summary"></a>Resumen

En esta iteración, se ha mejorado el diseño visual de nuestra aplicación ASP.NET MVC. En primer lugar, se presentó a la galería de diseño de MVC de ASP.NET. Aprendió a descargar plantillas de diseño gratuitas desde la galería de diseño de ASP.NET MVC que puede usar en las aplicaciones de ASP.NET MVC.

A continuación, se describe cómo se puede crear un diseño personalizado mediante la modificación del archivo de hoja de estilos en cascada y el archivo de página de vista maestra predeterminados. Con el fin de admitir el nuevo diseño, tuvimos que realizar algunos cambios menores en nuestra aplicación de Contact Manager. Por ejemplo, se ha agregado una nueva aplicación auxiliar HTML denominada MenuItemHelper que muestra pestañas seleccionadas y no seleccionadas.

En la siguiente iteración, abordaremos el importante asunto de la validación. Agregamos código de validación a nuestra aplicación para que un usuario no pueda crear un nuevo contacto sin proporcionar los valores necesarios, como el nombre y el apellido de una persona.

> [!div class="step-by-step"]
> [Anterior](iteration-1-create-the-application-vb.md)
> [Siguiente](iteration-3-add-form-validation-vb.md)
