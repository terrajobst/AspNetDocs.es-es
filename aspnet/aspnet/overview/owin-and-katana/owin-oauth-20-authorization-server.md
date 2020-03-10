---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: Servidor de autorización de OAuth 2,0 OWIN | Microsoft Docs
author: hongyes
description: Este tutorial le guiará en la implementación de un servidor de autorización de OAuth 2,0 mediante middleware de OAuth de OWIN. Este es un tutorial avanzado que solo outlin...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: 39c78ee57f791a94af7a5fb66c3ac42d7239760f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78500239"
---
# <a name="owin-oauth-20-authorization-server"></a><span data-ttu-id="dda6b-104">Servidor de autorización de OAuth 2.0 de OWIN</span><span class="sxs-lookup"><span data-stu-id="dda6b-104">OWIN OAuth 2.0 Authorization Server</span></span>

<span data-ttu-id="dda6b-105">El [marco OAuth 2,0](http://tools.ietf.org/html/rfc6749) permite a una aplicación de terceros obtener acceso limitado a un servicio http.</span><span class="sxs-lookup"><span data-stu-id="dda6b-105">The [OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749) enables a third-party app to obtain limited access to an HTTP service.</span></span> <span data-ttu-id="dda6b-106">En lugar de usar las credenciales del propietario del recurso para tener acceso a un recurso protegido, el cliente obtiene un token de acceso (que es una cadena que denota un ámbito específico, una duración y otros atributos de acceso).</span><span class="sxs-lookup"><span data-stu-id="dda6b-106">Instead of using the resource owner's credentials to access a protected resource, the client obtains an access token (which is a string denoting a specific scope, lifetime, and other access attributes).</span></span> <span data-ttu-id="dda6b-107">Un servidor de autorización emite los tokens de acceso a los clientes de terceros con la aprobación del propietario del recurso.</span><span class="sxs-lookup"><span data-stu-id="dda6b-107">Access tokens are issued to third-party clients by an authorization server with the approval of the resource owner.</span></span>

<span data-ttu-id="dda6b-108">OWIN define una interfaz estándar entre los servidores Web .NET y las aplicaciones Web.</span><span class="sxs-lookup"><span data-stu-id="dda6b-108">OWIN defines a standard interface between .NET web servers and web applications.</span></span> <span data-ttu-id="dda6b-109">El objetivo de la interfaz OWIN es desacoplar el servidor y la aplicación, fomentar el desarrollo de módulos simples para el desarrollo web de .NET y, al ser un estándar abierto, estimular el ecosistema de código abierto de las herramientas de desarrollo web de .NET.</span><span class="sxs-lookup"><span data-stu-id="dda6b-109">The goal of the OWIN interface is to decouple server and application, encourage the development of simple modules for .NET web development, and, by being an open standard, stimulate the open source ecosystem of .NET web development tools.</span></span>

<span data-ttu-id="dda6b-110">Para obtener más información, consulte [OWIN](http://owin.org/)</span><span class="sxs-lookup"><span data-stu-id="dda6b-110">For more information, see [OWIN](http://owin.org/)</span></span>
