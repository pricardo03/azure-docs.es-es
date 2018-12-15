---
title: Visualización y análisis de datos en Azure Log Analytics | Microsoft Docs
description: En este artículo se describen los portales que puede utilizar en Azure Log Analytics para crear y editar búsquedas de registros.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: bwren
ms.openlocfilehash: 9a5472a6dfc944eb793e863704897c92b1a7572e
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53183359"
---
# <a name="viewing-and-analyzing-data-in-log-analytics"></a>Visualización y análisis de datos en Azure Log Analytics
Hay dos opciones disponibles en Azure Portal para analizar los datos almacenados en Log Analytics y para crear consultas de análisis ad hoc. Las consultas que se crean con estos portales pueden usarse para otras características, como las alertas y los paneles.

## <a name="log-analytics-page"></a>Página Log Analytics
Abra la página Log Analytics desde **Registros** en el menú Log Analytics. Se trata de una nueva experiencia para trabajar con datos de registro y crear consultas. Puede obtener una introducción a este portal e inspeccionar sus características en [Introducción al portal de Analytics](../../azure-monitor/log-query/get-started-portal.md).

La página Log Analytics proporciona las siguientes mejoras con respecto a la experiencia de [Búsqueda de registros (clásico)](#log-search-classic).

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
> La página Log Analytics tiene la misma funcionalidad que el portal de análisis avanzado, que es una herramienta externa de Azure Portal. El portal de análisis avanzado sigue estando disponible, pero los vínculos y otras referencias a él en Azure Portal se reemplazan por esta nueva página.

![Portal de análisis avanzado](media/portals/advanced-analytics-portal.png)

### <a name="resource-logs"></a>Registros del recurso
La nueva experiencia de Log Analytics se integra con distintos recursos de Azure, como Virtual Machines. Esto significa que puede abrir la página Log Analytics directamente a través del menú de supervisión del recurso sin cambiar a Azure Monitor o Log Analytics ni perder el contexto del recurso. Los **registros** todavía no se han habilitado para todos los recursos de Azure, pero empezarán a aparecer en el menú del portal para diferentes tipos de recursos.

Al abrir Log Analytics desde un recurso específico, se limita de forma automática a la escritura de registros de ese recurso únicamente.   Si quiere escribir una consulta que incluya otros registros, deberá abrirla desde el menú Log Analytics o Azure Monitor.

Las siguientes opciones aún no están disponibles a través de la vista de recursos de Log Analytics:

- Save
- Establecer alerta
- Explorador de consultas
- Cambio a diferentes áreas de trabajo o recursos (actualmente sin planear)


### <a name="firewall-requirements"></a>Requisitos de firewall
El explorador requiere acceso a las direcciones siguientes para obtener acceso a la página Log Analytics y al portal de análisis avanzado.  Si el explorador tiene acceso a Azure Portal a través de un firewall, debe habilitar el acceso a estas direcciones.

| Identificador URI | IP | Puertos |
|:---|:---|:---|
| portal.loganalytics.io | Dinámica | 80 443 |
| api.loganalytics.io    | Dinámica | 80 443 |
| docs.loganalytics.io   | Dinámica | 80 443 |


## <a name="log-search-classic"></a>Búsqueda de registros (clásico)
Abra la página Búsqueda de registros de **Registros (clásico)** en el menú Log Analytics o desde **Log Analytics** en el menú Azure Monitor. Esta es la página clásica utilizada para trabajar con consultas de Log Analytics que carece de las características adicionales de la [página Log Analytics](#log-analytics-page) mencionadas anteriormente.



![Página de búsqueda de registros](media/portals/log-search-portal.png)


## <a name="next-steps"></a>Pasos siguientes

- Vea un [tutorial sobre el uso de la búsqueda de registros](../../azure-monitor/learn/tutorial-viewdata.md) para aprender a crear consultas mediante el lenguaje de consultas.
- Consulte una [lección sobre el uso del portal de análisis avanzado](../../azure-monitor/log-query/get-started-portal.md), que proporciona la misma experiencia que la página Log Analytics.

