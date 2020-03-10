---
uid: web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
title: '[How Do I]: Persist the State of a User Control During a Postback | Microsoft Docs'
author: rick-anderson
description: En este vídeo, Chris Pels muestra cómo conservar el estado de uno o varios objetos en un control de usuario. En primer lugar, se crea un control de usuario que representa el abilit...
ms.author: riande
ms.date: 04/02/2009
ms.assetid: d1bca4c6-838c-40f7-87ec-80bb67e483e5
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
msc.type: video
ms.openlocfilehash: c87bd6c5c993a1bde8f8a84f6d53b431e54541d9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78516277"
---
# <a name="how-do-i-persist-the-state-of-a-user-control-during-a-postback"></a>[Cómo]: conservar el estado de un control de usuario durante un postback
[How Do I]: Persist the State of a User Control During a Postback

<span data-ttu-id="63079-104">por [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="63079-104">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="63079-105">En este vídeo, Chris Pels muestra cómo conservar el estado de uno o varios objetos en un control de usuario.</span><span class="sxs-lookup"><span data-stu-id="63079-105">In this video Chris Pels shows how to persist the state of one or more objects in a user control.</span></span> <span data-ttu-id="63079-106">En primer lugar, se crea un control de usuario que representa la capacidad de un usuario para especificar criterios de filtro para una búsqueda.</span><span class="sxs-lookup"><span data-stu-id="63079-106">First, a user control is created that represents the ability for a user to specify filter criteria for a search.</span></span> <span data-ttu-id="63079-107">Además, se crea una clase de filtro complementaria para almacenar la información de filtro.</span><span class="sxs-lookup"><span data-stu-id="63079-107">In addition, a companion Filter class is created to store the filter information.</span></span> <span data-ttu-id="63079-108">Se han agregado varios elementos de la interfaz de usuario al control de filtro junto con algunos métodos y propiedades para almacenar la información de filtro actual en la instancia de clase de filtro.</span><span class="sxs-lookup"><span data-stu-id="63079-108">Several user interface elements are added to the filter control along with some methods and properties to store the current filter information in the Filter class instance.</span></span> <span data-ttu-id="63079-109">A continuación, la persistencia del control de usuario se implementa mediante el método RegisterRequiresControlState y los métodos de guardado y restauración asociados.</span><span class="sxs-lookup"><span data-stu-id="63079-109">Next, the user control persistence is implemented using the RegisterRequiresControlState method and associated Save/Restore methods.</span></span> <span data-ttu-id="63079-110">Estos métodos almacenan la instancia de la clase de filtro y sus datos durante las devoluciones de página.</span><span class="sxs-lookup"><span data-stu-id="63079-110">These methods store the instance of the filter class and its data during page postbacks.</span></span> <span data-ttu-id="63079-111">Por último, hay una explicación de cómo almacenar varios objetos en la implementación de estado del control.</span><span class="sxs-lookup"><span data-stu-id="63079-111">Finally, there is a discussion of how to store multiple objects in control state implementation.</span></span>

[<span data-ttu-id="63079-112">&#9654;Ver vídeo (23 minutos)</span><span class="sxs-lookup"><span data-stu-id="63079-112">&#9654; Watch video (23 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-persist-the-state-of-a-user-control-during-a-postback)
