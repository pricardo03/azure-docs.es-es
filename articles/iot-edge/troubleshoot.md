---
title: Solución de problemas en Azure IoT Edge | Microsoft Docs
description: Resolución de problemas habituales y adquisición de conocimientos sobre solución de problemas para Azure IoT Edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/26/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: efe3e31a1a92e21f2c3a3461deba248d2a8c97fa
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37029448"
---
# <a name="common-issues-and-resolutions-for-azure-iot-edge"></a>Problemas habituales y soluciones para Azure IoT Edge

Si experimenta problemas al ejecutar Azure IoT Edge en el entorno, use este artículo como guía para solucionarlos. 

## <a name="standard-diagnostic-steps"></a>Pasos de diagnóstico estándar 

Si detecta un problema, consiga más información sobre el estado del dispositivo de IoT Edge mediante la revisión de los registros de contenedor y de los mensajes que se envían hacia el dispositivo y desde este. Use los comandos y las herramientas de esta sección para recopilar información. 

### <a name="check-the-status-of-the-iot-edge-security-manager-and-its-logs"></a>Comprobación del estado del administrador de seguridad de IoT Edge y sus registros

En Linux:
- Para ver el estado del administrador de seguridad de IoT Edge:

   ```bash
   sudo systemctl status iotedge
   ```

- Para ver los registros del administrador de seguridad de IoT Edge:

    ```bash
    sudo journalctl -u iotedge -f
    ```

- Para ver registros más detallados del administrador de seguridad de IoT Edge:

   - Edite la configuración del demonio de iotedge:

      ```bash
      sudo systemctl edit iotedge.service
      ```
   
   - Actualice las líneas siguientes:
    
      ```
      [Service]
      Environment=IOTEDGE_LOG=edgelet=debug
      ```
    
   - Reinicie el demonio de seguridad de IoT Edge:
    
      ```bash
      sudo systemctl cat iotedge.service
      sudo systemctl daemon-reload
      sudo systemctl restart iotedge
      ```

En Windows:
- Para ver el estado del administrador de seguridad de IoT Edge:

   ```powershell
   Get-Service iotedge
   ```

