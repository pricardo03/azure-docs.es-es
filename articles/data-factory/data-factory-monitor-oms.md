---
title: Supervisión de Azure Data Factory con OMS | Microsoft Docs
description: Aprenda a supervisar Azure Data Factory mediante el enrutamiento de datos Operations Management Suite (OMS) para el análisis.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/20/2018
ms.author: douglasl
ms.openlocfilehash: fb5da75d0e42dd0700cad008d348ff846d602409
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2018
ms.locfileid: "42143766"
---
# <a name="monitor-azure-data-factory-with-operations-management-suite-oms"></a>Supervisión de Azure Data Factory con Operations Management Suite (OMS)

Puede usar la integración de Azure Data Factory con Azure Monitor para enrutar datos a Operations Management Suite (OMS). Esta integración resulta útil en los escenarios siguientes:

1.  Quiere escribir consultas complejas en un amplio conjunto de métricas que se publican mediante Data Factory en OMS. También puede crear alertas personalizadas sobre estas consultas a través de OMS.

2.  Quiere realizar la supervisión entre fábricas de datos. Puede enrutar datos desde varias fábricas de datos a una sola área de trabajo de OMS.

Si desea una demostración y una introducción de siete minutos de esta característica, vea el siguiente vídeo:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Monitor-Data-Factory-pipelines-using-Operations-Management-Suite-OMS/player]

## <a name="get-started"></a>Introducción

### <a name="configure-diagnostic-settings-and-oms-workspace"></a>Configuración de las opciones de diagnóstico y el área de trabajo de OMS

Habilite la configuración de diagnóstico de su fábrica de datos.

1.  Seleccione **Azure Monitor** -> **Configuración de diagnóstico** -> Seleccione la fábrica de datos -> Activar diagnóstico.

    ![monitor-oms-image1.png](media/data-factory-monitor-oms/monitor-oms-image1.png)

2.  Proporcione la configuración de diagnóstico, incluida la configuración del área de trabajo de OMS.

    ![monitor-oms-image2.png](media/data-factory-monitor-oms/monitor-oms-image2.png)

### <a name="install-azure-data-factory-analytics-oms-pack-from-azure-marketplace"></a>Instale el paquete OMS de Azure Data Factory Analytics desde Azure Marketplace

![monitor-oms-image3.png](media/data-factory-monitor-oms/monitor-oms-image3.png)

![monitor-oms-image4.png](media/data-factory-monitor-oms/monitor-oms-image4.png)

Haga clic en **Crear** y seleccione Área de trabajo de OMS y Configuración del área de trabajo de OMS.

![monitor-oms-image5.png](media/data-factory-monitor-oms/monitor-oms-image5.png)

## <a name="monitor-azure-data-factory-metrics-using-oms"></a>Supervisión de métricas de Azure Data Factory mediante OMS

Con la instalación del paquete OMS de **Azure Data Factory Analytics** se crea un conjunto de vistas predeterminado que habilita las métricas siguientes:

- Ejecuciones de ADF: 1) Ejecuciones de canalización por Data Factory

- Ejecuciones de ADF: 2) Ejecuciones de actividad por Data Factory

- Ejecuciones de ADF: 3) Ejecuciones de desencadenador por Data Factory

- Errores de ADF: 1) Principales errores de canalización por Data Factory

- Errores de ADF: 2) Principales errores de actividad por Data Factory

- Errores de ADF: 3) Principales errores de desencadenador por Data Factory

- Estadísticas de ADF: 1) Ejecuciones de actividad por tipo

- Estadísticas de ADF: 2) Ejecuciones de desencadenador por tipo

- Estadísticas de ADF: 3) Duración máxima de las ejecuciones de canalización

![monitor-oms-image6.png](media/data-factory-monitor-oms/monitor-oms-image6.png)

![monitor-oms-image7.png](media/data-factory-monitor-oms/monitor-oms-image7.png)

Puede visualizar las métricas anteriores, examinar las consultas detrás de estas métricas, editar las consultas, crear alertas, etc.

![monitor-oms-image8.png](media/data-factory-monitor-oms/monitor-oms-image8.png)

## <a name="next-steps"></a>Pasos siguientes

Consulte el artículo sobre [supervisión y administración de canalizaciones mediante programación](https://docs.microsoft.com/en-us/azure/data-factory/monitor-programmatically) para obtener información sobre la supervisión y administración de canalizaciones mediante la ejecución de scripts.
