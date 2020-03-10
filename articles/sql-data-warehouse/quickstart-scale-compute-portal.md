---
title: 'Inicio rápido: escalado de procesos: Azure Portal '
description: Escale proceso en el grupo de SQL en Azure Portal. Escale horizontalmente un proceso para aumentar el rendimiento, o bien revierta la escalabilidad del proceso para ahorrar costos.
services: sql-data-warehouse
author: Antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.subservice: implement
ms.date: 04/17/2018
ms.author: anvang
ms.reviewer: jrasnick
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 7463849223fdf81466237c7d0c912763988e80e6
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/29/2020
ms.locfileid: "78200364"
---
# <a name="quickstart-scale-compute-in-azure-synapse-analytics-sql-pool-in-the-azure-portal"></a>Inicio rápido: Escalado de proceso en el grupo de SQL de Azure Synapse Analytics en Azure Portal

Escale proceso en el grupo de SQL en Azure Portal. [Escale horizontalmente un proceso](sql-data-warehouse-manage-compute-overview.md) para aumentar el rendimiento, o bien revierta la escalabilidad del proceso para ahorrar costos. 

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en [Azure Portal](https://portal.azure.com/).

## <a name="before-you-begin"></a>Antes de empezar

Puede escalar un grupo de SQL que ya tenga o usar el [inicio rápido para la creación y conexión en el portal](create-data-warehouse-portal.md) para crear un grupo de SQL llamado **mySampleDataWarehouse**.  En esta guía de inicio rápido se escala **mySampleDataWarehouse**.

>[!Note]
>El grupo de SQL debe estar en línea para que se pueda escalar. 

## <a name="scale-compute"></a>Escalado de proceso

Los recursos de proceso del grupo de SQL se pueden escalar mediante el aumento o la reducción de las unidades de almacenamiento de datos. En el inicio rápido Creación y conexión: portal (create-data-warehouse-portal.md), creó **mySampleDataWarehouse** y lo inició con 400 DWU. En los siguientes pasos se ajustan las DWU para **mySampleDataWarehouse**.

Para cambiar las unidades de almacenamiento de datos:

1. Haga clic en **Azure Synapse Analytics (formerly SQL DW)** a la izquierda de la página de Azure Portal.
2. Seleccione **mySampleDataWarehouse** en la página de **Azure Synapse Analytics (formerly SQL DW)** . Se abre el grupo de SQL.
3. Haga clic en **Escalar**.

    ![Haga clic en Escala](media/quickstart-scale-compute-portal/click-scale.png)

2. En el panel Escalar, mueva el control deslizante hacia la izquierda o la derecha para cambiar el valor de DWU. A continuación, seleccione Escalar.

    ![Mueva el control deslizante](media/quickstart-scale-compute-portal/scale-dwu.png)

## <a name="next-steps"></a>Pasos siguientes
Ahora ha aprendido a escalar proceso del grupo de SQL. Para más información sobre el grupo de SQL, continúe con el tutorial sobre la carga de datos.

> [!div class="nextstepaction"]
>[Carga de datos en un grupo de SQL](load-data-from-azure-blob-storage-using-polybase.md)
