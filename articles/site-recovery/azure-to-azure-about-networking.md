---
title: Acerca de las redes en la recuperación ante desastres de máquinas virtuales de Azure con Azure Site Recovery
description: Proporciona información general de las redes para la replicación de máquinas virtuales de Azure mediante Azure Site Recovery.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 1/8/2020
ms.author: sutalasi
ms.openlocfilehash: 9fe3b4c0b7acc9c1e980d5885043d30503c211c4
ms.sourcegitcommit: 380e3c893dfeed631b4d8f5983c02f978f3188bf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2020
ms.locfileid: "75754501"
---
# <a name="about-networking-in-azure-vm-disaster-recovery"></a>Acerca de las redes para la recuperación ante desastres de máquinas virtuales de Azure



En este artículo se proporciona la guía de redes cuando se van a replicar y recuperar máquinas virtuales de Azure de una región a otra, mediante [Azure Site Recovery](site-recovery-overview.md).

## <a name="before-you-start"></a>Antes de comenzar

Obtenga información sobre cómo Site Recovery proporciona recuperación ante desastres para [este escenario](azure-to-azure-architecture.md).

## <a name="typical-network-infrastructure"></a>Infraestructura de red típica

El siguiente diagrama representa el entorno de Azure típico para aplicaciones que se ejecutan en máquinas virtuales de Azure:

![Entorno de cliente](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

Si usa Azure ExpressRoute o una conexión VPN desde su red local a Azure, el entorno tendrá este aspecto:

![Entorno de cliente](./media/site-recovery-azure-to-azure-architecture/source-environment-expressroute.png)

Normalmente, los clientes protegen sus redes mediante firewalls y grupos de seguridad de red (NSG). Los firewalls usan la inclusión en lista blanca basada en direcciones URL o IP para controlar la conectividad de red. Los NSG proporcionan reglas que usan intervalos de direcciones IP para controlar la conectividad de red.

>[!IMPORTANT]
> Site Recovery no admite el uso de un proxy autenticado para controlar la conectividad de red y la replicación no se podrá habilitar.


## <a name="outbound-connectivity-for-urls"></a>Conectividad de salida para las direcciones URL

Si usa un proxy de firewall basado en la dirección URL para controlar la conectividad de salida, admita estas direcciones URL de Site Recovery:


**URL** | **Detalles**  
--- | ---
*.blob.core.windows.net | Se requiere para que los datos se puedan escribir en la cuenta de almacenamiento de la caché en la región de origen de la máquina virtual. Si conoce todas las cuentas de almacenamiento en caché de las máquinas virtuales, puede permitir el acceso a las direcciones URL de las cuentas de almacenamiento específicas (por ejemplo, cache1.blob.core.windows.net y cache2.blob.core.windows.net) en lugar de *.blob.core.windows.net
login.microsoftonline.com | Se requiere para la autorización y la autenticación de las direcciones URL del servicio Site Recovery.
*.hypervrecoverymanager.windowsazure.com | Se requiere para la comunicación del servicio Site Recovery desde la máquina virtual. Puede usar la "IP de recuperación del sitio" correspondiente si el proxy del firewall es compatible con las IP.
*.servicebus.windows.net | Se requiere para que se puedan escribir datos de supervisión y diagnóstico de Site Recovery desde la máquina virtual. Puede usar la "IP de supervisión de recuperación del sitio" correspondiente si el proxy del firewall es compatible con las IP.

## <a name="outbound-connectivity-for-ip-address-ranges"></a>Conectividad de salida para rangos de direcciones IP

Si utiliza un firewall basado en IP, proxy o NSG para controlar la conectividad saliente, estos intervalos IP tienen que permitirse.

- Todos los intervalos de direcciones IP que correspondan a las cuentas de almacenamiento en la región de origen.
    - Cree una regla de NSG basada en la [etiqueta del servicio Storage](../virtual-network/security-overview.md#service-tags) para la región de origen.
    - Permita estas direcciones para que los datos se puedan escribir en la cuenta de almacenamiento en caché, desde la máquina virtual.
- Cree una regla de grupos de seguridad de red basada en la [etiqueta de servicio de Azure Active Directory (AAD)](../virtual-network/security-overview.md#service-tags) para permitir el acceso a todas las direcciones IP correspondientes a AAD.
    - Si en el futuro se agregan nuevas direcciones a Azure Active Directory (AAD), tendrá que crear nuevas reglas de grupos de seguridad de red.
- Cree una regla de NSG basada en una etiqueta de servicio EventsHub para la región de destino, lo que permite el acceso a la supervisión de Site Recovery.
- Cree una regla de NSG basada en una etiqueta de servicio AzureSiteRecovery para permitir el acceso al servicio Site Recovery en cualquier región.
- Se recomienda crear las reglas de NSG necesarias en un grupo NSG de NSG de prueba y comprobar que no haya ningún problema antes de crear las reglas en un grupo de NSG de producción.


Si prefiere usar intervalos de direcciones IP de Site Recovery (no recomendado), consulte la tabla siguiente:

   **Destino** | **IP de Site Recovery** |  **IP de supervisión de Site Recovery**
   --- | --- | ---
   Asia oriental | 52.175.17.132 | 13.94.47.61
   Sudeste asiático | 52.187.58.193 | 13.76.179.223
   India Central | 52.172.187.37 | 104.211.98.185
   Sur de la India | 52.172.46.220 | 104.211.224.190
   Centro-Norte de EE. UU | 23.96.195.247 | 168.62.249.226
   Europa del Norte | 40.69.212.238 | 52.169.18.8
   Europa occidental | 52.166.13.64 | 40.68.93.145
   East US | 13.82.88.226 | 104.45.147.24
   Oeste de EE. UU. | 40.83.179.48 | 104.40.26.199
   Centro-Sur de EE. UU | 13.84.148.14 | 104.210.146.250
   Centro de EE. UU. | 40.69.144.231 | 52.165.34.144
   Este de EE. UU. 2 | 52.184.158.163 | 40.79.44.59
   Este de Japón | 52.185.150.140 | 138.91.1.105
   Oeste de Japón | 52.175.146.69 | 138.91.17.38
   Sur de Brasil | 191.234.185.172 | 23.97.97.36
   Este de Australia | 104.210.113.114 | 191.239.64.144
   Sudeste de Australia | 13.70.159.158 | 191.239.160.45
   Centro de Canadá | 52.228.36.192 | 40.85.226.62
   Este de Canadá | 52.229.125.98 | 40.86.225.142
   Centro occidental de EE.UU. | 52.161.20.168 | 13.78.149.209
   Oeste de EE. UU. 2 | 52.183.45.166 | 13.66.228.204
   Oeste de Reino Unido | 51.141.3.203 | 51.141.14.113
   Sur de Reino Unido 2 | 51.140.43.158 | 51.140.189.52
   Sur del Reino Unido 2 | 13.87.37.4| 13.87.34.139
   Norte del Reino Unido | 51.142.209.167 | 13.87.102.68
   Corea Central | 52.231.28.253 | 52.231.32.85
   Corea del Sur | 52.231.198.185 | 52.231.200.144
   Centro de Francia | 52.143.138.106 | 52.143.136.55
   Sur de Francia | 52.136.139.227 |52.136.136.62
   Centro de Australia| 20.36.34.70 | 20.36.46.142
   Centro de Australia 2| 20.36.69.62 | 20.36.74.130
   Oeste de Sudáfrica | 102.133.72.51 | 102.133.26.128
   Norte de Sudáfrica | 102.133.160.44 | 102.133.154.128
   Gobierno de EE. UU. - Virginia | 52.227.178.114 | 23.97.0.197
   US Gov Iowa | 13.72.184.23 | 23.97.16.186
   Gobierno de EE. UU.: Arizona | 52.244.205.45 | 52.244.48.85
   Gobierno de EE. UU.: Texas | 52.238.119.218 | 52.238.116.60
   Departamento de Defensa de EE. UU. Este | 52.181.164.103 | 52.181.162.129
   Departamento de Defensa de EE. UU. Centro | 52.182.95.237 | 52.182.90.133
   Norte de China | 40.125.202.254 | 42.159.4.151
   Norte de China 2 | 40.73.35.193 | 40.73.33.230
   Este de China | 42.159.205.45 | 42.159.132.40
   Este de China 2 | 40.73.118.52| 40.73.100.125
   Norte de Alemania| 51.116.208.58| 51.116.58.128
   Centro-oeste de Alemania | 51.116.156.176 | 51.116.154.192
   Oeste de Suiza | 51.107.231.223| 51.107.154.128
   Norte de Suiza | 51.107.68.31| 51.107.58.128
   Este de Noruega | 51.120.100.64| 51.120.98.128
   Oeste de Noruega | 51.120.220.65| 51.120.218.160

## <a name="example-nsg-configuration"></a>Configuración de NSG de ejemplo

En este ejemplo se muestra cómo configurar reglas de NSG para la replicación de una máquina virtual.

- Si usa reglas de NSG para controlar la conectividad de salida, utilice reglas para permitir el tráfico HTTPS de salida al puerto 443 para todos los intervalos de direcciones IP necesarios.
- En el ejemplo se supone que la ubicación de origen de la máquina virtual es "Este de EE. UU." y la ubicación de destino es "Centro de EE. UU".

### <a name="nsg-rules---east-us"></a>Reglas de NSG: este de EE. UU.

1. Cree una regla de seguridad HTTPS (443) de salida para "Storage.EastUS" en el NSG tal y como se muestra en la captura de pantalla siguiente.

      ![storage-tag](./media/azure-to-azure-about-networking/storage-tag.png)

2. Cree una regla de seguridad HTTPS (443) de salida para "AzureActiveDirectory" en el NSG tal y como se muestra en la captura de pantalla siguiente.

      ![aad-tag](./media/azure-to-azure-about-networking/aad-tag.png)

3. De forma similar a las reglas de seguridad anteriores, cree una regla de seguridad HTTPS (443) de salida para ""EventHub.CentralUS" en el NSG que corresponda a la ubicación de destino. Esto permite el acceso a la supervisión de Site Recovery.

4. Cree una regla de seguridad HTTPS (443) de salida para "AzureSiteRecovery" en el NSG. Esto permite el acceso al servicio Site Recovery en cualquier región.

### <a name="nsg-rules---central-us"></a>Reglas de NSG: centro de EE. UU.

Estas reglas son necesarias para que la replicación se pueda habilitar de la región de destino a la región de origen con posterioridad a la conmutación por error:

1. Cree una regla de seguridad HTTPS (443) de salida saliente para "Storage.CentralUS" en el NSG.

2. Cree una regla de seguridad HTTPS (443) de salida para "AzureActiveDirectory" en el NSG.

3. De forma similar a las reglas de seguridad anteriores, cree una regla de seguridad HTTPS (443) de salida para "EventHub.EastUS" en el NSG que corresponda a la ubicación de origen. Esto permite el acceso a la supervisión de Site Recovery.

4. Cree una regla de seguridad HTTPS (443) de salida para "AzureSiteRecovery" en el NSG. Esto permite el acceso al servicio Site Recovery en cualquier región.

## <a name="network-virtual-appliance-configuration"></a>Configuración de la aplicación virtual de red

Si usa aplicaciones virtuales de red (NVA) para controlar el tráfico de red saliente de las máquinas virtuales, el dispositivo podría verse limitado si todo el tráfico de replicación pasa a través de la NVA. Se recomienda crear un punto de conexión de servicio de red en la red virtual de "Storage" para que el tráfico de replicación no se dirija a la NVA.

### <a name="create-network-service-endpoint-for-storage"></a>Crear el punto de conexión de servicio de red de Storage
Puede crear un punto de conexión de servicio de red en la red virtual de "Storage" para que el tráfico de replicación no sobrepase el límite de Azure.

- Seleccione la red virtual de Azure y haga clic en "Puntos de conexión de servicio".

    ![storage-endpoint](./media/azure-to-azure-about-networking/storage-service-endpoint.png)

- Haga clic en "Agregar" y se abrirá la pestaña "Agregar extremos del servicio".
- Seleccione "Microsoft.Storage" en "Servicio", elija las subredes necesarias en el campo "Subredes" y haga clic en "Agregar".

>[!NOTE]
>No restrinja el acceso de red virtual a las cuentas de almacenamiento que usa para ASR. Debe permitir el acceso desde todas las redes.

### <a name="forced-tunneling"></a>Tunelización forzada

Puede invalidar la ruta del sistema predeterminada de Azure para el prefijo de dirección 0.0.0.0/0 con una [ruta personalizada](../virtual-network/virtual-networks-udr-overview.md#custom-routes) y desviar el tráfico de la máquina virtual a una aplicación virtual de red (NVA) local, pero esta configuración no se recomienda para la replicación de Site Recovery. Si va a usar rutas personalizadas, debe [crear un punto de conexión de servicio de red virtual](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) en su red virtual de "Storage" para que el tráfico de replicación no salga de los límites de Azure.

## <a name="next-steps"></a>Pasos siguientes
- Comience a proteger las cargas de trabajo mediante la [replicación de máquinas virtuales de Azure](site-recovery-azure-to-azure.md).
- Más información sobre la [retención de direcciones IP](site-recovery-retain-ip-azure-vm-failover.md) en la conmutación por error de máquinas virtuales de Azure.
- Más información sobre la recuperación ante desastres de [máquinas virtuales de Azure con ExpressRoute](azure-vm-disaster-recovery-with-expressroute.md).
