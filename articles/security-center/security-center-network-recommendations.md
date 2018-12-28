---
title: Protección de los recursos de red en Azure Security Center | Microsoft Docs
description: En este documento se explica cómo las recomendaciones de Azure Security Center ayudan a proteger los recursos de red y a cumplir las directivas de seguridad.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 96c55a02-afd6-478b-9c1f-039528f3dea0
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 3bb8d4796d23b84f815d28efa45af619efe19af3
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53338240"
---
# <a name="protect-your-network-resources-in-azure-security-center"></a>Protección de los recursos de red en Azure Security Center
Azure Security Center analiza continuamente el estado de seguridad de los recursos de Azure para los procedimientos recomendados de seguridad de red. Cuando Security Center identifica posibles vulnerabilidades de seguridad, crea recomendaciones que lo guiarán por el proceso de configuración de los controles necesarios para reforzar y proteger sus recursos.

En este artículo se abordan las recomendaciones que se aplican a los recursos de Azure desde una perspectiva de seguridad de red. Las recomendaciones sobre redes se centran en los firewalls de próxima generación, los grupos de seguridad de red, el acceso a la VM JIT a través de reglas de tráfico entrantes permisivas y mucho más. Para obtener una lista de posibles recomendaciones de red y acciones de corrección, consulte [Administración de recomendaciones de seguridad en Azure Security Center.](security-center-recommendations.md)

