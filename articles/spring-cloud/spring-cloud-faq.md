---
title: Preguntas frecuentes sobre Azure Spring Cloud | Microsoft Docs
description: Examine las preguntas frecuentes acerca de Azure Spring Cloud
services: spring-cloud
author: v-vasuke
manager: jeconnoc
editor: ''
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 10/07/2019
ms.author: v-vasuke
ms.openlocfilehash: 09970468a277dcaf3f28b4f5065572568089a12e
ms.sourcegitcommit: d773b5743cb54b8cbcfa5c5e4d21d5b45a58b081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72038494"
---
# <a name="frequently-asked-questions"></a>Preguntas frecuentes

En este artículo se abordan las preguntas más frecuentes sobre Azure Spring Cloud. 

## <a name="general"></a>General

### <a name="why-azure-spring-cloud"></a>¿Por qué Azure Spring Cloud?

Azure Spring Cloud proporciona una plataforma como servicio (PaaS) para los desarrolladores de Spring. Azure Spring Cloud administra la infraestructura de la aplicación, para que el usuario pueda centrarse en el código de la aplicación y en la lógica de negocios. Las características centrales integradas en Azure Spring Cloud incluyen Eureka, Config Server, Service Registry Server, Pivotal Build Service e implementaciones Blue-Green, entre otras. Este servicio también permite a los desarrolladores enlazar sus aplicaciones con servicios de Azure como CosmosDB, MySQL y Azure Cache for Redis.

Azure Spring Cloud mejora la experiencia de diagnóstico de aplicaciones de los desarrolladores y operadores, ya que integra Azure Monitor, Application Insights y Log Analytics.

### <a name="what-service-plans-does-azure-spring-cloud-offer"></a>¿Qué planes de servicio ofrece Azure Spring Cloud?

Azure Spring Cloud es gratis durante el período de versión preliminar.

Resource | Importe
------- | -------
vCPU | 4
Memoria | 8 GBytes
Instancias de aplicación por aplicación de Spring | 20
Instancias de aplicación totales por instancia del servicio Azure Spring Cloud | 50*
Instancias del servicio Azure Spring Cloud por región y suscripción | 2*
Volúmenes persistentes | 10 x 50 GBytes

