---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: Proteger aplicaciones con autenticación y autorización | Microsoft Docs
author: microsoft
description: En el paso 9 se muestra cómo agregar autenticación y autorización para proteger nuestra aplicación NerdDinner, de modo que los usuarios tengan que registrarse e iniciar sesión en el sitio para crear...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: 8d509c5f15bb4d5014e53b8dc2a736454238e72c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486427"
---
# <a name="secure-applications-using-authentication-and-authorization"></a>Proteger las aplicaciones mediante la autenticación y la autorización

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 9 de un [tutorial de aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito en el que se explica cómo crear una aplicación web pequeña, pero completa, con ASP.NET MVC 1.
> 
> En el paso 9 se muestra cómo agregar autenticación y autorización para proteger nuestra aplicación de NerdDinner, de modo que los usuarios tengan que registrarse e iniciar sesión en el sitio para crear nuevas cenas y solo el usuario que hospeda una cena puede editarla más adelante.
> 
> Si usa ASP.NET MVC 3, se recomienda seguir los tutoriales [de la introducción con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o MVC [Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-9-authentication-and-authorization"></a>NerdDinner paso 9: autenticación y autorización

En la actualidad, nuestra aplicación NerdDinner concede a cualquier persona que visite el sitio la capacidad de crear y editar los detalles de cualquier cena. Vamos a cambiar esto para que los usuarios tengan que registrarse e iniciar sesión en el sitio para crear nuevas cenas y agregar una restricción para que solo el usuario que hospeda una cena pueda editarla más adelante.

Para habilitarlo, usaremos la autenticación y la autorización para proteger nuestra aplicación.

### <a name="understanding-authentication-and-authorization"></a>Descripción de la autenticación y la autorización

La *autenticación* es el proceso de identificar y validar la identidad de un cliente que tiene acceso a una aplicación. En otras palabras, se trata de identificar "quién" es el usuario final cuando visitan un sitio Web. ASP.NET admite varias maneras de autenticar a los usuarios del explorador. En el caso de las aplicaciones Web de Internet, el enfoque de autenticación más común que se utiliza se denomina "autenticación de formularios". La autenticación mediante formularios permite a un desarrollador crear un formulario de inicio de sesión HTML dentro de su aplicación y, a continuación, validar el nombre de usuario y la contraseña que un usuario final envía a una base de datos u otro almacén de credenciales de contraseña. Si la combinación de nombre de usuario y contraseña es correcta, el desarrollador puede solicitar a ASP.NET que emita una cookie HTTP cifrada para identificar al usuario en las solicitudes futuras. Usaremos la autenticación de formularios con nuestra aplicación NerdDinner.

La *autorización* es el proceso de determinar si un usuario autenticado tiene permiso para obtener acceso a una dirección URL o recurso determinado o para realizar alguna acción. Por ejemplo, en nuestra aplicación NerdDinner nos gustaría autorizar que solo los usuarios que han iniciado sesión puedan acceder a la dirección URL de */Dinners/Create* y crear nuevas cenas. También queremos agregar lógica de autorización para que solo el usuario que hospeda una cena pueda editarla y denegar el acceso de edición a todos los demás usuarios.

### <a name="forms-authentication-and-the-accountcontroller"></a>Autenticación de formularios y AccountController

La plantilla de proyecto de Visual Studio predeterminada para ASP.NET MVC habilita automáticamente la autenticación de formularios cuando se crean nuevas aplicaciones de ASP.NET MVC. También agrega automáticamente una implementación de página de inicio de sesión de cuenta pregenerada al proyecto, lo que facilita la integración de la seguridad dentro de un sitio.

La página maestra site. Master predeterminada muestra un vínculo "iniciar sesión" en la parte superior derecha del sitio cuando el usuario que tiene acceso a él no está autenticado:

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

Al hacer clic en el vínculo "iniciar sesión", se toma un usuario de la dirección URL de */account/Logon* :

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

Los visitantes que no se han registrado pueden hacerlo haciendo clic en el vínculo "registrar", que los llevará a la dirección URL de */account/Register* y les permitirá especificar los detalles de la cuenta:

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

Al hacer clic en el botón "registrar", se creará un nuevo usuario en el sistema de pertenencia a ASP.NET y se autenticará al usuario en el sitio mediante la autenticación de formularios.

Cuando un usuario ha iniciado sesión, site. Master cambia la parte superior derecha de la página para generar un "Welcome [username]!" Message y representa un vínculo "cerrar sesión" en lugar de "iniciar sesión". Al hacer clic en el vínculo "cerrar sesión", se cierra la sesión del usuario:

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

La funcionalidad de inicio de sesión, cierre de sesión y registro anterior se implementa dentro de la clase AccountController que Visual Studio agregó a nuestro proyecto al crear el proyecto. La interfaz de usuario de AccountController se implementa mediante plantillas de vista en el directorio \Views\Account:

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

La clase AccountController usa el sistema de autenticación de formularios ASP.NET para emitir cookies de autenticación cifradas y la API de pertenencia a ASP.NET para almacenar y validar nombres de usuario y contraseñas. La API de pertenencia a ASP.NET es extensible y permite el uso de cualquier almacén de credenciales de contraseña. ASP.NET incluye implementaciones de proveedor de pertenencia integradas que almacenan el nombre de usuario y contraseñas en una base de datos SQL o en Active Directory.

Podemos configurar el proveedor de pertenencia que debe usar nuestra aplicación NerdDinner. para ello, abra el archivo "Web. config" en la raíz del proyecto y busque la sección &lt;pertenencia&gt; en él. El archivo Web. config predeterminado agregado cuando se creó el proyecto registra el proveedor de pertenencia de SQL y lo configura para que use una cadena de conexión denominada "ApplicationServices" para especificar la ubicación de la base de datos.

La cadena de conexión predeterminada "ApplicationServices" (que se especifica dentro de la sección &lt;connectionStrings&gt; del archivo Web. config) se configura para usar SQL Express. Apunta a una base de datos SQL Express denominada "ASPNETDB. MDF "en el directorio" App\_Data "de la aplicación. Si esta base de datos no existe la primera vez que se usa la API de pertenencia dentro de la aplicación, ASP.NET creará automáticamente la base de datos y aprovisionará el esquema de la base de datos de pertenencia adecuado en ella:

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

Si en lugar de usar SQL Express deseamos usar una instancia de SQL Server completa (o conectarse a una base de datos remota), lo único que debemos hacer es actualizar la cadena de conexión "ApplicationServices" en el archivo Web. config y asegurarse de que el esquema de pertenencia adecuado se ha agregado a la base de datos a la que apunta. Puede ejecutar la utilidad "ASPNET\_regsql. exe" en el directorio \Windows\Microsoft.NET\Framework\v2.0.50727\ para agregar el esquema adecuado para la pertenencia y los otros servicios de aplicación de ASP.NET a una base de datos.

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>Autorización de la dirección URL de/Dinners/Create mediante el filtro [Authorize]

No tuvimos que escribir ningún código para habilitar una implementación segura de administración de cuentas y autenticación para la aplicación NerdDinner. Los usuarios pueden registrar cuentas nuevas con nuestra aplicación e iniciar sesión o cerrar la sesión del sitio.

Ahora podemos agregar lógica de autorización a la aplicación y usar el estado de autenticación y el nombre de usuario de los visitantes para controlar lo que pueden y no pueden hacer en el sitio. Comencemos agregando lógica de autorización a los métodos de acción "Create" de nuestra clase DinnersController. En concreto, se requerirá que los usuarios que accedan a la dirección URL de */Dinners/Create* deban iniciar sesión. Si no se registran, se les redirigirá a la página de inicio de sesión para que puedan iniciar sesión.

La implementación de esta lógica es bastante fácil. Lo único que debemos hacer es agregar un atributo de filtro [Authorize] a nuestros métodos de acción de creación como este:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

ASP.NET MVC admite la capacidad de crear "filtros de acción" que se pueden usar para implementar una lógica reutilizable que se puede aplicar mediante declaración a métodos de acción. El filtro [Authorize] es uno de los filtros de acción integrados que proporciona ASP.NET MVC y permite a un desarrollador aplicar de forma declarativa reglas de autorización a métodos de acción y clases de controlador.

Cuando se aplica sin ningún parámetro (como el anterior), el filtro [Authorize] exige que el usuario que realiza la solicitud del método de acción deba iniciar sesión, y redirigirá automáticamente el explorador a la dirección URL de inicio de sesión si no lo están. Al hacerlo, se pasa la dirección URL solicitada originalmente como argumento QueryString (por ejemplo:/Account/LogOn? ReturnUrl =% 2fDinners% 2fCreate). El AccountController redirigirá al usuario a la dirección URL solicitada originalmente una vez que inicie sesión.

El filtro [Authorize] admite opcionalmente la capacidad de especificar una propiedad de "usuarios" o "roles" que se puede usar para requerir que el usuario inicie sesión y en una lista de usuarios permitidos o un miembro de un rol de seguridad permitido. Por ejemplo, el código siguiente solo permite que dos usuarios específicos, "scottgu" y "billg", tengan acceso a la dirección URL de/Dinners/Create:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

No obstante, la incrustación de nombres de usuario específicos en el código tiende a ser bastante fácil de mantener. Un enfoque mejor es definir "roles" de nivel superior con el que se comprueba el código y, a continuación, asignar usuarios al rol mediante una base de datos o un sistema de Active Directory (lo que permite que la lista de asignación de usuarios real se almacene externamente desde el código). ASP.NET incluye una API de administración de roles integrada, así como un conjunto integrado de proveedores de roles (incluidos los de SQL y Active Directory) que pueden ayudar a realizar esta asignación de roles o usuarios. A continuación, podríamos actualizar el código para permitir que los usuarios de un rol "admin" específico accedan a la dirección URL de/Dinners/Create:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>Uso de la propiedad User.Identity.Name al crear cenas

Podemos recuperar el nombre de usuario del usuario que ha iniciado sesión actualmente de una solicitud mediante la propiedad User.Identity.Name expuesta en la clase base del controlador.

Antes de implementar la versión HTTP-POST de nuestro método de acción Create (), hemos codificado la propiedad "HostedBy" de la cena en una cadena estática. Ahora podemos actualizar este código para usar en su lugar la propiedad User.Identity.Name, así como agregar automáticamente un RSVP para el host que crea la cena:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

Dado que hemos agregado un atributo [Authorize] al método Create (), ASP.NET MVC garantiza que el método de acción solo se ejecute si el usuario que visita la dirección URL de/Dinners/Create está conectado en el sitio. Como tal, el valor de la propiedad User.Identity.Name siempre contendrá un nombre de usuario válido.

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>Uso de la propiedad User.Identity.Name al editar cenas

Vamos a agregar ahora cierta lógica de autorización que restringe a los usuarios para que solo puedan editar las propiedades de las cenas que hospedan.

Para ayudarlo, primero agregaremos un método auxiliar "IsHostedBy (nombre de usuario)" a nuestro objeto cena (dentro de la clase parcial Dinner.cs que hemos creado anteriormente). Este método auxiliar devuelve true o false en función de si un nombre de usuario proporcionado coincide con la propiedad cena HostedBy y encapsula la lógica necesaria para realizar una comparación de cadenas que no distingue mayúsculas de minúsculas:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

A continuación, agregaremos un atributo [Authorize] a los métodos de acción Edit () dentro de nuestra clase DinnersController. Esto garantizará que los usuarios deben iniciar sesión para solicitar una dirección URL de */Dinners/Edit/[id]* .

A continuación, podemos agregar código a los métodos de edición que usa el método auxiliar cena. IsHostedBy (nombre de usuario) para comprobar que el usuario que ha iniciado sesión coincide con el host de la cena. Si el usuario no es el host, se mostrará una vista "InvalidOwner" y finalizará la solicitud. El código para hacerlo tiene el siguiente aspecto:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

A continuación, podemos hacer clic con el botón derecho en el directorio \Views\Dinners y elegir el comando de menú Ver&gt;para crear una nueva vista "InvalidOwner". Lo rellenaremos con el siguiente mensaje de error:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

Y ahora cuando un usuario intenta editar una cena que no es de su propiedad, recibirá un mensaje de error:

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

Podemos repetir los mismos pasos para los métodos de acción Delete () en nuestro controlador para bloquear el permiso para eliminar también las cenas y asegurarse de que solo el anfitrión de una cena puede eliminarla.

### <a name="showinghiding-edit-and-delete-links"></a>Mostrar u ocultar vínculos de edición y eliminación

Estamos vinculando al método de acción Edit y Delete de nuestra clase DinnersController de nuestra dirección URL de detalles:

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

Actualmente se muestran los vínculos de acción de edición y eliminación independientemente de si el visitante de la dirección URL de detalles es el host de la cena. Vamos a cambiar esto para que los vínculos solo se muestren si el usuario visitante es el propietario de la cena.

El método de acción Details () de nuestro DinnersController recupera un objeto cena y, a continuación, lo pasa como el objeto de modelo a nuestra plantilla de vista:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

Podemos actualizar nuestra plantilla de vista para mostrar u ocultar condicionalmente los vínculos de edición y eliminación con el método auxiliar cena. IsHostedBy (), como se muestra a continuación:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>Pasos siguientes

Ahora veremos cómo podemos habilitar a los usuarios autenticados para que consigan las cenaes mediante AJAX.

> [!div class="step-by-step"]
> [Anterior](implement-efficient-data-paging.md)
> [Siguiente](use-ajax-to-deliver-dynamic-updates.md)
