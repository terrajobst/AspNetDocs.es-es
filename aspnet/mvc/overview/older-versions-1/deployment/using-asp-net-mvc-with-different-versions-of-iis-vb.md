---
uid: mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-vb
title: Usar ASP.NET MVC con distintas versiones de IIS (VB) | Microsoft Docs
author: microsoft
description: En este tutorial, aprenderá a usar ASP.NET MVC y el enrutamiento de direcciones URL con diferentes versiones de Internet Information Services. Aprenderá distintas estrategias...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 1c1283b2-6956-4937-b568-d30de432ce23
msc.legacyurl: /mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-vb
msc.type: authoredcontent
ms.openlocfilehash: b754175c853c20eec6be3521376b62d62f33106d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78470017"
---
# <a name="using-aspnet-mvc-with-different-versions-of-iis-vb"></a>Usar ASP.NET MVC con distintas versiones de IIS (VB)

por [Microsoft](https://github.com/microsoft)

> En este tutorial, aprenderá a usar ASP.NET MVC y el enrutamiento de direcciones URL con diferentes versiones de Internet Information Services. Aprenderá diferentes estrategias para usar ASP.NET MVC con IIS 7,0 (modo clásico), IIS 6,0 y versiones anteriores de IIS.

El marco de MVC de ASP.NET depende del enrutamiento de ASP.NET para enrutar las solicitudes del explorador a las acciones de controlador. Con el fin de aprovechar el enrutamiento de ASP.NET, es posible que tenga que realizar pasos de configuración adicionales en el servidor Web. Todo depende de la versión de Internet Information Services (IIS) y el modo de procesamiento de solicitudes de la aplicación.

A continuación se muestra un resumen de las distintas versiones de IIS:

- IIS 7,0 (modo integrado): no se necesita ninguna configuración especial para usar el enrutamiento ASP.NET.
- IIS 7,0 (modo clásico): debe realizar una configuración especial para usar el enrutamiento ASP.NET.
- IIS 6,0 o inferior: debe realizar una configuración especial para usar el enrutamiento ASP.NET.

La versión más reciente de IIS es la versión 7,5 (en Win7). IIS 7 de IIS se incluye con Windows Server 2008 y VISTA/SP1 y versiones posteriores. También puede instalar IIS 7,0 en cualquier versión del sistema operativo de vista excepto Home Basic (consulte [https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx](https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx)).

IIS 7,0 admite dos modos de procesamiento de solicitudes. Puede usar el modo integrado o el modo clásico. No es necesario realizar ningún paso de configuración especial al usar IIS 7,0 en el modo integrado. Sin embargo, es necesario realizar una configuración adicional cuando se usa IIS 7,0 en modo clásico.

Microsoft Windows Server 2003 incluye IIS 6,0. No se puede actualizar IIS 6,0 a IIS 7,0 cuando se usa el sistema operativo Windows Server 2003. Debe realizar pasos de configuración adicionales al usar IIS 6,0.

Microsoft Windows XP Professional incluye IIS 5,1. Debe realizar pasos de configuración adicionales al usar IIS 5,1.

Por último, Microsoft Windows 2000 y Microsoft Windows 2000 Professional incluyen IIS 5,0. Debe realizar pasos de configuración adicionales al usar IIS 5,0.

## <a name="integrated-versus-classic-mode"></a>Modo integrado frente al modo clásico

IIS 7,0 puede procesar solicitudes con dos modos de procesamiento de solicitudes diferentes: integrado y clásico. El modo integrado proporciona un mejor rendimiento y más características. El modo clásico se incluye para la compatibilidad con versiones anteriores de IIS.

El modo de procesamiento de solicitudes viene determinado por el grupo de aplicaciones. Puede determinar qué modo de procesamiento utiliza una aplicación web determinada determinando el grupo de aplicaciones asociado a la aplicación. Siga estos pasos:

1. Iniciar el administrador de Internet Information Services
2. En la ventana conexiones, seleccione una aplicación.
3. En la ventana acciones, haga clic en el vínculo **configuración básica** para abrir el cuadro de diálogo Editar aplicación (vea la ilustración 1).
4. Tome nota del grupo de aplicaciones seleccionado.

De forma predeterminada, IIS está configurado para admitir dos grupos de aplicaciones: **DefaultAppPool** y **clásico .net AppPool**. Si DefaultAppPool está seleccionado, la aplicación se ejecuta en modo de procesamiento de solicitudes integrado. Si se selecciona clásico .NET AppPool, la aplicación se ejecuta en modo de procesamiento de solicitudes clásico.

[![el cuadro de diálogo nuevo proyecto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.png)

**Figura 1**: detección del modo de procesamiento de solicitudes ([haga clic para ver la imagen de tamaño completo](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.png))

Tenga en cuenta que puede modificar el modo de procesamiento de solicitudes en el cuadro de diálogo Editar aplicación. Haga clic en el botón seleccionar y cambie el grupo de aplicaciones asociado a la aplicación. Observe que hay problemas de compatibilidad al cambiar una aplicación de ASP.NET del modo clásico al modo integrado. Para obtener más información, vea los artículos siguientes:

- Actualización de ASP.NET 1,1 a IIS 7,0 en Windows Vista y Windows Server 2008: [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)

- Integración de ASP.NET con IIS 7,0- [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)

Si una aplicación de ASP.NET usa DefaultAppPool, no es necesario realizar ningún paso adicional para que el enrutamiento de ASP.NET (y, por lo tanto, ASP.NET MVC) funcione. Sin embargo, si la aplicación ASP.NET está configurada para usar el .NET AppPool clásico, siga leyendo y tendrá más trabajo que hacer.

## <a name="using-aspnet-mvc-with-older-versions-of-iis"></a>Usar ASP.NET MVC con versiones anteriores de IIS

Si necesita usar ASP.NET MVC con una versión anterior de IIS que IIS 7,0 o necesita usar IIS 7,0 en modo clásico, tiene dos opciones. En primer lugar, puede modificar la tabla de rutas para usar las extensiones de archivo. Por ejemplo, en lugar de solicitar una dirección URL como/Store/Details, solicitaría una dirección URL como/Store.aspx/Details.

La segunda opción consiste en crear algo denominado *mapa de script comodín*. Una asignación de script comodín permite asignar cada solicitud al marco ASP.NET.

Si no tiene acceso al servidor Web (por ejemplo, la aplicación ASP.NET MVC se hospeda en un proveedor de servicios de Internet), deberá usar la primera opción. Si no desea modificar la apariencia de las direcciones URL y tiene acceso al servidor Web, puede usar la segunda opción.

En las secciones siguientes se explora cada opción en detalle.

## <a name="adding-extensions-to-the-route-table"></a>Agregar extensiones a la tabla de rutas

La forma más fácil de conseguir que el enrutamiento de ASP.NET funcione con versiones anteriores de IIS es modificar la tabla de rutas en el archivo global. asax. El archivo global. asax predeterminado y sin modificar de la lista 1 configura una ruta denominada ruta predeterminada.

**Lista 1: global. asax (sin modificar)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample1.vb)]

