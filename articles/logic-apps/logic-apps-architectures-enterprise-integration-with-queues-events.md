---
title: Arquitectura de referencia de los servicios de integración de Azure
description: Arquitectura de referencia que muestra cómo implementar un patrón de integración empresarial con Logic Apps, API Management, Service Bus y Event Grid
author: mattfarm
manager: jonfan
editor: ''
services: logic-apps api-management
documentationcenter: ''
ms.assetid: ''
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 06/15/2018
ms.author: LADocs; estfan
ms.openlocfilehash: db3350b3c70f6e6f35949e8423334061849b7b37
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232212"
---
# <a name="enterprise-integration-with-queues-and-events-reference-architecture"></a>Integración empresarial con colas y eventos: arquitectura de referencia

## <a name="overview"></a>Información general

Esta arquitectura de referencia muestra un conjunto de prácticas demostradas para una aplicación web que usa los servicios de integración de Azure. Esta arquitectura puede servir de base de muchos patrones de aplicaciones distintos que requieren API HTTP, flujo de trabajo y orquestación.

*Hay muchas aplicaciones posibles de tecnología de integración, desde una aplicación sencilla punto a punto hasta un bus de servicio empresarial completo. Esta serie de arquitectura expone los componentes reutilizables para la creación de cualquier aplicación de integración: los arquitectos deben tener en cuenta los componentes que necesitan para sus aplicaciones e infraestructura.*

![Diagrama de arquitectura: integración empresarial con colas y eventos](media/logic-apps-architectures-enterprise-integration-with-queues-events/integr_queues_events_arch_diagram.png)

## <a name="architecture"></a>Architecture

Esta arquitectura se basa en la que se muestra en la [integración empresarial sencilla](logic-apps-architectures-simple-enterprise-integration.md). Incluye los siguientes componentes:

- Un grupo de recursos Un grupo de recursos es un contenedor lógico de recursos de Azure.
- Azure API Management. Azure API Management es una plataforma totalmente administrada para publicar, proteger y transformar API HTTP.
- Portal para desarrolladores de Azure API Management. Cada instancia de Azure API Management incluye un portal para desarrolladores, que proporciona acceso a documentación y códigos de ejemplo, además de la posibilidad de probar una API.
- Azure Logic Apps. Logic Apps es una plataforma sin servidor para la creación de integración y flujo de trabajo de ámbito empresarial.
- Conectores. Logic Apps usa los conectores para conectarse a servicios de uso habitual. Logic Apps ya dispone de cientos de conectores diferentes, pero también se pueden crear con un conector personalizado.
- Azure Service Bus. Service Bus proporciona mensajería segura y de confianza. La mensajería se puede usar para desacoplar las aplicaciones e integrarlas con otros sistemas basados en mensajes.
- Azure Event Grid. Event Grid es una plataforma sin servidor para publicar y entregar eventos de aplicación.
- Dirección IP. El servicio Azure API Management tiene una dirección IP pública fija y un nombre de dominio. El nombre de dominio es un subdominio de azure-api.net; por ejemplo, contoso.azure-api.net. Logic Apps y Service Bus también tienen una dirección IP pública; sin embargo, en esta arquitectura, se restringe el acceso para llamar a puntos de conexión de Logic Apps solo a la dirección IP de API Management (por motivos de seguridad). Las llamadas a Service Bus se protegen mediante una firma de acceso compartido.
- Azure DNS. DNS de Azure es un servicio de hospedaje para los dominios DNS, que permite resolver nombres mediante la infraestructura de Microsoft Azure. Al hospedar dominios en Azure, puede administrar los registros DNS con las mismas credenciales, API, herramientas y facturación que con los demás servicios de Azure. Para usar un nombre de dominio personalizado, como contoso.com, cree registros DNS que asignen el nombre de dominio personalizado a la dirección IP. Para más información, consulte Configuración de un nombre de dominio personalizado en Azure API Management.
- Azure Active Directory (Azure AD). Use Azure AD u otro proveedor de identidades para la autenticación. Azure AD proporciona autenticación para acceder a los puntos de conexión de API (pasando una instancia de JSON Web Token para API Management a efectos de validación) y puede proteger el acceso al portal para desarrolladores de API Management (solo los niveles Estándar y Premium).

