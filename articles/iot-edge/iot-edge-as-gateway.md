---
title: Uso de dispositivos Azure IoT Edge como puertas de enlace | Microsoft Docs
description: Use Azure IoT Edge para crear un dispositivo de puerta de enlace transparente, opaco o proxy que envíe datos desde varios dispositivos de nivel inferior a la nube y los procese localmente.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/01/2017
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 53f0f1e77f8879807b2fb93b86d825e3bd044f41
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2018
ms.locfileid: "51567220"
---
# <a name="how-an-iot-edge-device-can-be-used-as-a-gateway"></a>Uso de un dispositivo IoT Edge como puerta de enlace

Las puertas de enlace en las soluciones de IoT proporcionan conectividad de dispositivos y análisis perimetral a los dispositivos de IoT que, de lo contrario, no tendrían esas funcionalidades. Azure IoT Edge se puede usar para satisfacer todas las necesidades de una puerta de enlace de IoT con independencia de si están relacionadas con la conectividad, la identidad o los análisis perimetrales. Los patrones de puerta de enlace de este artículo únicamente hacen referencia a las características de la conectividad de los dispositivos de nivel inferior y la identidad de los dispositivos, y no a cómo los datos del dispositivo se procesan en la puerta de enlace.

## <a name="patterns"></a>Patrones

Existen tres patrones para usar un dispositivo IoT Edge como puerta de enlace: transparente, traducción del protocolo y traducción de la identidad:
* **Transparente**: los dispositivos que podrían conectarse teóricamente a IoT Hub pueden conectarse en su lugar a un dispositivo de puerta de enlace. Los dispositivos de nivel inferior tienen sus propias identidades de IoT Hub y usan cualquiera de los protocolos MQTT, AMQP o HTTP. La puerta de enlace simplemente pasa las comunicaciones entre los dispositivos e IoT Hub. Los dispositivos no saben que se están comunicando con la nube a través de una puerta de enlace y el usuario que interactúa con los dispositivos en IoT Hub no sabe que hay un dispositivo de puerta de enlace intermedio. Por lo tanto, la puerta de enlace es transparente. Consulte [Creación de una puerta de enlace transparente](how-to-create-transparent-gateway.md) para obtener información específica sobre como usar un dispositivo IoT Edge como puerta de enlace transparente.
* **Traducción del protocolo**: también se conoce como patrón de puerta de enlace opaca. Los dispositivos que no admiten MQTT, AMQP o HTTP pueden usar un dispositivo de puerta de enlace para enviar datos a IoT Hub en su nombre. La puerta de enlace comprende el protocolo utilizado por los dispositivos de nivel inferior; sin embargo, es el único dispositivo que tiene identidad en IoT Hub. Toda la información parece proceder de un dispositivo, la puerta de enlace. Los dispositivos de nivel inferior deben insertar información de identificación adicional en sus mensajes si las aplicaciones de nube quieren analizar los datos de cada dispositivo. Además, las primitivas de IoT Hub, como los gemelos y los métodos, solo están disponibles para el dispositivo de puerta de enlace, no para los dispositivos de nivel inferior.
* **Traducción de la identidad**: los dispositivos que no se pueden conectar a IoT Hub en su lugar pueden conectarse a un dispositivo de puerta de enlace. La puerta de enlace proporciona traducción de protocolo e identidad de IoT Hub en nombre de los dispositivos de nivel inferior. La puerta de enlace es lo suficientemente inteligente como para comprender el protocolo utilizado por los dispositivos de nivel inferior, proporcionarles una identidad y traducir los primitivos de IoT Hub. Los dispositivos de nivel inferior aparecen en IoT Hub como dispositivos de primera clase con gemelos y métodos. Un usuario puede interactuar con los dispositivos IoT Hub sin saber que hay un dispositivo de puerta de enlace intermedio.

![Diagramas de patrones de puerta de enlace](./media/iot-edge-as-gateway/edge-as-gateway.png)

