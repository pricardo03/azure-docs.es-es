---
title: Automatización de la migración de un gran número de máquinas virtuales a Azure | Microsoft Docs
description: Describe cómo usar scripts para migrar un gran número de máquinas virtuales con Azure Site Recovery
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: article
ms.date: 02/07/2019
ms.author: snehaa
ms.openlocfilehash: c0fc4fa0bdd58b8ecdf4f26051d60324118c4b21
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2019
ms.locfileid: "55896641"
---
# <a name="scale-migration-of-vms-using-azure-site-recovery"></a>Escala de la migración de máquinas virtuales con Azure Site Recovery

Este artículo le ayuda a comprender el proceso de usar scripts para migrar un gran número de máquinas virtuales con Azure Site Recovery. Estos scripts están disponibles para su descarga entre los [ejemplos de Azure PowerShell](https://github.com/Azure/azure-docs-powershell-samples) en GitHub. Los scripts pueden usarse para migrar VMware, AWS, máquinas virtuales de GCP y servidores físicos en Azure. También puede usar estos scripts para migrar máquinas virtuales de Hyper-V si va a migrar las máquinas virtuales como servidores físicos. Los scripts aprovechan PowerShell en Azure Site Recovery, que se documenta [aquí](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell).

## <a name="current-limitations"></a>Limitaciones actuales:
- Los scripts actualmente admiten la migración solo a discos no administrados
- Compatibilidad solo con la migración a discos Estándar
- Compatibilidad de la especificación de la dirección IP estática solo para la NIC principal de la máquina virtual de destino
- Los scripts no toman las entradas relacionadas con la Ventaja híbrida de Azure, tendrá que actualizar manualmente las propiedades de la VM replicada en el portal

## <a name="how-does-it-work"></a>¿Cómo funciona?

### <a name="prerequisites"></a>Requisitos previos
Antes de comenzar, siga estos pasos:
- Asegúrese de que se ha creado el almacén de Site Recovery en su suscripción de Azure
- Asegúrese de que el servidor de configuración y el servidor de procesos están instalados en el entorno de origen y el almacén es capaz de detectar el entorno
- Asegúrese de que se crea una directiva de replicación y de que se asocia al servidor de configuración
- Asegúrese de que ha agregado la cuenta de administrador de la máquina virtual al servidor de configuración (que se usará para replicar las máquinas virtuales de un entorno local)
- Asegúrese de que se crean los artefactos de destino en Azure
    - Grupo de recursos de destino
    - Cuenta de almacenamiento de destino (y su grupo de recursos)
    - Red virtual de destino para la conmutación por error (y su grupo de recursos)
    - Subred de destino
    - Red virtual de destino para la conmutación por error de prueba (y su grupo de recursos)
    - Conjunto de disponibilidad (si se necesita)
    - Grupo de seguridad de red de destino y su grupo de recursos
- Asegúrese de que ha decidido las propiedades de la máquina virtual de destino
    - Nombre de máquina virtual de destino
    - Tamaño de la máquina virtual de destino en Azure (se puede decidir mediante la evaluación de Azure Migrate)
    - Dirección de IP privada de la NIC principal en la máquina virtual
- Descarga de los scripts de los [ejemplos de Azure PowerShell](https://github.com/Azure/azure-docs-powershell-samples) en GitHub

### <a name="csv-input-file"></a>Archivo de entrada CSV
Una vez que haya completado todos los requisitos previos, deberá crear un archivo CSV que contenga datos para cada máquina de origen que desea migrar. El archivo CSV de entrada debe tener una línea de encabezado con los detalles de entrada y una fila con los detalles de cada máquina que se debe migrar. Todos los scripts están diseñados para funcionar en el mismo archivo CSV. Hay una plantilla de CSV de ejemplo disponible en la carpeta de scripts para su referencia.

### <a name="script-execution"></a>Ejecución de script
Una vez que el archivo CSV está listo, puede ejecutar los pasos siguientes para realizar la migración de las máquinas virtuales locales:

**Núm. de paso** | **Nombre del script** | **Descripción**
--- | --- | ---
1 | asr_startmigration.ps1 | Habilite la replicación para todas las máquinas virtuales que se muestran en el archivo .csv, el script crea una salida .csv con los detalles del trabajo para cada máquina virtual.
2 | asr_replicationstatus.ps1 | Compruebe el estado de replicación, el script crea un archivo .csv con el estado de cada máquina virtual.
3 | asr_updateproperties.ps1 | Una vez que las máquinas virtuales se replican/protegen, use este script para actualizar las propiedades de destino de la máquina virtual (propiedades de proceso y red).
4 | asr_propertiescheck.ps1 | Compruebe si las propiedades se actualizan correctamente.
5 | asr_testmigration.ps1 |  Inicie la conmutación por error de prueba para las máquinas virtuales que se muestran en el archivo .csv, el script crea una salida .csv con los detalles del trabajo para cada máquina virtual.
6 | asr_cleanuptestmigration.ps1 | Una vez que valide manualmente las máquinas virtuales que realizaron la conmutación por error de prueba, puede usar este script para limpiar las máquinas virtuales de conmutación por error de prueba.
7 | asr_migration.ps1 | Realice una conmutación por error no planeada para las máquinas virtuales que se muestran en el archivo .csv, el script crea una salida .csv con los detalles del trabajo para cada máquina virtual. El script no apaga las máquinas virtuales de un entorno local antes de desencadenar la conmutación por error; por motivos de coherencia de la aplicación, se recomienda que apague manualmente las máquinas virtuales antes de ejecutar el script.
8 | asr_completemigration.ps1 | Realice la operación de confirmación en las máquinas virtuales y elimine las entidades de ASR.
9 | asr_postmigration.ps1 | Si tiene previsto asignar grupos de seguridad de red a las NIC después de la conmutación por error, puede usar este script para hacerlo. Asigna un NSG a una NIC en la máquina virtual de destino.

## <a name="next-steps"></a>Pasos siguientes

[Obtenga más información](https://docs.microsoft.com/azure/site-recovery/migrate-tutorial-on-premises-azure) sobre cómo migrar servidores a Azure mediante Azure Site Recovery.
