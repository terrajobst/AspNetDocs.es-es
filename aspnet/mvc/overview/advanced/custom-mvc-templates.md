---
uid: mvc/overview/advanced/custom-mvc-templates
title: Plantilla de MVC personalizada | Microsoft Docs
author: joeloff
description: Cree una plantilla como una extensión VSIX.
ms.author: riande
ms.date: 12/10/2012
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: 0603bc24e070e223551813f66a75889a2e46fd35
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78499603"
---
# <a name="custom-mvc-template"></a>Plantilla personalizada de MVC

por [Jacques Eloff](https://github.com/joeloff)

La versión de MVC 3 Tools Update para Visual Studio 2010 presentó un asistente de proyecto independiente para proyectos de MVC. El cambio se ha controlado por dos factores. En primer lugar, la introducción de nuevas plantillas en MVC 3 y la compatibilidad con motores de vista adicionales como Razor conducen a saturar el cuadro de diálogo nuevo proyecto en Visual Studio. En segundo lugar, los clientes habían solicitado puntos de extensibilidad y el Asistente para nuevo proyecto de MVC nos proporcionaría la oportunidad de responder a estas solicitudes.

Agregar plantillas personalizadas era un proceso arduo en el que se usaba el registro para hacer que las nuevas plantillas estuvieran visibles para el Asistente para proyectos de MVC. El autor de una nueva plantilla tenía que ajustarla dentro de una MSI para asegurarse de que se crearían las entradas del registro necesarias en el momento de la instalación. La alternativa era crear un archivo ZIP que contenga la plantilla disponible y hacer que el usuario final cree manualmente las entradas del registro necesarias.

Ninguno de los enfoques anteriores es idóneo, por lo que hemos decidido aprovechar algunas de las infraestructuras existentes que proporcionan las extensiones [VSIX](https://msdn.microsoft.com/library/ff363239.aspx) para que sea más fácil crear, distribuir e instalar plantillas MVC personalizadas a partir de MVC 4 para Visual Studio 2012. Algunas de las ventajas que ofrece este enfoque son:

- Una extensión VSIX puede contener varias plantillas que admiten diferentes lenguajes (C# y Visual Basic) y varios motores de vista (aspx y Razor).
- Una extensión VSIX puede tener como destino varias SKU de Visual Studio, incluidas las SKU Express.
- La [Galería de Visual Studio](https://visualstudiogallery.msdn.microsoft.com/) facilita la distribución de la extensión a un público ancho.
- Las extensiones VSIX se pueden actualizar facilitando la creación de correcciones y actualizaciones en las plantillas personalizadas.

## <a name="prerequisites"></a>Requisitos previos

- Los usuarios deben estar familiarizados con la creación de plantillas de proyecto, incluido el marcado necesario para los archivos vstemplate, etc.
- Los usuarios deberán tener Visual Studio Professional y versiones posteriores instaladas. Las SKU Express no admiten la creación de proyectos VSIX.
- [SDK de Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=30668) instalado.

## <a name="example"></a>Ejemplo

El primer paso es crear un nuevo proyecto de VSIX con C# o Visual Basic. Seleccione **archivo > nuevo proyecto**y, a continuación, haga clic en **extensibilidad** en el panel izquierdo y seleccione el **Proyecto VSIX**.

![nuevo proyecto](custom-mvc-templates/_static/image1.jpg)

Una vez creado el proyecto, se abrirá el diseñador VSIX.

![Metadatos del diseñador de proyectos](custom-mvc-templates/_static/image2.jpg)

El diseñador se puede usar para editar algunas de las propiedades generales de la extensión que se mostrarán a los usuarios cuando instalen la extensión o examinen las extensiones instaladas en Visual Studio (**herramientas > extensiones y actualizaciones**). Una vez que haya completado la información general, haga clic en la **pestaña destinos de instalación**.

![Destinos de instalación del diseñador de proyectos](custom-mvc-templates/_static/image3.jpg)

Esta pestaña se usa para especificar las SKU y las versiones de Visual Studio que son compatibles con la extensión. Active la casilla de **este VSIX está instalado para que todos los usuarios** habiliten las instalaciones por equipo de VSIX. Haga clic en el botón **nuevo** de la derecha para agregar SKU adicionales, como Web Developer Express (vWD existente).

![Agregar nuevo destino de instalación](custom-mvc-templates/_static/image4.jpg)

Si piensa admitir todas las SKU profesionales y posteriores (Professional, Premium y Ultimate), solo tiene que seleccionar la SKU mínima en la familia **Microsoft.VisualStudio.Pro**. Recuerde guardar todos los cambios una vez que haya completado los destinos de instalación.

![Destinos de instalación del diseñador de proyectos](custom-mvc-templates/_static/image5.jpg)

La pestaña **activos** se usa para agregar todos los archivos de contenido a VSIX. Puesto que MVC requiere metadatos personalizados, editará el XML sin formato del archivo de manifiesto VSIX en lugar de usar la pestaña **activos** para agregar contenido. Comience agregando el contenido de la plantilla al Proyecto VSIX. Es importante que la estructura de la carpeta y el contenido reflejen el diseño del proyecto. El ejemplo siguiente contiene cuatro plantillas de proyecto que se derivaron de la plantilla de proyecto básica de MVC. Asegúrese de que todos los archivos que componen la plantilla de proyecto (todo lo que se encuentra debajo de la carpeta ProjectTemplates) se agregan al **contenido** de ItemGroup en el archivo de Proyecto VSIX y que cada elemento contiene el conjunto de metadatos **CopyToOutputDirectory** y **IncludeInVsix** , como se muestra en el ejemplo siguiente.

el contenido de &lt;incluye =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;

&lt;CopyToOutputDirectory&gt;siempre&lt;/CopyToOutputDirectory&gt;

&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;

&lt;/Content&gt;

Si no es así, el IDE intentará compilar el contenido de la plantilla al compilar el VSIX y es probable que vea un error. Los archivos de código de las plantillas suelen contener [parámetros de plantilla](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx) especiales usados por Visual Studio cuando se crea una instancia de la plantilla de proyecto y, por lo tanto, no se pueden compilar en el IDE.

![Explorador de soluciones](custom-mvc-templates/_static/image6.jpg)

Cierre el diseñador VSIX, haga clic con el botón derecho en el archivo **source. Extension. manifest** en **Explorador de soluciones** y seleccione **abrir con** y elija la opción **Editor XML (texto)** .

![Cuadro de diálogo Abrir con](custom-mvc-templates/_static/image7.jpg)

Cree un elemento **&lt;Assets&gt;** y agregue un elemento **&lt;&gt;Asset** para cada archivo que se debe incluir en VSIX. El atributo de **tipo** de cada **&lt;elemento&gt;** debe establecerse en **Microsoft. VisualStudio. Mvc. template**. Se trata de un espacio de nombres personalizado que solo entiende el Asistente para proyectos de MVC. Consulte la documentación del esquema de VSIX 2,0 para obtener información adicional sobre la estructura y el diseño del archivo de manifiesto.

Simplemente agregar los archivos a VSIX no es suficiente para registrar las plantillas con el Asistente de MVC. Debe proporcionar información como el nombre de la plantilla, la descripción, los motores de vista admitidos y el lenguaje de programación para el Asistente de MVC. Esta información se lleva a cabo en atributos personalizados asociados al elemento **&lt;&gt;de recursos** para cada archivo **VSTemplate** .

&lt;Asset d:VsixSubPath =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;

Type =&quot;Microsoft. VisualStudio. Mvc. template&quot;

d:Source =&quot;&quot; archivo

Path =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;

ProjectType =&quot;MVC&quot;

Language =&quot;C#&quot;

ViewEngine =&quot;&quot; aspx

TemplateId =&quot;MyMvcApplication&quot;

Title =&quot;aplicación web básica personalizada&quot;

Description =&quot;una plantilla personalizada derivada de una aplicación web MVC (Razor) básica&quot;

Versión =&quot;4,0&quot;/&gt;

A continuación se muestra una explicación de los atributos personalizados que deben estar presentes:

- **ProjectType** debe establecerse en MVC.
- **Language** designa el lenguaje de desarrollo que admite la plantilla. Los valores válidos C# son o VB.
- **ViewEngine** designa el motor de vistas admitido por la plantilla como aspx o Razor. Puede especificar un valor personalizado para este campo.
- **TemplateId** se utiliza para agrupar las plantillas. Si el valor coincide con un ID. de plantilla existente, se invalidarán las plantillas registradas anteriormente con el Asistente de MVC.
- **Título** designa la breve descripción que se muestra en el Asistente de MVC debajo de cada plantilla de proyecto.
- **Descripción** designa una descripción más detallada de la plantilla.

Una vez que haya agregado todos los archivos al manifiesto y lo haya guardado, observará que en la pestaña **activos** del diseñador se mostrarán todos los archivos, pero no los atributos personalizados que ha agregado a los elementos **&lt;activo&gt;** de los archivos **VSTemplate** .

![Activos del diseñador de proyectos](custom-mvc-templates/_static/image8.jpg)

Lo único que queda ahora es compilar el Proyecto VSIX e instalarlo.

Asegúrese de que todas las instancias de Visual Studio están cerradas en el equipo en el que desea probar la extensión VSIX. Visual Studio busca nuevas extensiones durante el inicio, por lo que si el IDE está abierto mientras se instala un VSIX, deberá reiniciar Visual Studio. En el explorador, haga doble clic en el archivo VSIX para iniciar el **instalador de VSIX**, haga clic en **instalar** y, a continuación, inicie Visual Studio.

![Instalador de VSIX](custom-mvc-templates/_static/image9.jpg)

En el menú, seleccione **herramientas > extensiones y actualizaciones** para confirmar que se ha instalado la extensión. Si el instalador de VSIX ha detectado errores durante la instalación de la extensión, puede ver el registro del instalador de VSIX para obtener más información. El registro se crea normalmente en la carpeta **% temp%** del usuario que instaló la extensión, por ejemplo **C:\Users\Bob\AppData\Local\Temp**.

![Extensiones y actualizaciones](custom-mvc-templates/_static/image10.jpg)

Después de cerrar la ventana, puede crear un proyecto de MVC 4 para ver si las nuevas plantillas se muestran en el Asistente de MVC.

![Nuevo proyecto ASP.NET MVC 4](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a>Limitaciones

1. El Asistente de MVC no admite plantillas personalizadas localizadas.
2. El asistente no notificará ningún error si no encuentra plantillas personalizadas. Si falta alguno de los atributos personalizados necesarios, la plantilla se excluiría simplemente del asistente.
