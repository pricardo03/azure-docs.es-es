---
title: Supervisión de la seguridad en Azure Security Center | Microsoft Docs
description: Este artículo le ayuda a comenzar a trabajar con las funcionalidades de supervisión en Azure Security Center.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: mbaldwin
editor: ''
ms.assetid: 3bd5b122-1695-495f-ad9a-7c2a4cd1c808
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/26/2018
ms.author: terrylan
ms.openlocfilehash: 434b73d4625f86fab195dbda1fed9c841791f5b6
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37099466"
---
# <a name="security-health-monitoring-in-azure-security-center"></a>Supervisión del estado de seguridad en el Centro de seguridad de Azure
Este artículo le ayuda a usar las funcionalidades de supervisión de Azure Security Center para supervisar el cumplimiento de las directivas.

## <a name="what-is-security-health-monitoring"></a>¿Qué es la supervisión del estado de seguridad?
Con frecuencia se piensa que supervisar es observar y esperar que se produzca un evento, para así poder reaccionar ante la situación. La supervisión de seguridad se refiere a contar con una estrategia proactiva que audita los recursos a fin de identificar los sistemas que no cumplen con los estándares o los procedimientos recomendados de la organización.

## <a name="monitoring-security-health"></a>Supervisión del estado de seguridad
Después de habilitar las [directivas de seguridad](security-center-policies.md) para los recursos de una suscripción, Security Center analizará la seguridad de los recursos para identificar vulnerabilidades potenciales. La información acerca de la configuración de la red está disponible de inmediato. Dependiendo del número de máquinas virtuales y de los equipos que tenga con el agente instalado, la recopilación de información sobre máquinas virtuales y la configuración del equipo, como el estado de actualización de seguridad y la configuración del sistema operativo, puede tardar una hora o más en estar disponible. Puede ver el estado de seguridad de sus recursos, además de cualquier problema, en la sección **Prevención**. Una lista de dichos problemas también se puede encontrar en el icono **Recomendaciones**.

Para más información sobre cómo aplicar las recomendaciones, lea [Implementación de recomendaciones de seguridad en Azure Security Center](security-center-recommendations.md).

En **Supervisión del estado de los recursos**, podrá supervisar el estado de seguridad de los recursos. En el ejemplo siguiente, puede ver que, en el icono de cada recurso (proceso y aplicaciones, redes, almacenamiento, seguridad de datos e identidad y acceso) tiene el número total de problemas que se han identificado.

![Icono de estado de seguridad de los recursos](./media/security-center-monitoring/security-center-monitoring-fig1-newUI-2017.png)


### <a name="monitor-compute--apps"></a>Supervisión de proceso y aplicaciones
Consulte [Protección de las máquinas virtuales en Azure Security Center](security-center-virtual-machine-recommendations.md) para más información.

### <a name="monitor-virtual-networks"></a>Supervisión de redes virtuales
Al hacer clic en el icono **Redes**, se abre la hoja **Redes**, donde encontrará más información, como se muestra en la siguiente captura de pantalla:

![Hoja Redes](./media/security-center-monitoring/security-center-monitoring-fig9-new3.png)

#### <a name="networking-recommendations"></a>Recomendaciones de redes
De manera similar a la información del estado de los recursos de las máquinas virtuales, aquí puede ver una lista resumida de los problemas en la parte superior y una lista de las redes supervisadas en la parte inferior.

La sección de desglose del estado de las redes enumera los potenciales problemas de seguridad y ofrece [recomendaciones](security-center-network-recommendations.md). Entre los posibles problemas se pueden incluir:

* Firewall de próxima generación (NGFW) no instalado
* Grupos de seguridad de red en subredes no habilitados
* Grupos de seguridad de red en máquinas virtuales no habilitados
* Restringir el acceso externo por medio de puntos de conexión externos públicos
* Puntos de conexión con conexión a Internet correctos

Al hacer clic en una recomendación, puede ver más detalles sobre la recomendación, como se muestra en el ejemplo siguiente:

![Detalles de una recomendación en el servicio de red](./media/security-center-monitoring/security-center-monitoring-fig9-ga.png)

En este ejemplo, la hoja **Configurar los grupos de seguridad de red que faltan para las subredes** tiene una lista de subredes y máquinas virtuales que carecen de la protección de grupo de seguridad de red. Si hace clic en la subred a la que desea aplicar el grupo de seguridad de red, verá la opción **Elegir grupo de seguridad de red**. Aquí puede seleccionar el grupo de seguridad de red más adecuado para la subred o crear uno nuevo.

#### <a name="internet-facing-endpoints-section"></a>Sección Internet facing endpoints
En la sección **Puntos de conexión accesibles desde Internet**, podrá ver las máquinas virtuales que están configuradas con un punto de conexión accesible desde Internet y su estado actual.

![Máquinas virtuales configuradas con puntos de conexión accesibles desde Internet y su estado](./media/security-center-monitoring/security-center-monitoring-fig10-ga.png)

