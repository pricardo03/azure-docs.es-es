---
title: 'Funcionamiento de dispositivos sin conexión: Azure IoT Edge | Microsoft Docs'
description: Descubra cómo los módulos y los dispositivos de IoT Edge pueden funcionar sin conexión a Internet durante largos períodos de tiempo y cómo IoT Edge puede permitir que dispositivos de IoT comunes funcionen también sin conexión.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 01/30/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 74d2601c2319ccad9cc980b83894a3242705aa46
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65148117"
---
# <a name="understand-extended-offline-capabilities-for-iot-edge-devices-modules-and-child-devices"></a>Comprender extendidas capacidades sin conexión para los dispositivos IoT Edge, módulos y dispositivos secundarios

Azure IoT Edge permite realizar operaciones sin conexión ampliadas en los dispositivos de IoT Edge y operaciones sin conexión en dispositivos secundarios que no son de Edge. Si un dispositivo de IoT Edge ha podido conectarse a IoT Hub aunque sea una sola vez, ese dispositivo y cualquier dispositivo secundario podrá seguir funcionando con una conexión a Internet intermitente o sin conexión. 


## <a name="how-it-works"></a>Cómo funciona

Cuando un dispositivo de IoT Edge está en modo sin conexión, el centro de IoT Edge asume tres roles. En primer lugar, almacena los mensajes que deberían enviarse y los guarda hasta que el dispositivo vuelva a tener conexión. En segundo lugar, actúa en nombre de IoT Hub para autenticar los módulos y los dispositivos secundarios, de forma que puedan seguir trabajando. En tercer lugar, permite la comunicación entre los dispositivos secundarios, que normalmente se realizaría mediante IoT Hub. 

En el ejemplo siguiente, se muestra cómo funciona un escenario de IoT Edge en modo sin conexión:

1. **Configure un dispositivo de IoT Edge.**

   De forma automática, los dispositivos de IoT Edge tienen habilitadas funcionalidades sin conexión. Para ampliar esa funcionalidad a otros dispositivos de IoT, debe declarar una relación de tipo primario/secundario entre los dispositivos de IoT Hub. 

2. **Sincronice con IoT Hub.**

   Es necesario que, al menos una vez después de instalar el entorno de ejecución de IoT Edge, el dispositivo de IoT Edge tenga conexión para sincronizarse con IoT Hub. En esta sincronización, el dispositivo IoT Edge obtiene información detallada sobre los dispositivos secundarios asignados a él. El dispositivo IoT Edge también actualiza de forma segura su caché local para permitir las operaciones sin conexión y recupera la configuración del almacenamiento local de mensajes de telemetría. 

3. **Active el modo sin conexión.**

   Mientras el dispositivo de IoT Edge está desconectado de IoT Hub, los módulos implementados y los dispositivos secundarios IoT pueden operar indefinidamente. Mientras están sin conexión, los módulos y los dispositivos secundarios pueden iniciarse y reiniciarse autenticándose con el centro de IoT Edge. Los datos de telemetría que deben enviarse a IoT Hub se almacenan localmente. La comunicación entre los módulos o los dispositivos IoT secundarios se realiza mediante mensajes o métodos directos. 

4. **Vuelva a conectar y a sincronizar con IoT Hub.**

   Una vez que se restaura la conexión con IoT Hub, el dispositivo de IoT Edge vuelve a sincronizarse. Los mensajes almacenados localmente se entregan en el mismo orden en el que se almacenaron. Las diferencias entre las propiedades deseadas y notificadas de los módulos y los dispositivos se concilian. El dispositivo de IoT Edge actualiza los cambios en el grupo de dispositivos IoT secundarios que tiene asignados.

## <a name="restrictions-and-limits"></a>Restricciones y límites

