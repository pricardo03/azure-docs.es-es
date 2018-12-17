---
title: Modelo de datos de Log Analytics para Azure Backup
description: En este artículo se explican los detalles del modelo de datos de Log Analytics para los datos de Azure Backup.
services: backup
author: adiganmsft
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 07/24/2017
ms.author: adigan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f4ecc87a0a7a0c74b02b72164fe129daa6530ea2
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52877588"
---
# <a name="log-analytics-data-model-for-azure-backup-data"></a>Modelo de datos de Log Analytics para datos de Azure Backup
Use el modelo de datos de Log Analytics para crear informes. Con el modelo de datos, puede crear consultas y paneles personalizados o puede personalizar los datos Azure Backup, como lo prefiera.

## <a name="using-azure-backup-data-model"></a>Uso del modelo de datos de Azure Backup
Puede usar los siguientes campos que se proporcionan como parte del modelo de datos para crear objetos visuales, consultas personalizadas y paneles, según sus requisitos.

### <a name="alert"></a>Alerta
Esta tabla proporciona detalles acerca de los campos relacionados con la alerta.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| AlertUniqueId_s |Texto |Identificador único de la alerta generada |
| AlertType_s |Texto |Tipo de alerta, por ejemplo, Copia de seguridad |
| AlertStatus_s |Texto |Estado de la alerta, por ejemplo, Activa |
| AlertOccurrenceDateTime_s |Fecha y hora |Fecha y hora en que se creó la alerta |
| AlertSeverity_s |Texto |Gravedad de la alerta, por ejemplo, Crítica |
| EventName_s |Texto |Nombre del evento. Siempre AzureBackupCentralReport |
| BackupItemUniqueId_s |Texto |Identificador único del elemento de copia de seguridad asociado con la alerta |
| SchemaVersion_s |Texto |Versión actual del esquema, por ejemplo **V1** |
| State_s |Texto |Estado actual del objeto de la alerta por ejemplo, Active o Deleted |
| BackupManagementType_s |Texto |Tipo de proveedor para realizar la copia de seguridad, por ejemplo, IaaSVM o FileFolder al que pertenece esta alerta |
| nombreOperación |Texto |Nombre de la operación actual, por ejemplo, Alerta |
| Categoría |Texto |Categoría de datos de diagnóstico insertada en Log Analytics. Siempre AzureBackupReport |
| Recurso |Texto |Este es el recurso para el que se están recopilando datos; muestra el nombre del almacén de Recovery Services |
| ProtectedServerUniqueId_s |Texto |Identificador único del servidor protegido asociado a la alerta |
| VaultUniqueId_s |Texto |Identificador único del almacén protegido asociado a la alerta |
| SourceSystem |Texto |Sistema de origen de los datos actuales: Azure |
| ResourceId |Texto |Identificador único para el recurso sobre el que se recopilan los datos. Por ejemplo, el id. del recurso del almacén de Recovery Services |
| SubscriptionId |Texto |Identificador de la suscripción del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos |
| ResourceGroup |Texto |Grupo de recursos del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos |
| ResourceProvider |Texto |Proveedor de recursos para el que se recopilan los datos. Por ejemplo, Microsoft.RecoveryServices. |
| ResourceType |Texto |Tipo de recurso para el que se recopilan los datos. Por ejemplo: almacenes |

