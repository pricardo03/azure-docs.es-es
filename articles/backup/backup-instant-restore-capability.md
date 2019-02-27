---
title: Funcionalidad de restauración instantánea de Azure
description: Funcionalidad de restauración instantánea de Azure y preguntas frecuentes de la pila de copia de seguridad de VM, modelo de implementación de Resource Manager
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: sogup
ms.openlocfilehash: 1a25a9c3e0d099349286476f0ae3791efee1642f
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2019
ms.locfileid: "56452821"
---
# <a name="get-improved-backup-and-restore-performance-with-azure-backup-instant-restore-capability"></a>Rendimiento mejorado de la copia de seguridad y la restauración con la funcionalidad de restauración instantánea de Azure Backup

> [!NOTE]
> De acuerdo con los comentarios de los usuarios, vamos a cambiar el nombre de **pila de copia de seguridad de VM V2** por **restauración instantánea** para reducir la confusión con la funcionalidad de Azure Stack.

El nuevo modelo de restauración instantánea proporciona las siguientes mejoras en la característica:

* Posibilidad de ver instantáneas tomadas como parte de un trabajo de copia de seguridad que está disponible para la recuperación sin tener que esperar a que finalice la transferencia de datos al almacén. Reduce el tiempo de espera para la copia de instantáneas en el almacén antes de desencadenar la restauración.
* Reduce los tiempos de copia de seguridad y restauración al conservarse las instantáneas localmente durante dos días (de forma predeterminada). Este almacén predeterminado es configurable en cualquier valor entre 1 y 5 días.
* Compatibilidad con tamaños de disco de hasta 4 TB.
* Compatibilidad con discos SSD estándar.
*   Capacidad de usar cuentas de almacenamiento originales de una máquina virtual no administrada (por disco) al restaurar. Esta capacidad existe aun cuando la máquina virtual tenga discos distribuidos entre cuentas de almacenamiento. Acelera las operaciones de restauración para una amplia variedad de configuraciones de máquina virtual.



## <a name="whats-new-in-this-feature"></a>Novedades de esta característica

En la actualidad, el trabajo de copia de seguridad consta de dos fases:

1.  Toma de una instantánea de máquina virtual.
2.  Transferencia de una instantánea de máquina virtual al almacén de Azure Recovery Services.

Un punto de recuperación se considera creado solo después de que se terminan las fases 1 y 2. Como parte de esta actualización, se crea un punto de recuperación en cuanto finaliza la instantánea. Este punto de recuperación de tipo instantánea se puede usar para realizar una restauración con el mismo flujo de restauración. Este punto de recuperación se puede identificar en Azure Portal al usar "instantánea" como tipo de punto de recuperación y, después de transferir la instantánea al almacén, el tipo de punto de recuperación cambia a "instantánea y almacén".

![Trabajo de copia de seguridad de la pila de copia de seguridad de VM, modelo de implementación de Resource Manager, almacenamiento y almacén](./media/backup-azure-vms/instant-rp-flow.png)

Las instantáneas se conservan durante siete días. Esta característica permite la operación de restauración a partir de estas instantáneas, lo cual reduce los tiempos de restauración. Reduce el tiempo necesario para transformar y copiar datos desde el almacén en la cuenta de almacenamiento del usuario para escenarios de disco no administrado; mientras, para los usuarios de disco administrado, crea discos administrados fuera de los datos de la copia de seguridad.

## <a name="feature-considerations"></a>Consideraciones de la característica

