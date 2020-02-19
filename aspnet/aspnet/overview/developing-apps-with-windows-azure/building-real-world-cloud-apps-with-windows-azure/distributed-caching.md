---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: Almacenamiento en caché distribuido (creación de aplicaciones en la nube reales con Azure) | Microsoft Docs
author: MikeWasson
description: La creación de aplicaciones reales en la nube con el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden...
ms.author: riande
ms.date: 07/20/2015
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: 87a7516415895e761d1589fd459b93e5c15c0f85
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457003"
---
# <a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a>Almacenamiento en caché distribuido (creación de aplicaciones en la nube reales con Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de corrección de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar el libro electrónico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **creación de aplicaciones reales en la nube con** el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden ayudarle a desarrollar correctamente aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).

En el capítulo anterior se buscó el control de errores transitorios y el almacenamiento en caché mencionado como una estrategia de disyuntor. Este capítulo proporciona más información sobre el almacenamiento en caché, lo que incluye Cuándo usarlo, los patrones comunes para su uso y cómo implementarlo en Azure.

## <a name="what-is-distributed-caching"></a>Qué es el almacenamiento en caché distribuido

Una memoria caché proporciona acceso de baja latencia y alto rendimiento a los datos de la aplicación a los que se accede con frecuencia almacenando los datos en la memoria. En el caso de una aplicación en la nube, el tipo de caché más útil es la caché distribuida, lo que significa que los datos no se almacenan en la memoria del servidor Web individual sino en otros recursos en la nube, y los datos almacenados en caché se ponen a disposición de todos los servidores Web de la aplicación (u otras máquinas virtuales de la nube que sean ar e usado por la aplicación).

![diagrama que muestra varios servidores web que tienen acceso a los mismos servidores de caché](distributed-caching/_static/image1.png)

Cuando la aplicación se escala mediante la adición o eliminación de servidores, o cuando los servidores se reemplazan debido a actualizaciones o errores, los datos almacenados en caché permanecen accesibles para todos los servidores que ejecutan la aplicación.

Al evitar el acceso a los datos de latencia alta de un almacén de datos persistente, el almacenamiento en caché puede mejorar drásticamente la capacidad de respuesta de la aplicación. Por ejemplo, la recuperación de datos de la memoria caché es mucho más rápida que recuperarlos de una base de datos relacional.

Una ventaja lateral del almacenamiento en caché es reducir el tráfico al almacén de datos persistente, lo que puede dar lugar a costos menores cuando hay cargos de salida de datos para el almacén de datos persistente.

## <a name="when-to-use-distributed-caching"></a>Cuándo usar el almacenamiento en caché distribuido

El almacenamiento en caché funciona mejor para cargas de trabajo de aplicaciones que hacen más lectura que la escritura de datos y cuando el modelo de datos admite la organización de clave/valor que se usa para almacenar y recuperar datos en la memoria caché. También es más útil cuando los usuarios de la aplicación comparten muchos datos comunes. por ejemplo, la memoria caché no proporcionaría tantas ventajas si cada usuario normalmente recupera datos únicos para ese usuario. Un ejemplo en el que el almacenamiento en caché podría ser muy ventajoso es un catálogo de productos, ya que los datos no cambian con frecuencia y todos los clientes están examinando los mismos datos.

La ventaja de que el almacenamiento en caché es cada vez más mensurable a medida que se escala la aplicación, ya que los límites de rendimiento y los retrasos de latencia del almacén de datos persistentes se convierten en más de un límite en el rendimiento general de la aplicación. Sin embargo, podría implementar el almacenamiento en caché por otras razones que el rendimiento también. En el caso de los datos que no tienen que estar absolutamente actualizados cuando se muestran al usuario, el acceso a la caché puede actuar como un disyuntor para cuando el almacén de datos persistente no responde o no está disponible.

## <a name="popular-cache-population-strategies"></a>Estrategias de rellenado de caché populares

