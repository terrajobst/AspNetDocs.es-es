---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
title: Estrategias de particionamiento de datos (creación de aplicaciones en la nube reales con Azure) | Microsoft Docs
author: MikeWasson
description: La creación de aplicaciones reales en la nube con el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 513837a7-cfea-4568-a4e9-1f5901245d24
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
msc.type: authoredcontent
ms.openlocfilehash: efc3fa0255aa765e515412c5fa4098303a9d9234
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457029"
---
# <a name="data-partitioning-strategies-building-real-world-cloud-apps-with-azure"></a>Estrategias de particionamiento de datos (creación de aplicaciones en la nube reales con Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de corrección de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar el libro electrónico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **creación de aplicaciones reales en la nube con** el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden ayudarle a desarrollar correctamente aplicaciones web para la nube. Para obtener información sobre la serie, vea [el primer capítulo](introduction.md).

Anteriormente vimos lo fácil que es escalar el nivel Web de una aplicación en la nube, agregando y quitando servidores Web. Pero si se encuentran en el mismo almacén de datos, el cuello de botella de la aplicación se mueve del front-end al back-end y el nivel de datos es el más difícil de escalar. En este capítulo, veremos cómo puede hacer que la capa de datos sea escalable mediante la creación de particiones de datos en varias bases de datos relacionales o mediante la combinación de almacenamiento de bases de datos relacionales con otras opciones de almacenamiento de datos.

La configuración de un esquema de particiones se realiza mejor por adelantado por el mismo motivo mencionado anteriormente: es muy difícil cambiar la estrategia de almacenamiento de datos después de que una aplicación esté en producción. Si piensa más bien en los distintos enfoques, puede evitar tener un "momento de Twitter" cuando la aplicación se bloquea o deja de funcionar durante mucho tiempo mientras reorganiza los datos de la aplicación y el código de acceso a datos.

## <a name="the-three-vs-of-data-storage"></a>Los tres frente al almacenamiento de datos

Con el fin de determinar si necesita una estrategia de particionamiento y qué debe ser, tenga en cuenta tres preguntas sobre los datos:

- Volumen: ¿Cuántos datos se almacenan en última instancia? ¿Hay un par de gigabytes? ¿Hay un par de cientos de gigabytes? Terabytes? Petabytes?
- Velocidad: ¿Cuál es la velocidad a la que crecen los datos? ¿Es una aplicación interna que no está generando muchos datos? ¿Una aplicación externa en la que los clientes van a cargar imágenes y vídeos?
- Variedad: ¿Qué tipo de datos se almacenan? ¿Es relacional, imágenes, pares de clave-valor, gráficos sociales?

Si cree que va a tener una gran cantidad de volumen, velocidad o variedad, tendrá que considerar detenidamente qué tipo de esquema de partición permitirá a la aplicación escalar de forma eficiente y eficaz a medida que crezca y para asegurarse de que no se encuentre en ningún cuello de botella.

Básicamente, hay tres enfoques para la creación de particiones:

- Particiones verticales
- Particionamiento horizontal
- Creación de particiones híbridas

## <a name="vertical-partitioning"></a>Particiones verticales

La parte vertical es como la división de una tabla por columnas: un conjunto de columnas entra en un almacén de datos y otro conjunto de columnas entra en un almacén de datos diferente.

Por ejemplo, supongamos que mi aplicación almacena datos sobre personas, incluidas las imágenes:

![Tabla de datos](data-partitioning-strategies/_static/image1.png)

Cuando represente estos datos como una tabla y examine los distintos tipos de datos, puede ver que las tres columnas de la izquierda tienen datos de cadena que se pueden almacenar de manera eficaz mediante una base de datos relacional, mientras que las dos columnas de la derecha son básicamente matrices de bytes que c ome de archivos de imagen. Es posible almacenar datos de archivos de imagen en una base de datos relacional, y una gran cantidad de personas lo hacen porque no quieren guardar los datos en el sistema de archivos. Es posible que no tengan un sistema de archivos capaz de almacenar los volúmenes de datos necesarios o que no quieran administrar un sistema de copia de seguridad y restauración independiente. Este enfoque funciona bien para las bases de datos locales y para pequeñas cantidades de datos en las bases de datos en la nube. En el entorno local, podría ser más fácil permitir que el DBA se encargue de todo.

