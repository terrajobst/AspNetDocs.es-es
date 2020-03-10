---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
title: Usar CascadingDropDown con una base deC#datos () | Microsoft Docs
author: wenz
description: El control CascadingDropDown en el kit de herramientas de control de AJAX extiende un control DropDownList para que los cambios en un objeto DropDownList carguen los valores asociados en Anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 684f0c28-a490-4e5b-b5e5-5dfb77464b49
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: bcf453170d17807b4e3b2d2a8b545cba43139f89
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78483751"
---
# <a name="using-cascadingdropdown-with-a-database-c"></a>Usar CascadingDropDown con una base de datos (C#)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1CS.pdf)

> El control CascadingDropDown en el kit de herramientas de control de AJAX extiende un control DropDownList para que los cambios en un objeto DropDownList carguen los valores asociados en otro DropDownList. Para que esto funcione, se debe crear un servicio Web especial.

## <a name="overview"></a>Información general

El control CascadingDropDown en el kit de herramientas de control de AJAX extiende un control DropDownList para que los cambios en un objeto DropDownList carguen los valores asociados en otro DropDownList. (Por ejemplo, una lista proporciona una lista de Estados de EE. UU. y la siguiente lista se rellena con las ciudades principales en ese estado). Para que esto funcione, se debe crear un servicio Web especial.

## <a name="steps"></a>Pasos

En primer lugar, se necesita un origen de datos. Este ejemplo utiliza la base de datos AdventureWorks y el Microsoft SQL Server 2005 Express Edition. La base de datos es una parte opcional de una instalación de Visual Studio (incluida Express Edition) y también está disponible como descarga independiente en [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). La base de datos AdventureWorks forma parte de los ejemplos de SQL Server 2005 y de las bases de datos de ejemplo (descarga en [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). La forma más sencilla de configurar la base de datos es usar el Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) y adjuntar el archivo de base de datos `AdventureWorks.mdf`.

En este ejemplo, se supone que la instancia del SQL Server 2005 Express Edition se denomina `SQLEXPRESS` y reside en el mismo equipo que el servidor Web. Esta es también la configuración predeterminada. Si el programa de instalación es diferente, tendrá que adaptar la información de conexión de la base de datos.

Para activar la funcionalidad de ASP.NET AJAX y el kit de herramientas de control, el control `ScriptManager` debe colocarse en cualquier parte de la página (pero dentro del &lt;`form`elemento &gt;):

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample1.aspx)]

En el paso siguiente, se necesitan dos controles DropDownList. En este ejemplo, usamos la información de proveedor y contacto de AdventureWorks, por lo que creamos una lista para los proveedores disponibles y otra para los contactos disponibles:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample2.aspx)]

A continuación, se deben agregar dos extensores CascadingDropDown a la página. Una rellena la primera lista (proveedores) y la otra rellena la segunda lista (contactos). Se deben establecer los siguientes atributos:

- `ServicePath`: dirección URL de un servicio Web que entrega las entradas de la lista
- `ServiceMethod`: método Web que entrega las entradas de la lista
- `TargetControlID`: ID. de la lista desplegable
- `Category`: información de categoría que se envía al método Web cuando se llama
- `PromptText`: texto que se muestra cuando se cargan datos de lista de forma asincrónica desde el servidor
- `ParentControlID`: (opcional) lista desplegable principal que desencadena la carga de la lista actual

Dependiendo del lenguaje de programación utilizado, el nombre del servicio Web en cuestión cambia, pero todos los demás valores de atributo son los mismos. Este es el elemento CascadingDropDown de la primera lista desplegable:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample3.aspx)]

Los extensores de control de la segunda lista deben establecer el atributo de `ParentControlID` para que la selección de una entrada en la lista de proveedores desencadene la carga de elementos asociados en la lista de contactos.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample4.aspx)]

El trabajo real se realiza entonces en el servicio Web, que se configura como se indica a continuación. Tenga en cuenta que se usa el atributo `[ScriptService]`; de lo contrario, ASP.NET AJAX no puede crear el proxy de JavaScript para tener acceso a los métodos Web desde el código de script del lado cliente.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample5.aspx)]

La firma de los métodos Web a los que llama CascadingDropDown es la siguiente:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample6.cs)]

Por lo tanto, el valor devuelto debe ser una matriz de tipo `CascadingDropDownNameValue` definida por el kit de herramientas del control. El método `GetVendors()` es bastante fácil de implementar: el código se conecta a la base de datos AdventureWorks y consulta los 25 primeros proveedores. El primer parámetro del constructor `CascadingDropDownNameValue` es el título de la entrada de la lista, el segundo, su valor (atributo de valor en el &lt;de HTML `option`elemento &gt;). Este es el código:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample7.cs)]

Obtener los contactos asociados para un proveedor (nombre de método: `GetContactsForVendor()`) es un poco más complicado. En primer lugar, se debe determinar el proveedor que se ha seleccionado en la primera lista desplegable. El kit de herramientas de control define un método auxiliar para esa tarea: el método `ParseKnownCategoryValuesString()` devuelve un elemento `StringDictionary` con los datos desplegables:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample8.cs)]

Por motivos de seguridad, estos datos se deben validar primero. Por lo tanto, si hay una entrada de proveedor (porque la propiedad `Category` del primer elemento CascadingDropDown está establecida en `"Vendor"`), se puede recuperar el ID. del proveedor seleccionado:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample9.cs)]

El resto del método es bastante directo y, a continuación,. El identificador del proveedor se usa como parámetro de una consulta SQL que recupera todos los contactos asociados para ese proveedor. Una vez más, el método devuelve una matriz de tipo `CascadingDropDownNameValue`.

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample10.cs)]

Cargue la página ASP.NET y, después de un breve, la lista de proveedores se rellena con 25 entradas. Seleccione una entrada y observe cómo la segunda lista desplegable se rellena con datos.

[![la primera lista se rellena automáticamente](using-cascadingdropdown-with-a-database-cs/_static/image2.png)](using-cascadingdropdown-with-a-database-cs/_static/image1.png)

La primera lista se rellena automáticamente ([haga clic para ver la imagen de tamaño completo](using-cascadingdropdown-with-a-database-cs/_static/image3.png))

[![la segunda lista se rellena de acuerdo con la selección de la primera lista](using-cascadingdropdown-with-a-database-cs/_static/image5.png)](using-cascadingdropdown-with-a-database-cs/_static/image4.png)

La segunda lista se rellena de acuerdo con la selección de la primera lista ([haga clic para ver la imagen de tamaño completo](using-cascadingdropdown-with-a-database-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Anterior](filling-a-list-using-cascadingdropdown-cs.md)
> [Siguiente](presetting-list-entries-with-cascadingdropdown-cs.md)
