---
title: Transferencia de datos adicionales entre un dispositivo y Azure Device Provisioning Service
description: En este documento se describe cómo transferir datos adicionales entre el dispositivo y DPS.
author: menchi
ms.author: menchi
ms.date: 10/29/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: 90402dd6fbe19811b5bb6d5ac0fbdd984b71fd33
ms.sourcegitcommit: 5a8c65d7420daee9667660d560be9d77fa93e9c9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2019
ms.locfileid: "74123185"
---
# <a name="how-to-transfer-additional-data-between-device-and-dps"></a>Transferencia de datos adicionales entre un dispositivo y DPS
A veces, DPS necesita más datos de los dispositivos a fin de aprovisionarlos adecuadamente en la instancia correcta de IoT Hub, y el dispositivo necesita proporcionar los datos. A la inversa, DPS puede devolver datos al dispositivo para facilitar la lógica del cliente. 

## <a name="when-to-use-it"></a>Cuándo se debe usar
Esta característica se puede usar como una mejora de la [asignación personalizada](https://docs.microsoft.com/azure/iot-dps/how-to-use-custom-allocation-policies). Por ejemplo, quiere asignar los dispositivos en función de su modelo sin la intervención de personas. En este caso, usará [asignación personalizada](https://docs.microsoft.com/azure/iot-dps/how-to-use-custom-allocation-policies). Puede configurar el dispositivo para que incluya la información del modelo como parte de la [llamada de registro del dispositivo](https://docs.microsoft.com/rest/api/iot-dps/runtimeregistration/registerdevice). DPS pasará la información del dispositivo en el webhook de asignación personalizada. Y la función puede decidir a qué instancia de IoT Hub se dirigirá el dispositivo cuando reciba información del modelo de dispositivo. Asimismo, si el webhook quiere devolver algunos datos al dispositivo, pasará los datos como cadena en la respuesta del webhook.  

## <a name="device-sends-data-to-dps"></a>Envío de datos del dispositivo a DPS
Cuando su dispositivo envía una [llamada de dispositivo de registro](https://docs.microsoft.com/rest/api/iot-dps/runtimeregistration/registerdevice) a DPS, la llamada de registro se puede mejorar para que incorpore otros campos en el cuerpo. El cuerpo será similar al siguiente: 
   ```
   { 
       “registrationId”: “mydevice”, 
       “tpm”:                
       { 
           “endorsementKey”: “stuff”, 
           “storageRootKey”: “things” 
       }, 
       “interfaces”: “TODO: get how interfaces are reported by devices from PnP folks.”, 
       “data”: “your additional data goes here. It can be nested JSON.” 
    } 
   ```

## <a name="dps-returns-data-to-the-device"></a>Devolución de datos de DPS al dispositivo
Si el webhook de la directiva de asignación personalizada quiere devolver algunos datos al dispositivo, pasará los datos como cadena en la respuesta del webhook. El cambio está en la sección returnData siguiente. 
   ```
   { 
       "iotHubHostName": "sample-iot-hub-1.azure-devices.net", 
       "initialTwin": { 
           "tags": { 
               "tag1": true 
               }, 
               "properties": { 
                   "desired": { 
                       "tag2": true 
                    } 
                } 
            }, 
        "returnData": "whatever is returned by the webhook" 
    } 
   ```

## <a name="sdk-support"></a>Compatibilidad con SDK
Esta característica está disponible en los [SDK de cliente](https://docs.microsoft.com/azure/iot-dps/) de C, C#, JAVA y Node.js.  

## <a name="next-steps"></a>Pasos siguientes
* Desarrollo con el [SDK de IoT de Azure]( https://github.com/Azure/azure-iot-sdks) para Azure IoT Hub y Azure IoT Hub Device Provisioning Service
