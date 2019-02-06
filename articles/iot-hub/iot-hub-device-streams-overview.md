---
title: Flujos de dispositivos Azure IoT Hub (versión preliminar) | Microsoft Docs
description: Información general sobre los flujos de dispositivos IoT Hub.
author: rezasherafat
manager: briz
services: iot-hub
ms.service: iot-hub
ms.topic: conceptual
ms.date: 01/15/2019
ms.author: rezas
ms.openlocfilehash: 426c8995e5c3d98e42d0ad334b8ae52171556dce
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54884969"
---
# <a name="iot-hub-device-streams-preview"></a>Flujos de dispositivos IoT Hub (versión preliminar)

## <a name="overview"></a>Información general
Los *flujos de dispositivos* Azure IoT Hub facilitan la creación de túneles TCP seguros y bidireccionales para diversos escenarios de comunicación de la nube al dispositivo. Un flujo de dispositivos se realiza mediante un *punto de conexión de streaming* de IoT Hub que actúa como proxy entre el dispositivo y los puntos de conexión de servicio. Esta configuración, que se ilustra en el diagrama siguiente, resulta especialmente útil cuando los dispositivos están detrás de un firewall de red o se encuentran dentro de una red privada. Por tanto, los flujos de dispositivos IoT Hub ayudan a enfrentar la necesidad que tienen los clientes de llegar a los dispositivos IoT de manera compatible con el firewall y sin tener que abrir ampliamente los puertos de entrada y salida del firewall de la red.

![Texto alternativo](./media/iot-hub-device-streams-overview/iot-hub-device-streams-overview.png "Introducción a los flujos de dispositivos de IoT Hub")


Con los flujos de dispositivos IoT Hub, los dispositivos permanecen protegidos y solo se tendrán que abrir conexiones TCP de salida al punto de conexión de streaming del centro de IoT a través del puerto 443. Una vez que se establece un flujo, cada una de las aplicaciones del lado del servicio y del lado del dispositivo tendrá acceso mediante programación a un objeto del cliente de WebSocket para enviar y recibir bytes sin formato entre sí. Las garantías de orden y la confiabilidad que este túnel proporciona son equivalentes a las de TCP.

## <a name="benefits"></a>Ventajas
Los flujos de dispositivos de IoT Hub ofrecen las siguientes ventajas:
- **Conectividad segura apta para firewall:** es posible alcanzar los dispositivos de IoT desde los puntos de conexión de servicio sin abrir el puerto de entrada del firewall en los perímetros de la red o el dispositivo (solo se necesita conectividad de salida a IoT Hub a través del puerto 443).

- **Autenticación:** tanto el lado del dispositivo como el lado del servicio del túnel se deben autenticar con IoT Hub mediante las credenciales correspondientes.

- **Cifrado:** de forma predeterminada, los flujos de dispositivos de IoT Hub utilizan conexiones habilitadas para TLS. Esto garantiza que el tráfico siempre esté cifrado, independientemente de si la aplicación utiliza o no el cifrado.

- **Simplicidad de conectividad:** en muchos casos, el uso de flujos de datos de dispositivos elimina la necesidad de realizar una configuración compleja de redes privadas virtuales para habilitar la conectividad con dispositivos de IoT.

- **Compatibilidad con la pila TCP/IP:** los flujos de dispositivos de IoT Hub pueden acomodar el tráfico de aplicaciones TCP/IP. Esto significa que una amplia gama de protocolos propietarios y basados ​​en estándares pueden aprovechar esta función.

- **Facilidad de uso en configuraciones de redes privadas:** para comunicarse con un dispositivo, el servicio hace referencia a su identificador de dispositivo en lugar de a su dirección IP. Esto resulta útil cuando un dispositivo se encuentra dentro de una red privada y tiene una dirección IP privada, o bien cuando su dirección IP se asigna de manera dinámica y es desconocida para el lado del servicio.

## <a name="device-stream-workflows"></a>Flujos de trabajo de los flujos de dispositivos
Un flujo de dispositivos se inicia cuando el servicio solicita conectarse a un dispositivo al proporcionar su id. de dispositivo. Este flujo de trabajo en particular se adapta al modelo de comunicación cliente/servidor, lo que incluye a SSH y RDP, donde un usuario intenta conectarse de manera remota al servidor SSH o RDP que se ejecuta en el dispositivo utilizando un programa cliente SSH o RDP.

El proceso de creación de flujo de dispositivos implica una negociación entre el dispositivo, el servicio, el punto de conexión principal y los puntos de conexión de streaming del centro de IoT. Si bien el punto de conexión principal del centro de IoT organiza la creación de un flujo de dispositivos, el punto de conexión de streaming controla el tráfico que fluye entre el servicio y el dispositivo.