Las funcionalidades sin conexión ampliadas que se describen en este artículo están disponibles en [IoT Edge 1.0.4 o versiones posteriores](https://github.com/Azure/azure-iotedge/releases). Las versiones anteriores tienen un subconjunto de características sin conexión. Los dispositivos de IoT Edge existentes que no tienen funcionalidades sin conexión ampliadas no se pueden actualizar cambiando la versión del entorno de ejecución, sino que deben volver a configurarse con una nueva identidad de dispositivo de IoT Edge para obtener estas características. 

La compatibilidad con las funcionalidades sin conexión ampliadas está disponible en todas las regiones en las que lo está IoT Hub, **excepto** en Este de EE. UU.

Solo los dispositivos IoT que no son de Edge pueden agregarse como dispositivos secundarios. 

Los dispositivos de IoT Edge y sus dispositivos secundarios asignados pueden funcionar indefinidamente sin conexión tras realizar una vez la sincronización inicial. Sin embargo, el almacenamiento de mensajes depende de la configuración del período de vida (TTL) y del espacio en disco disponible para almacenar los mensajes. 

## <a name="set-up-an-iot-edge-device"></a>Configuración de un dispositivo de IoT Edge

Para que un dispositivo de IoT Edge amplíe sus funcionalidades sin conexión a los dispositivos IoT secundarios, es necesario declarar las relaciones primario/secundario en Azure Portal.

### <a name="assign-child-devices"></a>Asignación de dispositivos secundarios

Los dispositivos secundarios pueden ser cualquier dispositivo que no pertenezca a Edge que esté registrado en la misma instancia de IoT Hub. Los dispositivos primarios pueden tener varios dispositivos secundarios, pero un dispositivo secundario solo puede tener un dispositivo primario. Hay tres opciones para configurar dispositivos secundarios en un dispositivo perimetral:

#### <a name="option-1-iot-hub-portal"></a>Opción 1: Portal de IoT Hub

 Puede administrar la relación primario/secundario al crear un nuevo dispositivo o desde la página de detalles del dispositivo de IoT Edge primario o del dispositivo IoT secundario. 

   ![Administración de dispositivos secundarios desde la página de detalles del dispositivo de IoT Edge](./media/offline-capabilities/manage-child-devices.png)


#### <a name="option-2-use-the-az-command-line-tool"></a>Opción 2: Use el `az` herramienta de línea de comandos

Mediante el [interfaz de línea de comandos de Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) con [extensión de IoT](https://github.com/azure/azure-iot-cli-extension) (v0.7.0 o posterior), puede administrar relaciones de elementos secundarios del elemento primario con el [identidad de dispositivo](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest) subcomandos. En el ejemplo siguiente, se ejecuta una consulta para asignar dispositivos en el centro de todos los que no son de IoT Edge como dispositivos secundarios de un dispositivo IoT Edge. 

```shell
# Set IoT Edge parent device
egde_device="edge-device1"

# Get All IoT Devices
device_list=$(az iot hub query \
        --hub-name replace-with-hub-name \
        --subscription replace-with-sub-name \
        --resource-group replace-with-rg-name \
        -q "SELECT * FROM devices WHERE capabilities.iotEdge = false" \
        --query 'join(`, `, [].deviceId)' -o tsv)

# Add all IoT devices to IoT Edge (as child)
az iot hub device-identity add-children \
  --device-id $egde_device \
  --child-list $device_list \
  --hub-name replace-with-hub-name \
  --resource-group replace-with-rg-name \
  --subscription replace-with-sub-name 
```

Puede modificar el [consulta](../iot-hub/iot-hub-devguide-query-language.md) para seleccionar un subconjunto diferente de los dispositivos. El comando puede tardar varios segundos si especifica un conjunto grande de dispositivos.

#### <a name="option-3-use-iot-hub-service-sdk"></a>Opción 3: Usar el servicio SDK de IoT Hub 

Por último, puede administrar relaciones de elementos secundarios del elemento primario mediante programación con cualquiera C#, Java o el SDK del servicio IoT Hub de Node.js. Este es un [ejemplo de asignación de un dispositivo secundario](https://aka.ms/set-child-iot-device-c-sharp) utilizando el C# SDK.

### <a name="specifying-dns-servers"></a>Especificar los servidores DNS 

Para mejorar la solidez, se recomienda que especificar las direcciones de servidor DNS, usadas en su entorno. Consulte la [dos opciones para hacerlo desde el artículo de solución](troubleshoot.md#resolution-7).

## <a name="optional-offline-settings"></a>Ajustes opcionales del modo sin conexión

Si espera recopilar todos los mensajes generados por sus dispositivos durante largos períodos sin conexión, configure el centro de IoT Edge para que pueda almacenar todos los mensajes. Hay dos cambios que puede hacer en el centro de IoT Edge para habilitar el almacenamiento de mensajes a largo plazo. En primer lugar, aumente la configuración del período de vida. A continuación, agregue espacio en disco adicional para el almacenamiento de mensajes. 

### <a name="time-to-live"></a>Período de vida

La configuración del período de vida es la cantidad de tiempo (en segundos) que puede esperar un mensaje para entregarse antes de que expire. El valor predeterminado es 7200 segundos (dos horas). El valor máximo solo está limitado por el valor máximo de una variable de entero, que es aproximadamente 2 mil millones. 

Esta configuración es una propiedad deseada del centro de IoT Edge, que se almacena en el módulo gemelo. Puede configurarla en Azure Portal, en la sección **Configurar las opciones avanzadas del entorno en tiempo de ejecución de Edge** o directamente en el manifiesto de implementación. 

```json
"$edgeHub": {
    "properties.desired": {
        "schemaVersion": "1.0",
        "routes": {},
        "storeAndForwardConfiguration": {
            "timeToLiveSecs": 7200
        }
    }
}
```

### <a name="additional-offline-storage"></a>Almacenamiento adicional sin conexión

De forma predeterminada, los mensajes se guardan en el sistema de archivos del contenedor del centro de IoT Edge. Si la cantidad de almacenamiento no es suficiente para sus necesidades sin conexión, puede dedicar un almacenamiento local del dispositivo de IoT Edge. Cree una variable de entorno en el centro de IoT Edge que apunte a una carpeta de almacenamiento del contenedor. Después, tendrá que utilizar las opciones de creación para enlazar esa carpeta de almacenamiento con una carpeta del equipo host. 

Puede configurar las variables de entorno y las opciones de creación del módulo del centro de IoT Edge en Azure Portal, en la sección **Configurar las opciones avanzadas del entorno en tiempo de ejecución de Edge**. También puede configurarlas directamente en el manifiesto de implementación. 

```json
"edgeHub": {
    "type": "docker",
    "settings": {
        "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
        "createOptions": {
            "HostConfig": {
                "Binds": ["<HostStoragePath>:<ModuleStoragePath>"],
                "PortBindings": {
                    "8883/tcp": [{"HostPort":"8883"}],
                    "443/tcp": [{"HostPort":"443"}],
                    "5671/tcp": [{"HostPort":"5671"}]
                }
            }
        }
    },
    "env": {
        "storageFolder": {
            "value": "<ModuleStoragePath>"
        }
    },
    "status": "running",
    "restartPolicy": "always"
}
```

Reemplace `<HostStoragePath>` y `<ModuleStoragePath>` con las rutas de almacenamiento de su host y de su módulo; ambas rutas deben ser una ruta de acceso absoluta. En las opciones de creación, enlace juntas las rutas de acceso de almacenamiento de host y del módulo. A continuación, cree una variable de entorno que apunte a la ruta de acceso de almacenamiento del módulo.  

Por ejemplo, `"Binds":["/etc/iotedge/storage/:/iotedge/storage/"]` significa que el directorio **/etc/iotedge/storage** en el sistema host se asigna al directorio **/iotedge/storage/** en el contenedor. O en otro ejemplo para sistemas Windows, `"Binds":["C:\\temp:C:\\contemp"]` significa que el directorio **C:\\temp** en el sistema host se asigna al directorio **C:\\contemp** en el contenedor. 

También puede encontrar más detalles sobre las opciones de creación en la [documentación de Docker](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate).

## <a name="next-steps"></a>Pasos siguientes

Habilitar operaciones sin conexión ampliadas en escenarios de [puerta de enlace transparente](how-to-create-transparent-gateway.md).
