---
title: Preguntas más frecuentes (P+F) acerca de Azure Network Watcher | Microsoft Docs
description: Este artículo contiene respuestas a algunas preguntas frecuentes sobre el servicio Azure Network Watcher.
services: network-watcher
documentationcenter: na
author: damendo
manager: ''
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2019
ms.author: damendo
ms.openlocfilehash: 3305590f2d8abf0d894bc1df42b84edcc96a2b2d
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2019
ms.locfileid: "72598219"
---
# <a name="frequently-asked-questions-faq-about-azure-network-watcher"></a>Preguntas más frecuentes (P+F) sobre Azure Network Watcher
El servicio [Azure Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) proporciona un conjunto de herramientas para supervisar, diagnosticar, ver las métricas y habilitar o deshabilitar registros de recursos en una red virtual de Azure. En este artículo se responde a preguntas habituales sobre el servicio.

## <a name="general"></a>General

### <a name="what-is-network-watcher"></a>¿Qué es Network Watcher?
Network Watcher está diseñado para supervisar y reparar el estado de la red de los componentes de IaaS (infraestructura como servicio), que incluye máquinas virtuales, redes virtuales, puertas de enlace de aplicaciones, equilibradores de carga y otros recursos de una red virtual de Azure. No es una solución para supervisar la infraestructura de PaaS (plataforma como servicio) u obtener análisis web/móviles.

