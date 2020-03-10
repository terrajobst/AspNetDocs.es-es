---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
title: Descripción de la localización de ASP.NET AJAX | Microsoft Docs
author: scottcate
description: La localización es el proceso de diseño e integración de la compatibilidad con un idioma y una referencia cultural específicos en una aplicación o un componente de la aplicación. El micrófono...
ms.author: riande
ms.date: 03/14/2008
ms.assetid: c1a35f18-bab9-41f7-8497-15530c37a09d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
msc.type: authoredcontent
ms.openlocfilehash: 003e7939accd7a68dab97441b3d999bca835b85a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78456631"
---
# <a name="understanding-aspnet-ajax-localization"></a>Descripción de localización de ASP.NET AJAX

por [Scott Cate](https://github.com/scottcate)

[Descargar PDF](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial04_Localization_cs.pdf)

> La localización es el proceso de diseño e integración de la compatibilidad con un idioma y una referencia cultural específicos en una aplicación o un componente de la aplicación. La plataforma de Microsoft ASP.NET proporciona una amplia compatibilidad para la localización de aplicaciones ASP.NET estándar mediante la integración del modelo de localización estándar de .NET. el marco de Microsoft AJAX utiliza el modelo integrado para admitir los diversos escenarios en los que se puede realizar la localización.

## <a name="introduction"></a>Introducción

La tecnología ASP.NET de Microsoft aporta un modelo de programación orientado a objetos y basado en eventos y lo agrupa con las ventajas del código compilado. Sin embargo, su modelo de procesamiento del lado servidor tiene varias desventajas inherentes a la tecnología, muchas de las cuales se pueden solucionar con las nuevas características incluidas en el espacio de nombres System. Web. Extensions, que encapsula los servicios de Microsoft AJAX en el .NET Framework 3,5. Estas extensiones habilitan muchas características de cliente enriquecidas, que anteriormente estaban disponibles como parte de las extensiones de AJAX de ASP.NET 2,0, pero ahora forman parte de la biblioteca de clases base de .NET Framework. Los controles y las características de este espacio de nombres incluyen la representación parcial de las páginas sin necesidad de una actualización de página completa, la capacidad de obtener acceso a los servicios web a través de scripts de cliente (incluida la API de generación de perfiles de ASP.NET) y una extensa API del lado cliente diseñada para reflejar muchos de los esquemas de control que se han detectado en el control del lado servidor ASP.NET.

En estas notas del producto se examinan las características de localización presentes en Microsoft AJAX Framework y Microsoft AJAX script Library, en el contexto de la necesidad empresarial de soporte técnico de localización y revisión de la compatibilidad ya integrada para la localización en Web. aplicaciones proporcionadas por el .NET Framework. La biblioteca de scripts de Microsoft AJAX usa el formato de archivo. resx que ya usan las aplicaciones .NET, que proporciona compatibilidad IDE integrada y un tipo de recurso compartible.

Estas notas del producto se basan en la versión beta 2 de Microsoft Visual Studio 2008. En estas notas del producto también se da por hecho que va a trabajar con Visual Studio 2008, no con Visual Web Developer Express, y proporcionará tutoriales de acuerdo con la interfaz de usuario de Visual Studio. Algunos ejemplos de código utilizarán plantillas de proyecto que pueden no estar disponibles en Visual Web Developer Express.

## <a name="the-need-for-localization"></a>*La necesidad de localización*

Especialmente en el caso de los desarrolladores de aplicaciones empresariales y los desarrolladores de componentes, la capacidad de crear herramientas que puedan tener en cuenta las diferencias entre las referencias culturales y los lenguajes es cada vez más necesaria. El diseño de componentes con la posibilidad de adaptarse a la configuración regional del cliente aumenta la productividad del desarrollador y reduce la cantidad de trabajo necesario para la adaptación de un componente para que funcione globalmente.

La localización es el proceso de diseño e integración de la compatibilidad con un idioma y una referencia cultural específicos en una aplicación o un componente de la aplicación. La plataforma de Microsoft ASP.NET proporciona una amplia compatibilidad para la localización de aplicaciones ASP.NET estándar mediante la integración del modelo de localización estándar de .NET. el marco de Microsoft AJAX utiliza el modelo integrado para admitir los diversos escenarios en los que se puede realizar la localización. Con el marco de Microsoft AJAX, los scripts se pueden localizar implementando en ensamblados satélite o utilizando una estructura de sistema de archivos estática.

## <a name="embedding-scripts-with-satellite-assemblies"></a>*Incrustar scripts con ensamblados satélite*

Con la estrategia de localización de .NET Framework estándar, los recursos pueden incluirse en ensamblados satélite. Los ensamblados satélite proporcionan varias ventajas con respecto a la inclusión de recursos tradicional en binarios: cualquier localización determinada se puede actualizar sin necesidad de actualizar la imagen más grande, por lo que se pueden implementar localizaciones adicionales simplemente instalando ensamblados satélite en la carpeta del proyecto y los ensamblados satélite se pueden implementar sin que se produzca una recarga del ensamblado del proyecto principal. Especialmente en los proyectos de ASP.NET, esto resulta beneficioso, ya que puede reducir significativamente la cantidad de recursos del sistema que se usan en las actualizaciones incrementales y se interrumpe el uso del sitio web de producción de forma mínima.

Los scripts se incrustan en los ensamblados incluyéndolos en archivos. resx administrados (o compilados. Resources), que se incluyen en el ensamblado en tiempo de compilación. Sus recursos se ponen a disposición de la aplicación de script a través del código generado en tiempo de ejecución de AJAX mediante atributos de nivel de ensamblado.

*Convenciones de nomenclatura para archivos de script incrustados*

La administración de scripts de Microsoft AJAX Framework admite diversas opciones de uso en la implementación y prueba de scripts, y se proporcionan directrices para facilitar estas opciones.

*Para facilitar la depuración:*

Los scripts de versión (producción) no deben incluir el calificador `.debug` en el nombre de archivo. Los scripts diseñados para la depuración deben incluir `.debug` en el nombre de archivo.

*Para facilitar la localización:*

Los scripts de referencia cultural neutra no deben incluir ningún identificador de referencia cultural en el nombre del archivo. En el caso de los scripts que contienen recursos localizados, el código de idioma ISO debe especificarse en el nombre de archivo. Por ejemplo, `es-CO` representa el español, Colombia.

En la tabla siguiente se resumen las convenciones de nomenclatura de archivos con ejemplos:

| Filename | Significado |
| --- | --- |
| Script.js | Un script neutro de versión de lanzamiento. |
| Script.debug.js | Un script de referencia cultural neutra de versión de depuración. |
| Script.en-US.js | Una versión de lanzamiento en inglés, Estados Unidos script. |
| Script.debug.es-CO.js | Un script de la versión de depuración Español de Columbia. |

## <a name="walkthrough-create-an-localized-embedded-script"></a>Tutorial: crear un script incrustado y adaptado

*Nota: este tutorial requiere el uso de Visual Studio 2008, ya que Visual Web Developer Express no incluye una plantilla de proyecto para los proyectos de biblioteca de clases.*

1. Cree un nuevo proyecto de sitio web con extensiones de ASP.NET AJAX integradas. Cree otro proyecto, un proyecto de biblioteca de clases, dentro de la solución denominada LocalizingResources.
2. Agregue un archivo JScript denominado VerifyDeletion. js al proyecto LocalizingResources, así como archivos de recursos. resx denominados DeletionResources. resx y DeletionResources. es. resx. El primero contendrá recursos de referencia cultural neutra; Este último contendrá recursos de idioma español.
3. Agregue el código siguiente a VerifyDeletion. js:

[!code-javascript[Main](understanding-asp-net-ajax-localization/samples/sample1.js)]

Para aquellos que no estén familiarizados con la sintaxis Regex de JavaScript, el texto dentro de las barras diagonales simples (en el ejemplo anterior,/FILENAME/es un ejemplo) denota un objeto RegExp. MSDN Library contiene una extensa referencia de JavaScript y los recursos de objetos nativos de JavaScript se pueden encontrar en línea.

1. Agregue las siguientes cadenas de recursos a DeletionResources. resx: 

    **VerifyDelete**: ¿está seguro de que desea eliminar el nombre de archivo?

    **Eliminado**: se ha eliminado el nombre de archivo.

1. Agregue las siguientes cadenas de recursos a DeletionResources. es. resx: 

    **VerifyDelete**: est seguro que se desee Quito nombreDeArchivo?

    **Eliminado**: nombre de archivo se quitado.
2. Agregue las siguientes líneas de código al archivo AssemblyInfo:

[!code-csharp[Main](understanding-asp-net-ajax-localization/samples/sample2.cs)]

1. Agregue referencias a System. Web y System. Web. Extensions al proyecto LocalizingResources.
2. Agregue una referencia al proyecto LocalizingResources desde el proyecto de sitio Web.
3. En default. aspx, en el proyecto de sitio web, actualice el control ScriptManager con el siguiente marcado adicional:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample3.aspx)]