La ruta predeterminada configurada en la lista 1 le permite enrutar las direcciones URL que tienen el siguiente aspecto:

/Home/Index

/Product/Details/3

/Product

Desafortunadamente, las versiones anteriores de IIS no pasarán estas solicitudes al marco ASP.NET. Por lo tanto, estas solicitudes no se enrutarán a un controlador. Por ejemplo, si realiza una solicitud de explorador para la dirección URL/Home/Index, obtendrá la página de error en la figura 2.

[![el cuadro de diálogo nuevo proyecto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.png)

**Figura 2**: recepción de un error 404 no encontrado ([haga clic para ver la imagen de tamaño completo](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.png))

Las versiones anteriores de IIS solo asignan determinadas solicitudes al marco ASP.NET. La solicitud debe ser para una dirección URL con la extensión de archivo correcta. Por ejemplo, una solicitud de/SomePage.aspx se asigna al marco de ASP.NET. Sin embargo, una solicitud para/SomePage.htm no lo hace.

Por lo tanto, para que el enrutamiento de ASP.NET funcione, es necesario modificar la ruta predeterminada para que incluya una extensión de archivo que esté asignada al marco de ASP.NET.

Esto se hace mediante un script denominado `registermvc.wsf`. Se incluyó con la versión ASP.NET MVC 1 en `C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`, pero a partir de ASP.NET 2 este script se ha pasado a los futuros ASP.NET, disponible en [http://aspnet.codeplex.com/releases/view/39978](http://aspnet.codeplex.com/releases/view/39978).

Al ejecutar este script, se registra una nueva extensión. MVC con IIS. Después de registrar la extensión. MVC, puede modificar las rutas en el archivo global. asax para que las rutas usen la extensión. Mvc.

El archivo global. asax modificado de la lista 2 funciona con versiones anteriores de IIS.

**Lista 2-global. asax (modificado con extensiones)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample2.vb)]