### <a name="device-stream-creation-flow"></a>Flujo de creación de los flujos de dispositivos
La creación mediante programación de un flujo de dispositivos a través del SDK implica estos pasos, los que también se muestran en la ilustración siguiente:

![Texto alternativo](./media/iot-hub-device-streams-overview/iot-hub-device-streams-handshake.png "Proceso del protocolo de enlace del flujo de dispositivos")


1. La aplicación de dispositivo registra una devolución de llamada de antemano para recibir notificaciones cuando se inicie un flujo de dispositivos nuevo al dispositivo. Por lo general, ese paso ocurre cuando el dispositivo se inicia y se conecta a IoT Hub.

2. El programa del lado del servicio proporciona el id. de dispositivo (_no_ la dirección IP) para iniciar un flujo de dispositivos cuando sea necesario.

3. Para notificar al programa del lado del dispositivo, el centro de IoT invoca la devolución de llamada registrada en el paso 1. El dispositivo puede aceptar o rechazar la solicitud de iniciación del flujo. Esta lógica puede ser específica para el escenario de la aplicación. Si el dispositivo rechaza la solicitud de flujo, IoT Hub se lo informa al servicio como corresponde; de lo contrario, se realizan los pasos siguientes.

4. El dispositivo crea una conexión TCP de salida segura al punto de conexión de streaming a través del puerto 443 y actualiza la conexión a un WebSocket. IoT Hub proporciona al dispositivo la dirección URL del punto de conexión de streaming, así como las credenciales usadas para realizar la autenticación, como parte de la solicitud enviada en el paso 3.

5. El servicio recibe una notificación del resultado cuando el dispositivo acepta el flujo y pasa a crear su propio cliente de WebSocket en el punto de conexión de streaming. De manera similar, recibe la información de autenticación y la dirección URL del punto de conexión de streaming desde IoT Hub.

En el proceso de protocolo de enlace anterior:
- El proceso de protocolo de enlace se debe completar en menos de 60 segundos (del paso 2 al paso 5) o el protocolo de enlace generará un error de tiempo de expiración y el servicio recibirá la notificación correspondiente.

- Una vez que se completa el flujo de creación del flujo anterior, el punto de conexión de streaming actuará como proxy y transferirá el tráfico entre el servicio y el dispositivo a través de sus respectivos WebSockets.

- Tanto el dispositivo como el servicio necesitan conectividad de salida al punto de conexión principal de IoT Hub, así como el punto de conexión de streaming a través del puerto 443. La dirección URL de estos puntos de conexión está disponible en la pestaña *Información general* del portal de IoT Hub.

- Las garantías de orden y la confiabilidad de un flujo establecido son equivalentes a las de TCP.

- Todas las conexiones a IoT Hub y el punto de conexión de streaming usan TLS y están cifradas.

### <a name="termination-flow"></a>Flujo de terminación
Un flujo establecido termina cuando se desconecta cualquiera de las conexiones TCP con la puerta de enlace (ya sea por el servicio o el dispositivo). Esto puede ocurrir voluntariamente si se cierra el WebSocket en el programa de dispositivo o en el programa de servicio, o bien de manera involuntaria en caso de un error de proceso o de tiempo de expiración de conectividad de red. Tras la finalización de cualquier conexión de dispositivo o servicio con el punto de conexión de streaming, la otra conexión TCP también finalizará (de manera forzada) y el servicio y el dispositivo serán responsables de volver a crear el flujo, en caso de que sea necesario.


## <a name="connectivity-requirements"></a>Requisitos de conectividad

Tanto el lado del dispositivo como del servicio de un flujo de dispositivos debe ser capaz de establecer conexiones habilitadas para TLS con IoT Hub y su punto de conexión de streaming. Esto requiere conectividad de salida a través del puerto 443 con estos puntos de conexión. El nombre de host asociado con estos puntos de conexión se puede encontrar en la pestaña *Información general* de IoT Hub, como se muestra en la ilustración siguiente: ![Texto alternativo](./media/iot-hub-device-streams-overview/device-stream-portal.png "Puntos de conexión del flujo de dispositivos")

Como alternativa, la información sobre los puntos de conexión se puede recuperar mediante la CLI de Azure en la sección de propiedades del centro, específicamente, con las teclas `property.hostname` y `property.deviceStreams`.

```azurecli-interactive
az iot hub show --name <YourIoTHubName>
```

## <a name="whitelist-device-streaming-endpoints"></a>Lista blanca de los puntos de conexión de streaming del dispositivo