Esta tabla incluye el nombre del punto de conexión que representa la máquina virtual, la dirección IP accesible desde Internet, el estado de gravedad actual del grupo de seguridad de red y el NGFW. La tabla está ordenada por gravedad:

* Rojo (arriba): alta prioridad; se debe solucionar de inmediato.
* Naranja: prioridad media; se debe solucionar lo antes posible.
* Verde (al final): estado de mantenimiento.

#### <a name="networking-topology-section"></a>Sección Networking topology
En la sección **Topología de red**, se ofrece una vista jerárquica de los recursos, como se muestra en la siguiente captura de pantalla:

![Vista jerárquica de los recursos en la sección de topología de redes](./media/security-center-monitoring/security-center-monitoring-fig121-new4.png)

Esta tabla está ordenada (máquinas virtuales y subredes) por gravedad:

* Rojo (arriba): alta prioridad; se debe solucionar de inmediato.
* Naranja: prioridad media; se debe solucionar lo antes posible.
* Verde (al final): estado de mantenimiento.

En esta vista de topología, el primer nivel tiene [redes virtuales](../virtual-network/virtual-networks-overview.md), [puertas de enlace de red virtual](../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md) y [redes virtuales (clásicas)](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). El segundo nivel tiene subredes y el tercero, las máquinas virtuales que pertenecen a esas subredes. La columna derecha contiene el estado actual del grupo de seguridad de red para esos recursos, tal como se muestra en el ejemplo siguiente:

![Estado del grupo de seguridad de red en la sección de topología de redes](./media/security-center-monitoring/security-center-monitoring-fig12-ga.png)

En la parte inferior de esta hoja, aparecerán las recomendaciones para esta máquina virtual, parecidas a las que se han descrito antes. Puede hacer clic en una recomendación para ver más información o aplicar la configuración o el control de seguridad necesario.

### <a name="monitor-data-security"></a>Supervisión de seguridad de datos

Al hacer clic en **Seguridad de los datos** en la sección **Prevención**, se abre la hoja **Recursos de datos** con las recomendaciones para SQL y Storage. También tiene [recomendaciones](security-center-sql-service-recommendations.md) sobre el estado general de la base de datos. Para más información acerca del cifrado de almacenamiento, consulte [Enable encryption for Azure storage account in Azure Security Center](security-center-enable-encryption-for-storage-account.md) (Habilitación del cifrado para la cuenta de almacenamiento de Azure en Azure Security Center).

![Recursos de datos](./media/security-center-monitoring/security-center-monitoring-fig13-newUI-2017.png)

En **SQL Recommendations** (Recomendaciones de SQL) puede hacer clic en cualquier recomendación y obtener más detalles acerca de cómo realizar más acciones para resolver un problema. El ejemplo siguiente muestra la expansión de la recomendación **Auditoría de la base de datos y detección de amenazas en bases de datos de SQL**.

![Detalles acerca de una recomendación de SQL](./media/security-center-monitoring/security-center-monitoring-fig14-ga-new.png)

La hoja **Habilitar la auditoría y la detección de amenazas en bases de datos de SQL** contiene la siguiente información:

* Una lista de bases de datos SQL.
* El servidor en el que se encuentran.
* Información sobre si esta configuración se heredó del servidor o si es única en la base de datos.
* El estado actual.
* La gravedad del problema.

Al hacer clic en la base de datos para abordar esta recomendación, se abre la hoja **Auditoría y detección de amenazas**, como se muestra en la siguiente pantalla.

![Auditoría y detección de amenazas](./media/security-center-monitoring/security-center-monitoring-fig15-ga.png)

Para habilitar la auditoría, seleccione **Activar** en la opción **Auditoría**.

### <a name="monitor-identity--access"></a>Supervisión de la identidad y acceso

Consulte [Supervisión de identidad y acceso en Azure Security Center](security-center-identity-access.md) para más información.

## <a name="see-also"></a>Otras referencias
En este artículo, aprendió a usar las funcionalidades de supervisión en Azure Security Center. Para obtener más información sobre el Centro de seguridad de Azure, consulte los siguientes recursos:

* [Establecimiento de directivas de seguridad en Azure Security Center](security-center-policies.md): obtenga información sobre cómo configurar los ajustes de seguridad en Azure Security Center.
* [Administración y respuesta a las alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md): obtenga información sobre cómo administrar y responder a alertas de seguridad.
* [Supervisión de las soluciones de asociados con Azure Security Center](security-center-partner-solutions.md): aprenda a supervisar el estado de mantenimiento de las soluciones de asociados.
* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md): encuentre las preguntas más frecuentes sobre el uso del servicio.
* [Blog de seguridad de Azure](http://blogs.msdn.com/b/azuresecurity/): encuentre entradas de blog sobre el cumplimiento y la seguridad de Azure.