*_Abra una [incidencia de soporte técnico](https://azure.microsoft.com/support/faq/) para aumentar el límite._

Para más información, consulte [Preguntas más frecuentes de soporte técnico de Azure](https://azure.microsoft.com/support/faq/).

### <a name="how-secure-is-azure-spring-cloud"></a>¿Es seguro Azure Spring Cloud?

La seguridad y la privacidad son dos de las principales prioridades para los clientes de Azure y de Azure Spring Cloud. Azure garantiza que el cliente es el único que tiene acceso a los datos, registros o configuraciones de la aplicación mediante el cifrado seguro de todos estos datos. Las instancias de servicio de Azure Spring Cloud están todas aisladas unas de otras.

Azure Spring Cloud proporciona administración completa de certificados y de SSL.

Las revisiones de seguridad críticas de los runtimes de OpenJDK y Spring Cloud se aplicarán a Azure Spring Cloud lo antes posible.

### <a name="which-regions-azure-spring-cloud-are-available"></a>¿En qué regiones está disponible Azure Spring Cloud?

Este de EE. UU., Oeste de EE. UU. 2, Oeste de Europa y Sudeste Asiático.

### <a name="what-are-the-known-limitations-of-azure-spring-cloud"></a>Limitaciones conocidas de Azure Spring Cloud

Estas son las limitaciones conocidas de Azure Spring Cloud mientras el servicio está en versión preliminar.

* `spring.application.name` se sustituirá por el nombre de la aplicación usado para crear cada aplicación.
* `server.port` no se permite en el archivo de configuración del repositorio de Git. Si lo agrega al archivo de configuración, es probable que provoque que no se pueda acceder a la aplicación desde otras aplicaciones o desde Internet.
* Azure Portal y las plantillas de Resource Manager no admiten la carga de paquetes de aplicación. Esto se puede hacer mediante la implementación de la aplicación por parte de la CLI de Azure.
* Para ver las limitaciones de la cuota, consulte [Planes de servicio que ofrece Azure Spring Cloud](#what-service-plans-does-azure-spring-cloud-offer).

### <a name="how-can-i-provide-feedback-and-report-issues"></a>¿Cómo puedo realizar comentarios y notificar incidencias?

Si ha creado instancias de servicio de Spring en Azure Spring Cloud, puede solicitar una [solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Si aún no lo ha incorporado a Azure Spring Cloud, puede ir al foro de [comentarios de Azure](https://feedback.azure.com/) para solicitar características o aportar comentarios.

## <a name="development"></a>Desarrollo

### <a name="i-am-a-spring-developer-but-new-to-azure-what-is-the-quickest-way-for-me-to-learn-how-to-develop-aazure-spring-cloud-application"></a>Soy desarrollador de Spring y nunca he usado Azure, ¿cuál es la forma más rápida de aprender a desarrollar una aplicación de Azure Spring Cloud?

La forma más rápida de empezar a trabajar con Azure Spring Cloud es seguir [este inicio rápido](spring-cloud-quickstart-launch-app-portal.md).

### <a name="what-java-runtime-does-azure-spring-cloud-support"></a>¿Qué runtime de Java es compatible con Azure Spring Cloud?

Azure Spring Cloud es compatible con Java 8 y 11.

### <a name="where-can-i-see-my-spring-application-logs-and-metrics"></a>¿Dónde puedo ver mis métricas y registros de aplicaciones de Spring?

Busque las métricas en la pestaña Información general de la aplicación y en la pestaña [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-metrics#interacting-with-azure-monitor-metrics).

Azure Spring Cloud admite la exportación de las métricas y los registros de aplicaciones de Spring a Azure Storage, EventHub y [Log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-logs#log-queries). El nombre de la tabla de Log Analytics es `AppPlatformLogsforSpring`. Para habilitarla, consulte este artículo acerca de nuestros [servicios de diagnóstico](diagnostic-services.md).

### <a name="does-azure-spring-cloud-support-distributed-tracing"></a>¿Admite Azure Spring Cloud el seguimiento distribuido?

Sí. Para más información, vaya a [Seguimiento distribuido](spring-cloud-tutorial-distributed-tracing.md).

### <a name="what-resource-types-does-service-binding-support"></a>¿Qué tipos de recursos admiten el enlace de servicios?

Actualmente se admiten tres servicios: Azure Cosmos DB, Azure Database for MySQL y Azure Cache for Redis.

### <a name="can-i-viewaddmove-persistent-volumes-from-inside-my-applications"></a>¿Puedo ver, agregar o mover volúmenes persistentes desde dentro de mis aplicaciones?
Sí.

## <a name="deployment"></a>Implementación

### <a name="does-azure-spring-cloud-support-blue-green-deployment"></a>¿Admite Azure Spring Cloud la implementación Blue-green?
Sí. Para más información, visite la [guía del entorno de ensayo](spring-cloud-howto-staging-environment.md).

### <a name="can-i-access-kubernetes-to-manipulate-my-application-containers"></a>¿Puedo acceder a Kubernetes para manipular mis contenedores de aplicaciones?

No.  Azure Spring Cloud ayuda a desviar la atención del desarrollador de la arquitectura subyacente, lo que le permite concentrarse en el código de la aplicación y en la lógica de negocios.

### <a name="does-azure-spring-cloud-support-building-containers-from-source"></a>¿Admite Azure Spring Cloud la creación de contenedores desde el origen?

Sí. Para más información, visite el inicio rápido en que se explica cómo [realizar implementaciones desde el código fuente](spring-cloud-launch-from-source.md).

### <a name="does-azure-spring-cloud-support-autoscaling-in-app-instances"></a>¿Admite Azure Spring Cloud la escalabilidad automática en instancias de aplicaciones?

No.

### <a name="what-are-the-best-practices-for-migrating-existing-spring-microservices-to-azure-spring-cloud"></a>¿Cuáles son los procedimientos recomendados para migrar los microservicios de Spring existentes a Azure Spring Cloud?

Antes de migrar los microservicios de Spring existentes a Azure Spring Cloud:
* Todas las dependencias de la aplicación deben resolverse.
* Es preciso preparar las entradas de configuración, las variables de entorno y los parámetros de JVM para que pueda compararlos con los de la implementación en Azure Spring Cloud.
* Si desea usar el enlace de servicios, examine los servicios de Azure y asegúrese de que ha establecido los permisos de acceso adecuados.
* Se recomienda quitar o deshabilitar los servicios insertados que podrían entrar en conflicto con los servicios administrados por Azure Spring Cloud, como nuestro servicio Service Discovery, Config Server, etc.
*-* Es aconsejable usar las bibliotecas de Pivotal Spring oficiales y estables. Las versiones no oficiales, beta o bifurcadas de las bibliotecas de Pivotal Spring no tienen el soporte de un Acuerdo de Nivel de Servicio.

Después de la migración, supervise las métricas de la CPU y RAM, y el tráfico de red para asegurarse de que las instancias de la aplicación se escalan adecuadamente.

## <a name="next-steps"></a>Pasos siguientes

[Si tiene cualquier otra duda, consulte la guía para la solución de problemas](spring-cloud-troubleshoot.md).