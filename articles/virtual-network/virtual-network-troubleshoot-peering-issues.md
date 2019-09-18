---
title: Solución de problemas de emparejamiento de redes virtuales
description: Pasos para ayudar a resolver la mayoría de los problemas de emparejamiento de redes virtuales.
services: virtual-network
documentationcenter: na
author: v-miegge
manager: dcscontentpm
editor: ''
ms.assetid: 1a3d1e84-f793-41b4-aa04-774a7e8f7719
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/28/2019
ms.author: kaushika
ms.openlocfilehash: fc01a20a077a1c624ed490600db919fe3197556c
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70901396"
---
# <a name="troubleshoot-virtual-network-peering-issues"></a>Solución de problemas de emparejamiento de redes virtuales

En esta guía de solución de problemas se proporcionan los pasos para ayudarle a resolver la mayoría de los problemas de [emparejamiento de redes virtuales](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).

![IMAGEN](./media/virtual-network-troubleshoot-peering-issues/4489538_en_1.png)

## <a name="scenario-1-configure-virtual-network-peering-between-two-virtual-networks"></a>Escenario 1: Configuración de emparejamiento de redes virtuales entre dos redes virtuales

¿Las redes virtuales pueden estar en la misma suscripción o en suscripciones distintas?

### <a name="connection-type-1-the-virtual-networks-are-in-the-same-subscription"></a>Tipo de conexión 1: Las redes virtuales deben estar en la misma suscripción

Para configurar el emparejamiento de redes virtuales para las redes virtuales que están en la misma suscripción, use los métodos que se proporcionan en los artículos siguientes, según corresponda:

