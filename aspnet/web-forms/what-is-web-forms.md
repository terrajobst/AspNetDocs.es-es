---
uid: web-forms/what-is-web-forms
title: ¿Qué son los formularios Web Forms? Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 02/21/2014
ms.assetid: 5fa1daf9-1161-4cfa-bd4c-658f48b2c229
msc.legacyurl: /web-forms/what-is-web-forms
msc.type: content
ms.openlocfilehash: 19be419c499759713971a6c77674c924867d1bbc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78516925"
---
# <a name="what-is-web-forms"></a>Qué son los formularios Web Forms

Los formularios Web Forms ASP.NET forman parte del marco de trabajo de la aplicación Web ASP.NET y se incluyen con [Visual Studio](https://www.asp.net/downloads). Es uno de los cuatro modelos de programación que puede usar para crear aplicaciones Web de ASP.NET, mientras que las otras son ASP.NET MVC, ASP.NET Web Pages y ASP.NET de una sola página.

Los formularios Web Forms son páginas que los usuarios solicitan mediante el explorador. Estas páginas se pueden escribir utilizando una combinación de HTML, script de cliente, controles de servidor y código de servidor. Cuando los usuarios solicitan una página, se compilan y ejecutan en el servidor mediante el marco de trabajo y, a continuación, el marco de trabajo genera el marcado HTML que el explorador puede representar. Una página de formularios Web Forms ASP.NET presenta información al usuario en cualquier explorador o dispositivo cliente.

Con Visual Studio, puede crear formularios Web Forms de ASP.NET. El entorno de desarrollo integrado (IDE) de Visual Studio le permite arrastrar y colocar controles de servidor para diseñar la página de formularios Web Forms. Después, puede establecer fácilmente las propiedades, los métodos y los eventos de los controles de la página o de la propia página. Estas propiedades, métodos y eventos se utilizan para definir el comportamiento de la página web, la apariencia y el funcionamiento, etc. Para escribir código de servidor que controle la lógica de la página, puede usar un lenguaje .NET como Visual Basic C#o.

> [!NOTE] 
> 
> La documentación de ASP.NET y Visual Studio abarca varias versiones. Los temas que resaltan las características de versiones anteriores pueden ser útiles para sus tareas y escenarios actuales con las versiones más recientes.

**Los formularios Web Forms de ASP.NET son:** 

- Basado en la tecnología de Microsoft ASP.NET, en la que el código que se ejecuta en el servidor genera dinámicamente el resultado de la página web en el explorador o el dispositivo cliente.
- Compatible con cualquier explorador o dispositivo móvil. Una página Web ASP.NET representa automáticamente el código HTML compatible con el explorador correcto para características como estilos, diseño, etc.
- Compatible con cualquier lenguaje compatible con .NET Common Language Runtime, como Microsoft Visual Basic y Microsoft Visual C#.
- Basado en el marco de Microsoft .NET. Esto proporciona todas las ventajas del marco, incluido un entorno administrado, seguridad de tipos y herencia.
- Flexible porque puede agregarle controles de terceros y creados por el usuario.

**Oferta de formularios Web Forms ASP.NET:** 

- Separación de HTML y otro código de interfaz de usuario de la lógica de la aplicación.
- Un amplio conjunto de controles de servidor para tareas comunes, incluido el acceso a datos.
- Enlace de datos eficaz, con una excelente compatibilidad con las herramientas.
- Compatibilidad con scripting del lado cliente que se ejecuta en el explorador.
- Compatibilidad con otras funciones, como el enrutamiento, la seguridad, el rendimiento, la internacionalización, las pruebas, la depuración, el control de errores y la administración de estado.

## <a name="aspnet-web-forms-helps-you-overcome-challenges"></a>Los formularios Web Forms de ASP.NET le ayudan a superar los desafíos

La programación de aplicaciones web presenta desafíos que normalmente no surgen al programar aplicaciones tradicionales basadas en cliente. Entre los desafíos se encuentran:

- **Implementar una interfaz de usuario Web enriquecida** : puede ser difícil y tedioso diseñar e implementar una interfaz de usuario mediante el uso de recursos HTML básicos, especialmente si la página tiene un diseño complejo, una gran cantidad de contenido dinámico y objetos interactivos de usuario con todas las características.
- **Separación del cliente y del servidor** : en una aplicación Web, el cliente (explorador) y el servidor son programas diferentes que se ejecutan a menudo en equipos diferentes (e incluso en sistemas operativos diferentes). Por lo tanto, las dos mitades de la aplicación comparten muy poca información; pueden comunicarse, pero normalmente solo intercambian pequeños fragmentos de información simple.
- **Ejecución sin estado** : cuando un servidor web recibe una solicitud de una página, encuentra la página, la procesa, la envía al explorador y, a continuación, descarta toda la información de la página. Si el usuario solicita la misma página de nuevo, el servidor repite toda la secuencia y vuelve a procesar la página desde cero. Otra manera, un servidor no tiene memoria de páginas que ha procesado, la página no tiene estado. Por lo tanto, si una aplicación necesita mantener información acerca de una página, su naturaleza sin estado puede convertirse en un problema.
- **Capacidades de cliente desconocidas** : en muchos casos, se puede acceder a las aplicaciones web a muchos usuarios que usan exploradores diferentes. Los exploradores tienen capacidades diferentes, lo que dificulta la creación de una aplicación que se ejecutará igualmente correctamente en todos ellos.
- **Complicaciones con el acceso a datos** : la lectura y la escritura en un origen de datos en las aplicaciones web tradicionales pueden ser complicadas y que consumen muchos recursos.
- **Complicaciones con la escalabilidad** : en muchos casos, las aplicaciones Web diseñadas con métodos existentes no cumplen los objetivos de escalabilidad debido a la falta de compatibilidad entre los distintos componentes de la aplicación. Suele ser un punto de error común para las aplicaciones en un ciclo de crecimiento pesado.

Cumplir estos desafíos para las aplicaciones web puede requerir mucho tiempo y esfuerzo. Los formularios Web Forms de ASP.NET y ASP.NET Framework abordan estos desafíos de las siguientes maneras:

- **Modelo de objetos intuitivo y coherente** : el marco de trabajo de la página ASP.net presenta un modelo de objetos que le permite pensar en los formularios como una unidad, no en partes independientes del cliente y del servidor. En este modelo, puede programar la página de una forma más intuitiva que en las aplicaciones web tradicionales, incluida la capacidad de establecer las propiedades de los elementos de página y responder a los eventos. Además, los controles de servidor ASP.NET son una abstracción del contenido físico de una página HTML y de la interacción directa entre el explorador y el servidor. En general, puede usar controles de servidor de la forma en que puede trabajar con controles en una aplicación cliente y no tener que pensar en cómo crear el código HTML para presentar y procesar los controles y su contenido.
- **Modelo de programación** basado en eventos: los formularios web forms ASP.net proporcionan a las aplicaciones web el modelo conocido de escritura de controladores de eventos para los eventos que se producen en el cliente o el servidor. El marco de trabajo de la página ASP.NET abstrae este modelo de forma que el mecanismo subyacente para capturar un evento en el cliente, transmitirlo al servidor y llamar al método adecuado es todo lo que se puede hacer de forma automática e invisible. El resultado es una estructura de código clara y escrita fácilmente que admite el desarrollo controlado por eventos.
- **Administración de estado intuitiva** : el marco de trabajo de la página ASP.net controla automáticamente la tarea de mantener el estado de la página y sus controles, y proporciona métodos explícitos para mantener el estado de la información específica de la aplicación. Esto se logra sin un uso intensivo de los recursos del servidor y se puede implementar con o sin enviar cookies al explorador.
- **Aplicaciones independientes del explorador** : el marco de trabajo de la página ASP.net permite crear toda la lógica de la aplicación en el servidor, lo que elimina la necesidad de codificar explícitamente las diferencias en los exploradores. Sin embargo, aún le permite aprovechar las características específicas del explorador escribiendo código del lado cliente para proporcionar un rendimiento mejorado y una experiencia de cliente más enriquecida.
- **Compatibilidad con .NET Framework Common Language Runtime** : el marco de trabajo de páginas de ASP.net se basa en el .NET Framework, por lo que todo el marco está disponible para cualquier aplicación de ASP.net. Las aplicaciones se pueden escribir en cualquier lenguaje que sea compatible con el motor en tiempo de ejecución. Además, el acceso a los datos se simplifica mediante la infraestructura de acceso a datos proporcionada por el .NET Framework, incluido ADO.NET.
- **.NET Framework rendimiento de servidor escalable** : el marco de trabajo de páginas de ASP.net permite escalar la aplicación web desde un equipo con un solo procesador a una granja de servidores Web de varios equipos sin problemas y sin complicados cambios en la lógica de la aplicación.

## <a name="features-of-aspnet-web-forms"></a>Características de formularios Web Forms de ASP.NET

- **Controles de servidor**: los controles de servidor Web ASP.net son objetos en páginas web de ASP.net que se ejecutan cuando se solicita la página y que representan el marcado en el explorador. Muchos controles de servidor web son similares a los elementos HTML conocidos, como botones y cuadros de texto. Otros controles incluyen un comportamiento complejo, como controles de calendario, y controles que puede usar para conectarse a orígenes de datos y Mostrar datos.
- **Páginas maestras**: las páginas maestras de ASP.net le permiten crear un diseño coherente para las páginas de la aplicación. Una sola página maestro define la apariencia y el comportamiento estándar que desea para todas las páginas (o un grupo de páginas) en su aplicación. Después, puede crear páginas de contenido individuales con el contenido que desee mostrar. Cuando los usuarios solicitan las páginas de contenido, se combinan con la página maestra para producir una salida que combina el diseño de la página maestra con el contenido de la página de contenido.
- **Trabajar con datos**: ASP.net proporciona muchas opciones para almacenar, recuperar y Mostrar datos. En una aplicación de formularios Web Forms ASP.NET, se usan controles enlazados a datos para automatizar la presentación o la entrada de datos en elementos de la interfaz de usuario de la página web, como tablas y cuadros de texto y listas desplegables.
- **Pertenencia**: ASP.net Identity almacena las credenciales de los usuarios en una base de datos creada por la aplicación. Cuando los usuarios inician sesión, la aplicación valida las credenciales mediante la lectura de la base de datos. La carpeta de la *cuenta* del proyecto contiene los archivos que implementan las distintas partes de la pertenencia: registro, Inicio de sesión, cambio de contraseña y autorización de acceso. Además, los formularios Web Forms de ASP.NET admiten OAuth y OpenID. Estas mejoras de autenticación permiten a los usuarios iniciar sesión en el sitio con las credenciales existentes, desde cuentas como Facebook, Twitter, Windows Live y Google. De forma predeterminada, la plantilla crea una base de datos de pertenencia con un nombre de base de datos predeterminado en una instancia de SQL Server Express LocalDB, el servidor de base de datos de desarrollo que viene con Visual Studio Express 2013 para Web.
- **Scripts de cliente y marcos de cliente**: puede mejorar las características basadas en servidor de ASP.net incluyendo la funcionalidad de scripts de cliente en páginas de formularios web forms de ASP.net. Puede usar el script de cliente para proporcionar a los usuarios una interfaz de usuario más enriquecida y con mayor capacidad de respuesta. También puede usar el script de cliente para realizar llamadas asincrónicas al servidor web mientras se ejecuta una página en el explorador.
- **Enrutamiento: el**enrutamiento de direcciones URL permite configurar una aplicación para que acepte direcciones URL de solicitud que no se asignan a archivos físicos. Una dirección URL de solicitud es simplemente la dirección URL que un usuario escribe en el explorador para buscar una página en el sitio Web. El enrutamiento se usa para definir direcciones URL semánticamente significativas para los usuarios y que pueden ayudar con la optimización del motor de búsqueda (SEO).
- **Administración de Estados**: ASP.NET Web Forms incluye varias opciones que le ayudan a conservar los datos en cada página y en toda la aplicación.
- **Seguridad**: una parte importante del desarrollo de una aplicación más segura es comprender las amenazas. Microsoft ha desarrollado una manera de categorizar amenazas: suplantación de identidad, manipulación, rechazo, divulgación de información, denegación de servicio, elevación de privilegios (STRIde). En los formularios Web Forms de ASP.NET, puede Agregar puntos de extensibilidad y opciones de configuración que le permitan personalizar distintos comportamientos de seguridad en los formularios Web Forms de ASP.NET.
- **Rendimiento**: el rendimiento puede ser un factor clave en un proyecto o sitio web correcto. Los formularios Web Forms de ASP.NET permiten modificar el rendimiento relacionado con el procesamiento de controles de páginas y servidores, la administración de Estados, el acceso a datos, la configuración y carga de aplicaciones y las prácticas de codificación eficientes.
- Los formularios Web de **internacionalización**-ASP.net permiten crear páginas web que pueden obtener contenido y otros datos en función de la configuración de idioma preferida para el explorador o de acuerdo con la elección explícita del idioma del usuario. El contenido y otros datos se conocen como recursos y estos datos se pueden almacenar en archivos de recursos u otros orígenes. En una página de formularios Web Forms de ASP.NET, configure los controles para obtener sus valores de propiedad de los recursos. En tiempo de ejecución, las expresiones de recursos se reemplazan por recursos del archivo de recursos localizado adecuado.
- **Depuración y control de errores**: ASP.net incluye características para ayudarle a diagnosticar los problemas que pueden surgir en la aplicación de formularios Web Forms. La depuración y el control de errores se admiten en los formularios Web Forms de ASP.NET para que las aplicaciones se compilen y se ejecuten de forma eficaz.
- **Implementación y hospedaje**: Visual Studio, ASP.net, Azure e IIS proporcionan herramientas que le ayudarán en el proceso de implementación y hospedaje de su aplicación de formularios Web Forms.

## <a name="deciding-when-to-create-a-web-forms-application"></a>Decidir cuándo crear una aplicación de formularios Web Forms

Debe considerar detenidamente si desea implementar una aplicación Web mediante el modelo de formularios Web Forms de ASP.NET o cualquier otro modelo, como el marco de MVC de ASP.NET. El marco de MVC no reemplaza el modelo de formularios Web Forms; puede usar cualquiera de los dos marcos para las aplicaciones web. Antes de decidirse a usar el modelo de formularios Web Forms o el marco MVC para un sitio web concreto, sopese las ventajas de cada enfoque.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Ventajas de una aplicación web basada en formularios Web Forms

El marco basado en formularios Web Forms ofrece las ventajas siguientes:

- Admite un modelo de eventos que conserva el estado sobre HTTP, lo cual favorece al desarrollo de la aplicación web de línea de negocio. La aplicación basada en formularios Web Forms proporciona docenas de eventos que se admiten en centenares de controles de servidor.
- Usa un modelo de controlador de página que agrega funcionalidad a las páginas individuales. Para obtener más información, vea [controlador de páginas](https://go.microsoft.com/fwlink/?LinkId=106359 "Controlador de páginas") en el sitio web de MSDN.
- Usa el estado de vista o los formularios basados en servidor, lo que puede facilitar la administración de la información de estado.
- Funciona bien para los equipos pequeños de desarrolladores web y los diseñadores que deseen aprovechar el gran número de componentes disponible para el desarrollo rápido de aplicaciones.
- En general, es menos complejo para el desarrollo de aplicaciones, porque los componentes (la clase de **Página** , los controles, etc.) están estrechamente integrados y normalmente requieren menos código que el modelo de MVC.

### <a name="advantages-of-an-mvc-based-web-application"></a>Ventajas de una aplicación web basada en MVC

El marco de ASP.NET MVC ofrece las ventajas siguientes:

- Facilita la administración de la complejidad, al dividir una aplicación en el modelo, la vista y el controlador.
- No usa el estado de vista ni formularios basados en servidor. Esto hace que el marco de MVC sea ideal para los desarrolladores que deseen un control completo sobre el comportamiento de una aplicación.
- Usa un modelo de controlador frontal que procesa las solicitudes de la aplicación web a través de un controlador único. Esto permite diseñar una aplicación que admite una infraestructura de enrutamiento avanzada. Para obtener más información, vea [front controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Controlador frontal") en el sitio web de MSDN.
- Proporciona una mayor compatibilidad con el desarrollo basado en pruebas (TDD).
- Funciona bien para las aplicaciones web que son compatibles con los grandes equipos de desarrolladores y diseñadores web que necesitan un alto grado de control sobre el comportamiento de la aplicación.
