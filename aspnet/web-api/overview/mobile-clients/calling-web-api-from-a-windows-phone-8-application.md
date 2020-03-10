---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Llamada a la API Web desde una aplicación Windows PhoneC#8 ()-ASP.net 4. x
author: rmcmurray
description: 'Tutorial con código: cree una aplicación ASP.NET Web API en ASP.NET 4. x que proporciona un catálogo de libros para una aplicación Windows Phone 8.'
ms.author: riande
ms.date: 10/09/2013
ms.custom: seoapril2019
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: c5da14a6856f551343b6fb14f0aedc659e792f6b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78498217"
---
# <a name="calling-web-api-from-a-windows-phone-8-application-c"></a>Llamar a Web API desde una aplicación de Windows Phone 8 (C#)

por [Robert McMurray](https://github.com/rmcmurray)

En este tutorial, aprenderá a crear un escenario completo de un extremo a otro que consiste en una aplicación ASP.NET Web API que proporciona un catálogo de libros para una aplicación Windows Phone 8.

### <a name="overview"></a>Información general

Los servicios RESTful como ASP.NET Web API simplifican la creación de aplicaciones basadas en HTTP para desarrolladores mediante la abstracción de la arquitectura para las aplicaciones del lado servidor y del lado cliente. En lugar de crear un protocolo basado en sockets propietario para la comunicación, los desarrolladores de la API Web simplemente necesitan publicar los métodos HTTP necesarios para su aplicación (por ejemplo: GET, POST, PUT, DELETE) y los desarrolladores de aplicaciones cliente solo necesitan consumir los métodos HTTP necesarios para su aplicación.

En este tutorial de un extremo a otro, aprenderá a usar la API Web para crear los siguientes proyectos:

- En la [primera parte de este tutorial](#STEP1), creará una aplicación ASP.net web API que admita todas las operaciones de creación, lectura, actualización y eliminación (CRUD) para administrar un catálogo de libros. Esta aplicación usará el [archivo XML de ejemplo (Books. xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) de MSDN.
- En la [segunda parte de este tutorial](#STEP2), creará una aplicación interactiva Windows Phone 8 que recupera los datos de la aplicación de API Web.

#### <a name="prerequisites"></a>Requisitos previos

- Visual Studio 2013 con el SDK de Windows Phone 8 instalado
- Windows 8 o posterior en un sistema de 64 bits con Hyper-V instalado
- Para obtener una lista de requisitos adicionales, consulte la sección *requisitos del sistema* en la página de descarga del [SDK de Windows Phone 8,0](https://www.microsoft.com/download/details.aspx?id=35471) .

> [!NOTE]
> Si va a probar la conectividad entre los proyectos de API Web y Windows Phone 8 en el sistema local, deberá seguir las instrucciones del artículo *[conexión del emulador de Windows Phone 8 a aplicaciones de API Web en un equipo local](https://go.microsoft.com/fwlink/?LinkId=324014)* para configurar el entorno de pruebas.

<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a>Paso 1: creación del proyecto de librería de API Web

El primer paso de este tutorial de un extremo a otro es crear un proyecto de API Web que admita todas las operaciones CRUD; Tenga en cuenta que agregará el proyecto de aplicación de Windows Phone a esta solución en el [paso 2](#STEP2) de este tutorial.

1. Abra **Visual Studio 2013**.
2. Haga clic en **archivo**, **nuevo**y **proyecto**.
3. Cuando se muestre el cuadro de diálogo **nuevo proyecto** , expanda **instalado**, **plantillas**, **Visual C#** y, a continuación, **Web**.

   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Haga clic en la imagen para expandir                                                                |

4. Resalte la **aplicación Web ASP.net**, escriba **BookStore** como nombre del proyecto y, a continuación, haga clic en **Aceptar**.
5. Cuando se muestre el cuadro de diálogo **nuevo proyecto ASP.net** , seleccione la plantilla **API Web** y, a continuación, haga clic en **Aceptar**.

   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Haga clic en la imagen para expandir                                                                |

6. Cuando se abra el proyecto de API Web, quite el controlador de ejemplo del proyecto:

    1. Expanda la carpeta **Controladores** en el explorador de soluciones.
    2. Haga clic con el botón secundario en el archivo **ValuesController.CS** y, a continuación, haga clic en **eliminar**.
    3. Haga clic en **Aceptar** cuando se le pida que confirme la eliminación.
7. Agregar un archivo de datos XML al proyecto de API Web; este archivo incluye el contenido del catálogo de la librería:

   1. Haga clic con el botón secundario en la carpeta **App\_Data** del explorador de soluciones, haga clic en **Agregar**y, a continuación, haga clic en **nuevo elemento**.
   2. Cuando se muestre el cuadro de diálogo **Agregar nuevo elemento** , resalte la plantilla de **archivo XML** .
   3. Asigne al archivo el nombre **books. XML**y, a continuación, haga clic en **Agregar**.
   4. Cuando se abra el archivo **books. XML** , reemplace el código del archivo por el XML del archivo **books. XML** de ejemplo en MSDN: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. Guarde y cierre el archivo XML.

8. Agregue el modelo Bookstore al proyecto de API Web. Este modelo contiene la lógica de creación, lectura, actualización y eliminación (CRUD) de la aplicación Bookstore:

   1. Haga clic con el botón secundario en la carpeta **modelos** en el explorador de soluciones, haga clic en **Agregar**y, a continuación, en **clase**.
   2. Cuando se muestre el cuadro de diálogo **Agregar nuevo elemento** , asigne un nombre al archivo de clase **BookDetails.CS**y, a continuación, haga clic en **Agregar**.
   3. Cuando se abra el archivo **BookDetails.CS** , reemplace el código del archivo por lo siguiente: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. Guarde y cierre el archivo **BookDetails.CS** .

9. Agregue el controlador de la librería al proyecto de API Web:

   1. Haga clic con el botón secundario en la carpeta **Controllers** del explorador de soluciones, haga clic en **Agregar**y, a continuación, en **controlador**.
   2. Cuando aparezca el cuadro de diálogo **Agregar scaffold** , resalte el **controlador de Web API 2-vacío**y, a continuación, haga clic en **Agregar**.
   3. Cuando se muestre el cuadro de diálogo **Agregar controlador** , asigne al controlador el nombre **BooksController**y, a continuación, haga clic en **Agregar**.
   4. Cuando se abra el archivo **BooksController.CS** , reemplace el código del archivo por lo siguiente: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. Guarde y cierre el archivo **BooksController.CS** .

10. Cree la aplicación de API Web para comprobar si hay errores.

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a>Paso 2: agregar el proyecto de catálogo de la librería de Windows Phone 8

El siguiente paso de este escenario de un extremo a otro es crear la aplicación de catálogo para Windows Phone 8. Esta aplicación usará el Windows Phone plantilla de *aplicación de enlace* de datos para la interfaz de usuario predeterminada y usará la aplicación de API Web que creó en el [paso 1](#STEP1) de este tutorial como origen de datos.

1. Haga clic con el botón secundario en la solución **BookStore** en el explorador de soluciones, haga clic en **Agregar**y, a continuación, en **nuevo proyecto**.
2. Cuando se muestre el cuadro de diálogo **nuevo proyecto** , expanda **instalado**, luego **Visual C#** y, a continuación, **Windows Phone**.
3. Resalte **Windows Phone aplicación DataBound**, escriba **BookCatalog** para el nombre y, a continuación, haga clic en **Aceptar**.
4. Agregue el paquete NuGet Json.NET al proyecto **BookCatalog** :

    1. Haga clic con el botón derecho en **referencias** para el proyecto **BookCatalog** en el explorador de soluciones y, a continuación, haga clic en **administrar paquetes NuGet**.
    2. Cuando aparezca el cuadro de diálogo **administrar paquetes NuGet** , expanda la sección **en línea** y resalte **Nuget.org**.
    3. Escriba **JSON.net** en el campo de búsqueda y haga clic en el icono de búsqueda.
    4. Resalte **JSON.net** en los resultados de la búsqueda y, a continuación, haga clic en **instalar**.
    5. Una vez finalizada la instalación, haga clic en **cerrar**.
5. Agregue el modelo **BookDetails** al proyecto **BookCatalog** ; contiene un modelo genérico de la clase Bookstore:

   1. Haga clic con el botón secundario en el proyecto **BookCatalog** en el explorador de soluciones, haga clic en **Agregar**y, a continuación, en **nueva carpeta**.
   2. Asigne un nombre a los nuevos **modelos**de carpeta.
   3. Haga clic con el botón secundario en la carpeta **modelos** en el explorador de soluciones, haga clic en **Agregar**y, a continuación, en **clase**.
   4. Cuando se muestre el cuadro de diálogo **Agregar nuevo elemento** , asigne un nombre al archivo de clase **BookDetails.CS**y, a continuación, haga clic en **Agregar**.
   5. Cuando se abra el archivo **BookDetails.CS** , reemplace el código del archivo por lo siguiente: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. Guarde y cierre el archivo **BookDetails.CS** .

6. Actualice la clase **MainViewModel.CS** para incluir la funcionalidad para comunicarse con la aplicación de API Web de BookStore:

   1. Expanda la carpeta **ViewModels** en el explorador de soluciones y, a continuación, haga doble clic en el archivo **MainViewModel.CS** .
   2. Cuando se abra el archivo **MainViewModel.CS** , reemplace el código del archivo por lo siguiente: Tenga en cuenta que tendrá que actualizar el valor de la constante `apiUrl` con la dirección URL real de la API Web: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. Guarde y cierre el archivo **MainViewModel.CS** .

7. Actualice el archivo **mainpage. Xaml** para personalizar el nombre de la aplicación:

   1. Haga doble clic en el archivo **mainpage. Xaml** en el explorador de soluciones.
   2. Cuando se abra el archivo **mainpage. Xaml** , busque las siguientes líneas de código: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. Reemplace esas líneas por lo siguiente: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. Guarde y cierre el archivo **mainpage. Xaml** .

8. Actualice el archivo **DetailsPage. Xaml** para personalizar los elementos mostrados:

   1. Haga doble clic en el archivo **DetailsPage. Xaml** en el explorador de soluciones.
   2. Cuando se abra el archivo **DetailsPage. Xaml** , busque las siguientes líneas de código: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. Reemplace esas líneas por lo siguiente: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. Guarde y cierre el archivo **DetailsPage. Xaml** .

9. Compile la aplicación de Windows Phone para comprobar si hay errores.

### <a name="step-3-testing-the-end-to-end-solution"></a>Paso 3: probar la solución de un extremo a otro

Como se mencionó en la sección de *requisitos previos* de este tutorial, al probar la conectividad entre la API Web y los proyectos de Windows Phone 8 en el sistema local, deberá seguir las instrucciones del artículo *[conexión del emulador de Windows Phone 8 a aplicaciones de API Web en un equipo local](https://go.microsoft.com/fwlink/?LinkId=324014)* para configurar el entorno de pruebas.

Una vez configurado el entorno de pruebas, tendrá que establecer la aplicación Windows Phone como el proyecto de inicio. Para ello, resalte la aplicación **BookCatalog** en el explorador de soluciones y, a continuación, haga clic en **establecer como proyecto de inicio**:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| Haga clic en la imagen para expandir |

Al presionar F5, Visual Studio iniciará el emulador de Windows Phone, que mostrará una &quot;espere&quot; mensaje mientras se recuperan los datos de la aplicación de la API Web:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| Haga clic en la imagen para expandir |

Si todo se realiza correctamente, debería ver el catálogo mostrado:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| Haga clic en la imagen para expandir |

Si puntea en cualquier título de libro, la aplicación mostrará la descripción del libro:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| Haga clic en la imagen para expandir |

Si la aplicación no puede comunicarse con la API Web, se mostrará un mensaje de error:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| Haga clic en la imagen para expandir |

Si puntea en el mensaje de error, se mostrarán los detalles adicionales sobre el error:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 Haga clic en la imagen para expandir                                                                 |