Para poder recuperar datos de la memoria caché, primero tiene que almacenarlos ahí. Existen varias estrategias para obtener los datos que necesita en una memoria caché:

- A petición/almacenamiento en caché

    La aplicación intenta recuperar datos de la memoria caché y, cuando la memoria caché no tiene los datos (un "error"), la aplicación almacena los datos en la memoria caché para que estén disponibles la próxima vez. La próxima vez que la aplicación intente obtener los mismos datos, detectará lo que está buscando en la memoria caché (una "visita"). Para evitar que se recuperen los datos en caché que han cambiado en la base de datos, invalide la memoria caché al realizar cambios en el almacén de datos.
- Inserciones de datos de fondo

    Los servicios en segundo plano envían datos a la memoria caché según una programación regular y la aplicación siempre extrae de la memoria caché. Este enfoque funciona bien con orígenes de datos de alta latencia que no requieren que siempre se devuelvan los datos más recientes.
- Disyuntor

    Normalmente, la aplicación se comunica directamente con el almacén de datos persistente, pero cuando el almacén de datos persistente tiene problemas de disponibilidad, la aplicación recupera los datos de la memoria caché. Es posible que los datos se hayan puesto en memoria caché con la estrategia de extracción de datos en segundo plano o en caché. Se trata de una estrategia de control de errores en lugar de una estrategia de mejora del rendimiento.

Para mantener actualizados los datos en la memoria caché, puede eliminar las entradas de caché relacionadas cuando la aplicación crea, actualiza o elimina datos. Si, en ocasiones, la aplicación obtiene datos ligeramente desactualizados, puede confiar en un tiempo de expiración configurable para establecer un límite en el modo en que los datos de caché antiguos pueden ser.

Puede configurar la expiración absoluta (cantidad de tiempo desde que se creó el elemento de caché) o la expiración variable (cantidad de tiempo transcurrido desde la última vez que se obtuvo acceso a un elemento de caché). La expiración absoluta se usa cuando se depende del mecanismo de expiración de la memoria caché para evitar que los datos se vuelvan obsoletos. En la aplicación Fix it expulsaremos manualmente los elementos obsoletos de la memoria caché y usaremos la expiración variable para mantener los datos más actualizados en la memoria caché. Independientemente de la Directiva de expiración que elija, la memoria caché expulsará automáticamente los elementos más antiguos (usados menos recientemente o LRU) cuando se alcance el límite de memoria de la memoria caché.

## <a name="sample-cache-aside-code-for-fix-it-app"></a>Código de la caché de ejemplo para la aplicación de corrección

En el código de ejemplo siguiente, se comprueba la memoria caché en primer lugar al recuperar una tarea de corrección de ti. Si la tarea se encuentra en la memoria caché, la devolvemos; Si no se encuentra, se obtiene de la base de datos y se almacena en la memoria caché. Se resaltan los cambios que se realizaron para agregar almacenamiento en caché al método `FindTaskByIdAsync`.

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

Al actualizar o eliminar una tarea corregir ti, tiene que invalidar (quitar) la tarea almacenada en caché. De lo contrario, los intentos futuros de leer esa tarea seguirán recibiendo los datos antiguos de la memoria caché.

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

Estos son ejemplos para mostrar el código de almacenamiento en caché simple; el almacenamiento en caché no se ha implementado en el proyecto de corrección de ti descargable.

## <a name="azure-caching-services"></a>Servicios de Caching de Azure