Como se mencionó [anteriormente](#Overview), el dispositivo crea una conexión de salida al punto de conexión de streaming de IoT Hub durante el proceso de iniciación de los flujos de dispositivos. Los firewalls del dispositivo o de la red deben permitir la conectividad de salida hacia la puerta de enlace de streaming en el puerto 443 (tenga en cuenta que la comunicación se produce mediante una conexión WebSocket cifrada con TLS).

El nombre de host del punto de conexión de streaming se puede encontrar en el portal de Azure IoT Hub, en la pestaña Información general. ![Texto alternativo](./media/iot-hub-device-streams-overview/device-stream-portal.PNG "Puntos de conexión del flujo de dispositivos")

También puede encontrar esta información mediante la CLI de Azure:
```cmd/sh
az iot hub show --name <YourIoTHubName>
```

## <a name="troubleshoot-via-device-streams-activity-logs"></a>Solución de problemas a través de los registros de actividad de los flujos de dispositivos

Puede configurar Azure Log Analytics para recopilar el registro de actividad de los flujos de dispositivos en IoT Hub. Esto puede ser muy útil en los escenarios de solución de problemas.

Siga los pasos siguientes para configurar Azure Log Analytics según sus actividades de flujo de dispositivos de IoT Hub:

1. Vaya a la pestaña *Configuración de diagnóstico* en IoT Hub y haga clic en el vínculo *Activación del diagnóstico*.

  ![Texto alternativo](./media/iot-hub-device-streams-overview/device-streams-diagnostics-settings.PNG "Habilitación de los registros de diagnósticos")


2. Proporcione un nombre para la configuración de diagnóstico y elija la opción *Enviar a Log Analytics*. Se le guiará para que elija un recurso de Log Analytics existente o para que cree uno. Además, compruebe *DeviceStreams* de la lista.

    ![Texto alternativo](./media/iot-hub-device-streams-overview/device-streams-diagnostics.PNG "Habilitación de los registros de flujos de dispositivos")

3. Ahora puede acceder a los registros de flujos de dispositivos en la pestaña *Registros* del portal de IoT Hub. Los registros de actividad del flujo de dispositivos aparecerán en la tabla `AzureDiagnostics` y tendrán `Category=DeviceStreams`.

    <p>
Como se muestra a continuación, la identidad del dispositivo de destino y el resultado de la operación también están disponibles en los registros.
    ![Texto alternativo](./media/iot-hub-device-streams-overview/device-streams-log-analytics.PNG "Acceso a los registros de flujos de dispositivos")


## <a name="regional-availability"></a>Disponibilidad regional

Durante la versión preliminar pública, los flujos de datos de los dispositivos de IoT Hub estarán disponibles en las regiones Centro de EE. UU. y EUAP del centro de EE. UU. Asegúrese de que crea el centro en una de estas regiones. 


## <a name="sdk-availability"></a>Disponibilidad del SDK

Dos lados de cada flujo (en el lado del dispositivo y del servicio) usan el SDK de IoT Hub para establecer el túnel. Durante la versión preliminar pública, los clientes pueden elegir entre estos lenguajes del SDK:
- Los SDK de C y C# admiten flujos de dispositivos en el lado del dispositivo.

- Los SDK de NodeJS y C# admiten flujos de dispositivos en el lado del servicio.


## <a name="iot-hub-device-stream-samples"></a>Ejemplos de flujo de dispositivos de IoT Hub

Hemos publicado [dos ejemplos de inicio rápido](/azure/iot-hub) para mostrar el uso que hacen las aplicaciones de los flujos de datos de los dispositivos.
* En el ejemplo *echo*, se muestra el uso mediante programación de los flujos de datos de los dispositivos (llamando directamente a las API del SDK).
* En el ejemplo *local proxy*, se muestra el uso de la tunelización del tráfico de las aplicaciones cliente/servidor existentes (como aplicaciones SSH, RDP o web) mediante flujos de datos de dispositivos.

Estos ejemplos se tratan con más detalle a continuación.

### <a name="echo-sample"></a>Ejemplo Eco
En el ejemplo de echo, se muestra el uso mediante programación de flujos de datos de dispositivos para enviar y recibir bytes entre la aplicación del servicio y la aplicación del dispositivo. Use los vínculos siguientes para acceder a las guías de inicio rápido. Tenga en cuenta que los programas del servicio y del dispositivo pueden utilizar lenguajes distintos; por ejemplo, un programa de dispositivo escrito en C puede funcionar con un programa de servicio escrito en C#.

| SDK    | Programa de servicio                                          | Programa de dispositivo                                           |
|--------|----------------------------------------------------------|----------------------------------------------------------|
| C#     | [Vínculo](quickstart-device-streams-echo-csharp.md) | [Vínculo](quickstart-device-streams-echo-csharp.md) |
| Node.js | [Vínculo](quickstart-device-streams-echo-nodejs.md) | -                                                        |
| C      | -                                                        | [Vínculo](quickstart-device-streams-echo-c.md)      |

### <a name="local-proxy-sample-for-ssh-or-rdp"></a>Ejemplo Proxy local (para SSH o RDP)
En el ejemplo Proxy local se muestra una forma de habilitar la tunelización del tráfico de una aplicación existente que implica la comunicación entre un programa de cliente y un programa de servidor. Esta configuración funciona para los protocolos de cliente/servidor como SSH y RDP, donde el lado del servicio actúa como cliente (que ejecuta un programa de cliente SSH o RDP) y el lado del dispositivo actúa como el servidor (que ejecuta un programa de servidor RDP o demonio SSH). 

En esta sección, se describe el uso de los flujos de datos de dispositivos para permitir que un usuario pueda hacer SSH en un dispositivo utilizando los flujos de datos de los dispositivos (el caso de RDP u de otra aplicación cliente/servidor sería similar, pero se utilizaría el puerto correspondiente del protocolo).

El programa de instalación aprovecha los dos programas de *proxy local* que se muestran en la ilustración siguiente, es decir, *proxy local de dispositivo* y *proxy local de servicio*. Los programas del proxy local son responsables de ejecutar el [protocolo de enlace de iniciación del flujo de datos de los dispositivos](#device-stream-creation-flow) con IoT Hub y de interactuar con el cliente SSH y el demonio SSH utilizando sockets cliente/servidor normales.

![Texto alternativo](./media/iot-hub-device-streams-overview/iot-hub-device-streams-ssh.png "Configuración de los servidores proxy de flujos de dispositivos para SSH/RDP")

1. El usuario ejecuta el proxy local de servicio para iniciar un flujo de dispositivos al dispositivo.

2. El proxy local del dispositivo acepta la solicitud de iniciación del flujo y el túnel se establece en el punto de conexión de streaming de IoT Hub (tal y como se explicó anteriormente).

3. El proxy local de dispositivo se conecta al punto de conexión del demonio SSH que escucha en el puerto 22 del dispositivo.

4. El proxy local del servicio se mantiene a la escucha en un puerto designado a la espera de que lleguen nuevas conexiones SSH desde el usuario (el puerto 2222 del ejemplo es arbitrario y puede establecerse en cualquier otro puerto disponible). El usuario apunta el cliente SSH al puerto del proxy local de servicio en el host local.

### <a name="notes"></a>Notas
- Los pasos anteriores se completan en un túnel de un extremo a otro entre el cliente SSH (a la derecha) y el demonio SSH (a la izquierda). Parte de esta conectividad de un extremo a otro conlleva el envío de tráfico a IoT Hub mediante el flujo de datos de un dispositivo.

- Las flechas de la ilustración indican la dirección en la que se establecieron las conexiones entre los puntos de conexión. Tenga en cuenta específicamente que no hay conexiones entrantes que vayan al dispositivo (con frecuencia, las bloquea un firewall).

- El uso del puerto 2222 en el proxy local del servicio es una elección arbitraria. El proxy se puede configurar para que use cualquier otro puerto disponible.

- La elección del puerto 22 depende del protocolo y, en este caso, es específica para SSH. En el caso de RDP, debe usarse el puerto 3389. Se puede configurar en los programas de ejemplo que se proporcionaron.

Use los vínculos siguientes para ver información sobre cómo ejecutar los programas de proxy local en el lenguaje que prefiera. Al igual que en el [ejemplo de echo](#echo-sample), puede ejecutar los programas del proxy local del dispositivo y del servicio en distintos lenguajes, ya que son completamente interoperables.

| SDK    | Proxy local de servicio                                       | Proxy local de dispositivo                                |
|--------|-----------------------------------------------------------|---------------------------------------------------|
| C#     | [Vínculo](quickstart-device-streams-proxy-csharp.md)  | [Vínculo](quickstart-device-streams-proxy-csharp.md) |
| NodeJS | [Vínculo](quickstart-device-streams-proxy-nodejs.md)         | -                                                 |
| C      | -                                                         | [Vínculo](quickstart-device-streams-proxy-c.md)      |

## <a name="next-steps"></a>Pasos siguientes

Use los siguientes vínculos para más información sobre los flujos de dispositivo:

> [!div class="nextstepaction"]
> [Programa sobre los flujos de dispositivos en IoT (Channel 9)](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fchannel9.msdn.com%2FShows%2FInternet-of-Things-Show%2FAzure-IoT-Hub-Device-Streams&data=02%7C01%7Crezas%40microsoft.com%7Cc3486254a89a43edea7c08d67a88bcea%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636831125031268909&sdata=S6u9qiehBN4tmgII637uJeVubUll0IZ4p2ddtG5pDBc%3D&reserved=0)
> [Guía de inicio rápido sobre los flujos de dispositivos](/azure/iot-hub)
