---
title: archivo de inclusión
description: archivo de inclusión
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 12/31/2018
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: dd4873017105db190f9a468ec1f1f77f4e8c9c0e
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/02/2019
ms.locfileid: "53977153"
---
Una de las funcionalidades clave de Azure IoT Edge es que puede implementar módulos en dispositivos de IoT Edge desde la nube. Un módulo de IoT Edge es un paquete ejecutable que se implementa como un contenedor. En esta sección, se va a implementar un módulo pregenerado desde la [sección de módulos de IoT Edge de Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules). Este módulo genera una telemetría simulada para el dispositivo IoT Edge.

1. En [Azure Portal](https://portal.azure.com), escriba **Sensor de temperatura simulado** en la búsqueda y abra el resultado de Marketplace.

   ![Sensor de temperatura simulado en la búsqueda de Azure Portal](./media/iot-edge-deploy-module/search-for-temperature-sensor.png)

2. Elija un dispositivo IoT Edge para recibir este módulo. En **Dispositivos de destino para el módulo de IoT Edge**, proporcione esta información:

   1. **Suscripción**: seleccione la suscripción que contiene el centro de IoT que usa.

   2. **IoT Hub**: seleccione el nombre del centro de IoT que usa.

   3. **Nombre de dispositivo IoT Edge**: si usó el nombre de dispositivo sugerido anteriormente en este inicio rápido, escriba **myEdgeDevice**. O bien, seleccione **Buscar dispositivo** para elegir de una lista de dispositivos en el centro de IoT. 
   
   4. Seleccione **Crear**.

3. Ahora que eligió un módulo de IoT Edge en Azure Marketplace y un dispositivo IoT Edge para recibir el módulo, verá un asistente de tres pasos que lo ayudará a definir exactamente cómo se implementará el módulo. En el paso **Agregar módulos** del asistente, observe que el módulo**SimulatedTemperatureSensor** se rellena automáticamente. En los tutoriales, usará esta página para agregar otros módulos a la implementación. En este inicio rápido, basta con implementar este módulo. Seleccione **Siguiente** para pasar al próximo paso del asistente.

4. En el paso **Especificar rutas** del asistente, defina cómo se transmiten los mensajes entre los módulos y a IoT Hub. Para el inicio rápido, querrá que todos los mensajes de todos los módulos vayan a IoT Hub (`$upstream`). Si no se rellena automáticamente, agregue el código siguiente y seleccione **Siguiente**:

   ```json
    {
    "routes": {
        "route": "FROM /messages/* INTO $upstream"
        }
    }
   ```

5. En el paso **Revisar implementación** del asistente, puede obtener una vista previa del archivo JSON que define todos los módulos que se implementarán en el dispositivo IoT Edge. Observe que se incluye el módulo **SimulatedTemperatureSensor**, así como dos módulos de sistema adicionales llamados **edgeAgent** y **edgeHub**. Seleccione **Enviar** cuando termine de revisar.

   Cuando se envía una implementación nueva a un dispositivo IoT Edge, no se inserta nada en el dispositivo. En lugar de eso, el dispositivo consulta a IoT Hub de manera periódica para comprobar cualquier instrucción nueva. Cuando ve la información de implementación nueva, el dispositivo la usa para extraer las imágenes de módulo desde la nube e inicia la ejecución local de los módulos. Este proceso puede tardar unos minutos. 

6. Después de enviar los detalles de la implementación del módulo, el asistente lo lleva nuevamente a la página **IoT Edge** del centro de IoT. Seleccione el dispositivo en la lista de dispositivos IoT Edge para ver sus detalles. 

7. En la página de detalles del dispositivo, desplácese a la sección **Módulos**. Deben aparecer tres módulos: $edgeAgent, $edgeHub y SimulatedTemperatureSensor. Si uno o varios de los módulos aparecen como especificados en la implementación pero no informados por el dispositivo, significa que el dispositivo IoT Edge todavía los está iniciando. Espere unos instantes y seleccione **Actualizar** en la parte superior de la página. 

   ![Visualización de SimulatedTemperatureSensor en la lista de módulos implementados](./media/iot-edge-deploy-module/deployed-modules-marketplace.png)
