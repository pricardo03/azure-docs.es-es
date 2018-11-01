---
title: Patrón de arquitectura de integración empresarial - Azure Integration Services
description: Esta referencia de arquitectura muestra cómo puede implementar un patrón de integración empresarial con Azure Logic Apps, Azure API Management, Azure Service Bus y Azure Event Grid
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: mattfarm
ms.author: mattfarm
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 06/15/2018
ms.openlocfilehash: 5b58f4b71d8d9f3ca91d8cefc6215073fd836765
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50093673"
---
# <a name="enterprise-integration-architecture-with-queues-and-events"></a>Arquitectura de integración empresarial con colas y eventos

En este artículo se describe una arquitectura de integración empresarial que usa procedimientos demostrados que se pueden aplicar a una aplicación de integración al usar Azure Integration Services. Puede usar esta arquitectura como base para muchos patrones de aplicaciones diferentes que requieren API, flujo de trabajo y orquestación de HTTP.

![Diagrama de arquitectura: integración empresarial con colas y eventos](media/logic-apps-architectures-enterprise-integration-with-queues-events/integr_queues_events_arch_diagram.png)

Esta serie describe los componentes reutilizables que se pueden aplicar para compilar una aplicación de integración genérica. Dado que la tecnología de integración tiene muchas aplicaciones posibles, que van desde sencillas aplicaciones de punto a punto hasta aplicaciones empresariales de Azure Service Bus, considere qué componentes debe implementar para sus aplicaciones y para la infraestructura.

## <a name="architecture-components"></a>Componentes de la arquitectura

