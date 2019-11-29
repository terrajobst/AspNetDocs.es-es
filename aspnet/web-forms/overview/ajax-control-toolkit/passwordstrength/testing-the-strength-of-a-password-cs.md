---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
title: Probar la seguridad de una contraseña (C#) | Microsoft Docs
author: wenz
description: Las contraseñas son necesarias casi en cualquier lugar, por lo que los usuarios diferidos tienden a elegir contraseñas sencillas que son fáciles de interrumpir. Control PasswordStrength en el ASP. N...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: cb4afbae-9b8f-483d-9729-476d4b9f85fc
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
msc.type: authoredcontent
ms.openlocfilehash: e55eab9feebc18f39dd40c59cfb423208296b6c5
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598846"
---
# <a name="testing-the-strength-of-a-password-c"></a>Probar la seguridad de una contraseña (C#)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0CS.pdf)

> Las contraseñas son necesarias casi en cualquier lugar, por lo que los usuarios diferidos tienden a elegir contraseñas sencillas que son fáciles de interrumpir. El control PasswordStrength en el kit de herramientas de control de AJAX de ASP.NET puede comprobar la calidad de una contraseña.

## <a name="overview"></a>Información general del

Las contraseñas son necesarias casi en cualquier lugar, por lo que los usuarios diferidos tienden a elegir contraseñas sencillas que son fáciles de interrumpir. El control `PasswordStrength` en el kit de herramientas de control de ASP.NET AJAX puede comprobar la calidad de una contraseña.

## <a name="steps"></a>Pasos

El control `PasswordStrength` extiende un cuadro de texto y comprueba si la contraseña es suficientemente adecuada. Ofrece una gran cantidad de opciones a través de atributos. Estos son solo algunos de ellos:

- `MinimumNumericCharacters` número mínimo de caracteres numéricos necesarios en la contraseña
- `MinimumSymbolCharacters` número mínimo de caracteres de símbolos (no Letras y dígitos) necesarios en la contraseña
- `PreferredPasswordLength` longitud mínima de la contraseña
- `RequiresUpperAndLowerCaseCharacters` si la contraseña necesita usar caracteres en mayúsculas y minúsculas

El `StrengthIndicatorType` proporciona la información sobre cómo presentar la fuerza de la contraseña, como texto (valor `"Text"`) o como un tipo de barra de progreso (valor `"BarIndicator"`). En el atributo `DisplayPosition`, configure dónde aparece la información. Este es un ejemplo completo, incluido el control de `ScriptManager` de ASP.NET AJAX, el `PasswordStrength` control y, por supuesto, un cuadro de texto en el que el usuario puede escribir una contraseña. Por motivos de demostración, el último campo de formulario es un campo de texto normal y no un campo de contraseña para que pueda ver durante el desarrollo lo que está escribiendo.

[!code-aspx[Main](testing-the-strength-of-a-password-cs/samples/sample1.aspx)]

Ejecute la página y escriba fuera: solo después de escribir letras minúsculas, letras mayúsculas, dígitos y símbolos, se considera que la contraseña es ininterrumpida.

[![ahora la contraseña es (bastante) buena](testing-the-strength-of-a-password-cs/_static/image2.png)](testing-the-strength-of-a-password-cs/_static/image1.png)

Ahora la contraseña es (bastante) buena ([haga clic para ver la imagen de tamaño completo](testing-the-strength-of-a-password-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Siguiente](testing-the-strength-of-a-password-vb.md)
