---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
title: Opciones de almacenamiento de datos (creación de aplicaciones en la nube reales con Azure) | Microsoft Docs
author: MikeWasson
description: La creación de aplicaciones reales en la nube con el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: e51fcecb-cb33-4f9e-8428-6d2b3d0fe1bf
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
msc.type: authoredcontent
ms.openlocfilehash: 9357ed5aef39bed501cdac9ac26d46c884d4fae0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78500833"
---
# <a name="data-storage-options-building-real-world-cloud-apps-with-azure"></a>Opciones de almacenamiento de datos (creación de aplicaciones en la nube reales con Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de corrección de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar el libro electrónico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **creación de aplicaciones reales en la nube con** el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden ayudarle a desarrollar correctamente aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).

La mayoría de las personas se usan para las bases de datos relacionales y tienden a pasar por alto otras opciones de almacenamiento de datos cuando están diseñando una aplicación en la nube. El resultado puede ser un rendimiento no óptimo, altos gastos o peor, ya que las bases de datos [NoSQL](http://en.wikipedia.org/wiki/NoSQL) (no relacionales) pueden controlar de forma más eficaz algunas tareas que las bases de datos relacionales. Cuando los clientes nos piden ayuda para resolver un problema crítico de almacenamiento de datos, a menudo se debe a que tienen una base de datos relacional en la que una de las opciones de NoSQL habría funcionado mejor. En esas situaciones, el cliente habría sido mejor desactivado si hubiera implementado la solución NoSQL antes de implementar la aplicación en producción.

Por otro lado, también sería un error asumir que una base de datos NoSQL puede hacer todo bien o suficientemente bien. No existe una única opción de administración de datos para todas las tareas de almacenamiento de datos; las distintas soluciones de administración de datos están optimizadas para diferentes tareas. La mayoría de las aplicaciones en la nube reales tienen una variedad de requisitos de almacenamiento de datos y a menudo se proporcionan mejor mediante una combinación de varias soluciones de almacenamiento de datos.

El propósito de este capítulo es ofrecer una idea más amplia de las opciones de almacenamiento de datos disponibles para una aplicación en la nube y algunas instrucciones básicas sobre cómo elegir las que se adaptan a su escenario. Es mejor tener en cuenta las opciones disponibles y pensar en sus puntos fuertes y débiles antes de desarrollar una aplicación. Cambiar las opciones de almacenamiento de datos en una aplicación de producción puede ser muy difícil, como tener que cambiar un motor jet mientras el plano está en vuelo.

## <a name="data-storage-options-on-azure"></a>Opciones de almacenamiento de datos en Azure

La nube hace que sea relativamente fácil usar una variedad de almacenes de datos relacionales y NoSQL. Estas son algunas de las plataformas de almacenamiento de datos que puede usar en Azure.

![](data-storage-options/_static/image1.png)

En la tabla se muestran cuatro tipos de bases de datos NoSQL:

- [Las bases de datos de clave/valor](https://msdn.microsoft.com/library/dn313285.aspx#sec7) almacenan un único objeto serializado para cada valor de clave. Se recomiendan para almacenar grandes volúmenes de datos donde se desee obtener un elemento para un valor de clave determinado valor de clave y no tiene que realizar consultas basándose en otras propiedades del elemento.

    [Azure BLOB Storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/) es una base de datos de clave-valor que funciona como el almacenamiento de archivos en la nube, con valores de clave que se corresponden con los nombres de carpeta y archivo. Puede recuperar un archivo por su nombre de archivo y carpeta, no buscando valores en el contenido del archivo.

    [Azure Table Storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/) es también una base de datos de clave/valor. Cada valor se denomina *entidad* (similar a una fila, identificada por una clave de partición y una clave de fila) y contiene varias *propiedades* (similares a las columnas, pero no todas las entidades de una tabla tienen que compartir las mismas columnas). La consulta en columnas distintas de la clave es extremadamente ineficaz y debe evitarse. Por ejemplo, puede almacenar los datos de Perfil de usuario, con una partición que almacena información acerca de un solo usuario. Puede almacenar datos como el nombre de usuario, el hash de contraseña, la fecha de nacimiento, etc., en propiedades independientes de una entidad o en entidades independientes en la misma partición. Pero no querrá consultar a todos los usuarios con un intervalo determinado de fechas de nacimiento y no puede ejecutar una consulta de combinación entre la tabla de perfil y otra tabla. El almacenamiento de tablas es más escalable y más económico que una base de datos relacional, pero no habilita consultas complejas ni combinaciones.
- [Documentdatabases](https://msdn.microsoft.com/library/dn313285.aspx#sec8) son bases de datos de clave/valor en las que los valores son *documentos*. "Documento" no se usa en el sentido de un documento de Word o Excel, sino que es una colección de campos y valores con nombre, cualquiera de los cuales podría ser un documento secundario. Por ejemplo, en una tabla de historial de pedidos, un documento de pedido podría tener los campos número de pedido, fecha de pedido y cliente. y el campo Customer puede tener los campos name y Address. La base de datos codifica los datos de campo en un formato como XML, YAML, JSON o BSON; o bien, puede usar texto sin formato. Una característica que establece las bases de datos de documentos además de las bases de datos de clave y valor es la posibilidad de realizar consultas en los campos que no son de clave y definir índices secundarios para que las consultas sean más eficaces. Esta capacidad hace que una base de datos de documentos sea más adecuada para las aplicaciones que necesitan recuperar datos en función de criterios más complejos que el valor de la clave del documento. Por ejemplo, en una base de datos de documentos de historial de pedidos de ventas, podría consultar varios campos como el ID. de producto, el ID. de cliente, el nombre del cliente, etc. [MongoDB](http://www.mongodb.org/) es una conocida base de datos de documentos.
- [Las bases de datos de familia de columnas](https://msdn.microsoft.com/library/dn313285.aspx#sec9) son almacenes de datos de clave/valor que permiten estructurar el almacenamiento de datos en colecciones de columnas relacionadas denominadas familias de columnas. Por ejemplo, una base de datos de censo podría tener un grupo de columnas para el nombre de una persona (primero, medio, último), un grupo para la dirección de la persona y un grupo para la información de Perfil de la persona (DOB, sexo, etc.). A continuación, la base de datos puede almacenar cada familia de columnas en una partición independiente manteniendo todos los datos de una persona relacionada con la misma clave. Después, puede leer toda la información del perfil sin tener que leer toda la información sobre el nombre y la dirección. [Cassandra](http://cassandra.apache.org/) es una conocida base de datos de familia de columnas.
- [Las bases](https://msdn.microsoft.com/library/dn313285.aspx#sec10) de datos de grafos almacenan información como una colección de objetos y relaciones. El propósito de una base de datos de grafos es permitir a una aplicación realizar de forma eficaz consultas que atraviesan la red de objetos y las relaciones entre ellos. Por ejemplo, los objetos podrían ser los empleados en una base de datos de recursos humanos y puede desear facilitar consultas como "buscar todos los empleados que trabajan directa o indirectamente para Scott". [Neo4j](http://www.neo4j.org/) es una conocida base de datos de gráficos.

En comparación con las bases de datos relacionales, las opciones de NoSQL ofrecen una escalabilidad y rentabilidad mucho mayores para el almacenamiento y el análisis de datos no estructurados. El inconveniente es que no proporcionan la rica capacidad de consulta y las sólidas capacidades de integridad de datos de las bases de datos relacionales. NoSQL funcionaría bien para los datos de registro de IIS, lo que implica un gran volumen sin necesidad de consultas de combinación. NoSQL no funcionaría tan bien para las transacciones bancarias, que requiere integridad de datos absoluta y implica muchas relaciones con otros datos relacionados con la cuenta.

También hay una categoría más reciente de la plataforma de base de datos denominada [NewSQL](http://en.wikipedia.org/wiki/NewSQL) que combina la escalabilidad de una base de datos NoSQL con la integridad transaccional y de consulta de una base de datos relacional. Las bases de datos de NewSQL están diseñadas para el procesamiento de consultas y almacenamiento distribuido, que suele ser difícil de implementar en las bases de datos de "OldSQL". [NuoDB](http://www.nuodb.com/) es un ejemplo de una base de datos de NewSQL que se puede usar en Azure.

<a id="hadoop"></a>
## <a name="hadoop-and-mapreduce"></a>Hadoop y MapReduce

Los grandes volúmenes de datos que se pueden almacenar en bases de datos NoSQL pueden ser difíciles de analizar de manera eficaz y oportuna. Para ello, puede usar un marco de trabajo como [Hadoop](http://hadoop.apache.org/) que implementa la funcionalidad de [MapReduce](http://en.wikipedia.org/wiki/MapReduce) . En esencia, lo que hace un proceso de MapReduce es el siguiente:

- Limite el tamaño de los datos que deben procesarse seleccionando fuera del almacén de datos solo los datos que realmente necesita analizar. Por ejemplo, desea conocer la estructura de la base de usuarios por año de nacimiento, de modo que seleccione solo los años de nacimiento de su almacén de datos de Perfil de usuario.
- Divida los datos en partes y envíelos a distintos equipos para su procesamiento. El equipo A calcula el número de personas con 1950-1959 fechas, el equipo B hace 1960-1969, etc. Este grupo de equipos se denomina *clúster de Hadoop*.
- Vuelva a colocar los resultados de cada parte una vez que se haya completado el procesamiento de los elementos. Ahora tiene una lista relativamente breve de cuántas personas para cada nacimiento Year y la tarea de calcular porcentajes en esta lista global es administrable.

En Azure, [HDInsight](https://azure.microsoft.com/services/hdinsight/) le permite procesar, analizar y obtener nuevas perspectivas a partir de macrodatos con la potencia de Hadoop. Por ejemplo, podría utilizarlo para analizar los registros del servidor Web:

- Habilite el registro del servidor Web en su cuenta de almacenamiento. Esto configura Azure para escribir registros en el servicio BLOB para cada solicitud HTTP a su aplicación. BLOB Service básicamente es el almacenamiento de archivos en la nube y se integra perfectamente con HDInsight.

    ![Registros para Blob Storage](data-storage-options/_static/image2.png)
- A medida que la aplicación obtiene tráfico, los registros de IIS del servidor Web se escriben en el almacenamiento de blobs.

    ![Registros de servidor web](data-storage-options/_static/image3.png)
- En el portal, haga clic en **nuevo** - **Data Services** - **HDInsight** - **creación rápida**y especifique un nombre de clúster de hdinsight, el tamaño del clúster (número de nodos de datos del clúster de hdinsight) y un nombre de usuario y una contraseña para el clúster de hdinsight.

    ![HDInsight](data-storage-options/_static/image4.png)

Ahora puede configurar trabajos de MapReduce para analizar los registros y obtener respuestas a preguntas como:

- ¿Qué horas del día obtienen el tráfico más o menos?
- ¿A qué países procede mi tráfico?
- ¿Cuál es el promedio de ingresos de los ámbitos de las áreas de las que procede mi tráfico? (Hay un conjunto de resultados público que proporciona los ingresos de los alrededores por dirección IP, y puede coincidir con el de la dirección IP en los registros del servidor Web).
- ¿Cómo se relacionan los ingresos de los alrededores entre las páginas o los productos específicos del sitio?

Después, puede usar las respuestas a preguntas como estas para dirigirse a los anuncios en función de la probabilidad de que un cliente le interese o pueda comprar un producto determinado.

Como se explica en el [capítulo automatizar todo](automate-everything.md), la mayoría de las funciones que puede realizar en el portal se pueden automatizar y que incluyen la instalación y ejecución de trabajos de análisis de HDInsight. Un script de HDInsight típico podría contener los siguientes pasos:

- Aprovisione un clúster de HDInsight y vincúlelo a su cuenta de almacenamiento para la entrada de almacenamiento de blobs.
- Cargue los ejecutables del trabajo de MapReduce (archivos. jar o. exe) en el clúster de HDInsight.
- Envíe un MapReduce que almacene los datos de salida en el almacenamiento de blobs.
- Espere a que se complete el trabajo.
- Elimine el clúster de HDInsight.
- Acceder a la salida desde el almacenamiento de blobs.

Al ejecutar un script que hace todo esto, se minimiza la cantidad de tiempo que se aprovisiona el clúster de HDInsight, lo que minimiza los costos.

<a id="paasiaas"></a>
## <a name="platform-as-a-service-paas-versus-infrastructure-as-a-service-iaas"></a>Plataforma como servicio (PaaS) frente a infraestructura como servicio (IaaS)

Las opciones de almacenamiento de datos enumeradas anteriormente incluyen soluciones de plataforma como servicio (PaaS) e infraestructura como servicio (IaaS). PaaS significa que administramos la infraestructura de hardware y software, y que solo se usa el servicio. SQL Database es una característica de PaaS de Azure. Se solicitan bases de datos y, en segundo plano, Azure configura y configura las máquinas virtuales y configura las bases de datos en ellas. No tiene acceso directo a las máquinas virtuales y no tiene que administrarlas. IaaS significa que se configuran, configuran y administran las máquinas virtuales que se ejecutan en nuestra infraestructura de centro de datos y se coloca todo lo que se desee en ellos. Proporcionamos una galería de imágenes de máquina virtual preconfiguradas para configuraciones comunes de máquinas virtuales. Por ejemplo, puede instalar imágenes de máquina virtual preconfiguradas para Windows Server 2008, Windows Server 2012, BizTalk Server, Oracle WebLogic Server, Oracle Database, etc.

Entre las soluciones de datos de PaaS que ofrece Azure se incluyen:

- Azure SQL Database (antes conocido como SQL Azure). Base de datos relacional en la nube basada en SQL Server.
- Azure Table Storage Una base de datos NoSQL de clave/valor.
- Azure Blob Storage. Almacenamiento de archivos en la nube.

En el caso de IaaS, puede ejecutar todo lo que puede cargar en una máquina virtual, por ejemplo:

- Bases de datos relacionales como SQL Server, Oracle, MySQL, SQL Compact, SQLite o postgres.
- Almacenes de datos de clave/valor como Memcached, Redis, Cassandra y Riak.
- Almacenes de datos de columna como HBase.
- Bases de datos de documentos como MongoDB, RavenDB y CouchDB.
- Bases de datos de grafos como Neo4j.

![Opciones de almacenamiento de datos en Azure](data-storage-options/_static/image5.png)

La opción IaaS proporciona casi ilimitados opciones de almacenamiento de datos y muchos de ellos son especialmente fáciles de usar porque se pueden crear máquinas virtuales con imágenes preconfiguradas. Por ejemplo, en el portal de administración, vaya a **virtual machines**, haga clic en la pestaña **imágenes** y, a continuación, haga clic en **examinar VM Depot**.

![Examinar VM Depot](data-storage-options/_static/image6.png)

A continuación, verá una lista de [cientos de imágenes de máquina virtual preconfiguradas](http://www.hanselman.com/blog/Over400VirtualMachineImagesOfOpenSourceSoftwareStacksInTheVMDepotAzureGallery.aspx), y puede crear una máquina virtual a partir de una imagen que tenga un sistema de administración de bases de datos preinstalado, como MongoDB, Neo4J, Redis, Cassandra o Couchdb:

![MongoDB en VM Depot](data-storage-options/_static/image7.png)

Azure hace que las opciones de almacenamiento de datos de IaaS sean tan fáciles de usar como sea posible, pero las ofertas de PaaS tienen muchas ventajas que hacen que sean más rentables y prácticas para muchos escenarios:

- No tiene que crear máquinas virtuales, solo tiene que usar el portal o un script para configurar un almacén de datos. Si desea un almacén de datos de 200 terabytes, simplemente puede hacer clic en un botón o ejecutar un comando y, en segundos, está listo para su uso.
- No tiene que administrar ni aplicar revisiones a las máquinas virtuales que usa el servicio. Microsoft lo hace automáticamente.-no tiene que preocuparse por la configuración de la infraestructura de escalado o alta disponibilidad. Microsoft se encarga de todo esto.
- No tiene que comprar licencias; las tarifas de licencias se incluyen en las tarifas de servicio.
- Pague solo por lo que usa.

Las opciones de almacenamiento de datos de PaaS en Azure incluyen ofertas por proveedores de terceros. Por ejemplo, puede elegir el [complemento MongoLab](https://azure.microsoft.com/documentation/articles/store-mongolab-web-sites-dotnet-store-data-mongodb/) de la tienda de Azure para aprovisionar una base de datos de MongoDB como servicio.

## <a name="choosing-a-data-storage-option"></a>Elección de una opción de almacenamiento de datos

No hay un enfoque adecuado para todos los escenarios. Si alguien dice que esta tecnología es la respuesta, lo primero que hay que hacer es "¿Cuál es la pregunta?", ya que las distintas soluciones están optimizadas para diferentes aspectos. Existen ventajas definitivas para el modelo relacional. Este es el motivo por el que se ha tardado mucho tiempo. Pero también hay desplegables de SQL que se pueden direccionar con una solución NoSQL.

A menudo, lo que vemos es más práctico es un enfoque composicional, donde se usa SQL y NoSQL en una única solución. Incluso cuando las personas dicen que están adoptando NoSQL, si profundiza en lo que están haciendo, a menudo verá que están usando varios marcos NoSQL diferentes: están usando [Couchdb](http://wiki.apache.org/couchdb/Introduction), [Redis](http://redis.io/)y [Riak](http://basho.com/riak/) para diferentes aspectos. Incluso Facebook, que usa NoSQL en gran medida, usa marcos NoSQL diferentes para distintas partes del servicio. La flexibilidad para mezclar y hacer coincidir los enfoques de almacenamiento de datos es una de las cosas que es agradable sobre la nube, ya que es fácil usar varias soluciones de datos e integrarlas en una sola aplicación.

Estas son algunas preguntas que hay que tener en cuenta a la hora de elegir un enfoque:

| Semántica de los datos | -¿Qué es el almacenamiento de datos principal y la semántica de acceso a datos (¿almacena datos relacionales o no estructurados)? Los datos no estructurados, como los archivos multimedia, se adaptan mejor al almacenamiento de blobs. una colección de datos relacionados, como productos, inventarios, proveedores, pedidos de clientes, etc., se adapta mejor a una base de datos relacional. |
| --- | --- |
| Compatibilidad con consultas | -¿Es fácil consultar los datos? -¿Qué tipos de preguntas se pueden preguntar eficazmente? Los almacenes de datos de clave/valor son muy buenos en obtener una sola fila a partir de un valor de clave, pero no es tan bueno para las consultas complejas. En el caso de un almacén de datos de Perfil de usuario en el que siempre obtiene los datos de un usuario determinado, un almacén de datos de clave/valor podría funcionar bien. para un catálogo de productos en el que desea obtener agrupaciones diferentes en función de los distintos atributos de producto, una base de datos relacional podría funcionar mejor. Las bases de datos NoSQL pueden almacenar grandes volúmenes de datos de forma eficaz, pero tiene que estructurar la base de datos en torno a la manera en que la aplicación consulta los datos y esto hace que las consultas ad hoc sean más difíciles de llevar a cabo. Con una base de datos relacional, puede crear prácticamente cualquier tipo de consulta. |
| Proyección funcional | -¿Se pueden ejecutar preguntas, agregaciones, etc.) en el lado servidor? Si ejecuto SELECT COUNT (\*) desde una tabla de SQL, se realizará todo el trabajo en el servidor y se devolverá el número que busco. Si quiero el mismo cálculo de un almacén de datos NoSQL que no admite la agregación, se trata de una "consulta sin enlazar" ineficaz y probablemente se agote el tiempo de espera. Incluso si la consulta se ejecuta correctamente, es necesario recuperar todos los datos del servidor en el cliente y contar las filas en el cliente. -¿Qué idiomas o tipos de expresiones se pueden usar? Con una base de datos relacional, puedo usar SQL. Con algunas bases de datos NoSQL como Azure Table Storage, usaré [OData](http://www.odata.org/)y todo lo que puedo hacer es filtrar por clave principal y obtener proyecciones (seleccionar un subconjunto de los campos disponibles). |
| Facilidad de escalabilidad | ¿Con qué frecuencia y cuántos serán los datos para escalar? -¿La plataforma implementa de forma nativa el escalado horizontal? -¿Es fácil agregar o quitar capacidad (tamaño y rendimiento)? Las tablas y bases de datos relacionales no se particionan automáticamente para que sean escalables, por lo que son difíciles de escalar más allá de ciertas limitaciones. Los almacenes de datos NoSQL, como Azure Table Storage, crean particiones inherentemente a todo, y casi no hay límite para agregar particiones. Puede escalar fácilmente Table Storage hasta 200 terabytes, pero el tamaño máximo de la base de datos para Azure SQL Database es de 500 gigabytes. Puede escalar los datos relacionales mediante la creación de particiones en varias bases de datos, pero la configuración de una aplicación para que admita ese modelo implica una gran cantidad de trabajo de programación. |
| Instrumentación y capacidad de administración | -¿Es fácil instrumentar, supervisar y administrar la plataforma? Deberá mantenerse informado sobre el estado y el rendimiento del almacén de datos, por lo que debe saber por adelantado las métricas que proporciona una plataforma de forma gratuita y lo que tiene que desarrollar usted mismo. |
| Operaciones | -¿Es fácil implementar y ejecutar la plataforma en Azure? PaaS? IaaS? Linuxun? Table Storage y SQL Database son fáciles de configurar en Azure. Las plataformas que no están integradas en las soluciones PaaS de Azure requieren más esfuerzo. |
| Compatibilidad de la API | -¿Está disponible una API que facilita el trabajo con la plataforma? Para el servicio tabla de Azure hay un SDK con una API de .NET que admite el modelo de programación asincrónica de .NET 4,5. Si va a escribir una aplicación de .NET, será mucho más fácil escribir y probar el código para el servicio tabla de Azure en comparación con otra plataforma de almacén de datos de columna de clave y valor que no tenga ninguna API o una menos completa. |
| Integridad transaccional y coherencia de los datos | -¿Es fundamental que la plataforma admita las transacciones para garantizar la coherencia de los datos? Para realizar un seguimiento de los correos electrónicos masivos enviados, el rendimiento y el bajo costo de almacenamiento de datos pueden ser más importantes que la compatibilidad automática con transacciones o la integridad referencial en la plataforma de datos, lo que hace que Azure Table Service sea una buena elección. Para realizar un seguimiento de los saldos de cuentas bancarias o los pedidos de compra, una plataforma de base de datos relacional que proporciona garantías transaccionales fuertes sería una mejor opción. |
| Continuidad empresarial | -¿Es fácil realizar copias de seguridad, restauraciones y recuperación ante desastres? Los datos de producción más pronto o posteriores se dañarán y necesitará una función para deshacer. Las bases de datos relacionales suelen tener una funcionalidad de restauración más específica, como la capacidad de restaurar a un momento dado. Entender qué características de restauración están disponibles en cada plataforma que está considerando es un factor importante a tener en cuenta. |
| Coste | -Si más de una plataforma puede admitir la carga de trabajo de datos, ¿cómo se comparan en el costo? Por ejemplo, si usa ASP.NET Identity, puede almacenar datos de Perfil de usuario en Azure Table Service o Azure SQL Database. Si no necesita las funciones de consulta enriquecidas de SQL Database, puede elegir las tablas de Azure en parte porque cuesta mucho menos para una cantidad determinada de almacenamiento. |

Lo que se suele recomendar es conocer la respuesta a las preguntas de cada una de estas categorías antes de elegir sus soluciones de almacenamiento de datos.

Además, la carga de trabajo puede tener requisitos específicos que algunas plataformas pueden admitir mejor que otras. Por ejemplo:

- ¿La aplicación requiere funcionalidades de auditoría?
- ¿Cuáles son los requisitos de longevidad de datos? ¿necesita funcionalidades de archivo o purga automatizadas?
- ¿Tiene necesidades de seguridad especializadas? Por ejemplo, los datos incluyen PII (información de identificación personal), pero debe asegurarse de que PII está excluido de los resultados de la consulta.
- Si tiene datos que no se pueden almacenar en la nube por razones normativas o tecnológicas, es posible que necesite una plataforma de almacenamiento de datos en la nube que facilite la integración con el almacenamiento local.

## <a name="demo--using-sql-database-in-azure"></a>Demostración: uso de SQL Database en Azure

La aplicación Fix it usa una base de datos relacional para almacenar tareas. El script de creación de entorno de Windows PowerShell que se muestra en el [capítulo automatizar todo](automate-everything.md) crea dos instancias de SQL Database. Para verlos en el portal, haga clic en la pestaña **bases de datos SQL** .

![Bases de datos SQL en el portal](data-storage-options/_static/image8.png)

También es fácil crear bases de datos mediante el portal.

Haga clic en **nuevo: Data Services** -- **SQL Database** -- **creación rápida**, escriba un nombre de base de datos, elija un servidor que ya tenga en la cuenta o cree uno nuevo y haga clic en **crear SQL Database**.

![Crear una base de datos de SQL Database](data-storage-options/_static/image9.png)

Espere unos segundos y tenga una base de datos en Azure lista para su uso.

![Nuevo SQL Database creado](data-storage-options/_static/image10.png)

Por lo tanto, Azure hace unos segundos lo que puede tardar un día o una semana o más en realizarse en el entorno local. Además, puesto que puede crear fácilmente bases de datos de forma automática en un script o mediante una API de administración, puede escalar horizontalmente de forma dinámica mediante la distribución de los datos entre varias bases de datos, siempre y cuando la aplicación se haya programado para ello.

Este es un ejemplo de nuestro modelo de plataforma como servicio. No tiene que administrar los servidores, lo hacemos. No tiene que preocuparse de las copias de seguridad, lo hacemos. Se está ejecutando en alta disponibilidad: los datos de la base de datos se replican automáticamente en tres servidores. Si un equipo deja de funcionar, la conmutación por error se realiza automáticamente y no se pierden datos. El servidor se revisa regularmente, no es necesario preocuparse por ello.

Haga clic en un botón para obtener la cadena de conexión exacta que necesita y puede empezar a usar la nueva base de datos inmediatamente.

![Cadenas de conexión](data-storage-options/_static/image11.png)

El panel muestra el historial de conexión y la cantidad de almacenamiento usado.

![Panel de SQL Database](data-storage-options/_static/image12.png)

Puede administrar bases de datos en el portal o usando SQL Server herramientas con las que ya está familiarizado, como SQL Server Management Studio (SSMS) y Visual Studio Tools Explorador de objetos de SQL Server (SSOX) y Explorador de servidores.

![SSOX](data-storage-options/_static/image13.png)

Otro buen aspecto es el modelo de precios. Puede iniciar el desarrollo con una base de datos gratuita de 20 MB y una base de datos de producción se inicia en unos $5 al mes. Solo se paga por la cantidad de datos que se almacenan realmente en la base de datos, no por la capacidad máxima. No tiene que comprar una licencia.

SQL Database es fácil de escalar. En el caso de la aplicación Fix it, la base de datos que creamos en nuestro script de automatización está limitada a 1 GB. Si desea escalar hasta 150 gigas, solo tiene que ir al portal y cambiar esa configuración, o bien ejecutar un comando de la API de REST, y en segundos tiene una base de datos de 150 Gig en la que puede implementar datos.

![Ediciones y tamaños de SQL Database](data-storage-options/_static/image14.png)

Esta es la eficacia de la nube para que la infraestructura se consigne de forma rápida y sencilla, y empiece a usarla inmediatamente.

La aplicación Fix it utiliza dos bases de datos SQL, una para la pertenencia (autenticación y autorización) y otra para los datos, y esto es todo lo que tiene que hacer para aprovisionarla y escalarla. Anteriormente, vio cómo aprovisionar las bases de datos a través de scripts de Windows PowerShell y ahora también ha visto lo fácil que es hacerlo en el portal.

## <a name="entity-framework-versus-direct-database-access-using-adonet"></a>Entity Framework frente al acceso directo a bases de datos mediante ADO.NET

La aplicación Fix it tiene acceso a estas bases de datos mediante el Entity Framework, el ORM recomendado de Microsoft (asignador relacional de objetos) para aplicaciones .NET. Un ORM es una herramienta excelente que facilita la productividad de los desarrolladores, pero la productividad se produce a costa de degradar el rendimiento en algunos escenarios. En una aplicación en la nube del mundo real, no podrá optar entre usar EF o usar ADO.NET directamente: usará ambos. La mayoría de las veces, cuando se escribe código que funciona con la base de datos, obtener el máximo rendimiento no es crítico y se puede aprovechar la codificación y las pruebas simplificadas que se obtienen con el Entity Framework. En situaciones en las que la sobrecarga de EF produciría un rendimiento inaceptable, puede escribir y ejecutar sus propias consultas mediante ADO.NET, idealmente llamando a procedimientos almacenados.

Sea cual sea el método que use para tener acceso a la base de datos, querrá minimizar el "grado de chat" tanto como sea posible. En otras palabras, si puede obtener todos los datos que necesita en un conjunto de resultados de consultas más grande, en lugar de docenas o cientos de otros más pequeños, suele ser preferible. Por ejemplo, si necesita enumerar a los estudiantes y los cursos en los que están inscritos, suele ser mejor obtener todos los datos en una consulta de combinación en lugar de obtener los estudiantes en una consulta y ejecutar consultas independientes para los cursos de cada estudiante.

## <a name="sql-databases-and-the-entity-framework-in-the-fix-it-app"></a>SQL Database y el Entity Framework en la aplicación Fix it

En la aplicación Fix it, la clase `FixItContext`, que deriva de la clase Entity Framework `DbContext`, identifica la base de datos y especifica las tablas en la base de datos. El contexto especifica un conjunto de entidades (tabla) para las tareas y el código pasa al contexto el nombre de la cadena de conexión. Ese nombre hace referencia a una cadena de conexión que se define en el archivo Web. config.

[!code-csharp[Main](data-storage-options/samples/sample1.cs?highlight=4,8)]

La cadena de conexión en el archivo *Web. config* se denomina appdb (aquí que apunta a la base de datos de desarrollo local):

[!code-xml[Main](data-storage-options/samples/sample2.xml?highlight=3)]

El Entity Framework crea una tabla *FixItTasks* basada en las propiedades incluidas en la clase de entidad `FixItTask`. Se trata de una clase simple POCO (objeto CLR antiguo), lo que significa que no hereda de ni tiene ninguna dependencia en el Entity Framework. Pero Entity Framework sabe cómo crear una tabla basada en ella y ejecutar operaciones CRUD (crear, leer, actualizar y eliminar) con ella.

[!code-csharp[Main](data-storage-options/samples/sample3.cs)]

![Tabla FixItTasks](data-storage-options/_static/image15.png)

La aplicación Fix it incluye una interfaz de repositorio que se usa para las operaciones CRUD que funcionan con el almacén de datos.

[!code-csharp[Main](data-storage-options/samples/sample4.cs)]

Tenga en cuenta que los métodos del repositorio son asincrónicos, por lo que todo el acceso a los datos se puede realizar de forma completamente asincrónica.

La implementación del repositorio llama Entity Framework métodos Async para trabajar con los datos, incluidas las consultas LINQ, así como para las operaciones de inserción, actualización y eliminación. Este es un ejemplo del código para buscar una tarea corregir ti.

[!code-csharp[Main](data-storage-options/samples/sample5.cs)]

Observará que también hay algún código de registro de errores y de temporización aquí, veremos esto más adelante en el [capítulo de supervisión y telemetría](monitoring-and-telemetry.md).

<a id="sqliaas"></a>
## <a name="choosing-sql-database-paas-versus-sql-server-in-a-vm-iaas-in-azure"></a>Elección de SQL Database (PaaS) frente a SQL Server en una máquina virtual (IaaS) en Azure

Un buen aspecto sobre SQL Server y Azure SQL Database es que el modelo de programación principal para ambos es idéntico. Puede usar la mayoría de los conocimientos en ambos entornos. Incluso puede usar una base de datos de SQL Server en desarrollo y una instancia de SQL Database en la nube, que es cómo se configura la aplicación de corrección de ti.

Como alternativa, puede ejecutar el mismo SQL Server en la nube que ejecuta de forma local mediante su instalación en máquinas virtuales IaaS. En el caso de algunas aplicaciones heredadas, la ejecución de SQL Server en una máquina virtual puede ser una solución mejor. Dado que una base de datos de SQL Server se ejecuta en una máquina virtual dedicada, tiene más recursos disponibles que una base de datos SQL Database que se ejecuta en un servidor compartido. Esto significa que una base de datos de SQL Server puede ser más grande y seguir funcionando bien. En general, cuanto menor sea el tamaño de la base de datos y el tamaño de la tabla, mejor será el caso de uso de SQL Database (PaaS).

Estas son algunas directrices sobre cómo elegir entre los dos modelos.

| Azure SQL Database (PaaS) | SQL Server en una máquina virtual (IaaS) |
| --- | --- |
| **Ventajas** : no tiene que crear ni administrar máquinas virtuales, actualizar o revisar os o SQL. Azure lo hace automáticamente. -Alta disponibilidad integrada, con un acuerdo de nivel de servicio de nivel de base de datos. -Costo total de propiedad (TCO) bajo, ya que solo paga por lo que usa (no se requiere licencia). -Bueno para controlar un gran número de bases de datos más pequeñas (&lt;= 500 GB cada una). -Fácil de crear dinámicamente nuevas bases de datos para habilitar el escalado horizontal. | ***Ventajas*** : características compatibles con la SQL Server local. -Puede implementar SQL Server [alta disponibilidad a través de AlwaysOn](https://www.microsoft.com/sqlserver/solutions-technologies/mission-critical-operations/high-availability.aspx) en 2 + máquinas virtuales, con un acuerdo de nivel de servicio de nivel de VM. -Tiene control completo sobre cómo se administra SQL. -Puede volver a usar las licencias de SQL que ya posee o pagar por una hora. -Bueno para controlar menos bases de datos de más de (1 TB +). |
| **Inconvenientes** : algunos huecos de características en comparación con los SQL Server locales (falta de [integración con CLR](https://technet.microsoft.com/library/ms131102.aspx), [TDE](https://technet.microsoft.com/library/bb934049.aspx), [compatibilidad de compresión](https://technet.microsoft.com/library/cc280449.aspx), [SQL Server Reporting Services](https://technet.microsoft.com/library/ms159106.aspx), etc.)-límite de tamaño de la base de datos de 500 GB. | ***Inconvenientes*** : las actualizaciones y revisiones (sistema operativo y SQL) son su responsabilidad: la creación y administración de bases de datos son su responsabilidad: IOPS de disco (operaciones de entrada/salida por segundo) limitadas a aproximadamente 8000 (a través de 16 unidades de datos). |

Si desea usar SQL Server en una máquina virtual, puede usar su propia licencia de SQL Server o puede pagar por una hora. Por ejemplo, en el portal o a través de la API de REST, puede crear una nueva máquina virtual mediante una imagen de SQL Server.

![Creación de una máquina virtual con SQL Server](data-storage-options/_static/image16.png)

![Lista de imágenes de máquina virtual SQL Server](data-storage-options/_static/image17.png)

Al crear una máquina virtual con una imagen de SQL Server, prorrateamos el costo de la licencia de SQL Server por la hora en función del uso de la máquina virtual. Si tiene un proyecto que solo va a ejecutarse durante un par de meses, es más barato pagar por la hora. Si cree que el proyecto va a durar años, es más barato comprar la licencia de la manera habitual.

## <a name="summary"></a>Resumen

La informática en la nube hace que sea práctico mezclar y hacer coincidir los enfoques de almacenamiento de datos para ajustarse mejor a las necesidades de la aplicación. Si va a crear una nueva aplicación, piense detenidamente en las preguntas que se enumeran aquí para elegir los enfoques que seguirán funcionando bien cuando crezca la aplicación. En el [siguiente capítulo](data-partitioning-strategies.md) se explican algunas estrategias de particionamiento que puede usar para combinar varios enfoques de almacenamiento de datos.

## <a name="resources"></a>Recursos

Para obtener más información, vea los siguientes recursos.

Elección de una plataforma de base de datos:

- [Acceso a datos para soluciones altamente escalables: uso de la persistencia de SQL, NoSQL y polyglot](https://aka.ms/dag-doc). Libro electrónico de Microsoft Patterns and Practices que profundiza en los diferentes tipos de almacenes de datos disponibles para las aplicaciones en la nube.
- [Patrones y prácticas de Microsoft: Guía de Azure](https://msdn.microsoft.com/library/ff898430.aspx). Vea información general sobre la coherencia de datos, la guía de sincronización y replicación de datos, el patrón de tabla de índice, el patrón de vista materializada.
- [Base: una alternativa acid](http://queue.acm.org/detail.cfm?id=1394128). Artículo sobre el equilibrio entre la coherencia de datos y la escalabilidad.
- [Siete bases de datos en siete semanas: una guía de las bases de datos modernas y el movimiento NoSQL](https://www.amazon.com/Seven-Databases-Weeks-Modern-Movement/dp/1934356921). Libro de Eric Redmond y Jim R. Wilson. Muy recomendable para introducirse en el rango de plataformas de almacenamiento de datos disponibles hoy en día.

Elección entre SQL Server y SQL Database:

- [Vista previa de Premium para instrucciones de SQL Database](https://msdn.microsoft.com/library/windowsazure/dn369873.aspx). Introducción a SQL Database Premium y orientación sobre cuándo elegirlo en las ediciones web y Business de SQL Database.
- [Instrucciones y limitaciones (Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx). Página del portal que se vincula a la documentación sobre las limitaciones de SQL Database, incluida una que se centra en SQL Server características que SQL Database no admite.
- [SQL Server en Azure virtual machines](https://msdn.microsoft.com/library/windowsazure/jj823132.aspx). Página del portal que vincula a la documentación sobre la ejecución de SQL Server en Azure.
- [Scott Guthrie explica las bases de datos SQL en Azure](https://azure.microsoft.com/documentation/videos/sql-in-azure-scottgu/). vídeo de 6 minutos de introducción a SQL Database de Scott Guthrie.
- [Patrones de aplicación y estrategias de desarrollo para SQL Server en Azure virtual machines](https://msdn.microsoft.com/library/windowsazure/dn574746.aspx).

Uso de Entity Framework y SQL Database en una aplicación Web de ASP.NET

- [Introducción con EF 6 con MVC 5](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Serie de tutoriales de nueve partes que le guía a través de la creación de una aplicación MVC que usa EF e implementa la base de datos en Azure y SQL Database.
- [Implementación web de ASP.NET con Visual Studio](../../../../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). Serie de tutoriales de doce partes que profundizan en la implementación de una base de datos mediante EF Code First.
- [Implemente una aplicación ASP.NET MVC 5 segura con suscripción, OAuth y SQL Database en un sitio web de Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/). Tutorial paso a paso que le guía en la creación de una aplicación web que usa la autenticación de, almacena tablas de aplicación en la base de datos de pertenencia, modifica el esquema de la base de datos e implementa la aplicación en Azure.
- [Mapa de contenido de ASP.net Data Access](https://go.microsoft.com/fwlink/p/?LinkId=282414). Vínculos a recursos para trabajar con EF y SQL Database.

Uso de MongoDB en Azure:

- [MongoLab-MongoDB en Azure](http://msopentech.com/opentech-projects/mongolab-mongodb-on-windows-azure/). Página del portal para obtener documentación sobre cómo ejecutar MongoDB en Azure.
- [Cree un sitio web de Azure que se conecte a MongoDB que se ejecuta en una máquina virtual de Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-store-data-mongodb-vm/). Tutorial paso a paso que muestra cómo usar una base de datos de MongoDB en una aplicación Web ASP.NET.

HDInsight (Hadoop en Azure):

- [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/). Portal a la documentación de HDInsight en el sitio web de [Azure](https://azure.microsoft.com/) .
- [Hadoop y HDInsight: Big Data en Azure](https://msdn.microsoft.com/magazine/dn385705.aspx). Artículo de MSDN Magazine de Bruno Terkaly y Ricardo Villalobos, en el que se presenta Hadoop en Azure.
- [Patrones y prácticas de Microsoft: Guía de Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte patrón de MapReduce.

> [!div class="step-by-step"]
> [Anterior](single-sign-on.md)
> [Siguiente](data-partitioning-strategies.md)
