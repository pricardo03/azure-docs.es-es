---
title: Arquitectura de referencia de integración empresarial sencilla de los servicios de integración de Azure
description: Describe la arquitectura de referencia que muestra cómo implementar un patrón de integración empresarial sencilla con Azure Logic Apps y Azure API Management.
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
ms.openlocfilehash: 982a5eabf8c6c3012a9b3e8fdbe2ff32ba439972
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39113599"
---
# <a name="reference-architecture-simple-enterprise-integration"></a>Arquitectura de referencia: integración empresarial sencilla

La arquitectura de referencia siguiente muestra un conjunto de prácticas demostradas que puede aplicar a una aplicación de integración que usa los servicios de integración de Azure. La arquitectura puede servir de base para muchos patrones de aplicaciones diferentes que requieren API HTTP, flujo de trabajo y orquestación.

![Diagrama de la arquitectura: integración empresarial sencilla](./media/logic-apps-architectures-simple-enterprise-integration/simple_arch_diagram.png)

*Hay muchas aplicaciones posibles para la tecnología de integración. Van desde una aplicación sencilla punto a punto a una aplicación de Azure Service Bus empresarial completa. La serie de arquitectura describe los componentes reutilizables que se pueden aplicar para compilar una aplicación de integración genérica. Los arquitectos deben considerar qué componentes necesitan implementar para la aplicación e infraestructura.*

## <a name="architecture"></a>Arquitectura

La arquitectura consta de los siguientes componentes:

- **Grupo de recursos**. Un [grupo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) es un contenedor lógico de recursos de Azure.
- **Azure API Management**. [API Management](https://docs.microsoft.com/azure/api-management/) es una plataforma completamente administrada que se usa para publicar, proteger y transformar las API HTTP.
- **Portal para desarrolladores de Azure API Management**. Cada instancia de Azure API Management incluye acceso al [portal para desarrolladores](https://docs.microsoft.com/azure/api-management/api-management-customize-styles). El portal para desarrolladores de API Management proporciona acceso a documentación y ejemplos de código. Puede probar las API en el portal para desarrolladores.
- **Azure Logic Apps**. [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview) es una plataforma sin servidor que se usa para crear integración y flujo de trabajo de ámbito empresarial.
- **Conectores**. Logic Apps usa los [conectores](https://docs.microsoft.com/azure/connectors/apis-list) para conectarse a servicios de uso habitual. Logic Apps ya dispone de cientos de conectores diferentes, pero también puede crear un conector personalizado.
- **Dirección IP**. El servicio Azure API Management tiene una [dirección IP](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm) pública fija y un nombre de dominio. El nombre de dominio es un subdominio de azure-api.net; por ejemplo, contoso.azure-api.net. Logic Apps y Service Bus también tienen una dirección IP pública. Sin embargo, en esta arquitectura, restringimos el acceso para llamar a puntos de conexión de Logic Apps solo a la dirección IP de API Management (para la seguridad). Las llamadas a Service Bus se protegen mediante una firma de acceso compartido (SAS).
- **Azure DNS**. [Azure DNS](https://docs.microsoft.com/azure/dns/) es un servicio de hospedaje para dominios DNS. Azure DNS proporciona resolución de nombres mediante el uso de la infraestructura de Microsoft Azure. Al hospedar dominios en Azure, puede administrar los registros DNS con las mismas credenciales, API, herramientas y facturación que usa con los demás servicios de Azure. Para usar un nombre de dominio personalizado, como contoso.com, cree registros DNS que asignen el nombre de dominio personalizado a la dirección IP. Para más información, consulte el artículo sobre cómo [configurar un nombre de dominio personalizado en API Management](https://docs.microsoft.com/en-us/azure/api-management/configure-custom-domain).
- **Azure Active Directory (Azure AD)**. Use [Azure AD](https://docs.microsoft.com/azure/active-directory/) u otro proveedor de identidades para la autenticación. Azure AD proporciona autenticación para acceder a los puntos de conexión de la API al pasar una [instancia de JSON Web Token para API Management](https://docs.microsoft.com/azure/api-management/policies/authorize-request-based-on-jwt-claims) para validarla. Azure AD puede proteger el acceso al portal para desarrolladores de API Management (solo niveles Estándar y Premium).

La arquitectura tiene algunos patrones que son fundamentales para su funcionamiento:

- Las API compuestas se crean mediante el uso de las aplicaciones lógicas. Coordinan las llamadas a sistemas de software como servicio (SaaS), a los servicios de Azure y a cualquier API que se publica en API Management. [Las aplicaciones lógicas también se publican](https://docs.microsoft.com/azure/api-management/import-logic-app-as-api) a través del portal para desarrolladores de API Management.
- Las aplicaciones usan Azure AD para [adquirir un token de seguridad de OAuth 2.0](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) necesario para obtener acceso a una API.
- Azure API Management [valida el token de seguridad](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) y pasa la solicitud a la aplicación lógica o a la API de back-end.

## <a name="recommendations"></a>Recomendaciones

Los requisitos específicos pueden diferir de la arquitectura genérica que se describe en este artículo. Use las recomendaciones de esta sección como punto de partida.

### <a name="azure-api-management-tier"></a>Niveles de Azure API Management

Use los niveles Básico, Estándar o Premium de API Management. Los niveles ofrecen un contrato de nivel de servicio (SLA) de producción y admite la escalabilidad horizontal dentro de la región de Azure (el número de unidades varía por nivel). El nivel Premium también admite la escalabilidad horizontal entre varias regiones de Azure. Base el nivel seleccionado en el nivel de rendimiento necesario y en el conjunto de características. Para más información, consulte los [precios de API Management](https://azure.microsoft.com/pricing/details/api-management/).

Se le cobra por todas las instancias de API Management cuando están en ejecución. Si ha realizado la escalabilidad vertical y no necesita dicho nivel de rendimiento en todo momento, benefíciese de la reducción vertical y la facturación por horas de API Management.

### <a name="logic-apps-pricing"></a>Precios de Logic Apps

Logic Apps usa un modelo [sin servidor](logic-apps-serverless-overview.md). La facturación se calcula en función de la acción y la ejecución del conector. Para obtener más información, consulte [Precios de Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/). Actualmente no existen consideraciones sobre los niveles en Logic Apps.

### <a name="logic-apps-for-asynchronous-api-calls"></a>Logic Apps para llamadas API asincrónicas

Logic Apps funciona mejor en escenarios que no requieren una baja latencia. Por ejemplo, funciona mejor para las llamada API asincrónicas o de ejecución semiprolongada. Si se requiere una latencia baja (por ejemplo, una llamada que bloquea una interfaz de usuario), se recomienda implementar esa API o esa operación mediante una tecnología distinta. Por ejemplo, use Azure Functions o una API Web que se implementa mediante el uso de Azure App Service. Se sigue recomendando que enfrente la API a los consumidores de la API con API Management.

### <a name="region"></a>Region

Aprovisione API Management y Logic Apps en la misma región para minimizar la latencia de red. Por lo general, elija la región más cercana a los usuarios.

El grupo de recursos también tiene una región. La región especifica dónde se almacenan los metadatos de implementación y desde dónde se ejecuta la plantilla de implementación. Coloque el grupo de recursos y sus recursos en la misma región para mejorar la disponibilidad durante la implementación.

## <a name="scalability"></a>Escalabilidad

Los administradores de API Management deben agregar [directivas de almacenamiento en caché](../api-management/api-management-howto-cache.md) cuando proceda, a fin de aumentar la escalabilidad del servicio. El almacenamiento en caché también ayuda a reducir la carga de sus servicios back-end.

Los niveles Básico, Estándar y Premium de Azure API Management se pueden escalar horizontalmente en una región de Azure para ofrecer mayor capacidad. Los administradores pueden usar la **métrica de capacidad** en el menú **Métricas** para analizar el uso de su servicio y luego escalar o reducir verticalmente, según corresponda.

Recomendaciones de escalado de un servicio API Management:

- El escalado debe tener en cuenta los patrones de tráfico. Los clientes con patrones de tráfico más volátiles tienen una mayor necesidad de contar con más capacidad.
- Una capacidad coherente superior al 66 % puede indicar la necesidad de escalar verticalmente.
- Una capacidad coherente inferior al 20 % puede indicar una oportunidad para reducir verticalmente.
- Siempre se recomienda realizar una prueba de carga del servicio API Management con una carga representativa antes de habilitar la carga en producción.

Los servicios del nivel Premium se pueden escalar horizontalmente en varias regiones de Azure. Los clientes que realizan la implementación mediante el escalado de servicios en las distintas regiones de Azure obtienen un SLA mayor (99,95 % versus 99,9 %) y pueden aprovisionar los servicios cerca de los usuarios en varias regiones.

El modelo sin servidor de Logic Apps significa que no es necesario que los administradores planifiquen la escalabilidad del servicio. El servicio se escala automáticamente para satisfacer la demanda.

## <a name="availability"></a>Disponibilidad

Actualmente, el SLA de Azure API Management es del 99,9% para los niveles Básico, Estándar y Premium. Las configuraciones del nivel premium con la implementación de al menos una unidad en dos o más regiones tienen un Acuerdo de Nivel de Servicio de un 99,95 %.

Actualmente, el SLA de Azure Logic Apps es del 99,9%.

### <a name="backups"></a>Copias de seguridad

La configuración de Azure API Management debe ser [crear copias de seguridad de manera periódica](../api-management/api-management-howto-disaster-recovery-backup-restore.md) (basada en la regularidad del cambio). Los archivos de copia de seguridad deben almacenarse en una ubicación o región de Azure distinta de donde reside el servicio. Los clientes pueden seleccionar posteriormente alguna de las dos opciones para su estrategia de recuperación ante desastres:

- En un evento de recuperación ante desastres, se aprovisiona una nueva instancia de API Management, la copia de seguridad se restaura en la instancia nueva y se redirigen los registros DNS.
- Los clientes mantienen una copia pasiva de su servicio en otra región de Azure (lo que incurre en costos adicionales). Las copias de seguridad se restauran de manera periódica en la copia. En un evento de recuperación ante desastres, solo es necesario redirigir los registros DNS para restaurar el servicio.

Como las aplicaciones lógicas se pueden volver a crear rápidamente y siguen un modelo sin servidor, la copia de seguridad se realiza guardando una copia de la plantilla asociada de Azure Resource Manager. Las plantillas se pueden guardar en el control de código fuente y se pueden integrar con el proceso de integración continua/implementación continua (CI/CD) de un cliente.

Si una aplicación lógica que se publicó a través de API Management se mueve a otro centro de datos, actualice la ubicación de la aplicación. Para actualizar la ubicación de la aplicación, use un script básico de PowerShell para actualizar la propiedad **Backend** de la API.

## <a name="manageability"></a>Manejabilidad

Cree grupos de recursos independientes para entornos de producción, desarrollo y pruebas. Usar grupos de recursos independientes facilita la administración de implementaciones, la eliminación de implementaciones de prueba y la asignación de derechos de acceso.

Cuando asigna recursos a los grupos de recursos, debe considerar estos factores:

- **Ciclo de vida**. En general, coloque los recursos que tienen el mismo ciclo de vida en el mismo grupo de recursos.
- **Acceso**. Puede usar el [control de acceso basado en rol](../role-based-access-control/overview.md) (RBAC) para aplicar directivas de acceso a los recursos de un grupo.
- **Facturación**. Puede ver los costos acumulados del grupo de recursos.
- **Plan de tarifa de API Management**. Se recomienda usar el nivel Desarrollador para los entornos de desarrollo y pruebas. Para entornos de preproducción, se recomienda implementar una réplica del entorno de producción, ejecutar pruebas y luego apagarla para minimizar costos.

Para más información, consulte [Información general de Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

### <a name="deployment"></a>Implementación

Se recomienda usar [plantillas de Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) para implementar API Management y Logic Apps. Las plantillas facilitan la automatización de las implementaciones a través de PowerShell o de la CLI de Azure.

Se recomienda colocar las instancias de Azure API Management y cualquier aplicación lógica individual en sus propias plantillas independientes de Resource Manager. Cuando usa plantillas independientes, puede almacenar los recursos en sistemas de control de código fuente. También puede implementar los recursos en conjunto o por separado como parte de un proceso de implementación de CI/CD.

### <a name="versions"></a>Versiones

Cada vez que realiza un cambio de configuración de una aplicación lógica o una implementación de una actualización con una plantilla de Resource Manager, se guarda una copia de dicha versión para su comodidad (se guardan todas las versiones que tengan un historial de ejecución). Puede usar estas versiones para realizar el seguimiento de cambios históricos y promover una versión para que sea la configuración actual de la aplicación lógica. Por ejemplo, puede revertir de manera efectiva una aplicación lógica.

API Management tiene dos [conceptos de control de versiones](https://blogs.msdn.microsoft.com/apimanagement/2018/01/11/versions-revisions-general-availibility/) distintos, pero no complementarios:

- Versiones que se usan para proporcionar a los clientes de API la opción de API que pueden consumir en función de sus necesidades (por ejemplo, v1, v2, beta o producción).
- Revisiones que permiten a los administradores de API realizar cambios seguros de una API y luego implementar esos cambios en usuarios con un comentario opcional.

En el contexto de la implementación, es conveniente considerar las revisiones de API Management como una manera de realizar cambios con seguridad, guardar un historial de los cambios e informar a los consumidores de API de dichos cambios. Una revisión puede crearse en un entorno de desarrollo e implementarse entre otros entornos con el uso de plantillas de Resource Manager.

Aunque puede usar las revisiones para probar una API antes de que se convierta en la "actual" y de que los usuarios puedan acceder a ella, no se recomienda usar este mecanismo para pruebas de integración o cargas. En su lugar, use entornos de prueba o preproducción independientes.

### <a name="configuration"></a>Configuración

Nunca inserte contraseñas, claves de acceso ni cadenas de conexión en el control de código fuente. Si estos valores son necesarios, use la técnica apropiada para implementar y proteger estos valores. 

En Logic Apps, cualquier valor confidencial necesario en la aplicación lógica (que no se pueda crear en forma de conexión) debe almacenarse en Azure Key Vault y hacer referencia a él desde una plantilla de Resource Manager. También se recomienda utilizar parámetros de plantilla de implementación y los archivos de parámetros para cada entorno. Para más información, consulte [Parámetros seguros y entradas dentro de un flujo de trabajo](logic-apps-securing-a-logic-app.md#secure-parameters-and-inputs-within-a-workflow).

En API Management, los secretos se administran con objetos denominados *valores con nombre* o *propiedades*. Los objetos almacenan de forma segura los valores a los que se puede acceder en directivas de API Management. Para más información, consulte cómo [administrar secretos en API Management](../api-management/api-management-howto-properties.md).

### <a name="diagnostics-and-monitoring"></a>Diagnóstico y supervisión

Tanto [API Management](../api-management/api-management-howto-use-azure-monitor.md) como [Logic Apps](logic-apps-monitor-your-logic-apps.md) admiten la supervisión operativa con [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md). Azure Monitor proporciona información según las métricas que se configuran para cada servicio. Azure Monitor está habilitado de manera predeterminada.

Estas opciones también están disponibles para cada servicio:

- Los registros de Logic Apps se pueden enviar a [Azure Log Analytics](logic-apps-monitor-your-logic-apps-oms.md) para realizar análisis más exhaustivos y agregarlos a los paneles.
- API Management permite configurar Azure Application Insights para la supervisión de DevOps.
- API Management admite la [plantilla de solución de Power BI para análisis de API personalizados](http://aka.ms/apimpbi). Los clientes pueden usar la plantilla de solución para crear su propia solución de análisis personalizada. Los informes están disponibles en Power BI para los usuarios empresariales.

## <a name="security"></a>Seguridad

En esta sección se enumeran las consideraciones de seguridad que son específicas de los servicios de Azure descritos en este artículo y que se implementan en la arquitectura según se describe. No es una lista completa de procedimientos de seguridad recomendados.

- Use el control de acceso basado en rol (RBAC) para garantizar niveles apropiados de acceso a los usuarios.
- Proteja los puntos de conexión de API públicos en API Management con OAuth/OpenID Connect. Para proteger los puntos de conexión de API públicos, configure un proveedor de identidades y agregue una directiva de validación de JSON Web Token (JWT).
- Conéctese a servicios back-end desde API Management mediante certificados mutuos.
- Proteja las aplicaciones lógicas basadas en desencadenadores HTTP mediante la creación de una lista blanca de direcciones IP que apunta a la dirección IP de API Management. Una dirección IP incluida en una lista blanca evita llamar a la aplicación lógica desde la red pública de Internet sin pasar primero por API Management.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre la [integración empresarial con colas y eventos](logic-apps-architectures-enterprise-integration-with-queues-events.md).
