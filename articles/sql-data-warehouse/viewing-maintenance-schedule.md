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
ms.openlocfilehash: 6fbf914c8035344d33e8d2739fb9d92d5757c3d1
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47228164"
---
# <a name="viewing-a-maintenance-schedule"></a>Ver una programación de mantenimiento 

## <a name="portal"></a>Portal

De forma predeterminada, todas las instancias de Azure SQL Data Warehouse recién creadas tendrán una ventana de mantenimiento principal y secundaria de 8 horas definida por el sistema aplicada durante la implementación, que podrá editar tan pronto como se complete la implementación. Sin notificación previa, no se llevará a cabo mantenimiento fuera de las ventanas de mantenimiento especificadas.
Complete los siguientes pasos para ver la programación de mantenimiento que se ha aplicado al almacenamiento de datos en Azure Portal.

Complete los siguientes pasos para ver la programación de mantenimiento que se ha aplicado al almacenamiento de datos en Azure Portal.
1.  Inicie sesión en el [Azure Portal](https://portal.azure.com/).
2.  Seleccione el almacenamiento de datos que desea ver. 
3.  Se abrirá el Azure SQL Data Warehouse seleccionado en la hoja de información general. La programación de mantenimiento que se aplican al almacén de datos seleccionado se mostrará debajo de Programación de mantenimiento (versión preliminar).

![Hoja Información general](media/sql-data-warehouse-maintenance-scheduling/clear-overview-blade.PNG)

## <a name="next-steps"></a>Pasos siguientes
[Obtenga más información](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-usage) sobre cómo crear, ver y administrar alertas mediante Azure Monitor.
[Obtenga más información](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) sobre las acciones de Webhook para reglas de alerta de registro.
[Obtenga más información](https://docs.microsoft.com/azure/service-health/service-health-overview) acerca de Azure Service Health


