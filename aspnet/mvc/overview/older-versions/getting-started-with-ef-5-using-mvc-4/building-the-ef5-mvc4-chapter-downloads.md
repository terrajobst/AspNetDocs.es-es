---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Compilación del capítulo descargas para los tutoriales de EF 5 MVC 4 | Microsoft Docs
author: Rick-Anderson
description: En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones ASP.NET MVC 4 con el Code First de Entity Framework 5 y Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: 10237af40e3914b65e5181f17555697e86adea4b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78485125"
---
# <a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a>Compilación del capítulo descargas para los tutoriales de EF 5 MVC 4

por [Rick Anderson](https://twitter.com/RickAndMSFT)

[Descargar proyecto completado](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones ASP.NET MVC 4 con el Code First de Entity Framework 5 y Visual Studio 2012. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

## <a name="building-the-chapter-downloads"></a>Crear las descargas de capítulos

1. Descargue y descomprima el archivo zip de ejemplo del proyecto. En el paquete de descarga descomprimido, encontrará archivos zip adicionales, uno para la realización de cada capítulo.
2. Haga clic con el botón derecho en el archivo zip deseado, haga clic en **propiedades**y, a continuación, haga clic en el botón **desbloquear** .  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. Descomprima el archivo.
4. Haga doble clic en el archivo *CUx. sln* para iniciar Visual Studio.
5. En el menú **herramientas** , haga clic en **Administrador de paquetes NuGet**y luego en **consola del administrador de paquetes**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. En la consola del administrador de paquetes (PMC), haga clic en **restaurar**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. Salga de Visual Studio.
8. Reinicie Visual Studio y abra el archivo de solución que cerró en el paso anterior.
9. En la consola del administrador de paquetes (PMC), escriba el comando `Update-Database`:  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > Si recibe el siguiente error:  
    >   
    >  *El término ' Update-Database ' no se reconoce como nombre de un cmdlet, función, archivo de script o programa ejecutable. Compruebe la ortografía del nombre, o bien, si se incluyó una ruta de acceso, compruebe que la ruta de acceso es correcta y vuelva a intentarlo.*  
    > Salga y reinicie Visual Studio.

    Cada migración se ejecutará y, a continuación, se ejecutará el método de inicialización. Ahora puede ejecutar la aplicación.

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [Anterior](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
