---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Uso de SSL en Web API | Microsoft Docs
author: MikeWasson
description: Muestra cómo usar SSL con ASP.NET Web API, incluido el uso de certificados de cliente SSL.
ms.author: riande
ms.date: 02/22/2019
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 31589b3713b1f1a9b98d12906bfef81f8bf5e3f9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484417"
---
# <a name="working-with-ssl-in-web-api"></a>Trabajar con SSL en Web API

por [Mike Wasson](https://github.com/MikeWasson)

Varios esquemas de autenticación comunes no son seguros en HTTP simple. En particular, la autenticación básica y la autenticación mediante formularios envían credenciales no cifradas. Para que sea seguro, estos esquemas de autenticación *deben* usar SSL. Además, los certificados de cliente SSL se pueden usar para autenticar a los clientes.

## <a name="enabling-ssl-on-the-server"></a>Habilitación de SSL en el servidor

Para configurar SSL en IIS 7 o posterior:

- Cree u obtenga un certificado. Para las pruebas, puede crear un certificado autofirmado.
- Agregue un enlace HTTPS.

Para obtener más información, consulte [configuración de SSL en IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

En el caso de las pruebas locales, puede habilitar SSL en IIS Express desde Visual Studio. En el ventana Propiedades, establezca **SSL habilitado** en **true**. Anote el valor de **dirección URL de SSL**; Use esta dirección URL para probar las conexiones HTTPS.

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a>Aplicación de SSL en un controlador de API Web

Si tiene un enlace HTTP y HTTPS, los clientes pueden seguir usando HTTP para tener acceso al sitio. Podría permitir que algunos recursos estén disponibles a través de HTTP, mientras que otros requieren SSL. En ese caso, use un filtro de acción para requerir SSL para los recursos protegidos. El código siguiente muestra un filtro de autenticación de API web que comprueba el estado de SSL:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

Añada este filtro a todas las acciones de API web que requieran SSL:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a>Certificados de cliente SSL

SSL proporciona autenticación mediante certificados de infraestructura de clave pública. El servidor debe proporcionar un certificado que autentique el servidor en el cliente. Es menos común que el cliente proporcione un certificado al servidor, pero esta es una opción para la autenticación de clientes. Para usar certificados de cliente con SSL, necesita una manera de distribuir certificados firmados a los usuarios. Para muchos tipos de aplicaciones, esto no será una buena experiencia del usuario, pero en algunos entornos (por ejemplo, Enterprise) puede ser factible.

| Ventajas | Desventajas |
| --- | --- |
| -Las credenciales de certificado son más seguras que el nombre de usuario y la contraseña. -SSL proporciona un canal seguro completo, con autenticación, integridad de mensajes y cifrado de mensajes. | -Debe obtener y administrar certificados PKI. -La plataforma cliente debe admitir certificados de cliente SSL. |

Para configurar IIS para que acepte certificados de cliente, abra el administrador de IIS y realice los pasos siguientes:

1. Haga clic en el nodo sitio en la vista de árbol.
2. Haga doble clic en la característica **configuración de SSL** en el panel central.
3. En **certificados de cliente**, seleccione una de estas opciones: 

    - **Aceptar**: IIS aceptará un certificado del cliente, pero no requiere uno.
    - **Requerir**: requiere un certificado de cliente. (Para habilitar esta opción, debe seleccionar también "requerir SSL")

También puede establecer estas opciones en el archivo ApplicationHost. config:

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

La marca **SslNegotiateCert** significa que IIS aceptará un certificado del cliente, pero no requiere uno (equivalente a la opción "Aceptar" en el administrador de IIS). Para requerir un certificado, establezca la marca **SslRequireCert** . Para las pruebas, también puede establecer estas opciones en IIS Express, en el ApplicationHost local. Archivo de configuración, que se encuentra en "Documents\IISExpress\config".

### <a name="creating-a-client-certificate-for-testing"></a>Creación de un certificado de cliente para pruebas

Con fines de prueba, puede usar [Makecert. exe](/windows/desktop/SecCrypto/makecert) para crear un certificado de cliente. En primer lugar, cree una entidad de certificación raíz de prueba:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

Makecert le pedirá que escriba una contraseña para la clave privada.

A continuación, agregue el certificado al almacén "entidades de certificación raíz de confianza" del servidor de prueba, como se indica a continuación:

1. Abra MMC.
2. En **archivo**, seleccione **Agregar o quitar complemento**.
3. Seleccione **cuenta de equipo**.
4. Seleccione **equipo local** y complete el asistente.
5. En el panel de navegación, expanda el nodo "entidades de certificación raíz de confianza".
6. En el menú **acción** , seleccione **todas las tareas**y, a continuación, haga clic en **importar** para iniciar el Asistente para importación de certificados.
7. Busque el archivo de certificado TempCA. cer.
8. Haga clic en **abrir**y, a continuación, haga clic en **siguiente** y complete el asistente. (Se le pedirá que vuelva a escribir la contraseña).

Ahora cree un certificado de cliente firmado por el primer certificado:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a>Uso de certificados de cliente en Web API

En el lado del servidor, puede obtener el certificado de cliente mediante una llamada a [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) en el mensaje de solicitud. El método devuelve NULL si no hay ningún certificado de cliente. De lo contrario, devuelve una instancia de **X509Certificate2** . Utilice este objeto para obtener información del certificado, como el emisor y el asunto. Después, puede usar esta información para la autenticación y/o la autorización.

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
