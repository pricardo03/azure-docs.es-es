---
title: Requisitos del sistema de Microsoft Azure Data Box Gateway | Microsoft Docs
description: Obtenga más información sobre los requisitos de software y red de Azure Data Box Gateway
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 10/17/2018
ms.author: alkohli
ms.openlocfilehash: f70cd5de29cc3050142dfff905acae2008dab115
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49379129"
---
# <a name="azure-data-box-gateway-system-requirements-preview"></a>Requisitos del sistema de Azure Data Box Gateway

En este artículo se describen los requisitos del sistema importantes de la solución Microsoft Azure Data Box Gateway y de los clientes que se conectan a Data Box Gateway. Es aconsejable que revise cuidadosamente la siguiente información antes de implementar Data Box Gateway y que luego la consulte según sea necesario durante la implementación y el funcionamiento posterior.

Los requisitos del sistema del dispositivo virtual de Data Box Gateway incluyen:

- **Requisitos de software para hosts**: describe las plataformas compatibles, los exploradores de la interfaz de usuario de configuración local, los clientes SMB y los requisitos adicionales de los hosts que se conectan al dispositivo.
- **Requisitos de red para el dispositivo**: proporciona información acerca de los requisitos de red para el funcionamiento del dispositivo virtual.

> [!IMPORTANT]
> Data Box Gateway está en versión preliminar. Revise los [términos de uso para la versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) antes de implementar esta solución. 

## <a name="supported-os-for-clients-connected-to-device"></a>Sistema operativo compatible con los clientes conectados al dispositivo

Esta es una lista de los sistemas operativos compatibles con los clientes o hosts conectados a Data Box Gateway.

| **Sistema operativo o plataforma** | **Versiones** |
| --- | --- |
| Windows Server |2012 R2 <br> 2016 |
| Windows |8, 10 |
| SUSE Linux |Enterprise Server 12 (x86_64)|
| Ubuntu |16.04.3 LTS|
| CentOS | 7.0 |

## <a name="supported-protocols-for-clients-accessing-device"></a>Protocolos compatibles con los clientes que acceden al dispositivo

|**Protocolo** |**Versiones**   |**Notas**  |
|---------|---------|---------|
|SMB    | 2.X, 3.X      | SMB 1 no se admite.|
|NFS     | V3 y V4        |         |

## <a name="supported-virtualization-platforms-for-device"></a>Plataformas de virtualización compatibles con el dispositivo

| **Sistema operativo o plataforma**  |**Versiones**   |**Notas**  |
|---------|---------|---------|
|Hyper-V  |  2012 R2 <br> 2016  |         |
|VMware ESXi     | 6.0 <br> 6.5        |No se admiten las herramientas de VMware.         |


## <a name="supported-storage-accounts"></a>Cuentas de almacenamiento admitidas

Aquí se proporciona una lista de los tipos de almacenamiento compatibles para Data Box Gateway.

| **Cuenta de almacenamiento** | **Notas** |
| --- | --- |
| Clásico | Estándar |
| Uso general  |Estándar; se admiten V1 y V2. Se admiten niveles frecuentes y esporádicos. |


## <a name="supported-storage-types"></a>Tipos de almacenamiento admitidos

Aquí se proporciona una lista de los tipos de almacenamiento compatibles para Data Box Gateway.

| **Formato de archivo** | **Notas** |
| --- | --- |
| Blob en bloques de Azure | |
| Blob en páginas de Azure  | |
| Archivos de Azure | |

## <a name="supported-browsers-for-local-web-ui"></a>Exploradores compatibles con la interfaz de usuario web local

Presentamos una lista de los exploradores compatibles con la interfaz de usuario web local del dispositivo virtual.

|Browser  |Versiones  |Requisitos/notas adicionales  |
|---------|---------|---------|
|Google Chrome   |La versión más reciente         |         |
|Microsoft Edge    | La versión más reciente        |         |
|Internet Explorer     | La versión más reciente        |         |
|FireFox    |La versión más reciente         |         |


## <a name="networking-requirements"></a>Requisitos de red

La siguiente tabla enumera los puertos que deben abrirse en el firewall para permitir el tráfico de administración, de la nube o de SMB. En esta tabla, *dentro* o *entrante* hace referencia a la dirección desde la que el cliente entrante solicita acceso al dispositivo. *Fuera* o *saliente* hace referencia a la dirección en la que el dispositivo de Data Box Gateway envía datos externamente después de la implementación: por ejemplo, saliente a Internet.

