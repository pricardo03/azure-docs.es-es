---
title: 'Características de la solución Connected Factory: Azure | Microsoft Docs'
description: Introducción a las características de la solución preconfigurada de Connected Factory.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: dobett
ms.openlocfilehash: 3478217771418ab31772d6a42a7ed8d8a2e8069a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34626503"
---
# <a name="what-is-connected-factory-iot-solution-accelerator"></a>¿Qué es el acelerador de la solución de IoT Factoría conectada?

Connected Factory es una implementación de la arquitectura de referencia de IoT industrial de Azure de Microsoft, empaquetada como solución de código abierto. Puede utilizarla como punto inicial para un producto comercial. Puede implementar una versión pregenerada de la solución Factoría conectada en la suscripción de Azure desde [aceleradores de soluciones de Azure IoT](https://www.azureiotsolutions.com/#solutions/types/CF).

![Panel de la solución Connected Factory](./media/iot-accelerators-connected-factory-features/dashboard.png)

Connected Factory incluye las siguientes características:

## <a name="industrial-device-interoperability"></a>Interoperabilidad con dispositivos industriales

- Conexión con recursos industriales gracias a la interfaz de OPC UA.
- Líneas de producción simuladas (que ejecutan servidores de OPC UA en contenedores de Docker) para ver la telemetría en vivo.
- Examen del modelo de información de OPC UA de los servidores de OPC UA desde un panel en la nube.

## <a name="remote-management"></a>Administración remota

- Configuración de los recursos de OPC UA desde el panel en la nube (métodos de llamada, lectura y escritura de datos).
- Publicación y anulación de la publicación de datos de telemetría de los recursos de OPC UA desde un panel en la nube.

## <a name="cloud-dashboard"></a>Panel en la nube

- Visualización de vistas previas de telemetría directamente en un panel en la nube.
- Visualización de las tendencias en los datos de telemetría y creación de correlaciones con el panel del Explorador de Time Series Insights.
- Visualización del valor de la eficiencia general del equipo (OEE) calculado y los indicadores clave de rendimiento (KPI) desde un panel en la nube.
- Visualización de las jerarquías de los recursos industriales en topología de árbol y en mapa interactivo.
- Visualización, confirmación y cierre de alertas desde un panel en la nube.

## <a name="azure-time-series-insights"></a>Azure Time Series Insights

- [Azure Time Series Insights](../time-series-insights/time-series-insights-overview.md) se ha compilado para almacenar, visualizar y consultar grandes cantidades de datos de series temporales. Connected Factory aprovecha este servicio.
- Factoría conectada se integra con este servicio para permitir los análisis profundos y en tiempo real de los datos del dispositivo.

## <a name="rules-and-alerts"></a>Reglas y alertas

[Configuración de las reglas basadas en umbrales para las alertas](iot-accelerators-connected-factory-configure.md).

## <a name="end-to-end-security"></a>Seguridad de un extremo a otro

- Configuración de permisos de seguridad para los usuarios con el control de acceso basado en rol (RBAC).
- El cifrado de un extremo a otro se implementa mediante la autenticación OPC UA (con certificados X.509), así como con tokens de seguridad.

## <a name="customizability"></a>Personalización

- Personalice la solución para satisfacer requisitos empresariales específicos.
- Código fuente de la solución completo disponible en GitHub. Consulte el repositorio de la [solución preconfigurada Factoría conectada](https://github.com/Azure/azure-iot-connected-factory).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre la solución preconfigurada Factoría conectada, lea los siguientes artículos:

* [Tutorial de la solución preconfigurada Factoría conectada](iot-accelerators-connected-factory-sample-walkthrough.md)
* [Implementación de una puerta de enlace para Factoría conectada]( iot-accelerators-connected-factory-gateway-deployment.md)