Esta arquitectura se basa en la arquitectura descrita en el artículo [Arquitectura de referencia: integración empresarial sencilla](/azure/architecture/reference-architectures/enterprise-integration/simple-enterprise-integration). Las [recomendaciones](/azure/architecture/reference-architectures/enterprise-integration/simple-enterprise-integration#recommendations) de ese artículo también se aplican aquí, pero por motivos de brevedad, este artículo las omite de la sección [Recomendaciones](#recommendations). Esta arquitectura de integración empresarial incluye estos componentes:

- **Grupo de recursos**: un [grupo de recursos](../azure-resource-manager/resource-group-overview.md) es un contenedor lógico de recursos de Azure.

- **Azure API Management**: el servicio [API Management](https://docs.microsoft.com/azure/api-management/) es una plataforma totalmente administrada para publicar, proteger y transformar API HTTP.

- **Portal para desarrolladores de Azure API Management**: cada instancia de Azure API Management proporciona acceso al [portal para desarrolladores](../api-management/api-management-customize-styles.md). Este portal le proporciona acceso a documentación y ejemplos de código. También puede probar las API en el portal para desarrolladores.

- **Azure Logic Apps**: [Logic Apps](../logic-apps/logic-apps-overview.md) es una plataforma sin servidor para la creación de integraciones y flujos de trabajo de ámbito empresarial.

- **Conectores**: Logic Apps usa los [conectores](../connectors/apis-list.md) para conectarse a servicios de uso habitual. Logic Apps ofrece cientos de conectores, pero también puede crear un conector personalizado.

- **Azure Service Bus**: [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md) proporciona mensajería segura y de confianza. La mensajería se puede usar para desacoplar las aplicaciones e integrarlas con otros sistemas basados en mensajes.

- **Azure Event Grid**: [Event Grid](../event-grid/overview.md) es una plataforma sin servidor para publicar y entregar eventos de aplicación.

- **Dirección IP**: el servicio Azure API Management tiene una [dirección IP pública](../virtual-network/virtual-network-ip-addresses-overview-arm.md) fija y un nombre de dominio. El nombre de dominio predeterminado es un subdominio de azure-api.net, por ejemplo, contoso.azure-api.net, pero también se pueden configurar [dominios personalizados](../api-management/configure-custom-domain.md). Logic Apps y Service Bus también tienen una dirección IP pública. Sin embargo, por motivos de seguridad, esta arquitectura restringe el acceso para llamar a puntos de conexión de Logic Apps solo a la dirección IP de API Management. Las llamadas a Service Bus se protegen mediante una firma de acceso compartido (SAS).

- **Azure DNS**: [Azure DNS](https://docs.microsoft.com/azure/dns/) es un servicio de hospedaje para dominios DNS. Azure DNS proporciona resolución de nombres mediante el uso de la infraestructura de Microsoft Azure. Al hospedar dominios en Azure, puede administrar los registros DNS con las mismas credenciales, API, herramientas y facturación que usa con los demás servicios de Azure. Para usar un nombre de dominio personalizado, como contoso.com, cree registros DNS que asignen el nombre de dominio personalizado a la dirección IP. Para más información, consulte el artículo sobre cómo [configurar un nombre de dominio personalizado en API Management](../api-management/configure-custom-domain.md).

- **Azure Active Directory (Azure AD)**: puede usar [Azure AD](https://docs.microsoft.com/azure/active-directory/) u otro proveedor de identidades para la autenticación. Azure AD proporciona autenticación para acceder a los puntos de conexión de la API al pasar una [instancia de JSON Web Token para API Management](../api-management/policies/authorize-request-based-on-jwt-claims.md) para validarla. En los niveles Estándar y Premium, Azure AD puede proteger el acceso al portal para desarrolladores de API Management.

## <a name="patterns"></a>Patrones 

Esta arquitectura usa algunos patrones que son fundamentales para su funcionamiento:

* Las API HTTP de back-end existentes se publican mediante el portal para desarrolladores de API Management. En el portal, los desarrolladores, que pueden ser internos de la organización, externos o de ambos tipos,  
pueden integrar llamadas a estas API en aplicaciones.

* Las API compuestas se compilan mediante el uso de aplicaciones lógicas que orquestan llamadas a los sistemas de software como servicio (SaaS), los servicios de Azure y cualquier API que esté publicada en API Management. Las aplicaciones lógicas también se [publican a través del portal para desarrolladores de API Management](../api-management/import-logic-app-as-api.md).

* Las aplicaciones usan Azure AD para [adquirir un token de seguridad de OAuth 2.0](../api-management/api-management-howto-protect-backend-with-aad.md) necesario para obtener acceso a una API.

* Azure API Management [valida el token de seguridad](../api-management/api-management-howto-protect-backend-with-aad.md) y pasa la solicitud a la aplicación lógica o a la API de back-end.

* Las colas de Azure Service Bus se usan para [desacoplar](../service-bus-messaging/service-bus-messaging-overview.md) la actividad de la aplicación y para [suavizar los picos de carga](https://docs.microsoft.com/azure/architecture/patterns/queue-based-load-leveling). Los mensajes se agregan a las colas mediante aplicaciones lógicas, aplicaciones de terceros o (no se muestran) a través de la publicación de la cola como una API HTTP mediante API Management.

* Un evento se activa cada vez que se agregan mensajes a una cola de Service Bus. El evento desencadena una aplicación lógica, que, a continuación, procesa el mensaje.

* Otros servicios de Azure como, por ejemplo, Azure Blob Storage y Azure Event Hubs, también publican eventos en Event Grid. Estos servicios desencadenan aplicaciones lógicas para recibir el evento y luego hacen acciones posteriores.

## <a name="recommendations"></a>Recomendaciones

Los requisitos específicos pueden diferir de la arquitectura genérica que se describe en este artículo. Use las recomendaciones de esta sección como punto de partida.

### <a name="service-bus-tier"></a>Nivel de Service Bus

Use el nivel Premium de Service Bus que admite las notificaciones de Event Grid. Para más información, consulte [Precios de Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).

### <a name="event-grid-pricing"></a>Precios de Event Grid

Event Grid usa un modelo sin servidor. La facturación se calcula en función del número de operaciones (ejecuciones de eventos). Para más información, consulte [Precios de Event Grid](https://azure.microsoft.com/pricing/details/event-grid/). Actualmente no existen consideraciones sobre los niveles en Event Grid.

### <a name="use-peeklock-to-consume-service-bus-messages"></a>Uso de PeekLock para consumir mensajes de Service Bus

Cuando cree una aplicación lógica para consumir mensajes de Service Bus, haga que esta aplicación use [PeekLock](../service-bus-messaging/service-bus-messaging-overview.md#queues) para acceder a un grupo de mensajes. Cuando usa PeekLock, la aplicación lógica puede realizar los pasos necesarios para validar cada mensaje antes de completar o abandonar el mensaje. Este enfoque protege contra la pérdida accidental de datos.

### <a name="check-for-multiple-objects-when-an-event-grid-trigger-fires"></a>Comprobación de varios objetos cuando se activa un desencadenador de Event Grid

Cuando se activa un desencadenador de Event Grid, esta acción simplemente significa que "al menos una de estas cosas sucedió". Por ejemplo, cuando Event Grid desencadena una aplicación lógica en un mensaje que aparece en una cola de Service Bus, la aplicación lógica siempre debería suponer que puede haber uno o varios mensajes disponibles para procesar.

### <a name="region"></a>Region

Para minimizar la latencia de red, elija la misma región para API Management, Logic Apps y Service Bus. Por lo general, elija la región más cercana a los usuarios.

El grupo de recursos también tiene una región. La región especifica dónde se almacenan los metadatos de implementación y dónde ejecutar la plantilla de implementación. Coloque el grupo de recursos y sus recursos en la misma región para mejorar la disponibilidad durante la implementación.

## <a name="scalability"></a>Escalabilidad

El nivel Premium de Service Bus puede escalar horizontalmente el número de unidades de mensajería para alcanzar una mayor escalabilidad. Las configuraciones de nivel Premium pueden tener una, dos o cuatro unidades de mensajería. Para más información sobre el escalado de Service Bus, consulte [Procedimientos recomendados para mejorar el rendimiento mediante la mensajería de Service Bus](../service-bus-messaging/service-bus-performance-improvements.md).

## <a name="availability"></a>Disponibilidad

* Actualmente, el Acuerdo de Nivel de Servicio (SLA) de Azure API Management es del 99,9 % para los niveles Básico, Estándar y Premium. Las configuraciones del nivel Premium con una implementación de al menos una unidad en dos o más regiones tienen un Acuerdo de Nivel de Servicio de un 99,95 %.

* Actualmente, el Acuerdo de Nivel de Servicio de Azure Logic Apps es del 99,9%.

### <a name="disaster-recovery"></a>Recuperación ante desastres

Considere la posibilidad de implementar la recuperación ante desastres geográfica en el nivel Premium de Service Bus para habilitar la conmutación por error si se produce una interrupción grave. Para más información, consulte [Recuperación ante desastres geográfica de Azure Service Bus](../service-bus-messaging/service-bus-geo-dr.md).

## <a name="manageability"></a>Manejabilidad

Cree grupos de recursos independientes para entornos de producción, desarrollo y pruebas. Los grupos de recursos independientes facilitan la administración de implementaciones, la eliminación de implementaciones de prueba y la asignación de derechos de acceso.

Cuando asigna recursos a los grupos de recursos, debe considerar estos factores:

* **Ciclo de vida**: en general, coloque los recursos que tienen el mismo ciclo de vida en el mismo grupo de recursos.

* **Acceso**: puede usar el [control de acceso basado en rol (RBAC)](../role-based-access-control/overview.md) para aplicar directivas de acceso a los recursos de un grupo.

* **Facturación**: puede ver los costos acumulados del grupo de recursos.

* **Plan de tarifa de API Management**: use el nivel Desarrollador para entornos de desarrollo y pruebas. Para minimizar los costos durante la preproducción, implemente una réplica del entorno de producción, ejecute las pruebas y, a continuación, apáguela.

Para más información, consulte [Información general de Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

## <a name="deployment"></a>Implementación

* Use las [plantillas de Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) para implementar API Management, Logic Apps, Event Grid y Service Bus. Las plantillas facilitan la automatización de las implementaciones mediante PowerShell o la CLI de Azure.

* Coloque API Management, cualquier instancia de Logic Apps, temas de Event Grid y espacios de nombres de Service Bus en sus propias plantillas de Resource Manager. Mediante el uso de plantillas independientes, puede almacenar los recursos en sistemas de control de código fuente. Luego puede implementar estas plantillas juntas o por separado como parte de un proceso de integración continua/implementación continua (CI/CD).

## <a name="diagnostics-and-monitoring"></a>Diagnóstico y supervisión

Al igual que API Management y Logic Apps, puede supervisar Service Bus con Azure Monitor, que está habilitado de forma predeterminada. Azure Monitor proporciona información según las métricas que se configuran para cada servicio. 

## <a name="security"></a>Seguridad

Proteja Service Bus con una firma de acceso compartido (SAS). Por ejemplo, puede usar la [autenticación de SAS](../service-bus-messaging/service-bus-sas.md) para conceder acceso a un usuario a los recursos de Service Bus con derechos específicos. Para más información, consulte [Autenticación y autorización de Service Bus](../service-bus-messaging/service-bus-authentication-and-authorization.md).

Si necesita exponer una cola de Service Bus como un punto de conexión de HTTP para, por ejemplo, publicar nuevos mensajes, use API Management para proteger la cola por delante del punto de conexión. El punto de conexión se puede proteger con certificados o con autenticación de OAuth, según corresponda. La manera más sencilla de proteger un punto de conexión es mediante una aplicación lógica con un desencadenador HTTP de solicitud o respuesta como intermediario.

El servicio Event Grid protege la entrega de eventos con un código de validación. Si usa Logic Apps para consumir el evento, la validación se realiza automáticamente. Para más información, vea [Event Grid security and authentication](../event-grid/security-authentication.md) (Seguridad y autenticación de Event Grid).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre la [integración empresarial sencilla](/azure/architecture/reference-architectures/enterprise-integration/simple-enterprise-integration).
