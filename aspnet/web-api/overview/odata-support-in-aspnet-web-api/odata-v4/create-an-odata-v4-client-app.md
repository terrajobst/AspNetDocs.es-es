---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Crear una aplicación cliente de OData V4C#() | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: a0016cf2cc7bffe6268664395ccb38e140090310
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448123"
---
# <a name="create-an-odata-v4-client-app-c"></a>Crear una aplicación de cliente de OData v4 (C#)

por [Mike Wasson](https://github.com/MikeWasson)

En el tutorial anterior, creó un servicio OData básico que admite operaciones CRUD. Ahora vamos a crear un cliente para el servicio.

Inicie una nueva instancia de Visual Studio y cree un nuevo proyecto de aplicación de consola. En el cuadro de diálogo **nuevo proyecto** , seleccione **instalado** &gt; **plantillas** &gt;  **C# visual** &gt; **escritorio de Windows**y seleccione la plantilla aplicación de **consola** . Asigne al proyecto el nombre &quot;ProductsApp&quot;.

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> También puede Agregar la aplicación de consola a la misma solución de Visual Studio que contiene el servicio OData.

## <a name="install-the-odata-client-code-generator"></a>Instalación del generador de código de cliente de OData

En el menú **Herramientas**, seleccione **Extensiones y actualizaciones**. Seleccione **en línea** &gt; **Galería de Visual Studio**. En el cuadro de búsqueda, busque &quot;generador de código de cliente de OData&quot;. Haga clic en **Descargar** para instalar el VSIX. Es posible que se le pida que reinicie Visual Studio.

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a>Ejecutar el servicio de OData localmente

Ejecute el proyecto ProductService desde Visual Studio. De forma predeterminada, Visual Studio inicia un explorador en la raíz de la aplicación. Anote el URI; lo necesitará en el paso siguiente. Deje la aplicación en ejecución.

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> Si coloca ambos proyectos en la misma solución, asegúrese de ejecutar el proyecto ProductService sin depuración. En el paso siguiente, tendrá que mantener el servicio en ejecución mientras modifica el proyecto de aplicación de consola.

## <a name="generate-the-service-proxy"></a>Generar el proxy del servicio

El proxy de servicio es una clase .NET que define los métodos para tener acceso al servicio OData. El proxy convierte las llamadas al método en solicitudes HTTP. Va a crear la clase de proxy mediante la ejecución de una [plantilla T4](https://msdn.microsoft.com/library/bb126445.aspx).

Haga clic con el botón derecho en el proyecto. Seleccione **Agregar** &gt; **Nuevo elemento**.

![](create-an-odata-v4-client-app/_static/image5.png)

En el cuadro de diálogo **Agregar nuevo elemento** , seleccione  **C# elementos visuales** &gt; **código** &gt; **cliente de oData**. Asigne a la plantilla el nombre &quot;ProductClient.tt&quot;. Haga clic en **Agregar** y haga clic en la advertencia de seguridad.

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

En este momento, obtendrá un error, que se puede omitir. Visual Studio ejecuta automáticamente la plantilla, pero la plantilla necesita algunas opciones de configuración en primer lugar.

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

Abra el archivo ProductClient. OData. config. En el elemento `Parameter`, pegue el URI del proyecto ProductService (paso anterior). Por ejemplo:

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

Vuelva a ejecutar la plantilla. En Explorador de soluciones, haga clic con el botón derecho en el archivo ProductClient.tt y seleccione **Ejecutar herramienta personalizada**.

La plantilla crea un archivo de código denominado ProductClient.cs que define el proxy. Al desarrollar la aplicación, si cambia el punto de conexión de OData, vuelva a ejecutar la plantilla para actualizar el proxy.

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a>Usar el proxy de servicio para llamar al servicio OData

Abra el archivo Program.cs y reemplace el código reutilizable con lo siguiente.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

Reemplace el valor de *serviceUri* por el URI del servicio anterior.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

Al ejecutar la aplicación, debe generar lo siguiente:

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