Importante: Recuerde volver a compilar la aplicación ASP.NET MVC después de cambiar el archivo global. asax.

Hay dos cambios importantes en el archivo global. asax en la lista 2. Ahora hay dos rutas definidas en global. asax. El patrón de dirección URL para la ruta predeterminada, la primera ruta, ahora tiene el siguiente aspecto:

{controller}.mvc/{action}/{id}

La adición de la extensión. Mvc cambia el tipo de archivos que intercepta el módulo de enrutamiento ASP.NET. Con este cambio, la aplicación ASP.NET MVC ahora enruta solicitudes como las siguientes:

/Home.mvc/Index/

/Product.mvc/Details/3

/Product.mvc/

La segunda ruta, la ruta raíz, es nueva. Este patrón de dirección URL para la ruta raíz es una cadena vacía. Esta ruta es necesaria para hacer coincidir las solicitudes realizadas con la raíz de la aplicación. Por ejemplo, la ruta raíz coincidirá con una solicitud similar a la siguiente:

[http://www.YourApplication.com/](http://www.YourApplication.com/)

Después de realizar estas modificaciones en la tabla de rutas, deberá asegurarse de que todos los vínculos de la aplicación sean compatibles con estos nuevos patrones de direcciones URL. En otras palabras, asegúrese de que todos los vínculos incluyen la extensión. Mvc. Si usa el método auxiliar HTML. ActionLink () para generar los vínculos, no debería ser necesario realizar ningún cambio.

En lugar de usar el script registermvc. WCF, puede Agregar una nueva extensión a IIS que esté asignada a ASP.NET Framework manualmente. Cuando agregue una nueva extensión, asegúrese de que la casilla **comprobar que el archivo existe** no está activada.

## <a name="hosted-server"></a>Servidor hospedado

No siempre tiene acceso al servidor Web. Por ejemplo, si hospeda la aplicación ASP.NET MVC mediante un proveedor de hospedaje de Internet, no tendrá necesariamente acceso a IIS.

En ese caso, debe usar una de las extensiones de archivo existentes que se asignan al marco ASP.NET. Entre los ejemplos de extensiones de archivo asignadas a ASP.NET se incluyen las extensiones. aspx,. axd y. ashx.

Por ejemplo, el archivo global. asax modificado de la lista 3 utiliza la extensión. aspx en lugar de la extensión. Mvc.

**Lista 3-global. asax (modificado con extensiones. aspx)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample3.vb)]

El archivo global. asax de la lista 3 es exactamente el mismo que el archivo global. asax anterior, excepto por el hecho de que usa la extensión. aspx en lugar de la extensión. Mvc. No es necesario realizar ninguna configuración en el servidor Web remoto para utilizar la extensión. aspx.

## <a name="creating-a-wildcard-script-map"></a>Creación de una asignación de script comodín

Si no desea modificar las direcciones URL de la aplicación ASP.NET MVC y tiene acceso al servidor Web, tiene una opción adicional. Puede crear una asignación de script comodín que asigne todas las solicitudes al servidor Web al marco ASP.NET. De este modo, puede usar la tabla de ruta predeterminada de ASP.NET MVC con IIS 7,0 (en modo clásico) o IIS 6,0.

