---
title: Preparación de máquinas virtuales de VMware que ejecutan Linux para la migración a Azure con Azure Migrate Server Migration | Microsoft Docs
description: En este artículo se describe cómo se prepara una máquina virtual Linux para la migración a Azure con Azure Migrate Server Migration.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 11/14/2018
ms.author: raynew
ms.openlocfilehash: efc410699ef6f4722857c812b38473c8f54b911b
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67810219"
---
# <a name="prepare-linux-vmware-vms-for-migration-to-azure"></a>Preparación de máquinas virtuales Linux de VMware para la migración a Azure 

En este artículo se describe cómo se preparan máquinas virtuales de VMware que ejecutan Linux para migrarlas a Azure mediante [Azure Migrate](migrate-overview.md). 

> [!NOTE]
> Azure Migrate Server Migration está actualmente en versión preliminar pública. Puede usar la versión de disponibilidad general existente de Azure Migrate para detectar y evaluar las máquinas virtuales para la migración, pero la migración en sí misma no se puede llevar a cabo con la versión de disponibilidad general existente.

Prepare las máquinas Linux de la siguiente manera:

1. Instale Linux Integration Services para Hyper-V. (Es posible que las nuevas versiones de las distribuciones de Linux lo tengan instalado de forma predeterminada).
2. Recompile la imagen init de Linux para que contenga los controladores de Hyper-V necesarios y para que la máquina virtual se arranque en Azure (necesario para algunas distribuciones).
3. Habilite el registro de la consola serie para la solución de problemas. [Más información](https://docs.microsoft.com/azure/virtual-machines/linux/serial-console).
4. Actualice el archivo de asignación de dispositivos que contiene las asociaciones de nombre de dispositivo y volumen para usar identificadores de dispositivo persistentes.
5. Actualice las entradas fstab para usar identificadores de volumen persistentes.
6. Elimine las reglas udev que reserven los nombres de interfaz en función de la dirección MAC, etc.
7. Actualice las interfaces de red para recibir direcciones IP de DHCP.
8. Asegúrese de que SSH está habilitado. Compruebe que el servicio SSHD está configurado para iniciarse automáticamente al reiniciar.
9. Asegúrese de que las solicitudes de conexión SSH entrantes no están bloqueadas por el firewall del sistema operativo o las reglas de la tabla de IP.

[Obtenga más información](https://docs.microsoft.com/azure/virtual-machines/linux/serial-console) sobre cómo realizar estos cambios en las distribuciones de Linux más populares.

## <a name="sample-script"></a>Script de ejemplo

Este script (prepare-for-azure.sh) proporciona un ejemplo para preparar las máquinas Linux. Es posible que el script no funcione en todas las distribuciones y entornos, pero es un punto de partida útil.

El script muestra cómo: 

- Regenerar la imagen init de Linux con los controladores necesarios, si es necesario.
- Actualizar las entradas fstab para usar identificadores de volumen persistentes.
- Redirigir los registros de la consola al puerto serie.
- Habilitar la consola de acceso serie de Azure.
- Quitar reglas de red udev.
- Insertar un script de ejecución en el inicio que se ejecute al iniciar la máquina virtual para comprobar si la máquina se ejecuta en Azure. En caso afirmativo, actualiza la configuración de red de la máquina virtual y establece la primera interfaz Ethernet de modo que use DHCP para adquirir una dirección IP.

### <a name="before-you-start"></a>Antes de comenzar

- El script de ejemplo contiene pasos de ejemplo. No debe ejecutarse en sistemas de producción, ya que podría dañar la máquina virtual en la que se ejecuta.
- Se recomienda ejecutarlo en una máquina virtual de prueba. Antes de empezar, realice una copia de seguridad o una instantánea de la máquina virtual para poder restaurarla si es necesario. 
- El script funciona cuando la máquina virtual ejecuta una de estas distribuciones de Linux:
    - Red Hat Enterprise Linux 6.5+, 7.1+
    - Cent OS 6.5+, 7.1+
    - SUSE Linux Enterprise Server 12 SP1, SP2, SP3
    - Ubuntu 14.04, 16.04, 18.04
    - Debian 7, 8

### <a name="run-the-script"></a>Ejecute el script

1. Copie el script en la máquina virtual Linux de prueba mediante un cliente SFTP o SCP, como FileZilla o WinScp.
2. Conéctese mediante SSH a la máquina Linux mediante una cuenta de administrador.
3. Vaya al directorio de scripts.
4. Para convertir el script en un archivo ejecutable, ejecute **sudo chmod 777 prepare-for-azure.sh**.
5. Ejecute el script con **./prepare-for-azure.sh**.

Aquí se muestra cómo se ejecuta el script:

![Script de Linux](./media/how-to-prepare-linux-for-migration/script1.png)
![Script de Linux](./media/how-to-prepare-linux-for-migration/script2.png)
![Script de Linux](./media/how-to-prepare-linux-for-migration/script3.png)
![Script de Linux](./media/how-to-prepare-linux-for-migration/script4.png)
![Script de Linux](./media/how-to-prepare-linux-for-migration/script5.png)
![Script de Linux](./media/how-to-prepare-linux-for-migration/script6.png)
![Script de Linux](./media/how-to-prepare-linux-for-migration/script7.png)
![Script de Linux](./media/how-to-prepare-linux-for-migration/script8.png)
![Script de Linux](./media/how-to-prepare-linux-for-migration/script9.png)
![Script de Linux](./media/how-to-prepare-linux-for-migration/script10.png)
![Script de Linux](./media/how-to-prepare-linux-for-migration/script11.png)
![Script de Linux](./media/how-to-prepare-linux-for-migration/script12.png)
![Script de Linux](./media/how-to-prepare-linux-for-migration/script13.png)
![Script de Linux](./media/how-to-prepare-linux-for-migration/script14.png)
![Script de Linux](./media/how-to-prepare-linux-for-migration/script15.png)
![Script de Linux](./media/how-to-prepare-linux-for-migration/script16.png)
![Script de Linux](./media/how-to-prepare-linux-for-migration/script17.png)
![Script de Linux](./media/how-to-prepare-linux-for-migration/script18.png)
![Script de Linux](./media/how-to-prepare-linux-for-migration/script19.png)
![Script de Linux](./media/how-to-prepare-linux-for-migration/script20.png)
![Script de Linux](./media/how-to-prepare-linux-for-migration/script21.png)
![Script de Linux](./media/how-to-prepare-linux-for-migration/script22.png)
![Script de Linux](./media/how-to-prepare-linux-for-migration/script23.png)
![Script de Linux](./media/how-to-prepare-linux-for-migration/script24.png)
![Script de Linux](./media/how-to-prepare-linux-for-migration/script25.png)



## <a name="next-steps"></a>Pasos siguientes

- Aprenda a usar la [asignación de dependencias de máquina](how-to-create-group-machine-dependencies.md) para crear grupos de confianza alta.
- [Obtenga más información](concepts-assessment-calculation.md) sobre cómo se calculan las evaluaciones.
