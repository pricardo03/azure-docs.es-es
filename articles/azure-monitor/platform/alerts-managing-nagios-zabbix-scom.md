---
title: Administración de alertas de otros servicios de supervisión en Azure Monitor
description: Administración de alertas de Nagios, Zabbix y SCOM en Azure Monitor
author: anantr
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: anantr
ms.component: alerts
ms.openlocfilehash: bc597d42fe89c0e03c4af1db3a935031b9043a98
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53345848"
---
# <a name="manage-alerts-from-other-monitoring-services"></a>Administración de alertas de otros servicios de supervisión

Ahora puede ver las alertas de Nagios, Zabbix y System Center Operations Manager en la [experiencia unificada de alertas](https://aka.ms/azure-alerts-overview). Estas alertas proceden de integraciones con servidores Nagios o Zabbix o con System Center Operations Manager en Log Analytics. 

## <a name="prerequisites"></a>Requisitos previos
Los registros del repositorio de Log Analytics con un tipo de alerta se importarán a la experiencia unificada de alertas, por lo que debe realizar las configuraciones que sean necesarias para recopilar estos registros.
1. Para las alertas de **Nagios** y **Zabbix**, [configure esos servidores](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents) para que envíen alertas a Log Analytics.
1. Para las alertas de **System Center Operations Manager**,[conecte el grupo de administración de Operations Manager al área de trabajo de Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-om-agents). Las alertas creadas en System Center Operations Manager se importan en Log Analytics.

## <a name="view-your-alert-instances"></a>Visualización de las instancias de alertas
Una vez que haya configurado la importación en Log Analytics, puede empezar a ver instancias de alertas de estos servicios de supervisión en la [experiencia unificada de alertas](https://aka.ms/azure-alerts-overview). Una vez que están presentes en la experiencia unificada de alertas, podrá [administrar las instancias de alertas](https://aka.ms/managing-alert-instances), [administrar los grupos inteligentes creados en estas alertas](https://aka.ms/managing-smart-groups) y [cambiar el estado de las alertas y de los grupos inteligentes](https://aka.ms/managing-alert-smart-group-states).

> [!NOTE]
>  Las alertas de Nagios en la experiencia unificada de alertas no tienen estado; por ejemplo, la [condición de supervisión](https://aka.ms/azure-alerts-overview) de una alerta no pasará de "Activada" a "Resuelta". En su lugar, "Activada" y "Resuelta" se muestran como instancias independientes de la alerta. 