### <a name="backupitem"></a>BackupItem
Esta tabla proporciona detalles acerca de los campos relacionados con el elemento de copia de seguridad.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| EventName_s |Texto |Nombre del evento. Siempre AzureBackupCentralReport |  
| BackupItemUniqueId_s |Texto |Identificador único del elemento de copia de seguridad |
| BackupItemId_s |Texto |Identificador del elemento de copia de seguridad |
| BackupItemName_s |Texto |Nombre de elemento de copia de seguridad |
| BackupItemFriendlyName_s |Texto |Nombre descriptivo del elemento de copia de seguridad |
| BackupItemType_s |Texto |Tipo de elemento de copia de seguridad, por ejemplo, VM o FileFolder |
| ProtectedServerName_s |Texto |Nombre del servidor protegido al que pertenece el elemento de copia de seguridad |
| ProtectionState_s |Texto |Estado actual de protección del elemento copia de seguridad, por ejemplo, Protected o ProtectionStopped |
| SchemaVersion_s |Texto |Versión del esquema, por ejemplo, **V1** |
| State_s |Texto |Estado del objeto del elemento de copia de seguridad, por ejemplo, Active o Deleted |
| BackupManagementType_s |Texto |Tipo de proveedor para realizar la copia de seguridad, por ejemplo, IaaSVM o FileFolder al que pertenece este elemento de copia de seguridad |
| nombreOperación |Texto |Nombre de la operación, por ejemplo, BackupItem |
| Categoría |Texto |Categoría de datos de diagnóstico insertada en Log Analytics. Siempre AzureBackupReport |
| Recurso |Texto |Recurso para el que se recopilan datos, por ejemplo, nombre de almacén de Recovery Services |
| SourceSystem |Texto |Sistema de origen de los datos actuales: Azure |
| ResourceId |Texto |Id. de recurso para el que se recopilan datos, por ejemplo, id. de recurso del almacén de Recovery Services. |
| SubscriptionId |Texto |Identificador de la suscripción del recurso (p. ej., Almacén de Recovery Services) para los datos que se recopilan |
| ResourceGroup |Texto |Grupo de recursos del recurso (p. ej., Almacén de Recovery Services) para los datos que se recopilan |
| ResourceProvider |Texto |Proveedor de recursos de datos que se recopilan, por ejemplo, Microsoft.RecoveryServices |
| ResourceType |Texto |Tipo del recurso de datos que se recopilan, por ejemplo, los almacenes. |

### <a name="backupitemassociation"></a>BackupItemAssociation
Esta tabla proporciona detalles acerca de las asociaciones de elementos de copia de seguridad con varias entidades.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| EventName_s |Texto |Este campo representa el nombre de este evento; es siempre AzureBackupCentralReport |  
| BackupItemUniqueId_s |Texto |Identificador exclusivo del elemento de copia de seguridad |
| SchemaVersion_s |Texto |Este campo indica la versión actual del esquema; es **V1** |
| State_s |Texto |Estado actual del objeto de la asociación del elemento de copia de seguridad, por ejemplo, Active o Deleted |
| BackupManagementType_s |Texto |Tipo de proveedor para el servidor que realiza el trabajo de copia de seguridad, por ejemplo, IaaSVM o FileFolder. |
| nombreOperación |Texto |Este campo representa el nombre de la operación actual: BackupItemAssociation |
| Categoría |Texto |Este campo representa la categoría de datos de diagnóstico insertada en Log Analytics; es AzureBackupReport |
| Recurso |Texto |Este es el recurso para el que se están recopilando datos; muestra el nombre del almacén de Recovery Services |
| PolicyUniqueId_g |Texto |Identificador único para la política asociada al elemento de copia de seguridad. |
| ProtectedServerUniqueId_s |Texto |Identificador único del servidor protegido asociado al elemento de copia de seguridad. |
| VaultUniqueId_s |Texto |Identificador único del almacén que contiene el elemento de copia de seguridad. |
| SourceSystem |Texto |Sistema de origen de los datos actuales: Azure |
| ResourceId |Texto |Identificador de recursos de datos que se recopilan. Por ejemplo, el id. del recurso del almacén de Recovery Services. |
| SubscriptionId |Texto |Identificador de la suscripción del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos. |
| ResourceGroup |Texto |Grupo de recursos del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos. |
| ResourceProvider |Texto |Proveedor de recursos de datos que se recopilan, por ejemplo, Microsoft.RecoveryServices |
| ResourceType |Texto |Tipo del recurso de datos que se recopilan, por ejemplo, los almacenes. |