| N.º de puerto| Dentro o fuera | Ámbito de puerto| Obligatorio|   Notas                                                             |                                                                                     |
|--------|---------|----------|--------------|----------------------|---------------|
| TCP 80 (HTTP)|Fuera|WAN |Sin |El puerto de salida se usa para obtener acceso a Internet para así recuperar las actualizaciones. <br>El usuario puede configurar el proxy web de salida. |                          
| TCP 443 (HTTPS)|Fuera|WAN|SÍ|El puerto de salida se usa para tener acceso a los datos en la nube.<br>El usuario puede configurar el proxy web de salida.|   
| UDP 53 (DNS)|Fuera|WAN|En algunos casos.<br>Vea las notas|Este puerto es necesario solo si está utilizando un servidor DNS basado en Internet.<br>Se recomienda usar un servidor DNS local. |
| UDP 123 (NTP)|Fuera|WAN|En algunos casos.<br>Vea las notas|Este puerto solo es necesario si está utilizando un servidor DNS basado en Internet.  |
| UDP 67 (DHCP)|Fuera|WAN|En algunos casos.<br>Vea las notas|Este puerto solo es necesario si se utiliza un servidor DHCP.  |
| TCP 80 (HTTP)|En|LAN|SÍ|Este es el puerto de entrada de la interfaz de usuario local del dispositivo para la administración local. <br>Si obtiene acceso a la interfaz de usuario local mediante HTTP, se le redireccionará automáticamente a HTTPS.  | 
| TCP 443 (HTTPS)|En|LAN|SÍ|Este es el puerto de entrada de la interfaz de usuario local del dispositivo para la administración local. | 

## <a name="url-patterns-for-firewall-rules"></a>Patrones de URL para reglas de firewall

Con frecuencia, los administradores de red pueden configurar reglas avanzadas de firewall de acuerdo con los patrones de URL para filtrar el tráfico saliente y entrante. El dispositivo de Data Box Gateway y el servicio Data Box Gateway dependen de otras aplicaciones de Microsoft, como Azure Service Bus, Azure Active Directory Access Control, las cuentas de almacenamiento y los servidores de Microsoft Update. Es posible usar los patrones de URL asociados a estas aplicaciones para configurar las reglas de firewall. Es importante entender que los patrones de URL asociados a estas aplicaciones pueden cambiar. Esta realidad, a su vez, requiere que el administrador de red supervise y actualice las reglas de firewall de Data Box Gateway de forma pertinente y oportuna.

Se recomienda que establezca las reglas de firewall para el tráfico saliente, basándose en las direcciones IP fijas de Data Box Gateway, de forma generosa en la mayoría de los casos. Sin embargo, puede utilizar la información siguiente con el objetivo de establecer las reglas avanzadas de firewall que se necesitan para crear entornos seguros.

> [!NOTE]
> - Las direcciones IP del dispositivo (origen) siempre se deben establecer en todas las interfaces de red habilitadas para la nube.
> - Las IP de destino, por su parte, se deben establecer en [Intervalos IP de centro de datos de Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653).

|     Patrón de URL                                                                                                                                                                                                                                                                                                                                                                                                                                       |     Componente o funcionalidad                                                                             |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
|    https://*.databoxedge.azure.com/*<br>https://*.servicebus.windows.net/*<br>https://login.windows.net                                                                                                                                                                                                                                                                                                        |    Servicio Azure Data Box Gateway<br>Azure Service Bus<br>Servicio de autenticación    |
|    http://*.backup.windowsazure.com                                                                                                                                                                                                                                                                                                                                                                                                                   |    Activación de dispositivo                                                                                    |
|    http://crl.microsoft.com/pki/*   http://www.microsoft.com/pki/*                                                                                                                                                                                                                                                                                                                                                                                    |    Revocación de certificados                                                                               |
|    https://*.core.windows.net/*   https://*.data.microsoft.com   http://*.msftncsi.com                                                                                                                                                                                                                                                                                                                                                                |    Supervisión y cuentas de Almacenamiento de Azure                                                                |
|    http://windowsupdate.microsoft.com<br>http://*.windowsupdate.microsoft.com<br>https://*.windowsupdate.microsoft.com<br>http://*.update.microsoft.com<br>https://*.update.microsoft.com<br>http://*.windowsupdate.com<br>http://download.microsoft.com<br>http://*.download.windowsupdate.com<br>http://wustat.windows.com<br>http://ntservicepack.microsoft.com<br>http://*.ws.microsoft.com<br>https://*.ws.microsoft.com<br>http://*.mp.microsoft.com        |    Servidores de Microsoft Update                                                                             |
|    http://*.deploy.akamaitechnologies.com                                                                                                                                                                                                                                                                                                                                                                                                             |    CDN de Akamai                                                                                           |
|    https://*.partners.extranet.microsoft.com/*                                                                                                                                                                                                                                                                                                                                                                                                        |    Paquete de soporte                                                                                      |
|    http://*.data.microsoft.com                                                                                                                                                                                                                                                                                                                                                                                                                        |    Para información acerca del servicio de telemetría en Windows, consulte la actualización para la experiencia del usuario y la telemetría de diagnóstico.      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                       |                                                                                                         |



## <a name="internet-bandwidth"></a>Ancho de banda de Internet

Los siguientes requisitos se aplican a ancho de banda mínimo de Internet disponible para los dispositivos de Data Box Gateway.

- El recurso de Data Box Gateway tiene un ancho de banda de Internet de 20 Mbps (o más) disponible en todo momento. Este ancho de banda no debe compartirse con otras aplicaciones. 
- Data Box Gateway tiene un ancho de banda dedicado de Internet de 32 Mbps (o más) cuando se usa el límite.

## <a name="next-step"></a>Paso siguiente

* [Implementación de Azure Data Box Gateway](data-box-gateway-deploy-prep.md)

