---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: Asignación de usuarios de Signalr a las conexiones en Signalr 1. x | Microsoft Docs
author: bradygaster
description: En este tema se muestra cómo conservar la información sobre los usuarios y sus conexiones.
ms.author: bradyg
ms.date: 10/17/2013
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 9d948495e9b8821fcb465611b6926603c3756a19
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78450001"
---
# <a name="mapping-signalr-users-to-connections-in-signalr-1x"></a><span data-ttu-id="51942-103">Asignar usuarios de SignalR a las conexiones en SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="51942-103">Mapping SignalR Users to Connections in SignalR 1.x</span></span>

<span data-ttu-id="51942-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="51942-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="51942-105">En este tema se muestra cómo conservar la información sobre los usuarios y sus conexiones.</span><span class="sxs-lookup"><span data-stu-id="51942-105">This topic shows how to retain information about users and their connections.</span></span>

## <a name="introduction"></a><span data-ttu-id="51942-106">Introducción</span><span class="sxs-lookup"><span data-stu-id="51942-106">Introduction</span></span>

<span data-ttu-id="51942-107">Cada cliente que se conecta a un concentrador pasa un identificador de conexión único. Puede recuperar este valor en la propiedad `Context.ConnectionId` del contexto del concentrador.</span><span class="sxs-lookup"><span data-stu-id="51942-107">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="51942-108">Si la aplicación necesita asignar un usuario al identificador de conexión y conservar esa asignación, puede utilizar una de las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="51942-108">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- <span data-ttu-id="51942-109">[Almacenamiento en memoria](#inmemory), como un diccionario</span><span class="sxs-lookup"><span data-stu-id="51942-109">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="51942-110">Grupo de signalr para cada usuario</span><span class="sxs-lookup"><span data-stu-id="51942-110">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="51942-111">[Almacenamiento externo permanente](#database), como una tabla de base de datos o Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="51942-111">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="51942-112">En este tema se muestra cada una de estas implementaciones.</span><span class="sxs-lookup"><span data-stu-id="51942-112">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="51942-113">Use los métodos `OnConnected`, `OnDisconnected`y `OnReconnected` de la clase `Hub` para realizar un seguimiento del estado de conexión del usuario.</span><span class="sxs-lookup"><span data-stu-id="51942-113">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="51942-114">El mejor enfoque para la aplicación depende de:</span><span class="sxs-lookup"><span data-stu-id="51942-114">The best approach for your application depends on:</span></span>

- <span data-ttu-id="51942-115">El número de servidores web que hospedan la aplicación.</span><span class="sxs-lookup"><span data-stu-id="51942-115">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="51942-116">Si necesita obtener una lista de los usuarios conectados actualmente.</span><span class="sxs-lookup"><span data-stu-id="51942-116">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="51942-117">Si necesita conservar la información del grupo y del usuario cuando se reinicia la aplicación o el servidor.</span><span class="sxs-lookup"><span data-stu-id="51942-117">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="51942-118">Si la latencia de llamar a un servidor externo es un problema.</span><span class="sxs-lookup"><span data-stu-id="51942-118">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="51942-119">En la tabla siguiente se muestra qué enfoque funciona en estas consideraciones.</span><span class="sxs-lookup"><span data-stu-id="51942-119">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="51942-120">Más de un servidor</span><span class="sxs-lookup"><span data-stu-id="51942-120">More than one server</span></span> | <span data-ttu-id="51942-121">Obtiene una lista de usuarios conectados actualmente</span><span class="sxs-lookup"><span data-stu-id="51942-121">Get list of currently connected users</span></span> | <span data-ttu-id="51942-122">Conservar la información después de los reinicios</span><span class="sxs-lookup"><span data-stu-id="51942-122">Persist information after restarts</span></span> | <span data-ttu-id="51942-123">Rendimiento óptimo</span><span class="sxs-lookup"><span data-stu-id="51942-123">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="51942-124">En memoria</span><span class="sxs-lookup"><span data-stu-id="51942-124">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="51942-125">Grupos de usuario único</span><span class="sxs-lookup"><span data-stu-id="51942-125">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="51942-126">Permanente, externa</span><span class="sxs-lookup"><span data-stu-id="51942-126">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="51942-127">Almacenamiento en memoria</span><span class="sxs-lookup"><span data-stu-id="51942-127">In-memory storage</span></span>

<span data-ttu-id="51942-128">En los siguientes ejemplos se muestra cómo conservar la información de conexión y de usuario en un diccionario almacenado en memoria.</span><span class="sxs-lookup"><span data-stu-id="51942-128">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="51942-129">El Diccionario usa un `HashSet` para almacenar el identificador de conexión. En cualquier momento, un usuario podría tener más de una conexión a la aplicación Signalr.</span><span class="sxs-lookup"><span data-stu-id="51942-129">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="51942-130">Por ejemplo, un usuario que está conectado a través de varios dispositivos o más de una pestaña del explorador tendría más de un identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="51942-130">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="51942-131">Si la aplicación se cierra, se perderá toda la información, pero se volverá a rellenar cuando los usuarios vuelvan a establecer sus conexiones.</span><span class="sxs-lookup"><span data-stu-id="51942-131">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="51942-132">El almacenamiento en memoria no funciona si su entorno incluye más de un servidor Web, ya que cada servidor tendría una colección independiente de conexiones.</span><span class="sxs-lookup"><span data-stu-id="51942-132">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="51942-133">En el primer ejemplo se muestra una clase que administra la asignación de usuarios a las conexiones.</span><span class="sxs-lookup"><span data-stu-id="51942-133">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="51942-134">La clave de HashSet será el nombre del usuario.</span><span class="sxs-lookup"><span data-stu-id="51942-134">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="51942-135">En el ejemplo siguiente se muestra cómo usar la clase de asignación de conexión de un concentrador.</span><span class="sxs-lookup"><span data-stu-id="51942-135">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="51942-136">La instancia de la clase se almacena en un nombre de variable `_connections`.</span><span class="sxs-lookup"><span data-stu-id="51942-136">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="51942-137">Grupos de usuario único</span><span class="sxs-lookup"><span data-stu-id="51942-137">Single-user groups</span></span>

<span data-ttu-id="51942-138">Puede crear un grupo para cada usuario y, a continuación, enviar un mensaje a ese grupo cuando quiera llegar solo a ese usuario.</span><span class="sxs-lookup"><span data-stu-id="51942-138">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="51942-139">El nombre de cada grupo es el nombre del usuario.</span><span class="sxs-lookup"><span data-stu-id="51942-139">The name of each group is the name of the user.</span></span> <span data-ttu-id="51942-140">Si un usuario tiene más de una conexión, cada identificador de conexión se agrega al grupo del usuario.</span><span class="sxs-lookup"><span data-stu-id="51942-140">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="51942-141">No debe quitar manualmente el usuario del grupo cuando el usuario se desconecta.</span><span class="sxs-lookup"><span data-stu-id="51942-141">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="51942-142">Esta acción la realiza automáticamente Signalr Framework.</span><span class="sxs-lookup"><span data-stu-id="51942-142">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="51942-143">En el ejemplo siguiente se muestra cómo implementar grupos de usuario único.</span><span class="sxs-lookup"><span data-stu-id="51942-143">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="51942-144">Almacenamiento externo permanente</span><span class="sxs-lookup"><span data-stu-id="51942-144">Permanent, external storage</span></span>

<span data-ttu-id="51942-145">En este tema se muestra cómo usar una base de datos o Azure Table Storage para almacenar información de conexión.</span><span class="sxs-lookup"><span data-stu-id="51942-145">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="51942-146">Este enfoque funciona cuando hay varios servidores Web, ya que cada servidor web puede interactuar con el mismo repositorio de datos.</span><span class="sxs-lookup"><span data-stu-id="51942-146">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="51942-147">Si los servidores web dejan de funcionar o se reinicia la aplicación, no se llama al método `OnDisconnected`.</span><span class="sxs-lookup"><span data-stu-id="51942-147">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="51942-148">Por lo tanto, es posible que el repositorio de datos tenga registros para los identificadores de conexión que ya no son válidos.</span><span class="sxs-lookup"><span data-stu-id="51942-148">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="51942-149">Para limpiar estos registros huérfanos, es posible que desee invalidar cualquier conexión que se haya creado fuera de un período de tiempo relevante para su aplicación.</span><span class="sxs-lookup"><span data-stu-id="51942-149">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="51942-150">En los ejemplos de esta sección se incluye un valor para realizar un seguimiento de Cuándo se creó la conexión, pero no se muestra cómo limpiar los registros antiguos porque tal vez desee hacerlo como proceso en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="51942-150">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="51942-151">Base de datos</span><span class="sxs-lookup"><span data-stu-id="51942-151">Database</span></span>

<span data-ttu-id="51942-152">En los siguientes ejemplos se muestra cómo conservar la información de conexión y de usuario en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="51942-152">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="51942-153">Puede utilizar cualquier tecnología de acceso a datos; sin embargo, en el ejemplo siguiente se muestra cómo definir modelos mediante Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="51942-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="51942-154">Estos modelos de entidad corresponden a tablas y campos de base de datos.</span><span class="sxs-lookup"><span data-stu-id="51942-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="51942-155">La estructura de los datos puede variar considerablemente en función de los requisitos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="51942-155">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="51942-156">En el primer ejemplo se muestra cómo definir una entidad de usuario que se puede asociar a muchas entidades de conexión.</span><span class="sxs-lookup"><span data-stu-id="51942-156">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="51942-157">Después, desde el concentrador, puede realizar un seguimiento del estado de cada conexión con el código que se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="51942-157">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a><span data-ttu-id="51942-158">Almacenamiento de tablas de Azure</span><span class="sxs-lookup"><span data-stu-id="51942-158">Azure table storage</span></span>

<span data-ttu-id="51942-159">El siguiente ejemplo de Azure Table Storage es similar al ejemplo de base de datos.</span><span class="sxs-lookup"><span data-stu-id="51942-159">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="51942-160">No incluye toda la información necesaria para empezar a trabajar con el servicio Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="51942-160">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="51942-161">Para obtener más información, consulte [uso de Table Storage desde .net](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="51942-161">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="51942-162">En el ejemplo siguiente se muestra una entidad de tabla para almacenar información de conexión.</span><span class="sxs-lookup"><span data-stu-id="51942-162">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="51942-163">Divide los datos por nombre de usuario e identifica cada entidad por el identificador de conexión, por lo que un usuario puede tener varias conexiones en cualquier momento.</span><span class="sxs-lookup"><span data-stu-id="51942-163">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<span data-ttu-id="51942-164">En el concentrador, se realiza un seguimiento del estado de la conexión de cada usuario.</span><span class="sxs-lookup"><span data-stu-id="51942-164">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
