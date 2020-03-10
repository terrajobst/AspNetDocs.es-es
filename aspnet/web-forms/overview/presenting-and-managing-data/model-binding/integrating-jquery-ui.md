---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: Integración del DatePicker de JQuery UI con el enlace de modelos y formularios Web Forms | Microsoft Docs
author: Rick-Anderson
description: En esta serie de tutoriales se muestran los aspectos básicos del uso del enlace de modelos con un proyecto de formularios Web Forms ASP.NET. El enlace de modelos hace que la interacción de datos sea más recta-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: c8d711dd44950116f3a3e09d5d12c507918c543f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78521983"
---
# <a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a>Integración del DatePicker de JQuery UI con el enlace de modelos y formularios Web Forms

por [Tom FitzMacken](https://github.com/tfitzmac)

> En esta serie de tutoriales se muestran los aspectos básicos del uso del enlace de modelos con un proyecto de formularios Web Forms ASP.NET. El enlace de modelos hace que la interacción de datos sea más directa que tratar con objetos de origen de datos (como ObjectDataSource o SqlDataSource). Esta serie comienza con material introductorio y se traslada a conceptos más avanzados en los tutoriales posteriores.
> 
> En este tutorial se muestra cómo agregar el [Widget DatePicker](http://jqueryui.com/datepicker/) de la interfaz de usuario de jQuery a un formulario web y cómo usar el enlace de modelos para actualizar la base de datos con el valor seleccionado.
> 
> Este tutorial se basa en el proyecto creado en la [primera](retrieving-data.md) y [segunda](updating-deleting-and-creating-data.md) parte de la serie.
> 
> Puede [Descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o VB. El código descargable funciona con Visual Studio 2012 o Visual Studio 2013. Usa la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2013 que se muestra en este tutorial.

## <a name="what-youll-build"></a>Lo que va a compilar

En este tutorial, hará lo siguiente:

1. Agregar una propiedad al modelo para registrar la fecha de inscripción del estudiante
2. Permitir al usuario seleccionar la fecha de inscripción mediante el widget DatePicker de JQuery UI
3. Aplicar reglas de validación para la fecha de inscripción

El widget DatePicker de la interfaz de usuario de JQuery permite a los usuarios seleccionar fácilmente una fecha de un calendario que aparece cuando el usuario interactúa con el campo. El uso de este widget puede ser más cómodo para los usuarios que escribir manualmente una fecha. La integración del widget DatePicker en una página que usa el enlace de modelos para las operaciones de datos requiere solo una pequeña cantidad de trabajo adicional.

## <a name="add-a-new-property-to-the-model"></a>Agregar una nueva propiedad al modelo

En primer lugar, agregará una propiedad **DateTime** a su modelo Student y migrará ese cambio a la base de datos. Abra **UniversityModels.CS**y agregue el código resaltado al modelo Student.

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

**RangeAttribute** se incluye para aplicar reglas de validación para la propiedad. En este tutorial, supondremos que contoso University se fundó el 1 de enero de 2013 y, por lo tanto, las fechas de inscripción anteriores no son válidas.

En la ventana de Administración de paquetes, agregue una migración ejecutando el comando **Add-Migration AddEnrollmentDate**. Tenga en cuenta que el código de migración agrega la nueva columna de fecha y hora a la tabla Student. Para que coincida con el valor especificado en RangeAttribute, agregue un valor predeterminado para la nueva columna, tal como se muestra en el código resaltado a continuación.

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

Guarde el cambio en el archivo de migración.

No es necesario volver a inicializar los datos. Por lo tanto, Abra **Configuration.CS** en la carpeta Migrations y quite o comente el código en el método de **inicialización** . Guarde y cierre el archivo.

Ahora, ejecute el comando **Update-Database**. Observe que la columna existe ahora en la base de datos y todos los registros existentes tienen el valor predeterminado para EnrollmentDate.

## <a name="add-dynamic-controls-for-enrollment-date"></a>Agregar controles dinámicos para la fecha de inscripción

Ahora agregará controles para mostrar y editar la fecha de inscripción. En este punto, el valor se edita a través de un cuadro de texto. Más adelante en el tutorial, cambiará el cuadro de texto al widget DatePicker de JQuery.

En primer lugar, es importante tener en cuenta que no es necesario realizar ningún cambio en el archivo **AddStudent. aspx** . El control DynamicEntity representará automáticamente la nueva propiedad.

Abra **Students. aspx**y agregue el siguiente código resaltado.

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

Ejecute la aplicación y observe que puede establecer el valor de la fecha de inscripción escribiendo una fecha. Al agregar un estudiante nuevo:

![establecer fecha](integrating-jquery-ui/_static/image1.png)

O bien, editando un valor existente:

![Editar fecha](integrating-jquery-ui/_static/image2.png)

Escribir la fecha funciona, pero puede que no sea la experiencia del cliente que desea proporcionar. En la siguiente sección, habilitará la selección de una fecha a través de un calendario.

## <a name="install-nuget-package-to-work-with-jquery-ui"></a>Instalación de un paquete NuGet para trabajar con JQuery UI

El paquete NuGet de la **interfaz de usuario de zumo** permite integrar fácilmente los widgets de la interfaz de usuario de jQuery en la aplicación Web. Para usar este paquete, instálelo a través de NuGet.

![agregar la interfaz de usuario de zumo](integrating-jquery-ui/_static/image3.png)

La versión de la interfaz de usuario de zumo que instale puede entrar en conflicto con la versión de JQuery en la aplicación. Antes de continuar con este tutorial, intente ejecutar la aplicación. Si encuentra un error de JavaScript, debe conciliar la versión de JQuery. Puede Agregar la versión esperada de JQuery a la carpeta scripts (versión 1.8.2 en el momento de escribir este tutorial) o en site. Master especifique la ruta de acceso al archivo JQuery.

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a>Personalización de la plantilla de fecha y hora para incluir el widget DatePicker

Agregará el widget DatePicker a la plantilla de datos dinámicos para editar un valor DateTime. Al agregar el widget a la plantilla, se representa automáticamente en el formulario para agregar un nuevo estudiante y en la vista de cuadrícula para editar alumnos. Abra **DateTime\_Edit. ascx**y agregue el siguiente código resaltado.

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

En el archivo de código subyacente, se establecerán las fechas mínima y máxima del DatePicker. Al establecer estos valores, impedirá que los usuarios naveguen a fechas no válidas. Recuperará los valores mínimo y máximo de **RangeAttribute** en la propiedad DateTime, si se proporciona uno. Abra **DateTime\_Edit.ascx.CS**y agregue el siguiente código resaltado a la página\_método Load.

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

Ejecute la aplicación web y vaya a la página AddStudent. Proporcione valores para los campos y tenga en cuenta que al hacer clic en el cuadro de texto para la fecha de inscripción, se muestra el calendario.

![Selector de fecha](integrating-jquery-ui/_static/image4.png)

Seleccione una fecha y haga clic en **Insertar**. RangeAttribute aplica la validación en el servidor. Al establecer la propiedad minDate en DatePicker, también se aplica la validación en el cliente. El calendario no permite al usuario desplazarse a una fecha anterior al valor de minDate.

Al editar un registro en la vista de cuadrícula, también se muestra el calendario.

![DatePicker en GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a>Conclusión

En este tutorial, aprendió a incorporar un widget de JQuery a un formulario web que usa el enlace de modelos.

En el siguiente [tutorial](using-query-string-values-to-retrieve-data.md), usará un valor de cadena de consulta al seleccionar los datos.

> [!div class="step-by-step"]
> [Anterior](sorting-paging-and-filtering-data.md)
> [Siguiente](using-query-string-values-to-retrieve-data.md)