### <a name="job"></a>Trabajo
Esta tabla proporciona detalles acerca de los campos relacionados con los trabajos.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| EventName_s |Texto |Nombre del evento. Siempre AzureBackupCentralReport |
| BackupItemUniqueId_s |Texto |Identificador único del elemento de copia de seguridad |
| SchemaVersion_s |Texto |Versión del esquema, por ejemplo, **V1** |
| State_s |Texto |Estado actual del objeto del trabajo, por ejemplo, Active o Deleted |
| BackupManagementType_s |Texto |Tipo de proveedor para el servidor que realiza el trabajo de copia de seguridad, por ejemplo, IaaSVM o FileFolder. |
| nombreOperación |Texto |Este campo representa el nombre de la operación actual: Job |
| Categoría |Texto |Este campo representa la categoría de datos de diagnóstico insertada en Log Analytics; es AzureBackupReport |
| Recurso |Texto |Este es el recurso para el que se están recopilando datos; muestra el nombre del almacén de Recovery Services |
| ProtectedServerUniqueId_s |Texto |Identificador único del servidor protegido asociado al trabajo. |
| VaultUniqueId_s |Texto |Identificador único del almacén protegido. |
| JobOperation_s |Texto |Operación para la que se ejecuta el trabajo, por ejemplo, Backup, Restore o Configure Backup |
| JobStatus_s |Texto |Estado del trabajo terminado, por ejemplo, Completed o Failed |
| JobFailureCode_s |Texto |Cadena del código de error por el que produjo el error del trabajo |
| JobStartDateTime_s |Fecha y hora |Fecha y hora en las que comenzó la ejecución del trabajo |
| BackupStorageDestination_s |Texto |Destino de almacenamiento de almacenamiento de copias de seguridad, por ejemplo, Cloud o Disk  |
| JobDurationInSecs_s | Number |Duración del trabajo total en segundos |
| DataTransferredInMB_s | Number |Datos transferidos en MB para este trabajo|
| JobUniqueId_g |Texto |Identificador único que identifica el trabajo |
| SourceSystem |Texto |Sistema de origen de los datos actuales: Azure |
| ResourceId |Texto |Identificador de recursos de datos que se recopilan. Por ejemplo, el id. del recurso del almacén de Recovery Services.|
| SubscriptionId |Texto |Identificador de la suscripción del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos |
| ResourceGroup |Texto |Grupo de recursos del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos |
| ResourceProvider |Texto |Proveedor de recursos para el que se recopilan los datos. Por ejemplo, Microsoft.RecoveryServices. |
| ResourceType |Texto |Tipo de recurso para el que se recopilan los datos. Por ejemplo: almacenes |

