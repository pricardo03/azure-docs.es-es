---
title: Programaciones de mantenimiento de Azure (versión preliminar) | Microsoft Docs
description: La programación de mantenimiento permite a los clientes planear los eventos de mantenimiento necesarios que el servicio de Azure SQL Data Warehouse usa para implementar revisiones, actualizaciones y nuevas características.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 09/20/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: 85e8327d1cac17059b2e91b1ea21becc23002c8e
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47228280"
---
# <a name="using-maintenance-schedules-to-manage-service-updates-and-maintenance"></a>Uso de programaciones de mantenimiento para administrar las actualizaciones del servicio y el mantenimiento

Programación de mantenimiento de Azure SQL Data Warehouse está ahora en versión preliminar. Esta nueva característica integra perfectamente las notificaciones de mantenimiento planeado de Service Health, el monitor de comprobación de Resource Health y el servicio de programación de mantenimiento de Azure SQL Data Warehouse.

Programación de mantenimiento le permite programar un período de tiempo cuando sea conveniente recibir las nuevas características, actualizaciones y revisiones. Los clientes seleccionarán una ventana de mantenimiento principal y una secundaria dentro de un período de 7 días, es decir, del sábado a las 22:00 al domingo a la 01:00 (principal) y desde el miércoles a las 19:00 hasta las 22:00 (secundaria). Si no se puede realizar el mantenimiento durante la ventana de mantenimiento principal, se intentará nuevamente durante la ventana de mantenimiento secundaria.

Todos las instancias de Azure SQL Data Warehouse recién creadas tendrán una programación de mantenimiento definida por el sistema aplicada durante la implementación. La programación se podrá editar tan pronto como se complete la implementación.

Cada ventana de mantenimiento puede tener entre 3 y 8 horas, siendo 3 horas la opción más corta disponible actualmente. El mantenimiento puede producirse en cualquier momento dentro de la ventana y debe esperar una breve pérdida de conectividad ya que el servicio implementa código nuevo en el almacenamiento de datos. 

Durante la versión preliminar de la característica estamos pidiendo a los clientes que identifiquen sus ventanas principales y secundarias dentro de los intervalos de días diferentes.   
Todas las operaciones de mantenimiento deben realizarse dentro de las ventanas de mantenimiento programadas y no se llevará a cabo mantenimiento fuera de las ventanas de mantenimiento especificadas sin notificación previa. Si el almacenamiento de datos se pausa durante un período de mantenimiento programado, se actualizará durante la operación de reanudación.  


## <a name="alerts-and-monitoring"></a>Supervisión y alertas

Una integración sin problemas con las notificaciones de Service Health y el monitor de comprobación de Recurse Health permite a los clientes mantenerse informados de la actividad de mantenimiento inminente. La nueva automatización aprovecha Azure Monitor y permite a los clientes determinar cómo desean recibir notificaciones de eventos de mantenimiento inminentes y qué flujos automatizados deben activarse para administrar el tiempo de inactividad y minimizar el impacto en sus operaciones.

Todos los eventos de mantenimiento van precedidos de una notificación con 24 horas de antelación. Para minimizar el tiempo de inactividad de la instancia, debe asegurarse de que no hay transacciones de larga ejecución en el almacenamiento de datos antes del comienzo del período de mantenimiento seleccionado. Cuando se inicia el mantenimiento se cancelan todas las sesiones activas, se revierten las transacciones no confirmadas y el almacenamiento de datos experimenta una breve pérdida de conectividad. Se le notificará inmediatamente después de que se haya completado el mantenimiento en el almacenamiento de datos. 

Si recibió una notificación anticipada indicándole que se llevará a cabo el mantenimiento, pero no se puede realizar el mantenimiento durante ese tiempo, recibirá una notificación de cancelación. Después se reanudará el mantenimiento durante el siguiente período de mantenimiento programado.
 
Todos los eventos de mantenimiento activos se mostrarán en la sección “Service Health: mantenimiento planeado”. Se conserva un registro completo de eventos anteriores como parte del historial de Service Health. El mantenimiento puede supervisarse mediante el panel del portal de comprobación de Azure Service Health durante un evento activo.

### <a name="maintenance-schedule-availability"></a>Disponibilidad de la programación de mantenimiento

Aunque Programación de mantenimiento no esté aún disponible en la región seleccionada, puede ver y editar la programación de mantenimiento en cualquier momento. Cuando Programación de mantenimiento esté disponible en su región, la programación identificada pasará a estar activa inmediatamente en el almacenamiento de datos.


## <a name="next-steps"></a>Pasos siguientes

- [Obtenga más información](viewing-maintenance-schedule.md) sobre cómo ver una programación de mantenimiento 
- [Obtenga más información](changing-maintenance-schedule.md) sobre cómo cambiar una programación de mantenimiento
- [Obtenga más información](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-usage) sobre cómo crear, ver y administrar alertas mediante Azure Monitor
- [Obtenga más información](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) sobre las acciones de Webhook para reglas de alerta de registro
- [Obtenga más información](https://docs.microsoft.com/azure/service-health/service-health-overview) acerca de Azure Service Health







