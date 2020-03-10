---
uid: whitepapers/aspnet-and-iis6
title: Ejecución de ASP.NET 1,1 con IIS 6,0 | Microsoft Docs
author: rick-anderson
description: Aunque Windows Server 2003 incluye tanto IIS 6,0 como ASP.NET 1,1, estos componentes están deshabilitados de forma predeterminada. En estas notas del producto se describe cómo habilitar IIS 6,0...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 2e7812f34481afe9a71927c0d9ba2a9abc9632e4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419851"
---
# <a name="running-aspnet-11-with-iis-60"></a>Ejecutar ASP.NET 1.1 con IIS 6.0

> Aunque Windows Server 2003 incluye tanto IIS 6,0 como ASP.NET 1,1, estos componentes están deshabilitados de forma predeterminada. En estas notas del producto se describe cómo habilitar IIS 6,0 y ASP.NET 1,1, y se recomiendan varias opciones de configuración para obtener el rendimiento óptimo de IIS y ASP.NET.
> 
> Se aplica a ASP.NET 1,1 e IIS 6,0.

ASP.NET 1,1 se incluye con Windows Server 2003, que también incluye la versión más reciente de Internet Information Server (IIS) versión 6,0. IIS 6,0 y ASP.NET 1,1 están diseñados para integrarse sin problemas y ASP.NET ahora tiene como valor predeterminado el nuevo modelo de proceso de trabajo de IIS 6,0.

## <a name="aspnet-11-is-not-installed-by-default"></a>ASP.NET 1,1 no está instalado de forma predeterminada

A diferencia de las versiones anteriores de los sistemas operativos de servidor de Microsoft, Internet Information Server (IIS) no está habilitado de forma predeterminada. ni es ASP.NET 1,1. Hay dos opciones para habilitar IIS:

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a>Habilitar IIS, opción #1: Asistente para configurar el servidor

Windows Server 2003 envía un nuevo "Asistente para configurar el servidor" para ayudarle a configurar correctamente el servidor en el modo deseado.

Para iniciar el asistente: tenga en cuenta que, para ejecutar el asistente, debe haber iniciado sesión como administrador. vaya a: Inicio | Programas | Herramientas administrativas y seleccione "configurar el servidor".

Una vez seleccionado, debería ver la pantalla de apertura del Asistente para configurar el servidor:

![](aspnet-and-iis6/_static/image1.jpg)

Haga clic en "siguiente &gt;":

![](aspnet-and-iis6/_static/image2.jpg)

Haga clic en ' siguiente &gt;'

![](aspnet-and-iis6/_static/image3.jpg)

En esta pantalla deberá seleccionar ' servidor de aplicaciones (IIS, ASP.NET) ' como opciones para configurar.

Haga clic en "siguiente &gt;".

![](aspnet-and-iis6/_static/image4.jpg)

Después de seleccionar la opción para configurar el servidor como un servidor de aplicaciones, se mostrará esta pantalla en la que se le preguntará qué capacidades adicionales deben instalarse. Ninguna opción está seleccionada de forma predeterminada. Para habilitar ASP.NET automáticamente, debe seleccionar "habilitar ASP". NET '.

Haga clic en "siguiente &gt;".

![](aspnet-and-iis6/_static/image5.jpg)

Esta pantalla muestra las opciones que se van a instalar.

Haga clic en "siguiente &gt;".

![](aspnet-and-iis6/_static/image6.jpg)

Verá esta pantalla mientras se instalan las opciones seleccionadas. Es normal ver otros cuadros de diálogo que aparecen cuando se instalan los servicios. Además, es posible que se le solicite la ubicación del CD de instalación de Windows 2003 Server.

Cuando termine, haga clic en "siguiente &gt;".

![](aspnet-and-iis6/_static/image7.jpg)

Haga clic en "finalizar": ahora, Windows Server 2003 está configurado para admitir IIS 6,0 y ASP.NET 1,1.

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a>Habilitar IIS, opción #2: configurar manualmente IIS y ASP.NET

Si no desea usar el Asistente para configurar el servidor, puede instalar IIS 6,0 y ASP.NET 1,1 mediante la opción ' Agregar o quitar programas ' del panel de control.

En primer lugar, abra el panel de control:

![](aspnet-and-iis6/_static/image8.jpg)

A continuación, haga clic en "Agregar o quitar componentes de Windows", que abrirá el "Asistente para componentes de Windows":

![](aspnet-and-iis6/_static/image9.jpg)

Resalte y seleccione "servidor de aplicaciones" y, a continuación, haga clic en "detalles". botón

![](aspnet-and-iis6/_static/image10.jpg)

Para instalar ASP.NET, Active "ASP. NET '.

