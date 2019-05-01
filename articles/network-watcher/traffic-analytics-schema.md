---
title: Esquema de análisis de tráfico de Azure | Microsoft Docs
description: Comprender el esquema de análisis de tráfico para analizar registros de flujo de grupo de seguridad de red de Azure.
services: network-watcher
documentationcenter: na
author: vinynigam
manager: agummadi
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/26/2019
ms.author: vinigam
ms.openlocfilehash: 491f19abfd87c28ede45e98a24f31fe7e599b18b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64691412"
---
# <a name="schema-and-data-aggregation-in-traffic-analytics"></a>Agregación de esquema y los datos de análisis de tráfico

Análisis de tráfico es una solución basada en la nube, que proporciona visibilidad de la actividad de usuarios y aplicaciones en las redes en la nube. Análisis de tráfico permite analizar los registros de flujo del grupo de seguridad de Network Watcher para proporcionar información sobre el flujo de tráfico en la nube de Azure. Con Análisis de tráfico, puede:

- Visualizar la actividad de la red en las suscripciones de Azure e identificar las zonas activas.
- Identificar las amenazas de seguridad y proteger la red, con información sobre puertos abiertos, aplicaciones que intentan acceder a Internet y máquinas virtuales (VM) que se conectan a redes no autorizadas.
- Entender los patrones de flujo de tráfico entre regiones de Azure e Internet para optimizar el rendimiento y la capacidad de su implementación de red.
- Identificar errores de configuración de red que dan lugar a errores de conexión.
- Conocer el uso de red en bytes, paquetes o flujos.

### <a name="data-aggregation"></a>Agregación de datos

1. Todos los registros de flujo en un NSG entre "FlowIntervalStartTime_t" y "FlowIntervalEndTime_t" se capturan en un minuto en la cuenta de almacenamiento como blobs antes de ser procesados por el análisis de tráfico. 
2. Intervalo de procesamiento de análisis de tráfico predeterminado es 60 minutos. Esto significa que cada 60 minutos, que el análisis de tráfico elige blobs de almacenamiento para la agregación.
3. Protocolo (TCP o UDP) de capa de flujos que tengan la misma dirección IP de origen, IP de destino, puerto de destino, nombre NSG, regla de NSG, dirección del flujo y transporte (tenga en cuenta: Se excluye el puerto de origen para la agregación) se clubbed en un único flujo mediante el análisis de tráfico
4. Este registro solo es representativos (más detalles en la sección siguiente) y registro ingerido en análisis de tráfico Analytics.This proceso pueden tardar hasta una hora máxima.
5. Campo FlowStartTime_t indica la primera aparición de este tipo un flujo agregado (mismo cuatro-tupla) en el intervalo entre "FlowIntervalStartTime_t" y "FlowIntervalEndTime_t" del procesamiento de registro de flujo. 
6. Para cualquier recurso en TA, los flujos que se indica en la interfaz de usuario son flujos totales que se procesó el NSG, pero en Log Anlaytics usuario verá solo el registro único, reduciendo. Para ver todos los flujos, use el campo blob_id, que se puede hacer referencia desde el almacenamiento. El flujo total número total de registros que coincidan los flujos individuales que ve en el blob.


### <a name="fields-used-in-traffic-analytics-schema"></a>Campos utilizados en el esquema de análisis de tráfico

Análisis de tráfico se basa en Log Analytics, para que pueda ejecutar consultas personalizadas en los datos decorados con análisis de tráfico y establecer alertas en el mismo.

A continuación aparecen los campos en el esquema y lo significan

