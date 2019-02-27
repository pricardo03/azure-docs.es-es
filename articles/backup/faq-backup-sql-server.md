---
title: Preguntas frecuentes sobre la copia de seguridad de bases de datos de SQL Server en máquinas virtuales de Azure con Azure Backup
description: Se proporcionan respuestas a preguntas habituales sobre la copia de seguridad de bases de datos de SQL Server en máquinas virtuales de Azure con Azure Backup.
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 8/16/2018
ms.author: sogup
ms.openlocfilehash: a14406733ff60d53d4bf7792ff0c9a015c57d9b3
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430929"
---
# <a name="faq-on-sql-server-running-on-azure-vm-backup"></a>Preguntas frecuentes sobre SQL Server cuando se ejecuta en una copia de seguridad de máquina virtual de Azure

En este artículo se responde a preguntas comunes sobre la copia de seguridad de bases de datos de SQL Server que se ejecutan en máquinas virtuales de Azure con el servicio [Azure Backup](backup-overview.md).

> [!NOTE]
> Esta característica actualmente está en su versión preliminar pública.



## <a name="can-i-throttle-the-backup-speed"></a>¿Puedo limitar la velocidad de la copia de seguridad?

Sí. Puede limitar la velocidad a la que se ejecuta la directiva de copia de seguridad para minimizar el impacto en una instancia de SQL Server. Para cambiar la configuración:
1. En la instancia de SQL Server, en la carpeta *C:\Archivos de programa\Azure Workload Backup\bin*, cree el archivo **ExtensionSettingsOverrides.json**.
2. En el archivo **ExtensionSettingsOverrides.json**, cambie el valor de **DefaultBackupTasksThreshold** por uno inferior, por ejemplo, 5. <br>
  ` {"DefaultBackupTasksThreshold": 5}`

3. Guarde los cambios. Cierre el archivo.
4. En la instancia de SQL Server, abra el **Administrador de tareas**. Reinicie el servicio **AzureWLBackupCoordinatorSvc**.

## <a name="can-i-run-a-full-backup-from-a-secondary-replica"></a>¿Puedo ejecutar una copia de seguridad completa desde una réplica secundaria?
 No. No se admite esta característica.

## <a name="do-successful-backup-jobs-create-alerts"></a>¿Generan alertas los trabajos de copia de seguridad que se han realizado correctamente?

 No. Los trabajos de copia de seguridad correctos no generan alertas. Las alertas se envían únicamente para los trabajos de copia de seguridad con errores.

## <a name="can-i-see-scheduled-backup-jobs-in-the-jobs-menu"></a>¿Se pueden ver los trabajos de copia de seguridad programados en el menú de trabajos?

 No. El menú **Backup Jobs** (Trabajos de copia de seguridad) muestra los detalles de los trabajos a petición, pero no los trabajos de copia de seguridad programados. Si se producen errores en los trabajos de copia de seguridad programados, puede encontrar los detalles en las alertas de trabajos con errores. Para supervisar todos los trabajos de copia de seguridad ad hoc y los programados, use [SQL Server Management Studio](manage-monitor-sql-database-backup.md).

## <a name="are-future-databases-automatically-added-for-backup"></a>¿Las bases de datos futuras se agregan automáticamente para copia de seguridad?

 No. Al configurar la protección para una instancia de SQL Server, si selecciona la opción de nivel de servidor, se agregan todas las bases de datos. Si agrega las bases de datos a una instancia de SQL Server después de configurar la protección, deberá agregar manualmente las nuevas bases de datos para protegerlas. Las bases de datos no se incluyen de forma automática en la protección configurada.

##  <a name="how-do-i-restart-protection-after-changing-recovery-type"></a>¿Cómo se puede reiniciar la protección después de cambiar el tipo de recuperación?

Desencadene una copia de seguridad completa. Las copias de seguridad de registros empiezan según lo previsto.

