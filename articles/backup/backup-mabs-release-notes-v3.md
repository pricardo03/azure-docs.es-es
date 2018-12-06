---
title: Notas de la versión de Microsoft Azure Backup Server v3
description: En este artículo se proporciona la información sobre los problemas conocidos y las soluciones alternativas para MABS v3.
services: backup
author: JYOTHIRMAISURI
manager: vvithal
ms.service: backup
ms.topic: conceptual
ms.date: 11/22/2018
ms.author: v-jysur
ms.asset: 0c4127f2-d936-48ef-b430-a9198e425d81
ms.openlocfilehash: 1d077fc8a853f145d271fa69d453ea2ea70ab696
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "52319084"
---
# <a name="release-notes-for-microsoft-azure-backup-server"></a>Notas de la versión de Microsoft Azure Backup Server
Este artículo describe los problemas conocidos y las soluciones alternativas para Microsoft Azure Backup Server (MABS) v3.

##  <a name="backup-and-recovery-fails-for-clustered-workloads"></a>Errores de copia de seguridad y recuperación para cargas de trabajo agrupadas

**Descripción:** Errores de copia de seguridad y restauración de orígenes de datos agrupados como un clúster de Hyper-V, un clúster de SQL (SQL Always On) o Exchange en un grupo de disponibilidad de base de datos (DAG) después de actualizar MABS v2 a MABS v3.

**Solución alternativa:** Para evitar estos errores, abra SQL Server Management Studio (SSMS) y ejecute el siguiente script SQL en la BD de DPM:


    IF EXISTS (SELECT * FROM dbo.sysobjects
        WHERE id = OBJECT_ID(N'[dbo].[tbl_PRM_DatasourceLastActiveServerMap]')
        AND OBJECTPROPERTY(id, N'IsUserTable') = 1)
        DROP TABLE [dbo].[tbl_PRM_DatasourceLastActiveServerMap]
        GO

        CREATE TABLE [dbo].[tbl_PRM_DatasourceLastActiveServerMap] (
            [DatasourceId]          [GUID]          NOT NULL,
            [ActiveNode]            [nvarchar](256) NULL,
            [IsGCed]                [bit]           NOT NULL
            ) ON [PRIMARY]
        GO

        ALTER TABLE [dbo].[tbl_PRM_DatasourceLastActiveServerMap] ADD
    CONSTRAINT [pk__tbl_PRM_DatasourceLastActiveServerMap__DatasourceId] PRIMARY KEY NONCLUSTERED
        (
            [DatasourceId]
        )  ON [PRIMARY],

    CONSTRAINT [DF_tbl_PRM_DatasourceLastActiveServerMap_IsGCed] DEFAULT
        (
            0
        ) FOR [IsGCed]
    GO


##  <a name="upgrade-to-mabs-v3-fails-in-russian-locale"></a>Error en la actualización a MABS v3 en la configuración regional en ruso

**Descripción:** Error al actualizar de MABS v2 a MABS v3 en la configuración regional en ruso con el código **4387**.

**Solución alternativa:** Realice los pasos siguientes para actualizar a MABS v3 con el paquete de instalación de ruso:

1.  [Realice una copia de seguridad](https://docs.microsoft.com/sql/relational-databases/backup-restore/create-a-full-database-backup-sql-server?view=sql-server-2017#SSMSProcedure) de su base de datos SQL y desinstale MABS v2 (seleccione conservar los datos protegidos durante la desinstalación).
2.  Actualice a SQL 2017 (Enterprise) y desinstale Reporting como parte de la actualización.
3. [Instale](https://docs.microsoft.com/sql/reporting-services/install-windows/install-reporting-services?view=sql-server-2017#install-your-report-server) SQL Server Reporting Services (SSRS).
4.  [Instale](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017#ssms-installation-tips-and-issues-ssms-1791) SQL Server Management Studio (SSMS).
5.  Configure Reporting mediante los parámetros tal como se describe en [Configuración de SSRS con SQL 2017](https://docs.microsoft.com/azure/backup/backup-azure-microsoft-azure-backup#upgrade-mabs).
6.  [Instale](backup-azure-microsoft-azure-backup.md) MABS v3.
7. [Restaure](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-database-backup-using-ssms?view=sql-server-2017) SQL con SSMS y ejecute la herramienta DPM-Sync tal como se describe [aquí](https://docs.microsoft.com/it-it/previous-versions/system-center/data-protection-manager-2010/ff634215(v=technet.10)).
8.  Actualice la propiedad “DataBaseVersion” en la tabla dbo.tbl_DLS_GlobalSetting con el comando siguiente:

        UPDATE dbo.tbl_DLS_GlobalSetting
        set PropertyValue = '13.0.415.0'
        where PropertyName = 'DatabaseVersion'


9.  Inicie el servicio MSDPM.


## <a name="next-steps"></a>Pasos siguientes

[Novedades de MABS v3](backup-mabs-whats-new-mabs.md)