* Si las redes virtuales están en la **misma región**, siga los pasos para [crear un emparejamiento para redes virtuales en la misma suscripción](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering#create-a-peering).
* Si las redes virtuales se encuentran en **distintas regiones**, siga los pasos para configurar el [emparejamiento de redes virtuales globales](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).  

> [!Note]
> La conectividad no funcionará a través del emparejamiento de VNET global para los siguientes recursos: 
>
> * VM tras la SKU de ILB básica
> * Redis Cache (usa la SKU de ILB básica)
> * Application Gateway (usa la SKU de ILB básica)
> * Scale Sets (usa la SKU de ILB básica)
> * Clústeres de Service Fabric (usa la SKU de ILB básica)
> * SQL Always On (usa la SKU de ILB básica)
> * App Service Environment (ASE) (usa la SKU de ILB básica)
> * API Management (usa la SKU de ILB básica)
> * Azure Active Directory Domain Services (ADDS) (usa la SKU de ILB básica)

Para más información, consulte los [requisitos y restricciones](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#requirements-and-constraints) del emparejamiento global.

### <a name="connection-type-2-the-virtual-networks-are-in-different-subscriptions-or-ad-tenants"></a>Tipo de conexión 2: Las redes virtuales se encuentran en suscripciones o inquilinos de AD diferentes.

Para configurar el emparejamiento de redes virtuales para redes virtuales en diferentes suscripciones o inquilinos de Active Directory, siga los pasos descritos en [Creación de emparejamiento: CLI de Azure](https://docs.microsoft.com/azure/virtual-network/create-peering-different-subscriptions#cli).

> [!Note]
> Para configurar el emparejamiento de red, debe tener permisos de **colaborador de red** en ambas suscripciones. Para obtener más información, consulte [Permisos de emparejamiento](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering%23permissions).

## <a name="scenario-2-configure-virtual-network-peering-with-hub-spoke-topology-that-uses-on-premises-resources"></a>Escenario 2: Configuración del emparejamiento de redes virtuales con una topología de red en estrella tipo hub-and-spoke que usa recursos locales

![IMAGEN](./media/virtual-network-troubleshoot-peering-issues/4488712_en_1a.png)

### <a name="connection-type-1-for-site-to-site-connection-or-expressroute-connection"></a>Tipo de conexión 1: Para una conexión de sitio a sitio o ExpressRoute

Siga los pasos descritos en: [Configuración del tránsito de la puerta de enlace de VPN para el emparejamiento de red virtual](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-peering-gateway-transit?toc=/azure/virtual-network/toc.json).

### <a name="connection-type-2-for-point-to-site-connections"></a>Tipo de conexión 2: Para conexiones de punto a sitio

1. Siga los pasos descritos en: [Configuración del tránsito de la puerta de enlace de VPN para el emparejamiento de red virtual](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-peering-gateway-transit?toc=/azure/virtual-network/toc.json).
2. Después de establecer o cambiar el emparejamiento de red virtual, el paquete de punto a sitio se debe descargar e instalar de nuevo para que los clientes de punto a sitio obtengan las rutas actualizadas en la red virtual radial.

## <a name="scenario-3-configure-virtual-network-peering-with-hub-spoke-topology-for-azure-virtual-network"></a>Escenario 3: Configuración del emparejamiento de redes virtuales con una topología de red en estrella tipo hub-and-spoke para Azure Virtual Network

![IMAGEN](./media/virtual-network-troubleshoot-peering-issues/4488712_en_1b.png)

### <a name="connection-type-1-the-virtual-networks-are-in-the-same-region"></a>Tipo de conexión 1: Las redes virtuales están en la misma región

Debe configurar un dispositivo de red virtual (NVA) en la red virtual de tipo hub-and-spoke y tener rutas definidas por el usuario con la "aplicación de red virtual" de próximo salto aplicadas en las redes virtuales de tipo hub-and-spoke. Para más información, consulte [Encadenamiento de servicios](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#service-chaining).

> [!Note]
> Si necesita ayuda para configurar un NVA, [póngase en contacto con el proveedor de NVA](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines).

Para obtener ayuda con la solución de problemas de configuración y enrutamiento del dispositivo NVA, consulte [Problemas de aplicaciones virtuales de red en Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-troubleshoot-nva).

### <a name="connection-type-2-the-virtual-networks-are-in-different-regions"></a>Tipo de conexión 2: Las redes virtuales están en regiones diferentes

Ya se admite el emparejamiento de VNET global. La conectividad no funciona a través del emparejamiento de VNET global para los siguientes recursos:

* VM tras la SKU de ILB básica
* Redis Cache (usa la SKU de ILB básica)
* Application Gateway (usa la SKU de ILB básica)
* Scale Sets (usa la SKU de ILB básica)
* Clústeres de Service Fabric (usa la SKU de ILB básica)
* SQL Always On (usa la SKU de ILB básica)
* App Service Environment (ASE) (usa la SKU de ILB básica)
* API Management (usa la SKU de ILB básica)
* Azure Active Directory Domain Services (ADDS) (usa la SKU de ILB básica)

Para más información acerca de los requisitos y restricciones del emparejamiento global, consulte [Emparejamiento de redes virtuales](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#requirements-and-constraints).

## <a name="scenario-4-i-have-a-connectivity-issue-between-two-peered-virtual-networks"></a>Escenario 4: Tengo un problema de conectividad entre dos redes virtuales emparejadas

Inicie sesión en [Azure Portal](https://portal.azure.com/) con una cuenta que disponga de los [roles y permisos](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering#roles-permissions) necesarios. Seleccione la red virtual, seleccione **Emparejamiento** y, a continuación, compruebe el campo **Estado**. ¿Cuál es el estado?

### <a name="connection-type-1-the-peering-status-shows-connected"></a>Tipo de conexión 1: El estado de emparejamiento muestra "Conectado"

Para solucionar el problema, siga estos pasos:

1. Compruebe los flujos de tráfico de red:

   Use [Solución de problemas de conexiones](https://docs.microsoft.com/azure/network-watcher/network-watcher-connectivity-overview) y la [Comprobación de flujo de IP](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) de la VM de origen a la VM de destino para determinar si hay un grupo de seguridad de red (NSG) o una ruta definida por el usuario (UDR) que cause interferencias en los flujos de tráfico.

   Si usa un firewall o un dispositivo NVA, siga estos pasos: 
   1. Documente los parámetros de UDR para poder restaurarlos una vez completado este paso.
   2. Quite el UDR de la subred de la VM de origen o el NIC que apunta a NVA como el próximo salto. Compruebe la conectividad de la VM de origen directamente con el destino que está omitiendo el NVA. Si este paso funciona, consulte el [solucionador de problemas de NVA](https://docs.microsoft.com/azure/virtual-network/virtual-network-troubleshoot-nva).

2. Tome un seguimiento de red: 
   1. Inicie un seguimiento de red en la VM de destino. En Windows, puede usar **Netsh**. Para Linux, use **TCPDump**.
   2. Ejecute **TcpPing** o **PsPing** de la IP de origen a la de destino.

   * Este es un ejemplo de un comando **TcpPing**: `tcping64.exe -t <destination VM address> 3389`.

   3. Una vez se haya completado **TcpPing**, detenga el seguimiento de red en el destino.
   4. Si los paquetes llegan desde el origen, no hay ningún problema de red. Examine el firewall de la VM y la aplicación que escucha en ese puerto para localizar el problema de configuración.

   > [!Note]
   > No se puede conectar a los siguientes tipos de recursos a través del emparejamiento de redes virtuales globales (redes virtuales en regiones diferentes):
   >
   > * VM tras la SKU de ILB básica
   > * Redis Cache (usa la SKU de ILB básica)
   > * Application Gateway (usa la SKU de ILB básica)
   > * Scale Sets (usa la SKU de ILB básica)
   > * Clústeres de Service Fabric (usa la SKU de ILB básica)
   > * SQL Always On (usa la SKU de ILB básica)
   > * App Service Environment (ASE) (usa la SKU de ILB básica)
   > * API Management (usa la SKU de ILB básica)
   > * Azure Active Directory Domain Services (ADDS) (usa la SKU de ILB básica)

Para más información, consulte los [requisitos y restricciones](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#requirements-and-constraints) del emparejamiento global.

### <a name="connection-type-2-the-peering-status-shows-disconnected"></a>Tipo de conexión 2: El estado de emparejamiento muestra "Desconectado"

Debe eliminar emparejamientos de ambas redes virtuales y volver a crearlos.

## <a name="scenario-5-i-have-a-connectivity-issue-between-a-hub-spoke-virtual-network-and-on-premises-resource"></a>Escenario 5: Tengo un problema de conectividad entre una red virtual de tipo hub-and-spoke y un recurso local

¿Usa una instancia de VPN Gateway o un NVA de terceros?

### <a name="connection-type-1-my-network-uses-a-third-party-nva-or-vpn-gateway"></a>Tipo de conexión 1: Mi red usa una instancia de VPN Gateway o un NVA de terceros

Para solucionar problemas de conectividad que afectan a una instancia de VPN Gateway o un NVA de terceros, consulte los siguientes artículos:

* [Solucionador de problemas de NVA](https://docs.microsoft.com/azure/virtual-network/virtual-network-troubleshoot-nva)
* [Encadenamiento de servicios](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#service-chaining)

### <a name="connection-type-2-my-network-does-not-a-third-party-nva-or-vpn-gateway"></a>Tipo de conexión 2: Mi red no usa una instancia de VPN Gateway ni un NVA de terceros

¿Las redes virtuales de tipo hub-and-spoke tienen una instancia de VPN Gateway?

#### <a name="both-the-hub-and-spoke-virtual-networks-have-a-vpn-gateway"></a>Las redes virtuales de tipo hub-and-spoke tienen una instancia de VPN Gateway

No se admite el uso de una puerta de enlace remota.

Debido a una limitación de emparejamiento de VNET, **Usar puerta de enlace remota** no se admite en la red virtual de tipo hub-and-spoke si dicha red virtual tiene una instancia de VPN Gateway.

#### <a name="both-the-hub-and-spoke-virtual-networks-dont-have-a-vpn-gateway"></a>Las redes virtuales de tipo hub-and-spoke no tienen una instancia de VPN Gateway

En el caso de las conexiones de sitio a sitio o ExpressRoute, compruebe estas causas principales de los problemas de conectividad con la red virtual remota desde el entorno local.

* Compruebe que la casilla **Permitir tráfico reenviado** se ha seleccionado en la red virtual que tiene una puerta de enlace.
* Compruebe que la casilla **Usar puerta de enlace remota** se ha seleccionado en la red virtual que no tiene una puerta de enlace.
* Haga que el administrador de red compruebe los dispositivos locales para comprobar que todos tienen el espacio de direcciones de red virtual remoto agregado.

Para conexiones de punto a sitio:

* Compruebe que la casilla **Permitir tráfico reenviado** se ha seleccionado en la red virtual que tiene una puerta de enlace.
* Compruebe que la casilla **Usar puerta de enlace remota** se ha seleccionado en la red virtual que no tiene una puerta de enlace.
* Descargue e instale de nuevo el paquete de cliente de punto a sitio. Las rutas de red virtual recién emparejadas no agregan automáticamente rutas a los clientes de punto a sitio.

## <a name="scenario-6-i-have-a-hub-spoke-network-connectivity-issue-between-spoke-virtual-networks-in-the-same-region"></a>Escenario 6: Tengo un problema de conectividad de red de tipo hub-and-spoke entre redes virtuales de la misma región

Debe tener un NVA en una red de tipo hub-and-spoke, configurar UDR en hub-and-spoke que tengan un NVA establecido como próximo salto y habilitar **Permitir tráfico reenviado** en la red virtual de tipo hub-and-spoke.

Para obtener más información, consulte [Encadenamiento de servicios](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#service-chaining) y analice estos requisitos con el [proveedor de NVA](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines) de su elección.

## <a name="scenario-7-i-have-a-hub-spoke-network-connectivity-issue-between-spoke-virtual-networks-in-different-regions"></a>Escenario 7: Tengo un problema de conectividad de red de tipo hub-and-spoke entre redes virtuales de diferentes regiones

Ya se admite el emparejamiento de VNET global. La conectividad no funcionará a través del emparejamiento de VNET global para los siguientes recursos:

* VM tras la SKU de ILB básica
* Redis Cache (usa la SKU de ILB básica)
* Application Gateway (usa la SKU de ILB básica)
* Scale Sets (usa la SKU de ILB básica)
* Clústeres de Service Fabric (usa la SKU de ILB básica)
* SQL Always On (usa la SKU de ILB básica)
* App Service Environment (ASE) (usa la SKU de ILB básica)
* API Management (usa la SKU de ILB básica)
* Azure Active Directory Domain Services (ADDS) (usa la SKU de ILB básica)

Para más información, consulte los [requisitos y restricciones](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#requirements-and-constraints) del emparejamiento global y las [diferentes topologías de VPN](https://blogs.msdn.microsoft.com/igorpag/2016/02/11/hubspoke-daisy-chain-and-full-mesh-vnet-topologies-in-azure-arm-v2/).

## <a name="scenario-8-i-have-a-hub-spoke-network-connectivity-issue-between-a-web-app-and-the-spoke-virtual-network"></a>Escenario 8: Tengo un problema de conectividad de red de tipo hub-and-spoke entre una aplicación web y la red virtual de hub-and-spoke

Para solucionar este problema, siga estos pasos:

1. Inicie sesión en Azure Portal. Vaya a la aplicación web, seleccione **Redes** y, a continuación, seleccione **Integración con red virtual**.
2. Compruebe si puede ver la red virtual remota. Escriba manualmente el espacio de direcciones de la red virtual remota (**Sincronizar red** y **Agregar rutas**).

Para más información, consulte los siguientes artículos.

* [Integración de una aplicación con Azure Virtual Network](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet)
* [Información sobre el enrutamiento de VPN de punto a sitio](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-point-to-site-routing)

## <a name="scenario-9-i-receive-an-error-when-configuring-virtual-network-peering"></a>Escenario 9: Recibo un error al configurar el emparejamiento de red virtual

### <a name="error-1-current-tenant-tenant-id-isnt-authorized-to-access-linked-subscription"></a>Error 1: El inquilino `<TENANT ID>` actual no está autorizado para acceder a la suscripción vinculada

Para resolver este problema, siga los pasos descritos en [Creación de emparejamiento: CLI de Azure](https://docs.microsoft.com/azure/virtual-network/create-peering-different-subscriptions#cli).

### <a name="error-2-not-connected"></a>Error 2: No conectado

Debe eliminar emparejamientos de ambas redes virtuales y volver a crearlos.

### <a name="error-3-failed-to-peer-a-databricks-virtual-network"></a>Error 3: No se pudo emparejar una red virtual de Databricks

Para resolver este problema, configure el emparejamiento de red virtual desde la hoja **Azure Databricks** y, a continuación, especifique la red virtual de destino mediante el **Id. de recurso**. Para obtener más información, consulte [Peer a Databricks virtual network to a remote virtual network](https://docs.azuredatabricks.net/administration-guide/cloud-configurations/azure/vnet-peering.html#id2) (Emparejamiento de una red virtual de Databricks con una red virtual remota).

## <a name="next-steps"></a>Pasos siguientes

* [Solución de problemas de conectividad entre máquinas virtuales de Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-troubleshoot-connectivity-problem-between-vms)