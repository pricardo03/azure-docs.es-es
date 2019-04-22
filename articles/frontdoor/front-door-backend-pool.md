---
title: Los grupos de servidores back-end y back-end en el servicio de puerta de entrada de Azure | Microsoft Docs
description: Este artículo describe qué grupos de servidores back-end y back-end en la parte delantera configuración de puerta.
services: front-door
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 2372f49c7280ee5c817f3d2f98cc80a196dae5f5
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "58879206"
---
# <a name="backends-and-backend-pools-in-azure-front-door-service"></a>Back-end y grupos de back-end en Azure Front Door Service
En este artículo se describe conceptos acerca de cómo asignar la implementación de aplicaciones con el servicio de puerta de entrada de Azure. También explica los distintos términos de configuración de la puerta delantera alrededor de back-ends de aplicación.

## <a name="backends"></a>Backends
Un back-end es igual a la instancia de la implementación de una aplicación en una región. Servicio de puerta de entrada es compatible con Azure y back-ends que no son de Azure, por lo que la región no está restringida solo a las regiones de Azure. Además, puede ser su centro de datos local o en una instancia de la aplicación en otra nube.

Back-ends de servicio de puerta de entrada hacen referencia al nombre de host o dirección IP pública de la aplicación, que puede atender solicitudes de cliente. Back-ends no deben confundirse con el nivel de base de datos, capa de almacenamiento y así sucesivamente. Back-ends deben considerarse como el punto de conexión público de un back-end. Al agregar un back-end en un grupo de back-end de la puerta delantera, también debe agregar lo siguiente:

- **Tipo de host de back-end**. El tipo de recurso que desea agregar. Servicio de puerta de entrada es compatible con detección automática de la aplicación de back-ends proceden de app service, el servicio en la nube o el almacenamiento. Si desea que un recurso diferente en Azure o incluso un ajeno a Azure back-end, seleccione **host personalizado**.

    >[!IMPORTANT]
    >Durante la configuración, las API no validan si el back-end no es accesible desde la puerta delantera entornos. Asegúrese de que puerta delantera puede llegar a su back-end.

- **Nombre de host de suscripción y el back-end**. Si no ha seleccionado **host personalizado** para el tipo de host de back-end, seleccione el back-end, elija la suscripción adecuada y el nombre de host de back-end correspondiente en la interfaz de usuario.

