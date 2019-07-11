---
title: 'Qué es OPC Publisher: Azure | Microsoft Docs'
description: Información general de OPC Publisher
author: dominicbetts
ms.author: dobett
ms.date: 06/10/2019
ms.topic: overview
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 99b3f6b4495ae5981322c90fce251b9b0a18bb5d
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67451496"
---
# <a name="what-is-opc-publisher"></a>¿Qué es OPC Publisher?

OPC Publisher es una implementación de referencia que demuestra cómo hacer lo siguiente:

- Conectarse a los servidores de OPC UA existentes.
- Publicar datos de telemetría JSON codificados desde servidores de OPC UA con formato Pub/Sub, mediante una carga JSON, en Azure IoT Hub.

Puede usar cualquiera de los protocolos de transporte que admite el SDK del cliente de Azure IoT Hub: HTTPS, AMQP y MQTT.

La implementación de referencia incluye lo siguiente:

- Un *cliente* de OPC UA para conectarse a los servidores de OPC UA que tiene en la red.
- Un *servidor* de OPC UA en el puerto 62222 que puede usar para administrar lo que se publica y ofrece métodos directos de IoT Hub para hacer lo mismo.

Puede descargar la [implementación de referencia de OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) desde GitHub.

La aplicación se implementa mediante tecnología .NET Core y se puede ejecutar en las plataformas compatibles con .NET Core.

OPC Publisher implementa la lógica de reintento para establecer conexiones a puntos de conexión que no responden a un determinado número de solicitudes de conexión persistente. Por ejemplo, si un servidor OPC UA deja de responder debido a un corte del suministro eléctrico.

Para cada intervalo de publicación diferente de un servidor de OPC UA, la aplicación crea una suscripción independiente en la que se actualizan todos los nodos con este intervalo de publicación.

OPC Publisher admite el procesamiento por lotes de los datos enviados a IoT Hub para reducir la carga de la red. Este procesamiento por lotes envía un paquete a IoT Hub solo si se alcanza el tamaño de paquete configurado.

Esta aplicación usa la pila de referencia de OPC UA de OPC Foundation como paquetes de NuGet. Consulte [https://opcfoundation.org/license/redistributables/1.3/](https://opcfoundation.org/license/redistributables/1.3/) para ver los términos de la licencia.

### <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido qué es OPC Publisher, el siguiente paso sugerido es aprender a [configurar OPC Publisher](howto-opc-publisher-configure.md).