## <a name="can-i-protect-availability-groups-on-premises"></a>¿Puedo proteger los grupos de disponibilidad locales?

 No. Azure Backup protege los servidores de SQL Server que se ejecutan en Azure. Si se distribuye un grupo de disponibilidad entre máquinas locales y de Azure, se podrá proteger este solo si la réplica principal se ejecuta en Azure. Además, Azure Backup solo protege los nodos que se ejecutan en la misma región de Azure que el almacén de Recovery Services.

## <a name="can-i-protect-availability-groups-across-regions"></a>¿Puedo proteger los grupos de disponibilidad entre regiones?

El almacén de Azure Backup Recovery Services puede detectar y proteger todos los nodos que se encuentran en la misma región que el almacén de Recovery Services. Si tiene un grupo de disponibilidad SQL AlwaysON que abarca varias regiones de Azure, deberá configurar la copia de seguridad desde la región que tiene el nodo principal. Azure Backup será capaz de detectar y proteger todas las bases de datos del grupo de disponibilidad según la preferencia de copia de seguridad. Si no se cumple la preferencia de copia de seguridad, se producirá un error en las copias de seguridad y se emitirá la alerta de error.

## <a name="can-i-exclude-databases-with-auto-protection-enabled"></a>¿Puedo excluir las bases de datos que tienen habilitada la protección automática?

No, la [protección automática](backup-azure-sql-database.md#enable-auto-protection) se aplica a toda la instancia. No puede proteger las bases de datos de una instancia de forma selectiva con la protección automática.

## <a name="can-i-have-different-policies-in-an-auto-protected-instance"></a>¿Puedo tener distintas directivas en una instancia protegida automáticamente?

Si ya tiene algunas bases de datos protegidas en una instancia, seguirá protegido por sus respectivas directivas incluso cuando establezca en **ON** (Activada) la opción de [protección automática](backup-azure-sql-database.md#enable-auto-protection). Sin embargo, todas las bases de datos no protegidas junto con las que desearía agregar en el futuro tendrán solo una única directiva que se defina en **Configurar copia de seguridad** una vez seleccionadas las bases de datos. De hecho, a diferencia de otras bases de datos protegidas, ni siquiera puede cambiar la directiva para una base de datos en una instancia protegida automáticamente.
Si desea hacerlo, la única manera es deshabilitar la protección automática en la instancia por el momento y, a continuación, cambiar la directiva para dicha base de datos. Ahora puede volver a habilitar la protección automática de esta instancia.

## <a name="if-i-delete-a-database-from-auto-protection-will-backups-stop"></a>Si se elimina una base de datos de la protección automática, ¿se detendrá las copias de seguridad?

No, si se quita una base de datos de una instancia protegida automáticamente, se siguen intentando las copias de seguridad en esa base de datos. Esto implica que la base de datos eliminada comienza a aparecer como incorrecta en **Elementos de copia de seguridad** y todavía se tratan como protegidas.

La única forma de dejar de proteger esta base de datos consiste en deshabilitar la [protección automática](backup-azure-sql-database.md#enable-auto-protection) en la instancia por el momento y, a continuación, elegir la opción **Detener copia de seguridad** en **Elementos de copia de seguridad** para esa base de datos. Ahora puede volver a habilitar la protección automática de esta instancia.

##  <a name="why-cant-i-see-an-added-database-for-an-auto-protected-instance"></a>¿Por qué no puedo ver una base de datos agregada para una instancia protegida automáticamente?

Puede que no vea al instante una base de datos recién agregada a una instancia [protegida automáticamente](backup-azure-sql-database.md#enable-auto-protection) con elementos protegidos. Esto se debe a que la detección normalmente se ejecuta cada ocho horas. Sin embargo, el usuario puede ejecutar una detección manual utilizando la opción **Recuperar bases de datos** para detectar y proteger las nuevas bases de datos inmediatamente, como se muestra en la imagen siguiente:

  ![Ver las bases de datos recién agregadas](./media/backup-azure-sql-database/view-newly-added-database.png)

## <a name="next-steps"></a>Pasos siguientes

[Aprenda a](backup-azure-sql-database.md) hacer una copia de seguridad de una base de datos de SQL Server que se ejecuta en una máquina virtual de Azure.
