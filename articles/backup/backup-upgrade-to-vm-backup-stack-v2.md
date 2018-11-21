---
title: Actualización a la versión 2 de la pila de copia de seguridad de máquinas virtuales de Azure
description: Proceso de actualización y preguntas más frecuentes de la pila de copia de seguridad de VM, modelo de implementación de Resource Manager
services: backup
author: trinadhk
manager: vijayts
tags: azure-resource-manager, virtual-machine-backup
ms.service: backup
ms.topic: conceptual
ms.date: 10/3/2018
ms.author: trinadhk
ms.openlocfilehash: c65cfedd398bbb18d65f36a3f2a768e11443687a
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51636516"
---
# <a name="upgrade-to-azure-vm-backup-stack-v2"></a>Actualización a la versión 2 de la pila de copia de seguridad de máquinas virtuales de Azure

El modelo de implementación de Resource Manager para la actualización a la pila de copia de seguridad de máquina virtual (VM) proporciona las siguientes mejoras de características:

* Posibilidad de ver instantáneas tomadas como parte de un trabajo de copia de seguridad que está disponible para la recuperación sin tener que esperar a que finalice la transferencia de datos. Reduce el tiempo de espera para la copia de instantáneas en el almacén antes de desencadenar la restauración. Además, esta posibilidad elimina el requisito de almacenamiento adicional para la copia de seguridad de máquinas virtuales Premium, a excepción de la primera copia de seguridad.  

* Reduce los tiempos de copia de seguridad y restauración al conservarse las instantáneas en local durante siete días.

* Compatibilidad con tamaños de disco de hasta 4 TB.

* Capacidad de usar cuentas de almacenamiento originales de una máquina virtual no administrada al restaurar. Esta capacidad existe aun cuando la máquina virtual tenga discos distribuidos entre cuentas de almacenamiento. Acelera las operaciones de restauración para una amplia variedad de configuraciones de máquina virtual.
    > [!NOTE]
    > Esta funcionalidad no es la misma que reemplazar los discos de la máquina virtual con los datos de punto de recuperación.


## <a name="whats-changing-in-the-new-stack"></a>¿Qué es lo que cambia en la nueva pila?
En la actualidad, el trabajo de copia de seguridad consta de dos fases:
1.  Toma de una instantánea de máquina virtual.
2.  Transferencia de una instantánea de máquina virtual a Azure Backup Vault.

Un punto de recuperación se considera creado solo después de que se terminan las fases 1 y 2. Como parte de la nueva pila, se crea un punto de recuperación en cuanto finaliza la instantánea. También puede restaurar desde este punto de recuperación mediante el mismo flujo de restauración. Este punto de recuperación se puede identificar en Azure Portal al usar "instantánea" como tipo de punto de recuperación. Una vez que se ha transferido la instantánea al almacén, el tipo de punto de recuperación cambia a "instantánea y almacén".

![Trabajo de copia de seguridad de la pila de copia de seguridad de VM, modelo de implementación de Resource Manager, almacenamiento y almacén](./media/backup-azure-vms/instant-rp-flow.jpg)

De forma predeterminada, las instantáneas se conservan durante siete días. Esta característica permite que la restauración finalice más rápido a partir de estas instantáneas. Reduce el tiempo necesario para volver a copiar los datos desde el almacén en la cuenta de almacenamiento del cliente.

## <a name="considerations-before-upgrade"></a>Consideraciones antes de la actualización

* La actualización de la pila de copia de seguridad de máquinas virtuales es unidireccional, todas las copias de seguridad siguen este flujo. Como el cambio se produce en el nivel de suscripción, todas las máquinas virtuales se encuadran en este flujo. Todas las nuevas adiciones de características se basan en la misma pila. Actualmente no puede controlar la pila en el nivel de directiva.

* Las instantáneas se almacenan en local para acelerar la creación de puntos de recuperación y también las operaciones de restauración. Como resultado, verá los costos de almacenamiento que corresponden a las instantáneas tomadas durante el período de siete días.