## <a name="use-cases"></a>Casos de uso
Todos los patrones de puerta de enlace proporcionan las siguientes ventajas:
* **Análisis en dispositivos perimetrales**: use los servicios de IA localmente para procesar los datos que proceden de dispositivos de nivel inferior sin enviar datos telemetría con total fidelidad a la nube. Busca y reacciona a la información localmente y solo envía un subconjunto de datos a IoT Hub. 
* **Aislamiento de dispositivos de nivel inferior**: el dispositivo de puerta de enlace puede proteger todos los dispositivos de nivel inferior de la exposición a Internet. Se puede situar entre una red de tecnología operativa (OT) que no tiene conectividad y una red de tecnología de la información (TI) que proporciona acceso a Internet. 
* **Multiplexación de conexiones**: todos los dispositivos que se conectan a IoT Hub mediante una puerta de enlace IoT Edge usan la misma conexión subyacente.
* **Suavizado de tráfico**: el dispositivo IoT Edge implementará automáticamente un retroceso exponencial si IoT Hub limita el tráfico, al tiempo que se conservan los mensajes localmente. Esta ventaja hace que la solución sea resistente a los picos de tráfico.
* **Compatibilidad sin conexión limitada**: el dispositivo de puerta de enlace almacena los mensajes y las actualizaciones gemelas que no se puedan entregar a IoT Hub.

Una puerta de enlace que realiza la traducción del protocolo también puede realizar el análisis perimetral, el aislamiento de dispositivos, el suavizado de tráfico y la compatibilidad sin conexión en dispositivos nuevos y existentes con restricciones de recursos. Muchos dispositivos existentes producen datos que pueden favorecer el conocimiento del negocio; sin embargo, no están diseñados teniendo en mente la conectividad a la nube. Las puertas de enlace opacas permiten que estos datos se desbloqueen y utilicen en una solución IoT de un extremo a otro.

Una puerta de enlace que realiza la traducción de la identidad proporciona las ventajas de la traducción del protocolo y además permite una manejabilidad completa de los dispositivos de nivel inferior desde la nube. Todos los dispositivos de la solución IoT se muestran en IoT Hub con independencia del protocolo que usen.

## <a name="cheat-sheet"></a>Hoja de referencia rápida
Se trata de una hoja de características clave rápida que compara los primitivos de IoT Hub cuando se usan puertas de enlace transparentes, opacas (protocolo) y proxy.

| &nbsp; | Puerta de enlace transparente | Traducción del protocolo | Traducción de la identidad |
|--------|-------------|--------|--------|
| Identidades almacenadas en el registro de identidades de IoT Hub | Identidades de todos los dispositivos conectados | Solo la identidad del dispositivo de puerta de enlace | Identidades de todos los dispositivos conectados |
| Dispositivo gemelo | Cada dispositivo conectado tiene su propio dispositivo gemelo | Solo la puerta de enlace tiene dispositivo y un módulo gemelos | Cada dispositivo conectado tiene su propio dispositivo gemelo |
| Métodos directos y mensajes de nube a dispositivo | La nube puede tratar individualmente cada dispositivo conectado | La nube solo puede tratar el dispositivo de puerta de enlace | La nube puede tratar individualmente cada dispositivo conectado |
| [Limitaciones y cuotas de IoT Hub](../iot-hub/iot-hub-devguide-quotas-throttling.md) | Aplicación a cada dispositivo | Aplicación al dispositivo de puerta de enlace | Aplicación a cada dispositivo |

Cuando se usa un patrón de puerta de enlace opaca (traducción de protocolo), todos los dispositivos que se conectan a través de esa puerta de enlace comparten la misma cola de nube a dispositivo, que puede contener 50 mensajes como máximo. De ello se desprende que el patrón de puerta de enlace opaca debe usarse solo cuando pocos dispositivos están conectados a través de cada puerta de enlace de campo y su tráfico de nube a dispositivo sea bajo.

## <a name="next-steps"></a>Pasos siguientes
Aprenda a configurar un dispositivo IoT Edge para que actúe como [puerta de enlace transparente](how-to-create-transparent-gateway-linux.md).
