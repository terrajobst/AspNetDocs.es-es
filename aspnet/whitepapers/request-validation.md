---
uid: whitepapers/request-validation
title: 'Validación de solicitudes: prevención de ataques de scripts | Microsoft Docs'
author: rick-anderson
description: En este documento se describe la característica de validación de solicitudes de ASP.NET en la que, de forma predeterminada, se evita que la aplicación procese el envío de contenido HTML sin codificar...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 807cccd6fe1acdd6359b014387abd3878840d4cd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78520585"
---
# <a name="request-validation---preventing-script-attacks"></a>Validación de solicitudes - Prevenir ataques de scripts

> En este documento se describe la característica de validación de solicitudes de ASP.NET en la que, de forma predeterminada, se evita que la aplicación procese el contenido HTML sin codificar que se envía al servidor. Esta característica de validación de solicitudes se puede deshabilitar cuando la aplicación se ha diseñado para procesar datos HTML de forma segura.
> 
> Se aplica a ASP.NET 1,1 y ASP.NET 2,0.

La validación de solicitudes, una característica de ASP.NET desde la versión 1.1, impide que el servidor acepte contenido con HTML sin codificar. Esta característica está diseñada para ayudar a evitar algunos ataques de inserción de script mediante los cuales el código de script del cliente o HTML puede enviarse sin que se sepa a un servidor, almacenarse y presentarse a otros usuarios. Se recomienda encarecidamente validar todos los datos de entrada y codificar el código HTML cuando corresponda.

Por ejemplo, crea una página web que solicita la dirección de correo electrónico de un usuario y, a continuación, almacena esa dirección de correo electrónico en una base de datos. Si el usuario escribe &lt;SCRIPT&gt;alerta ("Hello from script")&lt;/SCRIPT&gt; en lugar de una dirección de correo electrónico válida, cuando se presenten los datos, se puede ejecutar este script si el contenido no se ha codificado correctamente. La característica de validación de solicitudes de ASP.NET impide que esto suceda.

## <a name="why-this-feature-is-useful"></a>¿Por qué esta característica es útil?

Muchos sitios no saben que están abiertos a ataques de inyección de script simple. Si el propósito de estos ataques es desorientar el sitio al mostrar HTML o ejecutar el script de cliente para redirigir al usuario al sitio de un hacker, los ataques de inyección de script son un problema con el que los desarrolladores web deben competir.

Los ataques por inyección de script son un problema de todos los desarrolladores web, ya estén usando ASP.NET, ASP u otras tecnologías de desarrollo web.

La característica de validación de solicitudes de ASP.NET impide proactivamente estos ataques, ya que no permite que el servidor procese el contenido HTML sin codificar a menos que el desarrollador decida permitir dicho contenido.

## <a name="what-to-expect-error-page"></a>Qué esperar: Página de error

En la captura de pantalla siguiente se muestra un código de ejemplo de ASP.NET:

![](request-validation/_static/image1.png)

La ejecución de este código da como resultado una página simple que le permite escribir texto en el cuadro de texto, hacer clic en el botón y mostrar el texto en el control etiqueta:

![](request-validation/_static/image2.png)

Sin embargo, con JavaScript, como `<script>alert("hello!")</script>` que se va a escribir y enviar, se obtendría una excepción:

![](request-validation/_static/image3.png)

El mensaje de error indica que se detectó un valor de "solicitud potencialmente peligrosa. valor de formulario" y proporciona más detalles en la descripción en lo que se produjo exactamente y cómo cambiar el comportamiento. Por ejemplo:

La validación de solicitudes ha detectado un valor de entrada de cliente potencialmente peligroso y se ha anulado el procesamiento de la solicitud. Este valor puede indicar un intento de poner en peligro la seguridad de la aplicación, como un ataque de scripts entre sitios. Puede deshabilitar la validación de solicitudes estableciendo `validateRequest=false` en la Directiva de página o en la sección de configuración. Sin embargo, se recomienda encarecidamente que la aplicación Compruebe explícitamente todas las entradas en este caso.

## <a name="disabling-request-validation-on-a-page"></a>Deshabilitar la validación de solicitudes en una página

Para deshabilitar la validación de solicitudes en una página, debe establecer el atributo `validateRequest` de la Directiva de página en `false`:

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> Cuando la validación de solicitudes está deshabilitada, el contenido se puede enviar a una página. es responsabilidad del desarrollador de páginas asegurarse de que el contenido se codifica o procesa correctamente.

## <a name="disabling-request-validation-for-your-application"></a>Deshabilitación de la validación de solicitudes para la aplicación

Para deshabilitar la validación de solicitudes de la aplicación, debe modificar o crear un archivo Web. config para la aplicación y establecer el atributo validateRequest de la sección `<pages />` en `false`:

[!code-xml[Main](request-validation/samples/sample2.xml)]

Si desea deshabilitar la validación de solicitudes para todas las aplicaciones del servidor, puede realizar esta modificación en el archivo Machine. config.

> [!CAUTION]
> Cuando la validación de solicitudes está deshabilitada, el contenido se puede enviar a la aplicación; es responsabilidad del desarrollador de aplicaciones asegurarse de que el contenido se codifica o procesa correctamente.

El código siguiente se modifica para desactivar la validación de solicitudes:

![](request-validation/_static/image4.png)

Ahora, si se especificó el siguiente código JavaScript en el cuadro de texto `<script>alert("hello!")</script>` el resultado sería:

![](request-validation/_static/image5.png)

Para evitar que esto suceda, con la validación de solicitudes desactivada, es necesario codificar en HTML el contenido.

## <a name="how-to-html-encode-content"></a>Cómo codificar contenido en HTML

Si ha deshabilitado la validación de solicitudes, se recomienda codificar en HTML el contenido que se almacenará para su uso futuro. La codificación HTML reemplazará automáticamente cualquier '&lt;' o '&gt;' (junto con otros símbolos) por su representación codificada en HTML correspondiente. Por ejemplo, '&lt;' se reemplaza por '&amp;lt; ' y '&gt;' se reemplaza por '&amp;gt; '. Los exploradores usan estos códigos especiales para mostrar el '&lt;' o '&gt;' en el explorador.

El contenido se puede codificar en HTML con facilidad en el servidor mediante la API de `Server.HtmlEncode(string)`. El contenido también se puede descodificar con facilidad HTML, es decir, volver al código HTML estándar con el método `Server.HtmlDecode(string)`.

![](request-validation/_static/image6.png)

Resultado de:

![](request-validation/_static/image7.png)
