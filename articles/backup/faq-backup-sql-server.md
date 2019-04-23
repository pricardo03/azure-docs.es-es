---
title: Preguntas frecuentes sobre la copia de seguridad de bases de datos de SQL Server en máquinas virtuales de Azure con Azure Backup
description: Encuentre respuestas a preguntas habituales acerca de la copia de seguridad de bases de datos de SQL Server en máquinas virtuales de Azure con Azure Backup.
services: backup
author: sachdevaswati
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 03/19/2019
ms.author: sachdevaswati
ms.openlocfilehash: 8d6323c73e5313a29b7b0df09ebdd24a190879f5
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59791900"
---
# <a name="faq-about-sql-server-databases-that-are-running-on-an-azure-vm-backup"></a>Preguntas más frecuentes sobre las bases de datos de SQL Server que se ejecutan en una copia de seguridad de máquina virtual de Azure

Este artículo responden preguntas comunes acerca de seguridad de bases de datos de SQL Server que se ejecutan en máquinas virtuales (VM) y que usan el [Azure Backup](backup-overview.md) service.

## <a name="does-the-solution-retry-or-auto-heal-the-backups"></a>¿La solución vuelva a intentar o reparación automática de las copias de seguridad?

En algunas circunstancias, el servicio de copia de seguridad de Azure desencadena las copias de seguridad correctoras. Recuperación automática puede ocurrir por cualquiera de las seis condiciones que se mencionan a continuación:

  - Si la copia de seguridad diferencial o de registro produce un error debido a un Error de validación de LSN, copia de seguridad diferencial o de registro siguiente en su lugar, se convierte en una copia de seguridad completa.
  - Si no se ha producido ninguna copia de seguridad completa antes de la copia de seguridad diferencial o registro, ese registro o copia de seguridad diferencial en su lugar, se convierte en una copia de seguridad completa.
  - Si es más de 15 días en el momento de la última copia de seguridad completa, el siguiente inicio de sesión o la copia de seguridad diferencial en su lugar, se convierte en una copia de seguridad completa.
  - Todos los trabajos de copia de seguridad que obtengan cancelados debido a una actualización de la extensión vuelva a se activan una vez completada la actualización y se inicia la extensión.
  - Si elige sobrescribir la base de datos durante la restauración, se produce un error en la siguiente copia de seguridad del registro o diferencial y una copia de seguridad completa se desencadena en su lugar.
  - En casos donde una copia de seguridad completa es necesaria para restablecer las cadenas de registro debido al cambio en el modelo de recuperación de base de datos, un completo se activa automáticamente en la siguiente programación.

Reparación automática como una funcionalidad está habilitada para todos los usuarios de forma predeterminada; Sin embargo si elige participar del mismo, a continuación, realice los siguientes:

  * En la instancia de SQL Server, en el *C:\Program Files\Azure carga de trabajo Backup\bin* carpeta, cree o edite el **ExtensionSettingsOverrides.json** archivo.
  * En el **ExtensionSettingsOverrides.json**, establezca *{"EnableAutoHealer": false}*.
  * Guarde los cambios y cierre el archivo.
  * En la instancia de SQL Server, abra **administrar tareas** y, a continuación, reinicie el **AzureWLBackupCoordinatorSvc** service.  

## <a name="can-i-control-as-to-how-many-concurrent-backups-run-on-the-sql-server"></a>¿Puedo controlar sobre cuántas copias de seguridad simultáneas que se ejecutan en el servidor SQL server?

Sí. Puede limitar la velocidad a la que se ejecuta la directiva de copia de seguridad para minimizar el impacto en una instancia de SQL Server. Para cambiar la configuración:
1. En la instancia de SQL Server, en el *C:\Program Files\Azure carga de trabajo Backup\bin* carpeta, cree el *ExtensionSettingsOverrides.json* archivo.
2. En el *ExtensionSettingsOverrides.json* de archivo, cambie el **DefaultBackupTasksThreshold** si se establece en un valor inferior (por ejemplo, 5). <br>
  `{"DefaultBackupTasksThreshold": 5}`

3. Guarde los cambios y cierre el archivo.
4. En la instancia de SQL Server, abra el **Administrador de tareas**. Reinicie el servicio **AzureWLBackupCoordinatorSvc**.

> [!NOTE]
> En la experiencia del usuario todavía puede avanzar y programar tantas copias de seguridad en un momento dado, pero procesarán en una ventana deslizante, digamos, 5, según el ejemplo anterior.

## <a name="can-i-run-a-full-backup-from-a-secondary-replica"></a>¿Puedo ejecutar una copia de seguridad completa desde una réplica secundaria?
Según las limitaciones de SQL, puede ejecutar copia de seguridad copia solo completa en la réplica secundaria; Sin embargo, no se permite la copia de seguridad completa.

## <a name="can-i-protect-availability-groups-on-premises"></a>¿Puedo proteger a los grupos disponibilidad locales?
 No. Azure Backup protege las bases de datos de SQL Server que se ejecuta en Azure. Si un grupo de disponibilidad (AG) se distribuye entre las máquinas locales y Azure, se puede proteger el grupo de disponibilidad solo si la réplica principal se ejecuta en Azure. Además, Azure Backup protege solo los nodos que se ejecutan en la misma región de Azure como almacén de Recovery Services.

