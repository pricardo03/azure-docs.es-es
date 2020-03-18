---
title: Implementación de Azure Data Explorer en la red virtual
description: Obtenga información sobre cómo implementar Azure Data Explorer en la red virtual
author: basaba
ms.author: basaba
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 10/31/2019
ms.openlocfilehash: 5a2731e26ba4f371177cf2ae649f0695f27e6304
ms.sourcegitcommit: be53e74cd24bbabfd34597d0dcb5b31d5e7659de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2020
ms.locfileid: "79096756"
---
# <a name="deploy-azure-data-explorer-into-your-virtual-network"></a>Implementación de Azure Data Explorer en la red virtual

En este artículo se explican los recursos que están presentes cuando se implementa un clúster de Azure Data Explorer en una red virtual de Azure personalizada. Esta información le ayudará a implementar un clúster en una subred de la red virtual (VNet). Para más información sobre las redes virtuales de Azure, vea [¿Qué es Azure Virtual Network?](/azure/virtual-network/virtual-networks-overview)

   ![Diagrama de red virtual](media/vnet-deployment/vnet-diagram.png)

Azure Data Explorer admite la implementación de un clúster en una subred de la red virtual (VNet). Esta funcionalidad le permite:

* Aplicar reglas de [grupo de seguridad de red](/azure/virtual-network/security-overview) (NSG) al tráfico del clúster de Azure Data Explorer.
* Conectar la red local a la subred del clúster de Azure Data Explorer.
* Proteger los orígenes de conexión de datos ([Event Hub](/azure/event-hubs/event-hubs-about) y [Event Grid](/azure/event-grid/overview)) con [puntos de conexión de servicio](/azure/virtual-network/virtual-network-service-endpoints-overview).

## <a name="access-your-azure-data-explorer-cluster-in-your-vnet"></a>Acceso al clúster de Azure Data Explorer en la red virtual

Puede acceder al clúster de Azure Data Explorer mediante las siguientes direcciones IP para cada servicio (servicios de administración de datos y motor):

* **Dirección IP privada**: se usa para acceder al clúster dentro de la red virtual.
* **Dirección IP pública**: se usa para acceder al clúster desde fuera de la red virtual para la administración y supervisión, y como una dirección de origen para las conexiones salientes iniciadas desde el clúster.

Se crean los siguientes registros DNS para acceder al servicio: 

* `[clustername].[geo-region].kusto.windows.net` (motor) `ingest-[clustername].[geo-region].kusto.windows.net` (administración de datos) se asignan a la dirección IP pública de cada servicio. 

* `private-[clustername].[geo-region].kusto.windows.net` (motor) `private-ingest-[clustername].[geo-region].kusto.windows.net` (administración de datos) se asignan a la dirección IP privada de cada servicio.

## <a name="plan-subnet-size-in-your-vnet"></a>Planeamiento del tamaño de la subred en la red virtual

El tamaño de la subred que se usa para hospedar un clúster de Azure Data Explorer no se puede modificar una vez implementada la subred. En la red virtual, Azure Data Explorer usa una dirección IP privada para cada máquina virtual y dos direcciones IP privadas para los equilibradores de carga internos (motor y administración de datos). Las redes de Azure también usan cinco direcciones IP para cada subred. Azure Data Explorer aprovisiona dos máquinas virtuales para el servicio de administración de datos. Las máquinas virtuales del servicio de motor se aprovisionan por capacidad de escala de configuración de usuario.

El número total de direcciones IP:

| Uso | Número de direcciones |
| --- | --- |
| Servicio de motor | 1 por instancia |
| Servicio de administración de datos | 2 |
| Equilibradores de carga internos | 2 |
| Direcciones reservadas de Azure | 5 |
| **Total** | **#instancias_de_motor + 9** |

> [!IMPORTANT]
> El tamaño de la subred se debe planear con antelación, ya que no se puede cambiar una vez implementado Azure Data Explorer. Por tanto, reserve el tamaño de subred necesario en consecuencia.

## <a name="service-endpoints-for-connecting-to-azure-data-explorer"></a>Puntos de conexión de servicio para conectarse a Azure Data Explorer

