---
ms.openlocfilehash: d7ef9b11af8ee11e4ec84404f8cdeb0b89384a3c
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57130455"
---
## <a name="forward-request-information-with-a-proxy-or-load-balancer"></a><span data-ttu-id="5e352-101">Solicitar información con un proxy hacia delante o equilibrador de carga</span><span class="sxs-lookup"><span data-stu-id="5e352-101">Forward request information with a proxy or load balancer</span></span>

<span data-ttu-id="5e352-102">Si la aplicación se implementa detrás de un servidor proxy o un equilibrador de carga, parte de la información de la solicitud original se puede reenviar a la aplicación en los encabezados de solicitud.</span><span class="sxs-lookup"><span data-stu-id="5e352-102">If the app is deployed behind a proxy server or load balancer, some of the original request information might be forwarded to the app in request headers.</span></span> <span data-ttu-id="5e352-103">Normalmente, esta información incluye el esquema de solicitud seguro (`https`), host y dirección IP del cliente.</span><span class="sxs-lookup"><span data-stu-id="5e352-103">This information usually includes the secure request scheme (`https`), host, and client IP address.</span></span> <span data-ttu-id="5e352-104">Las aplicaciones no leen automáticamente estos encabezados de solicitud para detectar y usar la información de la solicitud original.</span><span class="sxs-lookup"><span data-stu-id="5e352-104">Apps don't automatically read these request headers to discover and use the original request information.</span></span>

<span data-ttu-id="5e352-105">Se usa el esquema de generación de vínculos que afecta el flujo de autenticación con proveedores externos.</span><span class="sxs-lookup"><span data-stu-id="5e352-105">The scheme is used in link generation that affects the authentication flow with external providers.</span></span> <span data-ttu-id="5e352-106">Perder el esquema seguro (`https`) da como resultado la aplicación de generación de direcciones URL de redirección no seguro incorrecta.</span><span class="sxs-lookup"><span data-stu-id="5e352-106">Losing the secure scheme (`https`) results in the app generating incorrect insecure redirect URLs.</span></span>

<span data-ttu-id="5e352-107">Usar software intermedio de encabezados reenviados a disposición la información de la solicitud original a la aplicación para procesar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="5e352-107">Use Forwarded Headers Middleware to make the original request information available to the app for request processing.</span></span>

<span data-ttu-id="5e352-108">Para obtener más información, consulta <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="5e352-108">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>
