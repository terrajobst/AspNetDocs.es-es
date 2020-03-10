---
uid: web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-cs
title: Conceptos básicos de seguridad y compatibilidad conC#ASP.net () | Microsoft Docs
author: rick-anderson
description: Este es el primer tutorial de una serie de tutoriales que explorarán técnicas para autenticar a los visitantes a través de un formulario web, para autorizar el acceso a una parte...
ms.author: riande
ms.date: 01/13/2008
ms.assetid: 07e15538-2f29-40c6-b2e7-e6115075ac83
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-cs
msc.type: authoredcontent
ms.openlocfilehash: 1ccaac101a83d0e28b07b220b8b7b61a9039227e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78520075"
---
# <a name="security-basics-and-aspnet-support-c"></a>Conceptos básicos de seguridad y compatibilidad de ASP.NET (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar PDF](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial01_Basics_cs.pdf)

> Este es el primer tutorial de una serie de tutoriales que explorarán técnicas para autenticar a los visitantes a través de un formulario web, para autorizar el acceso a determinadas páginas y funciones, y para administrar cuentas de usuario en una aplicación de ASP.NET.

## <a name="introduction"></a>Introducción

¿En qué se diferencian los foros, los sitios de comercio electrónico, los sitios web de correo electrónico en línea, los sitios web del portal y los sitios de red social? Todos ofrecen *cuentas de usuario*. Los sitios que ofrecen cuentas de usuario deben proporcionar una serie de servicios. Como mínimo, los nuevos visitantes deben poder crear una cuenta y devolver visitantes debe poder iniciar sesión. Estas aplicaciones Web pueden tomar decisiones basadas en el usuario que ha iniciado sesión: algunas páginas o acciones pueden estar restringidas a usuarios que solo tienen la sesión iniciada o a un determinado subconjunto de usuarios; otras páginas pueden mostrar información específica del usuario que ha iniciado sesión, o pueden mostrar más o menos información, en función del usuario que esté viendo la página.

Este es el primer tutorial de una serie de tutoriales que explorarán técnicas para autenticar a los visitantes a través de un formulario web, para autorizar el acceso a determinadas páginas y funciones, y para administrar cuentas de usuario en una aplicación de ASP.NET. En el transcurso de estos tutoriales, examinaremos cómo:

- Identificación e inicio de sesión de usuarios en un sitio web
- Utilice ASP. Marco de pertenencia de la red para administrar cuentas de usuario
- Crear, actualizar y eliminar cuentas de usuario
- Limitar el acceso a una página web, un directorio o una funcionalidad específica basada en el usuario que ha iniciado sesión
- Utilice ASP. Marco de trabajo de roles de la red para asociar cuentas de usuario a roles
- Administrar los roles de usuario
- Limitar el acceso a una página web, un directorio o una funcionalidad específica según el rol del usuario que ha iniciado sesión
- Personalizar y ampliar ASP. Controles Web de seguridad de la red

Estos tutoriales están orientados a ser concisos y proporcionan instrucciones paso a paso con muchas capturas de pantalla para guiarle en el proceso visualmente. Cada tutorial está disponible en C# y Visual Basic versiones e incluye una descarga del código completo usado. (Este primer tutorial se centra en los conceptos de seguridad de un punto de vista de alto nivel y, por tanto, no contiene ningún código asociado).

En este tutorial se tratarán importantes conceptos de seguridad y qué recursos están disponibles en ASP.NET para ayudar en la implementación de la autenticación de formularios, la autorización, las cuentas de usuario y los roles. Comencemos.

> [!NOTE]
> La seguridad es un aspecto importante de cualquier aplicación que abarque las decisiones físicas, tecnológicas y de directivas y requiere un alto grado de planeación y conocimiento del dominio. Esta serie de tutoriales no está pensada como guía para desarrollar aplicaciones web seguras. En su lugar, se centra específicamente en la autenticación de formularios, la autorización, las cuentas de usuario y los roles. Aunque algunos conceptos de seguridad que giran en torno a estos problemas se tratan en esta serie, otros se dejan sin explorar.

## <a name="authentication-authorization-user-accounts-and-roles"></a>Autenticación, autorización, cuentas de usuario y roles

