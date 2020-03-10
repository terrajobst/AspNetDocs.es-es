---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Detección de la clase de inicio OWIN | Microsoft Docs
author: Praburaj
description: En este tutorial se muestra cómo configurar la clase de inicio OWIN cargada. Para obtener más información sobre OWIN, consulte información general de Project Katana. Este tutorial fue...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: e1670c32049ec33dd4d1941a091a429d3929c452
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78500185"
---
# <a name="owin-startup-class-detection"></a>Detección de la clase de inicio OWIN

> En este tutorial se muestra cómo configurar la clase de inicio OWIN cargada. Para obtener más información sobre OWIN, consulte [información general de Project Katana](an-overview-of-project-katana.md). Este tutorial fue escrito por Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan y Howard Dierking ( [@howard\_Dierking](https://twitter.com/howard_dierking) ).
>
> ## <a name="prerequisites"></a>Requisitos previos
>
> [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)

## <a name="owin-startup-class-detection"></a>Detección de la clase de inicio OWIN

 Cada aplicación OWIN tiene una clase de inicio donde se especifican los componentes de la canalización de la aplicación. Hay diferentes formas de conectar la clase startup con el tiempo de ejecución, en función del modelo de hospedaje que elija (OwinHost, IIS e IIS-Express). La clase de inicio que se muestra en este tutorial se puede usar en todas las aplicaciones de hospedaje. La clase startup se conecta con el tiempo de ejecución de hospedaje mediante uno de estos métodos:

1. **Convención de nomenclatura**: Katana busca una clase denominada `Startup` en el espacio de nombres que coincide con el nombre de ensamblado o el espacio de nombres global.
2. **Atributo OwinStartup**: este es el método que la mayoría de los desarrolladores tomarán para especificar la clase startup. El atributo siguiente establecerá la clase startup en la clase `TestStartup` en el espacio de nombres `StartupDemo`.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   El atributo `OwinStartup` invalida la Convención de nomenclatura. También puede especificar un nombre descriptivo con este atributo; sin embargo, si usa un nombre descriptivo, también debe usar el elemento `appSetting` del archivo de configuración.
3. **El elemento appSetting del archivo de configuración**: el elemento `appSetting` invalida el atributo `OwinStartup` y la Convención de nomenclatura. Puede tener varias clases de inicio (cada una con un atributo `OwinStartup`) y configurar la clase de inicio que se cargará en un archivo de configuración con un marcado similar al siguiente:

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   También se puede usar la clave siguiente, que especifica explícitamente la clase de inicio y el ensamblado:

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   El siguiente XML del archivo de configuración especifica un nombre de clase de inicio descriptivo de `ProductionConfiguration`.

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   El marcado anterior se debe usar con el siguiente `OwinStartup` atributo, que especifica un nombre descriptivo y hace que se ejecute la clase `ProductionStartup2`.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. Para deshabilitar la detección de inicio de OWIN, agregue el `appSetting owin:AutomaticAppStartup` con un valor de `"false"` en el archivo Web. config.

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a>Creación de una aplicación Web de ASP.NET con el inicio de OWIN

1. Cree una aplicación Web de Asp.Net vacía y asígnele el nombre **StartupDemo**. -Instalar `Microsoft.Owin.Host.SystemWeb` mediante el administrador de paquetes NuGet. En el menú **herramientas** , seleccione **Administrador de paquetes NuGet**y, a continuación, **consola del administrador de paquetes**. Escriba el comando siguiente:

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. Agregue una clase de inicio OWIN. En Visual Studio 2017, haga clic con el botón derecho en el proyecto y seleccione **Agregar clase**.-en el cuadro de diálogo **Agregar nuevo elemento** , escriba *OWIN* en el campo de búsqueda y cambie el nombre a startup.CS y, a continuación, seleccione **Agregar**.

     ![](owin-startup-class-detection/_static/image1.png)

   La próxima vez que desee agregar una *clase de inicio Owin*, estará disponible en el menú **Agregar** .

     ![](owin-startup-class-detection/_static/image2.png)

   Como alternativa, puede hacer clic con el botón derecho en el proyecto, seleccionar **Agregar**, seleccionar **nuevo elemento**y, a continuación, seleccionar la **clase de inicio Owin**.

     ![](owin-startup-class-detection/_static/image3.png)

- Reemplace el código generado en el archivo *Startup.CS* por lo siguiente:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]

  El `app.Use` expresión lambda se usa para registrar el componente de middleware especificado en la canalización OWIN. En este caso, estamos configurando el registro de las solicitudes entrantes antes de responder a la solicitud entrante. El parámetro `next` es el delegado ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt;) al siguiente componente de la canalización. La expresión lambda `app.Run` enlaza la canalización a las solicitudes entrantes y proporciona el mecanismo de respuesta.
     > [!NOTE]
     > En el código anterior hemos comentado el atributo `OwinStartup` y estamos confiando en la Convención de ejecución de la clase denominada `Startup`.-Presione ***F5*** para ejecutar la aplicación. Actualización de aciertos varias veces.

    ![](owin-startup-class-detection/_static/image4.png) Nota: el número que se muestra en las imágenes de este tutorial no coincidirá con el número que vea. La cadena de milisegundos se usa para mostrar una nueva respuesta al actualizar la página.
  Puede ver la información de seguimiento en la ventana de **salida** .

    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a>Agregar más clases de inicio