Pero en una base de datos en la nube, el almacenamiento es relativamente costoso y un gran volumen de imágenes podría aumentar el tamaño de la base de datos más allá de los límites en los que puede funcionar de manera eficaz. Puede resolver estos problemas mediante la partición vertical de los datos, lo que significa que debe elegir el almacén de datos más adecuado para cada columna de la tabla de datos. Lo que podría funcionar mejor en este ejemplo es colocar los datos de la cadena en una base de datos relacional y las imágenes en el almacenamiento de blobs.

![Tabla de datos particionada verticalmente](data-partitioning-strategies/_static/image2.png)

Almacenar imágenes en el almacenamiento de blobs en lugar de una base de datos es más práctico en la nube que en un entorno local, ya que no tiene que preocuparse por la configuración de servidores de archivos o la administración de copias de seguridad y restauración de datos almacenados fuera de la base de datos relacional: todo Esto lo controla automáticamente el servicio de almacenamiento de blobs.

Este es el enfoque de creación de particiones que hemos implementado en la aplicación de corrección de ti y veremos el código que se encuentra en el [capítulo almacenamiento de blobs](unstructured-blob-storage.md). Sin este esquema de partición y suponiendo que el tamaño medio de la imagen sea de 3 megabytes, la aplicación Fix it solo podría almacenar aproximadamente 40.000 tareas antes de alcanzar el tamaño máximo de la base de datos de 150 gigabytes. Después de quitar las imágenes, la base de datos puede almacenar 10 veces tantas tareas; puede pasar mucho más tiempo antes de tener que pensar en implementar un esquema de particionamiento horizontal. Y a medida que se escala la aplicación, sus gastos crecen más lentamente, ya que la mayor parte de sus necesidades de almacenamiento están en un almacenamiento de blobs muy económico.

## <a name="horizontal-partitioning-sharding"></a>Creación de particiones horizontales (particionamiento)

La parte horizontal es como la división de una tabla por filas: un conjunto de filas entra en un almacén de datos y otro conjunto de filas entra en un almacén de datos diferente.

Dado el mismo conjunto de datos, otra opción sería almacenar diferentes intervalos de nombres de clientes en distintas bases de datos.

![Tabla de datos particionada horizontalmente](data-partitioning-strategies/_static/image3.png)

Desea tener mucho cuidado en el esquema de particionamiento para asegurarse de que los datos se distribuyen uniformemente con el fin de evitar las zonas activas. Este sencillo ejemplo en el que se usa la primera letra del apellido no cumple con este requisito, ya que muchas personas tienen los apellidos que comienzan con determinadas letras comunes. Las limitaciones de tamaño de tabla se alcanzaban antes de lo que cabría esperar, ya que algunas bases de datos se obtendrían muy grandes, mientras que la mayoría permanecería reducida.

Una parte inferior de la creación de particiones horizontales es que podría ser difícil realizar consultas en todos los datos. En este ejemplo, una consulta tendría que dibujar de hasta 26 bases de datos diferentes para obtener todos los datos almacenados por la aplicación.

## <a name="hybrid-partitioning"></a>Creación de particiones híbridas

Puede combinar el particionamiento vertical y horizontal. Por ejemplo, en los datos de ejemplo puede almacenar las imágenes en el almacenamiento de blobs y particionar horizontalmente los datos de la cadena.

![Tabla de datos con particiones híbridas](data-partitioning-strategies/_static/image4.png)

## <a name="partitioning-a-production-application"></a>Crear particiones en una aplicación de producción

