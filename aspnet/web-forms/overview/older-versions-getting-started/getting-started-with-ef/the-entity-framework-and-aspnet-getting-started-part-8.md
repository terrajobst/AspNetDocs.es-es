---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: Introducción con Entity Framework 4,0 Database First y ASP.NET 4 formularios Web Forms, parte 8 | Microsoft Docs
author: tdykstra
description: En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET mediante el Entity Framework. La aplicación de ejemplo es...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: ff6a808dd501df8056c0fb0784a8e42e094d5db7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78473509"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a>Introducción con Entity Framework 4,0 Database First y ASP.NET 4 formularios Web Forms, parte 8

por [Tom Dykstra](https://github.com/tdykstra)

> En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET mediante el Entity Framework 4,0 y Visual Studio 2010. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](the-entity-framework-and-aspnet-getting-started-part-1.md) .

## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a>Usar la funcionalidad de datos dinámicos para dar formato y validar datos

En el tutorial anterior, implementó procedimientos almacenados. En este tutorial se muestra cómo datos dinámicos funcionalidad puede proporcionar las siguientes ventajas:

- Los campos se formatean automáticamente para su presentación en función de su tipo de datos.
- Los campos se validan automáticamente según su tipo de datos.
- Puede agregar metadatos al modelo de datos para personalizar el comportamiento de formato y validación. Al hacerlo, puede Agregar las reglas de formato y validación en un solo lugar y se aplican de forma automática en todos los lugares en los que tiene acceso a los campos mediante controles de datos dinámicos.

Para ver cómo funciona esto, cambiará los controles que utiliza para mostrar y editar los campos de la página *Students. aspx* existente, y agregará metadatos de formato y validación a los campos nombre y fecha del tipo de entidad `Student`.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a>Usar los controles DynamicField y DynamicControl

Abra la página *Students. aspx* y, en el control `StudentsGridView`, reemplace el **nombre** y la **fecha de inscripción** `TemplateField` elementos por el marcado siguiente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

Este marcado usa `DynamicControl` controles en lugar de los controles `TextBox` y `Label` en el campo de plantilla nombre de estudiante y usa un control `DynamicField` para la fecha de inscripción. No se ha especificado ninguna cadena de formato.

Agregue un control `ValidationSummary` después del control `StudentsGridView`.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

En el control `SearchGridView` reemplazar el marcado de las columnas **nombre** y **fecha de inscripción** tal como hizo en el control `StudentsGridView`, excepto omitir el elemento `EditItemTemplate`. El elemento `Columns` del control de `SearchGridView` contiene ahora el marcado siguiente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

Abra *Students.aspx.CS* y agregue la siguiente instrucción `using`:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

Agregue un controlador para el evento de `Init` de la página:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

Este código especifica que datos dinámicos proporcionará formato y validación en estos controles enlazados a datos para los campos de la entidad `Student`. Si recibe un mensaje de error similar al del ejemplo siguiente al ejecutar la página, normalmente significa que ha olvidado llamar al método `EnableDynamicData` en `Page_Init`:

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

Ejecute la página.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)

En la columna **fecha de inscripción** , se muestra la hora junto con la fecha porque el tipo de propiedad es `DateTime`. Lo corregirá más adelante.

Por ahora, tenga en cuenta que datos dinámicos proporciona automáticamente la validación de datos básica. Por ejemplo, haga clic en **Editar**, desactive el campo fecha, haga clic en **Actualizar**y verá que datos dinámicos lo convierte automáticamente en un campo obligatorio porque el valor no acepta valores NULL en el modelo de datos. La página muestra un asterisco después del campo y un mensaje de error en el control `ValidationSummary`:

[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)

Puede omitir el control `ValidationSummary`, porque también puede mantener el puntero del mouse sobre el asterisco para ver el mensaje de error:

[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)

Datos dinámicos también validará que los datos introducidos en el campo de **fecha de inscripción** sean una fecha válida:

[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)

Como puede ver, se trata de un mensaje de error genérico. En la sección siguiente, verá cómo personalizar los mensajes, así como las reglas de formato y validación.

## <a name="adding-metadata-to-the-data-model"></a>Agregar metadatos al modelo de datos

Normalmente, desea personalizar la funcionalidad proporcionada por datos dinámicos. Por ejemplo, puede cambiar el modo en que se muestran los datos y el contenido de los mensajes de error. Normalmente, también se personalizan las reglas de validación de datos para proporcionar más funcionalidad de la que datos dinámicos proporciona automáticamente en función de los tipos de datos. Para ello, cree clases parciales que se correspondan con los tipos de entidad.

En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **ContosoUniversity** , seleccione **Agregar referencia**y agregue una referencia a `System.ComponentModel.DataAnnotations`.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)

En la carpeta *Dal* , cree un nuevo archivo de clase, asígnele el nombre *Student.CS*y reemplace el código de plantilla por el código siguiente.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

Este código crea una clase parcial para la entidad `Student`. El atributo `MetadataType` aplicado a esta clase parcial identifica la clase que se usa para especificar los metadatos. La clase de metadatos puede tener cualquier nombre, pero el uso del nombre de entidad más "metadatos" es una práctica común.

Los atributos aplicados a las propiedades de la clase Metadata especifican el formato, la validación, las reglas y los mensajes de error. Los atributos que se muestran aquí tendrán los siguientes resultados:

- `EnrollmentDate` se mostrará como una fecha (sin una hora).
- Ambos campos de nombre deben tener una longitud de 25 caracteres o menos, y se proporciona un mensaje de error personalizado.
- Ambos campos de nombre son obligatorios y se proporciona un mensaje de error personalizado.

Vuelva a ejecutar la página *Students. aspx* y verá que las fechas se muestran ahora sin las horas:

[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)

Edite una fila e intente borrar los valores de los campos nombre. Los asteriscos que indican errores de campo aparecen en cuanto deja un campo, antes de hacer clic en **Actualizar**. Al hacer clic en **Actualizar**, la página muestra el texto del mensaje de error especificado.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)

Intente especificar nombres de más de 25 caracteres, haga clic en **Actualizar**y la página mostrará el texto del mensaje de error especificado.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)

Ahora que ha configurado estas reglas de formato y validación en los metadatos del modelo de datos, las reglas se aplicarán automáticamente en todas las páginas que muestren o permitan cambios en estos campos, siempre y cuando use `DynamicControl` o `DynamicField` controles. Esto reduce la cantidad de código redundante que tiene que escribir, lo que facilita la programación y las pruebas, y garantiza que el formato y la validación de los datos son coherentes en toda la aplicación.

## <a name="more-information"></a>Más información

Esto concluye esta serie de tutoriales sobre Introducción con la Entity Framework. Para obtener más recursos que le ayudarán a aprender a usar el Entity Framework, continúe con [el primer tutorial de la próxima Entity Framework serie de tutoriales](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) o visite los sitios siguientes:

- [Preguntas más frecuentes sobre Entity Framework](http://www.ef-faq.org/introduction.html)
- [Blog del equipo de Entity Framework](https://blogs.msdn.com/b/adonet/)
- [Entity Framework en MSDN Library](https://msdn.microsoft.com/library/bb399572.aspx)
- [Entity Framework en el centro para desarrolladores de datos de MSDN](https://msdn.microsoft.com/data/ef.aspx)
- [Información general sobre el control de servidor Web EntityDataSource en MSDN Library](https://msdn.microsoft.com/library/cc488502.aspx)
- [Referencia de la API del control EntityDataSource en MSDN Library](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [Foros de Entity Framework en MSDN](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [Blog de Julia Lerman](http://thedatafarm.com/blog/)

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-7.md)
