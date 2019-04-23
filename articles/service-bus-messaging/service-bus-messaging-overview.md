---
title: Información general sobre la mensajería de Azure Service Bus | Microsoft Docs
description: Descripción de la mensajería de Service Bus
services: service-bus-messaging
documentationcenter: ''
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.topic: overview
ms.date: 09/22/2018
ms.custom: mvc
ms.author: aschhab
ms.openlocfilehash: dd73fd8105de2a5fd24ffa8ad5db85da5e52228c
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "58918848"
---
# <a name="what-is-azure-service-bus"></a>Qué es Azure Service Bus

Microsoft Azure Service Bus es un agente de mensajes de [integración](https://azure.com/integration) empresarial completamente administrado. Service Bus se usa normalmente para desacoplar las aplicaciones y los servicios entre sí y es una plataforma segura y confiable para datos asincrónicos y transferencia de estado. Los datos se transfieren entre distintas aplicaciones y servicios mediante *mensajes*. Un mensaje está en formato binario, que puede contener solo texto, JSON o XML. 

Algunos escenarios de mensajería comunes son:

* Mensajería: transferencia de datos de empresa, como ventas o pedidos de compra, diarios o movimientos del inventario.
* Desacoplamiento de aplicaciones: mejora de la confiabilidad y escalabilidad de las aplicaciones y los servicios (el cliente y el servicio no necesitan estar conectados al mismo tiempo).
* Temas y suscripciones: habilitación de relaciones 1:*n* entre publicadores y suscriptores.
* Sesiones de mensajes: implementación de flujos de trabajo que requieren ordenación en los mensajes o aplazamiento de los mensajes.

## <a name="namespaces"></a>Espacios de nombres

Un espacio de nombres es un contenedor con un ámbito para todos los componentes de la mensajería. Varias colas y temas pueden residir en un único espacio de nombres, y los espacios de nombres suelen servir de contenedores de aplicación.

## <a name="queues"></a>Colas

Los mensajes se envían y se reciben desde *colas*. Las colas permiten almacenar mensajes hasta que la aplicación receptora está disponible para recibirlos y procesarlos.

![Cola](./media/service-bus-messaging-overview/about-service-bus-queue.png)

Los mensajes de las colas se ordenan y se les asigna una marca de tiempo a su llegada. Una vez aceptado, el mensaje se conserva de forma segura en almacenamiento redundante. Los mensajes se entregan en modo de *extracción*, que entrega los mensajes cuando se solicitan.

## <a name="topics"></a>Temas

También puede usar *temas* para enviar y recibir mensajes. Mientras que una cola se utiliza a menudo para la comunicación punto a punto, los temas son útiles en escenarios de publicación y suscripción.

![Tema](./media/service-bus-messaging-overview/about-service-bus-topic.png)

Los temas pueden tener varias suscripciones independientes. Un suscriptor a un tema puede recibir una copia de cada mensaje enviado a ese tema. Las suscripciones son entidades con nombre, que se crean de forma duradera pero pueden, opcionalmente, expirar o eliminarse automáticamente.

En algunos escenarios, podría no desear suscripciones individuales para recibir todos los mensajes enviados a un tema. Si es así, puede usar [reglas y filtros](topic-filters.md) para definir condiciones que desencadenan [acciones](topic-filters.md#actions) opcionales, filtrar mensajes especificados y establecer o modificar las propiedades del mensaje.

## <a name="advanced-features"></a>Características avanzadas

Service Bus también tiene características avanzadas que permiten solucionar problemas de mensajería más complejos. Las siguientes secciones describen estas características principales:

### <a name="message-sessions"></a>Sesiones de mensajes

Para realizar una garantía primero en entrar/primero en salir (FIFO) en Service Bus, use sesiones. Las [sesiones de mensajes](message-sessions.md) permiten la administración ordenada y conjunta de secuencias sin enlace de mensajes relacionados. 

### <a name="auto-forwarding"></a>Reenvío automático

La característica de [reenvío automático](service-bus-auto-forwarding.md) permite encadenar una cola o suscripción a otra cola o tema que forme parte del mismo espacio de nombres. Cuando el reenvío automático está habilitado, Service Bus elimina automáticamente los mensajes que se colocan en la primera cola o suscripción (origen) y los coloca en la segunda cola o en el segundo tema (destino).

### <a name="dead-lettering"></a>Colas de mensajes fallidos

Service Bus admite una [cola de mensajes fallidos](service-bus-dead-letter-queues.md) (DLQ) para mantener los mensajes que no se pudieron entregar a ningún destinatario o los mensajes que no se pudieron procesar. A continuación, puede eliminar mensajes de la cola DLQ y examinarlos.

### <a name="scheduled-delivery"></a>Entrega programada

Puede enviar mensajes a una cola o un tema [para su procesamiento retrasado](message-sequencing.md#scheduled-messages); por ejemplo, para programar un trabajo de forma que esté disponible para que lo procese el sistema a una hora determinada.

### <a name="message-deferral"></a>Aplazamiento de mensajes

Cuando un cliente de una cola o una suscripción recibe un mensaje que se desea procesar, pero cuyo procesamiento no es posible en ese momento debido a circunstancias especiales dentro de la aplicación, la entidad tiene la opción de [aplazar la recuperación del mensaje](message-deferral.md) para un momento posterior. El mensaje permanece en la cola o suscripción, pero se mantiene separado.

### <a name="batching"></a>Lotes

El [procesamiento por lotes en el lado del cliente](service-bus-performance-improvements.md#client-side-batching) permite que un cliente de una cola o un tema retrase el envío de un mensaje durante un período determinado. Si el cliente envía más mensajes durante este período, los transmite en un único lote. 

### <a name="transactions"></a>Transacciones

Una [transacción](service-bus-transactions.md) agrupa dos o más operaciones en un ámbito de ejecución. Service Bus admite operaciones de agrupación en una sola entidad de mensajería (cola, tema, suscripción) dentro del ámbito de una transacción.

### <a name="filtering-and-actions"></a>Filtrado y acciones

Los suscriptores pueden definir los mensajes que quieren recibir de un tema. Estos mensajes se especifican en forma de una o varias [reglas de suscripción con nombre](topic-filters.md). Con cada condición de regla de coincidencia, la suscripción crea una copia del mensaje, que se puede anotar de manera diferente para cada regla coincidente.

### <a name="auto-delete-on-idle"></a>Eliminación automática en estado inactivo

La [eliminación automática en estado inactivo](/dotnet/api/microsoft.servicebus.messaging.queuedescription.autodeleteonidle) le permite especificar un intervalo de inactividad después del cual se eliminará automáticamente la cola. La duración mínima es de 5 minutos.

### <a name="duplicate-detection"></a>Detección de duplicados

Si se produce un error que hace que el cliente tenga dudas sobre el resultado de una operación de envío, la [detección de duplicados](duplicate-detection.md) saca de dudas en estas situaciones al permitir que el remitente reenvíe el mismo mensaje y la cola o el tema descartan cualquier copia duplicada.

### <a name="sas-rbac-and-managed-identities-for-azure-resources"></a>SAS, RBAC e Identidades administradas para recursos de Azure

Service Bus admite protocolos de seguridad como las [firmas de acceso compartido](service-bus-sas.md) (SAS), el [Control de acceso basado en rol](service-bus-role-based-access-control.md) (RBAC) y [Entidades administradas para recursos de Azure](service-bus-managed-service-identity.md).

### <a name="geo-disaster-recovery"></a>Recuperación ante desastres geográfica

Cuando las regiones o los centros de datos de Azure experimentan un tiempo de inactividad, la [recuperación ante desastres geográfica](service-bus-geo-dr.md) permite que el procesamiento de datos siga funcionando en una región o un centro de datos diferentes.

### <a name="security"></a>Seguridad

Service Bus admite los protocolos estándar [AMQP 1.0](service-bus-amqp-overview.md) y [HTTP/REST](/rest/api/servicebus/).

## <a name="client-libraries"></a>Bibliotecas de clientes

Service Bus es compatible con las bibliotecas de cliente para [.NET](https://github.com/Azure/azure-service-bus-dotnet/tree/master), [Java](https://github.com/Azure/azure-service-bus-java/tree/master) y [JMS](https://github.com/Azure/azure-service-bus/tree/master/samples/Java/qpid-jms-client).

## <a name="integration"></a>Integración

Service Bus se integra completamente con los siguientes servicios de Azure:

- [Event Grid](https://azure.microsoft.com/services/event-grid/) 
- [Logic Apps](https://azure.microsoft.com/services/logic-apps/) 
- [Funciones](https://azure.microsoft.com/services/functions/) 
- [Dynamics 365](https://dynamics.microsoft.com)
- [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/)
 
## <a name="next-steps"></a>Pasos siguientes

Para comenzar a trabajar con la mensajería de Service Bus, consulte los siguientes artículos:

* [Comparación de los servicios de mensajería de Azure](../event-grid/compare-messaging-services.md?toc=%2fazure%2fservice-bus-messaging%2ftoc.json&bc=%2fazure%2fservice-bus-messaging%2fbreadcrumb%2ftoc.json)
* Más información sobre los niveles [Estándar y Premium](https://azure.microsoft.com/pricing/details/service-bus/) de Azure Service Bus y su plan de tarifa
* [Rendimiento y latencia del nivel Premium de Azure Service Bus](https://techcommunity.microsoft.com/t5/Service-Bus-blog/Premium-Messaging-How-fast-is-it/ba-p/370722)
* Pruebe las guías de inicio rápido de [.NET](service-bus-dotnet-get-started-with-queues.md), [Java](service-bus-java-how-to-use-queues.md) o [JMS](service-bus-java-how-to-use-jms-api-amqp.md)