La autenticación, la autorización, las cuentas de usuario y los roles son cuatro términos que se usarán con mucha frecuencia a lo largo de esta serie de tutoriales, por lo que me gustaría tomar un momento rápido para definir estos términos en el contexto de la seguridad Web. En un modelo cliente-servidor, como Internet, hay muchos escenarios en los que el servidor necesita identificar el cliente que realiza la solicitud. La *autenticación* es el proceso de determinar la identidad del cliente. Se dice que un cliente que se ha identificado correctamente está *autenticado*. Se dice que un cliente no identificado no está *autenticado* o es *anónimo*.

Los sistemas de autenticación seguros incluyen al menos una de las siguientes tres aspectos: [algo que usted sabe, algo que tiene o algo que tiene](http://www.cs.cornell.edu/Courses/cs513/2005fa/NNLauthPeople.html). La mayoría de las aplicaciones web se basan en algo que el cliente conoce, como una contraseña o un PIN. La información que se usa para identificar un nombre de usuario y contraseña de usuario, por ejemplo, se conoce como *credenciales*. Esta serie de tutoriales se centra en la *autenticación de formularios*, que es un modelo de autenticación en el que los usuarios inician sesión en el sitio proporcionando sus credenciales en un formulario de página web. Todos hemos experimentado este tipo de autenticación antes. Vaya a cualquier sitio de comercio electrónico. Cuando esté listo para desproteger, se le pedirá que inicie sesión escribiendo su nombre de usuario y contraseña en los cuadros de texto de una página web.

Además de identificar los clientes, es posible que un servidor tenga que limitar qué recursos o funcionalidades son accesibles en función del cliente que realiza la solicitud. La *autorización* es el proceso de determinar si un usuario determinado tiene la autoridad para tener acceso a un recurso o una funcionalidad específicos.

Una *cuenta de usuario* es un almacén para conservar información sobre un usuario determinado. Las cuentas de usuario deben incluir mínimamente información que identifique de forma única al usuario, como el nombre de inicio de sesión y la contraseña del usuario. Junto con esta información esencial, las cuentas de usuario pueden incluir elementos como: la dirección de correo electrónico del usuario; fecha y hora de creación de la cuenta; fecha y hora en que se inició la sesión por última vez; nombre y apellidos; número de teléfono; y dirección de correo electrónico. Al utilizar la autenticación de formularios, la información de la cuenta de usuario se almacena normalmente en una base de datos relacional como Microsoft SQL Server.

Las aplicaciones web que admiten cuentas de usuario pueden agrupar los usuarios en *roles*de forma opcional. Un rol es simplemente una etiqueta que se aplica a un usuario y proporciona una abstracción para definir las reglas de autorización y la funcionalidad de nivel de página. Por ejemplo, un sitio web podría incluir un rol de administrador con reglas de autorización que prohíban a cualquier usuario, excepto un administrador, el acceso a un conjunto determinado de páginas Web. Además, una variedad de páginas que son accesibles para todos los usuarios (incluidos los que no son administradores) pueden mostrar datos adicionales u ofrecer funcionalidad adicional cuando lo visiten los usuarios del rol administradores. Mediante el uso de roles, podemos definir estas reglas de autorización en función de cada rol, en lugar de usuario por usuario.

## <a name="authenticating-users-in-an-aspnet-application"></a>Autenticación de usuarios en una aplicación de ASP.NET

Cuando un usuario escribe una dirección URL en la ventana de dirección del explorador o hace clic en un vínculo, el explorador realiza una solicitud de [Protocolo de transferencia de hipertexto (http)](http://en.wikipedia.org/wiki/HTTP) al servidor web para el contenido especificado, ya sea una página de ASP.net, una imagen, un archivo de JavaScript o cualquier otro tipo de contenido. El servidor Web tiene tareas que devuelven el contenido solicitado. Al hacerlo, debe determinar una serie de cosas sobre la solicitud, incluido quién realizó la solicitud y si la identidad está autorizada para recuperar el contenido solicitado.

De forma predeterminada, los exploradores envían solicitudes HTTP que carecen de cualquier tipo de información de identificación. Pero si el explorador incluye información de autenticación, el servidor Web inicia el flujo de trabajo de autenticación, que intenta identificar el cliente que realiza la solicitud. Los pasos del flujo de trabajo de autenticación dependen del tipo de autenticación utilizado por la aplicación Web. ASP.NET admite tres tipos de autenticación: Windows, Passport y Forms. Esta serie de tutoriales se centra en la autenticación de formularios, pero vamos a dedicar un minuto a comparar y contrastar los almacenes de usuarios y el flujo de trabajo de autenticación de Windows.

### <a name="authentication-via-windows-authentication"></a>Autenticación mediante autenticación de Windows

El flujo de trabajo de autenticación de Windows usa una de las técnicas de autenticación siguientes:

- Autenticación básica
- Autenticación implícita
- Autenticación integrada de Windows

Las tres técnicas funcionan aproximadamente de la misma manera: cuando llega una solicitud anónima no autorizada, el servidor Web devuelve una respuesta HTTP que indica que es necesaria la autorización para continuar. Después, el explorador muestra un cuadro de diálogo modal que solicita al usuario su nombre de usuario y contraseña (consulte la figura 1). A continuación, esta información se devuelve al servidor Web a través de un encabezado HTTP.

![Un cuadro de diálogo modal solicita al usuario sus credenciales](security-basics-and-asp-net-support-cs/_static/image1.png)

**Figura 1**: un cuadro de diálogo modal solicita al usuario sus credenciales

Las credenciales proporcionadas se validan en el almacén de usuario de Windows del servidor Web. Esto significa que cada usuario autenticado en la aplicación Web debe tener una cuenta de Windows en su organización. Esto es habitual en escenarios de intranet. De hecho, cuando se usa la autenticación integrada de Windows en una configuración de intranet, el explorador proporciona automáticamente el servidor Web con las credenciales usadas para iniciar sesión en la red, lo que suprime el cuadro de diálogo que aparece en la figura 1. Aunque la autenticación de Windows es excelente para las aplicaciones de intranet, normalmente no es factible para las aplicaciones de Internet, ya que no desea crear cuentas de Windows para cada uno de los usuarios que se suscriben a su sitio.

### <a name="authentication-via-forms-authentication"></a>Autenticación mediante la autenticación de formularios

La autenticación de formularios, por otro lado, es ideal para las aplicaciones Web de Internet. Recuerde que la autenticación mediante formularios identifica al usuario solicitando que escriba sus credenciales a través de un formulario Web. Por lo tanto, cuando un usuario intenta obtener acceso a un recurso no autorizado, se le redirige automáticamente a la página de inicio de sesión, donde puede escribir sus credenciales. Las credenciales enviadas se validan después con un almacén de usuario personalizado, normalmente una base de datos.

Después de comprobar las credenciales enviadas, se crea un *vale de autenticación de formularios* para el usuario. Este vale indica que el usuario se ha autenticado e incluye información de identificación, como el nombre de usuario. El vale de autenticación de formularios se almacena normalmente como una cookie en el equipo cliente. Por lo tanto, las visitas posteriores al sitio web incluyen el vale de autenticación de formularios en la solicitud HTTP, lo que permite que la aplicación web identifique al usuario una vez que haya iniciado sesión.

En la ilustración 2 se muestra el flujo de trabajo de autenticación de formularios desde un punto de estratégicos de alto nivel. Observe cómo las partes de autenticación y autorización de ASP.NET actúan como dos entidades independientes. El sistema de autenticación de formularios identifica al usuario (o informa de que son anónimos). El sistema de autorización es lo que determina si el usuario tiene acceso al recurso solicitado. Si el usuario no está autorizado (como se muestra en la figura 2 al intentar visitar de forma anónima ProtectedPage. aspx), el sistema de autorización notifica que el usuario está denegado, lo que hace que el sistema de autenticación de formularios redirija automáticamente al usuario a la página de inicio de sesión.

Una vez que el usuario ha iniciado sesión correctamente, las solicitudes HTTP subsiguientes incluyen el vale de autenticación de formularios. El sistema de autenticación de formularios simplemente identifica al usuario, es el sistema de autorización que determina si el usuario puede tener acceso al recurso solicitado.

![Flujo de trabajo de autenticación de formularios](security-basics-and-asp-net-support-cs/_static/image2.png)

**Figura 2**: flujo de trabajo de autenticación de formularios

Profundizaremos en la autenticación de formularios con mucha mayor detalle en los dos tutoriales siguientes,[una introducción a la autenticación de formularios](an-overview-of-forms-authentication-cs.md) y la [configuración de autenticación de formularios y temas avanzados](forms-authentication-configuration-and-advanced-topics-cs.md). Para obtener más información sobre ASP. Opciones de autenticación de red, consulte [autenticación de ASP.net](https://msdn.microsoft.com/library/eeyk640h.aspx).

## <a name="limiting-access-to-web-pages-directories-and-page-functionality"></a>Limitar el acceso a las páginas web, directorios y funcionalidad de página

ASP.NET incluye dos maneras de determinar si un usuario determinado tiene autoridad para tener acceso a un archivo o directorio específico:

- **Autorización de archivos** : como las páginas de ASP.net y los servicios web se implementan como archivos que residen en el sistema de archivos del servidor Web, el acceso a estos archivos puede especificarse mediante listas de Access Control (ACL). La autorización de archivos se usa normalmente con la autenticación de Windows, ya que las ACL son permisos que se aplican a las cuentas de Windows. Al utilizar la autenticación de formularios, todas las solicitudes de sistema operativo y de nivel de sistema de archivos se ejecutan con la misma cuenta de Windows, independientemente de que el usuario visite el sitio.
- **Autorización de dirección URL**: con autorización de URL, el desarrollador de páginas especifica las reglas de autorización en Web. config. Estas reglas de autorización especifican a qué usuarios o roles se les permite el acceso o se les deniega el acceso a determinadas páginas o directorios de la aplicación.

Autorización de archivos y autorización de URL define reglas de autorización para tener acceso a una página de ASP.NET determinada o para todas las páginas de ASP.NET en un directorio determinado. Con estas técnicas se puede indicar a ASP.NET que deniegue las solicitudes a una página determinada para un usuario determinado, o que permita el acceso a un conjunto de usuarios y deniegue el acceso a todos los demás. ¿Qué ocurre con los escenarios en los que todos los usuarios pueden tener acceso a la página, pero la funcionalidad de la página depende del usuario? Por ejemplo, muchos sitios que admiten cuentas de usuario tienen páginas que muestran contenido o datos diferentes para usuarios autenticados frente a usuarios anónimos. Un usuario anónimo podría ver un vínculo para iniciar sesión en el sitio, mientras que, en su lugar, un usuario autenticado verá un mensaje como, Bienvenido atrás, el *nombre de usuario* junto con un vínculo para cerrar la sesión. Otro ejemplo: al ver un elemento en un sitio de subastas, verá información diferente en función de si es un pujador o de la subasta del artículo.

Estos ajustes de nivel de página se pueden realizar de forma declarativa o mediante programación. Para mostrar contenido diferente para anónimos que los usuarios autenticados, simplemente arrastre un [control LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) a la página y escriba el contenido adecuado en sus plantillas AnonymousTemplate y LoggedInTemplate. Como alternativa, puede determinar mediante programación si la solicitud actual se ha autenticado, quién es el usuario y a qué roles pertenecen (si existe). Puede usar esta información para mostrar u ocultar las columnas en una cuadrícula o en los paneles de la página.

Esta serie incluye tres tutoriales que se centran en la autorización. La ***autorización basada***en el usuario examina cómo limitar el acceso a una página o páginas de un directorio para cuentas de usuario específicas; La ***autorización basada en roles*** examina el suministro de reglas de autorización en el nivel de rol. por último, el ***contenido que se muestra según el tutorial del usuario que ha iniciado sesión*** explora la modificación del contenido y la funcionalidad de una página determinada en función del usuario que visita la página. Para obtener más información sobre ASP. Las opciones de autorización de NET, consulte [autorización de ASP.net](https://msdn.microsoft.com/library/wce3kxhd.aspx).

## <a name="user-accounts-and-roles"></a>Roles y cuentas de usuario

ASP. La autenticación de formularios de la red proporciona una infraestructura para que los usuarios inicien sesión en un sitio y se recuerde su estado autenticado a través de visitas de página. Y la autorización de URL ofrece un marco para limitar el acceso a archivos o carpetas específicos en una aplicación ASP.NET. Sin embargo, ninguna de estas características proporciona un medio para almacenar información de cuentas de usuario o administrar roles.

Antes de ASP.NET 2,0, los desarrolladores eran responsables de crear sus propios almacenes de usuarios y roles. También estaban en el enlace para diseñar las interfaces de usuario y escribir el código para las páginas esenciales relacionadas con la cuenta de usuario, como la página de inicio de sesión y la página para crear una nueva cuenta, entre otras. Sin ningún marco de cuentas de usuario integrado en ASP.NET, cada desarrollador que implementa cuentas de usuario tenía que llegar a sus propias decisiones de diseño en preguntas como, Cómo almacenar contraseñas u otra información confidencial. ¿y qué instrucciones debo imponer con respecto a la longitud y la seguridad de la contraseña?

Hoy en día, la implementación de cuentas de usuario en una aplicación ASP.NET es mucho más sencilla gracias al *marco de pertenencia* y los controles Web de inicio de sesión integrados. El marco de pertenencia es una serie de clases del [espacio de nombres System. Web. Security](https://msdn.microsoft.com/library/system.web.security.aspx) que proporcionan funcionalidad para realizar tareas esenciales relacionadas con las cuentas de usuario. La clase clave en el marco de pertenencia es la [clase de pertenencia](https://msdn.microsoft.com/library/system.web.security.membership.aspx), que tiene métodos como:

- CreateUser
- DeleteUser
- GetAllUsers
- GetUser
- UpdateUser
- ValidateUser

El marco de pertenencia usa el [modelo de proveedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), que separa sin problemas la API del marco de pertenencia de su implementación. Esto permite a los desarrolladores utilizar una API común, pero permite usar una implementación que satisfaga las necesidades personalizadas de su aplicación. En Resumen, la clase de pertenencia define la funcionalidad esencial del marco (los métodos, las propiedades y los eventos), pero no proporciona ningún detalle de implementación. En su lugar, los métodos de la clase de pertenencia invocan al proveedor configurado, que es lo que realiza el trabajo real. Por ejemplo, cuando se invoca el método CreateUser de la clase de pertenencia, la clase de pertenencia no conoce los detalles del almacén de usuario. No sabe si los usuarios se mantienen en una base de datos o en un archivo XML o en otro almacén. La clase de pertenencia examina la configuración de la aplicación web para determinar el proveedor al que se va a delegar la llamada y esa clase de proveedor es responsable de crear realmente la nueva cuenta de usuario en el almacén de usuarios adecuado. Esta interacción se ilustra en la figura 3.

Microsoft envía dos clases de proveedor de pertenencia en el .NET Framework:

- [ActiveDirectoryMembershipProvider](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) : implementa la API de pertenencia en servidores Active Directory y Active Directory Application Mode (Adam).
- [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) : implementa la API de pertenencia en una base de datos SQL Server.

Esta serie de tutoriales se centra exclusivamente en el SqlMembershipProvider.

[![el modelo de proveedor permite que las distintas implementaciones se conecten sin problemas en el marco de trabajo&lt;/Strong&gt;](security-basics-and-asp-net-support-cs/_static/image4.png)](security-basics-and-asp-net-support-cs/_static/image3.png)

**Figura 03**: el modelo de proveedor permite que distintas implementaciones se conecten sin problemas en el marco de trabajo ([haga clic para ver la imagen de tamaño completo](security-basics-and-asp-net-support-cs/_static/image5.png))

La ventaja del modelo de proveedor es que las implementaciones alternativas pueden ser desarrolladas por Microsoft, proveedores de terceros o desarrolladores individuales y conectados sin problemas en el marco de pertenencia. Por ejemplo, Microsoft ha lanzado [un proveedor de pertenencia para las bases de datos de Microsoft Access](https://download.microsoft.com/download/5/5/b/55bc291f-4316-4fd7-9269-dbf9edbaada8/sampleaccessproviders.vsi). Para obtener más información sobre los proveedores de pertenencia, consulte el [Kit de herramientas del proveedor](https://msdn.microsoft.com/asp.net/aa336558.aspx), que incluye un tutorial de los proveedores de pertenencia, proveedores personalizados de ejemplo, más de 100 páginas de documentación sobre el modelo de proveedor y el código fuente completo de los proveedores de pertenencia integrados (es decir, ActiveDirectoryMembershipProvider y SqlMembershipProvider).

ASP.NET 2,0 también presentó el marco de trabajo de roles. Al igual que el marco de pertenencia, el marco de trabajo de roles se crea sobre el modelo de proveedor. Su API se expone a través de la [clase roles](https://msdn.microsoft.com/library/system.web.security.roles.aspx) y el .NET Framework se suministra con tres clases de proveedor:

- [AuthorizationStoreRoleProvider](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) : administra la información de roles en un almacén de directivas del administrador de autorizaciones, como Active Directory o Adam.
- [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) : implementa roles en una base de datos de SQL Server.
- [WindowsTokenRoleProvider](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx) : asocia información de roles basada en el grupo de Windows del visitante. Este método se utiliza normalmente con la autenticación de Windows.

Esta serie de tutoriales se centra exclusivamente en el SqlRoleProvider.

Dado que el modelo de proveedor incluye una única API orientada hacia delante (las clases de pertenencia y roles), es posible generar funcionalidad en torno a esa API sin tener que preocuparse por los detalles de la implementación, ya que se administran mediante los proveedores seleccionados por la página. Desarrollador. Esta API unificada permite a los proveedores de Microsoft y de terceros crear controles Web que intervienen con los marcos de pertenencia y roles. ASP.NET se suministra con varios [controles Web de inicio de sesión](https://msdn.microsoft.com/library/ms178329.aspx) para implementar interfaces de usuario comunes de cuentas de usuario. Por ejemplo, el [control de inicio de sesión](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) solicita a un usuario sus credenciales, las valida y, a continuación, las registra en mediante la autenticación de formularios. El [control LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) ofrece plantillas para mostrar diferentes marcas a usuarios anónimos frente a usuarios autenticados, o a un marcado diferente según el rol del usuario. Y el [control CreateUserWizard](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.aspx) proporciona una interfaz de usuario paso a paso para crear una nueva cuenta de usuario.

Debajo del apartado se tratan los distintos controles de inicio de sesión que interactúan con los marcos de pertenencia y roles. La mayoría de los controles de inicio de sesión se pueden implementar sin tener que escribir una sola línea de código. Examinaremos estos controles con más detalle en futuros tutoriales, incluidas las técnicas para extender y personalizar su funcionalidad.

## <a name="summary"></a>Resumen

Todas las aplicaciones web que admiten cuentas de usuario requieren características similares, por ejemplo: la capacidad de los usuarios de iniciar sesión y de que su estado de inicio de sesión se recuerda a través de visitas a páginas. una página web para que los nuevos visitantes creen una cuenta; y la capacidad del desarrollador de páginas para especificar qué recursos, datos y funcionalidad están disponibles para los usuarios o los roles. Las tareas de autenticación y autorización de usuarios y de la administración de roles y cuentas de usuario son fácilmente fáciles de realizar en las aplicaciones de ASP.NET gracias a la autenticación de formularios, la autorización de direcciones URL y los marcos de pertenencia y roles.

En el transcurso de los siguientes tutoriales, examinaremos estos aspectos mediante la creación de una aplicación Web de trabajo desde el principio en una manera paso a paso. En el siguiente tutorial, exploraremos la autenticación de formularios en detalle. Veremos el flujo de trabajo de autenticación de formularios en acción, analizar minuciosamente el vale de autenticación de formularios, trataremos los problemas de seguridad y veremos cómo configurar el sistema de autenticación de formularios-todo al compilar una aplicación web que permite a los visitantes iniciar sesión y cerrar sesión.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Pertenencia a ASP.NET 2,0, roles, autenticación de formularios y recursos de seguridad](https://weblogs.asp.net/scottgu/ASP.NET-2.0-Membership_2C00_-Roles_2C00_-Forms-Authentication_2C00_-and-Security-Resources-)
- [Directrices de seguridad de ASP.NET 2,0](https://msdn.microsoft.com/library/ms998258.aspx)
- [Autenticación de ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx)
- [Autorización de ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Información general sobre los controles de inicio de sesión ASP.NET](https://msdn.microsoft.com/library/ms178329.aspx)
- [Examen de la pertenencia, los roles y el perfil de ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Cómo: proteger mi sitio mediante la pertenencia y los roles](https://asp.net/learn/videos/video-45.aspx) Cámara
- [Introducción a la pertenencia](https://msdn.microsoft.com/library/yh26yfzy.aspx)
- [Centro para desarrolladores de seguridad de MSDN](https://msdn.microsoft.com/security/default.aspx)
- [Seguridad, pertenencia y administración de roles de Professional ASP.NET 2,0](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Kit de herramientas de proveedor](https://msdn.microsoft.com/asp.net/aa336558.aspx)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor responsable de este tutorial fue revisado por muchos revisores útiles. Los revisores responsables de este tutorial incluyen Alicja Maziarz, John Suru y Teresa Murphy. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Siguiente](an-overview-of-forms-authentication-cs.md)
