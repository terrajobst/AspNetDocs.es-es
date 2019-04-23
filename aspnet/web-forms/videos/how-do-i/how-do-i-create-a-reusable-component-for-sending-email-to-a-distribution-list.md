---
uid: web-forms/videos/how-do-i/how-do-i-create-a-reusable-component-for-sending-email-to-a-distribution-list
title: '[¿Cómo lo hago?:] Crear un componente reutilizable para enviar correo electrónico a una lista de distribución | Microsoft Docs'
author: rick-anderson
description: En este vídeo Chris Pels muestra cómo crear un componente que se puede usar en varias páginas web y sitios web que envía mensajes de correo electrónico a una lista de destinatarios. Primera persona...
ms.author: riande
ms.date: 12/04/2008
ms.assetid: 13dd3a26-c210-432e-91fe-355c979060b3
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-create-a-reusable-component-for-sending-email-to-a-distribution-list
msc.type: video
ms.openlocfilehash: 6a117d0bf029245c4929e903e0c0494f6ba2b072
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59400404"
---
# <a name="how-do-i-create-a-reusable-component-for-sending-email-to-a-distribution-list"></a><span data-ttu-id="689bf-104">[¿Cómo lo hago?:] Crear un componente reutilizable para enviar correo electrónico a una lista de distribución</span><span class="sxs-lookup"><span data-stu-id="689bf-104">[How Do I:] Create a Reusable Component for Sending Email to a Distribution List</span></span>

<span data-ttu-id="689bf-105">por [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="689bf-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="689bf-106">En este vídeo Chris Pels muestra cómo crear un componente que se puede usar en varias páginas web y sitios web que envía mensajes de correo electrónico a una lista de destinatarios.</span><span class="sxs-lookup"><span data-stu-id="689bf-106">In this video Chris Pels shows how to create a component that can be used on multiple web pages and web sites that sends emails to a list of recipients.</span></span> <span data-ttu-id="689bf-107">En primer lugar, el sitio web ASP.NET se configurará para enviar correo electrónico mediante el &lt;mailSettings&gt; en el archivo web.config.</span><span class="sxs-lookup"><span data-stu-id="689bf-107">First, the ASP.NET web site will be configured to send email using the &lt;mailSettings&gt; in the web.config file.</span></span> <span data-ttu-id="689bf-108">A continuación, se crean una clase y varios métodos para leer una lista de destinatarios de un origen de datos (DB, XML, etc.) y enviar un mensaje de correo electrónico a cada uno de los destinatarios mediante las clases System.Net.Mail.</span><span class="sxs-lookup"><span data-stu-id="689bf-108">Then a class and several methods are created to read a list of recipients from a data source (DB, XML, etc.) and send an email message to each of the recipients using the System.Net.Mail classes.</span></span> <span data-ttu-id="689bf-109">Como parte de esta excepción proceso control se incluye.</span><span class="sxs-lookup"><span data-stu-id="689bf-109">As part of this process exception handling is included.</span></span> <span data-ttu-id="689bf-110">Además, se crea una interfaz de usuario para permitir al usuario especificar elementos como la dirección de, asunto, agregue un archivo adjunto, etcetera.</span><span class="sxs-lookup"><span data-stu-id="689bf-110">In addition, a user interface is created to allow the user to specify items such as the From address, Subject, add an attachment, etc.</span></span>

[<span data-ttu-id="689bf-111">&#9654;Vea el vídeo (35 minutos)</span><span class="sxs-lookup"><span data-stu-id="689bf-111">&#9654; Watch video (35 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-create-a-reusable-component-for-sending-email-to-a-distribution-list)
