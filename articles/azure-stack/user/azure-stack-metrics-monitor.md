---
title: Consumo de datos de supervisión de Azure Stack | Microsoft Docs
description: Obtenga más información sobre las opciones para consumir datos de supervisión de Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/14/2018
ms.author: mabrigg
ms.openlocfilehash: b6196ec434d00a6fbc6714095fa4182ede98ce91
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2018
ms.locfileid: "45633442"
---
# <a name="how-to-consume-monitoring-data-from-azure-stack"></a>Consumo de datos de supervisión de Azure Stack

*Se aplica a: sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

Con la canalización de Azure Monitor, puede buscar los datos de supervisión en un único lugar, al igual que ocurre con Azure Monitor en Azure global. Sin embargo, no todos los datos de supervisión de Azure global están disponibles en Azure Stack. En este artículo, se resumen los distintos mecanismos para ingerir datos de supervisión del servicio mediante programación.
 
## <a name="options-for-data-consumption"></a>Opciones para el consumo de datos

| Tipo de datos | Categoría | Servicios admitidos | Métodos de acceso |
|-------------------------------------------------------------|----------|------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| Métricas en el nivel de plataforma de Azure Monitor | Métricas | [Métricas de Azure Stack compatibles con Azure Monitor](azure-stack-metrics-supported.md) | API DE REST |
| Cálculo de métricas del sistema operativo invitado (por ejemplo, el recuento de rendimiento) | Métricas | Máquinas virtuales Windows y Linux | Tabla o blob de almacenamiento:<br>Azure Diagnostics para Windows o Linux <br>Centro de eventos:<br>Windows Azure Diagnostics |
| Métricas de almacenamiento | Métricas | Azure Storage | Tabla de almacenamiento:<br>Storage Analytics |
| Registro de actividades | Eventos | Todos los servicios de Azure | API REST:<br>Azure Monitor Event API |
| Cálculo de los registros del sistema operativo invitado (por ejemplo, IIS, ETW, syslogs) | Eventos | Máquinas virtuales Windows y Linux | Tabla o blob de almacenamiento:<br>Azure Diagnostics para Windows o Linux <br>Centro de eventos:<br>Windows Azure Diagnostics |
| Registros de almacenamiento | Eventos | Azure Storage | Tabla de almacenamiento:<br>Storage Analytics |

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre [Azure Monitor en Azure Stack](azure-stack-metrics-azure-data.md).