- **Encabezado de host de back-end**. El valor del encabezado host enviado al back-end para cada solicitud. Para obtener más información, consulte [encabezado de host de back-end](#hostheader).

- **Prioridad**. Asignar prioridades a los back-end diferentes cuando desea usar un back-end del servicio principal para todo el tráfico. Además, proporcione las copias de seguridad si la réplica principal o el back-ends de copia de seguridad no están disponibles. Para obtener más información, consulte [prioridad](front-door-routing-methods.md#priority).

- **Peso**. Asignar pesos a sus back-ends diferentes para distribuir el tráfico a través de un conjunto de servidores back-end, ya sea de manera uniforme o según los coeficientes de peso. Para obtener más información, consulte [ponderaciones](front-door-routing-methods.md#weighted).

### <a name = "hostheader"></a>Encabezado de host de back-end

Las solicitudes reenviadas por la puerta de entrada a un back-end incluyen un campo de encabezado de host que usa el back-end para recuperar el recurso de destino. El valor de este campo normalmente procede del URI del back-end y tiene el host y el puerto.

Por ejemplo, una solicitud realizada para www\.contoso.com tendrá la www de encabezado de host\.contoso.com. Si usa Azure portal para configurar su back-end, el valor predeterminado para este campo es el nombre de host del backend. Si el back-end es contoso-westus.azurewebsites.net, en el portal de Azure, el valor de rellena automáticamente para el encabezado de host de back-end será westus.azurewebsites.net de contoso. Sin embargo, si usa plantillas de Azure Resource Manager u otro método sin tener que establecer explícitamente este campo, el servicio de puerta delantera enviará el nombre de host entrante como el valor para el encabezado de host. Si la solicitud se realizó para www\.contoso.com y el back-end es contoso-westus.azurewebsites.net que tiene un campo de encabezado vacío, el servicio de puerta de entrada se establecerá el encabezado de host como World Wide Web\.contoso.com.

La mayoría backends de aplicaciones (aplicaciones Web de Azure, almacenamiento de blobs y servicios en la nube) requieren el encabezado de host para que coincida con el dominio del back-end. Sin embargo, el host de front-end que enruta el back-end va a utilizar un nombre de host diferente, como www\.contoso.azurefd.net.

Si el back-end requiere el encabezado de host para que coincida con el nombre de host de back-end, asegúrese de que el encabezado de host de back-end incluye el nombre de host back-end.

#### <a name="configuring-the-backend-host-header-for-the-backend"></a>Configurar el encabezado de host de back-end para el back-end

Para configurar el **encabezado de host de back-end** field para un back-end en la sección del grupo de back-end:

1. Abra el recurso de puerta de entrada y seleccione el grupo de back-end con el back-end para configurar.

2. Si aún no lo hecho o edita uno existente, agregue un back-end.

3. Establecer el host de back-end de campo de encabezado en un valor personalizado o déjelo en blanco. El nombre de host para la solicitud entrante se utilizará como el valor del encabezado de host.

## <a name="backend-pools"></a>Grupos de back-end
Un grupo de back-end en la parte delantera puerta servicio hace referencia al conjunto de servidores back-end que reciben tráfico similar para las aplicaciones. En otras palabras, es una agrupación lógica de las instancias de aplicación en todo el mundo que recibirá el mismo tráfico y responderá con el comportamiento esperado. Estos servidores back-end se implementa en diferentes regiones o en la misma región. Todos los servidores back-end puede estar en modo de implementación activo/activo o lo que se define como configuración activa/pasiva.

Un grupo de back-end define cómo se deben evaluar el back-ends diferentes a través de los sondeos de estado. También define cómo se produce equilibrio de carga entre ellos.

### <a name="health-probes"></a>Sondeos de estado
Servicio de puerta principal envía solicitudes periódicas de sondeo HTTP/HTTPS a cada uno de los back-end configurados. Las solicitudes de sondeo determinan la proximidad y el estado de cada back-end para cargar equilibrar las solicitudes del usuario final. Configuración de sondeo de estado para un grupo de back-end define cómo se sondea el estado de mantenimiento de back-ends de aplicación. Las siguientes opciones están disponibles para la configuración de equilibrio de carga:

- **Ruta de acceso**. La URL utilizada para las solicitudes de sondeo para todos los back-ends en el grupo de back-end. Por ejemplo, si uno de los back-end es contoso-westus.azurewebsites.net y la ruta de acceso se establece en /probe/test.aspx, a continuación, entornos de servicio de puerta de entrada, suponiendo que el protocolo está establecido en HTTP, enviará las solicitudes de sondeo de estado HTTP\:/ / contoso-westus.azurewebsites.net/probe/test.aspx.

- **Protocolo**. Define si se debe enviar las solicitudes de sondeo de estado del servicio de puerta de entrada a los back-end con el protocolo HTTP o HTTPS.

- **Intervalo (segundos)**. Define la frecuencia de los sondeos de estado para los back-end o los intervalos en la que cada uno de los entornos de la puerta delantera envía un sondeo.

    >[!NOTE]
    >Para las conmutaciones por error más rápida, establezca el intervalo en un valor inferior. Cuanto menor sea el valor, cuanto mayor sea el volumen de sondeo de mantenimiento reciben los back-end. Por ejemplo, si el intervalo se establece en 30 segundos con 90 entornos puerta delantera o puntos de presencia global, cada back-end recibirá acerca de las solicitudes de sondeo de 3 a 5 por segundo.

Para obtener más información, consulte [sondeos de estado](front-door-health-probes.md).

### <a name="load-balancing-settings"></a>Configuración de equilibrio de carga
Configuración del equilibrio de carga para el grupo de back-end define cómo se evalúan los sondeos de estado. Esta configuración determina si el back-end es correcto o incorrecto. También comprueban cómo el tráfico de equilibrio de carga entre diferentes servidores back-end en el grupo de back-end. Las siguientes opciones están disponibles para la configuración de equilibrio de carga:

- **El tamaño del ejemplo**. Identifica el número de muestras de los sondeos de estado se debe tener en cuenta para la evaluación de mantenimiento de back-end.

- **Tamaño de muestra correcta**. Define el tamaño de muestra como se mencionó anteriormente, el número de muestras correcta necesaria para llamar al back-end en buen estado. Por ejemplo, suponga un intervalo de sondeo de mantenimiento de la puerta delantera es de 30 segundos, tamaño de muestra es de 5 segundos y tamaño de muestra correcta es de 3 segundos. Cada vez que se evalúe el mantenimiento sondea el back-end, nos centramos en las cinco últimas muestras más de 150 segundos (5 x 30). Se requieren al menos tres de los sondeos correctos para declarar el back-end como en buen estado.

- **Sensibilidad a la latencia (latencia adicional)**. Define si desea que la puerta de entrada para enviar la solicitud al back-ends dentro del intervalo de latencia medida confidencialidad o reenviar la solicitud para el back-end más cercano.

Para obtener más información, consulte [menor latencia según el método de enrutamiento](front-door-routing-methods.md#latency).

## <a name="next-steps"></a>Pasos siguientes

- [Crear un perfil de la puerta delantera](quickstart-create-front-door.md)
- [Cómo funciona la puerta de entrada](front-door-routing-architecture.md)