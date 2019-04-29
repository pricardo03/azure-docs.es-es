---
title: Visualización y análisis de datos de registro en Azure Monitor | Microsoft Docs
description: En este artículo se describe el uso de Log Analytics en Azure Portal para crear y editar consultas de registros en Azure Monitor.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/05/2019
ms.author: bwren
ms.openlocfilehash: 0e5b9b43e528b37fd994f9131f145abadb33c53b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61425951"
---
# <a name="viewing-and-analyzing-log-data-in-azure-monitor"></a>Visualización y análisis de datos de registro en Azure Monitor
Log Analytics es la principal experiencia para trabajar con datos de registro y crear consultas en Azure Monitor. Abra Log Analytics desde **Registros** en el menú **Azure Monitor**. Puede obtener una introducción a este portal e inspeccionar sus características en [Introducción a Log Analytics en Azure Portal](get-started-portal.md).

Log Analytics proporciona las siguientes características para trabajar con consultas de registros.

* Varias pestañas: cree pestañas independientes para trabajar con varias consultas.
* Visualizaciones enriquecidas: variedad de opciones de gráficos.
* Intellisense y relleno automático de lenguaje mejorados.
* Resaltado de sintaxis: mejora la legibilidad de las consultas. 
* Explorador de consultas: consultas y funciones guardadas por acceso.
* Vista de esquema: revise la estructura de los datos para ayudar a escribir las consultas.
* Uso compartido: cree vínculos a las consultas, ancle las consultas a cualquier panel compartido de Azure.
* Análisis inteligente: identifica los picos en los gráficos y un análisis rápido de la causa.
* Selección de columnas: ordene y agrupe las columnas en los resultados de la consulta.

> [!NOTE]
> Log Analytics tiene la misma funcionalidad que el portal de análisis avanzado, que es una herramienta externa de Azure Portal. El portal de análisis avanzado sigue estando disponible, pero los vínculos y otras referencias a él en Azure Portal se reemplazan por esta nueva página.

![Log Analytics](media/portals/log-analytics.png)

## <a name="resource-logs"></a>Registros del recurso
Log Analytics se integra con distintos recursos de Azure, como Virtual Machines. Esto significa que puede abrir Log Analytics directamente a través del menú de supervisión del recurso sin cambiar a Azure Monitor ni perder el contexto del recurso. Los **registros** todavía no se han habilitado para todos los recursos de Azure, pero empezarán a aparecer en el menú del portal para diferentes tipos de recursos.

Al abrir Log Analytics desde un recurso específico, se limita de forma automática a la escritura de registros de ese recurso únicamente.   Si quiere escribir una consulta que incluya otros registros, deberá abrirla desde el menú Azure Monitor.

Las siguientes opciones aún no están disponibles a través de la vista de recursos de Log Analytics:

- Save
- Establecer alerta
- Explorador de consultas
- Cambio a diferentes áreas de trabajo o recursos (actualmente sin planear)


## <a name="firewall-requirements"></a>Requisitos de firewall
El explorador requiere acceso a las direcciones siguientes para obtener acceso a Log Analytics.  Si el explorador tiene acceso a Azure Portal a través de un firewall, debe habilitar el acceso a estas direcciones.

| Identificador URI | IP | Puertos |
|:---|:---|:---|
| portal.loganalytics.io | Dinámica | 80 443 |
| api.loganalytics.io    | Dinámica | 80 443 |
| docs.loganalytics.io   | Dinámica | 80 443 |


## <a name="next-steps"></a>Pasos siguientes

- Recorra un [tutorial con Log Analytics](../../azure-monitor/log-query/get-started-portal.md).
- Recorra un [tutorial con Log Analytics](../../azure-monitor/learn/tutorial-viewdata.md).