Esta arquitectura tiene algunos patrones fundamentales para su funcionamiento:

1. Las API HTTP back-end se publican en el portal para desarrolladores de API Management, lo que permite a los desarrolladores, ya sean internos en la organización, externos o ambos, integrar las llamadas a dichas API en las aplicaciones.
2. Las API compuestas se compilan con Logic Apps, mediante la orquestación de llamadas a sistemas SAAS, servicios de Azure y cualquier API publicada en API Management. Las instancias de Logic Apps también se publican en el portal para desarrolladores de API Management.
3. Las aplicaciones adquieren un token de seguridad OAuth 2.0 que resulta necesario para obtener acceso a una API mediante Azure Active Directory.
4. Azure API Management valida el token de seguridad y pasa la solicitud a la aplicación lógica/API back-end.
5. Las colas de Service Bus se usan para desacoplar la actividad de la aplicación y suaviza los picos de carga. Los mensajes se agregan a las colas mediante Logic Apps, aplicaciones de terceros o (no en la imagen) a través de la publicación de la cola como una API de HTTP mediante API Management.
6. Un evento se activa cada vez que se agregan mensajes a una cola de Service Bus. Este evento desencadena una instancia de Logic Apps que procesa el mensaje.
7. De manera similar, otros servicios de Azure (como Blob Storage y Event Hub) también publican eventos en Event Grid. Estos desencadenan instancias de Logic Apps para recibir el evento y hacer otras acciones posteriores.

## <a name="recommendations"></a>Recomendaciones

Los requisitos pueden diferir de los de la arquitectura que se describe aquí. Use las recomendaciones de esta sección como punto de partida.

### <a name="service-bus-tier"></a>Nivel de Service Bus

Use el nivel premium de Service Bus, porque este admite actualmente las notificaciones de Event Grid (se espera compatibilidad en todos los niveles). Consulte [Precios de Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).

### <a name="event-grid-pricing"></a>Precios de Event Grid

