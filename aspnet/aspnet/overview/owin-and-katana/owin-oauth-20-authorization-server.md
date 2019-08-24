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
ms.sourcegitcommit: b95316530fa51087d6c400ff91814fe37e73f7e8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2019
ms.locfileid: "70000686"
---
# <a name="owin-oauth-20-authorization-server"></a>Servidor de autorización de OAuth 2.0 de OWIN

El [marco OAuth 2,0](http://tools.ietf.org/html/rfc6749) permite a una aplicación de terceros obtener acceso limitado a un servicio http. En lugar de usar las credenciales del propietario del recurso para tener acceso a un recurso protegido, el cliente obtiene un token de acceso (que es una cadena que denota un ámbito específico, una duración y otros atributos de acceso). Un servidor de autorización emite los tokens de acceso a los clientes de terceros con la aprobación del propietario del recurso.

OWIN define una interfaz estándar entre los servidores Web .NET y las aplicaciones Web. El objetivo de la interfaz OWIN es desacoplar el servidor y la aplicación, fomentar el desarrollo de módulos simples para el desarrollo web de .NET y, al ser un estándar abierto, estimular el ecosistema de código abierto de las herramientas de desarrollo web de .NET.

Para obtener más información, consulte [OWIN](http://owin.org/)