Tenga en cuenta que esta opción hace que IIS intercepte todas las solicitudes realizadas en el servidor Web. Esto incluye las solicitudes de imágenes, páginas ASP clásicas y páginas HTML. Por lo tanto, la habilitación de una asignación de script comodín a ASP.NET tiene implicaciones de rendimiento.

Aquí se muestra cómo habilitar una asignación de script comodín para IIS 7,0:

1. Seleccione la aplicación en la ventana conexiones
2. Asegúrese de que está seleccionada la vista **características** .
3. Haga doble clic en el botón **asignaciones de controlador**
4. Haga clic en el vínculo **Agregar asignación de script comodín** (vea la figura 3).
5. Escriba la ruta de acceso al archivo Aspnet\_ISAPI. dll (puede copiar esta ruta de acceso desde la asignación de script PageHandlerFactory)
6. Escriba el nombre MVC
7. Haga clic en el botón **Aceptar**

[![el cuadro de diálogo nuevo proyecto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.png)

**Figura 3**: creación de una asignación de script comodín con IIS 7,0 ([haga clic para ver la imagen de tamaño completo](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image6.png))

Siga estos pasos para crear un mapa de scripts comodín con IIS 6,0:

1. Haga clic con el botón secundario en un sitio web y seleccione Propiedades.
2. Seleccione la pestaña **directorio principal** .
3. Haga clic en el botón **configuración**
4. Seleccione la pestaña **asignaciones**
5. Haga clic en el botón **Insertar** (consulte la figura 4).
6. Pegue la ruta de acceso al archivo Aspnet\_ISAPI. dll en el campo ejecutable (puede copiar esta ruta de acceso de la asignación de scripts para los archivos. aspx).
7. Desactive la casilla **comprobar que el archivo existe**
8. Haga clic en el botón **Aceptar**

[![el cuadro de diálogo nuevo proyecto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image7.png)

**Figura 4**: creación de una asignación de script comodín con IIS 6,0 ([haga clic para ver la imagen de tamaño completo](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image8.png))

Después de habilitar las asignaciones de scripts comodín, debe modificar la tabla de rutas en el archivo global. asax para que incluya una ruta raíz. De lo contrario, obtendrá la página de error en la figura 5 cuando realice una solicitud para la página raíz de la aplicación. Puede usar el archivo global. asax modificado en la lista 4.

[![el cuadro de diálogo nuevo proyecto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image9.png)

**Figura 5**: falta un error en la ruta raíz ([haga clic para ver la imagen de tamaño completo](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image10.png))

**Lista 4: global. asax (modificado con ruta raíz)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample4.vb)]

Después de habilitar una asignación de script comodín para IIS 7,0 o IIS 6,0, puede hacer que las solicitudes que funcionan con la tabla de rutas predeterminada tengan el aspecto siguiente:

/

/Home/Index

/Product/Details/3

/Product

## <a name="summary"></a>Resumen

El objetivo de este tutorial era explicar cómo se puede usar ASP.NET MVC cuando se usa una versión anterior de IIS (o IIS 7,0 en modo clásico). Analizamos dos métodos para que el enrutamiento de ASP.NET funcione con versiones anteriores de IIS: modificar la tabla de rutas predeterminada o crear una asignación de script comodín.

La primera opción requiere que modifique las direcciones URL usadas en la aplicación ASP.NET MVC. Una ventaja muy importante de esta primera opción es que no necesita acceso a un servidor web para modificar la tabla de rutas. Esto significa que puede usar esta primera opción incluso al hospedar su aplicación ASP.NET MVC con una empresa de hospedaje de Internet.

La segunda opción consiste en crear una asignación de script comodín. La ventaja de esta segunda opción es que no es necesario modificar las direcciones URL. El inconveniente de esta segunda opción es que puede afectar al rendimiento de la aplicación ASP.NET MVC.

> [!div class="step-by-step"]
> [Anterior](using-asp-net-mvc-with-different-versions-of-iis-cs.md)