* Las instantáneas se almacenan junto con los discos para acelerar la creación de puntos de recuperación y las operaciones de restauración. Como resultado, verá los costos de almacenamiento que corresponden a las instantáneas tomadas durante este período.
* Las instantáneas incrementales se almacenan como blobs en páginas. A todos los usuarios que usen discos no administrados se les cobrará por las instantáneas almacenadas en su cuenta de almacenamiento local. Como las colecciones de puntos de restauración utilizadas por las copias de seguridad de máquinas virtuales administradas usan instantáneas de blobs en el nivel de almacenamiento subyacente, para discos administrados, verá los costos correspondientes a los precios de instantáneas de blobs, que son incrementales.
* Para las cuentas de almacenamiento premium, las instantáneas tomadas para los puntos de recuperación de instantánea se consideran en el límite de 10 TB de espacio asignado.
* Puede obtener la capacidad de configurar la retención de instantáneas según las necesidades de restauración. En función de los requisitos, puede configurar la retención de instantáneas en un día como mínimo en la hoja de la directiva de copia de seguridad como se explica a continuación. Esto puede ayudarle a ahorrar costos de retención de instantáneas si no lleva a cabo restauraciones con frecuencia.


>[!NOTE]
>Con esta actualización instantánea de la restauración, la duración de la retención de instantáneas de todos los clientes (**los nuevos y los ya existentes**) se establecerá en un valor predeterminado de dos días. Sin embargo, puede establecer la duración según sus necesidades en cualquier valor entre 1 y 5 días.


## <a name="cost-impact"></a>Impacto sobre los costos