Haga clic en "Aceptar" para volver al Asistente para componentes de Windows. Haga clic en "siguiente &gt;" en el Asistente para componentes de Windows para empezar a instalar:

![](aspnet-and-iis6/_static/image11.jpg)

Es normal ver otros cuadros de diálogo que aparecen cuando se instalan los servicios. Además, es posible que se le solicite la ubicación del CD de instalación de Windows 2003 Server.

Una vez completada la instalación, verá la última pantalla del Asistente para componentes de Windows:

![](aspnet-and-iis6/_static/image12.jpg)

IIS 6,0 y ASP.NET 1,1 ahora están configurados y disponibles.

## <a name="recommended-settings"></a>Configuración recomendada

Al ejecutar ASP.NET 1,1 con IIS 6,0 hay varias opciones de configuración que se recomiendan para obtener el rendimiento óptimo de ASP.NET:

- Configuración de límites de memoria de procesos de trabajo
- Configurar el reciclaje de procesos de trabajo

### <a name="configuring-worker-process-memory-limits"></a>Configuración de límites de memoria de procesos de trabajo

De forma predeterminada, IIS 6,0 no establece un límite en la cantidad de memoria que puede usar IIS. ASP. La característica de caché de la red se basa en una limitación de memoria, por lo que la memoria caché puede quitar de forma proactiva los elementos no utilizados de la memoria.

Se recomienda que configure la característica reciclaje de memoria de IIS 6,0. Para configurar este administrador de Internet Information Services abierto (Inicio | Programas | Herramientas administrativas | Internet Information Services). Una vez abierta, expanda la carpeta ' grupos de aplicaciones ':

Para cada grupo de aplicaciones:

![](aspnet-and-iis6/_static/image13.jpg)

1. Haga clic con el botón derecho en el grupo de aplicaciones, por ejemplo, "DefaultAppPool" y seleccione "propiedades":

![](aspnet-and-iis6/_static/image14.jpg)

2. A continuación, habilite el reciclaje de memoria; para ello, haga clic en ' máximo de memoria usada (en megabytes): '. El valor no debe ser mayor que la cantidad de memoria física (no virtual) del servidor, una buena aproximación es el 60% de la memoria física, es decir, para un servidor con 512 MB de memoria física, seleccione 310. También se recomienda que el máximo no supere el valor de 800MB al usar un espacio de direcciones de 2 GB. Si el espacio de direcciones de memoria del servidor es de 3 GB, el límite máximo de memoria para el proceso de trabajo puede ser tan alto como 1, 800MB:

![](aspnet-and-iis6/_static/image15.jpg)

Haga clic en "aplicar" y en "Aceptar" para salir del cuadro de diálogo Propiedades. Repita este paso para todos los grupos de aplicaciones disponibles.

### <a name="configuring-worker-recycling"></a>Configuración del reciclaje de trabajo

De forma predeterminada, IIS 6,0 se configura para reciclar el proceso de trabajo cada 29 horas. Esto es un poco agresivo para una aplicación que ejecuta ASP.NET y se recomienda que el reciclaje automático del proceso de trabajo esté deshabilitado.

Para deshabilitar el reciclaje automático de procesos de trabajo, abra primero Internet Information Services Manager (Start | Programas | Herramientas administrativas | Internet Information Services). Una vez abierta, expanda la carpeta ' grupos de aplicaciones ':

![](aspnet-and-iis6/_static/image16.jpg)

Para cada grupo de aplicaciones:

1. Haga clic con el botón derecho en el grupo de aplicaciones, por ejemplo, "DefaultAppPool" y seleccione "propiedades":

![](aspnet-and-iis6/_static/image17.jpg)

2. Desactive la casilla ' reciclar el proceso de trabajo (en minutos): ':

![](aspnet-and-iis6/_static/image18.jpg)

Haga clic en "aplicar" y en "Aceptar" para salir del cuadro de diálogo Propiedades. Repita este paso para todos los grupos de aplicaciones disponibles.

## <a name="granting-write-access-to-the-file-system"></a>Conceder acceso de escritura al sistema de archivos

Si la aplicación necesita acceso de escritura al sistema de archivos y usa NTFS, tendrá que modificar una lista de Access Control (ACL) en la carpeta o el archivo para conceder acceso a ASP.NET a.

Por ejemplo, para conceder acceso de escritura de ASP.NET a c:\inetpub\wwwroot, abra primero el explorador y navegue hasta el directorio:

![](aspnet-and-iis6/_static/image19.jpg)

A continuación, haga clic con el botón derecho en el directorio, por ejemplo, ' wwwroot ' y seleccione Propiedades. Después de abrir el cuadro de diálogo Propiedades, seleccione la pestaña ' seguridad ':

![](aspnet-and-iis6/_static/image20.jpg)