[Los puntos de conexión de servicio de Azure](/azure/virtual-network/virtual-network-service-endpoints-overview) permiten proteger los recursos multiinquilino de Azure en la red virtual.
La implementación del clúster de Azure Data Explorer en la subred permite configurar conexiones de datos con [Event Hub](/azure/event-hubs/event-hubs-about) o [Event Grid](/azure/event-grid/overview) mientras se restringen los recursos subyacentes de la subred de Azure Data Explorer.

> [!NOTE]
> Al usar la configuración de EventGrid con [Storage](/azure/storage/common/storage-introduction) y Event Hub, la cuenta de almacenamiento que se usa en la suscripción puede bloquearse con puntos de conexión de servicio a la subred de Azure Data Explorer, a la vez que permite los servicios de la plataforma de Azure de confianza en la [configuración de firewall](/azure/storage/common/storage-network-security). Sin embargo, Event Hub no puede habilitar el punto de conexión de servicio porque no admite [servicios de plataforma de Azure](/azure/event-hubs/event-hubs-service-endpoints) de confianza.

## <a name="dependencies-for-vnet-deployment"></a>Dependencias para la implementación de redes virtuales

### <a name="network-security-groups-configuration"></a>Configuración de grupos de seguridad de red

Los [grupos de seguridad de red (NSG)](/azure/virtual-network/security-overview) proporcionan la capacidad de controlar el acceso a la red dentro de una red virtual. Se puede acceder a Azure Data Explorer mediante dos puntos de conexión: HTTPs (443) y TDS (1433). Se deben configurar las reglas de NSG siguientes para permitir el acceso a estos puntos de conexión para la administración, la supervisión y el funcionamiento correcto del clúster.

#### <a name="inbound-nsg-configuration"></a>Configuración de NSG de entrada

