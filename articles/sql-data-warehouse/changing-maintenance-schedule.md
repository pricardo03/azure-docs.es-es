---
title: Programaciones de mantenimiento de Azure (versión preliminar) | Microsoft Docs
description: La programación de mantenimiento permite a los clientes planear los eventos de mantenimiento necesarios que el servicio de Azure SQL Data Warehouse usa para implementar revisiones, actualizaciones y nuevas características.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 10/07/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: a6eedc0bac7aab69a9138f4f63d0d9d802e74dfc
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47228194"
---
# <a name="change-a-maintenance-schedule"></a>Cambiar una programación de mantenimiento 

## <a name="portal"></a>Portal
Una programación de mantenimiento puede actualizarse o cambiarse en cualquier momento. No obstante, si la instancia seleccionada está pasando actualmente por ciclos de mantenimiento activos, la configuración se guardará y solo se activará durante el siguiente período de mantenimiento identificado. [Obtenga más información](https://docs.microsoft.com/azure/service-health/resource-health-overview) sobre cómo supervisar el almacenamiento de datos durante un evento de mantenimiento activo. 

En la versión preliminar, le pediremos que seleccione dos ventanas de mantenimiento durante un período de 7 días. Cada ventana de mantenimiento puede tener entre 3 y 8 horas, siendo 3 horas la opción más corta disponible. El mantenimiento puede producirse en cualquier momento dentro de una ventana de mantenimiento identificada, pero no ocurrirá fuera de esas ventanas identificadas con una notificación previa. También experimentará una breve pérdida de conectividad mientras el servicio implementa código nuevo en el almacenamiento de datos. 

## <a name="identifying-the-primary-and-secondary-windows"></a>Identificar las ventanas principales y secundarias

Se deben identificar las ventanas principales y secundarias dentro de intervalos de días diferentes (es decir, la ventana principal {martes – jueves}, la ventana secundaria {sábado – domingo})

Complete los siguientes pasos para cambiar la programación de mantenimiento que se ha aplicado al almacenamiento de datos en Azure Portal.
1.  Inicie sesión en el [Azure Portal](https://portal.azure.com/).
2.  Seleccione el almacenamiento de datos que desea actualizar. Se abrirá la página en la hoja de información general. 
3.  Puede obtenerse acceso a la página de configuración Programación de mantenimiento haciendo clic en el vínculo de resumen Programación de mantenimiento (versión preliminar) en la hoja de información general o a través de la opción Programación de mantenimiento en el menú Recursos situado en el lado izquierdo.  

    ![Opciones de la hoja Información general](media/sql-data-warehouse-maintenance-scheduling/maintenance-change-option.png)

4. Los botones de selección situados en la parte superior de la página sirven para identificar el intervalo de día preferido para la ventana de mantenimiento principal. Esta selección determina si la ventana principal se producirá en un día laborable o durante el fin de semana. La selección actualizará los siguientes valores de la lista desplegable según corresponda. Durante la versión preliminar es posible que algunas regiones no admitan el conjunto completo de opciones de día disponibles. Estos valores se actualizarán en los próximos meses.

   ![Hoja Configuración de mantenimiento](media/sql-data-warehouse-maintenance-scheduling/maintenance-settings-page.png)

5. Elija a continuación sus ventanas de mantenimiento principal y secundaria preferidas con los menús desplegables Día, Hora de inicio y Período de tiempo. El resumen de la programación de la parte inferior de la hoja se actualizará según los valores seleccionados en la lista desplegable.

#### <a name="dropdown-options"></a>Opciones de la lista desplegable
- Día: día preferido para realizar el mantenimiento durante el período seleccionado.
- Hora de inicio: hora de inicio preferida de la ventana de mantenimiento.
- Período de tiempo: duración preferida del período de tiempo.

  Después de haber seleccionado las ventanas de mantenimiento preferidas, haga clic en Guardar. Aparecerá un mensaje de confirmación indicando que la nueva programación no está activa. Si va a guardar una programación en una región que aún no admite la programación de mantenimiento, aparecerá el siguiente mensaje. La configuración se guardará y se activará cuando la característica esté disponible en la región seleccionada.    

    ![No está activo en el aviso de región](media/sql-data-warehouse-maintenance-scheduling/maintenance-notactive-toast.png)

## <a name="next-steps"></a>Pasos siguientes
- [Obtenga más información](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) sobre las acciones de Webhook para reglas de alerta de registro.
- [Obtenga más información](https://docs.microsoft.com/azure/service-health/service-health-overview) acerca de Azure Service Health


