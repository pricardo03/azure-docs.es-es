---
title: 'Arquitectura de referencia de aceleradores de soluciones de IoT: Azure | Microsoft Docs'
description: Obtenga información sobre la arquitectura de referencia de los aceleradores de soluciones de Azure IoT. Los aceleradores de soluciones existentes aprovechan esta arquitectura de referencia. También puede usar la arquitectura de referencia al crear sus propias soluciones de IoT personalizadas.
author: dominicbetts
ms.author: dobett
ms.date: 12/04/2018
ms.topic: conceptual
ms.custom: mvc
ms.service: iot-accelerators
services: iot-accelerators
manager: philmea
ms.openlocfilehash: ba5eb50dcf800c186124db348ac584ff6f55cebb
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54467322"
---
# <a name="introduction-to-the-azure-iot-reference-architecture"></a>Introducción a la arquitectura de referencia de Azure IoT

En este artículo, se presenta la [arquitectura de referencia de Azure IoT](https://aka.ms/iotrefarchitecture) y se proporcionan ejemplos sobre cómo los [aceleradores de soluciones de Azure IoT](about-iot-accelerators.md) siguen sus recomendaciones.

Los aceleradores de soluciones de código abierto [Remote Monitoring](iot-accelerators-remote-monitoring-sample-walkthrough.md) y [Connected Factory](iot-accelerators-connected-factory-sample-walkthrough.md) siguen muchas de las recomendaciones de la arquitectura de referencia. Puede usar los aceleradores de soluciones como punto de inicio para su propia solución de IoT o como herramientas de aprendizaje.

## <a name="overview"></a>Información general

En la arquitectura de referencia, se describen los elementos de una solución de IoT, como las entidades de tecnología, la composición de los servicios de Azure IoT y los dispositivos. La arquitectura también realiza recomendaciones sobre tecnologías de implementación.

En el nivel superior, puede ver que una solución de IoT se compone de:

* Elementos que generan y envían datos de telemetría, como medidas y eventos. En el acelerador de soluciones Remote Monitoring, los dispositivos como camiones o ascensores son los elementos que envían datos de telemetría.
* Conclusiones generadas a partir de la telemetría enviada desde los elementos. En el acelerador de soluciones Remote Monitoring, una regla genera una conclusión. Por ejemplo, una regla puede identificar el momento en que la temperatura de un motor alcanza un umbral.
* Acciones, basadas en conclusiones, que ayudan a mejorar un proceso o negocio. En el acelerador de soluciones Remote Monitoring, una acción de correo electrónico puede notificar a un operador sobre un posible problema con un motor.

La [arquitectura de referencia de Azure IoT](https://aka.ms/iotrefarchitecture) es un documento en continuo cambio que se actualiza a medida que evoluciona la tecnología.

## <a name="core-subsystems"></a>Subsistemas principales

La arquitectura de referencia identifica los subsistemas principales que se muestran en el diagrama siguiente:

![Subsistemas principales](media/iot-accelerators-architecture-overview/CoreSubsystems.png)

En las secciones siguientes, se describe cómo se asignan los componentes del acelerador de soluciones Remote Monitoring a los subsistemas principales.

### <a name="iot-devices"></a>Dispositivos IoT

Una solución de IoT tiene que habilitar una comunicación segura, eficiente y sólida entre prácticamente cualquier tipo de dispositivo y una puerta de enlace de nube. Los dispositivos son activos empresariales que varían desde sencillos sensores de temperatura a complejas líneas de producción de fábrica con cientos de componentes y sensores.

Una puerta de enlace de campo (o dispositivo perimetral) es un dispositivo especializado o un software de uso general que puede actuar como:

* Un activador de comunicación para controlar la conversión de protocolo.
* Un sistema de control de dispositivos local y un centro de procesamiento de telemetría de dispositivos. Un dispositivo perimetral puede procesar la telemetría localmente para controlar dispositivos sin comunicarse con la nube. Un dispositivo perimetral también puede filtrar o agregar telemetría de dispositivos para reducir la cantidad de telemetría que se transfiere a la nube.

En la solución Remote Monitoring, los dispositivos se conectan a un centro de IoT y envían telemetría para su procesamiento. La solución Remote Monitoring también permite a los operadores administrar dispositivos mediante trabajos o la administración automática de dispositivos. Use los SDK de dispositivos de Azure IoT para implementar los dispositivos.

La solución Remote Monitoring puede implementar y administrar dispositivos de IoT Edge. El procesamiento en el área perimetral permite reducir el volumen de telemetría enviado a la nube y agiliza las respuestas a los eventos de dispositivo.

### <a name="cloud-gateway"></a>Puerta de enlace en la nube

Una puerta de enlace de nube permite la comunicación hacia y desde dispositivos y dispositivos perimetrales. Estos dispositivos pueden estar en muchos sitios remotos.

La puerta de enlace administra la comunicación de dispositivos, incluida la administración de conexiones, la protección de la ruta de comunicación, la autenticación y la autorización. La puerta de enlace también exige cuotas de rendimiento y conexión, además de recopilar la telemetría usada para facturación, diagnósticos y otras tareas de supervisión.

La solución Remote Monitoring implementa un centro de IoT para proporcionar un punto de conexión seguro donde los dispositivos puedan enviar datos de telemetría. El centro de IoT también:

* Incluye un almacén de identidades de dispositivo para administrar los dispositivos que pueden conectarse a la solución.
* Permite que la solución envíe comandos a los dispositivos. Por ejemplo, para abrir una válvula para liberar presión.
* Permite la administración masiva de dispositivos. Por ejemplo, para actualizar el firmware en un conjunto de dispositivos.

### <a name="stream-processing"></a>Procesamiento de flujos

A medida que la solución obtiene datos de telemetría, es importante comprender cómo puede variar el flujo del procesamiento de datos de telemetría. Según el escenario, los registros de datos pueden fluir por varias fases:

* Almacenamiento, como cachés en memoria, colas temporales y archivos permanentes.

* Análisis, para ejecutar la telemetría de entrada mediante un conjunto de condiciones; además, puede producir distintos registros de datos de salida. Por ejemplo, la telemetría de entrada codificada en Avro puede devolver telemetría de salida codificada en JSON.

* Los registros de salida de análisis y telemetría de entrada originales suelen almacenarse y estar disponibles para su visualización. Los registros de salida y telemetría de entrada también pueden desencadenar acciones, como correos electrónicos, vales de incidentes y comandos de dispositivo.

El enrutamiento puede enviar la telemetría a uno o más puntos de conexión de almacenamiento, procesos de análisis y acciones. Una solución puede combinar las fases en distintos pedidos y procesarlos con tareas paralelas simultáneas.

La solución Remote Monitoring usa [Azure Stream Analytics](/azure/stream-analytics/) para el procesamiento de datos transmitidos por streaming. El motor de reglas de la solución usa las consultas de Stream Analytics para generar alertas y acciones. Por ejemplo, la solución puede usar una consulta para identificar el momento en que la temperatura media del compartimiento de almacenamiento de un camión sea inferior a 36 grados durante más de cinco minutos.

### <a name="storage"></a>Storage

Las soluciones de IoT pueden generar grandes cantidades de datos que, con frecuencia, son datos de series temporales. Es necesario almacenar estos datos en una ubicación donde puedan usarse con fines de visualización y generación de informes. Una solución también puede que necesite acceder a datos más tarde para su análisis o procesamiento adicional. Es habitual dividir datos en almacenes de datos activos e inactivos. El almacén de datos activos contiene datos recientes para el acceso con baja latencia. El almacén de datos inactivos suele almacenar datos históricos.

La solución Remote Monitoring usa [Azure Time Series Insights](/azure/time-series-insights/) como su almacén de datos activos y Cosmos DB como su almacén de datos inactivos.

### <a name="ui-and-reporting-tools"></a>Herramientas de informes e interfaz de usuario

La interfaz de usuario de la solución puede proporcionar:

* Acceso y visualización de datos de dispositivo de resultados de análisis.
* Detección de dispositivos mediante el registro.
* El comando y el control de dispositivos.
* Flujos de trabajo de aprovisionamiento de dispositivos.
* Notificaciones y alertas.
* Integración con paneles interactivos en directo para mostrar datos de un gran número de dispositivos.  
* Servicios de ubicación geográfica y detección geográfica.

En la solución Remote Monitoring, se incluye una interfaz de usuario web para ofrecer esta función. En la interfaz de usuario web, se incluye lo siguiente:

* Un mapa interactivo para mostrar la ubicación de los dispositivos.
* Acceso al explorador de Time Series Insights para consultar y analizar la telemetría.

### <a name="business-integration"></a>Integración de negocios

La capa de integración empresarial controla la integración de la solución de IoT con sistemas empresariales como CRM, ERP y aplicaciones de línea de negocio. Algunos ejemplos son la facturación de servicios, atención al cliente y suministro de piezas de repuesto.

La solución Remote Monitoring usa reglas para enviar correos electrónicos cuando se cumpla una condición. Por ejemplo, la solución puede notificar a un operador cuando la temperatura de un camión sea inferior a 36 grados.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha obtenido información sobre la arquitectura de referencia de Azure IoT y ha visto algunos ejemplos de cómo el acelerador de soluciones Remote Monitoring sigue sus recomendaciones. El paso siguiente es leer la [Arquitectura de referencia de Microsoft Azure IoT](https://aka.ms/iotrefarchitecture).