### <a name="policy"></a>Directiva
Esta tabla proporciona detalles acerca de los campos relacionados con las directivas.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| EventName_s |Texto |Este campo representa el nombre de este evento; es siempre AzureBackupCentralReport |
| SchemaVersion_s |Texto |Este campo indica la versión actual del esquema; es **V1** |
| State_s |Texto |Estado actual del objeto de la directiva, por ejemplo, Active o Deleted |
| BackupManagementType_s |Texto |Tipo de proveedor para el servidor que realiza el trabajo de copia de seguridad, por ejemplo, IaaSVM o FileFolder. |
| nombreOperación |Texto |Este campo representa el nombre de la operación actual: Policy |
| Categoría |Texto |Este campo representa la categoría de datos de diagnóstico insertada en Log Analytics; es AzureBackupReport |
| Recurso |Texto |Este es el recurso para el que se están recopilando datos; muestra el nombre del almacén de Recovery Services |
| PolicyUniqueId_g |Texto |Identificador único que identifica la directiva |
| PolicyName_s |Texto |Nombre de la directiva definida |
| BackupFrequency_s |Texto |Frecuencia con la que se ejecutan las copias de seguridad, por ejemplo, a diario o semanalmente |
| BackupTimes_s |Texto |Fecha y hora en que se programan las copias de seguridad |
| BackupDaysOfTheWeek_s |Texto |Días de la semana en los que se han programado copias de seguridad |
| RetentionDuration_s |Número entero |Duración de retención de las copias de seguridad configuradas |
| DailyRetentionDuration_s |Número entero |Duración de retención total de las copias de seguridad configuradas, en días |
| DailyRetentionTimes_s |Texto |Fecha y hora en que se configuró la retención diaria |
| WeeklyRetentionDuration_s |Número decimal |Duración total de la retención semanal de las copias de seguridad configuradas, en semanas |
| WeeklyRetentionTimes_s |Texto |Fecha y hora en que se ha configurado la retención semanal |
| WeeklyRetentionDaysOfTheWeek_s |Texto |Días de la semana seleccionados para la retención semanal |
| MonthlyRetentionDuration_s |Número decimal |Duración de retención total de las copias de seguridad configuradas, en meses |
| MonthlyRetentionTimes_s |Texto |Fecha y hora en que se ha configurado la retención mensual |
| MonthlyRetentionFormat_s |Texto |Tipo de configuración para la retención mensual ,por ejemplo, diariamente si se basa en día, semanalmente si se basa en semana |
| MonthlyRetentionDaysOfTheWeek_s |Texto |Días de la semana seleccionados para la retención mensual |
| MonthlyRetentionWeeksOfTheMonth_s |Texto |Semanas del mes en que se ha configurado la retención mensual, por ejemplo, la primera, la última, etc. |
| YearlyRetentionDuration_s |Número decimal |Duración total de la retención de las copias de seguridad configuradas, en años |
| YearlyRetentionTimes_s |Texto |Fecha y hora en que se ha configurado la retención anual |
| YearlyRetentionMonthsOfTheYear_s |Texto |Meses del año seleccionados para la retención anual |
| YearlyRetentionFormat_s |Texto |Tipo de configuración para la retención anual ,por ejemplo, diariamente si se basa en día, semanalmente si se basa en semana |
| YearlyRetentionDaysOfTheMonth_s |Texto |Fechas del mes seleccionadas para la retención anual |
| SourceSystem |Texto |Sistema de origen de los datos actuales: Azure |
| ResourceId |Texto |Identificador de recursos de datos que se recopilan. Por ejemplo, el id. del recurso del almacén de Recovery Services. |
| SubscriptionId |Texto |Identificador de la suscripción del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos |
| ResourceGroup |Texto |Grupo de recursos del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos |
| ResourceProvider |Texto |Proveedor de recursos para el que se recopilan los datos. Por ejemplo, Microsoft.RecoveryServices. |
| ResourceType |Texto |Tipo de recurso para el que se recopilan los datos. Por ejemplo: almacenes |

### <a name="policyassociation"></a>PolicyAssociation
Esta tabla proporciona detalles acerca de las asociaciones de directivas con varias entidades.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| EventName_s |Texto |Este campo representa el nombre de este evento; es siempre AzureBackupCentralReport |
| SchemaVersion_s |Texto |Este campo indica la versión actual del esquema; es **V1** |
| State_s |Texto |Estado actual del objeto de la directiva, por ejemplo, Active o Deleted |
| BackupManagementType_s |Texto |Tipo de proveedor para el servidor que realiza el trabajo de copia de seguridad, por ejemplo, IaaSVM o FileFolder. |
| nombreOperación |Texto |Este campo representa el nombre de la operación actual: PolicyAssociation |
| Categoría |Texto |Este campo representa la categoría de datos de diagnóstico insertada en Log Analytics; es AzureBackupReport |
| Recurso |Texto |Este es el recurso para el que se están recopilando datos; muestra el nombre del almacén de Recovery Services |
| PolicyUniqueId_g |Texto |Identificador único que identifica la directiva |
| VaultUniqueId_s |Texto |Identificador exclusivo del almacén al que pertenece esta directiva |
| SourceSystem |Texto |Sistema de origen de los datos actuales: Azure |
| ResourceId |Texto |Identificador de recursos de datos que se recopilan. Por ejemplo, el id. del recurso del almacén de Recovery Services. |
| SubscriptionId |Texto |Identificador de la suscripción del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos |
| ResourceGroup |Texto |Grupo de recursos del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos |
| ResourceProvider |Texto |Proveedor de recursos para el que se recopilan los datos. Por ejemplo, Microsoft.RecoveryServices. |
| ResourceType |Texto |Tipo de recurso para el que se recopilan los datos. Por ejemplo: almacenes |

