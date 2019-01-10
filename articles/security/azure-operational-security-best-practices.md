---
title: Procedimientos recomendados de seguridad operativa de Azure | Microsoft Docs
description: En este artículo se proporciona un conjunto de procedimientos recomendados para la reforzar la seguridad operativa de Azure.
services: security
documentationcenter: na
author: TerryLanfear
manager: mbaldwin
editor: tomsh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2018
ms.author: terrylan
ms.openlocfilehash: 14c21db9e1da23e97815953937f434fe7f25e354
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53725327"
---
# <a name="azure-operational-security-best-practices"></a>Procedimientos recomendados de seguridad operativa de Azure
Por seguridad operativa de Azure, se entienden los servicios, los controles y las características disponibles para los usuarios para proteger sus datos, aplicaciones y otros recursos en Azure. La seguridad operativa de Azure se basa en un marco que incorpora el conocimiento adquirido a través de diversas funcionalidades exclusivas de Microsoft, incluido el [ciclo de vida de desarrollo de seguridad (SDL)](https://www.microsoft.com/sdl), el programa [Microsoft Security Response Center](https://www.microsoft.com/msrc?rtc=1) y un conocimiento en profundidad del panorama de amenazas de ciberseguridad.

En este artículo se explica un conjunto de procedimientos recomendados de seguridad. Estos procedimientos recomendados se derivan de nuestra experiencia con la seguridad de las bases de datos de Azure, y de las experiencias de clientes como usted.

Para cada procedimiento recomendado, explicaremos:
-   Qué es el procedimiento recomendado
-   Por qué le conviene habilitar este procedimiento recomendado
-   Cuál podría ser el resultado si no habilita el procedimiento recomendado
- Cómo aprender a habilitar el procedimiento recomendado

Este artículo, Procedimientos recomendados de seguridad operativa de Azure, se basa en las funcionalidades y los conjuntos de características de la plataforma Azure existentes cuando se redactó. Las opiniones y las tecnologías cambian con el tiempo, por lo que se actualizará de forma periódica para reflejar esos cambios.

## <a name="monitor-storage-services-for-unexpected-changes-in-behavior"></a>Supervisar servicios de almacenamiento para detectar cambios inesperados de comportamiento
Diagnosticar y solucionar problemas en una aplicación distribuida hospedada en un entorno de nube puede ser más complicado que en los entornos tradicionales. Las aplicaciones se pueden implementar en una infraestructura PaaS o IaaS, en una ubicación local, en un dispositivo móvil o en varios de estos entornos combinados. El tráfico de red de la aplicación puede atravesar redes públicas y privadas, y la aplicación podría usar varias tecnologías de almacenamiento.

Debe supervisar continuamente los servicios de almacenamiento que usa la aplicación para detectar cualquier cambio inesperado de comportamiento (por ejemplo, tiempos de respuesta más lentos). Use los registros para recopilar datos más detallados y analizar un problema en profundidad. La información de diagnóstico obtenida de la supervisión y los registros le ayuda a determinar la causa raíz del problema detectado por la aplicación. Luego puede solucionar el problema y determinar los pasos adecuados para remediarlo.

[Azure Storage Analytics](../storage/storage-analytics.md) registra y proporciona datos de métricas de una cuenta de Azure Storage. Se recomienda usar estos datos para hacer un seguimiento de solicitudes, analizar tendencias de uso y diagnosticar problemas con la cuenta de almacenamiento.

## <a name="prevent-detect-and-respond-to-threats"></a>Prevención de las amenazas, detección y respuesta
[Azure Security Center](../security-center/security-center-intro.md) ayuda a evitar, detectar y responder a amenazas con más visibilidad y control sobre la seguridad de sus recursos de Azure. Proporciona administración de directivas y supervisión de seguridad integradas en las suscripciones de Azure, ayuda a detectar las amenazas que podrían pasar desapercibidas y funciona con distintas soluciones de seguridad.

El nivel Gratis de Security Center ofrece seguridad limitada solo para los recursos de Azure. El nivel Estándar amplía estas funcionalidades al entorno local y otras nubes. El nivel Estándar de Security Center le ayuda a encontrar y corregir vulnerabilidades de seguridad, aplicar controles de acceso y de aplicación para bloquear actividades malintencionadas, detectar amenazas mediante análisis e inteligencia y responder rápidamente en caso de ataque. Dicho plan se puede probar de forma gratuita los primeros 60 días. Se recomienda [incorporar la suscripción de Azure al nivel Estándar de Security Center](../security-center/security-center-get-started.md).

Use Security Center para obtener una vista centralizada del estado de seguridad de todos los recursos de Azure. Compruebe que los controles de seguridad adecuados se han implementado y configurado correctamente, e identifique de un vistazo cualquier recurso que demande atención.

## <a name="monitor-end-to-end-scenario-based-network-monitoring"></a>Supervisión de redes de un extremo a otro basada en escenarios
Los clientes crean una red de un extremo a otro en Azure mediante la combinación de recursos de red como una red virtual, ExpressRoute, Application Gateway y equilibradores de carga. Se puede supervisar cada uno de los recursos de la red.

[Azure Network Watcher](../network-watcher/network-watcher-monitoring-overview.md) es un servicio regional. Use sus herramientas de diagnóstico y visualización para supervisar y diagnosticar problemas en un nivel de escenario de red en, hacia y desde Azure.

A continuación se ofrecen procedimientos recomendados para la supervisión de redes y se indican las herramientas disponibles.

**Procedimiento recomendado**: Automatización de la supervisión de la red remota con captura de paquetes.  
**Detalles**: supervise y diagnostique problemas de red sin iniciar sesión en las máquinas virtuales mediante Network Watcher. Desencadene la [captura de paquetes](../network-watcher/network-watcher-alert-triggered-packet-capture.md) estableciendo alertas y obtenga acceso a información de rendimiento en tiempo real en el ámbito de paquete. Cuando vea un problema, podrá investigar en detalle para mejorar los diagnósticos.

**Procedimiento recomendado**: Extraer conclusiones sobre el tráfico de la red mediante registros de flujo.  
**Detalles**: conozca al detalle los patrones de tráfico de red mediante los [registros de flujo del grupo de seguridad de red](../network-watcher/network-watcher-nsg-flow-logging-overview.md). La información de los registros de flujo le ayuda a recopilar datos para el cumplimiento, la auditoría y la supervisión del perfil de seguridad de red.

**Procedimiento recomendado**: diagnóstico de problemas de conectividad VPN.  
**Detalles**: use Network Watcher para [diagnosticar los problemas más comunes de conexión y VPN Gateway](../network-watcher/network-watcher-diagnose-on-premises-connectivity.md). No solo puede identificar el problema, sino también usar registros detallados para investigar más.

## <a name="secure-deployment-by-using-proven-devops-tools"></a>Implementación segura mediante herramientas de DevOps comprobadas
Use los siguientes procedimientos recomendados de DevOps para garantizar que la empresa y los equipos sean productivos y eficientes.

**Procedimiento recomendado**: automatizar la compilación e implementación de servicios.  
**Detalles**: [Infraestructura como código](https://docs.microsoft.com/azure/devops/learn/what-is-infrastructure-as-code) es un conjunto de técnicas y procedimientos que ayudan a los profesionales de TI a eliminar la carga que supone la compilación y administración cotidianas de una infraestructura modular. Permite a los profesionales de TI compilar y mantener sus entornos de servidores modernos de forma similar a como los desarrolladores de software compilan y mantienen el código de las aplicaciones.

Puede usar [Azure Resource Manager](https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/) para aprovisionar las aplicaciones mediante una plantilla declarativa. En una plantilla, puede implementar varios servicios junto con sus dependencias. Use la misma plantilla para implementar la aplicación de forma repetida durante cada fase de su ciclo de vida.

**Procedimiento recomendado**: compilar e implementar automáticamente en aplicaciones web o servicios en la nube de Azure.  
**Detalles**: puede usar Azure Pipelines para [compilar e implementar automáticamente](https://docs.microsoft.com/azure/devops/pipelines/index?view=vsts) en aplicaciones web o servicios en la nube de Azure. Azure Pipelines implementa automáticamente los archivos binarios después de realizar una compilación en Azure tras cada inserción del código en el repositorio. El proceso de compilación del paquete es equivalente al comando Package de Visual Studio y los pasos de publicación son equivalentes al comando Publish de Visual Studio.

**Procedimiento recomendado**: usar la implementación continua.  
**Detalles**: [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/index?view=vsts) es una solución para automatizar la implementación en varias fases y la administración del proceso de publicación. Cree canalizaciones de implementación continua y administrada con el fin de que el lanzamiento sea rápido, sencillo y frecuente. Con Azure Pipelines, puede automatizar el proceso de publicación y tener flujos de trabajo de aprobación predefinidos. Realice la implementación de forma local y en la nube, amplíela y personalícela según sea necesario.

**Procedimiento recomendado**: Compruebe el rendimiento de su aplicación antes de iniciarla o de implementar actualizaciones en la producción.  
**Detalles**: ejecute [pruebas de carga](https://docs.microsoft.com/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) basadas en la nube mediante Azure Test Plans para:

- Detectar problemas de rendimiento en la aplicación.
- Mejorar la calidad de implementación.
- Asegurarse de que la aplicación siempre está disponible.
- Asegurarse de que la aplicación puede controlar el tráfico de la siguiente campaña de marketing o el siguiente lanzamiento.

**Procedimiento recomendado**: supervisar el rendimiento de las aplicaciones.  
**Detalles**: [Azure Application Insights](../application-insights/app-insights-overview.md) es un servicio de Application Performance Management (APM) extensible para desarrolladores web en varias plataformas. Use Application Insights para supervisar la aplicación web en vivo. Se detectan automáticamente las anomalías de rendimiento. Incluye herramientas de análisis que le ayudan a diagnosticar problemas y a comprender lo que hacen realmente los usuarios con la aplicación. Está diseñado para ayudarle a mejorar continuamente el rendimiento y la facilidad de uso.

## <a name="mitigate-and-protect-against-ddos"></a>Mitigar y proteger frente a DDoS
La denegación de servicio distribuido (DDoS) es un tipo de ataque que intenta agotar los recursos de la aplicación. El objetivo es afectar a la disponibilidad de la aplicación y a su capacidad para administrar solicitudes legítimas. Estos ataques son cada vez más sofisticados y tienen un mayor tamaño e impacto. Pueden ir dirigidos a cualquier punto de conexión que sea públicamente accesible a través de Internet.

Diseñar y compilar para la resistencia frente a DDoS requiere planear y diseñar para una serie de modos de error.

Estos son los procedimientos recomendados para compilar servicios resistentes a DDoS en Azure.

**Procedimiento recomendado**: Asegúrese de que la seguridad es prioritaria durante todo el ciclo de vida de una aplicación, desde su diseño e implementación hasta la implementación y las operaciones. Las aplicaciones pueden tener errores que permiten que un volumen relativamente bajo de solicitudes usen muchos recursos y produzcan una interrupción del servicio.  
**Detalles**: Para proteger un servicio que se ejecuta en Microsoft Azure, debe conocer bien la arquitectura de su aplicación y centrarse en los [cinco pilares de la calidad del software](https://docs.microsoft.com/azure/architecture/guide/pillars). Debe conocer los volúmenes de tráfico típicos, el modelo de conectividad entre la aplicación y otras aplicaciones, y los puntos de conexión del servicio expuestos a la red pública de Internet.

Es de vital importancia garantizar que una aplicación sea lo suficientemente resistente para tratar con un ataque de denegación de servicio dirigido a la propia aplicación. La seguridad y la privacidad están integradas en la plataforma Azure, comenzando por el [ciclo de vida del desarrollo de la seguridad (SDL)](https://www.microsoft.com/en-us/sdl). El SDL aborda la seguridad en cada fase de desarrollo y se asegura de que Azure se actualice continuamente para que sea aún más seguro.

**Procedimiento recomendado**: Debe diseñar sus aplicaciones de modo que se puedan [escalar horizontalmente](https://docs.microsoft.com/azure/architecture/guide/design-principles/scale-out) para satisfacer la demanda de una carga mayor, específicamente en caso de un ataque de DDoS. Si la aplicación depende de una única instancia de un servicio, crea un único punto de error. El aprovisionamiento de varias instancias hace que el sistema sea más resistente y más escalable.  
**Detalles**: Para [Azure App Service](../app-service/app-service-value-prop-what-is.md), seleccione un [Plan de App Service](../app-service/overview-hosting-plans.md) que ofrezca varias instancias.

Para Azure Cloud Services, configure cada uno de los roles para utilizar [varias instancias](../cloud-services/cloud-services-choose-me.md).

En el caso de [Azure Virtual Machines](../virtual-machines/windows/overview.md), asegúrese de que la arquitectura de las máquinas virtuales incluya más de una máquina virtual y de que cada una de ellas se incluya en un [conjunto de disponibilidad](../virtual-machines/virtual-machines-windows-manage-availability.md). Se recomienda usar conjuntos de escalado de máquinas virtuales para contar con funcionalidades de escalado automático.

**Procedimiento recomendado**: Disponer en niveles la defensa de la seguridad en una aplicación reduce las probabilidades de éxito de un ataque. Implemente diseños seguros para las aplicaciones con las funcionalidades integradas de la plataforma Azure.  
**Detalles**: el riesgo de ataque aumenta con el tamaño (área expuesta) de la aplicación. Puede reducir el área expuesta mediante la creación de listas blancas para cerrar el espacio de direcciones IP expuesto y los puertos de escucha que no sean necesarios en los equilibradores de carga ([Azure Load Balancer](../load-balancer/load-balancer-get-started-internet-portal.md) y [Azure Application Gateway](../application-gateway/application-gateway-create-probe-portal.md)).

Los [grupos de seguridad de red](../virtual-network/security-overview.md) constituyen otra manera de reducir el área expuesta a ataques. Puede usar [etiquetas de servicio](../virtual-network/security-overview.md#service-tags) y [grupos de seguridad de la aplicación](../virtual-network/security-overview.md#application-security-groups) para minimizar la complejidad de la creación de reglas de seguridad y configurar la seguridad de la red como una extensión natural de la estructura de una aplicación.

Debe implementar los servicios de Azure en una [red virtual](../virtual-network/virtual-networks-overview.md) siempre que sea posible. Este procedimiento permite que los recursos del servicio se comuniquen mediante direcciones IP privadas. De forma predeterminada, el tráfico de los servicios Azure desde una red virtual usa direcciones IP públicas como direcciones IP de origen.

Con los [puntos de conexión de servicio](../virtual-network/virtual-network-service-endpoints-overview.md), el tráfico del servicio cambia para usar direcciones privadas de red virtual como direcciones IP de origen al acceder al servicio de Azure desde una red virtual.

Con frecuencia vemos ataques a los recursos locales de un cliente, además de a los recursos en Azure. Si conecta un entorno local a Azure, minimice la exposición de los recursos locales a la red pública de Internet.

Azure tiene dos [ofertas de servicio](../virtual-network/ddos-protection-overview.md) de DDoS que proporcionan protección frente a ataques de red:

- La protección básica se integra en Azure de forma predeterminada sin costo adicional. El tamaño y la capacidad de la red de implementación global de Azure proporciona una defensa contra los ataques al nivel de red más comunes mediante la supervisión constante del tráfico y la mitigación en tiempo real. La protección básica no requiere ningún cambio de configuración ni de aplicación y ayuda a proteger todos los servicios de Azure, incluidos servicios PaaS como Azure DNS.
- La protección estándar proporciona funcionalidades avanzadas de mitigación de DDoS frente a ataques de red. Se ajusta automáticamente para proteger los recursos específicos de Azure. La protección se puede habilitar fácilmente durante la creación de redes virtuales. También puede realizarse después de la creación y no requiere ningún cambio de aplicación o recurso.

## <a name="next-steps"></a>Pasos siguientes
Vea [Patrones y procedimientos recomendados de seguridad en Azure](security-best-practices-and-patterns.md) para obtener más procedimientos recomendados de seguridad que pueda aplicar cuando diseñe, implemente y administre las soluciones en la nube mediante Azure.

En los siguientes recursos se ofrece más información general sobre la seguridad de Azure y los servicios de Microsoft relacionados:
* [Blog del equipo de seguridad de Azure](https://blogs.msdn.microsoft.com/azuresecurity/): ofrece información actualizada sobre lo último en seguridad de Azure
* [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx): aquí podrá notificar vulnerabilidades de seguridad de Microsoft, incluidos problemas con Azure, o también mediante correo electrónico a secure@microsoft.com.