1. En default. aspx, en cualquier lugar de la página, incluya este marcado:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample4.aspx)]

1. Presione F5. Si se le solicita, habilite la depuración. Cuando se cargue la página, presione el botón Eliminar. Tenga en cuenta que se le pedirá en inglés (a menos que el equipo esté configurado para preferir recursos en Español de forma predeterminada) para su confirmación.
2. Cierre la ventana del explorador y vuelva a default. aspx. En la Directiva de encabezado @Page, reemplace auto por Culture y UICulture por es-ES. Vuelva a presionar F5 para volver a iniciar la aplicación web en el explorador. Esta vez, tenga en cuenta que se le pedirá que elimine el archivo en Español:

[![](understanding-asp-net-ajax-localization/_static/image2.png)](understanding-asp-net-ajax-localization/_static/image1.png)

([Haga clic para ver la imagen de tamaño completo](understanding-asp-net-ajax-localization/_static/image3.png))

[![](understanding-asp-net-ajax-localization/_static/image5.png)](understanding-asp-net-ajax-localization/_static/image4.png)

([Haga clic para ver la imagen de tamaño completo](understanding-asp-net-ajax-localization/_static/image6.png))

Tenga en cuenta que hay varias variaciones para este tutorial. Por ejemplo, los scripts se pueden registrar con el control ScriptManager mediante programación durante la carga de la página.

