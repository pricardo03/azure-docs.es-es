---
title: Preparación de máquinas para la migración con Azure Migrate
description: Aprenda a preparar máquinas locales para la migración con Azure Migrate.
ms.topic: tutorial
ms.date: 02/17/2020
ms.custom: MVC
ms.openlocfilehash: adbe9e4b30bf57e8a2038b970306c126035abbe1
ms.sourcegitcommit: b8f2fee3b93436c44f021dff7abe28921da72a6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2020
ms.locfileid: "77426255"
---
# <a name="prepare-on-premises-machines-for-migration-to-azure"></a>Preparación de las máquinas locales para la migración a Azure

En este artículo se describe cómo preparar máquinas en el entorno local antes de comenzar a migrarlas a Azure con [Azure Migrate: Server Migration](migrate-services-overview.md#azure-migrate-server-migration-tool).


En este artículo:
> [!div class="checklist"]
> * Compruebe los límites de migración.
> * Compruebe los requisitos del sistema operativo y las limitaciones de compatibilidad.
> * Revise el acceso a la dirección URL y el puerto para las máquinas que desea migrar.
> * Revise los cambios que debe realizar antes de comenzar la migración.
> * Realice la configuración necesaria para que las letras de unidad se conserven después de la migración.
> * Prepare las máquinas para que pueda conectarse a las máquinas virtuales de Azure después de la migración.


## <a name="verify-migration-limitations"></a>Comprobación de los límites de migración

- Con Azure Migrate Server Migration puede evaluar hasta 35 000 máquinas virtuales de VMware o de Hyper-V en un único proyecto de Azure Migrate. Un proyecto puede combinar máquinas virtuales de VMware y de Hyper-V hasta los límites de cada tipo.
- Puede seleccionar hasta 10 máquinas virtuales a la vez para la migración. Si necesita replicar más, puede hacerlo en grupos de 10.
- En el caso de la migración sin agente de VMware, puede ejecutar hasta 100 replicaciones simultáneamente.

## <a name="verify-operating-system-requirements"></a>Comprobación de los requisitos de sistema operativo

- Compruebe que los [sistemas operativos Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) sean compatibles en Azure
- Compruebe que las [distribuciones de Linux](../virtual-machines/linux/endorsed-distros.md) sean compatibles en Azure.


## <a name="check-whats-supported"></a>Comprobación de la compatibilidad

- En máquinas virtuales de VMware, Server Migration admite la [migración con agente y sin agente](server-migrate-overview.md). Compruebe los [requisitos y el soporte técnico necesarios para la migración](migrate-support-matrix-vmware-migration.md) de las máquinas virtuales de VMware.
- Compruebe los [requisitos y el soporte técnico necesarios para la migración](migrate-support-matrix-hyper-v-migration.md) de Hyper-V.
- Compruebe los [requisitos y el soporte técnico necesarios para la migración](migrate-support-matrix-physical-migration.md) de máquinas físicas locales u otros servidores virtualizados. 




## <a name="review-urlport-access"></a>Revisión del acceso a la dirección URL o al puerto

Es posible que las máquinas necesiten acceder a Internet durante la migración.

- [Revise las direcciones URL](migrate-appliance.md#url-access) a las que el dispositivo de Azure Migrate deba acceso durante la migración sin agente. [Revise los requisitos de acceso a los puertos](migrate-support-matrix-vmware-migration.md#agentless-ports).
- Revise las [direcciones URL](migrate-replication-appliance.md#url-access) y los [puertos] (migrate-replication-appliance.md#port-access) que el dispositivo de replicación usa durante la migración de máquinas virtuales de VMware basada en agente. 
- [Revise](migrate-support-matrix-hyper-v-migration.md#hyper-v-hosts) las direcciones URL y los puertos a los que los hosts de Hyper-V necesitan acceder durante la migración. 
- Revise las [direcciones URL](migrate-replication-appliance.md#url-access) y los [puertos] (migrate-replication-appliance.md#port-access) que el dispositivo de replicación usa durante la migración del servidor físico.



## <a name="verify-required-changes-before-migration"></a>Comprobación de los cambios necesarios antes de la migración

Es posible que algunas máquinas virtuales requieran cambios para poder ejecutarse en Azure. Azure Migrate hace estos cambios automáticamente en las máquinas virtuales que ejecutan los siguientes sistemas operativos:
- Red Hat Enterprise Linux 6.5+, 7.0+
- CentOS 6.5+, 7.0+
- SUSE Linux Enterprise Server 12 SP1+
- Ubuntu 14.04LTS, 16.04LTS, 18.04LTS
- Debian 7, 8

En el caso de otros sistemas operativos, debe preparar las máquinas manualmente antes de la migración. 

### <a name="prepare-windows-machines"></a>Preparación de las máquinas Windows

Si va a migrar una máquina Windows, realice estos cambios antes de la migración. Si migra la máquina virtual antes de realizar los cambios, es posible que esta no arranque en Azure.

1. [Habilite la consola de acceso serie de Azure](../virtual-machines/troubleshooting/serial-console-windows.md) para la máquina virtual de Azure Esto ayuda a solucionar problemas. No es necesario reiniciar la máquina virtual. La máquina virtual de Azure se iniciará mediante la imagen de disco. Esto es equivalente a un reinicio para la nueva máquina virtual. 
2. Si va a migrar equipos que ejecutan Windows Server 2003, instale Hyper-V Guest Integration Services en el sistema operativo de la máquina virtual. [Más información](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services#install-or-update-integration-services).

### <a name="prepare-linux-machines"></a>Preparación de las máquinas Linux

1. Instale Linux Integration Services para Hyper-V. La mayoría de las nuevas versiones de las distribuciones de Linux incluyen esto de forma predeterminada.
2. Recompile la imagen init de Linux para que contenga los controladores de Hyper-V necesarios. Esto garantiza que la máquina virtual se iniciará en Azure y solo se requiere en algunas distribuciones.
3. [Habilite el registro de la consola serie de Azure](../virtual-machines/troubleshooting/serial-console-linux.md). Esto ayuda a solucionar problemas. No es necesario reiniciar la máquina virtual. La máquina virtual de Azure se iniciará mediante la imagen de disco. Esto es equivalente a un reinicio para la nueva máquina virtual.
4. Actualice el archivo de asignación de dispositivos que contiene las asociaciones de nombre de dispositivo y volumen para usar identificadores de dispositivo persistentes.
5. Actualizar las entradas fstab para usar identificadores de volumen persistentes.
6. Elimine las reglas udev que reserven los nombres de interfaz en función de la dirección MAC, etc.
7. Actualice las interfaces de red para recibir direcciones IP de DHCP.
8. [Más información](../virtual-machines/linux/create-upload-generic.md) sobre los pasos necesarios para ejecutar una máquina virtual Linux en Azure y obtenga instrucciones para algunas de las distribuciones de Linux más populares.

## <a name="preserve-drive-letters-after-migration"></a>Conservación de las letras de unidad después de la migración

Al migrar una máquina local a Microsoft Azure, es posible que las letras de unidad de los discos de datos adicionales cambien de valor. De forma predeterminada, las máquinas virtuales de Azure tienen asignada la unidad D para su uso como almacenamiento temporal. Esta asignación de unidad hace que todas las demás asignaciones de unidad de almacenamiento asociadas aumenten en una letra.

Por ejemplo, si la instalación local usa un disco de datos que se asigna a la unidad D para instalaciones de aplicaciones, la asignación de esta unidad se incrementa a la unidad E después de haber migrado la máquina virtual a Azure. Para evitar esta asignación automática y para asegurarse de que Azure asigna la siguiente letra de unidad libre a su volumen temporal, establezca la directiva de red de área de almacenamiento (SAN) en OnlineAll, como se indica a continuación:

1. En la máquina local (no en el servidor host), abra un símbolo del sistema con privilegios elevados.
2. Escriba **diskpart**.
3. Escriba **SAN**. Si no se mantiene la letra de unidad del sistema operativo invitado, se devuelven **Offline All** u **Offline Shared**.
4. En la petición de **DISKPART**, escriba **SAN Policy=OnlineAll**. Esta configuración garantiza que los discos se pongan en línea y que sean legibles y grabables.
5. Durante la migración de prueba, puede comprobar que se conserven las letras de unidad.


## <a name="check-azure-vm-requirements"></a>Comprobación de los requisitos de la máquina virtual de Azure

Las máquinas locales que se replican en Azure deben cumplir los requisitos de las máquinas virtuales de Azure para el sistema operativo y la arquitectura, los discos, la configuración de red y la nomenclatura de la máquina virtual. Compruebe los requisitos para los [servidores físicos y las máquinas virtuales de VMWare](migrate-support-matrix-vmware-migration.md#azure-vm-requirements) y para las [máquinas virtuales de Hyper-V](migrate-support-matrix-hyper-v-migration.md#azure-vm-requirements) antes de la migración.


## <a name="prepare-to-connect-after-migration"></a>Preparación para la conexión después de la migración

Las máquinas virtuales de Azure se crean durante la migración a Azure. Después de la migración, debe ser capaz de conectarse a las nuevas máquinas virtuales de Azure. Hay una serie de pasos necesarios para conectarse correctamente.

### <a name="prepare-to-connect-to-windows-azure-vms"></a>Preparación de la conexión a las máquinas virtuales Windows de Azure

En las máquinas Windows locales, realice lo siguiente:

1. Configure Windows. Esto incluye la eliminación de cualquier ruta estática o proxy WinHTTP persistente.
2. Asegúrese de que se están ejecutando [estos servicios](../virtual-machines/windows/prepare-for-upload-vhd-image.md#check-the-windows-services).
3. Habilite el protocolo de escritorio remoto (RDP) para permitir las conexiones remotas a la máquina local. [Aprenda](../virtual-machines/windows/prepare-for-upload-vhd-image.md#update-remote-desktop-registry-settings) a habilitar el protocolo de escritorio remoto con PowerShell.
4. Para acceder a una máquina virtual de Azure mediante Internet tras la migración, en el firewall de Windows de la máquina local, permita TCP y UDP en el perfil público y establezca el protocolo de escritorio remoto como aplicación permitida para todos los perfiles.
5. Si quiere acceder a una máquina virtual de Azure mediante una conexión VPN de sitio a sitio, en el firewall de Windows de la máquina local, permita el protocolo de escritorio remoto para los perfiles de dominio y privado. [Aprenda](../virtual-machines/windows/prepare-for-upload-vhd-image.md#configure-windows-firewall-rules) cómo permitir el tráfico RDP. 
6. Asegúrese de que no haya actualizaciones de Windows pendientes en la máquina virtual local al migrarla. Si es el caso, las actualizaciones podrían empezar a instalarse en la máquina virtual de Azure después de la migración y no podrá iniciar sesión en ella hasta que finalicen.


### <a name="prepare-to-connect-with-linux-azure-vms"></a>Preparación para la conexión a las máquinas virtuales Linux en Azure

En las máquinas Linux locales, haga lo siguiente:

1. Asegúrese de que el servicio de Secure Shell está configurado para iniciarse automáticamente al arrancar el sistema.
2. Compruebe que las reglas de firewall permiten una conexión SSH.

### <a name="configure-azure-vms-after-migration"></a>Configuración de máquinas virtuales de Azure después de la migración

Después de la migración, haga lo siguiente en las máquinas virtuales de Azure que se han creado.

1. Para conectarse a la máquina virtual desde Internet, asígnele una dirección IP pública. No puede usar la misma dirección IP pública para la máquina virtual de Azure que usó para la máquina local. [Más información](../virtual-network/virtual-network-public-ip-address.md).
2. Compruebe que las reglas del grupo de seguridad de red (NSG) en la máquina virtual permiten las conexiones entrantes al puerto RDP o SSH.
3. Haga clic en [Diagnósticos de arranque](../virtual-machines/troubleshooting/boot-diagnostics.md#enable-boot-diagnostics-on-existing-virtual-machine) para ver la máquina virtual.

> [!NOTE]
> El servicio Azure Bastion ofrece acceso de RDP y SSH privado a las máquinas virtuales de Azure. [Más información](../bastion/bastion-overview.md) sobre este servicio.



## <a name="next-steps"></a>Pasos siguientes

Decida qué método desea usar para la [migración de las máquinas virtuales de VMware](server-migrate-overview.md) a Azure o comience la migración de [máquinas virtuales de Hyper-V](tutorial-migrate-hyper-v.md) o de [servidores físicos o virtualizados/máquinas virtuales en la nube](tutorial-migrate-physical-virtual-machines.md).
