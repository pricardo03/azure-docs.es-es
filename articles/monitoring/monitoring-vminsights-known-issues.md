---
title: Problemas conocidos de Azure Monitor para VM | Microsoft Docs
description: Azure Monitor para VM es una solución de Azure que combina la supervisión de rendimiento y de mantenimiento del sistema operativo de la VM de Azure, así como la detección automática de las dependencias y los componentes de aplicación con otros recursos, y asigna la comunicación entre ellos. En este artículo se trata los problemas conocidos.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/18/2018
ms.author: magoedte
ms.openlocfilehash: c03adc239ea7025fe154db315daa17b26f8237f1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46980226"
---
# <a name="known-issues-with-azure-monitor-for-vms"></a>Problemas conocidos con Azure Monitor para VM

Los siguientes son problemas conocidos de la característica de Mantenimiento de Azure Monitor para VM:

- El período y la frecuencia de los criterios de mantenimiento no se puede modificar con esta versión. 
- No se pueden deshabilitar los criterios de mantenimiento. 
- Después de la incorporación, los datos pueden tardar en mostrarse en Azure Monitor -> Máquinas virtuales-> Mantenimiento o desde la hoja de recursos de VM -> Insights
- El diagnósticos de mantenimiento experimenta actualizaciones más rápido que cualquier otra vista, por lo que puede experimentar retrasos al cambiar entre vistas.  
- El resumen de alertas que se proporciona en Azure Monitor para el mantenimiento de VM corresponde solo a alertas activadas para problemas de mantenimiento detectados con VM de Azure supervisadas.
- La página de visa **Lista de alertas** en la VM propia y en Azure Monitor muestra alertas cuya condición de supervisión está establecida en "activada" en los últimos 30 días.  No pueden configurarse. Sin embargo, después de hacer clic en el resumen, una vez que se inicia la página de vista **Lista de alertas**, puede cambiar el valor de filtro tanto de la condición de supervisión como del intervalo de tiempo.
- En la página de vista **Lista de alertas**, se recomienda no modificar los filtros **Tipo de recurso**, **Recursos** y **Supervisar servicio**, ya que se han configurado de manera específica para la solución (esta vista de lista muestra algunos campos adicionales en comparación con la vista de lista Azure Monitor -> Alertas).    
- En VM Linux, la vista **Diagnósticos de estado** tiene el nombre de dominio completo de la VM, en lugar del nombre de VM definido por el usuario.
- Apagar las VM actualizará algunos de los criterios de mantenimiento a un estado crítico y otros a un estado correcto con estado neto de la VM en un estado crítico.
- No se puede modificar la gravedad de la alerta de mantenimiento, solo se habilitar o deshabilitar.  Además, algunas gravedades se actualizan según el estado de los criterios de mantenimiento.
- Modificar cualquier configuración de una instancia de criterio de mantenimiento dará lugar a una modificación de la misma configuración en todas las instancias de criterio de mantenimiento del mismo tipo en la VM. Por ejemplo, si el umbral de la instancia de criterio de mantenimiento del espacio libre en el disco correspondiente al disco lógico C: se modifica, este umbral se aplicará a todos los demás discos lógicos detectados y supervisados para la misma VM.   
- Los umbrales para algunos criterios de mantenimiento de Windows, como el estado del servicio cliente DNS, no se pueden modificar, ya que su estado correcto ya está bloqueada para el estado **en ejecución**, **disponible** del servicio o la entidad, según el contexto.  En su lugar, el valor se representa por el número 4, se convertirá en la cadena de presentación real en una versión futura.  
- Los umbrales para algunos criterios de estado de Linux no se pueden modificar, como el estado del disco lógico, ya que ya están establecidos para desencadenarse en un estado incorrecto.  Estos indican si algo está en línea o sin conexión, o activado o desactivado, y se representan e indican al mostrar el valor 1 o 0.
- Actualizar el filtro Grupo de recursos en cualquier grupo de recursos al usar la visita de lista a escala Azure Monitor -> Máquinas virtuales -> Mantenimiento -> Cualquier con la suscripción y el grupo de recursos preseleccionados hará que la vista de lista no muestre **ningún resultado**.  Vuelva a la pestaña Azure Monitor -> Máquinas virtuales -> Mantenimiento y seleccione la suscripción y el grupo de recursos deseados y, a continuación, vaya a la vista de lista.
