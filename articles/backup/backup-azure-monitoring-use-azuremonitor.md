---
title: 'Azure Backup: Supervisión de Azure Backup con Azure Monitor'
description: Supervise las cargas de trabajo de Azure Backup y cree alertas personalizadas mediante Azure Monitor.
author: pvrk
manager: shivamg
keywords: Log Analytics; Azure Backup; Alertas; Configuración de diagnóstico; Grupos de acciones
ms.service: backup
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: pullabhk
ms.assetid: 01169af5-7eb0-4cb0-bbdb-c58ac71bf48b
ms.openlocfilehash: 15b701a9ccc469636875736b6e316c150615aa16
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68465943"
---
# <a name="monitor-at-scale-by-using-azure-monitor"></a>Supervisión a escala mediante Azure Monitor

Azure Backup proporciona [funcionalidades de supervisión y alerta integradas](backup-azure-monitoring-built-in-monitor.md) en un almacén de Recovery Services. Estas funcionalidades están disponibles sin ninguna infraestructura de administración adicional. Sin embargo, este servicio integrado está limitado en los escenarios siguientes:

- Si supervisa datos de varios almacenes de Recovery Services entre suscripciones
- Si el canal de notificación preferido *no* es el correo electrónico
- Si los usuarios desean alertas en más escenarios
- Si desea ver la información de un componente local, como System Center Data Protection Manager en Azure, que el portal no muestra en [**Trabajos de copia de seguridad**](backup-azure-monitoring-built-in-monitor.md#backup-jobs-in-recovery-services-vault) o [**Alertas de copia de seguridad**](backup-azure-monitoring-built-in-monitor.md#backup-alerts-in-recovery-services-vault)

## <a name="using-log-analytics-workspace"></a>Uso del área de trabajo de Log Analytics

> [!NOTE]
> Los datos de las copias de seguridad de VM de Azure, el agente de Azure Backup, System Center Data Protection Manager, las copias de seguridad de SQL de las VM de Azure y las copias de seguridad del recurso compartido de Azure Files se bombean al área de trabajo de Log Analytics a través de la configuración del diagnóstico. 

Para supervisar o informar a escala, necesita las funcionalidades de dos servicios de Azure. *Configuración de diagnóstico* envía datos de varios recursos de Azure Resource Manager a otro recurso. *Log Analytics* genera alertas personalizadas en las que puede usar grupos de acciones para definir otros canales de notificación. 

En las secciones siguientes se explica cómo usar Log Analytics para supervisar Azure Backup a escala.

### <a name="configure-diagnostic-settings"></a>Configuración de valores de diagnóstico

Recursos de Azure Resource Manager como, por ejemplo, el almacén de Recovery Services registra información acerca de las operaciones programadas y las operaciones desencadenadas por el usuario como datos de diagnóstico. 

En la sección de supervisión, seleccione **Configuración de diagnóstico** y especifique el destino de los datos de diagnóstico del almacén de Recovery Services.

![La configuración de diagnóstico del almacén de Recovery Services, dirigida a Log Analytics](media/backup-azure-monitoring-laworkspace/rs-vault-diagnostic-setting.png)

Puede dirigirse a un área de trabajo de Log Analytics desde otra suscripción. Para supervisar los almacenes de varias suscripciones en un solo lugar, seleccione el mismo área de trabajo de Log Analytics para varios almacenes de Recovery Services. Para canalizar toda la información sobre Azure Backup hacia el área de trabajo de Log Analytics, seleccione **AzureBackupReport** como registro.

> [!IMPORTANT]
> Una vez que finalice la configuración, debe esperar 24 horas para la finalización de la inserción de datos inicial. Tras dicha inserción de datos inicial, todos los datos se insertan tal como se describe posteriormente en este artículo, en la [sección sobre frecuencia](#diagnostic-data-update-frequency).

### <a name="deploy-a-solution-to-the-log-analytics-workspace"></a>Implementación de una solución en el área de trabajo de Log Analytics

> [!IMPORTANT]
> Lanzamos una [plantilla](https://azure.microsoft.com/resources/templates/101-backup-la-reporting/) actualizada y de varias vistas para la supervisión y los informes basados en LA en Azure Backup. Tenga en cuenta que los usuarios que usaban la [solución anterior](https://azure.microsoft.com/resources/templates/101-backup-oms-monitoring/) seguirán viéndola en sus áreas de trabajo incluso después de implementar la solución nueva. Sin embargo, es posible que la solución anterior brinde resultados inexactos debido a algunos cambios de esquema menores. Por lo tanto, los usuarios deben implementar la plantilla nueva.

Una vez que los datos estén en el área de Log Analytics, [implemente una plantilla de GitHub](https://azure.microsoft.com/resources/templates/101-backup-la-reporting/) en Log Analytics para visualizar los datos. Para identificar correctamente el área de trabajo, asegúrese de asignarle el mismo grupo de recursos, nombre de área de trabajo y ubicación de área de trabajo. A continuación, instale esta plantilla en el área de trabajo.

### <a name="view-azure-backup-data-by-using-log-analytics"></a>Visualización de los datos de Azure Backup mediante Log Analytics

Una vez implementada la plantilla, la solución para supervisar e informar en Azure Backup se mostrará en la región resumen del área de trabajo. Para ir al resumen, siga una de estas rutas de acceso:

- **Azure Monitor**: en la sección **Insights**, seleccione **Más** y, a continuación, elija el área de trabajo pertinente.
- **Áreas de trabajo de Log Analytics**: seleccione el área de trabajo pertinente y, a continuación, en **General**, seleccione **Resumen del área de trabajo**.

![Los iconos de supervisión e informes de Log Analytics](media/backup-azure-monitoring-laworkspace/la-azurebackup-overview-dashboard.png)

Cuando seleccione cualquiera de los iconos de información general, podrá ver más información. Estos son algunos de los informes que verá:

* Trabajos sin copia de seguridad de registros

   ![Gráficos de Log Analytics para los trabajos de copia de seguridad](media/backup-azure-monitoring-laworkspace/la-azurebackup-backupjobsnonlog.png)

* Alertas de copia de seguridad de los recursos de Azure

   ![Gráfico de Log Analytics para los trabajos de restauración](media/backup-azure-monitoring-laworkspace/la-azurebackup-alertsazure.png)

Del mismo modo, al hacer clic en los otros iconos, podrá ver informes sobre los trabajos de restauración, el almacenamiento en la nube, los elementos de copia de seguridad, las alertas de copia de seguridad de los recursos locales y los trabajos de copia de seguridad de registros.
 
Estos gráficos se proporcionan con la plantilla. Puede editar los gráficos o agregar más si es necesario.

### <a name="create-alerts-by-using-log-analytics"></a>Creación de alertas mediante Log Analytics

En Azure Monitor, puede crear sus propias alertas en un área de trabajo de Log Analytics. En el área de trabajo, usa *grupos de acciones de Azure* para seleccionar su mecanismo de notificación preferido. 

> [!IMPORTANT]
> Para obtener información sobre el costo que supone la creación de esta consulta, consulte [Precios de Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/).

Seleccione cualquiera de los gráficos para abrir la sección **Registros** del área de trabajo de Log Analytics. En la sección **Registros**, edite las consultas y cree alertas basadas en ellas.

![Creación de una alerta en un área de trabajo de Log Analytics](media/backup-azure-monitoring-laworkspace/la-azurebackup-customalerts.png)

Al seleccionar **Nueva regla de alertas**, se abre la página de creación de alertas de Azure Monitor, como se muestra en la siguiente imagen. El recurso ya está marcado aquí como área de trabajo de Log Analytics y se ha realizado la integración del grupos de acciones.

![Página de creación de alertas de Log Analytics](media/backup-azure-monitoring-laworkspace/inkedla-azurebackup-createalert.jpg)

#### <a name="alert-condition"></a>Condición de alerta

La característica que define una alerta es su condición desencadenadora. Seleccione **Condición** para cargar automáticamente la consulta de Kusto en la página **Registros**, como se muestra en la siguiente imagen. Aquí podrá editar la condición para satisfacer sus necesidades. Para obtener más información, consulte [Ejemplos de consultas de Kusto](#sample-kusto-queries).

![Configuración de una condición de alerta](media/backup-azure-monitoring-laworkspace/la-azurebackup-alertlogic.png)

Si es necesario, puede editar la consulta de Kusto. Elija un umbral, un período y una frecuencia. El umbral determina cuándo se generará la alerta. El período es el período de tiempo en el que se ejecuta la consulta. Por ejemplo, si el umbral es superior a 0, el período será de 5 minutos y, si la frecuencia equivale a ese tiempo, la regla ejecutará la consulta cada 5 minutos, lo que conlleva que se revisen los 5 minutos anteriores. Si el número de resultados es mayor que 0, se le notificará a través del grupo de acciones seleccionado.

#### <a name="alert-action-groups"></a>Grupos de acciones de alerta

Use un grupo de acciones para especificar un canal de notificación. Para ver los mecanismos de notificación disponibles, en **Grupos de acciones**, seleccione **Crear nuevo**.

![Mecanismos de notificación disponibles en la ventana "Agregar grupo de acciones"](media/backup-azure-monitoring-laworkspace/LA-AzureBackup-ActionGroup.png)

Puede satisfacer todos los requisitos de supervisión y alertas de Log Analytics solo, o puede usar Log Analytics para complementar las notificaciones integradas.

Para obtener más información, consulte [Creación, visualización y administración de alertas de registro mediante Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) y [Creación y administración de grupos de acciones en Azure Portal](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups).

### <a name="sample-kusto-queries"></a>Ejemplos de consultas de Kusto

Los gráficos predeterminados le proporcionan consultas de Kusto para los escenarios básicos en los que se pueden crear alertas. También puede modificar las consultas para obtener los datos de los que desee recibir alertas. Pegue los siguientes ejemplos de consultas de Kusto en la página **Registros** y, a continuación, cree alertas basadas en las consultas:

* Todos los trabajos de copia de seguridad con resultado satisfactorio

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | where OperationName == "Job" and JobOperation_s == "Backup"
    | where JobStatus_s == "Completed"
    ````
    
* Todos los trabajos de copia de seguridad con errores

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | where OperationName == "Job" and JobOperation_s == "Backup"
    | where JobStatus_s == "Failed"
    ````
    
* Todos los trabajos de copia de seguridad de máquinas virtuales de Azure con resultado satisfactorio

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | extend JobOperationSubType_s = columnifexists("JobOperationSubType_s", "")
    | where OperationName == "Job" and JobOperation_s == "Backup" and JobStatus_s == "Completed" and JobOperationSubType_s != "Log" and JobOperationSubType_s != "Recovery point_Log"
    | join kind=inner
    (
        AzureDiagnostics
        | where Category == "AzureBackupReport"
        | where OperationName == "BackupItem"
        | where SchemaVersion_s == "V2"
        | where BackupItemType_s == "VM" and BackupManagementType_s == "IaaSVM"
        | distinct BackupItemUniqueId_s, BackupItemFriendlyName_s
        | project BackupItemUniqueId_s , BackupItemFriendlyName_s
    )
    on BackupItemUniqueId_s
    | extend Vault= Resource
    | project-away Resource
    ````

* Todos los trabajos de copia de seguridad del registro de SQL con resultado satisfactorio

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | extend JobOperationSubType_s = columnifexists("JobOperationSubType_s", "")
    | where OperationName == "Job" and JobOperation_s == "Backup" and JobStatus_s == "Completed" and JobOperationSubType_s == "Log"
    | join kind=inner
    (
        AzureDiagnostics
        | where Category == "AzureBackupReport"
        | where OperationName == "BackupItem"
        | where SchemaVersion_s == "V2"
        | where BackupItemType_s == "SQLDataBase" and BackupManagementType_s == "AzureWorkload"
        | distinct BackupItemUniqueId_s, BackupItemFriendlyName_s
        | project BackupItemUniqueId_s , BackupItemFriendlyName_s
    )
    on BackupItemUniqueId_s
    | extend Vault= Resource
    | project-away Resource
    ````

* Todos los trabajos del agente de Azure Backup con resultado satisfactorio

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | extend JobOperationSubType_s = columnifexists("JobOperationSubType_s", "")
    | where OperationName == "Job" and JobOperation_s == "Backup" and JobStatus_s == "Completed" and JobOperationSubType_s != "Log" and JobOperationSubType_s != "Recovery point_Log"
    | join kind=inner
    (
        AzureDiagnostics
        | where Category == "AzureBackupReport"
        | where OperationName == "BackupItem"
        | where SchemaVersion_s == "V2"
        | where BackupItemType_s == "FileFolder" and BackupManagementType_s == "MAB"
        | distinct BackupItemUniqueId_s, BackupItemFriendlyName_s
        | project BackupItemUniqueId_s , BackupItemFriendlyName_s
    )
    on BackupItemUniqueId_s
    | extend Vault= Resource
    | project-away Resource
    ````

### <a name="diagnostic-data-update-frequency"></a>Frecuencia de actualización de los datos de diagnóstico

Los datos de diagnóstico del almacén se bombean al área de trabajo de Log Analytics con cierto retraso. Cada evento llega al área de trabajo de Log Analytics *entre 20 y 30 minutos* después de insertarse desde el almacén de Recovery Services. Estos son detalles adicionales sobre el retardo:

- En todas las soluciones, las alertas integradas del servicio de copia de seguridad se insertan en cuanto se crean. Así pues, suelen aparecer en el área de trabajo de Log Analytics tras haber transcurrido de 20 a 30 minutos.
- En todas las soluciones, los trabajos de copia de seguridad ad hoc y los trabajos de restauración se insertan en cuanto *finalizan*.
- En todas las soluciones salvo la copia de seguridad SQL, los trabajos de copia de seguridad programados se insertan en cuanto *finalizan*.
- En el caso de la copia de seguridad de SQL, como pueden realizarse copias de seguridad de registros cada 15 minutos, de todos los trabajos de copia de seguridad programados completados, incluidos los registros, la información se une en lotes y se inserta cada 6 horas.
- En todas las soluciones, el resto de información como el elemento de copia de seguridad, la directiva, los puntos de recuperación, el almacenamiento, etc. se inserta al menos *una vez al día.*
- Un cambio en la configuración de copia de seguridad (como por ejemplo un cambio de directiva o la edición de la directiva) desencadena la inserción de toda la información de copia de seguridad relacionada.

## <a name="using-the-recovery-services-vaults-activity-logs"></a>Uso de los registros de actividad del almacén de Recovery Services

> [!CAUTION]
> Los siguientes pasos se aplican solo a las *copias de seguridad de máquinas virtuales de Azure.* No se pueden utilizar estos pasos para soluciones como el agente de Azure Backup, las copias de seguridad de SQL dentro de Azure o Azure Files.

Los registros de actividad también se pueden usar para recibir notificaciones de eventos, como copias de seguridad correctas. Para comenzar, siga estos pasos:

1. Inicie sesión en Azure Portal.
1. Abra el almacén de Recovery Services pertinente. 
1. En las propiedades del almacén, abra la sección **Registro de actividad**.

Para identificar el registro adecuado y crear una alerta:

1. Compruebe que va a recibir registros de actividad por las copias de seguridad correctas aplicando los filtros que se muestran en la imagen siguiente. Cambie el valor **Intervalo de tiempo** según sea necesario para ver los registros.

   ![Filtrado para buscar los registros de actividad de las copias de seguridad de máquinas virtuales de Azure](media/backup-azure-monitoring-laworkspace/activitylogs-azurebackup-vmbackups.png)

1. Seleccione el nombre de la operación para ver los detalles pertinentes.
1. Seleccione **Nueva regla de alertas** para abrir la página **Crear regla**. 
1. Cree una alerta siguiendo los pasos en [Crear, ver y administrar las alertas del registro de actividad mediante Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log).

   ![Nueva alerta de reglas](media/backup-azure-monitoring-laworkspace/new-alert-rule.png)

Aquí, el recurso es el propio almacén de Recovery Services. Debe repetir los mismos pasos para todos los almacenes en los que desea recibir notificaciones a través de registros de actividad. La condición no tendrá un umbral, un período o una frecuencia, ya que esta alerta está basada en eventos. En cuanto se genera el registro de actividad pertinente, lo hace también la alerta.

## <a name="using-log-analytics-to-monitor-at-scale"></a>Uso de Log Analytics para supervisar a escala

Puede ver todas las alertas creadas a partir de registros de actividad y áreas de trabajo de Log Analytics en Azure Monitor. Solo tiene que abrir el panel **Alertas** situado a la izquierda.

Aunque puede recibir notificaciones a través de registros de actividad, recomendamos encarecidamente que se use Log Analytics en su lugar para supervisar a escala. Aquí se detallan los motivos:

- **Escenarios limitados**: las notificaciones a través de registros de actividad se aplican solo a las copias de seguridad de máquinas virtuales de Azure. Las notificaciones deben configurarse para todos los almacenes de Recovery Services.
- **Ajuste de definición**: la actividad de copia de seguridad programada no se ajusta a la definición más reciente de los registros de actividad. En su lugar, se alinea con los [registros de diagnóstico](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-overview#what-you-can-do-with-diagnostic-logs). Esta alineación da lugar a efectos inesperados al cambiar los datos que fluyen a través del canal del registro de actividad.
- **Problemas con el canal del registro de actividad**: en los almacenes de Recovery Services, los registros de actividad que se bombean desde Azure Backup siguen un nuevo modelo. Desafortunadamente, este cambio afecta a la generación de registros de actividad en Azure Government, Azure Alemania y Azure China 21Vianet. Si los usuarios de estos servicios en la nube crean o configuran cualquier alerta de los registros de actividad en Azure Monitor, no se desencadenarán las alertas. Además, en todas las regiones públicas de Azure, si un usuario [recopila los registros de actividad de Recovery Services en un área de trabajo de Log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/collect-activity-logs), estos registros no aparecerán.

Use un área de trabajo de Log Analytics para la supervisión y generación de alertas a escala en todas las cargas de trabajo que Azure Backup protege.

## <a name="next-steps"></a>Pasos siguientes

Para crear consultas personalizadas, consulte [Modelo de datos de Log Analytics](backup-azure-log-analytics-data-model.md).
