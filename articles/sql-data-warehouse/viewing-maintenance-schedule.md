---
title: Programaciones de mantenimiento de Azure (versión preliminar) | Microsoft Docs
description: La programación de mantenimiento permite a los clientes planear los eventos de mantenimiento necesarios que el servicio de Azure SQL Data Warehouse usa para implementar revisiones, actualizaciones y nuevas características.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 10/06/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: db5502b796fc345b2e2bf083f864d80bd59e7293
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "49427134"
---
# <a name="view-a-maintenance-schedule"></a>Vista de una programación de mantenimiento 

## <a name="portal"></a>Portal

De forma predeterminada, todas las instancias de Azure SQL Data Warehouse recién creadas tendrán una ventana de mantenimiento principal y secundaria de 8 horas aplicada durante la implementación. Puede cambiar las ventanas tan pronto como se complete la implementación. Sin notificación previa, no se llevará a cabo mantenimiento fuera de las ventanas de mantenimiento especificadas.

Para ver la programación de mantenimiento que se ha aplicado al almacenamiento de datos, complete los siguientes pasos:

1.  Inicie sesión en el [Azure Portal](https://portal.azure.com/).
2.  Seleccione el almacenamiento de datos que quiere ver. 
3.  El almacenamiento de datos seleccionado se abre en la hoja de información general. La programación de mantenimiento que se aplica al almacén de datos se mostrará debajo de **Programación de mantenimiento (versión preliminar)**.

![Hoja Información general](media/sql-data-warehouse-maintenance-scheduling/clear-overview-blade.PNG)

## <a name="next-steps"></a>Pasos siguientes
- [Obtenga más información](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-usage) sobre cómo crear, ver y administrar alertas mediante Azure Monitor.
- [Obtenga más información](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) sobre las acciones del webhook para las reglas de alerta de registro.
- [Obtenga más información](https://docs.microsoft.com/azure/service-health/service-health-overview) acerca de Azure Service Health.


