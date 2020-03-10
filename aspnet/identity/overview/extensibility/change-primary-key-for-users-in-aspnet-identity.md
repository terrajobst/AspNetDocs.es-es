---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: Cambiar la clave principal de los usuarios en ASP.NET Identity ASP.NET 4. x
author: Rick-Anderson
description: En Visual Studio 2013, la aplicación web predeterminada utiliza un valor de cadena para la clave para las cuentas de usuario. ASP.NET Identity permite cambiar el tipo de la...
ms.author: riande
ms.date: 09/30/2014
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0afea8eacfc646f1489b87629fdb2d437815d88c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78472267"
---
# <a name="change-primary-key-for-users-in-aspnet-identity"></a>Cambiar la clave principal de los usuarios en ASP.NET Identity

por [Tom FitzMacken](https://github.com/tfitzmac)

> En Visual Studio 2013, la aplicación web predeterminada utiliza un valor de cadena para la clave para las cuentas de usuario. ASP.NET Identity le permite cambiar el tipo de la clave para satisfacer sus requisitos de datos. Por ejemplo, puede cambiar el tipo de la clave de una cadena a un entero.
> 
> En este tema se muestra cómo iniciar con la aplicación web predeterminada y cambiar la clave de la cuenta de usuario a un número entero. Puede usar las mismas modificaciones para implementar cualquier tipo de clave en el proyecto. Muestra cómo realizar estos cambios en la aplicación web predeterminada, pero podría aplicar modificaciones similares a una aplicación personalizada. Muestra los cambios necesarios al trabajar con MVC o formularios Web Forms.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - Visual Studio 2013 con Update 2 (o posterior)
> - ASP.NET Identity 2,1 o posterior

Para realizar los pasos de este tutorial, debe tener Visual Studio 2013 Update 2 (o posterior) y una aplicación Web creada a partir de la plantilla de aplicación Web ASP.NET. La plantilla cambió en Update 3. En este tema se muestra cómo cambiar la plantilla en Update 2 y Update 3.

Este tema contiene las siguientes secciones:

- [Cambiar el tipo de la clave en la clase de usuario Identity](#userclass)
- [Agregar clases de identidad personalizadas que utilizan el tipo de clave](#customclass)
- [Cambiar la clase de contexto y el administrador de usuarios para usar el tipo de clave](#context)
- [Cambiar la configuración de inicio para usar el tipo de clave](#startup)
- [Para MVC con Update 2, cambie AccountController para pasar el tipo de clave.](#mvcupdate2)
- [Para MVC con Update 3, cambie AccountController y ManageController para pasar el tipo de clave.](#mvcupdate3)
- [Para formularios Web Forms con Update 2, cambie las páginas de cuenta para pasar el tipo de clave.](#webformsupdate2)
- [Para formularios Web Forms con Update 3, cambie las páginas de cuenta para pasar el tipo de clave.](#webformsupdate3)
- [Ejecutar aplicación](#run)
- [Otros recursos](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a>Cambiar el tipo de la clave en la clase de usuario Identity

En el proyecto creado a partir de la plantilla de aplicación Web ASP.NET, especifique que la clase ApplicationUser usa un entero para la clave para las cuentas de usuario. En IdentityModels.cs, cambie la clase ApplicationUser para que herede de IdentityUser que tiene un tipo de **int** para el parámetro genérico TKey. También se pasan los nombres de tres clases personalizadas que todavía no se han implementado.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

Ha cambiado el tipo de la clave, pero, de forma predeterminada, el resto de la aplicación todavía supone que la clave es una cadena. Debe indicar explícitamente el tipo de la clave en el código que supone una cadena.

En la clase **ApplicationUser** , cambie el método **GenerateUserIdentityAsync** para incluir int, tal como se muestra en el código resaltado a continuación. Este cambio no es necesario para los proyectos de formularios Web Forms con la plantilla Update 3.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a>Agregar clases de identidad personalizadas que utilizan el tipo de clave

Las demás clases de identidad, como IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, todavía se configuran para usar una clave de cadena. Cree versiones nuevas de estas clases que especifiquen un entero para la clave. No es necesario proporcionar mucho código de implementación en estas clases, principalmente simplemente se establece int como la clave.

Agregue las siguientes clases al archivo IdentityModels.cs.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a>Cambiar la clase de contexto y el administrador de usuarios para usar el tipo de clave

En IdentityModels.cs, cambie la definición de la clase **ApplicationDbContext** para usar las nuevas clases personalizadas y un **int** para la clave, como se muestra en el código resaltado.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

El parámetro ThrowIfV1Schema ya no es válido en el constructor. Cambie el constructor para que no pase un valor de ThrowIfV1Schema.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

Abra IdentityConfig.cs y cambie la clase **ApplicationUserManger** para usar la nueva clase de almacén de usuario para conservar los datos y un valor **int** para la clave.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

En la plantilla Update 3, debe cambiar la clase ApplicationSignInManager.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a>Cambiar la configuración de inicio para usar el tipo de clave

En Startup.Auth.cs, reemplace el código de OnValidateIdentity, como se indica a continuación. Observe que la definición de getUserIdCallback, analiza el valor de cadena en un entero.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

Si el proyecto no reconoce la implementación genérica del método **GetUserId** , puede que tenga que actualizar el paquete NuGet ASP.net Identity a la versión 2,1

Ha realizado muchos cambios en las clases de infraestructura utilizadas por ASP.NET Identity. Si intenta compilar el proyecto, observará que se producen muchos errores. Afortunadamente, los errores restantes son similares. La clase Identity espera un entero para la clave, pero el controlador (o formulario web) está pasando un valor de cadena. En cada caso, debe convertir de una cadena a y entero llamando a **GetUserId&lt;int&gt;** . Puede trabajar a través de la lista de errores desde la compilación o seguir los cambios siguientes.

Los cambios restantes dependen del tipo de proyecto que se va a crear y de la actualización que se haya instalado en Visual Studio. Puede ir directamente a la sección correspondiente a través de los vínculos siguientes:

- [Para MVC con Update 2, cambie AccountController para pasar el tipo de clave.](#mvcupdate2)
- [Para MVC con Update 3, cambie AccountController y ManageController para pasar el tipo de clave.](#mvcupdate3)
- [Para formularios Web Forms con Update 2, cambie las páginas de cuenta para pasar el tipo de clave.](#webformsupdate2)
- [Para formularios Web Forms con Update 3, cambie las páginas de cuenta para pasar el tipo de clave.](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a>Para MVC con Update 2, cambie AccountController para pasar el tipo de clave.

Abra el archivo AccountController.cs. Debe cambiar los métodos siguientes.

Método **ConfirmEmail**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

**Unsociate** (método)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

**Manage (ManageUserViewModel) (método)**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

Método **LinkLoginCallback**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

Método **RemoveAccountList**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

Método **HasPassword**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

Ahora puede [ejecutar la aplicación](#run) y registrar un nuevo usuario.

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a>Para MVC con Update 3, cambie AccountController y ManageController para pasar el tipo de clave.

Abra el archivo AccountController.cs. Debe cambiar el método siguiente.

Método **ConfirmEmail**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

Método **SendCode**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

Abra el archivo ManageController.cs. Debe cambiar los métodos siguientes.

**Index** (método)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

Métodos **RemoveLogin**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

Método **AddPhoneNumber**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

Método **EnableTwoFactorAuthentication**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

Método **DisableTwoFactorAuthentication**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

Métodos **VerifyPhoneNumber**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

Método **RemovePhoneNumber**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

**ChangePassword** (método)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

**SetPassword** (método)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

Método **ManageLogins**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

Método **LinkLoginCallback**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

Método **HasPassword**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

Método **HasPhoneNumber**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

Ahora puede [ejecutar la aplicación](#run) y registrar un nuevo usuario.

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a>Para formularios Web Forms con Update 2, cambie las páginas de cuenta para pasar el tipo de clave.

En el caso de los formularios Web Forms con Update 2, debe cambiar las páginas siguientes.

**Confirm.aspx.cx**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

Ahora puede [ejecutar la aplicación](#run) y registrar un nuevo usuario.

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a>Para formularios Web Forms con Update 3, cambie las páginas de cuenta para pasar el tipo de clave.

En el caso de los formularios Web Forms con Update 3, debe cambiar las páginas siguientes.

**Confirm.aspx.cx**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

**VerifyPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

**AddPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

**ManagePassword.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

**ManageLogins.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

**TwoFactorAuthenticationSignIn.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a>Ejecución de la aplicación

Ha finalizado todos los cambios necesarios en la plantilla de aplicación web predeterminada. Ejecute la aplicación y registre un nuevo usuario. Después de registrar al usuario, observará que la tabla AspNetUsers tiene una columna de identificador que es un entero.

![nueva clave principal](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

Si ya ha creado las tablas de ASP.NET Identity con una clave principal diferente, deberá realizar algunos cambios adicionales. Si es posible, solo tiene que eliminar la base de datos existente. La base de datos se volverá a crear con el diseño correcto al ejecutar la aplicación web y agregar un nuevo usuario. Si no es posible la eliminación, ejecute migraciones de Code First para cambiar las tablas. Sin embargo, la nueva clave principal de entero no se configurará como una propiedad de identidad de SQL en la base de datos. Debe establecer manualmente la columna ID como una identidad.

<a id="other"></a>
## <a name="other-resources"></a>Otros recursos

- [Información general sobre los proveedores de almacenamiento personalizado para ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Migrar un sitio web existente desde la pertenencia de SQL a ASP.NET Identity](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [Migrar datos del proveedor universal para la pertenencia y los perfiles de usuario a ASP.NET Identity](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- [Aplicación de ejemplo](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/ChangePK) con clave principal modificada
