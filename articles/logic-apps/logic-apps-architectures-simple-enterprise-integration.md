---
title: 'Servicios de integración de Azure: integración empresarial sencilla'
description: Arquitectura de referencia que muestra cómo implementar un patrón de integración empresarial sencilla con Azure Logic Apps y Azure API Management
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
ms.openlocfilehash: 17f591a470bf65d3c9365bca9c5ae2d5a6c9574f
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232116"
---
# <a name="simple-enterprise-integration---reference-architecture"></a>Integración empresarial sencilla: arquitectura de referencia

## <a name="overview"></a>Información general

Esta arquitectura de referencia muestra un conjunto de prácticas demostradas para una aplicación web que usa los servicios de integración de Azure. Esta arquitectura puede servir de base de muchos patrones de aplicaciones distintos que requieren API HTTP, flujo de trabajo y orquestación.

*Hay muchas aplicaciones posibles de tecnología de integración, desde una aplicación sencilla punto a punto hasta un bus de servicio empresarial completo. Esta serie de arquitectura expone los componentes reutilizables para la creación de cualquier aplicación de integración: los arquitectos deben tener en cuenta los componentes que necesitan para sus aplicaciones e infraestructura.*

   ![Diagrama de la arquitectura: integración empresarial sencilla](./media/logic-apps-architectures-simple-enterprise-integration/simple_arch_diagram.png)

## <a name="architecture"></a>Architecture

La arquitectura consta de los siguientes componentes:

- Un grupo de recursos Un grupo de recursos es un contenedor lógico de recursos de Azure.
- Azure API Management. Azure API Management es una plataforma totalmente administrada para publicar, proteger y transformar API HTTP.
- Portal para desarrolladores de Azure API Management. Cada instancia de Azure API Management incluye un portal para desarrolladores, que proporciona acceso a documentación y códigos de ejemplo, además de la posibilidad de probar una API.
- Azure Logic Apps. Logic Apps es una plataforma sin servidor para la creación de integración y flujo de trabajo de ámbito empresarial.
- Conectores. Logic Apps usa los conectores para conectarse a servicios de uso habitual. Logic Apps ya dispone de cientos de conectores diferentes, pero también se pueden crear con un conector personalizado.
- Dirección IP. El servicio Azure API Management tiene una dirección IP pública fija y un nombre de dominio. El nombre de dominio es un subdominio de azure-api.net; por ejemplo, contoso. azure-api.net. Logic Apps también tiene una dirección IP pública; sin embargo, en esta arquitectura, se restringe el acceso para llamar a puntos de conexión de Logic Apps solo a la dirección IP de API Management (por motivos de seguridad).
- Azure DNS. DNS de Azure es un servicio de hospedaje para los dominios DNS, que permite resolver nombres mediante la infraestructura de Microsoft Azure. Al hospedar dominios en Azure, puede administrar los registros DNS con las mismas credenciales, API, herramientas y facturación que con los demás servicios de Azure. Para usar un nombre de dominio personalizado, como contoso.com, cree registros DNS que asignen el nombre de dominio personalizado a la dirección IP. Para más información, consulte Configuración de un nombre de dominio personalizado en Azure API Management.
- Azure Active Directory (Azure AD). Use Azure AD u otro proveedor de identidades para la autenticación. Azure AD proporciona autenticación para acceder a los puntos de conexión de API (pasando una instancia de JSON Web Token para API Management a efectos de validación) y puede proteger el acceso al portal para desarrolladores de API Management (solo los niveles Estándar y Premium).

Esta arquitectura tiene algunos patrones fundamentales para su funcionamiento:

1. Las API HTTP back-end se publican en el portal para desarrolladores de API Management, lo que permite a los desarrolladores, ya sean internos en la organización, externos o ambos, integrar las llamadas a dichas API en las aplicaciones.
2. Las API compuestas se compilan con Logic Apps, mediante la orquestación de llamadas a sistemas SAAS, servicios de Azure y cualquier API publicada en API Management. Las instancias de Logic Apps también se publican en el portal para desarrolladores de API Management.
3. Las aplicaciones adquieren un token de seguridad OAuth 2.0 que resulta necesario para obtener acceso a una API mediante Azure Active Directory.
4. Azure API Management valida el token de seguridad y pasa la solicitud a la aplicación lógica/API back-end.

