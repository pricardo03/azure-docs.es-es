---
title: Programaciones de mantenimiento de Azure (versión preliminar) | Microsoft Docs
description: La programación de mantenimiento permite a los clientes planear los eventos de mantenimiento programados necesarios que el servicio de Azure SQL Data Warehouse usa para implementar nuevas características, revisiones, actualizaciones.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: design
ms.date: 04/03/2019
ms.author: anvang
ms.reviewer: jrasnick
ms.openlocfilehash: 4bc1867f0dd18a101b2e764970de4e1904b960e0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60680443"
---
# <a name="change-a-maintenance-schedule"></a>Cambiar una programación de mantenimiento 

## <a name="portal"></a>Portal
Una programación de mantenimiento puede actualizarse o cambiarse en cualquier momento. Si la instancia seleccionada está en un ciclo de mantenimiento activo, se guardará la configuración. Se activará durante el siguiente período de mantenimiento identificado. [Obtenga más información](https://docs.microsoft.com/azure/service-health/resource-health-overview) sobre cómo supervisar el almacenamiento de datos durante un evento de mantenimiento activo. 

Para usar las programaciones de mantenimiento, deberá seleccionar dos ventanas de mantenimiento durante un período de siete días. Cada ventana de mantenimiento puede ser de tres a ocho horas. El mantenimiento puede realizarse en cualquier momento dentro de una ventana de mantenimiento, pero no se realizará fuera estos períodos de tiempo sin notificación previa. También experimentará una breve pérdida de conectividad a medida que el servicio implementa el nuevo código en el almacenamiento de datos. 

## <a name="identifying-the-primary-and-secondary-windows"></a>Identificar las ventanas principales y secundarias

Las ventanas principales y secundarias deben tener intervalos de días independientes. Un ejemplo es una ventana principal de martes a jueves, y otra secundaria de sábado a domingo.

Para cambiar la programación de mantenimiento del almacenamiento de datos, complete los siguientes pasos:
1.  Inicie sesión en el [Azure Portal](https://portal.azure.com/).
2.  Seleccione el almacenamiento de datos que quiere actualizar. La página se abre en la hoja de información general. 
3.  Abra la página de configuración de la programación de mantenimiento seleccionando el vínculo de **resumen de la Programación de mantenimiento (versión preliminar)** en la hoja de información general. O bien, seleccione la opción **Programación de mantenimiento** en el menú de recursos del lado izquierdo.  

    ![Opciones de la hoja Información general](media/sql-data-warehouse-maintenance-scheduling/maintenance-change-option.png)

4. Puede identificar el intervalo de día preferido para la ventana de mantenimiento principal con las opciones situadas en la parte superior de la página. Esta selección determina si la ventana principal se producirá en un día laborable o durante el fin de semana. La selección actualizará los siguientes valores de la lista desplegable. Durante la versión preliminar es posible que algunas regiones no admitan el conjunto completo de opciones de **día** disponibles.

   ![Hoja Configuración de mantenimiento](media/sql-data-warehouse-maintenance-scheduling/maintenance-settings-page.png)

5. Elija las ventanas de mantenimiento principales y secundarias que prefiera con el cuadro de lista desplegable:
   - **Día**: día preferido para realizar el mantenimiento durante el período seleccionado.
   - **Hora de inicio:** hora de inicio preferida de la ventana de mantenimiento.
   - **Período de tiempo**: duración preferida del período de tiempo.

   El área **Resumen de la programación** de la parte inferior de la hoja se actualiza según los valores que seleccione. 
  
6. Seleccione **Guardar**. Aparece un mensaje que confirma que la nueva programación está activa. 

   Si va a guardar una programación en una región que aún no admite la programación de mantenimiento, aparecerá el siguiente mensaje. La configuración se guarda y se activa cuando la característica esté disponible en la región seleccionada.    

   ![Mensaje sobre la disponibilidad de la región](media/sql-data-warehouse-maintenance-scheduling/maintenance-notactive-toast.png)

## <a name="next-steps"></a>Pasos siguientes
- [Obtenga más información](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) sobre las acciones del webhook para las reglas de alerta de registro.
- [Obtenga más información](https://docs.microsoft.com/azure/service-health/service-health-overview) acerca de Azure Service Health.


