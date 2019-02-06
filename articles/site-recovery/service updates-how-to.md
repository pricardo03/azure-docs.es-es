---
title: Actualizaciones de Azure Site Recovery | Microsoft Docs
description: Se proporciona información general sobre las actualizaciones del servicio y cómo actualizar los componentes que se usan en Azure Site Recovery.
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 01/8/2019
ms.author: rajanaki
ms.openlocfilehash: 4af881a3c49505e07a7fb73bb8af5a62e5e5a06f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55176937"
---
# <a name="service-updates-in-azure-site-recovery"></a>Actualizaciones del servicio Azure Site Recovery
Las organizaciones tienen que saber cómo mantener los datos seguros y las aplicaciones y cargas de trabajo en funcionamiento cuando se producen interrupciones tanto planeadas como no planeadas. Azure Site Recovery ayuda a garantizar la continuidad empresarial y la recuperación ante desastrse, ya que mantiene las aplicaciones en funcionamiento tanto en las máquinas virtuales como en los servidores físicos disponibles si un sitio deja de funcionar. Site Recovery replica las cargas de trabajo que se ejecutan en máquinas virtuales y servidores físicos para que estén disponibles en una ubicación secundaria si el sitio principal no está disponible. Recupera las cargas de trabajo en el sitio principal cuando vuelva a estar en funcionamiento.

Site Recovery puede administrar la replicación de:

- [Máquinas virtuales de Azure que se replican entre regiones de Azure](azure-to-azure-tutorial-dr-drill.md).
- Máquinas virtuales locales y servidores físicos que se replican en Azure o en un sitio secundario.
Para más información, consulte la documentación [aquí](https://docs.microsoft.com/azure/site-recovery).

Azure Site Recovery publica actualizaciones del servicio de forma periódica, que incluyen la incorporación de nuevas características, mejoras en la matriz de compatibilidad y correcciones de errores, si hubiera. Para seguir disfrutando de las características, mejoras y correcciones de errores más recientes, se aconseja a los usuarios actualizar siempre a las últimas versiones de los componentes de Azure SIte Recovery. 
 
## <a name="support-statement-for-azure-site-recovery"></a>Declaración de soporte técnico de Azure Site Recovery 

> [!IMPORTANT]
> **Con cada nueva versión "N" de un componente de Azure Site Recovery que se publica, todas las versiones anteriores a "N-4" se consideran excluidas del soporte técnico.** Por este motivo, siempre se recomienda actualizar a las versiones más recientes disponibles.

> [!IMPORTANT]
> Se da soporte técnico oficial para las actualizaciones de las versiones >N-4 a N (siendo N la versión más reciente). Si tiene la versión N-6, deberá actualizar primero a N-4 y, después, a N.

### <a name="upgrading-when-the-difference-between-current-version-and-latest-released-version-is-greater-than-4"></a>Actualización cuando la diferencia entre la versión actual y la versión más reciente es mayor que 4

1. Como primer paso, actualice el componente instalado actualmente de, por ejemplo, la versión N a N+4 y, después, cambie a la siguiente versión compatible. Supongamos que la versión actual es 9.24 y tiene la versión 9,16; primero, actualice a 9.20 y, después, a 9.24.
2. Siga el mismo proceso para todos los componentes según el escenario.

### <a name="support-for-latest-oskernel-versions"></a>Soporte técnico para versiones de kernel o del sistema operativo más recientes

> [!NOTE]
> Si tiene programado un mantenimiento y, como parte de él, el equipo se va a reiniciar, se recomienda que primero actualice los componentes de Site Recovery y continúe con el resto de las actividades programadas.

1. Antes de actualizar las versiones del kernel o el sistema operativo, compruebe si la versión de destino es compatible con Azure Site Recovery. Encontrará la información en nuestra documentación sobre máquinas virtuales de Azure, [máquinas virtuales de VMware](vmware-physical-azure-support-matrix.md) y máquinas virtuales de Hyper-V en
2. Consulte las [actualizaciones del servicio](https://azure.microsoft.com/updates/?product=site-recovery) para ver qué versión de los componentes de Site Recovery admiten la versión específica que desea actualizar.
3. En primer lugar, actualice a la versión más reciente de Site Recovery.
4. Ahora, actualice el kernel o el sistema operativo a las versiones deseadas.
5. Reinicie el equipo.
6. Así se asegura de que la versión del kernel o del sistema operativo de las máquinas se actualicen a la versión más reciente, y de que se carguen los cambios más recientes de Site Recovery necesarios para admitir la nueva versión en la máquina de origen.



## <a name="azure-vm-disaster-recovery-to-azure"></a>Recuperación ante desastres de máquinas virtuales de Azure en Azure
En este escenario, es muy recomendable que [habilite](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-autoupdate) las actualizaciones automáticas. Puede optar por permitir que Site Recovery administre las actualizaciones de las siguientes maneras:

- Como parte del paso para habilitar la replicación:
- Alternar la configuración de actualización de la extensión dentro del almacén

Si ha elegido administrar las actualizaciones manualmente, siga estos pasos:

1. Vaya a Azure Portal y vaya a su almacén de Recovery Services.
2. Vaya al panel "Elementos replicados" de Azure Portal correspondiente al almacén de Recovery Services.
3. Haga clic en la notificación situada en la parte superior de la pantalla:
    
    *Hay una nueva actualización del agente de replicación de Site recovery disponible. Haga clic para instalar*
    
    *Haga clic para instalar ->*.

4. Seleccione las máquinas virtuales a las que desea aplicar la actualización y haga clic en **Aceptar**.

## <a name="between-two-on-premises-vmm-sites"></a>Entre dos sitios VMM locales
1. Descargue el paquete acumulativo de actualizaciones más reciente para el proveedor de Microsoft Azure Site Recovery.
2. Instale el paquete acumulativo de actualizaciones en primer lugar en el servidor de VMM local que administre el sitio de recuperación.
3. Una vez actualizado el sitio de recuperación, instale el paquete acumulativo de actualizaciones en el servidor de VMM que administra el sitio principal.

> [!NOTE]
> Si el servicio VMM es de alta disponibilidad (VMM en clúster), asegúrese de instalar la actualización en todos los nodos del clúster donde esté instalado el servicio VMM.

## <a name="between-an-on-premises-vmm-site-and-azure"></a>Entre un sitio VMM local y Azure
1. Descargue el paquete acumulativo de actualizaciones para el proveedor de Microsoft Azure Site Recovery.
2. Instale el paquete acumulativo de actualizaciones en el servidor de VMM local.
3. Instale el agente MARS más reciente en todos los hosts de Hyper-V.

> [!NOTE]
> Si el servicio VMM es de alta disponibilidad (VMM en clúster), asegúrese de instalar la actualización en todos los nodos del clúster donde esté instalado el servicio VMM.

## <a name="between-an-on-premises-hyper-v-site-and-azure"></a>Entre un sitio Hyper-V local y Azure

1. Descargue el paquete acumulativo de actualizaciones para el proveedor de Microsoft Azure Site Recovery.
2. Instale el proveedor en todos los nodos de los servidores de Hyper-V que haya registrado en Azure Site Recovery.

> [!NOTE]
> Si el suyo es un servidor host de Hyper-V en clúster, asegúrese de instalar la actualización en todos los nodos del clúster.

## <a name="between-an-on-premises-vmware-or-physical-site-to-azure"></a>Entre un sitio de VMware local o un sitio físico y Azure

1. Instale primero la actualización en el servidor de administración local. Este es el servidor que tiene los roles Servidor de configuración y Servidor de proceso. 
2. Si tiene servidores de procesos de escalado horizontal, actualícelos a continuación.
3. Vaya a Azure Portal y, a continuación, vaya a la página **Elementos protegidos** > **Elementos replicados**.
Seleccione una máquina virtual en esta página. Seleccione el botón **Actualizar agente** que aparece en la parte inferior de la página de cada máquina virtual. Esto actualiza el agente de Mobility Service en todas las máquinas virtuales protegidas.

Se recomienda reiniciar después de cada actualización del agente de Mobility Service para asegurarse de que se cargan todos los cambios más recientes en la máquina de origen. Sin embargo, **no es obligatorio**. Si, durante el último reinicio, la diferencia entre la versión del agente y la versión actual es mayor que 4, es obligatorio reiniciar. Consulte una explicación detallada en la tabla siguiente:

|**Versión del agente durante el último reinicio** | **Actualizando a** | **¿El reinicio es obligatorio?**|
|---------|---------|---------|--------|
|9.16 |  9.18 | No es obligatorio|
|9.16 | 9.19 | No es obligatorio|
| 9.16 | 9.20 | No es obligatorio
 | 9.16 | 9.21 | Sí, actualice primero a la versión 9.20; después, reinicie antes de actualizar a la versión 9.21 porque la diferencia entre las versiones (9.16 donde se realizó el último reinicio y la versión de destino 9.21) es >4.



## <a name="links-to-currently-supported-update-rollups"></a>Vínculos a paquetes acumulativos de actualizaciones admitidos actualmente


|Paquete acumulativo de actualizaciones  |Proveedor  |Instalación unificada| OVF  |MARS|
|---------|---------|---------|---------|--------|
|[Paquete acumulativo de actualizaciones 32](https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery)     |   5.1.3800.0  |  9.21.5091.1   |  5.1.3800.0  |2.0.9144.0
|[Paquete acumulativo de actualizaciones 31](https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery)     |     5.1.3700.0      |   9.20.5051.1      |     5.1.3700.0    |2.0.9144.0
|[Paquete acumulativo de actualizaciones 30](https://support.microsoft.com/help/4468181/azure-site-recovery-update-rollup-30)     |    5.1.3650.0   |   9.19.5007.1    |     5.1.3650.0    |2.0.9139.0
|[Paquete acumulativo de actualizaciones 29](https://support.microsoft.com/help/4466466/update-rollup-29-for-azure-site-recovery)     |   5.1.3650.0      |   9.19.4973.1     |     5.1.3700.0    |2.0.9131.0
|[Paquete acumulativo de actualizaciones 28](https://support.microsoft.com/help/4460079/update-rollup-28-for-azure-site-recovery)     |  5.1.3600 .0      |    9.19.4973.1     |  5.1.3600.0       |2.0.9131.0
| [Paquete acumulativo de actualizaciones 27](https://support.microsoft.com/help/4055712/update-rollup-27-for-azure-site-recovery)       |   5.1.3550.0      |    9.18.4946.1     | 5.1.3550.0         |2.0.9125.0


## <a name="previous-update-rollups"></a>Paquetes acumulativos de actualizaciones anteriores
- [Paquete acumulativo de actualizaciones 26](https://support.microsoft.com/help/4344054/update-rollup-26-for-azure-site-recovery)  
- [Paquete acumulativo de actualizaciones 25](https://support.microsoft.com/help/4278275/update-rollup-25-for-azure-site-recovery) 
- [Paquete acumulativo de actualizaciones 23](https://support.microsoft.com/help/4091311/update-rollup-23-for-azure-site-recovery) 
- [Paquete acumulativo de actualizaciones 22](https://support.microsoft.com/help/4072852/update-rollup-22-for-azure-site-recovery) 
- [Paquete acumulativo de actualizaciones 21](https://support.microsoft.com/help/4051380/update-rollup-21-for-azure-site-recovery) 
- [Paquete acumulativo de actualizaciones 20](https://support.microsoft.com/help/4041105/update-rollup-20-for-azure-site-recovery) 
- [Paquete acumulativo de actualizaciones 19](https://support.microsoft.com/help/4034599/update-rollup-19-for-azure-site-recovery) 
