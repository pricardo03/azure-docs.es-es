---
title: Administración de instancias de alertas
description: Administración de instancias de alertas en Azure
author: anantr
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: anantr
ms.subservice: alerts
ms.openlocfilehash: 0324fffdfaaa512c3610d5389d68dba2164f8d55
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54436457"
---
# <a name="manage-alert-instances"></a>Administración de instancias de alertas
Con la [experiencia de alertas unificadas](https://aka.ms/azure-alerts-overview) en Azure Monitor, ahora puede ver todos los tipos de alertas en Azure que abarcan varias suscripciones, en un solo panel de datos. En este artículo se explica cómo puede ver sus instancias de alerta y cómo realizar una inmersión profunda en el portal para encontrar instancias de alerta específicas para la solución de problemas.

1. Hay tres formas de llegar a la página Alertas:

   + En [Azure Portal](https://portal.azure.com/), seleccione **Supervisar** y, en la sección Supervisar, elija **Alertas**.  
    ![Supervisión](media/alerts-managing-alert-instances/monitoring-alerts-managing-alert-instances-toc.jpg)
  
   + Puede ir a la sección Alertas desde el contexto de un **recurso** determinado. Una vez que se abra un recurso, explore la tabla de contenido y vaya a la sección Supervisión; elija **Alertas** y asegúrese de que la página de inicio esté filtrada previamente para recibir alertas sobre ese recurso específico.
   
    ![Supervisión](media/alerts-managing-alert-instances/alert-resource.JPG)
    
   + Puede ir a la sección Alertas desde el contexto de un **grupo de recursos** determinado. Una vez abierto el grupo de recursos, explore la tabla de contenido y vaya a la sección Supervisión; elija **Alertas** y asegúrese de que la página de inicio esté filtrada previamente para recibir alertas sobre ese grupo de recursos específico.    
   
    ![Supervisión](media/alerts-managing-alert-instances/alert-rg.JPG)

1.  Llegará a la página **Resumen de alertas**, que le proporciona información general de todas sus instancias de alerta en Azure. Puede modificar la vista de resumen seleccionando **varias suscripciones** (un máximo de 5) o filtrando por **grupos de recursos**, ya sean **recursos** o **intervalos de tiempo** específicos. Haga clic en Alertas totales o en cualquiera de las bandas de gravedad para ir a la vista de lista de sus alertas.     
    ![Resumen de alertas](media/alerts-managing-alert-instances/alerts-summary.jpg)
 
1.  Llegará a la página **Todas las alertas**, donde verá todas las instancias de alerta enumeradas en Azure. Si accede al portal desde una notificación de alerta, puede usar los filtros disponibles para limitar esa instancia de alerta específica. (**Nota**: si llegó a la página haciendo clic en cualquiera de las bandas de gravedad, la lista se filtrará previamente para esa gravedad en cuanto acceda a la página). Además de los filtros disponibles en la página anterior, ahora también puede realizar procesos de filtrado en función del servicio de supervisión (por ejemplo, Plataforma para métricas), la condición de la supervisión (activada o resuelta), la gravedad, el estado de las alertas (nuevo/confirmado/cerrado), o el identificador de grupo inteligente.

    ![Todas las alertas](media/alerts-managing-alert-instances/all-alerts.jpg)

    > [!NOTE]
    >  Si llegó a la página haciendo clic en cualquiera de las bandas de gravedad, la lista se filtrará previamente para esa gravedad en cuanto acceda a la página.
 
1.  Al hacer clic en cualquier instancia de alerta, se abre la página **Detalles de alerta**, lo que le permite profundizar en la información sobre esa instancia de alerta específica.   
![Detalles de alertas](media/alerts-managing-alert-instances/alert-details.jpg)  

