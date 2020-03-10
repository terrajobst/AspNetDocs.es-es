---
uid: mvc/overview/older-versions-1/overview/asp-net-mvc-overview
title: Información general de ASP.NET MVC | Microsoft Docs
author: microsoft
description: Obtenga información sobre las diferencias entre las aplicaciones ASP.NET MVC y ASP.NET Web Forms. Aprenda a decidir cuándo compilar una aplicación ASP.NET MVC.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 2dcb44a4-5cbf-4d62-b363-718104082d86
msc.legacyurl: /mvc/overview/older-versions-1/overview/asp-net-mvc-overview
msc.type: authoredcontent
ms.openlocfilehash: 73965c71f37de13e3813df089a253fde528ea7ee
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78435493"
---
# <a name="aspnet-mvc-overview"></a>Información general sobre ASP.NET MVC

por [Microsoft](https://github.com/microsoft)

> Obtenga información sobre las diferencias entre las aplicaciones ASP.NET MVC y ASP.NET Web Forms. Aprenda a decidir cuándo compilar una aplicación ASP.NET MVC.

El modelo de arquitectura Model-View-Controller (MVC) separa una aplicación en tres componentes principales: el modelo, la vista y el controlador. El marco de MVC de ASP.NET proporciona una alternativa al patrón de formularios Web Forms de ASP.NET para crear aplicaciones web basadas en MVC. El marco de ASP.NET MVC es un marco de presentación de poca complejidad y fácil de comprobar que (como las aplicaciones basadas en formularios Web Forms) se integra con las características de ASP.NET existentes, tales como páginas maestras y la autenticación basada en pertenencia. El marco de MVC se define en el espacio de nombres **System. Web. Mvc** y es una parte fundamental y compatible del espacio de nombres **System. Web** .   
  
MVC es un modelo de diseño estándar con el que están familiarizados muchos desarrolladores. Algunos tipos de aplicaciones web salen beneficiadas con el marco de MVC. Otras seguirán usando el modelo de la aplicación ASP.NET tradicional que está basado en formularios Web Forms y devoluciones. Otros tipos de aplicaciones web combinarán las dos estrategias; una no excluye a la otra.   
  
El marco de MVC incluye los componentes siguientes:

[![invocar una acción de controlador que espera un valor de parámetro](asp-net-mvc-overview/_static/image1.jpg)](asp-net-mvc-overview/_static/image1.png)

**Figura 01**: invocación de una acción de controlador que espera un valor de parámetro ([haga clic para ver la imagen de tamaño completo](asp-net-mvc-overview/_static/image2.png))

- **Modelos**. Los objetos de modelo son las partes de la aplicación que implementan la lógica para el dominio de datos de la aplicación. A menudo, los objetos de modelo recuperan y almacenan el estado del modelo en una base de datos. Por ejemplo, un objeto Product podría recuperar información de una base de datos, trabajar en ella y, a continuación, escribir la información actualizada de nuevo en una tabla Products en SQL Server.

En las aplicaciones pequeñas, el modelo es a menudo una separación conceptual en lugar de física. Por ejemplo, si la aplicación solo Lee un conjunto de datos y lo envía a la vista, la aplicación no tiene una capa de modelo físico y las clases asociadas. En ese caso, el conjunto de datos toma el rol de un objeto de modelo.

- **Vistas**. Las vistas son los componentes que muestran la interfaz de usuario (UI) de la aplicación. Normalmente, esta interfaz de usuario se crea a partir de los datos de modelo. Un ejemplo sería una vista de edición de una tabla Products que muestra cuadros de texto, listas desplegables y casillas en función del estado actual de un objeto Products.

- **Controladores**. Los controladores son los componentes que controlan la interacción del usuario, trabajan con el modelo y por último seleccionan una vista para representar la interfaz de usuario. En una aplicación de MVC, la vista solo muestra información; el controlador controla y responde a la interacción y los datos que introducen los usuarios. Por ejemplo, el controlador controla los valores de la cadena de consulta y pasa estos valores al modelo, que a su vez consulta la base de datos utilizando los valores de.

El modelo de MVC le ayuda a crear aplicaciones que separan los diferentes aspectos de la aplicación (lógica de entrada, lógica comercial y lógica de la interfaz de usuario), a la vez que proporciona un vago acoplamiento entre estos elementos. El modelo especifica dónde se debería encontrar cada tipo de lógica en la aplicación. La lógica de la interfaz de usuario pertenece a la vista. La lógica de entrada pertenece al controlador. La lógica de negocios pertenece al modelo. Esta separación le ayuda a administrar la complejidad al compilar una aplicación, ya que le permite centrarse en cada momento en un único aspecto de la implementación. Por ejemplo, se puede centrar en la vista sin estar condicionado por la lógica comercial.   
  
Además de administrar la complejidad, el modelo de MVC hace que sea más fácil probar las aplicaciones que probar una aplicación web ASP.NET basada en formularios Web Forms. Por ejemplo, en una aplicación web ASP.NET basada en formularios Web Forms, se usa una clase única para mostrar la salida y para responder a los datos proporcionados por el usuario. Escribir pruebas automatizadas para las aplicaciones ASP.NET basadas en formularios Web Forms puede ser complejo, ya que para probar una página individual se deben crear instancias de la clase de página, todos sus controles secundarios y las clases dependientes adicionales de la aplicación. Dado que se crean instancias de tantas clases para ejecutar la página, puede ser difícil escribir pruebas que se centren exclusivamente en partes individuales de la aplicación. Las pruebas para las aplicaciones ASP.NET basadas en formularios Web Forms pueden ser por consiguiente más difíciles de implementar que las pruebas de una aplicación MVC. Es más, las pruebas en una aplicación ASP.NET basada en formularios Web Forms requieren un servidor web. El marco de MVC desacopla los componentes y hace un uso intensivo de las interfaces, lo cual hace posible probar los componentes individuales aislados del resto del marco.   
  
El acoplamiento vago entre los tres componentes principales de una aplicación MVC también favorece el desarrollo paralelo. Por ejemplo, un desarrollador puede trabajar en la vista, un segundo desarrollador puede trabajar en la lógica del controlador y un tercer desarrollador puede centrarse en la lógica de negocios del modelo.

## <a name="deciding-when-to-create-an-mvc-application"></a>Decidir cuándo crear una aplicación MVC

Debe considerar cuidadosamente si desea implementar una aplicación web mediante el marco de ASP.NET MVC o el modelo de formularios Web Forms de ASP.NET. El marco de MVC no reemplaza el modelo de formularios Web Forms; puede usar cualquiera de los dos marcos para las aplicaciones web. (Si ya tiene aplicaciones basadas en formularios Web Forms, estas seguirán funcionando exactamente igual que siempre).   
  
Antes de decidir usar el marco de MVC o el modelo de formularios Web Forms para un sitio web concreto, sopese las ventajas de cada método.

### <a name="advantages-of-an-mvc-based-web-application"></a>Ventajas de una aplicación web basada en MVC

El marco de ASP.NET MVC ofrece las ventajas siguientes:

- Facilita la administración de la complejidad, al dividir una aplicación en el modelo, la vista y el controlador.
- No usa el estado de vista ni formularios basados en servidor. Esto hace que el marco de MVC sea ideal para los desarrolladores que deseen un control completo sobre el comportamiento de una aplicación.
- Usa un modelo de controlador frontal que procesa las solicitudes de la aplicación web a través de un controlador único. Esto permite diseñar una aplicación que admite una infraestructura de enrutamiento avanzada. Para obtener más información, vea [front controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Controlador frontal") en el sitio web de MSDN.
- Proporciona una mayor compatibilidad con el desarrollo basado en pruebas (TDD).
- Funciona bien para las aplicaciones web que son compatibles con los grandes equipos de desarrolladores y diseñadores web que necesitan un alto grado de control sobre el comportamiento de la aplicación.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Ventajas de una aplicación web basada en formularios Web Forms

El marco basado en formularios Web Forms ofrece las ventajas siguientes:

- Admite un modelo de eventos que conserva el estado sobre HTTP, lo cual favorece al desarrollo de la aplicación web de línea de negocio. La aplicación basada en formularios Web Forms proporciona docenas de eventos que se admiten en centenares de controles de servidor.
- Usa un modelo de controlador de página que agrega funcionalidad a las páginas individuales. Para obtener más información, vea [controlador de páginas](https://go.microsoft.com/fwlink/?LinkId=106359 "Controlador de páginas") en el sitio web de MSDN.
- Usa el estado de vista o los formularios basados en servidor, lo que puede facilitar la administración de la información de estado.
- Funciona bien para los equipos pequeños de desarrolladores web y los diseñadores que deseen aprovechar el gran número de componentes disponible para el desarrollo rápido de aplicaciones.
- En general, es menos complejo para el desarrollo de aplicaciones, porque los componentes (la clase de **Página** , los controles, etc.) están estrechamente integrados y normalmente requieren menos código que el modelo de MVC.

## <a name="features-of-the-aspnet-mvc-framework"></a>Características del marco de ASP.NET MVC

El marco de ASP.NET MVC ofrece las características siguientes:

- Separación de las tareas de la aplicación (lógica de entrada, lógica de negocios y lógica de la interfaz de usuario), capacidad de prueba y desarrollo controlado por pruebas (TDD) de forma predeterminada. Todos los contratos principales del marco de MVC se basan en interfaz y se pueden probar mediante objetos ficticios, que son objetos simulados que imitan el comportamiento de objetos reales en la aplicación. Puede hacer una prueba unitaria de la aplicación sin tener que ejecutar los controladores en un proceso de ASP.NET, lo cual hace que las pruebas unitarias sean rápidas y flexibles. Puede usar cualquier marco de pruebas unitarias que sea compatible con .NET Framework.
- Un marco extensible y conectable. Los componentes del marco de ASP.NET MVC están diseñados para que se puedan reemplazar o personalizar con facilidad. Puede conectar su propio motor de vista, directiva de enrutamiento de URL, serialización de parámetros de método y acción, y otros componentes. El marco de ASP.NET MVC también admite el uso de los modelos de contenedor Inyección de dependencia (DI) e Inversión de control (IOC). DI permite insertar objetos en una clase, en lugar de confiar en la clase para crear el propio objeto. IOC especifica que si un objeto requiere otro objeto, el primer objeto debe obtener el segundo objeto de un origen externo como un archivo de configuración. Esto facilita las pruebas.
- Un eficaz componente de asignación de URL que le permite compilar aplicaciones que tienen direcciones URL comprensibles y que se pueden buscar. Las direcciones URL no tienen que incluir las extensiones de los nombres de archivo y están diseñadas para admitir patrones de nombres de direcciones URL que funcionan bien para la optimización del motor de búsqueda (SEO) y el direccionamiento de transferencia de estado representacional (REST, Representational State Transfer).
- Compatibilidad para usar el marcado en archivos de marcado de páginas de ASP.NET existentes (archivos .aspx), de controles de usuario (archivos .ascx) y de páginas maestras (archivos .master) como plantillas de vista. Puede usar las características de ASP.NET existentes con el marco de ASP.NET MVC, como páginas maestras anidadas, expresiones en línea (&lt;% =%&gt;), controles de servidor declarativos, plantillas, enlace de datos, localización, etc.
- Compatibilidad con las características de ASP.NET existentes. ASP.NET MVC le permite usar características, tales como la autenticación de formularios y la autenticación de Windows, la autorización para URL, la pertenencia y los roles, el almacenamiento en caché de resultados y datos, la administración de estados de sesión y perfil, el seguimiento de estado, el sistema de configuración y la arquitectura de proveedor.
