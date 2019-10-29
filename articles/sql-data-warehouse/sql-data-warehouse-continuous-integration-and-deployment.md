---
title: Integración e implementación continuas | Microsoft Docs
description: Experiencia de DevOps de bases de datos de clase empresarial para SQL Data Warehouse con compatibilidad integrada para la integración e implementación continuas mediante Azure Pipelines.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: overview
ms.subservice: integration
ms.date: 08/28/2019
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 7afb616fffaf01b1efa4f5d5a4af9dddd9683781
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2019
ms.locfileid: "70143851"
---
# <a name="continuous-integration-and-deployment-for-azure-sql-data-warehouse"></a>Integración e implementación continuas para Azure SQL Data Warehouse

En este sencillo tutorial se describe cómo integrar un proyecto de base de datos de SQL Server Data Tools (SSDT) con Azure DevOps y aprovechar Azure Pipelines para configurar la integración e implementación continuas. Este tutorial es el segundo paso en la creación de la canalización de la integración e implementación continuas con SQL Data Warehouse. 

## <a name="before-you-begin"></a>Antes de empezar

- Realice el [tutorial de integración del control de código fuente](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-source-control-integration).

- Cree un [agente autohospedado](https://docs.microsoft.com/azure/devops/pipelines/agents/agents?view=azure-devops#install) que tenga instalados los bits de la versión preliminar de SSDT (16.3 versión preliminar 2 o posterior) para SQL Data Warehouse (versión preliminar).

- Configuración y conexión con Azure DevOps

  > [!NOTE]
  > SSDT está actualmente en versión preliminar y tendrá que aprovechar un agente autohospedado. Los agentes hospedados por Microsoft se actualizarán en los próximos meses.

## <a name="continuous-integration-with-visual-studio-build"></a>Integración continua con la compilación de Visual Studio

1. Vaya a Azure Pipelines y cree una nueva canalización de compilación.

      ![Nueva canalización](media/sql-data-warehouse-continuous-integration-and-deployment/1-new-build-pipeline.png "Nueva canalización")

2. Seleccione el repositorio de código fuente (Git de Azure Repos) y elija la plantilla de aplicaciones Escritorio de .NET.

      ![Configuración de la canalización](media/sql-data-warehouse-continuous-integration-and-deployment/2-pipeline-setup.png "Configuración de la canalización") 

3. Edite el archivo YAML para usar el grupo adecuado del agente. El archivo YAML debe tener un aspecto similar al siguiente:

      ![YAML](media/sql-data-warehouse-continuous-integration-and-deployment/3-yaml-file.png "YAML")

En este punto, tiene un entorno simple en el que cualquier inserción en la rama maestra del repositorio de control de código fuente debe desencadenar automáticamente una compilación correcta en Visual Studio del proyecto de base de datos. Realice un cambio en el proyecto de base de datos local e insértelo en la rama maestra para asegurarse de que la automatización funciona de principio a fin.


## <a name="continuous-deployment-with-the-azure-sql-database-deployment-task"></a>Implementación continua con la tarea de implementación de Azure SQL Database

1. Agregue una nueva tarea mediante la [tarea de implementación de Azure SQL Database](https://docs.microsoft.com/azure/devops/pipelines/tasks/deploy/sql-azure-dacpac-deployment?view=azure-devops) y rellene los campos necesarios para conectarse al almacenamiento de datos de destino. Cuando se ejecuta esta tarea, el paquete de aplicación de capa de datos generado a partir del proceso de compilación anterior se implementa en el almacenamiento de datos de destino.

      ![Tarea de implementación](media/sql-data-warehouse-continuous-integration-and-deployment/4-deployment-task.png "Tarea de implementación")

2. Si usa un agente autohospedado, asegúrese de establecer la variable de entorno de modo que use el archivo SqlPackage.exe correcto para SQL Data Warehouse. La ruta de acceso debe tener un aspecto similar al siguiente:

      ![Variable de entorno](media/sql-data-warehouse-continuous-integration-and-deployment/5-environment-variable-preview.png "Variable de entorno")

   C:\Archivos de programa (x86)\Microsoft Visual Studio\2019\Preview\Common7\IDE\Extensions\Microsoft\SQLDB\DAC\150  

   Ejecute y valide la canalización. Puede realizar cambios localmente e insertarlos en el repositorio de control de código fuente; deberían generar una compilación e implementación automáticas.

## <a name="next-steps"></a>Pasos siguientes

- Explore [Arquitectura de Azure SQL Data Warehouse](/azure/sql-data-warehouse/massively-parallel-processing-mpp-architecture).
- [Cree rápidamente una instancia de SQL Data Warehouse][create a SQL Data Warehouse].
- [Cargue datos de ejemplo][load sample data].
- Explore [vídeos](/azure/sql-data-warehouse/sql-data-warehouse-videos).



<!--Image references-->

[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Create a support ticket]: ./sql-data-warehouse-get-started-create-support-ticket.md
[load sample data]: ./sql-data-warehouse-load-sample-databases.md
[create a SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md
[Migration documentation]: ./sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[Integrated tools overview]: ./sql-data-warehouse-overview-integrate.md
[Backup and restore overview]: ./sql-data-warehouse-restore-database-overview.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Customer Advisory Team blogs]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Feature requests]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN forum]: https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureSQLDataWarehouse
[Stack Overflow forum]: https://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videos]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[SLA for SQL Data Warehouse]: https://azure.microsoft.com/support/legal/sla/sql-data-warehouse/v1_0/
[Volume Licensing]: https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[Service Level Agreements]: https://azure.microsoft.com/support/legal/sla/