En esta sección, vamos a agregar otra clase de inicio. Puede agregar varias clases de inicio de OWIN a la aplicación. Por ejemplo, puede que desee crear clases de inicio para desarrollo, pruebas y producción.

1. Cree una nueva clase de inicio OWIN y asígnele el nombre `ProductionStartup`.
2. Reemplace el código generado con el siguiente:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. Presione Ctrl F5 para ejecutar la aplicación. El atributo `OwinStartup` especifica que se ejecuta la clase de inicio de producción.

    ![](owin-startup-class-detection/_static/image6.png)
4. Cree otra clase de inicio de OWIN y asígnele el nombre `TestStartup`.
5. Reemplace el código generado con el siguiente:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   La sobrecarga de `OwinStartup` atributo anterior especifica `TestingConfiguration` como el nombre *descriptivo* de la clase startup.
6. Abra el archivo *Web. config* y agregue la clave de inicio de la aplicación OWIN que especifica el nombre descriptivo de la clase startup:

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. Presione Ctrl F5 para ejecutar la aplicación. El elemento de configuración de la aplicación toma el precedente y se ejecuta la configuración de prueba.

    ![](owin-startup-class-detection/_static/image7.png)
8. Quite el nombre *descriptivo* del atributo `OwinStartup` de la clase `TestStartup`.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. Reemplace la clave de inicio de la aplicación OWIN en el archivo *Web. config* por lo siguiente:

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. Revierta el atributo `OwinStartup` de cada clase en el código de atributo predeterminado generado por Visual Studio:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    Cada una de las claves de inicio de la aplicación OWIN siguiente hará que se ejecute la clase Production.

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    La última clave de inicio especifica el método de configuración de inicio. La siguiente clave de inicio de la aplicación OWIN permite cambiar el nombre de la clase de configuración a `MyConfiguration`.

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a>Usar Owinhost. exe

1. Reemplace el archivo Web. config por el marcado siguiente:

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   La última clave gana, por lo que en este caso se especifica `TestStartup`.
2. Instale Owinhost desde el PMC:

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. Navegue hasta la carpeta de la aplicación (la carpeta que contiene el archivo *Web. config* ) y en un símbolo del sistema y escriba:

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   La ventana comandos mostrará:

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. Inicie un explorador con la dirección URL `http://localhost:5000/`.

    ![](owin-startup-class-detection/_static/image8.png)

   OwinHost respeta las convenciones de inicio mencionadas anteriormente.
5. En la ventana de comandos, presione Entrar para salir de OwinHost.
6. En la clase `ProductionStartup`, agregue el siguiente atributo OwinStartup que especifica un nombre descriptivo de *ProductionConfiguration*.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. En el símbolo del sistema y escriba:

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   Se carga la clase de inicio de producción.
    ![](owin-startup-class-detection/_static/image9.png)

   Nuestra aplicación tiene varias clases de inicio y en este ejemplo hemos diferido qué clase de inicio se va a cargar hasta el tiempo de ejecución.
8. Pruebe las siguientes opciones de inicio en tiempo de ejecución:

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