* Las instantáneas incrementales se almacenan como blobs en páginas. A todos los clientes que usan discos no administrados se les cobra por los siete días que las instantáneas están almacenadas en la cuenta de almacenamiento local del cliente. Como las colecciones de puntos de restauración utilizadas por las copias de seguridad de máquinas virtuales administradas usan instantáneas de blobs en el nivel de almacenamiento subyacente, para discos administrados, verá los costos correspondientes a los [precios de instantáneas de blobs](https://docs.microsoft.com/rest/api/storageservices/understanding-how-snapshots-accrue-charges) y son incrementales.

* Si restaura una máquina virtual premium desde un punto de recuperación de instantánea, se usa una ubicación de almacenamiento temporal mientras se crea la máquina virtual.

* Para las cuentas de almacenamiento premium, las instantáneas tomadas para los puntos de recuperación de instantánea se consideran en el límite de 10 TB de espacio asignado.

> [!NOTE]
> Actualice a la versión 2 de la pila de copia de seguridad de máquinas virtuales de Azure para obtener soporte técnico de Azure Backup para [discos administrados SSD estándar](https://azure.microsoft.com/blog/announcing-general-availability-of-standard-ssd-disks-for-azure-virtual-machine-workloads/).

## <a name="upgrade"></a>Actualizar
### <a name="the-azure-portal"></a>El Portal de Azure
Si usa Azure Portal, ve una notificación en el panel del almacén. Esta notificación está relacionada con la compatibilidad con discos grandes y las mejoras de velocidad de copia de seguridad y restauración. También puede ir a la página de propiedades del almacén para obtener la opción de actualización.

![Trabajo de copia de seguridad de la pila de copia de seguridad de VM, modelo de implementación de Resource Manager, notificación de compatibilidad](./media/backup-azure-vms/instant-rp-banner.png)

Para abrir una pantalla para actualizarse a la nueva pila, seleccione el banner.

![Trabajo de copia de seguridad de la pila de copia de seguridad de VM, modelo de implementación de Resource Manager, actualización](./media/backup-azure-vms/instant-rp.png)

### <a name="powershell"></a>PowerShell
Ejecute los siguientes cmdlets desde un terminal de PowerShell con privilegios elevados:
1.  Inicio de sesión en la cuenta de Azure

    ```
    PS C:> Connect-AzureRmAccount
    ```

2.  Seleccione la suscripción que quiere registrar:

    ```
    PS C:>  Get-AzureRmSubscription –SubscriptionName "Subscription Name" | Select-AzureRmSubscription
    ```

3.  Registre esta suscripción:

    ```
    PS C:>  Register-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" –ProviderNamespace Microsoft.RecoveryServices
    ```
### <a name="cli"></a>CLI
Ejecute los siguientes comandos desde un shell:
1.  Inicio de sesión en la cuenta de Azure

    ```
    az login
    ```

2.  Seleccione la suscripción que quiere registrar:

    ```
    az account set --subscription "Subscription Name"
    ```

3.  Registre esta suscripción:

    ```
    az feature register --namespace Microsoft.RecoveryServices --name InstantBackupandRecovery
    ```

## <a name="verify-that-the-upgrade-is-finished"></a>Compruebe que la actualización ha finalizado
### <a name="powershell"></a>PowerShell
Desde un terminal de PowerShell con privilegios elevados, ejecute el siguiente cmdlet:

```
Get-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" –ProviderNamespace Microsoft.RecoveryServices
```

### <a name="cli"></a>CLI
Desde ashell, ejecute el siguiente comando:

```
az feature show --namespace Microsoft.RecoveryServices --name InstantBackupandRecovery
```

Si pone "Registrado", la suscripción se actualiza a la pila de copia de seguridad V2.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

Las siguientes preguntas y respuestas se han recopilado en foros y desde las preguntas de los clientes.

### <a name="will-upgrading-to-v2-impact-current-backups"></a>¿Actualizar a la versión 2 afectará las copias de seguridad actuales?
Si actualiza a la versión 2, no hay ningún impacto en las copias de seguridad actuales y no es necesario volver a configurar su entorno. Actualice y el entorno de copia de seguridad seguirá funcionando como lo ha hecho.

### <a name="what-does-it-cost-to-upgrade-to-azure-vm-backup-stack-v2"></a>¿Cuánto cuesta actualizar a la versión 2 de la pila de Azure VM Backup?
No hay ningún costo para actualizar la pila a la versión 2. Las instantáneas se almacenan de manera local para acelerar la creación de puntos de recuperación y las operaciones de restauración. Como resultado, verá los costos de almacenamiento que corresponden a las instantáneas tomadas durante el período de siete días.

### <a name="does-upgrading-to-stack-v2-increase-the-premium-storage-account-snapshot-limit-by-10-tb"></a>¿Actualizar a la versión 2 de la pila aumenta el límite de 10 TB para las instantáneas de la cuenta de almacenamiento?
No, el límite total de instantáneas por cuenta de almacenamiento sigue estando en 10 TB.

### <a name="in-premium-storage-accounts-do-snapshots-taken-for-instant-recovery-point-occupy-the-10-tb-snapshot-limit"></a>En las cuentas de Premium Storage, ¿las instantáneas que se toman para el punto de recuperación instantáneo ocupan el límite de 10 TB para instantáneas?
Si, en el caso de las cuentas de Premium Storage, las instantáneas que se toman para el punto de recuperación instantáneo ocupan los 10 TB de espacio asignado.

### <a name="how-does-the-snapshot-work-during-the-seven-day-period"></a>¿Cómo funciona la instantánea durante el período de siete días?
Cada día se toma una instantánea nueva. Hay siete instantáneas individuales. **No** es que el servicio crea una copia el primer día y le agrega cambios durante los próximos seis días.

### <a name="is-a-v2-snapshot-an-incremental-snapshot-or-full-snapshot"></a>¿Una instantánea de la versión 2 es una instantánea incremental o una instantánea completa?
Las instantáneas incrementales se usan para los discos no administrados. Para discos administrados, la colección de puntos de restauración creada por Azure Backup usa las instantáneas de blob y, por tanto, son incrementales.

### <a name="how-to-get-standard-ssd-managed-disk-support-for-a-virtual-machine"></a>¿Cómo obtener soporte técnico para discos administrados SSD estándar para una máquina virtual?
Actualice a la versión 2 de la pila de copia de seguridad de máquinas virtuales de Azure para obtener soporte técnico de Azure Backup para [discos administrados SSD estándar](https://azure.microsoft.com/blog/announcing-general-availability-of-standard-ssd-disks-for-azure-virtual-machine-workloads/).
