---
title: Información sobre el entorno de ejecución de Azure IoT Edge | Microsoft Docs
description: Información sobre el entorno de ejecución de Azure IoT Edge y cómo aumenta la capacidad de trabajo de los dispositivos perimetrales
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 08/13/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 9c9c04a8310a46605cf5733131db1418b7cb7f7a
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47218791"
---
# <a name="understand-the-azure-iot-edge-runtime-and-its-architecture"></a>Información del entorno de ejecución de Azure IoT Edge y su arquitectura

El entorno de ejecución de IoT Edge es una colección de programas que deben instalarse en un dispositivo para que se considere un dispositivo IoT Edge. Colectivamente, los componentes de la instancia de IoT Edge en tiempo de ejecución permiten que los dispositivos IoT Edge reciban el código para ejecutarse en el perímetro y comunican los resultados. 

La instancia de IoT Edge en tiempo de ejecución realiza las siguientes funciones en los dispositivos IoT Edge:

* Instala y actualiza cargas de trabajo en el dispositivo.
* Mantiene los estándares de seguridad de Azure IoT Edge en el dispositivo.
* Garantiza que los [módulos de IoT Edge][lnk-modules] estén siempre en ejecución.
* Informa del estado de los módulos a la nube para realizar la supervisión remota.
* Facilita la comunicación entre los dispositivos de hoja de nivel inferior y los dispositivos IoT Edge.
* Facilita la comunicación entre los módulos y el dispositivo IoT Edge.
* Facilita la comunicación entre el dispositivo IoT Edge y la nube.

![La instancia de IoT Edge en tiempo de ejecución comunica la información y el estado del módulo a IoT Hub][1]

Las responsabilidades del entorno de ejecución de IoT Edge se dividen en dos categorías: comunicación y administración de los módulos. Estas dos funciones las realizan dos componentes que constituyen la instancia en tiempo de ejecución de IoT Edge. El centro de IoT Edge es responsable de la comunicación, mientras que el agente de IoT Edge administra la implementación y supervisión de los módulos. 

El agente y el centro de Edge son módulos, como cualquier otro que se ejecuta en un dispositivo de IoT Edge. 

## <a name="iot-edge-hub"></a>Centro de IoT Edge

El centro de Edge es uno de los dos módulos que componen el entorno de ejecución de Azure IoT Edge. Actúa como un proxy local de IoT Hub exponiendo los mismos puntos de conexión de protocolo que IoT Hub. Esta coherencia significa que los clientes (dispositivos o módulos) pueden conectarse a la instancia de IoT Edge en tiempo de ejecución de la misma forma que lo harían a IoT Hub. 

>[!NOTE]
>Edge Hub admite clientes que se conectan mediante MQTT o AMQP. No admite los clientes que usan HTTP. 

No se trata de una versión completa de IoT Hub que se ejecuta localmente. Hay algunos procesos que el centro de Edge delega silenciosamente en IoT Hub. Por ejemplo, el centro de Edge reenvía las solicitudes de autenticación a IoT Hub la primera vez que un dispositivo trata de conectarse. Una vez establecida la primera conexión, el centro de Edge almacena la información de seguridad en caché localmente. Las conexiones posteriores desde dicho dispositivo se permiten sin tener que autenticarse en la nube. 

>[!NOTE]
>El entorno de tiempo de ejecución debe estar conectado cada vez que trata de autenticar un dispositivo.

Para reducir el ancho de banda que usa la solución IoT Edge, el centro de Edge optimiza el número real de conexiones a la nube. El centro de Edge toma las conexiones lógicas de clientes, como módulos o dispositivos de hoja, y las combina para crear una sola conexión física a la nube. Los detalles de este proceso son transparentes para el resto de la solución. Los clientes creen que tienen su propia conexión a la nube, aunque todos los datos van a enviarse a través de la misma. 

![El centro de Edge actúa como una puerta de enlace entre varios dispositivos físicos y la nube][2]

El centro de Edge puede determinar si está conectado a IoT Hub. Si se pierde la conexión, el centro de Edge guarda los mensajes o las actualizaciones gemelas localmente. Una vez que se vuelva a establecer una conexión, se sincronizan todos los datos. La ubicación que utiliza esta caché temporal viene determinada por una propiedad del módulo gemelo del centro de Edge. El tamaño de la caché no está limitado y aumentará siempre y cuando el dispositivo tenga capacidad de almacenamiento. 

### <a name="module-communication"></a>Comunicación entre módulos

El centro de Edge facilita la comunicación entre módulos. Al usar el centro de Edge como un agente de mensajes, los módulos seguirán siendo independientes entre sí. Los módulos solo necesitan especificar las entradas en las que aceptan mensajes y las salidas en las que los escriben. Después, un desarrollador de soluciones une estas entradas y salidas para que los módulos procesen los datos en el orden específico de esa solución. 

![El centro de Edge facilita la comunicación entre módulos][3]

Para enviar datos al centro de Edge, un módulo llama al método SendEventAsync. El primer argumento especifica en qué salida enviará el mensaje. El siguiente seudocódigo envía un mensaje a la salida output1:

   ```csharp
   ModuleClient client = new ModuleClient.CreateFromEnvironmentAsync(transportSettings); 
   await client.OpenAsync(); 
   await client.SendEventAsync(“output1”, message); 
   ```

