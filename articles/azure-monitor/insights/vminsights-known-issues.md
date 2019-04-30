---
title: Problemas conocidos de Azure Monitor para VM (versión preliminar) | Microsoft Docs
description: En este articulo se tratan problemas conocidos de Azure Monitor para VM, una solución en Azure que combina la supervisión del estado, la detección de dependencias de la aplicación y el rendimiento del sistema operativo de la máquina virtual de Azure.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/02/2019
ms.author: magoedte
ms.openlocfilehash: 5fdee4edce0301a5a4524725879f599b0a2e3532
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61386385"
---
# <a name="known-issues-with-azure-monitor-for-vms-preview"></a>Problemas conocidos con Azure Monitor para VM (versión preliminar)

En este articulo se tratan problemas conocidos de Azure Monitor para VM, una solución en Azure que combina la supervisión del estado, la detección de componentes de la aplicación y el rendimiento del sistema operativo de la máquina virtual de Azure. 

## <a name="health"></a>Health 
Los siguientes son problemas conocidos de la versión actual de la característica de Mantenimiento:

- El panel de propiedades de la máquina virtual muestra el sistema operativo Windows Server 2019 como Windows Server 2016. Este error se corregirá en una próxima versión.
- Si se quita o se elimina una máquina virtual de Azure, se muestra en la vista de lista de la máquina virtual durante un tiempo. Además, si hace clic en el estado de una máquina virtual que se ha eliminado, se abre la vista **Diagnóstico de mantenimiento** y, a continuación, pasa a un bucle de carga. Si selecciona el nombre de una máquina virtual eliminada, se abre un panel con un mensaje que indica que la máquina virtual se ha eliminado.
- Los cambios de configuración, como la actualización de un umbral, tardan aproximadamente 30 minutos incluso aunque el portal o en la API Workload Monitor se actualicen inmediatamente. 
- El Diagnóstico de mantenimiento experimenta las actualizaciones más rápido que cualquier otra vista. La información podría retrasarse al cambiar entre ellas. 
- Para las máquinas virtuales de Linux, el título de la página que enumera los criterios de mantenimiento de una vista única de la máquina virtual tiene el nombre de dominio completo de la máquina virtual en lugar del nombre de máquina virtual definido por el usuario. 
- Después de deshabilitar la supervisión de una máquina virtual mediante los métodos admitidos e intentar implementarla de nuevo, debería implementarla en la misma área de trabajo. Si elige una nueva área de trabajo e intenta ver el estado de mantenimiento para esa máquina virtual, es posible que muestre comportamientos incoherentes.
- El criterio de mantenimiento de utilización de la CPU total para Windows muestra un umbral de *no igual a* **4**, lo que significa que la utilización de la CPU es mayor que el 95 % y la longitud de la cola del sistema es mayor que 15. Este criterio de mantenimiento no es configurable en esta versión preliminar.  
- Después de quitar los componentes de la solución del área de trabajo, puede continuar viendo el estado de mantenimiento de las máquinas virtuales de Azure; específicamente, datos de rendimiento y de mapa cuando se desplaza a cualquiera de las dos vistas en el portal. Eventualmente, los datos dejarán de aparecer en la vista de rendimiento y mapa después de algún tiempo; sin embargo, la vista de mantenimiento continuará mostrando el estado de mantenimiento de las máquinas virtuales. La opción **Probar ahora** estará disponible para repetir la incorporación solo desde las vistas Rendimiento y Mapa.

## <a name="next-steps"></a>Pasos siguientes
Para conocer los requisitos y los métodos para habilitar la supervisión de máquinas virtuales, consulte [Implementación de Azure Monitor para VM](vminsights-onboard.md).