- Para ver los registros del administrador de seguridad de IoT Edge:

   ```powershell
   # Displays logs from today, newest at the bottom.
 
   Get-WinEvent -ea SilentlyContinue `
   -FilterHashtable @{ProviderName= "iotedged";
     LogName = "application"; StartTime = [datetime]::Today} |
   select TimeCreated, Message |
   sort-object @{Expression="TimeCreated";Descending=$false}
   ```

### <a name="if-the-iot-edge-security-manager-is-not-running-verify-your-yaml-configuration-file"></a>Comprobación del archivo de configuración de YAML si no se está ejecutando el administrador de seguridad de IoT Edge

> [!WARNING]
> Los archivos de YAML no pueden contener tabulaciones como sangría. Utilice en su lugar dos espacios.

En Linux:

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

En Windows:

   ```cmd
   notepad C:\ProgramData\iotedge\config.yaml
   ```

### <a name="check-container-logs-for-issues"></a>Comprobación de los registros del contenedor para detectar problemas

Cuando el demonio de seguridad de IoT Edge se esté ejecutando, examine los registros de los contenedores para detectar problemas. Empiece por los contenedores implementados y, a continuación, examine los contenedores que componen el entorno de ejecución de IoT Edge: agente de Edge y centro de Edge. Los registros del agente de Edge normalmente proporcionan información sobre el ciclo de vida de cada contenedor. Los registros del centro de Edge proporcionan información sobre mensajería y enrutamiento. 

   ```cmd
   iotedge logs <container name>
   ```

### <a name="view-the-messages-going-through-the-edge-hub"></a>Visualización de mensajes que se envían mediante el centro de Edge

Vea los mensajes que se envían mediante el centro de Edge y recopile información sobre las actualizaciones de las propiedades del dispositivo con los registros detallados procedentes de los contenedores del entorno de ejecución de edgeAgent y edgeHub. Para habilitar los registros detallados en estos contenedores, establezca la variable de entorno `RuntimeLogLevel`: 

En Linux:
    
   ```cmd
   export RuntimeLogLevel="debug"
   ```
    
En Windows:
    
   ```powershell
   [Environment]::SetEnvironmentVariable("RuntimeLogLevel", "debug")
   ```

También puede comprobar los mensajes que se envían entre los dispositivos de IoT Hub e IoT Edge. Vea estos mensajes mediante la extensión de [Azure IoT Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) para Visual Studio Code. Para más instrucciones, consulte [Handy tool when you develop with Azure IoT](https://blogs.msdn.microsoft.com/iotdev/2017/09/01/handy-tool-when-you-develop-with-azure-iot/) (Herramienta práctica para desarrollar con Azure IoT).

### <a name="restart-containers"></a>Reinicio de los contenedores
Después de investigar los registros y mensajes para obtener información, puede intentar reiniciar los contenedores:

```
iotedge restart <container name>
```

Reinicie los contenedores del entorno de ejecución de IoT Edge:

```
iotedge restart edgeAgent && iotedge restart edgeHub
```

### <a name="restart-the-iot-edge-security-manager"></a>Reinicio del administrador de seguridad de IoT Edge

Si el problema persiste, puede probar a reiniciar el administrador de seguridad de IoT Edge.

En Linux:

   ```cmd
   sudo systemctl restart iotedge
   ```

En Windows:

   ```powershell
   Stop-Service iotedge -NoWait
   sleep 5
   Start-Service iotedge
   ```

## <a name="edge-agent-stops-after-about-a-minute"></a>El agente de Edge se detiene después de aproximadamente un minuto

El agente de Edge se inicia y se ejecuta correctamente durante un minuto aproximadamente y, a continuación, se detiene. Los registros indican que el agente de Edge está intentando conectarse a IoT Hub a través de AMQP y aproximadamente 30 segundos después intenta conectarse mediante AMQP con el protocolo Websocket. Cuando se produce un error, se cierra el agente de Edge. 

Registros de ejemplo de Edge:

```output
2017-11-28 18:46:19 [INF] - Starting module management agent. 
2017-11-28 18:46:19 [INF] - Version - 1.0.7516610 (03c94f85d0833a861a43c669842f0817924911d5) 
2017-11-28 18:46:19 [INF] - Edge agent attempting to connect to IoT Hub via AMQP... 
2017-11-28 18:46:49 [INF] - Edge agent attempting to connect to IoT Hub via AMQP over WebSocket... 
```

### <a name="root-cause"></a>Causa principal
Una configuración de redes en la red del host está impidiendo que el agente de Edge alcance la red. El agente intentará conectarse en primer lugar a través de AMQP (puerto 5671). Si se produce un error, lo intentará mediante los protocolos websocket (puerto 443).

El entorno de ejecución de IoT Edge configura una red para cada uno de los módulos en los que se comunica. En Linux, esta red es una red de puente. En Windows, se usa NAT. Este problema es más frecuente en los dispositivos de Windows que usan contenedores de Windows que usan la red NAT. 

### <a name="resolution"></a>Resolución
Asegúrese de que hay una ruta a Internet para las direcciones IP asignadas a esta red de puente o NAT. A veces, una configuración de VPN en el host invalida la red de IoT Edge. 

## <a name="edge-hub-fails-to-start"></a>No se puede iniciar el centro de Edge

Se produce un error al iniciar el centro de Edge y se imprime el siguiente mensaje en los registros: 

```output
One or more errors occurred. 
(Docker API responded with status code=InternalServerError, response=
{\"message\":\"driver failed programming external connectivity on endpoint edgeHub (6a82e5e994bab5187939049684fb64efe07606d2bb8a4cc5655b2a9bad5f8c80): 
Error starting userland proxy: Bind for 0.0.0.0:443 failed: port is already allocated\"}\n) 
```

### <a name="root-cause"></a>Causa principal
Algún otro proceso del equipo host se ha enlazado con el puerto 443. El centro de Edge asigna los puertos 5671 y 443 para su uso en escenarios de puerta de enlace. Se producirá un error en la asignación de puertos si hay algún otro proceso enlazado con este puerto. 

### <a name="resolution"></a>Resolución
Busque y detenga el proceso que está utilizando el puerto 443. Este proceso suele ser un servidor web.

## <a name="edge-agent-cant-access-a-modules-image-403"></a>El agente de Edge no puede acceder a la imagen de un módulo (403)
No se puede ejecutar un contenedor y los registros del agente de Edge muestran un error 403. 

### <a name="root-cause"></a>Causa principal
El agente de Edge no tiene permisos para acceder a la imagen de un módulo. 

### <a name="resolution"></a>Resolución
Asegúrese de que las credenciales del registro se hayan especificado correctamente en el manifiesto de implementación.

## <a name="iot-edge-security-daemon-fails-with-an-invalid-hostname"></a>Error de nombre de host no válido del demonio de seguridad de IoT Edge

El comando `sudo journalctl -u iotedge` produce un error e imprime el mensaje siguiente: 

```output
Error parsing user input data: invalid hostname. Hostname cannot be empty or greater than 64 characters
```

### <a name="root-cause"></a>Causa principal
El tiempo de ejecución de IoT Edge solo puede admitir los nombres de host que tienen menos de 64 caracteres. Normalmente, esto no supone un problema para las máquinas físicas, pero puede generarse al configurar el tiempo de ejecución en una máquina virtual. Los nombres de host generados automáticamente para las máquinas virtuales Windows hospedadas en Azure, en particular, suelen ser largos. 

### <a name="resolution"></a>Resolución
Cuando vea este error, puede resolverlo configurando el nombre DNS de la máquina virtual y, a continuación, estableciendo el nombre DNS como nombre de host en el comando de configuración.

1. En Azure Portal, navegue a la hoja de introducción de la máquina virtual. 
2. Seleccione **configurar** en el nombre DNS. Si la máquina virtual ya tiene configurado un nombre DNS, no es necesario configurar uno nuevo. 

   ![Configuración del nombre DNS](./media/troubleshoot/configure-dns.png)

3. Proporcione un valor para **Etiqueta de nombre DNS** y seleccione **Guardar**.
4. Copie el nuevo nombre DNS, que debe tener el formato **\<etiquetanombreDNS\>.\< ubicaciónvm\>.cloudapp.azure.com**.
5. Dentro de la máquina virtual, use el comando siguiente para configurar el tiempo de ejecución de IoT Edge con el nombre DNS:

   - En Linux:

      ```bash
      sudo nano /etc/iotedge/config.yaml
      ```

   - En Windows:

      ```cmd
      notepad C:\ProgramData\iotedge\config.yaml
      ```

## <a name="next-steps"></a>Pasos siguientes
¿Cree que encontró un error en la plataforma de IoT Edge? Use la opción [Enviar un problema](https://github.com/Azure/iot-edge/issues) para que podamos seguir mejorando. 