Las instantáneas incrementales se almacenan en la cuenta de almacenamiento de la máquina virtual, que se usa para la recuperación instantánea. Una "instantánea incremental" significa que el espacio ocupado por ella es igual que el espacio ocupado por las páginas escritas una vez creada la instantánea. La facturación se realiza aún por GB de espacio que ocupa la instantánea y el precio por GB es el que se menciona en la [página de precios](https://azure.microsoft.com/pricing/details/managed-disks/).


## <a name="upgrading-to-instant-restore"></a>Actualización a la restauración instantánea

Si usa Azure Portal, verá una notificación en el panel del almacén. Esta notificación está relacionada con la compatibilidad con discos grandes y las mejoras de velocidad de copia de seguridad y restauración.
Para abrir una pantalla para actualizarse a la restauración instantánea, seleccione el banner.

![Trabajo de copia de seguridad de la pila de copia de seguridad de VM, modelo de implementación de Resource Manager, notificación de compatibilidad](./media/backup-azure-vms/instant-rp-banner.png)

Haga clic en **Actualizar** como se indica en la siguiente captura de pantalla:

![Trabajo de copia de seguridad de la pila de copia de seguridad de VM, modelo de implementación de Resource Manager, actualización](./media/backup-azure-vms/instant-rp.png)

También puede ir a la página **Propiedades** del almacén para obtener la opción **Actualizar** en **Pila de copia de seguridad de VM**.

![Trabajo de copia de seguridad en la pila de copia de seguridad de VM: página Propiedades](./media/backup-azure-vms/instant-restore-capability-properties.png)


## <a name="configure-snapshot-retention-using-azure-portal"></a>Configuración de la retención de instantáneas mediante Azure Portal
Esta opción está actualmente disponible en las ubicaciones Centro-oeste de EE. UU., India del Sur y Este de Australia.

Para los usuarios actualizados, en Azure Portal puede ver que se ha agregado un campo a la hoja **Directiva de copia de seguridad de máquina virtual** en la sección **Restauración instantánea**. Puede cambiar la duración de la retención de instantáneas en la hoja **Directiva de copia de seguridad de máquina virtual** para todas las máquinas virtuales asociadas con la directiva de copia de seguridad específica.

![Funcionalidad de restauración instantánea](./media/backup-azure-vms/instant-restore-capability.png)

## <a name="upgrade-to-instant-restore-using-powershell"></a>Actualización a la restauración instantánea mediante PowerShell

Si desea el autoservicio y actualizarse a la restauración instantánea, ejecute los siguientes cmdlets desde un terminal de PowerShell con privilegios elevados:

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

## <a name="upgrade-to-instant-restore-using-cli"></a>Actualización a la restauración instantánea mediante la CLI

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

## <a name="verify-that-the-upgrade-is-successful"></a>Verifique que la actualización sea correcta

### <a name="powershell"></a>PowerShell
Desde un terminal de PowerShell con privilegios elevados, ejecute el siguiente cmdlet:

```
Get-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" -ProviderNamespace Microsoft.RecoveryServices
```

### <a name="cli"></a>CLI
Ejecute el siguiente comando desde un shell:

```
az feature show --namespace Microsoft.RecoveryServices --name InstantBackupandRecovery
```

Si pone "Registrado", la suscripción se ha actualizado a la restauración instantánea.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="what-are-the-cost-implications-of-instant-restore"></a>¿Cuáles son las implicaciones sobre el costo de la restauración instantánea?
Las instantáneas se almacenan junto con los discos para acelerar la creación de puntos de recuperación y las operaciones de restauración. Como resultado, verá los costos de almacenamiento que se corresponden con el período de retención de la instantánea seleccionada como parte de la directiva de copia de seguridad de máquina virtual.

### <a name="in-premium-storage-accounts-do-the-snapshots-taken-for-instant-recovery-point-occupy-the-10-tb-snapshot-limit"></a>En las cuentas de Premium Storage, ¿las instantáneas que se toman para el punto de recuperación instantáneo ocupan el límite de 10 TB para instantáneas?
Sí, en el caso de las cuentas de Premium Storage, las instantáneas que se toman para el punto de recuperación instantáneo ocupan los 10 TB de espacio asignado.

### <a name="how-does-the-snapshot-retention-work-during-the-five-day-period"></a>¿Cómo funciona la retención de instantáneas durante el período de cinco días?
Cada día se toma una instantánea nueva, por lo tanto, hay cinco instantáneas incrementales individuales. El tamaño de la instantánea depende de la actividad de datos, que, en la mayoría de los casos, es aproximadamente del 2 al 7 %.

### <a name="is-an-instant-restore-snapshot-an-incremental-snapshot-or-full-snapshot"></a>¿Es una instantánea de la restauración instantánea incremental o completa?
Las instantáneas realizadas como parte de la funcionalidad de restauración instantánea son incrementales.

### <a name="how-can-i-calculate-the-approximate-cost-increase-due-to-instant-restore-feature"></a>¿Cómo puedo calcular el aumento del costo aproximado debido a la característica de restauración instantánea?
Depende de la actividad de la máquina virtual. En un estado estable, podemos suponer que el aumento del costo es = actividad diaria del período de retención de instantáneas por costo de almacenamiento de máquina virtual por GB.

### <a name="if-the-recovery-type-for-a-restore-point-is-snapshot-and-vault-and-i-perform-a-restore-operation-which-recovery-type-will-be-used"></a>Si el tipo de recuperación de un punto de restauración es "Instantánea y almacén" y realizo una operación de restauración, ¿qué tipo de recuperación se usará?
Si el tipo de recuperación es "Instantánea y almacén", la restauración se realizará automáticamente desde la instantánea local, que se comparará con más rapidez con la restauración desde el almacén.

### <a name="what-happens-if-i-select-retention-period-of-restore-point-tier-2-less-than-the-snapshot-tier1-retention-period"></a>¿Qué ocurre si selecciono un período de retención de punto de restauración (nivel 2) inferior al de la instantánea (nivel 1)?
El nuevo modelo no permite eliminar el punto de restauración (nivel 2), a menos que se elimine la instantánea (nivel 1). Se recomienda programar un período de retención del punto de restauración (nivel 2) superior al período de retención de instantáneas.

### <a name="why-is-my-snapshot-existing-even-after-the-set-retention-period-in-backup-policy"></a>¿Por qué continúa existiendo mi instantánea incluso una vez expirado el tiempo de retención establecido en la directiva de copia de seguridad?
Si el punto de recuperación tiene instantáneas y que es el más reciente disponible, se conserva hasta el momento en que se realice la copia de seguridad siguiente correctamente. Esto atiende a lo estipulado en el diseño de la actual directiva de GC, que exige que haya al menos un punto de recuperación siempre presente en caso de que se produzca un error en todas las copias de seguridad por un problema en la máquina virtual. En escenarios normales, los puntos de recuperación se eliminan en un máximo de 24 horas después de su expiración.