| Campo | Formato | Comentarios | 
|:---   |:---    |:---  |
| TableName | AzureNetworkAnalytics_CL | Tabla de datos de tráfico Anlaytics
| SubType_s | FlowLog | Subtipo para los registros de flujo |
| FASchemaVersion_s |   1   | Versión del esquema. No refleja la versión del registro de flujo de NSG |
| TimeProcessed_t   | Fecha y hora en UTC  | Hora a la que el análisis de tráfico procesan los registros de flujo sin formato de la cuenta de almacenamiento |
| FlowIntervalStartTime_t | Fecha y hora en UTC |  Hora de inicio del intervalo de procesamiento de registros de flujo. Se trata de tiempo desde el que se mide el intervalo de flujo |
| FlowIntervalEndTime_t | Fecha y hora en UTC | Hora final del intervalo de procesamiento de registros de flujo |
| FlowStartTime_t | Fecha y hora en UTC |  Primera aparición del flujo (que obtener agregará) en el intervalo de procesamiento de registro de flujo entre "FlowIntervalStartTime_t" y "FlowIntervalEndTime_t". Este flujo obtiene agregado en función de la lógica de agregación |
| FlowEndTime_t | Fecha y hora en UTC | Última aparición del flujo (que obtener agregará) en el intervalo de procesamiento de registro de flujo entre "FlowIntervalStartTime_t" y "FlowIntervalEndTime_t". En cuanto a la versión 2 del registro de flujo, este campo contiene la hora (se marca como "B" en el registro de flujo sin formato) se inició el último flujo con el mismo tupla de cuatro elementos |
| FlowType_s |  * IntraVNet <br> * InterVNet <br> * S2S <br> * P2S <br> * AzurePublic <br> * ExternalPublic <br> * MaliciousFlow <br> * Desconocido privada <br> * Desconocido | Definición de notas de la tabla siguiente |
| SrcIP_s | Dirección IP de origen | Estará en blanco en el caso de AzurePublic y ExternalPublic flujos |
| DestIP_s | Dirección IP de destino | Estará en blanco en el caso de AzurePublic y ExternalPublic flujos |
| VMIP_s | Dirección IP de la máquina virtual | Utilizado para los flujos AzurePublic y ExternalPublic |
| PublicIP_s | Direcciones IP públicas | Utilizado para los flujos AzurePublic y ExternalPublic |
| DestPort_d | Puerto de destino | Puerto en el que el tráfico es entrante | 
| L4Protocol_s  | * T <br> * U  | Protocolo de transporte. T = TCP <br> U = UDP | 
| L7Protocol_s  | Nombre del protocolo | Deriva de puerto de destino |
| FlowDirection_s | * Me = entrada<br> * O = saliente | Dirección del flujo de NSG según el registro de flujo | 
| FlowStatus_s  | * A = permitido por la regla de NSG <br> * D. = denegado por la regla de NSG  | Estado de flujo permite/nblocked por NSG según el registro de flujo |
| NSGList_s | \<SUBSCRIPTIONID>\/<RESOURCEGROUP_NAME>\/<NSG_NAME> | Grupo de seguridad de red (NSG) asociado con el flujo |
| NSGRules_s | \<Índice de valor 0) >< NSG_RULENAME >\<dirección del flujo >\<flujo estado >\<FlowCount ProcessedByRule > |  Regla de NSG que permite o deniegan este flujo |
| NSGRuleType_s | * Definidas por el usuario * predeterminado |   El tipo de regla de NSG utilizado por el flujo |
| MACAddress_s | Dirección MAC | Dirección MAC de la NIC a la que se capturó el flujo |
| Subscription_s | Suscripción de Azure virtual network / interfaz de red / máquina virtual se rellena en este campo | Aplicable sólo a FlowType = tipos de flujo de S2S, P2S, AzurePublic, ExternalPublic, MaliciousFlow y UnknownPrivate (tipos de flujo donde sólo un lado es azure) |
| Subscription1_s | Id. de suscripción | Id. de suscripción de red virtual / interfaz de red / máquina virtual a la que pertenece la dirección IP de origen en el flujo |
| Subscription2_s | Id. de suscripción | Id. de suscripción de red virtual / interfaz de red / máquina virtual a la que pertenece la dirección IP de destino en el flujo |
| Region_s | Región de Azure de la red virtual / interfaz de red / máquina virtual a la que pertenece la dirección IP en el flujo | Aplicable sólo a FlowType = tipos de flujo de S2S, P2S, AzurePublic, ExternalPublic, MaliciousFlow y UnknownPrivate (tipos de flujo donde sólo un lado es azure) |
| Region1_s | Región de Azure | Región de Azure de la red virtual / interfaz de red / máquina virtual a la que pertenece la dirección IP de origen en el flujo |
| Region2_s | Región de Azure | Región de Azure de la red virtual a la que pertenece la dirección IP de destino en el flujo |
| NIC_s | \<resourcegroup_Name>\/\<NetworkInterfaceName> |  NIC asociada con la máquina virtual enviar o recibir el tráfico |
| NIC1_s | <resourcegroup_Name>/\<NetworkInterfaceName> | NIC asociada con la dirección IP de origen en el flujo |
| NIC2_s | <resourcegroup_Name>/\<NetworkInterfaceName> | NIC asociada con la dirección IP de destino en el flujo |
| VM_s | <resourcegroup_Name>\/\<NetworkInterfaceName> | Máquina virtual asociada con la interfaz de red NIC_s |
| VM1_s | <resourcegroup_Name>/\<VirtualMachineName> | Máquina virtual asociada con la dirección IP de origen en el flujo |
| VM2_s | <resourcegroup_Name>/\<VirtualMachineName> | Máquina virtual asociada con la dirección IP de destino en el flujo |
| Subnet_s | <ResourceGroup_Name>/<VNET_Name>/\<SubnetName> | Subred asociada con el NIC_s |
| Subnet1_s | <ResourceGroup_Name>/<VNET_Name>/\<SubnetName> | Subred asociada con la dirección IP de origen en el flujo |
| Subnet2_s | <ResourceGroup_Name>/<VNET_Name>/\<SubnetName>    | Subred asociada con la dirección IP de destino en el flujo |
| ApplicationGateway1_s | \<SubscriptionID>/\<ResourceGroupName>/\<ApplicationGatewayName> | Puerta de enlace de aplicaciones asociado con la dirección IP de origen en el flujo | 
| ApplicationGateway2_s | \<SubscriptionID>/\<ResourceGroupName>/\<ApplicationGatewayName> | Puerta de enlace de aplicaciones asociado con la dirección IP de destino en el flujo |
| LoadBalancer1_s | \<SubscriptionID>/\<ResourceGroupName>/\<LoadBalancerName> | Equilibrador de carga asociado con la dirección IP de origen en el flujo |
| LoadBalancer2_s | \<SubscriptionID>/\<ResourceGroupName>/\<LoadBalancerName> | Equilibrador de carga asociado con la dirección IP de destino en el flujo |
| LocalNetworkGateway1_s | \<SubscriptionID>/\<ResourceGroupName>/\<LocalNetworkGatewayName> | Puerta de enlace de red local asociado con la dirección IP de origen en el flujo |
| LocalNetworkGateway2_s | \<SubscriptionID>/\<ResourceGroupName>/\<LocalNetworkGatewayName> | Puerta de enlace de red local asociado con la dirección IP de destino en el flujo |
| ConnectionType_s | Los valores posibles son VNetPeering, VpnGateway y ExpressRoute |    Tipo de conexión |
| ConnectionName_s | \<SubscriptionID>/\<ResourceGroupName>/\<ConnectionName> | Nombre de la conexión |
| ConnectingVNets_s | Lista separada por espacios de nombres de red virtual | Topología de concentrador y radio, en el caso de las redes virtuales de concentrador se rellenará aquí |
| Country_s | Código de país (ISO 3166-1 alpha-2) de dos letras | Se rellena para el tipo de flujo ExternalPublic. Todas las direcciones IP en el campo PublicIPs_s compartirán el mismo código de país |
| AzureRegion_s | Ubicaciones de la región de Azure | Se rellena para el tipo de flujo AzurePublic. Todas las direcciones IP en el campo PublicIPs_s compartirán la región de Azure |
| AllowedInFlows_d | | Recuento de flujos de entrada que se permitían. Representa el número de flujos que comparten la misma tupla de cuatro a la interfaz netweork en el que se capturó el flujo de entrada | 
| DeniedInFlows_d |  | Recuento de flujos de entrada que se han denegado. (Para la interfaz de red a la que se capturó el flujo de entrada) |
| AllowedOutFlows_d | | Recuento de flujos de salida que se permitían (salida a la interfaz de red a la que se capturó el flujo) |
| DeniedOutFlows_d  | | Recuento de flujos de salida que se denegaron (salida a la interfaz de red a la que se capturó el flujo) |
| FlowCount_d | En desuso. Flujos totales que coinciden con el mismo tupla de cuatro elementos. En el caso de tipos de flujo ExternalPublic y AzurePublic, recuento incluirá los flujos de varias direcciones de PublicIP también.
| InboundPackets_d | Paquetes recibidos capturarse en la interfaz de red donde se aplica la regla de NSG | Esto solo se rellena para el esquema de registro de flujo de versión 2 de NSG |
| OutboundPackets_d  | Paquetes enviados al capturarse en la interfaz de red donde se aplica la regla de NSG | Esto solo se rellena para el esquema de registro de flujo de versión 2 de NSG |
| InboundBytes_d |  Bytes recibidos capturarse en la interfaz de red donde se aplica la regla de NSG | Esto solo se rellena para el esquema de registro de flujo de versión 2 de NSG |
| OutboundBytes_d | Bytes enviados al capturarse en la interfaz de red donde se aplica la regla de NSG | Esto solo se rellena para el esquema de registro de flujo de versión 2 de NSG |
| CompletedFlows_d  |  | Esto se rellenará con valor distinto de cero solo para el esquema de registro de flujo de versión 2 de NSG |
| PublicIPs_s | <PUBLIC_IP>\|\<FLOW_STARTED_COUNT>\|\<FLOW_ENDED_COUNT>\|\<OUTBOUND_PACKETS>\|\<INBOUND_PACKETS>\|\<OUTBOUND_BYTES>\|\<INBOUND_BYTES> | Las entradas separados por barras |
    