### <a name="what-tools-does-network-watcher-provide"></a>¿Qué herramientas proporciona Network Watcher?
Network Watcher proporciona tres conjuntos principales de funcionalidades
* Supervisión
  * [Vista de la topología](https://docs.microsoft.com/azure/network-watcher/view-network-topology) muestra los recursos de la red virtual y las relaciones entre ellos.
  * [Monitor de conexión](https://docs.microsoft.com/azure/network-watcher/connection-monitor) permite supervisar la conectividad y la latencia entre una máquina virtual y otro recurso de red.
  * [Network Performance Monitor](https://docs.microsoft.com/azure/azure-monitor/insights/network-performance-monitor) permite supervisar la conectividad y las latencias en las arquitecturas de red híbrida, los circuitos de Express Route y los puntos de conexión de servicio/aplicación.  
* Diagnóstico
  * [Comprobación del flujo de IP](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) permite detectar problemas de filtrado del tráfico a nivel de máquina virtual.
  * [Próximo salto](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview) ayuda a comprobar las rutas de tráfico y a detectar problemas de enrutamiento.
  * [Solución de problemas de conexión](https://docs.microsoft.com/azure/network-watcher/network-watcher-connectivity-portal) permite la supervisión puntual de la conectividad y la latencia entre una máquina virtual y otro recurso de red.
  * [Captura de paquetes](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview) permite capturar todo el tráfico de una máquina virtual de la red virtual.
  * [Solución de problemas de VPN](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-overview) ejecuta varias comprobaciones de diagnóstico en las puertas de enlace de VPN y las conexiones para ayudar a depurar problemas.
* Registro
  * [Registro de flujo de NSG ](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) permite registrar todo el tráfico de los [grupos de seguridad de red](https://docs.microsoft.com/azure/virtual-network/security-overview)
  * [Análisis de tráfico](https://docs.microsoft.com/azure/network-watcher/traffic-analytics) procesa los datos del registro de flujo del registro de flujo de NSG para que pueda visualizar, consultar, analizar y comprender el tráfico.


Para información más detallada, consulte la [página de información general de Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview).


### <a name="how-does-network-watcher-pricing-work"></a>¿Cómo son los precios de Network Watcher?
Visite la [página de precios](https://azure.microsoft.com/pricing/details/network-watcher/) para conocer los componentes de Network Watcher y sus precios.

### <a name="which-regions-is-network-watcher-available-in"></a>¿En qué regiones está Network Watcher disponible?
Puede ver la última disponibilidad regional en la [página de disponibilidad del servicio de Azure](https://azure.microsoft.com/global-infrastructure/services/?products=network-watcher)

### <a name="what-are-resource-limits-on-network-watcher"></a>¿Cuáles son los límites de recursos en Network Watcher?
Consulte la página de [límites de servicio](https://docs.microsoft.com/azure/azure-subscription-service-limits#network-watcher-limits) para conocer todos los límites.  

### <a name="why-is-only-one-instance-of-network-watcher-allowed-per-region"></a>¿Por qué solo se permite una instancia Network Watcher por región?
Network Watcher solo debe habilitarse una vez para que funcionen las características de la suscripción; no se trata de un límite de servicio.

## <a name="nsg-flow-logs"></a>Registro de flujo de NSG

### <a name="what-does-nsg-flow-logs-do"></a>¿Qué hace Registro de flujo de NSG?
Los recursos de red de Azure se pueden combinar y administrar mediante [grupos de seguridad de red](https://docs.microsoft.com/azure/virtual-network/security-overview). Registro de flujo de NSG permite registrar información de flujo de una tupla de 5 sobre todo el tráfico de los grupos de seguridad de red. Los registros de flujo sin procesar se escriben en una cuenta de Azure Storage desde donde se pueden procesar, analizar, consultar o exportar según sea necesario.

### <a name="are-there-any-caveats-to-using-nsg-flow-logs"></a>¿Existen advertencias para el uso de Registro de flujo de NSG?
No hay requisitos previos para el uso de Registro de flujo de NSG. Sin embargo, hay dos limitaciones
- **Los puntos de conexión de servicio no deben estar presentes en la red virtual** : Registro de flujo de NSG se emite desde los agentes de las máquinas virtuales a las cuentas de almacenamiento. Sin embargo, actualmente solo puede emitir registros directamente a las cuentas de almacenamiento y no puede usar un punto de conexión de servicio agregado a la red virtual.

- **La cuenta de almacenamiento no debe tener firewall**: Debido a las limitaciones internas, las cuentas de almacenamiento deben ser accesibles desde la red pública de Internet para que Registro de flujo de NSG funcione con ellas. El tráfico se seguirá enrutando por Azure internamente y no se verán cargos de salida adicionales.

Consulte las dos preguntas siguientes para obtener instrucciones sobre cómo solucionar estos problemas. Se espera que las dos limitaciones se solucionen a partir de enero de 2020.

### <a name="how-do-i-use-nsg-flow-logs-with-service-endpoints"></a>¿Cómo se usan los registros de flujo de NSG con puntos de conexión de servicio?

*Opción 1: Volver a configurar Registro de flujo de NSG para que emita a la cuenta de Azure Storage sin puntos de conexión de red virtual*

* Buscar subredes con puntos de conexión:

    - En Azure Portal, busque **Grupos de recursos** en la búsqueda global de la parte superior.
    - Vaya al grupo de recursos que contiene el NSG con el que está trabajando.
    - Use la segunda lista desplegable para filtrar por tipo y seleccione **Redes virtuales**
    - Haga clic en la red virtual que contiene los puntos de conexión de servicio.
    - Seleccione **Puntos de conexión de servicio** en **Configuración**, en el panel izquierdo.
    - Anote las subredes en las que **Microsoft.Storage** está habilitado.

* Deshabilite los puntos de conexión de servicio:

    - Siguiendo con lo anterior, seleccione **Subredes** en **Configuración**, en el panel izquierdo.
    * Haga clic en la subred que contiene los puntos de conexión de servicio.
    - En la sección **Puntos de conexión de servicio**, en **Servicios**, desactive **Microsoft.Storage**.

Puede comprobar los registros de almacenamiento después de unos minutos; verá una marca de tiempo actualizada o un nuevo archivo JSON.

*Opción 2: Deshabilitar Registro de flujo de NSG*

Si los puntos de conexión de servicio de Microsoft.Storage son imprescindibles, tendrá que deshabilitar los registros de flujo de NSG.

### <a name="how-do-i-disable-the--firewall-on-my-storage-account"></a>¿Cómo se deshabilita el firewall en la cuenta de almacenamiento?

Este problema se resuelve permitiendo que "Todas las redes" accedan a la cuenta de almacenamiento:

* Busque el nombre de la cuenta de almacenamiento, buscando el NSG en la [página de información general de los registros de flujo de NSG](https://ms.portal.azure.com/#blade/Microsoft_Azure_Network/NetworkWatcherMenuBlade/flowLogs)
* Para ir a la cuenta de almacenamiento, escriba el nombre de la cuenta de almacenamiento en la búsqueda global del portal.
* En la sección **CONFIGURACIÓN**, seleccione **Firewalls y redes virtuales**.
* Seleccione **Todas las redes** y guárdela. Si ya está seleccionada, no hay que hacer ningún cambio.  

### <a name="what-is-the-difference-between-flow-logs-versions-1--2"></a>¿Cuál es la diferencia entre la versión 1 y la versión 2 de los registros de flujo?
La versión 2 de los registros de flujo presenta el concepto de *estado del flujo* y almacena información sobre los bytes y los paquetes transmitidos. [Más información](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview#log-file).

## <a name="next-steps"></a>Pasos siguientes
 - Vaya a la [página de información general de documentación](https://docs.microsoft.com/azure/network-watcher/) para algunos tutoriales para empezar a trabajar con Network Watcher.
