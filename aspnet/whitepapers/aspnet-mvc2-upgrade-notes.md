---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: Actualización de una aplicación ASP.NET MVC 1,0 a ASP.NET MVC 2 | Microsoft Docs
author: rick-anderson
description: En este documento se describe cómo actualizar manualmente y con un asistente una aplicación ASP.NET MVC 1,0 a ASP.NET MVC 2. Este documento también está disponible para d...
ms.author: riande
ms.date: 04/08/2010
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: 27589f1b1c9d5038118e5ff0cc2e7cecae17d5ed
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78517303"
---
# <a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a>Actualizar una aplicación de ASP.NET MVC 1.0 a ASP.NET MVC 2

> En este documento se describe cómo actualizar manualmente y con un asistente una aplicación ASP.NET MVC 1,0 a ASP.NET MVC 2. Este documento también está disponible para su [descarga](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)

## <a name="introduction"></a>Introducción

ASP.NET MVC 2 se puede instalar en paralelo con ASP.NET MVC 1,0 en el mismo servidor. Esto proporciona a los desarrolladores de aplicaciones flexibilidad a la hora de elegir cuándo actualizar una aplicación ASP.NET MVC 1,0 a ASP.NET MVC 2.

Visual Studio 2010 incluye un asistente que actualiza los proyectos existentes de ASP.NET MVC 1,0 creados con Visual Studio 2008 a ASP.NET MVC 2. El Asistente para actualización se inicia abriendo un proyecto ASP.NET MVC 1,0 en Visual Studio 2010.

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a>Asistente para actualización de ASP.NET MVC 1,0 en Visual Studio 2008 SP1

Para actualizar una aplicación ASP.NET MVC 1,0 a ASP.NET MVC 2 en Visual Studio 2008 SP1, use la aplicación MvcAppConverter (no compatible). Puede descargar esta aplicación desde la dirección URL siguiente:

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a>Actualización manual de un proyecto de ASP.NET MVC 1,0

Para actualizar manualmente una aplicación existente de ASP.NET MVC 1,0 a la versión 2, siga estos pasos:

1. Realice una copia de seguridad del proyecto existente.
2. En un editor de texto, abra el archivo de proyecto (el archivo con la extensión de archivo. csproj o. vbproj) y busque el elemento ProjectTypeGuid. Como valor de ese elemento, reemplace el GUID {603c0e0b-db56-11dc-be95-000d561079b0} por {F85E285D-A4E0-4152-9332-AB1D724D3325}. Cuando haya terminado, el valor de ese elemento debe ser el siguiente: 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. En la carpeta raíz de la aplicación Web, edite el archivo Web. config. Busque System. Web. MVC, version = 1.0.0.0 y reemplace todas las instancias por System. Web. MVC, version = 2.0.0.0.
4. Repita el paso anterior para el archivo Web. config ubicado en la carpeta views.
5. Abra el proyecto con Visual Studio y, en **Explorador de soluciones**, expanda el nodo **referencias** . Elimine la referencia a System. Web. MVC (que apunta al ensamblado de la versión 1,0). Agregue una referencia a System. Web. MVC (v 2.0.0.0).
6. Agregue el siguiente elemento bindingRedirect al archivo Web. config en la raíz de la aplicación en la sección de configuración:   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. Cree una nueva aplicación ASP.NET MVC 2 vacía. Copie los archivos de la carpeta scripts de la nueva aplicación en la carpeta scripts de la aplicación existente.
8. Actualice el archivo CSS™ aplicación s existente con las definiciones de estilo CSS en el archivo site. CSS.
9. Compile la aplicación y ejecútela. Si se produce algún error, consulte la sección de cambios importantes de la página [novedades de ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) .