## <a name="including-a-static-script-file-structure"></a>*Incluir una estructura de archivos de script estáticos*

Cuando se usan archivos de script estáticos para la implementación, se pierden algunas de las ventajas de usar el esquema de localización de .NET inherente. Principalmente, es que se pierde el tipo automático generado a partir de los archivos de recursos de script. en el tutorial anterior, por ejemplo, los recursos se exponían mediante un tipo generado automáticamente denominado Message del control ScriptManager.

Sin embargo, hay algunas ventajas de usar una estructura de archivos de script estático. Las actualizaciones se pueden realizar sin volver a compilar y volver a implementar los ensamblados satélite, y el uso de una estructura de archivos estáticos también puede realizarse para invalidar el script incrustado, con el fin de integrar una parte secundaria de la funcionalidad que es posible que no se haya enviado con un componente.

Microsoft recomienda evitar un problema de control de versiones generando automáticamente los recursos de script durante la compilación del proyecto. Al mantener una base de código de script extensa, puede resultar cada vez más difícil asegurarse de que los cambios en el código se reflejan en cada script localizado. Como alternativa, puede simplemente mantener un script lógico y varios scripts de localización, combinando los archivos durante la compilación del proyecto.

Dado que no hay recursos para incluir de forma declarativa, se debe hacer referencia a los archivos de script estáticos agregando `<asp:ScriptElement>` elementos como un elemento secundario de la etiqueta de `<Scripts>` del control ScriptManager o agregando mediante programación objetos `ScriptReference` a la propiedad `Scripts` del control `ScriptManager` en la página en tiempo de ejecución.

## <a name="the-scriptmanager-and-its-role-in-localization"></a>*El ScriptManager y su rol en la localización*

El ScriptManager habilita varios comportamientos automáticos para aplicaciones localizadas:

- Ubica automáticamente los archivos de script según la configuración y las convenciones de nomenclatura. por ejemplo, carga los scripts habilitados para la depuración en modo de depuración y carga los scripts localizados según la selección de la interfaz de usuario del explorador.
- Habilita la definición de las referencias culturales, incluidas las referencias culturales personalizadas.
- Habilita la compresión de archivos de script a través de HTTP.
- Almacena en memoria caché los scripts para administrar de forma eficaz muchas solicitudes.
- Agrega una capa de direccionamiento indirecto a los scripts mediante la canalización a través de una dirección URL cifrada.

Las referencias de script se pueden agregar al control ScriptManager mediante programación o mediante el marcado declarativo. El marcado declarativo es especialmente útil cuando se trabaja con scripts incrustados en ensamblados distintos del proyecto de sitio web en sí, ya que es probable que el nombre del script no cambie a medida que se inserten revisiones.

## <a name="summary"></a>Resumen

A medida que las aplicaciones web crecen para llegar a un público más grande, la necesidad de tener acceso a referencias culturales y comunidades más amplias se convierte en un modelo de negocio. las aplicaciones Web de comercio electrónico deben ser capaces de tratar con monedas extranjeras y los sistemas de administración de contenido deben ser capaces de no solo presentar su contenido, sino también sus sugerencias de navegación y campos de formulario en otros lenguajes, y las empresas deben saber que esta necesidad es estarán.

El .NET Framework admite intrínsecamente un marco de localización enriquecida, mediante ensamblados satélite y archivos de recursos XML (. resx) para presentar una forma uniforme de buscar cadenas de recursos e imágenes. Las extensiones de AJAX de ASP.NET, incluidas Microsoft AJAX Framework y la biblioteca de scripts de Microsoft AJAX, proporcionan compatibilidad para este modelo de programación en el código de cliente, lo que permite búsquedas sencillas de cadenas de recursos. Los ensamblados satélite admiten la inclusión automática de recursos de script (archivos. js reales) mediante ScriptResource. axd siempre que los nombres de archivo sigan un esquema de nomenclatura determinado. Con esta compatibilidad, las extensiones de AJAX de ASP.NET simplifican la localización de scripts y la globalización de las aplicaciones.

## <a name="bio"></a>*Disponibilidad*

Scott Cate ha estado trabajando con las tecnologías Web de Microsoft desde 1997 y es el Presidente de myKB.com ([www.myKB.com](http://www.myKB.com)), donde se especializa en escribir aplicaciones basadas en ASP.net centradas en las soluciones de software de la base de conocimiento. Puede ponerse en contacto con Scott por correo electrónico en [scott.cate@myKB.com](mailto:scott.cate@myKB.com) o su blog en [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Anterior](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
> [Siguiente](understanding-asp-net-ajax-web-services.md)
