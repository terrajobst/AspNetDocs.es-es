---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: ASP.NET scaffolding en Visual Studio 2013 | Microsoft Docs
author: Rick-Anderson
description: ASP.NET scaffolding es una característica nueva que se incluye en Visual Studio 2013.
ms.author: riande
ms.date: 04/09/2014
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: cf4669b769cee28475e2dd6a6ddf07ea1434d04d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449569"
---
# <a name="aspnet-scaffolding-in-visual-studio-2013"></a>Scaffolding de ASP.NET en Visual Studio 2013

por [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET scaffolding es una característica nueva que se incluye en Visual Studio 2013.

## <a name="overview"></a>Información general

ASP.NET scaffolding es un marco de generación de código para aplicaciones Web de ASP.NET. Visual Studio 2013 incluye generadores de código preinstalados para proyectos de MVC y API Web. Agregue scaffolding al proyecto si desea agregar rápidamente código que interactúe con los modelos de datos. El uso de la técnica scaffolding puede reducir la cantidad de tiempo que se tarda en desarrollar operaciones de datos estándar en el proyecto.

De forma predeterminada, Visual Studio 2013 no admite la generación de código para un proyecto de formularios Web Forms, pero puede usar la técnica scaffolding con formularios Web Forms agregando dependencias de MVC al proyecto o instalando una extensión. A continuación se muestran ambos enfoques.

Visual Studio 2013 Update 2 (actualmente RC) proporciona la capacidad de ampliar el scaffolding de ASP.NET para satisfacer los requisitos de su escenario. Con esta funcionalidad, puede crear una plantilla de scaffolding personalizada y agregarla para agregar nuevo cuadro de diálogo de scaffolding. Dentro de la plantilla personalizada, especifique el código que se genera al agregar un elemento con scaffolding. Para obtener más información, vea [crear un scaffolding personalizado para Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

## <a name="prerequisites"></a>Requisitos previos

Para usar la técnica scaffolding ASP.NET, debe tener:

- Microsoft Visual Studio 2013
- Herramientas de desarrollo web (parte de la instalación Visual Studio 2013 predeterminada)
- Marcos y herramientas Web de ASP.NET 2013 (parte de la instalación de Visual Studio 2013 predeterminada)

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a>Adición de un elemento con scaffolding a MVC o API Web

Para agregar un scaffolding, haga clic con el botón derecho en el proyecto o en una carpeta del proyecto y seleccione **Agregar** – **nuevo elemento con scaffolding**, tal como se muestra en la siguiente imagen.

![Agregar elemento scaffold](aspnet-scaffolding-overview/_static/image1.png)

En la ventana **Agregar scaffolding** , seleccione el tipo de scaffolding que desea agregar.

![Seleccionar tipo de scaffolding](aspnet-scaffolding-overview/_static/image2.png)

La ventana **Agregar controlador** le ofrece la oportunidad de seleccionar opciones para generar el controlador, incluso si desea usar las nuevas características asincrónicas de Entity Framework 6.

![Agregar controlador](aspnet-scaffolding-overview/_static/image3.png)

Las clases y páginas relevantes se crean para su escenario. Por ejemplo, la siguiente imagen muestra el controlador MVC y las vistas que se crearon mediante scaffolding para una clase de modelo denominada movies.

![Archivos creados](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a>Agregar un elemento con scaffolding a formularios Web Forms

Para agregar scaffolding que genera código de formularios Web Forms, debe instalar una extensión en Visual Studio o agregar dependencias de MVC. Ambos enfoques se muestran a continuación, pero solo tiene que realizar uno de estos enfoques.

### <a name="web-forms-scaffolding-extension"></a>Extensión de scaffolding de formularios Web Forms

Puede instalar una extensión de Visual Studio que le permita utilizar la técnica scaffolding con un proyecto de formularios Web Forms. En Visual Studio, seleccione **herramientas** y **extensiones y actualizaciones**. En este cuadro de diálogo, busque en la galería de Visual Studio para la **técnica de formularios Web Forms**.

![instalar scaffolding de formularios Web Forms](aspnet-scaffolding-overview/_static/image5.png)

Para obtener más información, consulte [scaffolding de formularios Web Forms](https://go.microsoft.com/fwlink/p/?LinkId=396478).

### <a name="mvc-dependencies"></a>Dependencias de MVC

Para agregar dependencias de MVC, seleccione **agregar** - **nuevo elemento con scaffolding**. En la ventana Agregar scaffolding, seleccione **dependencias de MVC**, tal como se muestra a continuación.

![Agregar dependencias de MVC](aspnet-scaffolding-overview/_static/image6.png)

Hay dos opciones para la scaffolding de MVC; Mínima y completa. Si selecciona mínima, solo se agregan al proyecto los paquetes NuGet y las referencias para ASP.NET MVC. Si selecciona la opción completa, se agregan las dependencias mínimas, así como los archivos de contenido necesarios para un proyecto de MVC. Para usar con facilidad el scaffolding, seleccione dependencias completas.

![seleccionar dependencias completas](aspnet-scaffolding-overview/_static/image7.png)

Después de agregar las dependencias, verá un archivo **README. txt** . Siga cuidadosamente las instrucciones de este archivo para asegurarse de que el proyecto funciona correctamente.

Cuando haya completado los pasos del archivo README. txt, puede Agregar un nuevo elemento con scaffolding, tal como se muestra en la sección anterior sobre MVC y Web API. Las vistas y el controlador generados automáticamente funcionarán correctamente en el proyecto.

## <a name="tutorials"></a>Tutoriales

Para crear un scaffolding personalizado, consulte [crear un scaffolding personalizado para Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

Para personalizar los archivos generados, consulte [Cómo personalizar los archivos generados en el cuadro de diálogo nuevo elemento con scaffolding](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).

Para obtener un ejemplo del uso de la técnica scaffolding con el **desarrollo de Database First**, consulte [EF Database First con ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).

Para obtener un ejemplo del uso de la técnica scaffolding en un proyecto de **MVC** , vea [Introducción con ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Para ver un ejemplo del uso de la técnica scaffolding en un proyecto de **API Web** , consulte [creación de una API de REST con enrutamiento de atributos en Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).
