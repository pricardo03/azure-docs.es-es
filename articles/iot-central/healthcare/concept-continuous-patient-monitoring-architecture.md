---
title: Arquitectura de supervisión continua de pacientes en Azure IoT Central | Microsoft Docs
description: Más información sobre una arquitectura de una solución de supervisión continua de pacientes
author: philmea
ms.author: philmea
ms.date: 10/24/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
manager: eliotgra
ms.openlocfilehash: cf8cfc5fe75ff5364f812af034d772c7aadd891c
ms.sourcegitcommit: d47a30e54c5c9e65255f7ef3f7194a07931c27df
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027472"
---
# <a name="continuous-patient-monitoring-architecture"></a>Arquitectura de la supervisión de pacientes continua

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Las soluciones de supervisión continua de pacientes se pueden crear aprovechando la plantilla de aplicación proporcionada y usando la arquitectura que se describe a continuación como guía.

>[!div class="mx-imgBorder"] 
>![Arquitectura de CPM](media/cpm-architecture.png)

1. Dispositivos médicos que se comunican a través de Bluetooth de bajo consumo (BLE)
1. La puerta de enlace de teléfono móvil recibe datos de BLE y los envía a IoT Central
1. Exportación continua de datos de estado de pacientes a Azure API for FHIR&reg;
1. Aprendizaje automático basado en datos interoperables
1. Panel del equipo de asistencia basado en los datos de FHIR

## <a name="details"></a>Detalles
En esta sección se describe cada parte del diagrama de la arquitectura con más detalle.

### <a name="ble-medical-devices"></a>Dispositivos médicos de BLE
Muchos ponibles médicos usados en el espacio de IoT para el sector sanitario usan dispositivos Bluetooth de bajo consumo, lo que significa que no se pueden comunicar directamente con la nube y necesitan pasar a través de una puerta de enlace. Esta arquitectura sugiere el uso de una aplicación de teléfono móvil como puerta de enlace.

### <a name="mobile-phone-gateway"></a>Puerta de enlace de teléfono móvil
La función principal de la aplicación de teléfono móvil es ingerir datos de BLE de dispositivos médicos y comunicarse con Azure IoT Central. Además, la aplicación puede ayudar a guiar a los pacientes a través de la configuración del dispositivo y el flujo de aprovisionamiento, y a obtener una vista de los datos de salud personales. Otras soluciones pueden utilizar una puerta de enlace de tableta o una puerta de enlace estática si se encuentra dentro de una habitación de hospital para lograr el mismo flujo de comunicación.

### <a name="export-to-azure-api-for-fhirreg"></a>Exportar a Azure API for FHIR&reg;
Azure IoT Central cumple las directrices que establece HIPAA y cuenta con la certificación HITRUST&reg;, pero también puede enviar datos relacionados con la salud de los pacientes a Azure API for FHIR. [Azure API for FHIR](../../healthcare-apis/overview.md) es una API totalmente administrada, basada en estándares para datos clínicos que le permite crear nuevos sistemas de interacción con los datos clínicos. Permite el intercambio rápido de datos con API para FHIR y cuenta con el respaldo de una oferta de plataforma como servicio (PaaS) administrada en la nube. Con la funcionalidad de exportación continua de datos de IoT Central, puede enviar datos a Azure API for FHIR.

### <a name="machine-learning"></a>Machine Learning
Después de agregar los datos y convertirlos al formato FHIR, puede crear modelos de aprendizaje automático que puedan enriquecer las conclusiones y habilitar una toma de decisiones más inteligente para el equipo de atención. Existe una gran variedad de servicios que se pueden usar para crear, entrenar e implementar modelos de aprendizaje automático. Puede encontrar más información sobre el uso de las ofertas de aprendizaje automático de Azure en la [documentación sobre el aprendizaje automático](../../machine-learning/index.yml).

### <a name="provider-dashboard"></a>Panel del proveedor
Los datos que se encuentran en Azure API for FHIR se pueden usar para crear un panel de conclusiones de pacientes o se pueden integrar directamente en un EMR para ayudar a los equipos de atención al paciente a visualizar el estado de los pacientes. Los equipos de atención médica pueden usar este panel para tomar medidas respecto a los pacientes con necesidad de asistencia y detectar signos de deterioro alarmantes lo antes posible. Para obtener información sobre cómo crear un panel de proveedor en tiempo real de Power BI, siga nuestra [guía de procedimientos ](howto-health-data-triage.md).

## <a name="next-steps"></a>Pasos siguientes
* [Más información sobre cómo implementar una plantilla de aplicación de supervisión continua de pacientes](tutorial-continuous-patient-monitoring.md)