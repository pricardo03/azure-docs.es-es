---
title: Portales para la creación y edición de consultas de registros en Azure Log Analytics | Microsoft Docs
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
ms.date: 06/11/2018
ms.author: magoedte; bwren
ms.component: na
ms.openlocfilehash: e2ea0bf1fb3f1c63f4e6f037e465e8fdfd9a4374
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37133493"
---
# <a name="portals-for-creating-and-editing-log-queries-in-azure-log-analytics"></a>Portales para la creación y edición de consultas de registros en Azure Log Analytics

Puede usar búsquedas de registros en diversos lugares de Azure Log Analytics para recuperar datos del área de trabajo.  Para crear y editar realmente consultas además de trabajar interactivamente con los datos devueltos, tiene dos opciones que se describen a continuación.  

## <a name="log-search"></a>Búsqueda de registros 
La página de búsqueda de registros es accesible desde Azure Portal.  Es adecuado para la creación de consultas básicas que se pueden crear en una sola línea.  La búsqueda de registros se puede ejecutar sin necesidad de iniciar un portal externo y se puede usar para realizar una serie de funciones con búsquedas de registros, incluyendo la creación de reglas de alertas, la creación de grupos de equipos y la exportación de los resultados de la consulta.  

La opción de búsqueda de registros proporciona varias características que le permitirán editar la consulta sin tener un conocimiento completo del lenguaje de consulta.  Puede ver un resumen de estas características en [Creación de búsquedas de registros en Azure Log Analytics mediante la opción de búsqueda de registros](log-analytics-log-search-log-search-portal.md).


![Página de búsqueda de registros](media/log-analytics-log-search-portals/log-search-portal.png)

## <a name="advanced-analytics-portal"></a>Portal de análisis avanzado
El portal de análisis avanzados es un portal dedicado que proporciona funcionalidades avanzadas que no están disponibles en la búsqueda de registros de Azure Portal.  Las características incluyen la capacidad para editar una consulta en varias líneas, ejecutar código de forma selectiva, Intellisense sensible al contexto y análisis inteligente.  El portal de análisis avanzado es más adecuado para diseñar consultas más complejas que se guardan como una búsqueda de registros o que se copian y pegan en otros elementos de Log Analytics.  Inicie el portal de análisis avanzados desde un vínculo de la misma página de búsqueda de registros.

![Portal de análisis avanzado](media/log-analytics-log-search-portals/advanced-analytics-portal.png)


Debido a sus características avanzadas, normalmente usará el portal de análisis avanzado como la herramienta principal para crear y editar consultas.  Una vez que haya determinado que la consulta funciona según lo previsto, cópiela y péguela en otra parte como, por ejemplo, la página de búsqueda de registros o el Diseñador de vistas.  

### <a name="firewall-requirements"></a>Requisitos de firewall
El explorador requiere acceso a las direcciones siguientes para obtener acceso al portal de Log Analytics.  Si el explorador tiene acceso a Azure Portal a través de un firewall, debe habilitar el acceso a estas direcciones.

| Identificador URI | IP | Puertos |
|:---|:---|:---|
| portal.loganalytics.io | Dinámica | 80 443 |
| api.loganalytics.io    | Dinámica | 80 443 |
| docs.loganalytics.io   | Dinámica | 80 443 |


## <a name="next-steps"></a>Pasos siguientes

- Vea un tutorial sobre el uso de la [búsqueda de registros](log-analytics-tutorial-viewdata.md) para aprender a crear consultas mediante el lenguaje de consultas.
- Consulte el [portal de análisis avanzados](https://go.microsoft.com/fwlink/?linkid=856587) para crear sofisticadas consultas y usarlas como entorno de desarrollo de las búsquedas de registros.