### <a name="notes"></a>Notas
    
1. En el caso de los flujos AzurePublic y ExternalPublic, el cliente posee la que IP de la máquina virtual de Azure se rellena en el campo VMIP_s, mientras que las direcciones IP públicas que se rellenan en el campo PublicIPs_s. Para estos tipos de flujo de dos, debemos usar VMIP_s y PublicIPs_s en lugar de campos SrcIP_s y DestIP_s. Direcciones de AzurePublic y ExternalPublicIP, hemos agregado aún más, para que el número de registros ingeridos en el área de trabajo de análisis de registros de cliente es mínimo. (Este campo dejará de utilizarse pronto y deberíamos utilizar SrcIP_ y DestIP_s dependiendo de si la máquina virtual de azure era el origen o el destino del flujo de) 
1. Detalles de los tipos de flujo: En función de las direcciones IP implicadas en el flujo, se clasifican los flujos de los siguientes tipos de flujo de: 
1. IntraVNet: ambas direcciones IP en el flujo residen en la misma red Virtual de Azure. 
1. Entre redes virtuales - direcciones IP en el flujo residen en las diferentes dos redes virtuales de Azure. 
1. S2S: uno (sitio a sitio) de las direcciones IP pertenece a la red Virtual de Azure mientras la otra dirección IP pertenece a la red del cliente (sitio) conectado a la red Virtual de Azure a través de la puerta de enlace de VPN o Express Route. 
1. P2S - (punto a sitio) una de las direcciones IP pertenece a Azure Virtual Network, mientras que la otra dirección IP pertenece a la red de cliente (sitio) conectado a la red Virtual de Azure a través de la puerta de enlace de VPN.
1. AzurePublic - una de las direcciones IP pertenece a la red Virtual de Azure mientras la otra dirección IP pertenece a direcciones IP públicas de Azure interna que pertenecen a Microsoft. Clientes que pertenecen las direcciones IP públicas no formará parte de este tipo de flujo. Por ejemplo, cualquier cliente propiedad VM enviar tráfico a un servicio de Azure (punto de conexión de almacenamiento) se clasificarían en este tipo de flujo. 
1. ExternalPublic - una de las direcciones IP pertenece a la red Virtual de Azure mientras la otra dirección IP es una dirección IP pública que no está en Azure, no se notifica como malintencionado en las fuentes ASC que consume el análisis de tráfico para el intervalo de procesamiento entre " FlowIntervalStartTime_t"y"FlowIntervalEndTime_t". 
1. MaliciousFlow - una de las direcciones IP pertenecen a la red virtual de azure mientras la otra dirección IP es una dirección IP pública que no está en Azure y se notifica como malintencionado en las fuentes ASC que consume el análisis de tráfico para el intervalo de procesamiento entre" FlowIntervalStartTime_t"y"FlowIntervalEndTime_t". 
1. UnknownPrivate - una de las direcciones IP pertenecen a la red Virtual de Azure mientras la otra dirección IP pertenece al intervalo IP privadas, como se define en RFC 1918 y no se puede asignar mediante el análisis de tráfico a un cliente a la propiedad de sitio o Azure Virtual Network.
1. Desconocido: no se puede asignar cualquiera de la dirección IP direcciones en los flujos con la topología de cliente en Azure así como de forma local (sitio).
1. Algunos nombres de campo se anexan con _s o sufijo _d. Estos no significan origen y destino.

### <a name="next-steps"></a>Pasos siguientes
Para obtener respuestas a las preguntas más frecuentes, consulte [preguntas más frecuentes sobre análisis de tráfico](traffic-analytics-faq.md) para ver detalles acerca de la funcionalidad, consulte [documentación de análisis de tráfico](traffic-analytics.md)
    


    


