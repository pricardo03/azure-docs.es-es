---
title: 'Inicio rápido: Escalabilidad horizontal de un proceso de Azure SQL Data Warehouse (Azure Portal) | Microsoft Docs'
description: Escale un proceso en Azure SQL Data Warehouse en Azure Portal. Escale horizontalmente un proceso para aumentar el rendimiento, o bien revierta la escalabilidad del proceso para ahorrar costos.
services: sql-data-warehouse
author: Antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.subservice: implement
ms.date: 04/17/2018
ms.author: anvang
ms.reviewer: jrasnick
ms.openlocfilehash: 4197d7a4724199f8c42fc09a6d1ed18af46cc4f8
ms.sourcegitcommit: 5ded08785546f4a687c2f76b2b871bbe802e7dae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/19/2019
ms.locfileid: "69575368"
---
# <a name="quickstart-scale-compute-in-azure-sql-data-warehouse-in-the-azure-portal"></a>Inicio rápido: Escalado de un proceso de Azure SQL Data Warehouse en Azure Portal

Escale un proceso en Azure SQL Data Warehouse en Azure Portal. [Escale horizontalmente un proceso](sql-data-warehouse-manage-compute-overview.md) para aumentar el rendimiento, o bien revierta la escalabilidad del proceso para ahorrar costos. 

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Azure Portal](https://portal.azure.com/).

## <a name="before-you-begin"></a>Antes de empezar

Puede escalar un almacenamiento de datos que ya tenga o consultar [Guía de inicio rápido: Creación de una instancia de Azure SQL Data Warehouse en Azure Portal, y realización de consultas en ella](create-data-warehouse-portal.md) para crear un almacenamiento de datos llamado **mySampleDataWarehouse**.  En esta guía de inicio rápido se escala **mySampleDataWarehouse**.

>[!Note]
>Este almacenamiento de datos debe estar conectado para escalarse. 

## <a name="scale-compute"></a>Escalado de proceso

Los recursos de proceso de SQL Data Warehouse se pueden escalar aumentando o reduciendo las unidades de almacenamiento de datos. En el inicio rápido Creación y conexión: portal (create-data-warehouse-portal.md), creó **mySampleDataWarehouse** y lo inició con 400 DWU. En los siguientes pasos se ajustan las DWU para **mySampleDataWarehouse**.

Para cambiar las unidades de almacenamiento de datos:

1. En la página izquierda de Azure Portal, haga clic en **Almacenes de SQL Data Warehouse**.
2. Seleccione **mySampleDataWarehouse** en la página **Almacenes de SQL Data Warehouse**. Se abre el almacenamiento de datos.
3. Haga clic en **Escalar**.

    ![Haga clic en Escala](media/quickstart-scale-compute-portal/click-scale.png)

2. En el panel Escalar, mueva el control deslizante hacia la izquierda o la derecha para cambiar el valor de DWU.

    ![Mueva el control deslizante](media/quickstart-scale-compute-portal/scale-dwu.png)

3. Haga clic en **Save**(Guardar). Aparece un mensaje de confirmación. Haga clic en **Sí** para confirmar o **No** para cancelar.

    ![Haga clic en Guardar](media/quickstart-scale-compute-portal/confirm-change.png)



## <a name="next-steps"></a>Pasos siguientes
Ya ha aprendido a escalar procesos para el almacenamiento de datos. Para más información sobre Azure SQL Data Warehouse, siga el tutorial para cargar los datos.

> [!div class="nextstepaction"]
>[Carga de datos en una instancia de SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md)
