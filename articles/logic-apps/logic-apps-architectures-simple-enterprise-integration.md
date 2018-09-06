---
title: Patrón de arquitectura de integración empresarial sencilla - Azure Integration Services
description: Esta arquitectura de referencia muestra cómo implementar un patrón de integración empresarial sencilla con Azure Logic Apps y Azure API Management.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: mattfarm
ms.author: mattfarm
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 06/15/2018
ms.openlocfilehash: 7081c9e4f6e6deee196255f04180a8f2cc792876
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2018
ms.locfileid: "43122502"
---
# <a name="simple-enterprise-integration-architecture"></a>Arquitectura de integración empresarial sencilla

En este artículo se describe una arquitectura de integración empresarial que usa procedimientos demostrados que se pueden aplicar a una aplicación de integración al usar Azure Integration Services. Puede usar esta arquitectura como base para muchos patrones de aplicaciones diferentes que requieren API, flujo de trabajo y orquestación de HTTP.

![Diagrama de la arquitectura: integración empresarial sencilla](./media/logic-apps-architectures-simple-enterprise-integration/simple_arch_diagram.png)

Esta serie describe los componentes reutilizables que se pueden aplicar para compilar una aplicación de integración genérica. Dado que la tecnología de integración tiene muchas aplicaciones posibles, que van desde sencillas aplicaciones de punto a punto hasta aplicaciones empresariales de Azure Service Bus, considere qué componentes debe implementar para sus aplicaciones y para la infraestructura.

## <a name="architecture-components"></a>Componentes de la arquitectura

Esta arquitectura de integración empresarial incluye estos componentes:

- **Grupo de recursos**: un [grupo de recursos](../azure-resource-manager/resource-group-overview.md) es un contenedor lógico de recursos de Azure.