### <a name="protectedserver"></a>ProtectedServer
Esta tabla proporciona detalles acerca de los campos relacionados con el servidor protegido.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| EventName_s |Texto |Este campo representa el nombre de este evento; es siempre AzureBackupCentralReport |
| ProtectedServerName_s |Texto |Nombre del servidor protegido |
| SchemaVersion_s |Texto |Este campo indica la versión actual del esquema; es **V1** |
| State_s |Texto |Estado actual del objeto de servidor protegido, por ejemplo, Active o Deleted |
| BackupManagementType_s |Texto |Tipo de proveedor para el servidor que realiza el trabajo de copia de seguridad, por ejemplo, IaaSVM o FileFolder. |
| nombreOperación |Texto |Este campo representa el nombre de la operación actual: ProtectedServer |
| Categoría |Texto |Este campo representa la categoría de datos de diagnóstico insertada en Log Analytics; es AzureBackupReport |
| Recurso |Texto |Este es el recurso para el que se están recopilando datos; muestra el nombre del almacén de Recovery Services |
| ProtectedServerUniqueId_s |Texto |Identificador único del servidor protegido |
| RegisteredContainerId_s |Texto |Identificador de contenedor registrado para copia de seguridad |
| ProtectedServerType_s |Texto |Tipo de servidor protegido, por ejemplo, Windows. |
| ProtectedServerFriendlyName_s |Texto |Nombre descriptivo del servidor protegido |
| AzureBackupAgentVersion_s |Texto |Número de versión de Azure Backup Agent |
| SourceSystem |Texto |Sistema de origen de los datos actuales: Azure |
| ResourceId |Texto |Identificador de recursos de datos que se recopilan. Por ejemplo, el id. del recurso del almacén de Recovery Services. |
| SubscriptionId |Texto |Identificador de la suscripción del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos |
| ResourceGroup |Texto |Grupo de recursos del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos |
| ResourceProvider |Texto |Proveedor de recursos para el que se recopilan los datos. Por ejemplo, Microsoft.RecoveryServices. |
| ResourceType |Texto |Tipo de recurso para el que se recopilan los datos. Por ejemplo: almacenes |

### <a name="protectedserverassociation"></a>ProtectedServerAssociation
Esta tabla proporciona detalles acerca de las asociaciones de servidores protegidos con otras entidades.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| EventName_s |Texto |Este campo representa el nombre de este evento; es siempre AzureBackupCentralReport |
| SchemaVersion_s |Texto |Este campo indica la versión actual del esquema; es **V1** |
| State_s |Texto |Estado actual del objeto de asociación de servidores, por ejemplo, Active o Deleted |
| BackupManagementType_s |Texto |Tipo de proveedor para el servidor que realiza el trabajo de copia de seguridad, por ejemplo, IaaSVM o FileFolder. |
| nombreOperación |Texto |Este campo representa el nombre de la operación actual: ProtectedServerAssociation |
| Categoría |Texto |Este campo representa la categoría de datos de diagnóstico insertada en Log Analytics; es AzureBackupReport |
| Recurso |Texto |Este es el recurso para el que se están recopilando datos; muestra el nombre del almacén de Recovery Services |
| ProtectedServerUniqueId_s |Texto |Identificador único del servidor protegido |
| VaultUniqueId_s |Texto |Identificador exclusivo del almacén al que pertenece este servidor protegido |
| SourceSystem |Texto |Sistema de origen de los datos actuales: Azure |
| ResourceId |Texto |Identificador de recursos de datos que se recopilan. Por ejemplo, el id. del recurso del almacén de Recovery Services. |
| SubscriptionId |Texto |Identificador de la suscripción del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos |
| ResourceGroup |Texto |Grupo de recursos del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos |
| ResourceProvider |Texto |Proveedor de recursos para el que se recopilan los datos. Por ejemplo, Microsoft.RecoveryServices. |
| ResourceType |Texto |Tipo de recurso para el que se recopilan los datos. Por ejemplo: almacenes |