El directorio c:\inetpub\wwwroot\ es un directorio especial en el que el grupo especial IIS 6,0 ' IIS\_WPG ' ya tiene concedido el permiso leer &amp; ejecutar, mostrar el contenido de la carpeta y leer. Sin embargo, para conceder permiso de escritura, debe hacer clic en la casilla permitir para escritura:

![](aspnet-and-iis6/_static/image21.jpg)

IIS 6,0 ahora tiene permiso de escritura en esta carpeta. Para conceder permisos de escritura en otras carpetas, siga estos pasos: tenga en cuenta que es posible que tenga que agregar el grupo de IIS\_WPG si aún no existe.

> [!CAUTION]
> Conceder permiso de escritura a IIS\_WPG permitirá que cualquier aplicación ASP.NET escriba en este directorio.

## <a name="supporting-integrated-authentication-with-sql-server"></a>Compatibilidad con la autenticación integrada con SQL Server

La autenticación integrada permite a SQL Server aprovechar la autenticación de Windows NT para validar SQL Server cuentas de inicio de sesión. Esto permite al usuario omitir el proceso de inicio de sesión de SQL Server estándar. Con este enfoque, un usuario de red puede tener acceso a una base de datos de SQL Server sin proporcionar una identificación de inicio de sesión independiente o una contraseña porque SQL Server obtiene la información de usuario y contraseña del proceso de seguridad de red de Windows NT.

La elección de la autenticación integrada para aplicaciones ASP.NET es una buena opción, ya que ninguna de las credenciales se almacenan en la cadena de conexión de la aplicación. En su lugar, la cadena de conexión utilizada para conectarse a SQL tendrá el siguiente aspecto:

`"server=localhost; database=Northwind;Trusted_Connection=true"`

Esta cadena de conexión indica a SQL Server que use las credenciales de Windows de la aplicación que intenta tener acceso a SQL Server. En el caso de ASP.NET/IIS 6, sería una cuenta en el grupo de IIS\_WPG.

Para habilitar la autenticación integrada entre SQL Server y ASP.NET, primero deberá asegurarse de que SQL Server está configurado para la autenticación integrada o la autenticación de modo mixto. Consulte con su DBA para determinar esto. Si SQL Server está en uno de estos dos modos, puede utilizar la autenticación integrada.

Abra el administrador de SQL Server Enterprise (Inicio | Programas | Microsoft SQL Server | Administrador corporativo), seleccione el servidor adecuado y expanda la carpeta seguridad:

![](aspnet-and-iis6/_static/image22.jpg)

Si no aparece el grupo ' BUILTINT\IIS\_WPG ', haga clic con el botón derecho en inicios de sesión y seleccione ' nuevo login ':

![](aspnet-and-iis6/_static/image23.jpg)

En el cuadro de texto "nombre:", escriba "[nombre del servidor/dominio] \IIS\_WPG" o haga clic en el botón de puntos suspensivos para abrir el selector de usuarios o grupos de Windows NT:

![](aspnet-and-iis6/_static/image24.jpg)

Seleccione el grupo de IIS\_WPG del equipo actual y haga clic en "agregar" y en Aceptar para cerrar el selector.

A continuación, debe establecer también la base de datos predeterminada y los permisos para tener acceso a la base de datos. Para establecer la base de datos predeterminada, elija en la lista desplegable; por ejemplo, se selecciona Northwind:

![](aspnet-and-iis6/_static/image25.jpg)

A continuación, haga clic en la pestaña acceso a base de datos:

![](aspnet-and-iis6/_static/image26.jpg)

Haga clic en la casilla permitir para cada base de datos a la que desea permitir el acceso. También tendrá que seleccionar roles de base de datos, comprobando que el propietario de la base de datos\_debe asegurarse de que el inicio de sesión tenga todos los permisos necesarios para administrar y usar la base de datos seleccionada.

Haga clic en Aceptar para salir del cuadro de diálogo de propiedades. La aplicación ASP.NET ya está configurada para admitir la autenticación integrada de SQL Server.

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a>No ejecute ASP.NET 1,0 en el modo nativo de IIS 6,0

ASP.NET 1,0 en IIS 6,0 solo se admite en el modo de compatibilidad de IIS 5.

Para configurar ASP.NET 1,0 para que se ejecute en el modo de compatibilidad de IIS 5,0, abra Administrador de servicios Internet, haga clic con el botón derecho en sitios web y seleccione Propiedades:

![](aspnet-and-iis6/_static/image27.jpg)

Cambie a la pestaña servicio y seleccione? ¿Ejecutar el servicio WWW en el modo de aislamiento de IIS 5,0?:

![](aspnet-and-iis6/_static/image28.jpg)
