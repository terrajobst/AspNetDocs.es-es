---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
title: Combatir bots (C#) | Microsoft Docs
author: wenz
description: Los bots automatizados transforman Weblogs y otros sitios web con correo no deseado, enviando formularios de comentarios sin intervención del usuario. El control NoBot en el ASP.NET AJAX con...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0a1917e0-884a-4576-8e93-9ed660faae51
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
msc.type: authoredcontent
ms.openlocfilehash: fef55edf12a024e4dd66e2a18ea371ab4dac861f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78509071"
---
# <a name="fighting-bots-c"></a>Combatir los bots (C#)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)

> Los bots automatizados transforman Weblogs y otros sitios web con correo no deseado, enviando formularios de comentarios sin intervención del usuario. El control NoBot en el kit de herramientas de control de AJAX de ASP.NET puede ayudar a combatir esos bots.

## <a name="overview"></a>Información general

Los bots automatizados transforman Weblogs y otros sitios web con correo no deseado, enviando formularios de comentarios sin intervención del usuario. El control NoBot en el kit de herramientas de control de AJAX de ASP.NET puede ayudar a combatir esos bots.

## <a name="steps"></a>Pasos

Un enfoque común para derrotar bots es usar la prueba de convertir pública totalmente automatizada CAPTCHAs para indicar a los equipos y a los seres humanos la diferencia. Una prueba de convertir era originalmente una prueba en la que alguien necesitaba decidir si un asociado de comunicación es un usuario o un equipo. En la web, un CAPTCHA normalmente se compone de una imagen con algunas letras distorsionadas. La idea es que solo un usuario humano puede leer las letras de la imagen, mientras que los algoritmos de OCR no funcionarán.

Este enfoque tiene varias ventajas y desventajas, pero una explicación de esto está fuera del ámbito de este tutorial. No obstante, hay un control en el kit de herramientas de control de ASP.NET AJAX que proporciona un enfoque similar: `NoBot`. Es más fácil de resolver que un CAPTCHA, pero es muy fácil de usar y tarifas muy bien en sitios web como blogs en los que se considera un éxito si la mayoría de los intentos de correo no deseado son derrotas, lo que puede hacer el control de `NoBot`.

`NoBot` intercepta el PostBack del formulario Web ASP.NET actual si se cumple al menos una de estas condiciones:

- El explorador no puede resolver un rompecabezas de JavaScript (por ejemplo, cuando se desactiva JavaScript)
- El usuario envió el formulario a Fast
- La dirección IP del cliente envió el formulario demasiado a menudo en un período de tiempo determinado.

Para comprobar estas condiciones, el control `NoBot` requiere estos atributos (todos ellos opcionales):

- `ResponseMinimumDelaySeconds` cantidad mínima de segundos entre los postbacks
- `CutoffWindowSeconds` intervalo de tiempo en el que los postbacks de una IP son medidas
- `CutoffMaximumInstances` cantidad máxima de segundos por intervalo de tiempo

El marcado siguiente exige que transcurren al menos dos segundos entre los postbacks y que solo hay cinco postbacks o menos en un intervalo de 30 segundos:

[!code-aspx[Main](fighting-bots-cs/samples/sample1.aspx)]

Como es habitual, asegúrese de incluir el `ScriptManager` en la página para que se cargue la biblioteca de AJAX de ASP.NET y se pueda usar el kit de herramientas de control:

[!code-aspx[Main](fighting-bots-cs/samples/sample2.aspx)]

Dado que la mayoría de las comprobaciones que se `NoBot` realizar se producen en el lado servidor, debe comprobar el resultado de estas validaciones. Esto se puede hacer llamando al método `IsValid()` de `NoBot`. Tiene un argumento (como `out` parámetro/`ByRef` parámetro) que es de tipo `NoBotState`. Su representación de cadena contiene la razón cuando se produce un error en la comprobación y `Valid` de otro modo. El código siguiente genera un mensaje según el resultado de `NoBot`:

[!code-aspx[Main](fighting-bots-cs/samples/sample3.aspx)]

Por último, necesitará un formulario para enviar y un elemento etiqueta para generar el mensaje, y ya ha terminado.

[!code-aspx[Main](fighting-bots-cs/samples/sample4.aspx)]

Cuando ejecute este script y desactive JavaScript o envíe el formulario en los primeros dos segundos o envíe el formulario siete veces en un plazo de treinta segundos, recibirá un mensaje de error. Sin embargo, use este control con prudencia, ya que solo unos 90-95% de los usuarios tienen activado JavaScript, por lo que el 5-10% de los usuarios producirán un error en la prueba de `NoBot`.

[![este mensaje de error podría deberse a un bot](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)

Este mensaje de error puede deberse a un bot ([hacer clic para ver la imagen de tamaño completo](fighting-bots-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Siguiente](fighting-bots-vb.md)
