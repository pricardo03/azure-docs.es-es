---
title: Azure Monitor para las asignaciones de región de contenedores
description: En este artículo se describen las asignaciones de región compatibles entre Azure Monitor para contenedores, el área de trabajo de Log Analytics y las métricas personalizadas.
services: azure-monitor
ms.service: azure-monitor
ms.workload: infrastructure-services
author: mgoedtel
ms.author: magoedte
ms.date: 06/26/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: ca77277c8d8d405b8fe81f612a8d7600d3c937ab
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72388179"
---
# <a name="region-mappings-supported-by-azure-monitor-for-containers"></a>Asignaciones de región compatibles con Azure Monitor para contenedores

 Cuando se habilita Azure Monitor para los contenedores, solo ciertas regiones son compatibles para vincular un área de trabajo de Log Analytics y un clúster de AKS y recopilar las métricas personalizadas enviadas a Azure Monitor.

## <a name="log-analytics-workspace-supported-mappings"></a>Asignaciones admitidas por el área de trabajo de Log Analytics

Los recursos de clúster de AKS o el área de trabajo de Log Analytics pueden encontrarse en otras regiones. En la tabla siguiente se muestran las asignaciones.

|**Región de clúster de AKS** | **Región del área de trabajo de Log Analytics** |
|-----------------------|------------------------------------|
|**África** | |
|SouthAfricaNorth |WestEurope |
|SouthAfricaWest |WestEurope |
|**Australia** | |
|AustraliaEast |AustraliaEast |
|AustraliaCentral |AustraliaCentral |
|AustraliaCentral2 |AustraliaCentral |
|AustraliaEast |AustraliaEast |
|**Asia Pacífico** | |
|EastAsia |EastAsia |
|SoutheastAsia |SoutheastAsia |
|**Brasil** | |
|BrazilSouth | SouthCentralUS |
|**Canadá** ||
|CanadaCentral |CanadaCentral |
|CanadaEast |CanadaCentral |
|**Europa** | |
|FranceCentral |FranceCentral |
|FranceSouth |FranceCentral |
|NorthEurope |NorthEurope |
|UKSouth |UKSouth |
|UKWest |UKSouth |
|WestEurope |WestEurope |
|**India** | |
|CentralIndia |CentralIndia |
|SouthIndia |CentralIndia |
|WestIndia |CentralIndia |
|**Japón** | |
|JapanEast |JapanEast |
|JapanWest |JapanEast |
|**Corea** | |
|KoreaCentral |KoreaCentral |
|KoreaSouth |KoreaCentral |
|**EE. UU.** | |
|CentralUS |CentralUS|
|EastUS |EastUS |
|EastUS2 |EastUS2 |
|WestUS |WestUS |
|WestUS2 |WestUS2 |
|WestCentralUS<sup>1</sup>|EastUS<sup>1</sup>|
|Gobierno de EE. UU. - Virginia |Gobierno de EE. UU. - Virginia |

<sup>1</sup> Debido a restricciones de capacidad, la región no está disponible cuando se crean recursos nuevos. Esto incluye un área de trabajo de Log Analytics. Sin embargo, los recursos vinculados preexistentes en la región deberían seguir funcionando.

## <a name="custom-metrics-supported-regions"></a>Regiones compatibles con las métricas personalizadas

Las métricas recopiladas de los pods y los nodos de clústeres de Azure Kubernetes Service (AKS) se admiten para publicar como métricas personalizadas solo en las [regiones de Azure](../platform/metrics-custom-overview.md#supported-regions) siguientes.

## <a name="next-steps"></a>Pasos siguientes

Para comenzar a supervisar el clúster de AKS, revise [Cómo habilitar Azure Monitor para contenedores](container-insights-onboard.md), a fin de conocer los requisitos y los métodos disponibles para habilitar la supervisión.  