Para recibir un mensaje, registre una devolución de llamada que procese los mensajes procedentes de una entrada específica. El siguiente seudocódigo registra la función messageProcessor, que se usará para procesar todos los mensajes recibidos en la entrada input1:

   ```csharp
   await client.SetEventHandlerAsync(“input1”, messageProcessor, userContext);
   ```

El desarrollador de soluciones es responsable de especificar las reglas que determinan cómo el centro de Edge pasa los mensajes entre los módulos. Las reglas de enrutamiento se definen en la nube y se envían al centro de Edge de su dispositivo gemelo. Se utiliza la misma sintaxis de las rutas de IoT Hub para definir las rutas entre módulos de Azure IoT Edge. 

<!--- For more info on how to declare routes between modules, see []. --->   

![Rutas entre módulos][4]

## <a name="iot-edge-agent"></a>Agente de IoT Edge

El agente de IoT Edge es el otro módulo que constituye el entorno de ejecución de Azure IoT Edge. Es responsable de crear instancias de los módulos, lo que garantiza que continúen ejecutándose y notificando el estado de los módulos a IoT Hub. Al igual que cualquier otro módulo, el agente de Edge usa su módulo gemelo para almacenar estos datos de configuración. 

El [demonio de seguridad de IoT Edge](iot-edge-security-manager.md) inicia el agente de Edge durante el inicio del dispositivo. El agente recupera su módulo gemelo de IoT Hub e inspecciona el manifiesto de implementación. El manifiesto de implementación es un archivo JSON que declara al módulo que debe iniciarse. 

Cada elemento del manifiesto de implementación contiene información específica de un módulo y el agente de Edge lo utiliza para controlar el ciclo de vida del módulo. Estas son algunas de las propiedades más interesantes: 

* **Settings.Image**: la imagen de contenedor que utiliza el agente de Edge al iniciar el módulo. El agente de Edge debe configurarse con las credenciales del registro de contenedor si la imagen está protegida por una contraseña. Las credenciales para el registro de contenedor se pueden configurar mediante el manifiesto de implementación de forma remota o en el propio dispositivo Edge actualizando el archivo `config.yaml` en la carpeta del programa IoT Edge.
* **settings.createOptions**: una cadena que se pasa directamente al demonio de Docker cuando se inicia un contenedor del módulo. Al agregar las opciones de Docker en esta propiedad, se pueden obtener opciones avanzadas como reenvío de puertos o montaje de volúmenes en el contenedor de un módulo.  
* **status**: el estado en el que el agente de Edge pone el módulo. Este valor se establece normalmente en *running*, ya que la mayoría de los usuarios quieren que el agente de Edge inicie inmediatamente todos los módulos del dispositivo. Sin embargo, puede especificar que el estado inicial de un módulo sea stopped y esperar a indicar más adelante al agente de Edge que inicie un módulo. El agente de Edge notifica el estado de cada módulo a la nube en las propiedades notificadas. Una diferencia entre la propiedad deseada y la notificada es un indicador de un dispositivo con un comportamiento incorrecto. Los estados admitidos son los siguientes:
   * Downloading (Descargando)
   * En ejecución
   * Unhealthy (Incorrecto)
   * Con error
   * Stopped
* **restartPolicy**: cómo el agente de Edge reinicia un módulo. Los valores posibles son:
   * Never: el agente de Edge nunca reinicia el módulo.
   * onFailure: si se bloquea el módulo, el agente de Edge lo reinicia. Si el módulo se cierra sin problemas, el agente de Edge no lo reinicia.
   * Unhealthy: si el módulo se bloquea o se consideran incorrecto, el agente de Edge lo reinicia.
   * Always: si el módulo se bloquea, se considera incorrecto o se apaga, el agente de Edge lo reinicia. 

El agente de IoT Edge envía la respuesta de entorno de ejecución a IoT Hub. A continuación, se ofrece una lista de las posibles respuestas:
  * 200 - CORRECTO
  * 400 - La configuración de implementación tiene un formato incorrecto o no es válida.
  * 417 - El dispositivo no tiene un conjunto de configuración de implementación.
  * 412 - La versión del esquema de la configuración de implementación no es válida.
  * 406 - El dispositivo perimetral está sin conexión o no envía informes de estado.
  * 500 - Error en el tiempo de ejecución de Edge.

### <a name="security"></a>Seguridad

El agente de IoT Edge desempeña un papel fundamental en la seguridad de un dispositivo IoT Edge. Por ejemplo, realiza acciones como comprobar la imagen de un módulo antes de iniciarlo. 

Para obtener más información sobre el marco de seguridad de Azure IoT Edge, consulte [Administrador de seguridad de IoT Edge](iot-edge-security-manager.md)

## <a name="next-steps"></a>Pasos siguientes

[Información sobre los certificados de Azure IoT Edge][lnk-certs]

<!-- Images -->
[1]: ./media/iot-edge-runtime/Pipeline.png
[2]: ./media/iot-edge-runtime/Gateway.png
[3]: ./media/iot-edge-runtime/ModuleEndpoints.png
[4]: ./media/iot-edge-runtime/ModuleEndpointsWithRoutes.png

<!-- Links -->
[lnk-certs]: iot-edge-certs.md