| **Uso**   | **From**   | **To**   | **Protocolo**   |
| --- | --- | --- | --- |
| Administración  |[Direcciones de administración de ADX](#azure-data-explorer-management-ip-addresses)/AzureDataExplorerManagement(ServiceTag) | Subred de ADX: 443  | TCP  |
| Supervisión del estado  | [Direcciones de supervisión de estado de ADX](#health-monitoring-addresses)  | Subred de ADX: 443  | TCP  |
| Comunicación interna ADX  | Subred de ADX: Todos los puertos  | Subred de ADX: todos los puertos  | All  |
| Permitir entrada de Azure Load Balancer (sondeo de estado)  | AzureLoadBalancer  | Subred de ADX: 80,443  | TCP  |

#### <a name="outbound-nsg-configuration"></a>Configuración de NSG de salida

| **Uso**   | **From**   | **To**   | **Protocolo**   |
| --- | --- | --- | --- |
| Dependencia de Azure Storage  | Subred de ADX  | Almacenamiento: 443  | TCP  |
| Dependencia de Azure Data Lake  | Subred de ADX  | AzureDataLake: 443  | TCP  |
| Ingesta y supervisión de servicios de EventHub  | Subred de ADX  | EventHub: 443,5671  | TCP  |
| Publicación de métricas  | Subred de ADX  | AzureMonitor: 443 | TCP  |
| Descarga de la configuración de Azure Monitor  | Subred de ADX  | [Direcciones de punto de conexión de configuración de Azure Monitor](#azure-monitor-configuration-endpoint-addresses): 443 | TCP  |
| Active Directory (si es aplicable) | Subred de ADX | AzureActiveDirectory: 443 | TCP |
| Entidad de certificación | Subred de ADX | Internet: 80 | TCP |
| Comunicación interna  | Subred de ADX  | Subred de ADX: todos los puertos  | All  |
| Puertos que se usan para los complementos `sql\_request` y `http\_request`  | Subred de ADX  | Internet: personalizado  | TCP  |

### <a name="relevant-ip-addresses"></a>Direcciones IP pertinentes

#### <a name="azure-data-explorer-management-ip-addresses"></a>Direcciones IP de administración de Azure Data Explorer

| Region | Direcciones |
| --- | --- |
| Centro de Australia | 20.37.26.134 |
| Centro de Australia 2 | 20.39.99.177 |
| Este de Australia | 40.82.217.84 |
| Sudeste de Australia | 20.40.161.39 |
| BrazilSouth | 191.233.25.183 |
| Centro de Canadá | 40.82.188.208 |
| Este de Canadá | 40.80.255.12 |
| Centro de la India | 40.81.249.251 |
| Centro de EE. UU. | 40.67.188.68 |
| EUAP del centro de EE. UU. | 40.89.56.69 |
| Este de Asia | 20.189.74.103 |
| Este de EE. UU. | 52.224.146.56 |
| Este de EE. UU. 2 | 52.232.230.201 |
| EUAP de Este de EE. UU. 2 | 52.253.226.110 |
| Centro de Francia | 40.66.57.91 |
| Sur de Francia | 40.82.236.24 |
| Japón Oriental | 20.43.89.90 |
| Japón Occidental | 40.81.184.86 |
| Centro de Corea del Sur | 40.82.156.149 |
| Corea del Sur | 40.80.234.9 |
| Centro-Norte de EE. UU | 40.81.45.254 |
| Norte de Europa | 52.142.91.221 |
| Norte de Sudáfrica | 102.133.129.138 |
| Oeste de Sudáfrica | 102.133.0.97 |
| Centro-sur de EE. UU. | 20.45.3.60 |
| Sudeste de Asia | 40.119.203.252 |
| Sur de la India | 40.81.72.110 |
| Sur de Reino Unido 2 | 40.81.154.254 |
| Oeste de Reino Unido | 40.81.122.39 |
| Centro-Oeste de EE. UU. | 52.159.55.120 |
| Oeste de Europa | 51.145.176.215 |
| Oeste de la India | 40.81.88.112 |
| Oeste de EE. UU. | 13.64.38.225 |
| Oeste de EE. UU. 2 | 40.90.219.23 |

#### <a name="health-monitoring-addresses"></a>Direcciones de supervisión de estado

| Region | Direcciones |
| --- | --- |
| Centro de Australia | 191.239.64.128 |
| Centro de Australia 2 | 191.239.64.128 |
| Este de Australia | 191.239.64.128 |
| Sudeste de Australia | 191.239.160.47 |
| Sur de Brasil | 23.98.145.105 |
| Centro de Canadá | 168.61.212.201 |
| Este de Canadá | 168.61.212.201 |
| Centro de la India | 23.99.5.162 |
| Centro de EE. UU. | 168.61.212.201 |
| EUAP del centro de EE. UU. | 168.61.212.201 |
| Este de Asia | 168.63.212.33 |
| Este de EE. UU. | 137.116.81.189 |
| Este de EE. UU. 2 | 137.116.81.189 |
| EUAP de Este de EE. UU. 2 | 137.116.81.189 |
| Centro de Francia | 23.97.212.5 |
| Sur de Francia | 23.97.212.5 |
| Japón Oriental | 138.91.19.129 |
| Japón Occidental | 138.91.19.129 |
| Centro de Corea del Sur | 138.91.19.129 |
| Corea del Sur | 138.91.19.129 |
| Centro-Norte de EE. UU | 23.96.212.108 |
| Norte de Europa | 191.235.212.69 
| Norte de Sudáfrica | 104.211.224.189 |
| Oeste de Sudáfrica | 104.211.224.189 |
| Centro-sur de EE. UU. | 23.98.145.105 |
| Sur de la India | 23.99.5.162 |
| Sudeste de Asia | 168.63.173.234 |
| Sur de Reino Unido 2 | 23.97.212.5 |
| Oeste de Reino Unido | 23.97.212.5 |
| Centro-Oeste de EE. UU. | 168.61.212.201 |
| Oeste de Europa | 23.97.212.5 |
| Oeste de la India | 23.99.5.162 |
| Oeste de EE. UU. | 23.99.5.162 |
| Oeste de EE. UU. 2 | 23.99.5.162 |    

#### <a name="azure-monitor-configuration-endpoint-addresses"></a>Direcciones de punto de conexión de configuración de Azure Monitor

| Region | Direcciones |
| --- | --- |
| Centro de Australia | 52.148.86.165 |
| Centro de Australia 2 | 52.148.86.165 |
| Este de Australia | 52.148.86.165 |
| Sudeste de Australia | 52.148.86.165 |
| Sur de Brasil | 13.68.89.19 |
| Centro de Canadá | 13.90.43.231 |
| Este de Canadá | 13.90.43.231 |
| India Central | 13.71.25.187 |
| Centro de EE. UU. | 52.173.95.68 |
| EUAP de centro de EE. UU. | 13.90.43.231 |
| Asia Oriental | 13.75.117.221 |
| Este de EE. UU. | 13.90.43.231 |
| Este de EE. UU. 2 | 13.68.89.19 |    
| EUAP de Este de EE. UU. 2 | 13.68.89.19 |
| Centro de Francia | 52.174.4.112 |
| Sur de Francia | 52.174.4.112 |
| Este de Japón | 13.75.117.221 |
| Oeste de Japón | 13.75.117.221 |
| Centro de Corea del Sur | 13.75.117.221 |
| Sur de Corea del Sur | 13.75.117.221 |
| Centro-norte de EE. UU. | 52.162.240.236 |
| Norte de Europa | 52.169.237.246 |
| Norte de Sudáfrica | 13.71.25.187 |
| Oeste de Sudáfrica | 13.71.25.187 |
| Centro-sur de EE. UU. | 13.84.173.99 |
| India del Sur | 13.71.25.187 |
| Sudeste Asiático | 52.148.86.165 |
| Sur de Reino Unido | 52.174.4.112 |
| Oeste de Reino Unido | 52.169.237.246 |
| Centro-Oeste de EE. UU. | 52.161.31.69 |
| Oeste de Europa | 52.174.4.112 |
| Oeste de la India | 13.71.25.187 |
| Oeste de EE. UU. | 40.78.70.148 |
| Oeste de EE. UU. 2 | 52.151.20.103 |

## <a name="expressroute-setup"></a>Configuración de ExpressRoute

Use ExpressRoute para conectar la red local a Azure Virtual Network. Una configuración común consiste en anunciar la ruta predeterminada (0.0.0.0/0) a través de la sesión de Protocolo de puerta de enlace de borde (BGP). Esto obliga a que el tráfico que sale de la red virtual se reenvíe a la red local del cliente que puede quitar el tráfico, lo que interrumpirá los flujos de salida. Para superar este comportamiento predeterminado, se puede configurar [Ruta definida por el usuario (UDR)](/azure/virtual-network/virtual-networks-udr-overview#user-defined) (0.0.0.0/0) y el próximo salto será *Internet*. Como UDR tiene prioridad sobre BGP, el tráfico se destinará a Internet.

## <a name="securing-outbound-traffic-with-firewall"></a>Protección del tráfico de salida con el firewall

Si quiere proteger el tráfico de salida mediante [Azure Firewall](/azure/firewall/overview) o cualquier aplicación virtual para limitar los nombres de dominio, se deben permitir los siguientes nombres de dominio completos (FQDN) en el firewall.

```
prod.warmpath.msftcloudes.com:443
production.diagnostics.monitoring.core.windows.net:443
graph.windows.net:443
*.update.microsoft.com:443
shavamanifestcdnprod1.azureedge.net:443
login.live.com:443
wdcp.microsoft.com:443
login.microsoftonline.com:443
azureprofilerfrontdoor.cloudapp.net:443
*.core.windows.net:443
*.servicebus.windows.net:443
shoebox2.metrics.nsatc.net:443
prod-dsts.dsts.core.windows.net:443
ocsp.msocsp.com:80
*.windowsupdate.com:80
ocsp.digicert.com:80
go.microsoft.com:80
dmd.metaservices.microsoft.com:80
www.msftconnecttest.com:80
crl.microsoft.com:80
www.microsoft.com:80
adl.windows.com:80
crl3.digicert.com:80
```

También debe definir la [tabla de enrutamiento](/azure/virtual-network/virtual-networks-udr-overview) en la subred con las [direcciones de administración](#azure-data-explorer-management-ip-addresses) y las [direcciones de supervisión de estado](#health-monitoring-addresses) con el próximo salto *Internet* para evitar problemas de rutas asimétricas.

Por ejemplo, para la región **Oeste de EE. UU.** , se deben definir los UDR siguientes:

| Nombre | Prefijo de dirección | Próximo salto |
| --- | --- | --- |
| ADX_Management | 13.64.38.225/32 | Internet |
| ADX_Monitoring | 23.99.5.162/32 | Internet |

## <a name="deploy-azure-data-explorer-cluster-into-your-vnet-using-an-azure-resource-manager-template"></a>Implementación de un clúster de Azure Data Explorer en la red virtual mediante una plantilla de Azure Resource Manager

Para implementar el clúster de Azure Data Explorer en la red virtual, use la plantilla de Azure Resource Manager [Implementación del clúster de Azure Data Explorer en la red virtual](https://azure.microsoft.com/resources/templates/101-kusto-vnet/).

Esta plantilla crea el clúster, la red virtual, la subred, el grupo de seguridad de red y las direcciones IP públicas.

## <a name="troubleshooting"></a>Solución de problemas

En esta sección aprenderá a solucionar problemas de conectividad, de operación y de creación de clústeres para un clúster que se implementa en la [red virtual](/azure/virtual-network/virtual-networks-overview).

### <a name="access-issues"></a>Problemas de acceso

Si tiene un problema al acceder al clúster mediante el punto de conexión público (cluster.region.kusto.windows.net) o privado (private-cluster.region.kusto.windows.net) y sospecha que está relacionado con la configuración de la red virtual, lleve a cabo los pasos siguientes para solucionar el problema.

#### <a name="check-tcp-connectivity"></a>Comprobación de la conectividad TCP

El primer paso incluye la comprobación de la conectividad TCP mediante el sistema operativo Windows o Linux.

# <a name="windows"></a>[Windows](#tab/windows)

   1. Descargue [TCping](https://www.elifulkerson.com/projects/tcping.php) en la máquina que se conecta al clúster.
   2. Haga ping al destino desde la máquina de origen con el siguiente comando:

    ```cmd
     C:\> tcping -t yourcluster.kusto.windows.net 443 
    
     ** Pinging continuously.  Press control-c to stop **
    
     Probing 1.2.3.4:443/tcp - Port is open - time=100.00ms
     ```

# <a name="linux"></a>[Linux](#tab/linux)

   1. Instale *netcat* en la máquina que se conecta al clúster.

    ```bash
    $ apt-get install netcat
     ```

   2. Haga ping al destino desde la máquina de origen con el siguiente comando:

     ```bash
     $ netcat -z -v yourcluster.kusto.windows.net 443
    
     Connection to yourcluster.kusto.windows.net 443 port [tcp/https] succeeded!
     ```
---

Si la prueba no finaliza correctamente, continúe con los pasos siguientes. Si la prueba es correcta, el problema no se debe a la conectividad de TCP. Vaya a [Problemas operativos](#cluster-creation-and-operations-issues) para continuar con la solución del problema.

#### <a name="check-the-network-security-group-nsg"></a>Comprobación del grupo de seguridad de red (NSG)

   Compruebe que el [grupo de seguridad de red](/azure/virtual-network/security-overview) (NSG) conectado a la subred del clúster tiene una regla de entrada que permite el acceso desde la dirección IP del equipo cliente al puerto 443.

#### <a name="check-route-table"></a>Comprobación de la tabla de rutas

   Si la subred del clúster tiene la configuración de tunelización forzada en el firewall (subred con una [tabla de rutas](/azure/virtual-network/virtual-networks-udr-overview) que contiene la ruta predeterminada "0.0.0.0/0"), asegúrese de que la dirección IP de la máquina tenga una ruta con el [tipo de próximo salto](/azure/virtual-network/virtual-networks-udr-overview) establecido en VirtualNetwork/Internet. Esto se necesita para evitar problemas de rutas asimétricas.

### <a name="ingestion-issues"></a>Problemas de ingesta

Si está experimentando problemas de ingesta y sospecha que están relacionados con la configuración de la red virtual, lleve a cabo los pasos siguientes.

#### <a name="check-ingestion-health"></a>Comprobación del estado de la ingesta

    Check that the [cluster ingestion metrics](/azure/data-explorer/using-metrics#ingestion-health-and-performance-metrics) indicate a healthy state.

#### <a name="check-security-rules-on-data-source-resources"></a>Comprobación de las reglas de seguridad en los recursos de origen de datos

Si las métricas indican que no se procesó ningún evento desde el origen de datos (métrica de *eventos procesados* —para instancias de Event Hub o IoT Hub—), asegúrese de que los recursos del origen de datos (Event Hub o Storage) permiten el acceso desde la subred del clúster en las reglas de firewall o los puntos de conexión de servicio.

#### <a name="check-security-rules-configured-on-clusters-subnet"></a>Comprobación de las reglas de seguridad configuradas en la subred del clúster

Asegúrese de que la subred del clúster tiene las reglas de NSG, UDR y firewall configuradas correctamente. Además, pruebe la conectividad de red para todos los puntos de conexión dependientes. 

### <a name="cluster-creation-and-operations-issues"></a>Problemas de creación y operación de clústeres

Si experimenta problemas con la creación o la operación de un clúster y sospecha que está relacionado con la configuración de la red virtual, siga estos pasos para solucionar el problema.

#### <a name="diagnose-the-virtual-network-with-the-rest-api"></a>Diagnóstico de la red virtual con la API REST

Se usa [ARMClient](https://chocolatey.org/packages/ARMClient) para llamar a la API REST con PowerShell. 

1. Inicio de sesión con ARMClient

   ```powerShell
   armclient login
   ```

1. Invoque la operación de diagnóstico.

    ```powershell
    $subscriptionId = '<subscription id>'
    $clusterName = '<name of cluster>'
    $resourceGroupName = '<resource group name>'
    $apiversion = '2019-11-09'
    
    armclient post "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Kusto/clusters/$clusterName/diagnoseVirtualNetwork?api-version=$apiversion" -verbose
    ```

1. Compruebe la respuesta.

    ```powershell
    HTTP/1.1 202 Accepted
    ...
    Azure-AsyncOperation: https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Kusto/locations/{location}/operationResults/{operation-id}?api-version=2019-11-09
    ...
    ```

1. Espere a que finalice la operación.

    ```powershell
    armclient get https://management.azure.com/subscriptions/$subscriptionId/providers/Microsoft.Kusto/locations/{location}/operationResults/{operation-id}?api-version=2019-11-09
    
    {
      "id": "/subscriptions/{subscription-id}/providers/Microsoft.Kusto/locations/{location}/operationresults/{operation-id}",
      "name": "{operation-name}",
      "status": "[Running/Failed/Completed]",
      "startTime": "{start-time}",
      "endTime": "{end-time}",
      "properties": {...}
    }
    ```
    
   Espere hasta que la propiedad *status* muestre *Completed*; después, el campo *properties* debe mostrar:

    ```powershell
    {
      "id": "/subscriptions/{subscription-id}/providers/Microsoft.Kusto/locations/{location}/operationresults/{operation-id}",
      "name": "{operation-name}",
      "status": "Completed",
      "startTime": "{start-time}",
      "endTime": "{end-time}",
      "properties": {
        "Findings": [...]
      }
    }
    ```

Si la propiedad *Findings* muestra un resultado vacío, significa que se han superado todas las pruebas de red y no se ha interrumpido la conexión. Si muestra un error similar al siguiente: *Outbound dependency '{dependencyName}:{port}' might be not satisfied (Outbound)* (Puede que no se haya satisfecho la dependencia de salida '{nombreDeDependencia}:{puerto}' [salida]), el clúster no puede acceder a los puntos de conexión de servicio dependientes. Continúe con los pasos siguientes para solucionar el problema.

#### <a name="check-network-security-group-nsg"></a>Comprobación del grupo de seguridad de red (NSG)

Asegúrese de que el [grupo de seguridad de red](/azure/virtual-network/security-overview) esté configurado correctamente según las instrucciones de [Dependencias para la implementación de redes virtuales](/azure/data-explorer/vnet-deployment#dependencies-for-vnet-deployment).

#### <a name="check-route-table"></a>Comprobación de la tabla de rutas

Si la subred del clúster tiene configurada la tunelización forzada en el firewall (subred con una [tabla de rutas](/azure/virtual-network/virtual-networks-udr-overview) que contiene la ruta predeterminada "0.0.0.0/0"), asegúrese de que las [direcciones IP de administración](#azure-data-explorer-management-ip-addresses) y las [direcciones IP de supervisión de estado](#health-monitoring-addresses) tienen una ruta con el [tipo de próximo salto](/azure/virtual-network/virtual-networks-udr-overview##next-hop-types-across-azure-tools) establecido en *Internet* y un [prefijo de dirección de origen](/azure/virtual-network/virtual-networks-udr-overview#how-azure-selects-a-route) en *"management-ip/32"* y *"health-monitoring-ip/32"* . Esto se necesita para evitar problemas de rutas asimétricas.

#### <a name="check-firewall-rules"></a>Comprobación de las reglas de firewall

Si fuerza la tunelización del tráfico de salida de la subred a un firewall, asegúrese de que en la configuración del firewall se permiten todas las dependencias FQDN (por ejemplo, *.blob.core.windows.net*), tal como se describe en [Protección del tráfico de salida con el firewall](/azure/data-explorer/vnet-deployment#securing-outbound-traffic-with-firewall).
