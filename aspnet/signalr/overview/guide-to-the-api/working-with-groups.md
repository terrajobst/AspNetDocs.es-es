---
uid: signalr/overview/guide-to-the-api/working-with-groups
title: Trabajar con grupos en Signalr | Microsoft Docs
author: bradygaster
description: En este tema se describe cómo conservar la información de pertenencia a grupos con la API de Hub.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: cd378ecd-3e9e-4236-b902-65916d85a048
msc.legacyurl: /signalr/overview/guide-to-the-api/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 46dd952fc4902b37c981a111dfa344dad79bb668
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78450085"
---
# <a name="working-with-groups-in-signalr"></a><span data-ttu-id="7b405-103">Trabajar con grupos en SignalR</span><span class="sxs-lookup"><span data-stu-id="7b405-103">Working with Groups in SignalR</span></span>

<span data-ttu-id="7b405-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="7b405-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="7b405-105">En este tema se describe cómo agregar usuarios a grupos y conservar la información de pertenencia a grupos.</span><span class="sxs-lookup"><span data-stu-id="7b405-105">This topic describes how to add users to groups and persist group membership information.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="7b405-106">Versiones de software utilizadas en este tema</span><span class="sxs-lookup"><span data-stu-id="7b405-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="7b405-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="7b405-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="7b405-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="7b405-108">.NET 4.5</span></span>
> - <span data-ttu-id="7b405-109">Signalr versión 2</span><span class="sxs-lookup"><span data-stu-id="7b405-109">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="7b405-110">Versiones anteriores de este tema</span><span class="sxs-lookup"><span data-stu-id="7b405-110">Previous versions of this topic</span></span>
>
> <span data-ttu-id="7b405-111">Para obtener información sobre las versiones anteriores de Signalr, consulte [versiones anteriores de signalr](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="7b405-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="7b405-112">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="7b405-112">Questions and comments</span></span>
>
> <span data-ttu-id="7b405-113">Deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="7b405-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="7b405-114">Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [stackoverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="7b405-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="7b405-115">Información general</span><span class="sxs-lookup"><span data-stu-id="7b405-115">Overview</span></span>

<span data-ttu-id="7b405-116">Los grupos de Signalr proporcionan un método para difundir mensajes a subconjuntos específicos de clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="7b405-116">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="7b405-117">Un grupo puede tener cualquier número de clientes y un cliente puede ser miembro de cualquier número de grupos.</span><span class="sxs-lookup"><span data-stu-id="7b405-117">A group can have any number of clients, and a client can be a member of any number of groups.</span></span> <span data-ttu-id="7b405-118">No tiene que crear grupos explícitamente.</span><span class="sxs-lookup"><span data-stu-id="7b405-118">You don't have to explicitly create groups.</span></span> <span data-ttu-id="7b405-119">En efecto, se crea automáticamente un grupo la primera vez que se especifica su nombre en una llamada a groups. Add, y se elimina cuando se quita la última conexión de la pertenencia a él.</span><span class="sxs-lookup"><span data-stu-id="7b405-119">In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.</span></span> <span data-ttu-id="7b405-120">Para obtener una introducción al uso de grupos, consulte [Administración de la pertenencia a grupos desde la clase hub](hubs-api-guide-server.md#groupsfromhub) en la guía de hubs API-Server.</span><span class="sxs-lookup"><span data-stu-id="7b405-120">For an introduction to using groups, see [How to manage group membership from the Hub class](hubs-api-guide-server.md#groupsfromhub) in the Hubs API - Server Guide.</span></span>

<span data-ttu-id="7b405-121">No hay ninguna API para obtener una lista de pertenencia a grupos o una lista de grupos.</span><span class="sxs-lookup"><span data-stu-id="7b405-121">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="7b405-122">Signalr envía mensajes a clientes y grupos basados en un modelo pub/sub, y el servidor no mantiene listas de grupos o pertenencias a grupos.</span><span class="sxs-lookup"><span data-stu-id="7b405-122">SignalR sends messages to clients and groups based on a pub/sub model, and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="7b405-123">Esto ayuda a maximizar la escalabilidad, ya que cada vez que se agrega un nodo a una granja de servidores Web, cualquier Estado que mantenga Signalr debe propagarse al nuevo nodo.</span><span class="sxs-lookup"><span data-stu-id="7b405-123">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<span data-ttu-id="7b405-124">Cuando se agrega un usuario a un grupo mediante el método `Groups.Add`, el usuario recibe los mensajes dirigidos a ese grupo mientras dure la conexión actual, pero la pertenencia del usuario a ese grupo no se conserva más allá de la conexión actual.</span><span class="sxs-lookup"><span data-stu-id="7b405-124">When you add a user to a group using the `Groups.Add` method, the user receives messages directed to that group for the duration of the current connection, but the user's membership in that group is not persisted beyond the current connection.</span></span> <span data-ttu-id="7b405-125">Si desea conservar permanentemente información acerca de los grupos y la pertenencia a grupos, debe almacenar los datos en un repositorio, como una base de datos o Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="7b405-125">If you want to permanently retain information about groups and group membership, you must store that data in a repository such as a database or Azure table storage.</span></span> <span data-ttu-id="7b405-126">Después, cada vez que un usuario se conecta a la aplicación, recupera del repositorio al que pertenece el usuario y agrega manualmente el usuario a esos grupos.</span><span class="sxs-lookup"><span data-stu-id="7b405-126">Then, each time a user connects to your application, you retrieve from the repository which groups the user belongs to, and manually add that user to those groups.</span></span>

<span data-ttu-id="7b405-127">Al volver a conectarse después de una interrupción temporal, el usuario vuelve a unirse automáticamente a los grupos asignados previamente.</span><span class="sxs-lookup"><span data-stu-id="7b405-127">When reconnecting after a temporary disruption, the user automatically re-joins the previously-assigned groups.</span></span> <span data-ttu-id="7b405-128">La Unión automática de un grupo solo se aplica cuando se vuelve a conectar, no al establecer una nueva conexión.</span><span class="sxs-lookup"><span data-stu-id="7b405-128">Automatically rejoining a group only applies when reconnecting, not when establishing a new connection.</span></span> <span data-ttu-id="7b405-129">Se pasa un token firmado digitalmente desde el cliente que contiene la lista de grupos asignados previamente.</span><span class="sxs-lookup"><span data-stu-id="7b405-129">A digitally-signed token is passed from the client that contains the list of previously-assigned groups.</span></span> <span data-ttu-id="7b405-130">Si desea comprobar si el usuario pertenece a los grupos solicitados, puede invalidar el comportamiento predeterminado.</span><span class="sxs-lookup"><span data-stu-id="7b405-130">If you want to verify whether the user belongs to the requested groups, you can override the default behavior.</span></span>

<span data-ttu-id="7b405-131">En este tema, se incluyen las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="7b405-131">This topic includes the following sections:</span></span>

- [<span data-ttu-id="7b405-132">Adición y eliminación de usuarios</span><span class="sxs-lookup"><span data-stu-id="7b405-132">Adding and removing users</span></span>](#add)
- [<span data-ttu-id="7b405-133">Llamar a miembros de un grupo</span><span class="sxs-lookup"><span data-stu-id="7b405-133">Calling members of a group</span></span>](#call)
- [<span data-ttu-id="7b405-134">Almacenar la pertenencia a un grupo en una base de datos</span><span class="sxs-lookup"><span data-stu-id="7b405-134">Storing group membership in a database</span></span>](#storedatabase)
- [<span data-ttu-id="7b405-135">Almacenamiento de la pertenencia a grupos en Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="7b405-135">Storing group membership in Azure table storage</span></span>](#storeazuretable)
- [<span data-ttu-id="7b405-136">Comprobando la pertenencia al grupo al volver a conectar</span><span class="sxs-lookup"><span data-stu-id="7b405-136">Verifying group membership when reconnecting</span></span>](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a><span data-ttu-id="7b405-137">Adición y eliminación de usuarios</span><span class="sxs-lookup"><span data-stu-id="7b405-137">Adding and removing users</span></span>

<span data-ttu-id="7b405-138">Para agregar o quitar usuarios de un grupo, debe llamar a los métodos [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) o [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) y pasar el identificador de conexión del usuario y el nombre del grupo como parámetros.</span><span class="sxs-lookup"><span data-stu-id="7b405-138">To add or remove users from a group, you call the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) or [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods, and pass in the user's connection id and group's name as parameters.</span></span> <span data-ttu-id="7b405-139">No es necesario quitar manualmente un usuario de un grupo cuando finaliza la conexión.</span><span class="sxs-lookup"><span data-stu-id="7b405-139">You do not need to manually remove a user from a group when the connection ends.</span></span>

<span data-ttu-id="7b405-140">En el ejemplo siguiente se muestran los métodos `Groups.Add` y `Groups.Remove` usados en métodos de concentrador.</span><span class="sxs-lookup"><span data-stu-id="7b405-140">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

<span data-ttu-id="7b405-141">Los métodos `Groups.Add` y `Groups.Remove` se ejecutan de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="7b405-141">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span>

<span data-ttu-id="7b405-142">Si desea agregar un cliente a un grupo y enviar inmediatamente un mensaje al cliente mediante el grupo, debe asegurarse de que el método groups. Add finaliza primero.</span><span class="sxs-lookup"><span data-stu-id="7b405-142">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the Groups.Add method finishes first.</span></span> <span data-ttu-id="7b405-143">En los siguientes ejemplos de código se muestra cómo hacerlo.</span><span class="sxs-lookup"><span data-stu-id="7b405-143">The following code examples show how to do that.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

<span data-ttu-id="7b405-144">En general, no debe incluir `await` al llamar al método `Groups.Remove` porque es posible que el identificador de conexión que está intentando quitar ya no esté disponible.</span><span class="sxs-lookup"><span data-stu-id="7b405-144">In general, you should not include `await` when calling the `Groups.Remove` method because the connection id that you are trying to remove might no longer be available.</span></span> <span data-ttu-id="7b405-145">En ese caso, se produce `TaskCanceledException` una vez que se agota el tiempo de espera de la solicitud. Si la aplicación debe asegurarse de que el usuario se ha quitado del grupo antes de enviar un mensaje al grupo, puede Agregar `await` antes de `Groups.Remove`y, a continuación, capturar la excepción `TaskCanceledException` que se puede producir.</span><span class="sxs-lookup"><span data-stu-id="7b405-145">In that case, `TaskCanceledException` is thrown after the request times out. If your application must ensure that the user has been removed from the group before sending a message to the group, you can add `await` before `Groups.Remove`, and then catch the `TaskCanceledException` exception that might be thrown.</span></span>

<a id="call"></a>

## <a name="calling-members-of-a-group"></a><span data-ttu-id="7b405-146">Llamar a miembros de un grupo</span><span class="sxs-lookup"><span data-stu-id="7b405-146">Calling members of a group</span></span>

<span data-ttu-id="7b405-147">Puede enviar mensajes a todos los miembros de un grupo o solo a los miembros especificados del grupo, tal y como se muestra en los ejemplos siguientes.</span><span class="sxs-lookup"><span data-stu-id="7b405-147">You can send messages to all of the members of a group or only specified members of the group, as shown in the following examples.</span></span>

- <span data-ttu-id="7b405-148">**Todos** los clientes conectados en un grupo especificado.</span><span class="sxs-lookup"><span data-stu-id="7b405-148">**All** connected clients in a specified group.</span></span>

    [!code-css[Main](working-with-groups/samples/sample3.css)]
- <span data-ttu-id="7b405-149">Todos los clientes conectados en un grupo especificado, **excepto los clientes especificados**, identificados por el identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="7b405-149">All connected clients in a specified group **except the specified clients**, identified by connection ID.</span></span>

    [!code-csharp[Main](working-with-groups/samples/sample4.cs)]
- <span data-ttu-id="7b405-150">Todos los clientes conectados en un grupo especificado, **excepto el cliente que llama**.</span><span class="sxs-lookup"><span data-stu-id="7b405-150">All connected clients in a specified group **except the calling client**.</span></span>

    [!code-css[Main](working-with-groups/samples/sample5.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a><span data-ttu-id="7b405-151">Almacenar la pertenencia a un grupo en una base de datos</span><span class="sxs-lookup"><span data-stu-id="7b405-151">Storing group membership in a database</span></span>

<span data-ttu-id="7b405-152">En los siguientes ejemplos se muestra cómo conservar la información del grupo y del usuario en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="7b405-152">The following examples show how to retain group and user information in a database.</span></span> <span data-ttu-id="7b405-153">Puede utilizar cualquier tecnología de acceso a datos; sin embargo, en el ejemplo siguiente se muestra cómo definir modelos mediante Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="7b405-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="7b405-154">Estos modelos de entidad corresponden a tablas y campos de base de datos.</span><span class="sxs-lookup"><span data-stu-id="7b405-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="7b405-155">La estructura de los datos puede variar considerablemente en función de los requisitos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7b405-155">Your data structure could vary considerably depending on the requirements of your application.</span></span> <span data-ttu-id="7b405-156">En este ejemplo se incluye una clase denominada `ConversationRoom` que sería única para una aplicación que permite a los usuarios unir conversaciones sobre diferentes asuntos, como deportes o jardinería.</span><span class="sxs-lookup"><span data-stu-id="7b405-156">This example includes a class named `ConversationRoom` which would be unique to an application that enables users to join conversations about different subjects, such as sports or gardening.</span></span> <span data-ttu-id="7b405-157">En este ejemplo también se incluye una clase para las conexiones.</span><span class="sxs-lookup"><span data-stu-id="7b405-157">This example also includes a class for the connections.</span></span> <span data-ttu-id="7b405-158">La clase de conexión no es absolutamente necesaria para realizar el seguimiento de la pertenencia a grupos pero suele formar parte de una solución sólida para el seguimiento de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="7b405-158">The connection class is not absolutely required for tracking group membership but is frequently part of robust solution to tracking users.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample6.cs)]

<span data-ttu-id="7b405-159">A continuación, en el concentrador, puede recuperar la información de grupo y de usuario de la base de datos y agregar manualmente el usuario a los grupos adecuados.</span><span class="sxs-lookup"><span data-stu-id="7b405-159">Then, in the hub, you can retrieve the group and user information from the database and manually add the user to the appropriate groups.</span></span> <span data-ttu-id="7b405-160">En el ejemplo no se incluye código para realizar el seguimiento de las conexiones de usuario.</span><span class="sxs-lookup"><span data-stu-id="7b405-160">The example does not include code for tracking the user connections.</span></span> <span data-ttu-id="7b405-161">En este ejemplo, la palabra clave `await` no se aplica antes de `Groups.Add` porque no se envía inmediatamente un mensaje a los miembros del grupo.</span><span class="sxs-lookup"><span data-stu-id="7b405-161">In this example, the `await` keyword is not applied before `Groups.Add` because a message is not immediately sent to members of the group.</span></span> <span data-ttu-id="7b405-162">Si desea enviar un mensaje a todos los miembros del grupo inmediatamente después de agregar el nuevo miembro, querrá aplicar la palabra clave `await` para asegurarse de que se ha completado la operación asincrónica.</span><span class="sxs-lookup"><span data-stu-id="7b405-162">If you want to send a message to all members of the group immediately after adding the new member, you would want to apply the `await` keyword to make sure the asynchronous operation has completed.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a><span data-ttu-id="7b405-163">Almacenamiento de la pertenencia a grupos en Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="7b405-163">Storing group membership in Azure table storage</span></span>

<span data-ttu-id="7b405-164">El uso de Azure Table Storage para almacenar información de grupos y usuarios es similar al uso de una base de datos.</span><span class="sxs-lookup"><span data-stu-id="7b405-164">Using Azure table storage to store group and user information is similar to using a database.</span></span> <span data-ttu-id="7b405-165">En el ejemplo siguiente se muestra una entidad de tabla que almacena el nombre de usuario y el nombre de grupo.</span><span class="sxs-lookup"><span data-stu-id="7b405-165">The following example shows a table entity that stores the user name and group name.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<span data-ttu-id="7b405-166">En el concentrador, se recuperan los grupos asignados cuando el usuario se conecta.</span><span class="sxs-lookup"><span data-stu-id="7b405-166">In the hub, you retrieve the assigned groups when the user connects.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a><span data-ttu-id="7b405-167">Comprobando la pertenencia al grupo al volver a conectar</span><span class="sxs-lookup"><span data-stu-id="7b405-167">Verifying group membership when reconnecting</span></span>

<span data-ttu-id="7b405-168">De forma predeterminada, Signalr vuelve a asignar automáticamente un usuario a los grupos adecuados cuando se vuelve a conectar desde una interrupción temporal, como cuando una conexión se quita y se vuelve a establecer antes de que se agote el tiempo de espera de la conexión. La información del grupo del usuario se pasa en un token cuando se vuelve a conectar y ese token se comprueba en el servidor.</span><span class="sxs-lookup"><span data-stu-id="7b405-168">By default, SignalR automatically re-assigns a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. The user's group information is passed in a token when reconnecting, and that token is verified on the server.</span></span> <span data-ttu-id="7b405-169">Para obtener información sobre el proceso de comprobación para volver a unirse a los usuarios a grupos, consulte [reunir grupos cuando se vuelve a conectar](../security/introduction-to-security.md#rejoingroup).</span><span class="sxs-lookup"><span data-stu-id="7b405-169">For information about the verification process for rejoining users to groups, see [Rejoining groups when reconnecting](../security/introduction-to-security.md#rejoingroup).</span></span>

<span data-ttu-id="7b405-170">En general, debe usar el comportamiento predeterminado de volver a unirse automáticamente a los grupos al reconectarse.</span><span class="sxs-lookup"><span data-stu-id="7b405-170">In general, you should use the default behavior of automatically rejoining groups on reconnect.</span></span> <span data-ttu-id="7b405-171">Los grupos de signalr no están diseñados como un mecanismo de seguridad para restringir el acceso a los datos confidenciales.</span><span class="sxs-lookup"><span data-stu-id="7b405-171">SignalR groups are not intended as a security mechanism for restricting access to sensitive data.</span></span> <span data-ttu-id="7b405-172">Sin embargo, si la aplicación debe comprobar la pertenencia a grupos de un usuario al volver a conectarlo, puede invalidar el comportamiento predeterminado.</span><span class="sxs-lookup"><span data-stu-id="7b405-172">However, if your application must double-check a user's group membership when reconnecting, you can override the default behavior.</span></span> <span data-ttu-id="7b405-173">Cambiar el comportamiento predeterminado puede Agregar una carga a la base de datos porque se debe recuperar la pertenencia a grupos de un usuario para cada reconexión, en lugar de solo cuando el usuario se conecta.</span><span class="sxs-lookup"><span data-stu-id="7b405-173">Changing the default behavior can add a burden to your database because a user's group membership must be retrieved for each reconnection rather than just when the user connects.</span></span>

<span data-ttu-id="7b405-174">Si debe comprobar la pertenencia a grupos al volver a conectar, cree un nuevo módulo de canalización de concentrador que devuelva una lista de grupos asignados, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="7b405-174">If you must verify group membership on reconnect, create a new hub pipeline module that returns a list of assigned groups, as shown below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<span data-ttu-id="7b405-175">A continuación, agregue ese módulo a la canalización del concentrador, tal y como se resalta a continuación.</span><span class="sxs-lookup"><span data-stu-id="7b405-175">Then, add that module to the hub pipeline, as highlighted below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample11.cs?highlight=4)]
