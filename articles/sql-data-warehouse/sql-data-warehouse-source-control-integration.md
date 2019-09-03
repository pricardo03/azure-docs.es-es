---
title: Integración del control de código fuente | Microsoft Docs
description: Experiencia de DevOps de bases de datos de clase empresarial para SQL Data Warehouse con integración de control de código fuente nativa mediante Azure Repos (Git y GitHub).
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: overview
ms.subservice: integration
ms.date: 08/23/2019
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 10a10882efe05ef9e6bb86e54fcfcf8c5d73d225
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70098743"
---
# <a name="source-control-integration-for-azure-sql-data-warehouse"></a>Integración del control de código fuente para Azure SQL Data Warehouse

En este tutorial se describe cómo integrar el proyecto de base de datos de SQL Server Data Tools (SSDT) con control de código fuente.  La integración del control de código fuente es el primer paso en la creación de la implementación continua y la canalización de implementación con SQL Data Warehouse. 

## <a name="before-you-begin"></a>Antes de empezar

- Regístrese como [organización de Azure DevOps](https://azure.microsoft.com/services/devops/)
- Realice el tutorial de [creación y conexión](https://docs.microsoft.com/azure/sql-data-warehouse/create-data-warehouse-portal)
-  [Instale Visual Studio 2019](https://visualstudio.microsoft.com/vs/older-downloads/) 

## <a name="set-up-and-connect-to-azure-devops"></a>Configuración y conexión con Azure DevOps

1. En su organización de Azure DevOps, cree un proyecto que hospedará su proyecto de base de datos de SSDT mediante un repositorio de Azure Repos

   ![Crear proyecto](media/sql-data-warehouse-source-control-integration/1-create-project-azure-devops.png "Create Project")

2. Abra Visual Studio y conéctese a su organización y proyecto de Azure DevOps desde el paso 1 seleccionando “Administrar conexiones”

   ![Administrar conexiones](media/sql-data-warehouse-source-control-integration/2-manage-connections.png "Manage Connections")

   ![Conectar](media/sql-data-warehouse-source-control-integration/3-connect.png "Connect")

3. Clone el repositorio de Azure Repos del proyecto en la máquina local

   ![Clonar repositorio](media/sql-data-warehouse-source-control-integration/4-clone-repo.png "Clone repo")

## <a name="create-and-connect-your-project"></a>Creación y conexión del proyecto

1. En Visual Studio, cree un nuevo proyecto de base de datos de SQL Server con un directorio y un repositorio de Git local en el **repositorio local clonado**

   ![Crear nuevo proyecto](media/sql-data-warehouse-source-control-integration/5-create-new-project.png "Create new project")  

2. Haga clic con el botón derecho en el sqlproject vacío e importe el almacenamiento de datos en el proyecto de base de datos

   ![Importar proyecto](media/sql-data-warehouse-source-control-integration/6-import-new-project.png "Import Project")  

3. En Team Explorer en Visual Studio, confirme todos los cambios en el repositorio local de Git 

   ![Confirmar](media/sql-data-warehouse-source-control-integration/6.5-commit-push-changes.png "Commit")  

4. Ahora que ha confirmado los cambios localmente en el repositorio clonado, sincronice e inserte los cambios en el repositorio de Azure Repos en el proyecto de Azure DevOps.

   ![Sincronizar e insertar: almacenamiento provisional](media/sql-data-warehouse-source-control-integration/7-commit-push-changes.png "Sync and push - staging")

   ![Sincronizar e insertar](media/sql-data-warehouse-source-control-integration/7.5-commit-push-changes.png "Sync and push")  

## <a name="validation"></a>Validación

1. Compruebe que los cambios se han insertado en el repositorio de Azure Repos mediante la actualización de una columna de tabla del proyecto de base de datos desde Visual Studio SQL Server Data Tools (SSDT)

   ![Validar actualización de columna](media/sql-data-warehouse-source-control-integration/8-validation-update-column.png "Validate update column")

2. Confirme e inserte el cambio del repositorio local en el repositorio de Azure Repos

   ![Insertar cambios](media/sql-data-warehouse-source-control-integration/9-push-column-change.png "Push changes")

3. Compruebe que el cambio se ha insertado en el repositorio de Azure Repos

   ![Comprobar](media/sql-data-warehouse-source-control-integration/10-verify-column-change-pushed.png "Comprobar cambios")

4. (**Opcional**) Use la comparación de esquemas y actualice los cambios en el almacenamiento de datos de destino con SSDT para asegurarse de que las definiciones de objetos en el repositorio de Azure Repos y el repositorio local reflejan el almacenamiento de datos

## <a name="next-steps"></a>Pasos siguientes

- [Desarrollo para Azure SQL Data Warehouse](sql-data-warehouse-overview-develop.md)

<!--Image references-->

<!--Article references-->


<!--MSDN references-->

<!--Other Web references-->