- **Azure API Management**: el servicio [API Management](https://docs.microsoft.com/azure/api-management/) es una plataforma totalmente administrada para publicar, proteger y transformar API HTTP.

- **Portal para desarrolladores de Azure API Management**: cada instancia de Azure API Management proporciona acceso al [portal para desarrolladores](../api-management/api-management-customize-styles.md). Este portal le proporciona acceso a documentación y ejemplos de código. También puede probar las API en el portal para desarrolladores.

- **Azure Logic Apps**: [Logic Apps](../logic-apps/logic-apps-overview.md) es una plataforma sin servidor para la creación de integraciones y flujos de trabajo de ámbito empresarial.

- **Conectores**: Logic Apps usa los [conectores](../connectors/apis-list.md) para conectarse a servicios de uso habitual. Logic Apps ofrece cientos de conectores, pero también puede crear un conector personalizado.

- **Dirección IP**: el servicio Azure API Management tiene una [dirección IP pública](../virtual-network/virtual-network-ip-addresses-overview-arm.md) fija y un nombre de dominio. El nombre de dominio predeterminado es un subdominio de azure-api.net, por ejemplo, contoso.azure-api.net, pero también se pueden configurar [dominios personalizados](../api-management/configure-custom-domain.md). Logic Apps y Service Bus también tienen una dirección IP pública. Sin embargo, por motivos de seguridad, esta arquitectura restringe el acceso para llamar a puntos de conexión de Logic Apps solo a la dirección IP de API Management. Las llamadas a Service Bus se protegen mediante una firma de acceso compartido (SAS).

- **Azure DNS**: [Azure DNS](https://docs.microsoft.com/azure/dns/) es un servicio de hospedaje para dominios DNS. Azure DNS proporciona resolución de nombres mediante el uso de la infraestructura de Microsoft Azure. Al hospedar dominios en Azure, puede administrar los registros DNS con las mismas credenciales, API, herramientas y facturación que usa con los demás servicios de Azure. Para usar un nombre de dominio personalizado, como contoso.com, cree registros DNS que asignen el nombre de dominio personalizado a la dirección IP. Para más información, consulte el artículo sobre cómo [configurar un nombre de dominio personalizado en API Management](../api-management/configure-custom-domain.md).

- **Azure Active Directory (Azure AD)**: puede usar [Azure AD](https://docs.microsoft.com/azure/active-directory/) u otro proveedor de identidades para la autenticación. Azure AD proporciona autenticación para acceder a los puntos de conexión de la API al pasar una [instancia de JSON Web Token para API Management](../api-management/policies/authorize-request-based-on-jwt-claims.md) para validarla. En los niveles Estándar y Premium, Azure AD puede proteger el acceso al portal para desarrolladores de API Management.

## <a name="patterns"></a>Patrones 

Esta arquitectura usa algunos patrones que son fundamentales para su funcionamiento:

* Las API compuestas se compilan mediante el uso de aplicaciones lógicas que orquestan llamadas a los sistemas de software como servicio (SaaS), los servicios de Azure y cualquier API que esté publicada en API Management. Las aplicaciones lógicas también se [publican a través del portal para desarrolladores de API Management](../api-management/import-logic-app-as-api.md).

* Las aplicaciones usan Azure AD para [adquirir un token de seguridad de OAuth 2.0](../api-management/api-management-howto-protect-backend-with-aad.md) necesario para obtener acceso a una API.

* Azure API Management [valida el token de seguridad](../api-management/api-management-howto-protect-backend-with-aad.md) y pasa la solicitud a la aplicación lógica o a la API de back-end.

## <a name="recommendations"></a>Recomendaciones

Los requisitos específicos pueden diferir de la arquitectura genérica que se describe en este artículo. Use las recomendaciones de esta sección como punto de partida.

### <a name="azure-api-management-tier"></a>Niveles de Azure API Management

Use los niveles Básico, Estándar o Premium de API Management. Los niveles ofrecen un contrato de nivel de servicio (SLA) de producción y admiten la escalabilidad horizontal dentro de la región de Azure. El número de unidades varía según el nivel. El nivel Premium también admite la escalabilidad horizontal entre varias regiones de Azure. Elija su nivel en función de su conjunto de características y el nivel de rendimiento necesario. Para más información, consulte los [precios de API Management](https://azure.microsoft.com/pricing/details/api-management/).

Se le cobra por todas las instancias de API Management cuando están en ejecución. Si ha escalado verticalmente y no necesita dicho nivel de rendimiento en todo momento, benefíciese de la reducción vertical y la facturación por horas de API Management.

### <a name="logic-apps-pricing"></a>Precios de Logic Apps

Logic Apps usa un modelo [sin servidor](../logic-apps/logic-apps-serverless-overview.md). La facturación se calcula en función de la acción y la ejecución del conector. Para obtener más información, consulte [Precios de Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/). Actualmente no existen consideraciones sobre los niveles en Logic Apps.

### <a name="logic-apps-for-asynchronous-api-calls"></a>Logic Apps para llamadas API asincrónicas

Logic Apps funciona mejor en escenarios que no requieren una baja latencia. Por ejemplo, Logic Apps funciona mejor para las llamada API asincrónicas o de ejecución semiprolongada. Si se requiere una latencia baja (por ejemplo, una llamada que bloquea una interfaz de usuario), implemente esa API o esa operación mediante una tecnología distinta. Por ejemplo, use Azure Functions o una API Web que se implementa mediante el uso de Azure App Service. Use API Management para enfrentar la API a los consumidores de API.

### <a name="region"></a>Region

Para minimizar la latencia de red, elija la misma región para API Management, Logic Apps y Service Bus. Por lo general, elija la región más cercana a los usuarios.

El grupo de recursos también tiene una región. La región especifica dónde se almacenan los metadatos de implementación y dónde ejecutar la plantilla de implementación. Coloque el grupo de recursos y sus recursos en la misma región para mejorar la disponibilidad durante la implementación.

## <a name="scalability"></a>Escalabilidad

Para aumentar la escalabilidad del servicio al administrar un servicio de API Management, agregue [directivas de almacenamiento en caché](../api-management/api-management-howto-cache.md) cuando proceda. El almacenamiento en caché también ayuda a reducir la carga de sus servicios back-end.

Para ofrecer mayor capacidad, puede escalar horizontalmente los niveles Básico, Estándar y Premium de Azure API Management en una región de Azure. Para analizar el uso de su servicio, en el menú **Métricas**, seleccione la opción **Métrica de capacidad** y luego escale verticalmente u horizontalmente según corresponda.

Recomendaciones de escalado de un servicio API Management:

- Considere los patrones de tráfico al escalar. Los clientes con patrones de tráfico más volátiles necesitan más capacidad.

- Una capacidad coherente superior al 66 % puede indicar la necesidad de escalar verticalmente.

- Una capacidad coherente inferior al 20 % puede indicar una oportunidad para reducir verticalmente.

- Antes de habilitar la carga en producción, realice siempre una prueba de carga del servicio API Management con una carga representativa.

Puede escalar horizontalmente los servicios del nivel Premium en varias regiones de Azure. Si realiza la implementación mediante el escalado de servicios en las distintas regiones de Azure, puede obtener un Acuerdo de Nivel de Servicio mayor (99,95 % frente a 99,9 %) y aprovisionar los servicios cerca de los usuarios en varias regiones.

El modelo sin servidor de Logic Apps significa que no es necesario que los administradores planifiquen la escalabilidad del servicio. El servicio se escala automáticamente para satisfacer la demanda.

## <a name="availability"></a>Disponibilidad

* Actualmente, el Acuerdo de Nivel de Servicio (SLA) de Azure API Management es del 99,9 % para los niveles Básico, Estándar y Premium. Las configuraciones del nivel Premium con una implementación de al menos una unidad en dos o más regiones tienen un Acuerdo de Nivel de Servicio de un 99,95 %.

* Actualmente, el Acuerdo de Nivel de Servicio de Azure Logic Apps es del 99,9%.

### <a name="backups"></a>Copias de seguridad

En función de la periodicidad de los cambios, [realice copias de seguridad regularmente](../api-management/api-management-howto-disaster-recovery-backup-restore.md) de la configuración de API Management de Azure. Almacene los archivos de copia de seguridad en una ubicación o región de Azure diferente de donde reside el servicio. A continuación, puede elegir cualquier opción como estrategia de recuperación ante desastres:

* En un evento de recuperación ante desastres, aprovisione una nueva instancia de API Management, restaure la copia de seguridad en la instancia nueva y redirija los registros DNS.

* Los clientes mantienen una copia pasiva de su servicio en otra región de Azure (lo que genera costos adicionales). Restaure con regularidad las copias de seguridad a esa copia. Para restaurar el servicio durante un evento de recuperación ante desastres, solo es necesario redirigir los registros DNS.

Como las aplicaciones lógicas se pueden volver a crear rápidamente y siguen un modelo sin servidor, haga una copia de seguridad guardando una copia de la plantilla asociada de Azure Resource Manager. Puede guardar las plantillas en el control de código fuente, y puede integrar plantillas con el proceso de implementación continua e integración continua (CI/CD).

Si publicó una aplicación lógica a través de API Management, y esa aplicación lógica se mueve a otro centro de datos, actualice la ubicación de la aplicación. Puede actualizar la propiedad **back-end** de la API mediante un script de PowerShell básico.

## <a name="manageability"></a>Manejabilidad

Cree grupos de recursos independientes para entornos de producción, desarrollo y pruebas. Los grupos de recursos independientes facilitan la administración de implementaciones, la eliminación de implementaciones de prueba y la asignación de derechos de acceso.

Cuando asigna recursos a los grupos de recursos, debe considerar estos factores:

* **Ciclo de vida**: en general, coloque los recursos que tienen el mismo ciclo de vida en el mismo grupo de recursos.

* **Acceso**: puede usar el [control de acceso basado en rol (RBAC)](../role-based-access-control/overview.md) para aplicar directivas de acceso a los recursos de un grupo.

* **Facturación**: puede ver los costos acumulados del grupo de recursos.

* **Plan de tarifa de API Management**: use el nivel Desarrollador para entornos de desarrollo y pruebas. Para minimizar los costos durante la preproducción, implemente una réplica del entorno de producción, ejecute las pruebas y, a continuación, apáguela.

Para más información, consulte [Información general de Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

## <a name="deployment"></a>Implementación

* Para implementar API Management y Logic Apps, use [plantillas de Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md). Las plantillas facilitan la automatización de las implementaciones mediante PowerShell o la CLI de Azure.

* Coloque las instancias de Azure API Management y cualquier aplicación lógica individual en sus propias plantillas independientes de Resource Manager. Mediante el uso de plantillas independientes, puede almacenar los recursos en sistemas de control de código fuente. Luego puede implementar estas plantillas juntas o por separado como parte de un proceso de integración continua/implementación continua (CI/CD).

### <a name="versions"></a>Versiones

Cada vez que realice un cambio de configuración de una aplicación lógica o implemente una actualización con una plantilla de Resource Manager, Azure guarda una copia de dicha versión para su comodidad (se guardan todas las versiones que tengan un historial de ejecución). Puede usar estas versiones para hacer el seguimiento de los cambios históricos o promover una versión como configuración actual de la aplicación lógica. Por ejemplo, puede revertir de manera efectiva una aplicación lógica.

Azure API Management tiene estos [conceptos de control de versiones](https://blogs.msdn.microsoft.com/apimanagement/2018/01/11/versions-revisions-general-availibility/) distintos, pero complementarios:

* Versiones que proporcionan a los consumidores de la API la capacidad de elegir una versión de API en función de sus necesidades, por ejemplo, v1, v2, beta o producción

* Revisiones que permiten a los administradores de API realizar cambios seguros de una API y luego implementar esos cambios en usuarios con un comentario opcional

Para la implementación, considere las revisiones de API Management como medio de hacer cambios seguros, mantener un historial de cambios y comunicar esos cambios a los consumidores de la API. Puede crear una revisión en el entorno de desarrollo e implementar ese cambio entre otros entornos con el uso de plantillas de Resource Manager.

Aunque puede usar las revisiones para probar una API antes de que se convierta en la "actual" y de que los usuarios puedan acceder a ella, este método no se recomienda para pruebas de integración o cargas. En su lugar, use entornos de prueba o preproducción independientes.

### <a name="configuration-and-sensitive-information"></a>Configuración e información confidencial

Nunca compruebe contraseñas, claves de acceso ni cadenas de conexión en el control de código fuente. Si estos valores son necesarios, protéjalos e impleméntelos mediante las técnicas oportunas. 

En Logic Apps, si una aplicación lógica requiere valores confidenciales que no se puede crear dentro de una conexión, almacene esos valores en Azure Key Vault y haga referencia a ellos desde una plantilla de Resource Manager. Utilice parámetros de plantilla de implementación y archivos de parámetros para cada entorno. Para más información, consulte [Parámetros seguros y entradas dentro de un flujo de trabajo](../logic-apps/logic-apps-securing-a-logic-app.md#secure-parameters-and-inputs-within-a-workflow).

API Management administra los secretos con objetos denominados *valores con nombre* o *propiedades*. Estos objetos almacenan de forma segura los valores a los que se puede acceder a través de directivas de API Management. Para más información, consulte cómo [administrar secretos en API Management](../api-management/api-management-howto-properties.md).

## <a name="diagnostics-and-monitoring"></a>Diagnóstico y supervisión

Puede usar [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md) para la supervisión operativa tanto en [API Management](../api-management/api-management-howto-use-azure-monitor.md) como en [Logic Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md). Azure Monitor proporciona información según las métricas que se configuran para cada servicio y está habilitado de manera predeterminada.

Cada servicio también tiene estas opciones:

* Para realizar análisis más exhaustivos y agregarlos a los paneles, puede agregar registros de Logic Apps a [Azure Log Analytics](logic-apps-monitor-your-logic-apps-oms.md).

* Para la supervisión de DevOps, puede configurar Azure Application Insights para API Management.

* API Management admite la [plantilla de solución de Power BI para análisis de API personalizados](http://aka.ms/apimpbi). Puede usar esta plantilla de solución para crear su propia solución de análisis. Los informes están disponibles en Power BI para los usuarios empresariales.

## <a name="security"></a>Seguridad

Aunque esta lista no describe completamente todos los procedimientos recomendados de seguridad, estas son algunas consideraciones de seguridad que se aplican específicamente a los servicios de Azure implementados en la arquitectura que se describe en este artículo:

* Para asegurarse de que los usuarios tienen niveles de acceso adecuados, utilice el control de acceso basado en rol (RBAC).

* Proteja los puntos de conexión de API públicos en API Management con OAuth o bien OpenID Connect. Para proteger los puntos de conexión de API públicos, configure un proveedor de identidades y agregue una directiva de validación de JSON Web Token (JWT).

* Conéctese a servicios back-end desde API Management mediante certificados mutuos.

* Proteja las aplicaciones lógicas basadas en desencadenadores HTTP mediante la creación de una lista blanca de direcciones IP que apunta a la dirección IP de API Management. Una dirección IP incluida en una lista blanca evita llamar a la aplicación lógica desde la red pública de Internet sin pasar primero por API Management.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre la [integración empresarial con colas y eventos](../logic-apps/logic-apps-architectures-enterprise-integration-with-queues-events.md).