> [!NOTE]
> La página **Redes** permite profundizar en el estado de los recursos de Azure desde una perspectiva de red. El mapa de red y los controles de red adaptables están disponibles solo para el nivel estándar de Azure Security Center. [Si usa el nivel gratis, haga clic en el botón para **Ver redes heredadas** y recibir recomendaciones de recursos de red](#legacy-networking).
>

La página **Redes** proporciona información general de las secciones en las que puede profundizar para obtener más información sobre el estado de los recursos de red:

- Mapa de red (solo en el nivel estándar de Azure Security Center)
- Protección de NSG (disponible próximamente. Regístrese para obtener la versión preliminar)
- Recomendaciones de seguridad de red.
- Hoja **Redes** heredadas (anteriormente hoja de redes) 
 
![Panel de redes](./media/security-center-network-recommendations/networking-pane.png)

## <a name="network-map"></a>Mapa de red
El mapa de red interactivo proporciona una vista gráfica con superposiciones de seguridad que le ofrecen recomendaciones e información para proteger sus recursos de red. Con el mapa podrá ver la topología de red de sus cargas de trabajo de Azure, las conexiones entre sus máquinas virtuales y subredes, y la capacidad de explorar en profundidad el mapa para consultar recursos específicos y las recomendaciones para esos recursos.

Para abrir el mapa de red:

1. En Security Center, en Protección de seguridad de recursos, seleccione **Redes**.
2. En **Mapa de red**, haga clic en **Ver topología**.
 
La vista predeterminada del mapa topológico muestra:
- Suscripciones seleccionadas en Azure. El mapa admite varias suscripciones.
- Máquinas virtuales, subredes y redes virtuales del tipo de recurso Resource Manager (no se admiten los recursos de Azure clásico)
- Solo los recursos que tienen [recomendaciones de red](security-center-recommendations.md) con una gravedad media o alta  
- Recursos accesibles desde Internet
- El mapa está optimizado para las suscripciones que seleccionó en Azure. Si modifica la selección, el mapa se vuelve a calcular y a reoptimizar en función de la nueva configuración.  

![Mapa de topología de red](./media/security-center-network-recommendations/network-map-info.png)

## <a name="understanding-the-network-map"></a>Descripción del mapa de red

El mapa de red puede mostrar los recursos de Azure en una vista **Topología** y en una vista **Tráfico**.

### <a name="the-topology-view"></a>Vista de topología

En la vista **Topología** del mapa de red, puede ver la información siguiente acerca de los recursos de red:
- En el círculo interior, puede ver todas las redes virtuales dentro de las suscripciones seleccionadas, el círculo siguiente son todas las subredes, el círculo exterior son todas las máquinas virtuales.
- Las líneas que conectan los recursos en el mapa le permiten saber qué recursos se asocian entre sí, y cómo está estructurada la red de Azure. 
- Use los indicadores de gravedad para obtener rápidamente información general acerca de qué recursos tienen recomendaciones abiertas desde Security Center.
- Puede hacer clic en cualquiera de los recursos para explorar en profundidad cada uno de ellos y ver los detalles de ese recurso y sus recomendaciones directamente en el contexto de la red.  
- Si se muestran demasiados recursos en el mapa, Azure Security Center usa su algoritmo propietario para agruparlos de manera inteligente y resaltar aquellos en estado más crítico y con recomendaciones de gravedad alta. 

Dado que el mapa es interactivo y dinámico, todos los nodos son seleccionables y la vista puede cambiarse en función de los filtros:

1. Puede modificar lo que ve en el mapa de red mediante el uso de los filtros situados en la parte superior. Puede centrarse en el mapa basándose en:
   -  **Estado de seguridad**: puede filtrar el mapa basándose en la gravedad (alta, media, baja) de los recursos de Azure.
   - **Recomendaciones**: puede seleccionar qué recursos se muestran según las recomendaciones activas en esos recursos. Por ejemplo, puede ver únicamente los recursos para los que Security Center recomienda habilitar los grupos de seguridad de red.
   - **Zonas de red**: de manera predeterminada, el mapa muestra solo recursos accesibles desde Internet, también puede seleccionar VM internas.
 
2. Puede hacer clic en **Restablecer** en la esquina superior izquierda en cualquier momento para devolver el mapa al estado predeterminado.

Para explorar en profundidad un recurso:
1. Al seleccionar un recurso específico en el mapa, el panel derecho se abre y le ofrece información general sobre el recurso, soluciones de seguridad conectadas (de existir) y recomendaciones relevantes para el recurso. Es el mismo tipo de comportamiento para cada tipo de recurso que seleccione. 
2. Cuando mantiene el mouse sobre un nodo en el mapa, puede ver información general sobre el recurso, incluida la suscripción, el tipo de recurso y el grupo de recursos.
3. Utilice el vínculo para acercar la información sobre herramientas y reorientar el mapa en ese nodo concreto. 
4. Para reorientar el mapa fuera de un determinado nodo debe alejar.

### <a name="the-traffic-view"></a>Vista de tráfico

La vista de **tráfico** proporciona un mapa con todo el tráfico posible entre los recursos. Esto proporciona un mapa visual de todas las reglas configuradas que define qué recursos pueden comunicarse entre sí. Esto permite ver la configuración existente de los grupos de seguridad de red, así como identificar rápidamente las posibles configuraciones de riesgo en las cargas de trabajo.

### <a name="uncover-unwanted-connections"></a>Descubrir conexiones no deseadas

La importancia de esta vista reside en su capacidad para mostrar estas conexiones permitidas junto con las vulnerabilidades que existen, por lo que puede usar esta sección transversal de datos para aplicar la protección necesaria a los recursos. 

Por ejemplo, podría detectar dos máquinas que no sabía que se podían comunicar, lo que le permitiría aislar mejor cargas de trabajo y subredes.

### <a name="investigate-resources"></a>Investigar los recursos

Para explorar en profundidad un recurso:
1. Al seleccionar un recurso específico en el mapa, el panel derecho se abre y le ofrece información general sobre el recurso, soluciones de seguridad conectadas (de existir) y recomendaciones relevantes para el recurso. Es el mismo tipo de comportamiento para cada tipo de recurso que seleccione. 
2. Haga clic en **Tráfico** para ver la lista de posible tráfico entrante y saliente en el recurso. Se trata de una lista completa de quién puede comunicarse con el recurso y viceversa, así como a través de qué protocolos y puertos.

**Estos datos se basan en el análisis de los Grupos de seguridad de red, así como en algoritmos con aprendizaje automático que analizan varias reglas para comprender las interacciones y cruces.** 

![Mapa del tráfico de red](./media/security-center-network-recommendations/network-map-traffic.png)

## Redes heredadas <a name ="legacy-networking"></a>

Si no tiene el nivel estándar de Security Center, en esta sección se explica cómo obtener recomendaciones gratuitas de redes.

Para obtener acceso a esta información, en la hoja Redes, haga clic en **Ver redes heredadas**. 

![Redes heredadas](./media/security-center-network-recommendations/legacy-networking.png)

### <a name="internet-facing-endpoints-section"></a>Sección Internet facing endpoints
En la sección **Puntos de conexión accesibles desde Internet**, podrá ver las máquinas virtuales que están configuradas con un punto de conexión accesible desde Internet y su estado.

Esta tabla incluye el nombre del punto de conexión, la dirección IP accesible desde Internet, y el estado de gravedad actual del grupo de seguridad de red y las recomendaciones de NGFW. La tabla está ordenada por gravedad.

### <a name="networking-topology-section"></a>Sección Networking topology
En la sección **Topología de red**, se ofrece una vista jerárquica de los recursos.

Esta tabla está ordenada (máquinas virtuales y subredes) por gravedad.

En esta vista de topología, el primer nivel muestra las redes virtuales. El segundo nivel tiene subredes y el tercero muestra las máquinas virtuales que pertenecen a esas subredes. La columna derecha contiene el estado actual de las recomendaciones del grupo de seguridad de red para esos recursos.

El tercer nivel muestra máquinas virtuales, que es similar a lo que se ha descrito anteriormente. Puede hacer clic en un recurso para ver más información o aplicar la configuración o el control de seguridad necesario.

## <a name="network-recommendations"></a>Recomendaciones de red

|Tipo de recurso|Puntuación segura|Recomendación|DESCRIPCIÓN|
|----|----|----|----|
|Máquina|40|Habilitar grupos de seguridad de red en máquinas virtuales|Habilite grupos de seguridad de red para controlar el acceso de red de las máquinas virtuales.|
|Subred|35|Habilitar grupos de seguridad de red en subredes |Habilite grupos de seguridad de red para controlar el acceso de red de los recursos implementados en las subredes.|
|Máquina|30|Aplicar un control de acceso a redes Just-In-Time|Aplique el control de acceso de máquina virtual Just-In-Time para bloquear de forma permanente el acceso a puertos seleccionados y permitir que los usuarios autorizados los abran mediante el mismo mecanismo y durante una cantidad de tiempo limitada.|
|Máquina|20|Restricción del acceso a través de puntos de conexión accesibles desde Internet|Proteja los grupos de seguridad de red de las máquinas virtuales accesibles desde Internet mediante la restricción del acceso de las reglas de permiso existentes.|
|Máquina|10|Agregar un firewall de próxima generación|Agregue una solución de Firewall de próxima generación (NGFW) para proteger mejor sus máquinas virtuales accesibles desde Internet.|
|Máquina|5|Enrutar el tráfico solo a través del firewall de puerta de enlace de red|Para completar la implementación de su solución de firewall de próxima generación, se debe enrutar el tráfico a las máquinas virtuales accesibles desde Internet protegidas solo a través de la solución de firewall de próxima generación.|
|VNet|5|Habilitar la protección contra DDoS estándar|Las aplicaciones con direcciones IP públicas en estas redes virtuales no están protegidas con el servicio de protección contra DDOS estándar. Es recomendable habilitarlo para permitir la mitigación de los ataques volumétricos de red y protocolo.|
|Máquina|10|Agregar un firewall de próxima generación|Agregue una solución de firewall de próxima generación (NGFW) para proteger mejor las máquinas virtuales accesibles desde Internet.|
|Máquina|5|Enrutar el tráfico solo a través del firewall de puerta de enlace de red|Para completar la implementación de la solución de firewall de próxima generación, el tráfico hacia las máquinas virtuales accesibles desde Internet protegidas se debe enrutar solo a través de la solución de firewall de próxima generación.|
Vnet|5|Habilitar la protección contra DDoS estándar|Las aplicaciones con direcciones IP públicas en estas redes virtuales no están protegidas con el servicio de protección contra DDOS estándar. Es recomendable habilitarlo para permitir la mitigación de los ataques volumétricos de red y protocolo.|
## <a name="see-also"></a>Otras referencias
Para obtener más información sobre las recomendaciones que se aplican a otros tipos de recursos de Azure, consulte los siguientes artículos:

* [Protección de las máquinas virtuales en Azure Security Center](security-center-virtual-machine-recommendations.md)
* [Protección de las aplicaciones en Azure Security Center](security-center-application-recommendations.md)
* [Protección del servicio SQL de Azure en Azure Security Center](security-center-sql-service-recommendations.md)

Para más información sobre el Centro de seguridad, consulte los siguientes recursos:

* [Establecimiento de directivas de seguridad en Azure Security Center](tutorial-security-policy.md): aprenda a configurar directivas de seguridad para las suscripciones y los grupos de recursos de Azure.
* [Administración y respuesta a las alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md) : obtenga información sobre cómo administrar y responder a alertas de seguridad.
* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md) : encuentre las preguntas más frecuentes sobre el uso del servicio.