## <a name="can-i-protect-availability-groups-across-regions"></a>¿Puedo proteger grupos de disponibilidad entre regiones?
El almacén de copia de seguridad de Azure Recovery Services puede detectar y proteger todos los nodos que se encuentran en la misma región que el almacén. Si el grupo de disponibilidad SQL Server Always On abarca varias regiones de Azure, configure la copia de seguridad de la región que tiene el nodo principal. Azure Backup puede detectar todas las bases de datos del grupo de disponibilidad en función de sus preferencias con respecto a la copia de seguridad y protegerlas. Cuando no se cumplen sus preferencias de copia de seguridad, las copias de seguridad y obtener la alerta de error.

## <a name="do-successful-backup-jobs-create-alerts"></a>¿Generan alertas los trabajos de copia de seguridad que se han realizado correctamente?
 No. Los trabajos de copia de seguridad correctos no generan alertas. Las alertas se envían únicamente para los trabajos de copia de seguridad con errores. Se documenta el comportamiento detallado para alertas portal [aquí](backup-azure-monitoring-built-in-monitor.md). Sin embargo, en caso de que está interesado tienen alertas incluso para trabajos realizados correctamente, puede usar [supervisión con Azure Monitor](backup-azure-monitoring-use-azuremonitor.md).

## <a name="can-i-see-scheduled-backup-jobs-in-the-backup-jobs-menu"></a>¿Puedo ver los trabajos de copia de seguridad programados en el menú trabajos de copia de seguridad?
El **trabajo de copia de seguridad** menú, solo se mostrarán ad-hoc trabajos de copia de seguridad. Para el trabajo programado, utilice [supervisión con Azure Monitor](backup-azure-monitoring-use-azuremonitor.md).

## <a name="are-future-databases-automatically-added-for-backup"></a>¿Las bases de datos futuras se agregan automáticamente para copia de seguridad?
Sí, puede lograr esta funcionalidad con [protección automática](backup-sql-server-database-azure-vms.md#enable-auto-protection).  

## <a name="if-i-delete-a-database-from-an-autoprotected-instance-what-will-happen-to-the-backups"></a>Si elimino una base de datos de una instancia de autoprotected, ¿qué ocurrirá con las copias de seguridad?
Si se quita una base de datos de una instancia de autoprotected, todavía se intenta realizar las copias de seguridad de base de datos. Esto implica que la base de datos eliminada comienza a aparecer como incorrecto en **elementos de copia de seguridad** y seguirá estando protegido.

La manera correcta para detener la protección de esta base de datos es hacer **Detener copia de seguridad** con **eliminar datos** en esta base de datos.  

## <a name="if-i-do-stop-backup-operation-of-an-autoprotected-database-what-will-be-its-behavior"></a>¿Si se detiene la operación de copia de seguridad de una base de datos autoprotected cuál será su comportamiento?
Si lo hace **Detener copia de seguridad con conservar datos**, no hay copias de seguridad futuras llevarán a cabo y los puntos de recuperación existentes permanecerán intactos. La base de datos se seguirá considerando como protegido y se muestran bajo el **elementos de copia de seguridad**.

Si lo hace **Detener copia de seguridad con eliminación de datos**, no hay copias de seguridad futuras llevarán a cabo y se eliminarán también los puntos de recuperación existentes. Se considerará la base de datos no protegidos y se muestran en la instancia de la copia de seguridad de configuración. Sin embargo, a diferencia de otras hasta protegido bases de datos que se pueden seleccionar manualmente o que puede obtener autoprotected, esta base de datos aparece atenuado y no se pueden seleccionar. Es la única forma de volver a proteger esta base de datos deshabilitar la protección automática en la instancia. Ahora puede seleccionar esta base de datos y configurar la protección en él o volver a habilitar la protección automática en la instancia de nuevo.

## <a name="if-i-change-the-name-of-the-database-after-it-has-been-protected-what-will-be-the-behavior"></a>Si se cambia el nombre de la base de datos después de haber protegido, ¿cuál será el comportamiento?
Una base de datos vuelven a denominar se trata como una nueva base de datos. Por lo tanto, el servicio tratará esta situación como si la base de datos no se encontraron y con producirá un error en las copias de seguridad.

Puede seleccionar la base de datos, que ahora se denomina y configura la protección en él. En caso de que la protección automática está habilitada en la instancia, la base de datos ha cambiado el nombre se ha detectado y protegida automáticamente.

##  <a name="why-cant-i-see-an-added-database-for-an-autoprotected-instance"></a>¿Por qué no puedo ver se ha agregado una base de datos de una instancia de autoprotected?
Una base de datos que usted [agregar a una instancia de autoprotected](backup-sql-server-database-azure-vms.md#enable-auto-protection) no puede aparecer inmediatamente debajo de los elementos protegidos. Esto se debe a que la detección normalmente se ejecuta cada ocho horas. Sin embargo, puede detectar y proteger nuevas bases de datos inmediata si ejecuta una detección manualmente seleccionando **recuperar bases de datos**, tal y como se muestra en la siguiente imagen.

  ![Detectar manualmente una base de datos recién agregada](./media/backup-azure-sql-database/view-newly-added-database.png)


## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo [copia de seguridad de una base de datos de SQL Server](backup-azure-sql-database.md) que se está ejecutando en una máquina virtual de Azure.
