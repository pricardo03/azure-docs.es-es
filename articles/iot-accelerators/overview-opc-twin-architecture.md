---
title: 'Arquitectura de OPC Twin: Azure | Microsoft Docs'
description: Arquitectura de OPC Twin
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: overview
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: d1c10e056de1bc61b5365550bf2543d2cdf61229
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59783909"
---
# <a name="opc-twin-architecture"></a>Arquitectura de OPC Twin

Los siguientes diagramas ilustran la arquitectura de OPC Twin.

## <a name="discover-and-activate"></a>Detección y activación

1. El operador permite el examen de la red en el módulo o crea una detección puntual mediante una dirección URL de detección. Los puntos de conexión detectados y la información de la aplicación se envían mediante telemetría al agente de incorporación para su procesamiento.  El agente de incorporación de dispositivos OPC UA procesa los eventos de detección de servidores OPC UA enviados por el módulo de IoT Edge de OPC Twin cuando se encuentra en modo de examen o detección. Los eventos de detección dan lugar al registro de aplicaciones y a actualizaciones del registro de dispositivos OPC UA.

   ![Funcionamiento de OPC Twin](media/overview-opc-twin-architecture/opc-twin1.png)

1. El operador inspecciona el certificado del punto de conexión detectado y activa el punto de conexión gemelo registrado para proporcionar acceso. 

   ![Funcionamiento de OPC Twin](media/overview-opc-twin-architecture/opc-twin2.png)

## <a name="browse-and-monitor"></a>Examen y supervisión

1. Una vez activado, el operador puede usar la API REST del servicio Twin para examinar o inspeccionar el modelo de información de servidor, leer o escribir variables de objeto y llamar a métodos.  El usuario emplea una API de OPC UA simplificada expresada totalmente en HTTP y JSON.

   ![Funcionamiento de OPC Twin](media/overview-opc-twin-architecture/opc-twin3.png)

1. La interfaz REST del servicio Twin también puede usarse para crear suscripciones y elementos supervisados en OPC Publisher. OPC Publisher permite el envío de telemetría desde sistemas de servidor de OPC UA hasta IoT Hub. Para más información sobre OPC Publisher, consulte el repositorio de [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) en GitHub.

   ![Funcionamiento de OPC Twin](media/overview-opc-twin-architecture/opc-twin4.png)