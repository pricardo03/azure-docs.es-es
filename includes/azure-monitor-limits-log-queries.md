---
title: archivo de inclusión
description: archivo de inclusión
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 07/22/2019
ms.author: bwren
ms.custom: include file
ms.openlocfilehash: ed840352096f1a1739bc8f655be42096456d3c33
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2019
ms.locfileid: "68405678"
---
| Límite | DESCRIPCIÓN |
|:---|:---|
| Lenguaje de consulta | Azure Monitor usa el mismo [lenguaje de consulta Kusto](/azure/kusto/query/) que Azure Data Explorer. Consulte las [diferencias del lenguaje de consulta de registros de Azure Monitor](../articles/azure-monitor/log-query/data-explorer-difference.md) para los elementos del lenguaje KQL que no se admiten en Azure Monitor. |
| Regiones de Azure | Las consultas de registro pueden experimentar una sobrecarga excesiva cuando los datos abarcan áreas de trabajo Log Analytics de varias regiones de Azure. Para más información, consulte [Límites de consulta](../articles/azure-monitor/log-query/scope.md#query-limits). |
