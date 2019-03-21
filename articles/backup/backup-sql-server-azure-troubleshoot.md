---
title: Solución de problemas de copia de seguridad de base de datos de SQL Server con Azure Backup | Microsoft Docs
description: Información para solución de problemas para realizar copias de seguridad de bases de datos de SQL Server que se ejecutan en máquinas virtuales de Azure con Azure Backup.
services: backup
author: anuragm
manager: shivamg
ms.service: backup
ms.topic: article
ms.date: 03/13/2019
ms.author: anuragm
ms.openlocfilehash: b8fb6e2b23c275d198ac58fec874ad6627a7b43e
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58007169"
---
# <a name="troubleshoot-back-up-sql-server-on-azure"></a>Solución de problemas de copia de seguridad de SQL Server en Azure

En este artículo se proporciona información para la solución de problemas para proteger las máquinas virtuales con SQL Server en Azure (versión preliminar).

## <a name="feature-consideration-and-limitations"></a>Limitaciones y consideraciones de característica

Para ver la consideración de característica, consulte el artículo de [copia de seguridad acerca de SQL Server en máquinas virtuales de Azure](backup-sql-server-azure-vms.md#feature-consideration-and-limitations).

## <a name="sql-server-permissions"></a>Permisos de SQL Server

Para configurar la protección de una base de datos de SQL Server en una máquina virtual, se debe instalar en ella la extensión **AzureBackupWindowsWorkload**. Si recibe el error **UserErrorSQLNoSysadminMembership**, significa que la instancia de SQL no tiene los permisos de copia de seguridad requeridos. Para corregir este error, siga los pasos que se describen en [Definición de permisos para máquinas virtuales SQL no incluidas en el catálogo de soluciones](backup-azure-sql-database.md#fix-sql-sysadmin-permissions).

## <a name="troubleshooting-errors"></a>Solución de errores

La información de las tablas siguientes se usa para solucionar problemas y errores detectados durante la protección de SQL Server en Azure.

## <a name="alerts"></a>Alertas

### <a name="backup-type-unsupported"></a>Tipo de copia de seguridad no compatible

| Gravedad | DESCRIPCIÓN | Causas posibles | Acción recomendada |
|---|---|---|---|
| Advertencia | La configuración actual de esta base de datos no admite determinados tipos de copia de seguridad presentes en la directiva asociada. | <li>**Base de datos principal**: Se puede realizar solo una operación de copia de seguridad de base de datos completa en la base de datos maestra; ni **diferencial** copia de seguridad ni transacciones **registros** es posible la copia de seguridad. </li> <li>Ninguna base de datos del **modelo de recuperación simple** admite la realización de copias de seguridad de **registros** de transacciones.</li> | Modifique la configuración de la base de datos de tal forma que se admitan todos los tipos de copia de seguridad de la directiva. De forma alternativa, cambie la directiva actual para incluir solo los tipos de copia de seguridad compatibles. En caso contrario, se omitirán los tipos de copia de seguridad no admitidos durante la copia de seguridad programada o el trabajo de copia de seguridad se producirá un error de copia de seguridad ad hoc.


## <a name="backup-failures"></a>Errores de copia de seguridad

Las siguientes tablas se organizan por código de error.

### <a name="usererrorsqlpodoesnotsupportbackuptype"></a>UserErrorSQLPODoesNotSupportBackupType

| Mensaje de error | Causas posibles | Acción recomendada |
|---|---|---|
| Esta base de datos SQL no admite el tipo de copia de seguridad solicitado. | Se produce cuando el modelo de recuperación de base de datos no admite el tipo de copia de seguridad solicitado. El error puede ocurrir en las siguientes situaciones: <br/><ul><li>Una base de datos que usa un modelo de recuperación simple no permite la copia de seguridad de registros.</li><li>No se permiten copias de seguridad diferenciales y de registros de bases de datos maestras.</li></ul>Para más información, consulte el documento [Modelos de recuperación (SQL Server)](https://docs.microsoft.com/sql/relational-databases/backup-restore/recovery-models-sql-server). | Si se produce un error en la copia de seguridad de registros de la base de datos en el modelo de recuperación simple, pruebe una de estas opciones:<ul><li>Si la base de datos está en modo de recuperación simple, deshabilite las copias de seguridad de registros.</li><li>Use la [documentación de SQL](https://docs.microsoft.com/sql/relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server) para cambiar el modelo de recuperación de base de datos a Completo o Registro masivo. </li><li> Si no desea cambiar el modelo de recuperación y tiene una directiva estándar para realizar copias de seguridad de varias bases de datos que no se puede cambiar, ignore el error. Las copias de seguridad completas y diferenciales funcionarán en función de la programación. Se omitirán las copias de seguridad del registro, que es lo esperable en este caso.</li></ul>Si es una base de datos maestra y ha configurado una base de datos diferencial o de registros, siga cualquiera de estos pasos:<ul><li>Use el portal para cambiar la programación de la directiva de copia de seguridad para la base de datos maestra a Completa.</li><li>Si tiene una directiva estándar para realizar copias de seguridad de varias bases de datos que no se puede cambiar, ignore el error. La copia de seguridad completa funcionará según la programación. No se realizarán copias de seguridad diferenciales o de registro, que es lo que se espera en este caso.</li></ul> |
| La operación se canceló, porque ya se estaba ejecutando una operación en conflicto en la misma base de datos. | Consulte la [entrada del blog acerca de las limitaciones en las operaciones de copia de seguridad y restauración](https://blogs.msdn.microsoft.com/arvindsh/2008/12/30/concurrency-of-full-differential-and-log-backups-on-the-same-database) que se ejecutan de forma concurrente.| [Utilice SQL Server Management Studio (SSMS) para supervisar los trabajos de la base de datos](manage-monitor-sql-database-backup.md). Una vez que se produzca un error en la operación en conflicto, reinicie la operación.|

### <a name="usererrorsqlpodoesnotexist"></a>UserErrorSQLPODoesNotExist

| Mensaje de error | Causas posibles | Acción recomendada |
|---|---|---|
| La base de datos SQL no existe. | La base de datos se eliminó o se cambió de nombre. | Compruebe si la base de datos se ha eliminado o cambiado de nombre por accidente.<br/><br/> Si la base de datos se eliminó por accidente, para continuar realizando copias de seguridad, restaure la base de datos a la ubicación original.<br/><br/> Si ha eliminado la base de datos y no va a necesitar copias de seguridad en el futuro, en el almacén de Recovery Services, haga clic en [stop backup with "Delete/Retain data"](manage-monitor-sql-database-backup.md) (Detener copia de seguridad con "Eliminar o conservar datos").

### <a name="usererrorsqllsnvalidationfailure"></a>UserErrorSQLLSNValidationFailure

| Mensaje de error | Causas posibles | Acción recomendada |
|---|---|---|
| La cadena de registros se ha interrumpido. | Se realiza una copia de seguridad de la base de datos o de la máquina virtual mediante otra solución de copia de seguridad, lo que trunca la cadena de registros.|<ul><li>Compruebe si hay otra solución de copia de seguridad o script en uso. En ese caso, detenga la otra solución de copia de seguridad. </li><li>Si la copia de seguridad era una copia de seguridad del registro de ad hoc, desencadene una copia de seguridad completa para iniciar una nueva cadena de registros. Para las copias de seguridad de registros, no es preciso realizar ninguna acción, ya que el servicio Azure Backup desencadenará automáticamente una copia de seguridad completa para corregir este problema.</li>|

### <a name="usererroropeningsqlconnection"></a>UserErrorOpeningSQLConnection

| Mensaje de error | Causas posibles | Acción recomendada |
|---|---|---|
| Azure Backup no puede conectarse a la instancia de SQL. | Azure Backup no puede conectarse a la instancia de SQL. | Utilice los detalles adicionales del menú del error de Azure Portal para reducir las causas raíz. Para solucionar el error, consulte [Solución de problemas de copia de seguridad de SQL](https://docs.microsoft.com/sql/database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine).<br/><ul><li>Si la configuración predeterminada de SQL no permite conexiones remotas, cámbiela. Consulte los vínculos siguientes para cambiar la configuración.<ul><li>[https://msdn.microsoft.com/library/bb326495.aspx](https://msdn.microsoft.com/library/bb326495.aspx)</li><li>[https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-2-database-engine-error](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-2-database-engine-error)</li><li>[https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-53-database-engine-error](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-53-database-engine-error)</li></ul></li></ul><ul><li>Si hay problemas de inicio de sesión, consulte los vínculos de abajo para corregirlos:<ul><li>[https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error)</li><li>[https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18452-database-engine-error](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18452-database-engine-error)</li></ul></li></ul> |

### <a name="usererrorparentfullbackupmissing"></a>UserErrorParentFullBackupMissing

| Mensaje de error | Causas posibles | Acción recomendada |
|---|---|---|
| Falta la primera copia de seguridad completa de este origen de datos. | Falta la copia de seguridad completa de la base de datos. Las copias de seguridad diferenciales y de registros dependen de una copia de seguridad completa, por lo que deben realizarse copias de seguridad completas antes de desencadenar copias de seguridad diferenciales o de registros. | Desencadenar una copia de seguridad completa ad hoc.   |

### <a name="usererrorbackupfailedastransactionlogisfull"></a>UserErrorBackupFailedAsTransactionLogIsFull

| Mensaje de error | Causas posibles | Acción recomendada |
|---|---|---|
| No se puede realizar una copia de seguridad porque el registro de transacciones del origen de datos está lleno. | El espacio del registro de transacciones de la base de datos está lleno. | Para corregir este problema, consulte la [documentación de SQL](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-9002-database-engine-error). |
| Esta base de datos SQL no admite el tipo de copia de seguridad solicitado. | Las réplicas secundarias de los grupos de disponibilidad AlwaysOn no admiten copias de seguridad completas y diferenciales. | <ul><li>Si se desencadena una copia de seguridad ad hoc, desencadenar las copias de seguridad en el nodo principal.</li><li>Si la directiva ha programado la copia de seguridad, asegúrese de que está registrado el nodo principal. Para registrar el nodo [siga los pasos necesarios para detectar una base de datos de SQL Server](backup-azure-sql-database.md#discover-sql-server-databases).</li></ul> |

## <a name="restore-failures"></a>Errores de restauración

Los siguientes códigos de error se muestran cuando se produce un error al restaurar trabajos.

### <a name="usererrorcannotrestoreexistingdbwithoutforceoverwrite"></a>UserErrorCannotRestoreExistingDBWithoutForceOverwrite

| Mensaje de error | Causas posibles | Acción recomendada |
|---|---|---|
| Ya existe una base de datos con el mismo nombre en la ubicación de destino | El destino de la restauración ya tiene una base de datos con el mismo nombre.  | <ul><li>Cambie el nombre de la base de datos de destino</li><li>O bien, use la opción de forzar la sobrescritura en la página de restauración</li> |

### <a name="usererrorrestorefaileddatabasecannotbeofflined"></a>UserErrorRestoreFailedDatabaseCannotBeOfflined

| Mensaje de error | Causas posibles | Acción recomendada |
|---|---|---|
| Se produjo un error en la restauración, ya que no se pudo desconectar. | Al realizar una restauración, la base de datos de destino debe desconectarse. Azure Backup no puede ofrecer estos datos sin conexión. | Utilice los detalles adicionales del menú del error de Azure Portal para reducir las causas raíz. Para más información, consulte la [documentación de SQL](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-database-backup-using-ssms). |


###  <a name="usererrorcannotfindservercertificatewiththumbprint"></a>UserErrorCannotFindServerCertificateWithThumbprint

| Mensaje de error | Causas posibles | Acción recomendada |
|---|---|---|
| No se puede encontrar el certificado de servidor con la huella digital en el destino. | La base de datos maestra de la instancia de destino no tiene una huella digital de cifrado válida. | Importe en la instancia de destino la huella digital del certificado válida utilizada en la instancia de origen. |

## <a name="registration-failures"></a>Errores de registro

Los siguientes códigos de error son de errores de registro.

### <a name="fabricsvcbackuppreferencecheckfailedusererror"></a>FabricSvcBackupPreferenceCheckFailedUserError

| Mensaje de error | Causas posibles | Acción recomendada |
|---|---|---|
| No puede cumplir la preferencia de copia de seguridad del grupo de disponibilidad Always On de SQL, ya que algunos nodos del grupo de disponibilidad no están registrados. | Los nodos necesarios para realizar copias de seguridad no están registrados o no se puede acceder a ellos. | <ul><li>Asegúrese de que todos los nodos necesarios para realizar copias de seguridad de esta base de datos están registrados y en buen estado, y vuelva a intentar realizar la operación.</li><li>Cambie la preferencia de copia de seguridad del grupo de disponibilidad Always On de SQL.</li></ul> |

### <a name="vmnotinrunningstateusererror"></a>VMNotInRunningStateUserError

| Mensaje de error | Causas posibles | Acción recomendada |
|---|---|---|
| La máquina virtual con SQL Server está apagada o no se puede acceder al servicio Azure Backup. | La máquina virtual está apagada | Asegúrese de que el servidor de SQL Server funciona. |

### <a name="guestagentstatusunavailableusererror"></a>GuestAgentStatusUnavailableUserError

| Mensaje de error | Causas posibles | Acción recomendada |
|---|---|---|
| El servicio Azure Backup usa el agente invitado de la máquina virtual de Azure para realizar la copia de seguridad, pero dicho agente no está disponible en el servidor de destino. | El agente invitado no está habilitado o es incorrecto | [Instale el agente invitado de la máquina virtual](../virtual-machines/extensions/agent-windows.md) manualmente. |

## <a name="configure-backup-failures"></a>Configuración de errores de copia de seguridad

Los siguientes códigos de error son para la configuración de errores de copia de seguridad.

### <a name="autoprotectioncancelledornotvalid"></a>AutoProtectionCancelledOrNotValid

| Mensaje de error | Causas posibles | Acción recomendada |
|---|---|---|
| La intención de la protección automática se ha quitado o ya no es válida. | Cuando se habilita la protección automática en una instancia de SQL, se ejecutan los trabajos **Configurar copia de seguridad** para todas las bases de datos de esa instancia. Si se deshabilita la protección automática mientras se ejecutan los trabajos, los trabajos **en curso** se cancelan con este código de error. | Habilite la protección automática una vez más proteger todas las bases de datos restantes. |

## <a name="re-registration-failures"></a>Errores de nuevo registro

Comprobación de uno o varios de los [síntomas](#symptoms) antes de desencadenar la operación de volver a registrar.

### <a name="symptoms"></a>Síntomas

* Todas las operaciones como copia de seguridad, restaurar y configurar la copia de seguridad se producen errores en la máquina virtual con uno de los códigos de error siguiente: **WorkloadExtensionNotReachable**, **UserErrorWorkloadExtensionNotInstalled**, **WorkloadExtensionNotPresent**, **WorkloadExtensionDidntDequeueMsg**
* El **estado de copia de seguridad** para la copia de seguridad se muestra el elemento **denegado**. Aunque debe descartar todos los otros motivos que pueden provocar también el mismo estado:

  * Falta de permiso para realizar la copia de seguridad relacionados con las operaciones en la máquina virtual  
  * Máquina virtual se ha cerrado debido a que las copias de seguridad no pueden tener lugar
  * Problemas de red  

    ![Volver a registrar la máquina virtual](./media/backup-azure-sql-database/re-register-vm.png)

* En el caso de grupo de disponibilidad AlwaysOn, las copias de seguridad comienzan a generar errores después de cambiar la preferencia de copia de seguridad o cuando se ha producido una conmutación por error

### <a name="causes"></a>Causas
Estos síntomas pueden surgir debido a uno o varios de los siguientes motivos:

  * Se ha eliminado o se desinstala del portal de extensión 
  * Se ha desinstalado la extensión de la **Panel de Control** de la máquina virtual en **desinstalar o cambiar un programa** la interfaz de usuario
  * Máquina virtual se ha restaurado atrás en el tiempo mediante la restauración de discos en contexto
  * Máquina virtual se cerró durante un largo período debido a que la configuración de la extensión en ella ha expirado
  * Se ha eliminado la máquina virtual y otra máquina virtual se creó con el mismo nombre y en el mismo grupo de recursos que la máquina virtual eliminada
  * Uno de los nodos de grupo de disponibilidad no recibió la configuración de copia de seguridad completa, esto puede ocurrir en el momento del registro del grupo de disponibilidad en el almacén o cuando se agrega un nuevo nodo  <br>
    En los escenarios anteriores, se recomienda para desencadenar la operación de volver a registrar en la máquina virtual. Esta opción solo está disponible a través de PowerShell y pronto estará disponible en el portal de Azure.


## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de Azure Backup para las máquinas virtuales con SQL Server (versión preliminar pública), consulte [Azure Backup para máquinas virtuales SQL (versión preliminar pública)](../virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery.md#azbackup).
