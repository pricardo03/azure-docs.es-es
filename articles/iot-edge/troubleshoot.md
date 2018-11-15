---
title: Solución de problemas en Azure IoT Edge | Microsoft Docs
description: Resolución de problemas habituales y adquisición de conocimientos sobre solución de problemas para Azure IoT Edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/26/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: a553798a3ac15340805984a0e87312875f82c46c
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2018
ms.locfileid: "51567662"
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
   sort-object @{Expression="TimeCreated";Descending=$false} |
   format-table -autosize -wrap
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

Puede ver los mensajes que se envían mediante el centro de Edge y recopilar la información que se encuentra en los registros detallados procedentes de los contenedores del entorno de ejecución. Para activar los registros detallados en estos contenedores, establezca `RuntimeLogLevel` en el archivo de configuración yaml. Para abrir el archivo:

En Linux:

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

En Windows:

   ```cmd
   notepad C:\ProgramData\iotedge\config.yaml
   ```

De forma predeterminada, el elemento `agent` tendrá un aspecto similar al del ejemplo siguiente:

   ```yaml
   agent:
     name: edgeAgent
     type: docker
     env: {}
     config:
       image: mcr.microsoft.com/azureiotedge-agent:1.0
       auth: {}
   ```

Reemplace `env: {}` por:

> [!WARNING]
> Los archivos de YAML no pueden contener tabulaciones como sangría. Utilice en su lugar dos espacios.

   ```yaml
   env:
     RuntimeLogLevel: debug
   ```

Guarde el archivo y reinicie el administrador de seguridad de IoT Edge.

