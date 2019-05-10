---
title: Usar los datos de Azure Workbench Blockchain con SQL Server Management Studio
description: Obtenga información sobre cómo conectarse a la base de datos de SQL de Azure Blockchain desde SQL Server Management Studio.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/09/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: db43dd04da601650b3592dc74cdd804fae394c00
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65510912"
---
# <a name="using-azure-blockchain-workbench-data-with-sql-server-management-studio"></a>Usar los datos de Azure Workbench Blockchain con SQL Server Management Studio

Microsoft SQL Server Management Studio proporciona la capacidad para escribir y probar consultas rápidamente en la base de datos de SQL de Azure Blockchain Workbench. En esta sección se incluye un tutorial paso a paso sobre cómo conectarse a la base de datos de SQL de Azure Blockchain desde SQL Server Management Studio.

## <a name="prerequisites"></a>Requisitos previos

* Descargue [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017).

## <a name="connecting-sql-server-management-studio-to-data-in-azure-blockchain-workbench"></a>Conectar SQL Server Management Studio a los datos de Azure Blockchain Workbench

1. Abra SQL Server Management Studio y seleccione **Conectar**.
2. Seleccione **Motor de base de datos**.

    ![Motor de base de datos](./media/data-sql-management-studio/database-engine.png)

3. En el cuadro de diálogo **Conectar al servidor**, escriba el nombre del servidor y las credenciales de la base de dataos.

    Si usa las credenciales creadas por el proceso de implementación de Azure Blockchain Workbench, el nombre de usuario es **dbadmin** y la contraseña es la que proporcionó durante la implementación.

    ![Especificar las credenciales de SQL](./media/data-sql-management-studio/sql-creds.png)

   1. SQL Server Management Studio muestra la lista y vistas de bases de datos, los procedimientos almacenados en la base de datos de Azure Blockchain Workbench.

      ![Lista de base de datos](./media/data-sql-management-studio/db-list.png)

5. Para ver los datos asociados a cualquiera de las vistas de base de datos, puede generar automáticamente una instrucción select mediante los siguientes pasos.
6. Haga clic con el botón derecho en cualquiera de las vistas de base de datos en el Explorador de objetos.
7. Seleccione **Incluir vista como**.
8. Elija **SELECT To**.
9. Seleccione **Nueva ventana del Editor de consultas**.
10. Una consulta nueva se puede crear al seleccionar **Nueva consulta**.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Vistas de base de datos de Azure Blockchain Workbench](database-views.md)
