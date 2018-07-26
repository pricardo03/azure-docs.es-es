---
title: Arquitectura de referencia de integración empresarial de los servicios de integración de Azure
description: Describe la arquitectura de referencia que muestra cómo implementar un patrón de integración empresarial mediante Logic Apps, API Management, Service Bus y Event Grid.
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
ms.openlocfilehash: a86c4c4227795a712dd51ace1fbefe9d2b96518a
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39116119"
---
# <a name="reference-architecture-enterprise-integration-with-queues-and-events"></a>Arquitectura de referencia: integración empresarial con colas y eventos

La arquitectura de referencia siguiente muestra un conjunto de prácticas demostradas que puede aplicar a una aplicación de integración que usa los servicios de integración de Azure. La arquitectura puede servir de base para muchos patrones de aplicaciones diferentes que requieren API HTTP, flujo de trabajo y orquestación.

![Diagrama de arquitectura: integración empresarial con colas y eventos](media/logic-apps-architectures-enterprise-integration-with-queues-events/integr_queues_events_arch_diagram.png)

*Hay muchas aplicaciones posibles para la tecnología de integración. Van desde una aplicación sencilla punto a punto a una aplicación de Azure Service Bus empresarial completa. La serie de arquitectura describe los componentes reutilizables que se pueden aplicar para compilar una aplicación de integración genérica. Los arquitectos deben considerar qué componentes necesitan implementar para la aplicación e infraestructura.*

## <a name="architecture"></a>Arquitectura

