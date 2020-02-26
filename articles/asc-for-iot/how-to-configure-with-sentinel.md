---
title: Guía de Azure Security Center para IoT para la configuración con Azure Sentinel (versión preliminar)| Microsoft Docs
description: En esta guía de procedimientos se explica cómo configurar Azure Sentinel para recibir datos de la solución de Azure Security Center para IoT.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/18/2020
ms.author: mlottner
ms.openlocfilehash: f6e7eddd6ddbcec61c3d8d173891cbc9abaaf08f
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77465657"
---
> [!IMPORTANT]
> El conector de datos de Azure Security Center para IoT en Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta característica se ofrece sin contrato de nivel de servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

# <a name="connect-your-data-from-azure-security-center-for-iot-to-azure-sentinel-preview"></a>Conexión de los datos de Azure Security Center para IoT con Azure Sentinel (versión preliminar) 

En esta guía, aprenderá a conectar datos de Azure Security Center para IoT a Azure Sentinel.  

> [!div class="checklist"]
> * Prerrequisitos 
> * Configuración de conexión
> * Vista de alerta de Log Analytics 

Conecte alertas de Azure Security Center para IoT y transmítalas directamente a Azure Sentinel.

## <a name="prerequisites"></a>Prerrequisitos

- Debe tener permisos de **lectura** y **escritura** de área de trabajo.
- **Azure Security Center para IoT** debe estar **habilitado** en las instancias pertinentes de IoT Hub.
- Debe tener permisos de **lectura** y **escritura** en la instancia de **Azure IoT Hub** que quiera conectar.
- También debe tener permisos de **lectura** y **escritura** en el **grupo de recursos de Azure IoT Hub**.

> [!NOTE]
> Debe tener la licencia de nivel Estándar de Azure Security Center en ejecución en la suscripción para enviar alertas de recursos de Azure generales. Con la licencia de nivel Gratis necesaria para Azure Security Center para IoT, solo se reenviarán a Azure Sentinel las alertas relacionadas con Azure Security Center para IoT. 

## <a name="connect-to-azure-security-center-for-iot"></a>Conexión a Azure Security Center para IoT

1. En Azure Sentinel, seleccione **Conectores de datos** y, luego, haga clic en el mosaico **Azure Security Center para IoT**.
1. Desde el panel inferior derecho, haga clic en **Abrir la página del conector**. 
1. Haga clic en **Conectar** junto a cada suscripción de IoT Hub cuyas alertas y alertas de dispositivo quiera transmitir a Azure Sentinel. 
    - Si Azure Security Center para IoT no está habilitado en esa instancia de IoT Hub, verá el mensaje de advertencia Habilitar. Haga clic en el enlace **Habilitar** para iniciar el servicio. 
1. Puede decidir si quiere que las alertas de Azure Security Center para IoT generen incidentes automáticamente en Azure Sentinel. En **Creación de incidentes**, seleccione **Habilitar** para habilitar la regla analítica predeterminada que crea incidentes automáticamente a partir de alertas generadas en el servicio de seguridad conectado. Esta regla se puede cambiar o editar en **Análisis** > **Activar** reglas.

> [!NOTE]
>Puede tardar 10 segundos o más en actualizar la lista de centros de conectividad tras realizar cambios en la conexión. 

## <a name="log-analytics-alert-display"></a>Visualización de alertas de Log Analytics

Para usar el esquema correspondiente en Log Analytics para mostrar las alertas de Azure Security Center para IoT:

1. Abra **Registros** > **SecurityInsights** > **SecurityAlert** o busque **SecurityAlert**. 
2. Use el siguiente filtro kql para ver solo las alertas generadas por Azure Security Center para IoT:

```kusto
SecurityAlert | where ProductName == "Azure Security Center for IoT"
``` 

### <a name="service-notes"></a>Notas del servicio

Tras conectar una instancia de IoT Hub, los datos del centro de conectividad están disponibles en Azure Sentinel aproximadamente 15 minutos más tarde.


## <a name="next-steps"></a>Pasos siguientes

En este documento, ha aprendido a conectar Azure Security Center para IoT a Azure Sentinel. Para obtener más información sobre la detección de amenazas y el acceso a datos de seguridad, consulte los siguientes artículos:

- Aprenda a usar Azure Sentinel para [obtener visibilidad de los datos y de posibles amenazas](https://docs.microsoft.com/azure/sentinel/quickstart-get-visibility).

- Aprenda a [acceder a los datos de seguridad de IoT](how-to-security-data-access.md).