### <a name="storage"></a>Storage
Esta tabla proporciona detalles acerca de los campos relacionados con el almacenamiento.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| CloudStorageInBytes_s |Número decimal |Almacenamiento de copia de seguridad en la nube utilizado por las copias de seguridad, calculados basándose en el último valor |
| ProtectedInstances_s |Número decimal |Número de instancias protegidas que se utilizan para calcular el almacenamiento de front-end en la facturación; se calcula en función del valor más reciente |
| EventName_s |Texto |Este campo representa el nombre de este evento; es siempre AzureBackupCentralReport |
| SchemaVersion_s |Texto |Este campo indica la versión actual del esquema; es **V1** |
| State_s |Texto |Estado actual del objeto de almacenamiento, por ejemplo, Active o Deleted |
| BackupManagementType_s |Texto |Tipo de proveedor para el servidor que realiza el trabajo de copia de seguridad, por ejemplo, IaaSVM o FileFolder. |
| nombreOperación |Texto |Este campo representa el nombre de la operación actual: Storage |
| Categoría |Texto |Este campo representa la categoría de datos de diagnóstico insertada en Log Analytics; es AzureBackupReport |
| Recurso |Texto |Este es el recurso para el que se están recopilando datos; muestra el nombre del almacén de Recovery Services |
| ProtectedServerUniqueId_s |Texto |Id. exclusivo del servidor protegido para el que se calcula el almacenamiento |
| VaultUniqueId_s |Texto |Se calcula el identificador único del almacén para el almacenamiento |
| SourceSystem |Texto |Sistema de origen de los datos actuales: Azure |
| ResourceId |Texto |Identificador de recursos de datos que se recopilan. Por ejemplo, el id. del recurso del almacén de Recovery Services. |
| SubscriptionId |Texto |Identificador de la suscripción del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos |
| ResourceGroup |Texto |Grupo de recursos del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos |
| ResourceProvider |Texto |Proveedor de recursos para el que se recopilan los datos. Por ejemplo, Microsoft.RecoveryServices. |
| ResourceType |Texto |Tipo de recurso para el que se recopilan los datos. Por ejemplo: almacenes |

### <a name="vault"></a>Almacén
Esta tabla proporciona detalles acerca de los campos relacionados con el almacén.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| EventName_s |Texto |Este campo representa el nombre de este evento; es siempre AzureBackupCentralReport |
| SchemaVersion_s |Texto |Este campo indica la versión actual del esquema; es **V1** |
| State_s |Texto |Estado actual del objeto del almacén por ejemplo, Active o Deleted |
| nombreOperación |Texto |Este campo representa el nombre de la operación actual: Vault |
| Categoría |Texto |Este campo representa la categoría de datos de diagnóstico insertada en Log Analytics; es AzureBackupReport |
| Recurso |Texto |Este es el recurso para el que se están recopilando datos; muestra el nombre del almacén de Recovery Services |
| VaultUniqueId_s |Texto |Id. exclusivo del almacén |
| VaultName_s |Texto |Nombre del almacén |
| AzureDataCenter_s |Texto |Centro de datos donde se encuentra el almacén |
| StorageReplicationType_s |Texto |Tipo de replicación de almacenamiento para el almacén, por ejemplo, GeoRedundant |
| SourceSystem |Texto |Sistema de origen de los datos actuales: Azure |
| ResourceId |Texto |Identificador de recursos de datos que se recopilan. Por ejemplo, el id. del recurso del almacén de Recovery Services. |
| SubscriptionId |Texto |Identificador de la suscripción del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos |
| ResourceGroup |Texto |Grupo de recursos del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos |
| ResourceProvider |Texto |Proveedor de recursos para el que se recopilan los datos. Por ejemplo, Microsoft.RecoveryServices. |
| ResourceType |Texto |Tipo de recurso para el que se recopilan los datos. Por ejemplo: almacenes |

## <a name="next-steps"></a>Pasos siguientes
Cuando revise el modelo de datos para crear informes de Azure Backup, puede comenzar a [crear un panel](../azure-monitor/platform/dashboards.md) en Log Analytics.