La arquitectura *se basa en* la arquitectura de [integración empresarial sencilla](logic-apps-architectures-simple-enterprise-integration.md). Aquí se aplican las [recomendaciones para la arquitectura empresarial sencilla](logic-apps-architectures-simple-enterprise-integration.md#recommendations). Para mantener la brevedad, se omiten en las [recomendaciones](#recommendations) de este artículo. 

La arquitectura consta de los siguientes componentes:

- **Grupo de recursos**. Un [grupo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) es un contenedor lógico de recursos de Azure.
- **Azure API Management**. [API Management](https://docs.microsoft.com/azure/api-management/) es una plataforma completamente administrada que se usa para publicar, proteger y transformar las API HTTP.
- **Portal para desarrolladores de Azure API Management**. Cada instancia de Azure API Management incluye acceso al [portal para desarrolladores](https://docs.microsoft.com/azure/api-management/api-management-customize-styles). El portal para desarrolladores de API Management proporciona acceso a documentación y ejemplos de código. Puede probar las API en el portal para desarrolladores.
- **Azure Logic Apps**. [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview) es una plataforma sin servidor que se usa para crear integración y flujo de trabajo de ámbito empresarial.
- **Conectores**. Logic Apps usa los [conectores](https://docs.microsoft.com/azure/connectors/apis-list) para conectarse a servicios de uso habitual. Logic Apps ya dispone de cientos de conectores diferentes, pero también puede crear un conector personalizado.
- **Azure Service Bus**. [Service Bus](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-messaging-overview) proporciona mensajería segura y de confianza. La mensajería se puede usar para desacoplar las aplicaciones e integrarlas con otros sistemas basados en mensajes.
- **Azure Event Grid**. [Event Grid](https://docs.microsoft.com/azure/event-grid/overview) es una plataforma sin servidor que se usa para publicar y entregar eventos de aplicación.
- **Dirección IP**. El servicio Azure API Management tiene una [dirección IP](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm) pública fija y un nombre de dominio. El nombre de dominio es un subdominio de azure-api.net; por ejemplo, contoso.azure-api.net. Logic Apps y Service Bus también tienen una dirección IP pública. Sin embargo, en esta arquitectura, restringimos el acceso para llamar a puntos de conexión de Logic Apps solo a la dirección IP de API Management (para la seguridad). Las llamadas a Service Bus se protegen mediante una firma de acceso compartido (SAS).
- **Azure DNS**. [Azure DNS](https://docs.microsoft.com/azure/dns/) es un servicio de hospedaje para dominios DNS. Azure DNS proporciona resolución de nombres mediante el uso de la infraestructura de Microsoft Azure. Al hospedar dominios en Azure, puede administrar los registros DNS con las mismas credenciales, API, herramientas y facturación que usa con los demás servicios de Azure. Para usar un nombre de dominio personalizado (como contoso.com), cree registros DNS que asignen el nombre de dominio personalizado a la dirección IP. Para más información, consulte el artículo sobre cómo [configurar un nombre de dominio personalizado en API Management](https://docs.microsoft.com/en-us/azure/api-management/configure-custom-domain).
- **Azure Active Directory (Azure AD)**. Use [Azure AD](https://docs.microsoft.com/azure/active-directory/) u otro proveedor de identidades para la autenticación. Azure AD proporciona autenticación para acceder a los puntos de conexión de la API al pasar una [instancia de JSON Web Token para API Management](https://docs.microsoft.com/azure/api-management/policies/authorize-request-based-on-jwt-claims) para validarla. Azure AD puede proteger el acceso al portal para desarrolladores de API Management (solo niveles Estándar y Premium).

La arquitectura tiene algunos patrones que son fundamentales para su funcionamiento:

* Las API HTTP de back-end existentes se publican mediante el portal para desarrolladores de API Management. En el portal, los desarrolladores (ya sean internos de la organización, externos o ambos) pueden integrar las llamadas a estas API en las aplicaciones.
* Las API compuestas se crean mediante el uso de las aplicaciones lógicas y la orquestación de llamadas a los sistemas de software como servicio (SaaS), los servicios de Azure y cualquier API que esté publicada en API Management. [Las aplicaciones lógicas también se publican](https://docs.microsoft.com/azure/api-management/import-logic-app-as-api) a través del portal para desarrolladores de API Management.
- Las aplicaciones usan Azure AD para [adquirir un token de seguridad de OAuth 2.0](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) necesario para obtener acceso a una API.
- API Management [valida el token de seguridad](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) y pasa la solicitud a la aplicación lógica o a la API de back-end.
- Las colas de Service Bus se usan para [desacoplar](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview) la actividad de la aplicación y para [suavizar los picos de carga](https://docs.microsoft.com/azure/architecture/patterns/queue-based-load-leveling). Los mensajes se agregan a las colas mediante aplicaciones lógicas, aplicaciones de terceros o (no en la imagen) a través de la publicación de la cola como una API HTTP mediante API Management.
- Un evento se activa cada vez que se agregan mensajes a una cola de Service Bus. El evento desencadena una aplicación lógica. Luego, la aplicación lógica procesa el mensaje.
- Otros servicios de Azure (por ejemplo, Azure Blob Storage y Azure Event Hubs) también publican eventos en Event Grid. Estos servicios desencadenan aplicaciones lógicas para recibir el evento y luego hacen acciones posteriores.

## <a name="recommendations"></a>Recomendaciones

Los requisitos específicos pueden diferir de la arquitectura genérica que se describe en este artículo. Use las recomendaciones de esta sección como punto de partida.

### <a name="service-bus-tier"></a>Nivel de Service Bus

Use el nivel Premium de Service Bus. El nivel Premier admite las notificaciones de Event Grid. Para más información, consulte [Precios de Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).

### <a name="event-grid-pricing"></a>Precios de Event Grid

Event Grid usa un modelo sin servidor. La facturación se calcula en función del número de operaciones (ejecución de evento). Para más información, consulte [Precios de Event Grid](https://azure.microsoft.com/pricing/details/event-grid/). Actualmente no existen consideraciones sobre los niveles en Event Grid.

### <a name="use-peeklock-to-consume-service-bus-messages"></a>Uso de PeekLock para consumir mensajes de Service Bus

Cuando cree una aplicación lógica para consumir mensajes de Service Bus, use [PeekLock](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#queues) en la aplicación lógica para acceder a un grupo de mensajes. Cuando usa PeekLock, la aplicación lógica puede realizar los pasos necesarios para validar cada mensaje antes de completar o abandonar el mensaje. Este enfoque protege contra la pérdida accidental de datos.

### <a name="check-for-multiple-objects-when-an-event-grid-trigger-fires"></a>Comprobación de varios objetos cuando se activa un desencadenador de Event Grid

Cuando se activa un desencadenador de Event Grid, simplemente significa que "al menos una de estas cosas sucedió". Por ejemplo, cuando Event Grid desencadena una aplicación lógica en un mensaje que aparece en una cola de Service Bus, la aplicación lógica siempre debería suponer que puede haber uno o varios mensajes disponibles para procesar.

### <a name="region"></a>Region

Aprovisione API Management, Logic Apps y Services Bus en la misma región para minimizar la latencia de red. Por lo general, elija la región más cercana a los usuarios.

El grupo de recursos también tiene una región. La región especifica dónde se almacenan los metadatos de implementación y desde dónde se ejecuta la plantilla de implementación. Coloque el grupo de recursos y sus recursos en la misma región para mejorar la disponibilidad durante la implementación.

## <a name="scalability"></a>Escalabilidad

El nivel Premium de Service Bus puede escalar de manera horizontal el número de unidades de mensajería para alcanzar una mayor escalabilidad. Las configuraciones de nivel Premium pueden tener una, dos o cuatro unidades de mensajería. Para más información sobre el escalado de Service Bus, consulte [Procedimientos recomendados para mejorar el rendimiento mediante la mensajería de Service Bus](../service-bus-messaging/service-bus-performance-improvements.md).

## <a name="availability"></a>Disponibilidad

Actualmente, el Acuerdo de Nivel de Servicio (SLA) de Azure API Management es del 99,9 % para los niveles Básico, Estándar y Premium. Las configuraciones del nivel premium con la implementación de al menos una unidad en dos o más regiones tienen un Acuerdo de Nivel de Servicio de un 99,95 %.

Actualmente, el SLA de Azure Logic Apps es del 99,9%.

### <a name="disaster-recovery"></a>Recuperación ante desastres

Considere la posibilidad de implementar la recuperación ante desastres geográfica en el nivel Premium de Service Bus para habilitar la conmutación por error si se produce una interrupción grave. Para más información, consulte [Recuperación ante desastres geográfica de Azure Service Bus](../service-bus-messaging/service-bus-geo-dr.md).

## <a name="manageability"></a>Manejabilidad

Cree grupos de recursos independientes para entornos de producción, desarrollo y pruebas. Los grupos de recursos independientes facilitan la administración de implementaciones, la eliminación de implementaciones de prueba y la asignación de derechos de acceso.

Cuando asigna recursos a los grupos de recursos, debe considerar estos factores:

- **Ciclo de vida**. En general, coloque los recursos que tienen el mismo ciclo de vida en el mismo grupo de recursos.
- **Acceso**. Puede usar el [control de acceso basado en rol](../role-based-access-control/overview.md) (RBAC) para aplicar directivas de acceso a los recursos de un grupo.
- **Facturación**. Puede ver los costos acumulados del grupo de recursos.
- **Plan de tarifa de API Management**. Se recomienda usar el nivel Desarrollador para los entornos de desarrollo y pruebas. Para entornos de preproducción, se recomienda implementar una réplica del entorno de producción, ejecutar pruebas y luego apagarla para minimizar costos.

Para más información, consulte [Información general de Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

### <a name="deployment"></a>Implementación

Se recomienda usar [plantillas de Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) para implementar API Management, Logic Apps, Event Grid y Service Bus. Las plantillas facilitan la automatización de las implementaciones a través de PowerShell o de la CLI de Azure.

Se recomienda colocar API Management, cualquier instancia de Logic Apps, temas de Event Grid y espacios de nombres de Service Bus en sus propias plantillas de Resource Manager. Cuando usa plantillas independientes, puede almacenar los recursos en sistemas de control de código fuente. Luego puede implementar estas plantillas juntas o por separado como parte de un proceso de integración continua/implementación continua (CI/CD).

### <a name="diagnostics-and-monitoring"></a>Diagnóstico y supervisión

Al igual que API Management y Logic Apps, puede supervisar Service Bus con Azure Monitor. Azure Monitor proporciona información según las métricas que se configuran para cada servicio. Azure Monitor está habilitado de manera predeterminada.

## <a name="security"></a>Seguridad

Proteja Service Bus con una SAS. Puede usar la [autenticación de SAS](../service-bus-messaging/service-bus-sas.md) para conceder acceso a un usuario a los recursos de Service Bus con derechos específicos. Para más información, consulte [Autenticación y autorización de Service Bus](../service-bus-messaging/service-bus-authentication-and-authorization.md).

Si una cola de Service Bus se debe exponer como un punto de conexión de HTTP (para publicar mensajes nuevos), debe usar API Management para protegerla por delante del punto de conexión. El punto de conexión se puede proteger con certificados o con OAuth, según corresponda. La manera más sencilla de proteger un punto de conexión es mediante una aplicación lógica con un desencadenador HTTP de solicitud o respuesta como intermediario.

Event Grid protege la entrega de eventos con un código de validación. Si usa Logic Apps para consumir el evento, la validación se realiza automáticamente. Para más información, vea [Event Grid security and authentication](../event-grid/security-authentication.md) (Seguridad y autenticación de Event Grid).

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre la [integración empresarial sencilla](logic-apps-architectures-simple-enterprise-integration.md).