Azure ofrece los siguientes servicios de almacenamiento en caché: [Azure Redis cache](https://msdn.microsoft.com/library/dn690523.aspx) y [caché administrada de Azure](https://msdn.microsoft.com/library/dn386094.aspx). Azure Redis cache se basa en la conocida [Redis cache de código abierto](http://redis.io/) y es la primera opción para la mayoría de los escenarios de almacenamiento en caché.

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a>Estado de sesión ASP.NET mediante un proveedor de caché

Como se mencionó en el [capítulo prácticas recomendadas de desarrollo web](web-development-best-practices.md), se recomienda evitar el uso del estado de sesión. Si su aplicación requiere el estado de sesión, el siguiente procedimiento recomendado es evitar el proveedor en memoria predeterminado, ya que no habilita la escalabilidad horizontal (varias instancias del servidor Web). El proveedor de estado de sesión de ASP.NET SQL Server permite a un sitio que se ejecuta en varios servidores web utilizar el estado de sesión, pero supone un costo de latencia alto en comparación con un proveedor en memoria. La mejor solución si tiene que usar el estado de sesión es usar un proveedor de caché, como el [proveedor de estado de sesión para caché de Azure](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx).

## <a name="summary"></a>Resumen

Ha visto cómo la aplicación de corrección de ti podría implementar el almacenamiento en caché para mejorar el tiempo de respuesta y la escalabilidad, y para permitir que la aplicación siga respondiendo a las operaciones de lectura cuando la base de datos no está disponible. En el [siguiente capítulo](queue-centric-work-pattern.md) mostraremos cómo mejorar la escalabilidad y hacer que la aplicación siga respondiendo a las operaciones de escritura.

## <a name="resources"></a>Recursos

Para obtener más información sobre el almacenamiento en caché, vea los siguientes recursos.

Documentación

- [Caché de Azure](https://msdn.microsoft.com/library/gg278356.aspx). Documentación oficial de MSDN sobre el almacenamiento en caché en Azure.
- [Patrones y prácticas de Microsoft: Guía de Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte la guía de almacenamiento en caché y el patrón de reserva.
- [Failsafe: Guía para arquitecturas de nube resistentes](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Notas del producto de Marc Mercuri, Ulrich Homann y Andrew Townhill. Vea la sección sobre el almacenamiento en caché.
- [Prácticas recomendadas para el diseño de servicios a gran escala en Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Hora En las notas del producto, Mark SIMM y Michael Thomassy. Vea la sección sobre el almacenamiento en caché distribuido.
- [Almacenamiento en caché distribuido en la ruta de acceso a la escalabilidad](https://msdn.microsoft.com/magazine/dd942840.aspx). Un artículo de MSDN Magazine anterior (2009), pero una introducción claramente escrita al almacenamiento en caché distribuido en general. profundiza en más profundidad que las secciones de almacenamiento en caché de las notas del producto de infalible y procedimientos recomendados.

Vídeos

- [Failsafe: creación de Cloud Services escalables y resistentes](https://channel9.msdn.com/Series/FailSafe). Series de nueve partes de Ulrich Homann, Marc Mercuri y Mark SIMM. Presenta una vista de nivel 400 de cómo diseñar aplicaciones en la nube. Esta serie se centra en la teoría y los motivos por los que: para obtener más información sobre cómo hacerlo, consulte la creación de grandes series mediante Mark SIMM. Consulte la descripción de Caching en el episodio 3 a partir de 1:24:14.
- [Building Big: lecciones aprendidas de clientes de Azure, parte I](https://channel9.msdn.com/Events/Build/2012/3-029). Simon Davies describe el almacenamiento en caché distribuido a partir de 46:00. Similar a la serie failsafe, pero profundiza en más detalles. La presentación se proporcionó el 31 de octubre de 2012, por lo que no cubre el servicio de almacenamiento en caché de Web Apps en Azure App Service que se presentó en 2013.

Código de ejemplo

- [Aspectos básicos del servicio en la nube en Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Aplicación de ejemplo que implementa el almacenamiento en caché distribuido. Vea la entrada de blog adjunta [aspectos básicos del servicio en la nube: aspectos básicos de almacenamiento en caché](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx).

> [!div class="step-by-step"]
> [Anterior](transient-fault-handling.md)
> [Siguiente](queue-centric-work-pattern.md)
