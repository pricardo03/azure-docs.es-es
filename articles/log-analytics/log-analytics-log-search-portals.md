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
ms.devlang: na
ms.topic: conceptual
ms.date: 08/20/2018
ms.author: magoedte; bwren
ms.component: na
ms.openlocfilehash: 386aad94461fa3f2ceafb7564342797eefa2f086
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2018
ms.locfileid: "40246703"
---
# <a name="viewing-and-analyzing-data-in-log-analytics"></a>Visualización y análisis de datos en Azure Log Analytics
Hay dos opciones disponibles en Azure Portal para analizar los datos almacenados en Log Analytics y para crear consultas de análisis ad hoc. Las consultas que se crean con estos portales pueden usarse para otras características, como las alertas y los paneles.

## <a name="log-analytics-page-preview"></a>Página Log Analytics (versión preliminar)
Abra la página Log Analytics desde **Registros (vista previa)** en el menú Log Analytics. Se trata de una nueva experiencia para trabajar con datos de registro y crear consultas. Puede obtener una introducción a este portal e inspeccionar sus características en [Introducción al portal de Analytics](query-language/get-started-analytics-portal.md).

La página Log Analytics proporciona las siguientes mejoras con respecto a la experiencia de [Búsqueda de registros](#log-search).

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

![Portal de análisis avanzado](media/log-analytics-log-search-portals/advanced-analytics-portal.png)


### <a name="firewall-requirements"></a>Requisitos de firewall
El explorador requiere acceso a las direcciones siguientes para obtener acceso a la página Log Analytics y al portal de análisis avanzado.  Si el explorador tiene acceso a Azure Portal a través de un firewall, debe habilitar el acceso a estas direcciones.

| Identificador URI | IP | Puertos |
|:---|:---|:---|
| portal.loganalytics.io | Dinámica | 80 443 |
| api.loganalytics.io    | Dinámica | 80 443 |
| docs.loganalytics.io   | Dinámica | 80 443 |


## <a name="log-search"></a>Búsqueda de registros
Abra la página Búsqueda de registros de **Registros** en el menú Log Analytics o desde **Log Analytics** en el menú Azure Monitor. Esto es adecuado para analizar datos de registro mediante consultas básicas. Proporciona varias características que le permiten editar consultas sin tener un conocimiento completo del lenguaje de consulta.  Puede ver un resumen de estas características en [Creación de búsquedas de registros en Azure Log Analytics mediante la opción de búsqueda de registros](log-analytics-log-search-log-search-portal.md). 


![Página de búsqueda de registros](media/log-analytics-log-search-portals/log-search-portal.png)


## <a name="next-steps"></a>Pasos siguientes

- Vea un [tutorial sobre el uso de la búsqueda de registros](log-analytics-tutorial-viewdata.md) para aprender a crear consultas mediante el lenguaje de consultas.
- Consulte una [lección sobre el uso del portal de análisis avanzado](query-language/get-started-analytics-portal.md), que proporciona la misma experiencia que la página Log Analytics.