También puede comprobar los mensajes que se envían entre los dispositivos de IoT Hub e IoT Edge. Vea estos mensajes mediante la extensión de [Azure IoT Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) para Visual Studio Code. Para más información, consulte [Handy tool when you develop with Azure IoT](https://blogs.msdn.microsoft.com/iotdev/2017/09/01/handy-tool-when-you-develop-with-azure-iot/) (Herramienta práctica para desarrollar con Azure IoT).

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

El agente de Edge se inicia y se ejecuta correctamente durante un minuto aproximadamente y, a continuación, se detiene. Los registros indican que el agente de Edge intenta conectarse a IoT Hub a través de AMQP y después intenta conectarse mediante AMQP con el protocolo WebSocket. Cuando se produce un error, se cierra el agente de Edge. 

Registros de ejemplo de Edge:

```output
2017-11-28 18:46:19 [INF] - Starting module management agent. 
2017-11-28 18:46:19 [INF] - Version - 1.0.7516610 (03c94f85d0833a861a43c669842f0817924911d5) 
2017-11-28 18:46:19 [INF] - Edge agent attempting to connect to IoT Hub via AMQP... 
2017-11-28 18:46:49 [INF] - Edge agent attempting to connect to IoT Hub via AMQP over WebSocket... 
```

### <a name="root-cause"></a>Causa principal
Una configuración de redes en la red del host está impidiendo que el agente de Edge alcance la red. El agente intentará conectarse en primer lugar a través de AMQP (puerto 5671). Si se produce un error en la conexión, lo intentará mediante los protocolos WebSocket (puerto 443).

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
El tiempo de ejecución de IoT Edge solo puede admitir los nombres de host que tienen menos de 64 caracteres. Las máquinas físicas no suelen tener nombres de host largos, pero el problema es más común en una máquina virtual. Los nombres de host generados automáticamente para las máquinas virtuales Windows hospedadas en Azure, en particular, suelen ser largos. 

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

## <a name="stability-issues-on-resource-constrained-devices"></a>Problemas de estabilidad en dispositivos con recursos limitados 
Es posible que encuentre problemas de estabilidad en dispositivos restringidos como Raspberry Pi, especialmente cuando se utiliza como puerta de enlace. Los síntomas incluyen excepciones de memoria insuficiente en el módulo del concentrador perimetral, los dispositivos de nivel inferior no pueden conectarse o el dispositivo deja de enviar mensajes de telemetría después de unas horas.

### <a name="root-cause"></a>Causa principal
El centro de Edge, que forma parte del entorno de ejecución perimetral, está optimizado para el rendimiento de manera predeterminada y se intenta asignar grandes fragmentos de memoria. Esta optimización no es ideal para dispositivos con perímetro limitado y puede causar problemas de estabilidad.

### <a name="resolution"></a>Resolución
Para el centro de Edge, se establece una variable de entorno **OptimizeForPerformance** en **false**. Existen dos formas de hacerlo:

En la interfaz de usuario: 

En el portal de *Detalles del dispositivo*->*Establecer módulos*->*Configurar las opciones avanzadas del entorno en tiempo de ejecución de Edge*, cree una variable de entorno llamada *OptimizeForPerformance* que se establece en *false* para el *centro de Edge*.

![optimizeforperformance](./media/troubleshoot/OptimizeForPerformanceFalse.png)

**O bien**

En el manifiesto de implementación:

```json
  "edgeHub": {
    "type": "docker",
    "settings": {
      "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
      "createOptions": <snipped>
    },
    "env": {
      "OptimizeForPerformance": {
          "value": "false"
      }
    },
```
## <a name="cant-get-the-iot-edge-daemon-logs-on-windows"></a>No se obtienen los registros del demonio de IoT Edge en Windows
Si aparece EventLogException al usar`Get-WinEvent` en Windows, compruebe las entradas del registro.

### <a name="root-cause"></a>Causa principal
El comando de PowerShell `Get-WinEvent` se basa en que esté presente una entrada del Registro para encontrar los registros por un elemento `ProviderName` concreto.

### <a name="resolution"></a>Resolución
Establezca una entrada del registro para el demonio de IoT Edge. Cree un archivo **iotedge.reg** con el siguiente contenido e impórtelo en el Registro de Windows haciendo doble clic en él o mediante el comando `reg import iotedge.reg`:

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Application\iotedged]
"CustomSource"=dword:00000001
"EventMessageFile"="C:\\ProgramData\\iotedge\\iotedged.exe"
"TypesSupported"=dword:00000007
```

## <a name="iot-edge-module-fails-to-send-a-message-to-the-edgehub-with-404-error"></a>El módulo de IoT Edge no puede enviar un mensaje a edgeHub y se produce el error 404

El módulo de IoT Edge personalizado no puede enviar un mensaje a edgeHub y se produce el error 404 `Module not found`. El demonio de IoT Edge imprime el mensaje siguiente en los registros: 

```output
Error: Time:Thu Jun  4 19:44:58 2018 File:/usr/sdk/src/c/provisioning_client/adapters/hsm_client_http_edge.c Func:on_edge_hsm_http_recv Line:364 executing HTTP request fails, status=404, response_buffer={"message":"Module not found"}u, 04 ) 
```

### <a name="root-cause"></a>Causa principal
El demonio de IoT Edge aplica la identificación de proceso para todos los módulos que se conectan a edgeHub por motivos de seguridad. Comprueba que todos los mensajes enviados por un módulo proceden del identificador de proceso principal del módulo. Si un módulo envía un mensaje desde un identificador de proceso diferente del que se estableció inicialmente, se rechazará el mensaje y se generará el mensaje de error 404.

### <a name="resolution"></a>Resolución
Asegúrese de que el módulo de IoT Edge personalizado siempre usa el mismo identificador de proceso para enviar los mensajes a edgeHub. Por ejemplo, asegúrese de usar `ENTRYPOINT` en lugar del comando `CMD` en el archivo de Docker, ya que `CMD` dará lugar a un identificador de proceso para el módulo y a otro identificador de proceso para el comando bash que ejecuta el programa principal, mientras que `ENTRYPOINT` dará lugar a un identificador de proceso único.


## <a name="firewall-and-port-configuration-rules-for-iot-edge-deployment"></a>Reglas de configuración de puertos y firewall para la implementación de IoT Edge
Azure IoT Edge permite la comunicación desde un servidor perimetral de un entorno local a Azure en la nube mediante protocolos de IoT Hub compatibles; consulte cómo [elegir un protocolo de comunicación](../iot-hub/iot-hub-devguide-protocols.md). Para mejorar la seguridad, los canales de comunicación entre Azure IoT Edge y Azure IoT Hub siempre están configurados para que sea la salida. Esta configuración se basa en el [patrón de comunicación asistida de servicios](https://blogs.msdn.microsoft.com/clemensv/2014/02/09/service-assisted-communication-for-connected-devices/), que minimiza la superficie de ataque que una entidad malintencionada puede explorar. La comunicación entrante solo es necesaria para escenarios específicos donde Azure IoT Hub necesita insertar mensajes en el dispositivo de Azure IoT Edge. Los mensajes de nube a dispositivo están protegidos mediante canales TLS seguros y pueden protegerse aún más mediante los certificados X.509 y los módulos de dispositivos TPM. El Administrador de seguridad de Azure IoT Edge rige cómo se puede establecer esta comunicación; consulte [Administrador de seguridad de IoT Edge](../iot-edge/iot-edge-security-manager.md).

Aunque IoT Edge permite una mejor configuración para proteger el entorno de ejecución de Azure IoT Edge y los módulos implementados, todavía depende de la configuración de la máquina y la red subyacentes. Por lo tanto, es fundamental garantizar que las reglas adecuadas de red y del firewall se configuran para establecer una comunicación segura de Edge en la nube. Lo siguiente puede utilizarse como guía al configurar las reglas de firewall para los servidores subyacentes donde se hospeda el entorno de ejecución de Azure IoT Edge:

|Protocolo|Port|Entrante|Saliente|Guía|
|--|--|--|--|--|
|MQTT|8883|BLOQUEADO (valor predeterminado)|BLOQUEADO (valor predeterminado)|<ul> <li>Configure el valor de Saliente (de salida) para que sea Abierto cuando se usa MQTT como protocolo de comunicación.<li>1883 para MQTT no es compatible con IoT Edge. <li>Se deben bloquear las conexiones entrantes (de entrada).</ul>|
|AMQP|5671|BLOQUEADO (valor predeterminado)|ABIERTO (valor predeterminado)|<ul> <li>Protocolo de comunicación predeterminado de IoT Edge. <li> Debe configurarse para ser Abierto si Azure IoT Edge no está configurado para otros protocolos compatibles o AMQP es el protocolo de comunicación que se desea.<li>5672 para AMQP no es compatible con IoT Edge.<li>Bloquee este puerto cuando Azure IoT Edge use un protocolo compatible de IoT Hub diferente.<li>Se deben bloquear las conexiones entrantes (de entrada).</ul></ul>|
|HTTPS|443|BLOQUEADO (valor predeterminado)|ABIERTO (valor predeterminado)|<ul> <li>Configure el valor de las conexiones salientes (de salida) para que sea abierto en 443 para el aprovisionamiento de IoT Edge. Esta configuración es necesaria cuando se usen scripts manuales o Azure IoT Device Provisioning Service (DPS). <li>La conexión entrante (de entrada) solo debe ser Abierta en escenarios concretos: <ul> <li>  Si tiene una puerta de enlace transparente con dispositivos de hoja que puedan enviar solicitudes de método. En este caso, no es necesario que el puerto 443 esté abierto a las redes externas para conectarse a IOT Hub ni proporcionar servicios de IoTHub mediante Azure IoT Edge. Por lo tanto, la regla de entrada podría limitarse solo a los puertos entrantes (de entrada) abiertos desde la red interna. <li> En el caso de escenarios de cliente a dispositivo (C2D).</ul><li>80 para HTTP no es compatible con IoT Edge.<li>Si no se pueden configurar los protocolos que no sean HTTP (por ejemplo, AMQP o MQTT) en la empresa; los mensajes pueden enviarse a través de WebSockets. En ese caso, el puerto 443 se utilizará para la comunicación de WebSocket.</ul>|


## <a name="next-steps"></a>Pasos siguientes
¿Cree que encontró un error en la plataforma de IoT Edge? [Envíe un problema](https://github.com/Azure/iotedge/issues) para que podamos seguir mejorando. 

Si tiene más preguntas, cree una [solicitud de soporte técnico](https://portal.azure.com/#create/Microsoft.Support) para obtener ayuda. 