Conceptualmente, es fácil ver cómo funcionaría un esquema de partición, pero cualquier esquema de partición aumenta la complejidad del código y presenta muchas nuevas complicaciones que hay que solucionar. Si va a mover imágenes al almacenamiento de blobs, ¿qué se puede hacer cuando el servicio de almacenamiento está inactivo? ¿Cómo se controla la seguridad de blobs? ¿Qué ocurre si la base de datos y el almacenamiento de blobs no están sincronizados? Si va a realizar el particionamiento, ¿cómo va a controlar las consultas en todas las bases de datos?

Las complicaciones se pueden administrar siempre y cuando se planeen antes de pasar a producción. Muchas personas que no lo han hecho quieren que lo tuvieran más adelante. En promedio, nuestro equipo de equipo de asesoramiento al cliente (CAT) recibe llamadas telefónicas de una vez al mes desde los clientes cuyas aplicaciones están desconectadas de un modo muy grande y no lo hacen. Y dicen algo parecido a: "Help! Me pongo todo en un único almacén de datos y, en 45 días, voy a quedarse sin espacio en él. Además, si tiene una gran cantidad de lógica de negocios integrada en el modo de acceder a su almacén de datos y tiene clientes que usan la aplicación, no hay mucho tiempo para dejar de funcionar durante un día mientras realiza la migración. Acabamos trabajando en herculean para ayudar al cliente a particionar sus datos sobre la marcha sin tiempo de inactividad. Es muy emocionante y muy terrible, y no algo que quiere que participe en caso de que pueda evitarlo. Al pensar en esto por adelantado y integrarlo en la aplicación, su vida será mucho más fácil si la aplicación crece más adelante.

## <a name="summary"></a>Resumen

Un esquema de particionamiento eficaz puede permitir que la aplicación en la nube se escale a petabytes de datos en la nube sin cuellos de botella. Y no tiene que pagar por adelantado para máquinas masivas o una infraestructura amplia como podría si estuviera ejecutando la aplicación en un centro de datos local. En la nube, puede Agregar incrementalmente la capacidad a medida que la necesite, y solo pagará por todo lo que esté usando cuando la use.

En el [siguiente capítulo](unstructured-blob-storage.md) veremos cómo la aplicación de corrección de ti implementa las particiones verticales mediante el almacenamiento de imágenes en BLOB Storage.

## <a name="resources"></a>Recursos

Para obtener más información sobre las estrategias de particionamiento, vea los siguientes recursos.

Documentación:

- [Prácticas recomendadas para el diseño de servicios a gran escala en Windows Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). En las notas del producto, Mark SIMM y Michael Thomassy.
- [Patrones y prácticas de Microsoft: patrones de diseño en la nube](https://msdn.microsoft.com/library/dn568099.aspx). Vea Guía de creación de particiones de datos, patrón de particionamiento.

Videos:

- [Failsafe: creación de Cloud Services escalables y resistentes](https://channel9.msdn.com/Series/FailSafe). Series de nueve partes de Ulrich Homann, Marc Mercuri y Mark SIMM. Presenta conceptos de alto nivel y principios arquitectónicos de una manera muy accesible e interesante, con historias tomadas de la experiencia del equipo de asesoramiento al cliente (CAT) de Microsoft con clientes reales. Consulte la explicación de la creación de particiones en el episodio 7.
- [Building Big: lecciones aprendidas de clientes de Windows Azure, parte I](https://channel9.msdn.com/Events/Build/2012/3-029). Mark SIMM describe los esquemas de partición, las estrategias de particionamiento, cómo implementar el particionamiento y SQL Database federaciones, a partir de 19:49. Similar a la serie failsafe, pero profundiza en más detalles.

Ejemplo de código:

- [Aspectos básicos del servicio en la nube en Windows Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Aplicación de ejemplo que incluye una base de datos particionada. Para obtener una descripción del esquema de particionamiento implementado, vea [dal: particionamiento de RDBMS](https://blogs.msdn.com/b/windowsazure/archive/2013/09/05/dal-sharding-of-rdbms.aspx) en el blog de Windows Azure.

> [!div class="step-by-step"]
> [Anterior](data-storage-options.md)
> [Siguiente](unstructured-blob-storage.md)