Event Grid funciona con un modelo sin servidor: la facturación se calcula según la cantidad de operaciones (ejecución de evento). Consulte [Precios de Event Grid](https://azure.microsoft.com/pricing/details/event-grid/) para más información. Actualmente no existen consideraciones sobre los niveles en Event Grid.

### <a name="use-peeklock-when-consuming-service-bus-messages"></a>Uso de PeekLock al consumir mensajes de Service Bus

Cuando cree instancias de Logic Apps para consumir mensajes de Service Bus, use [PeekLock](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#queues) dentro de la aplicación lógica para acceder a un grupo de mensajes. Luego, la aplicación lógica puede llevar a cabo pasos para validar cada mensaje antes de completar o abandonar la operación. Este enfoque protege contra la pérdida accidental de datos.

### <a name="check-for-multiple-objects-when-an-event-grid-trigger-fires"></a>Comprobación de varios objetos cuando se activa un desencadenador de Event Grid

La activación de desencadenadores de Event Grid simplemente significa que "ocurrió al menos una de estas acciones". Por ejemplo, cuando Event Grid desencadena una aplicación lógica en un mensaje que aparece en una cola de Service Bus, la aplicación lógica siempre debería suponer que puede haber uno o varios mensajes disponibles para procesar.

### <a name="region"></a>Region

Aprovisione API Management, Logic Apps y Services Bus en la misma región para minimizar la latencia de red. Por lo general, elija la región más cercana a los usuarios.

El grupo de recursos también tiene una región, que especifica dónde se almacenan los metadatos de la implementación y desde dónde se ejecuta la plantilla de la implementación. Coloque el grupo de recursos y sus recursos en la misma región. Esto puede mejorar la disponibilidad durante la implementación.

## <a name="scalability-considerations"></a>Consideraciones sobre escalabilidad

El nivel premium de Azure Service Bus puede escalar de manera horizontal el número de unidades de mensajería para alcanzar una mayor escalabilidad. El nivel premium puede tener 1, 2 o 4 unidades de mensajería. Para más información sobre cómo escalar Azure Service Bus, consulte [Procedimientos recomendados para mejorar el rendimiento mediante la mensajería de Service Bus](../service-bus-messaging/service-bus-performance-improvements.md).

## <a name="availability-considerations"></a>Consideraciones sobre disponibilidad

En el momento de escribir este artículo, el Acuerdo de Nivel de Servicio (SLA) de Azure API Management es del 99,9 % para los niveles Básico, Estándar y Premium. Las configuraciones del nivel premium con la implementación de al menos una unidad en dos o más regiones tienen un Acuerdo de Nivel de Servicio de un 99,95 %.

En el momento de escribir este artículo, el Acuerdo de Nivel de Servicio para Azure Logic Apps es del 99,9 %.

### <a name="disaster-recovery"></a>Recuperación ante desastres

Considere la posibilidad de implementar la recuperación ante desastres geográfica en el nivel premium de Service Bus para habilitar la conmutación por error si se produce una interrupción grave. Lea más sobre la [recuperación ante desastres geográfica de Azure Service Bus](../service-bus-messaging/service-bus-geo-dr.md).

## <a name="manageability-considerations"></a>Consideraciones sobre la manejabilidad

Cree grupos de recursos independientes para entornos de producción, desarrollo y pruebas. Esto facilita la administración de implementaciones, la eliminación de implementaciones de prueba y la asignación de derechos de acceso.
Al asignar recursos a grupos de recursos, tenga en cuenta lo siguiente:

- Ciclo de vida. En general, coloque los recursos con el mismo ciclo de vida en el mismo grupo de recursos.
- Acceso. Puede usar el [control de acceso basado en rol](../role-based-access-control/overview.md) (RBAC) para aplicar directivas de acceso a los recursos de un grupo.
- Facturación. Puede ver los costes acumulados del grupo de recursos.
- Plan de tarifa de API Management: se recomienda usar el nivel Desarrollador para entornos de desarrollo y pruebas. Para entornos de preproducción, se recomienda implementar una réplica del entorno de producción, ejecutar pruebas y luego apagarla para minimizar costos.

Para más información, consulte [Información general del grupo de recursos](../azure-resource-manager/resource-group-overview.md).

### <a name="deployment"></a>Implementación

Se recomienda usar [plantillas de Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) para implementar Azure API Management, Logic Apps, Event Grid y Service Bus. Las plantillas facilitan la automatización de las implementaciones a través de PowerShell o de la interfaz de línea de comandos (CLI) de Azure.

Se recomienda colocar API Management, cualquier instancia de Logic Apps, temas de Event Grid y espacios de nombres de Service Bus en sus propias plantillas de Resource Manager. Esto permitirá almacenarlas en sistemas de control de código fuente. Estas plantillas pueden implementarse juntas o por separado como parte de un proceso de integración continua/implementación continua.

### <a name="diagnostics-and-monitoring"></a>Diagnóstico y supervisión

Service Bus, como API Management y Logic Apps, se puede supervisar con Azure Monitor. Azure Monitor está habilitado de forma predeterminada y proporcionará información basada en las diferentes métricas configuradas para cada servicio.

## <a name="security-considerations"></a>Consideraciones sobre la seguridad

Proteja Service Bus con una firma de acceso compartido. La [autenticación de SAS](../service-bus-messaging/service-bus-sas.md) le permite conceder acceso a un usuario a los recursos de Service Bus con derechos específicos. Lea más sobre [Autenticación y autorización de Service Bus](../service-bus-messaging/service-bus-authentication-and-authorization.md).

Además, si es necesario exponer una cola de Service Bus como un punto de conexión HTTP (para permitir la publicación de mensajes nuevos), se debe usar API Management para protegerla, poniéndolo por delante del punto de conexión. Esto se puede proteger con certificados o con OAuth, según corresponda. La manera más sencilla de hacerlo es usar una aplicación lógica con un desencadenador HTTP de solicitud o respuesta como intermediario.

Event Grid protege la entrega de eventos con un código de validación. Si usa LogicApps para consumir el evento, esto se realiza automáticamente. Consulte más detalles sobre [Seguridad y autenticación de Event Grid](../event-grid/security-authentication.md).

## <a name="next-steps"></a>Pasos siguientes

* [Integración empresarial sencilla](logic-apps-architectures-simple-enterprise-integration.md)
