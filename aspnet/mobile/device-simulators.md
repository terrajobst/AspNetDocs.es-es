---
uid: mobile/device-simulators
title: Simular dispositivos móviles conocidos para realizar pruebas con ASP.NET | Microsoft Docs
author: rick-anderson
description: Descargar emuladores para exploradores y dispositivos móviles más conocidos para probar con la aplicación ASP.NET. Incluye el iPhone, Android, BrowserStack y mucho más.
ms.author: riande
ms.date: 10/11/2018
ms.custom: seoapril2019
ms.assetid: bfb5612e-c3ec-4f28-b43b-63d781aa2272
msc.legacyurl: /mobile/device-simulators
msc.type: content
ms.openlocfilehash: aec442e05a7db69dfaea4b0cca53bbf41792500c
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59412156"
---
# <a name="simulate-popular-mobile-devices-for-testing"></a>Simular dispositivos móviles conocidos para efectuar pruebas

> Puede descargar emuladores para exploradores y dispositivos móviles más conocidos, siga estos vínculos.

| Dispositivo o explorador | Emulador o simulador |
| --- | --- |
| BrowserStack hospedado virtualización de explorador ![BrowserStack hospedado virtualización de explorador](device-simulators/_static/image1.png) | [Virtualización de BrowserStack hospedada en explorador](http://browserstack.com) probar su entorno local o de producción en cualquier explorador en cualquier plataforma. Puede crear un túnel entre su equipo y la red BrowserStack en su propia máquina virtual hospedada. Asegúrese de obtener el [extensión de Visual Studio BrowserStack](https://marketplace.visualstudio.com/items?itemName=browserstackcom.BrowserStack) para una experiencia incluso más sencilla. |
| iPhone / iPod / iPad | [Electric Studio móvil](http://www.electricplum.com/studio.aspx) iPhone y iPad simuladores para Windows, así como una capacidad de respuesta de herramienta de diseño. |
| Android | [Android Studio](https://developer.android.com/studio/) o [emulador de Visual Studio para Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/) |
| Opera Mobile | [Emulador de opera Mobile clásico](https://www.opera.com/developer/mobile-emulator) |
| Windows Mobile 6.5.3 | [Kit de herramientas para desarrolladores de Windows Mobile 6.5.3](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=c0213f68-2e01-4e5c-a8b2-35e081dcf1ca&amp;displaylang=en) tenga en cuenta que para conceder el acceso a la red de teléfono, también tiene el adaptador de red de VPC incluido en [Virtual PC 2007](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=04d26402-3199-48a3-afa2-2dc0b40a73b6&amp;DisplayLang=en). Para conectarse a Internet Explorer en el teléfono a su servidor de desarrollo de Visual Studio, consulte [entrada de blog de Kiran Patil](http://kiranpatils.wordpress.com/2009/11/19/access-internetlocal-website-from-your-windows-mobile-device-emulators/). |

> [!NOTE]
> Si desea ver la aplicación en un dispositivo móvil real (que es la única opción para realizar las pruebas completas iPhone o iPad, ya que no hay ningún verdadero emulador para Windows) necesita hospedar su aplicación en IIS o IIS Express. No se puede usar fácilmente servidor web integrado de Visual Studio para ello, ya que no responde a solicitudes de otros equipos.