## <a name="recommendations"></a>Recomendaciones

Los requisitos pueden diferir de los de la arquitectura que se describe aquí. Use las recomendaciones de esta sección como punto de partida.

### <a name="azure-api-management-tier"></a>Niveles de Azure API Management

Use los niveles Básico, Estándar y Premium, porque ofrecen un Acuerdo de Nivel de Servicio para producción y admiten la escalabilidad horizontal en la región de Azure (el número de unidades varía en función del nivel). El nivel Premium también admite la escalabilidad horizontal entre varias regiones de Azure. Base el nivel seleccionado en el grado de rendimiento necesario y en el conjunto de características. Para más información, consulte los [precios de API Management](https://azure.microsoft.com/pricing/details/api-management/).

Se le cobra por todas las instancias de API Management cuando están en ejecución. Si ha realizado la escalabilidad vertical y no necesita dicho nivel de rendimiento en todo momento, benefíciese de la reducción vertical y la facturación por horas de API Management.

### <a name="logic-apps-pricing"></a>Precios de Logic Apps

Logic Apps funciona como un modelo [sin servidor](logic-apps-serverless-overview.md): la facturación se calcula en función de la acción y la ejecución del conector. Consulte los [precios de Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/) para más información. Actualmente no existen consideraciones sobre los niveles en Logic Apps.

### <a name="logic-apps-for-asynchronous-api-calls"></a>Logic Apps para llamadas API asincrónicas

Logic Apps funciona mejor en escenarios que no requieren baja latencia, como por ejemplo las llamadas API asincrónicas o de ejecución semilarga. Si se requiere baja latencia (por ejemplo, una llamada que bloquea una interfaz de usuario), se recomienda implementar dicha API u operación con una tecnología distinta, como por ejemplo Azure Functions o Web API, implementada con App Service. Aún se recomienda ofrecer esta API a los clientes de API mediante API Management.

### <a name="region"></a>Region

Aprovisione API Management y Logic Apps en la misma región para minimizar la latencia de red. Por lo general, elija la región más cercana a los usuarios.

El grupo de recursos también tiene una región, que especifica dónde se almacenan los metadatos de la implementación y desde dónde se ejecuta la plantilla de la implementación. Coloque el grupo de recursos y sus recursos en la misma región. Esto puede mejorar la disponibilidad durante la implementación.

## <a name="scalability-considerations"></a>Consideraciones sobre escalabilidad

Los administradores de API Management deben agregar [directivas de almacenamiento en caché](../api-management/api-management-howto-cache.md) cuando proceda, a fin de aumentar la escalabilidad del servicio y reducir la carga de sus servicios back-end.

Los niveles Básico, Estándar y Premium de Azure API Management se pueden escalar horizontalmente en una región de Azure para ofrecer mayor capacidad. Los administradores pueden usar la métrica de capacidad en el menú Métricas para analizar el uso de su servicio y escalar o reducir verticalmente, según proceda.

Recomendaciones de escalado de un servicio API Management:

- Necesidades de escalado para tener en cuenta los patrones de tráfico: los clientes con patrones de tráfico más volátiles tendrán mayor necesidad de aumentar la capacidad.
- Una capacidad coherente superior al 66 % puede indicar la necesidad de escalar verticalmente.
- Una capacidad coherente inferior al 20 % puede indicar una oportunidad para reducir verticalmente.
- Siempre se recomienda realizar una prueba de carga del servicio API Management con una carga representativa antes de habilitarlo en producción.

Los servicios del nivel Premium se pueden escalar horizontalmente en varias regiones de Azure. Los clientes que realizan la implementación de esta forma se beneficiarán de un Acuerdo de Nivel de Servicio más amplio (99,95 % con respecto a un 99,9 %) y podrán aprovisionar servicios cerca de los usuarios en varias regiones.

Con el modelo sin servidor de Logic Apps, los administradores ya no necesitan realizar consideraciones adicionales sobre la escalabilidad del servicio; el servicio se escala automáticamente para satisfacer la demanda.

## <a name="availability-considerations"></a>Consideraciones sobre disponibilidad

En el momento de escribir este artículo, el Acuerdo de Nivel de Servicio (SLA) de Azure API Management es del 99,9 % para los niveles Básico, Estándar y Premium. Las configuraciones del nivel Premium con la implementación de al menos una unidad en dos o más regiones tienen un Acuerdo de Nivel de Servicio de un 99,95 %.

En el momento de escribir este artículo, el Acuerdo de Nivel de Servicio para Azure Logic Apps es del 99,9 %.

### <a name="backups"></a>Copias de seguridad

Se debe realizar una [copia de seguridad regular](../api-management/api-management-howto-disaster-recovery-backup-restore.md) (basada, según corresponda, en la regularidad de los cambios) de la configuración de Azure API Management, y los archivos de copia de seguridad deben almacenarse en una ubicación o región de Azure distintas a las de donde reside el servicio. Los clientes pueden seleccionar posteriormente alguna de las dos opciones para su estrategia de recuperación ante desastres:

1. En un evento de recuperación ante desastres, se aprovisiona una nueva instancia de API Management, donde se restablece la copia de seguridad y se redirigen los registros DNS.
2. Los clientes mantienen una copia pasiva de su servicio en otra región de Azure (lo que incurre en costos adicionales), donde se restablecen las copias de seguridad con regularidad. En un evento de recuperación ante desastres, solo es necesario redirigir los registros DNS para restaurar el servicio.

Como las instancias de Logic Apps se pueden volver a crear muy rápido y siguen un modelo sin servidor, la copia de seguridad se realiza guardando una copia de la plantilla asociada de Azure Resource Manager. Se pueden guardar en un control de código fuente o integrar en un proceso de integración continua/implementaciónc continua de un cliente.

Las instancias de Logic Apps publicadas con API Management necesitarán que sus ubicaciones se actualicen en caso de que se muevan a un centro de datos distinto. Esto puede realizarse con un script de PowerShell sencillo, a fin de actualizar la propiedad de back-end de la API.

## <a name="manageability-considerations"></a>Consideraciones sobre la manejabilidad

Cree grupos de recursos independientes para entornos de producción, desarrollo y pruebas. Esto facilita la administración de implementaciones, la eliminación de implementaciones de prueba y la asignación de derechos de acceso.
Al asignar recursos a grupos de recursos, tenga en cuenta lo siguiente:

- Ciclo de vida. En general, coloque los recursos con el mismo ciclo de vida en el mismo grupo de recursos.
- Acceso. Puede usar el [control de acceso basado en rol](../role-based-access-control/overview.md) (RBAC) para aplicar directivas de acceso a los recursos de un grupo.
- Facturación. Puede ver los costes acumulados del grupo de recursos.
- Plan de tarifa de API Management: se recomienda usar el nivel Desarrollador para entornos de desarrollo y pruebas. Para entornos de preproducción, se recomienda implementar una réplica del entorno de producción, ejecutar pruebas y luego apagarla para minimizar costos.

Para más información, consulte [Información general del grupo de recursos](../azure-resource-manager/resource-group-overview.md).

### <a name="deployment"></a>Implementación

Se recomienda utilizar [plantillas de Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) para implementar Azure API Management y Azure Logic Apps. Las plantillas facilitan la automatización de implementaciones mediante PowerShell o la interfaz de la línea de comandos (CLI) de Azure.

Se recomienda colocar las instancias de Azure API Management y las aplicaciones individuales de Logic Apps en sus propias plantillas independientes de Resource Manager. Esto permitirá almacenarlas en sistemas de control de código fuente. Estas plantillas pueden implementarse juntas o por separado como parte de un proceso de integración continua/implementación continua.

### <a name="versions"></a>Versiones

Cada vez que realiza un cambio de configuración de una aplicación lógica o una implementación de una actualización con una plantilla de Resource Manager, se guarda una copia de dicha versión para su comodidad (se guardarán todas las versiones que tengan un historial de ejecución). Puede usar estas versiones para realizar un seguimiento de los cambios históricos y promover también una versión para que sea la configuración actual de la aplicación lógica; con ello, por ejemplo, puede revertir una aplicación lógica con eficacia.

API Management tiene dos [conceptos de control de versiones](https://blogs.msdn.microsoft.com/apimanagement/2018/01/11/versions-revisions-general-availibility/) distintos, pero no complementarios:

- Versiones utilizadas para proporcionar a los clientes de API la opción de API que consumirían en función de sus necesidades (por ejemplo, v1, v2, beta o producción).
- Revisiones que permiten a los administradores de API realizar cambios seguros de una API e implementarlos para usuarios con un comentario opcional.

En el contexto de la implementación, las revisiones de API Management deben considerarse como una forma de realizar cambios con seguridad, guardar un historial de los cambios e informar a los consumidores de API de dichos cambios. Una revisión puede crearse en un entorno de desarrollo e implementarse entre otros entornos con el uso de plantillas de Resource Manager.

Aunque las revisiones pueden usarse para probar una API antes de que se convierta en la "actual" y de que los usuarios puedan acceder a ella, no se recomienda usar este mecanismo para cargas o pruebas de integración; en su lugar, se deben usar entornos de preproducción o de prueba independientes.

### <a name="configuration"></a>Configuración

Nunca compruebe contraseñas, claves de acceso ni cadenas de conexión en el control de código fuente. Si resultan necesarias, use la técnica apropiada para implementar y proteger estos valores. 

En Logic Apps, cualquier valor confidencial necesario en la aplicación lógica, que no se pueda crear en forma de conexión, debe almacenarse en Azure Key Vault y hacer referencia a él desde una plantilla de Resource Manager. También se recomienda utilizar parámetros de plantilla de implementación junto con los archivos de parámetros para cada entorno. Puede encontrar más instrucciones en [Parámetros seguros y entradas dentro de un flujo de trabajo](logic-apps-securing-a-logic-app.md#secure-parameters-and-inputs-within-a-workflow).

En API Management, los secretos se administran con objetos denominados Propiedades/Valores con nombre. Estos almacenan de forma segura los valores a los que se puede acceder en directivas de API Management. Vea cómo [administrar secretos en API Management](../api-management/api-management-howto-properties.md).

### <a name="diagnostics-and-monitoring"></a>Diagnóstico y supervisión

Tanto [API Management](../api-management/api-management-howto-use-azure-monitor.md) como [Logic Apps](logic-apps-monitor-your-logic-apps.md) admiten la supervisión operativa con [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md). Azure Monitor está habilitado de forma predeterminada y proporcionará información basada en las diferentes métricas configuradas para cada servicio.

Además, hay más opciones para cada servicio:

- Los registros de Logic Apps se pueden enviar a [Log Analytics](logic-apps-monitor-your-logic-apps-oms.md) para realizar análisis más exhaustivos y agregarlos a los paneles.
- API Management permite configurar Application Insights para la supervisión de DevOps.
- API Management admite la [plantilla de solución de Power BI para análisis de API personalizados](http://aka.ms/apimpbi). Esta plantilla de solución permite a los clientes crear su propia solución personalizada de análisis, con los informes disponibles en Power BI para los usuarios empresariales.

## <a name="security-considerations"></a>Consideraciones sobre la seguridad

En esta sección se enumeran las consideraciones de seguridad que son específicas de los servicios de Azure descritos en este artículo, implementadas en la arquitectura según se describe. No es una lista completa de procedimientos de seguridad recomendados.

- Use el control de acceso basado en rol (RBAC) para garantizar niveles apropiados de acceso a los usuarios.
- Proteja los puntos de conexión de API públicos en API Management con OAuth/Open IDConnect. Para ello, configure un proveedor de identidades y agregue una directiva de validación de JWT.
- Conexión a servicios back-end desde API Management mediante certificados mutuos
- Proteja instancias de Logic Apps basadas en desencadenadores HTTP con la creación de una lista blanca de direcciones IP que apunte a la dirección IP de API Management. Esto evita llamar a la aplicación lógica desde la red pública de Internet sin pasar primero por API Management.

En esta arquitectura de referencia se ha mostrado cómo compilar una plataforma de integración empresarial sencilla con los servicios de integración de Azure.

## <a name="next-steps"></a>Pasos siguientes

* [Integración empresarial con colas y eventos](logic-apps-architectures-enterprise-integration-with-queues-events.md)
