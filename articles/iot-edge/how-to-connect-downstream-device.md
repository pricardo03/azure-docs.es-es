---
title: 'Conexión de dispositivos de bajada: Azure IoT Edge | Microsoft Docs'
description: Cómo configurar dispositivos de bajada o dispositivos hoja para su conexión mediante dispositivos de puerta de enlace Azure IoT Edge.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/01/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: d41ec0bc959eb264564d49ae6ac31aa30b3be98a
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55492766"
---
# <a name="connect-a-downstream-device-to-an-azure-iot-edge-gateway"></a>Conexión de un dispositivo de bajada a una puerta de enlace Azure IoT Edge

Azure IoT Edge permite escenarios de puerta de enlace transparente, en los que uno o varios dispositivos pueden pasar sus mensajes a través de un dispositivo de puerta de enlace único que mantiene la conexión con IoT Hub. Una vez que el dispositivo de puerta de enlace está configurado, deberá saber cómo conectar de forma segura los dispositivos de bajada. 

Este artículo identifica problemas comunes con las conexiones de los dispositivos de bajada y le guía en la configuración de los dispositivos de bajada, lo que incluye: 

* Descripción de los aspectos básicos de la seguridad de la capa de transporte (TLS) y los certificados. 
* Descripción del funcionamiento de las bibliotecas de TLS en distintos sistemas operativos y del tratamiento de los certificados en cada sistema operativo.
* Ejemplos de Azure IoT en varios lenguajes que le ayudarán a comenzar. 

En este artículo, los términos *puerta de enlace* y *puerta de enlace IoT Edge* hacen referencia a un dispositivo IoT Edge configurado como una puerta de enlace transparente. 

## <a name="prerequisites"></a>Requisitos previos

Antes de seguir los pasos descritos en este artículo, debe tener dos dispositivos listos para su uso:

1. Un dispositivo IoT Edge configurado como puerta de enlace transparente. 
    [Configuración de un dispositivo IoT Edge para que actúe como puerta de enlace transparente](how-to-create-transparent-gateway.md)

    Una vez que el dispositivo de puerta de enlace está configurado, copie el certificado **azure-iot-test-only.root.ca.cert.pem** de la puerta de enlace y asegúrese de que está disponible en algún lugar en el dispositivo de bajada. 

2. Un dispositivo de bajada que tenga una identidad de dispositivo de IoT Hub. 
    No se puede usar un dispositivo IoT Edge como el dispositivo de bajada. En su lugar, use un dispositivo registrado como un dispositivo IoT normal en IoT Hub. En el portal, puede registrar un nuevo dispositivo en la sección **Dispositivos IoT**. O bien puede usar la CLI de Azure para [registrar un dispositivo](../iot-hub/quickstart-send-telemetry-c.md#register-a-device). Copie la cadena de conexión para su uso en secciones posteriores. 

    Actualmente, solo pueden conectarse mediante puertas de enlace IoT Edge los dispositivos de bajada con autenticación de clave simétrica. Las entidades de certificación X.509 y los certificados autofirmados X.509 no se admiten actualmente. 

## <a name="prepare-a-downstream-device"></a>Preparación de un dispositivo de bajada

Un dispositivo de bajada puede ser cualquier aplicación o plataforma que tenga una identidad creada con el servicio en la nube [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub). En muchos casos, estas aplicaciones utilizan el [SDK de dispositivo IoT de Azure](../iot-hub/iot-hub-devguide-sdks.md). En la práctica, un dispositivo de bajada podría ser incluso una aplicación que se ejecuta en el propio dispositivo de puerta de enlace IoT Edge. 

Para conectar un dispositivo de bajada a una puerta de enlace IoT Edge, necesita dos cosas:

1. Un dispositivo o aplicación que esté configurada con una cadena de conexión de dispositivo de IoT Hub anexada con información para conectarse a la puerta de enlace. 

    La cadena de conexión tiene un formato similar a: `HostName=yourHub.azure-devices.net;DeviceId=yourDevice;SharedAccessKey=XXXYYYZZZ=;`. Anexe la propiedad **GatewayHostName** con el nombre de host del dispositivo de puerta de enlace al final de la cadena de conexión. El valor de **GatewayHostName** debe coincidir con el valor de **hostname** en el archivo config.yaml del dispositivo de la puerta de enlace. 

    La cadena final es similar a: `HostName=yourHub.azure-devices.net;DeviceId=yourDevice;SharedAccessKey=XXXYYYZZZ=;GatewayHostName=mygateway.contoso.com`.

2. El dispositivo o la aplicación tienen que confiar en el certificado de la **entidad de certificación raíz** o la **entidad de certificación del propietario** de la puerta de enlace para validar las conexiones TLS con el dispositivo de puerta de enlace. 

    Este paso más complicado se explica con detalle en el resto de este artículo. Este paso se puede ser realizar de dos maneras: mediante la instalación del certificado de entidad de certificación en el almacén de certificados del sistema operativo o (para determinados lenguajes) haciendo referencia al certificado dentro de las aplicaciones mediante los SDK de Azure IoT.

## <a name="tls-and-certificate-fundamentals"></a>Aspectos básicos de TLS y los certificados

El desafío de una conexión segura de los dispositivos de bajada a IoT Edge es igual que en cualquier otra comunicación cliente/servidor segura que se produce en Internet. Un cliente y un servidor se comunican de forma segura en Internet mediante la [Seguridad de la capa de transporte (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security). TLS se ha creado mediante construcciones de [Infraestructura de clave pública (PKI)](https://en.wikipedia.org/wiki/Public_key_infrastructure) estándar llamadas certificados. TLS es una especificación bastante compleja y aborda una amplia gama de temas relacionados con la protección de dos puntos de conexión, pero la siguiente sección describe de forma concisa lo que se necesita para conectar dispositivos de forma segura a una puerta de enlace IoT Edge.

Cuando un cliente se conecta a un servidor, el servidor presenta una cadena de certificados llamada *cadena de certificados de servidor*. Una cadena de certificados normalmente consta de un certificado de entidad de certificación (CA) raíz, uno o varios certificados de entidad de certificación intermedios y, finalmente, el propio certificado del servidor. Un cliente establece la confianza con un servidor mediante la comprobación criptográfica de la cadena de certificados de servidor completa. Esta validación del cliente de la cadena de certificados de servidor se llama *autenticación de servidor*. Para validar una cadena de certificados de servidor, un cliente necesita una copia del certificado de la entidad de certificación raíz que se usó para crear (o emitir) el certificado del servidor. Normalmente, al conectarse a sitios web, un explorador viene preconfigurado con certificados de entidad de certificación usados habitualmente para que el cliente tenga un proceso de conexión directo. 

Cuando un dispositivo se conecta a Azure IoT Hub, el dispositivo es el cliente y el servicio en la nube de IoT Hub es el servidor. El servicio en la nube de IoT Hub está respaldado por un certificado de entidad de certificación raíz llamado **Baltimore CyberTrust Root**, que es ampliamente utilizado y está disponible públicamente. Puesto que el certificado de entidad de certificación de IoT Hub ya está instalado en la mayoría de los dispositivos, muchas implementaciones de TLS (OpenSSL, Schannel, LibreSSL) lo utilizan automáticamente durante la validación de certificados de servidor. Un dispositivo que puede conectarse correctamente a IoT Hub puede tener problemas al intentar conectarse a una puerta de enlace IoT Edge.

Cuando un dispositivo se conecta a una puerta de enlace IoT Edge, el dispositivo de bajada es el cliente y el dispositivo de puerta de enlace es el servidor. Azure IoT Edge permite a los operadores (o usuarios) crear cadenas de certificados de puerta de enlace como consideren oportuno. El operador puede usar un certificado de entidad de certificación pública, como Baltimore o usar un certificado de entidad de certificación de raíz autofirmado (o interno). Los certificados de entidad de certificación pública a menudo tienen un costo asociado, por lo que se utilizan normalmente en escenarios de producción. Los certificados de entidad de certificación autofirmados se prefieren para desarrollo y pruebas. Los artículos de configuración de la puerta de enlace transparente enumerados en la sección de requisitos previos usan certificados de entidad de certificación raíz autofirmados. 

Cuando se usa un certificado de entidad de certificación raíz autofirmado para una puerta de enlace IoT Edge, este se debe instalar o proporcionar a todos los dispositivos de bajada que intentan conectarse a la puerta de enlace. 

Para más información acerca de los certificados de IoT Edge y algunas implicaciones de producción, consulte [Detalles de uso de los certificados de IoT Edge](iot-edge-certs.md).

## <a name="install-certificates-using-the-os"></a>Instalación de certificados mediante el sistema operativo

En este artículo se usa *entidad de certificación del propietario* para hacer referencia al certificado de entidad de certificación raíz, ya que es el término utilizado por los scripts en el artículo de requisitos previos de la puerta de enlace. 

La instalación del certificado de entidad de certificación del propietario en el almacén de certificados del sistema operativo generalmente permite que la mayoría de las aplicaciones lo usen. Hay algunas excepciones, como las aplicaciones de NodeJS, que no utilizan el almacén de certificados del sistema operativo y utilizan en su lugar el almacén de certificados interno del entorno de ejecución de Node. Si no se puede instalar el certificado en el nivel de sistema operativo, consulte los ejemplos específicos de cada lenguaje más adelante en este artículo para usar el certificado con los SDK de Azure IoT en las aplicaciones. 

### <a name="ubuntu"></a>Ubuntu

Los siguientes comandos son un ejemplo de cómo instalar un certificado de entidad de certificación en un host con Ubuntu. En este ejemplo se da por supuesto que usa el certificado **azure-iot-test-only.root.ca.cert.pem** de los artículos de requisitos previos y que ha copiado el certificado en una ubicación en el dispositivo de bajada.  

```bash
sudo cp <path>/azure-iot-test-only.root.ca.cert.pem /usr/local/share/ca-certificates/azure-iot-test-only.root.ca.cert.pem.crt
sudo update-ca-certificates
```

Verá un mensaje que dice "Updating certificates in /etc/ssl/certs... 1 added, 0 removed; done." (Actualizando certificados en /etc/ssl/certs...1 agregado, 0 quitados, listo)

### <a name="windows"></a> Windows

Los siguientes pasos son un ejemplo de cómo instalar un certificado de entidad de certificación en un host con Windows. En este ejemplo se da por supuesto que usa el certificado **azure-iot-test-only.root.ca.cert.pem** de los artículos de requisitos previos y que ha copiado el certificado en una ubicación en el dispositivo de bajada.  

1. En el menú Inicio, busque y seleccione **Administrar certificados de equipo**. Se abre una utilidad llamada **certlm**.
2. Vaya a **Certificados: equipo local** > **Entidades de certificación raíz de confianza**.
3. Haga clic con el botón derecho en **Certificados** y seleccione **Todas las tareas** > **Importar**. Se debe iniciar el asistente para importación de certificados. 
4. Siga los pasos tal como se indica e importe el archivo del certificado `<path>/azure-iot-test-only.root.ca.cert.pem`. Cuando se haya completado, verá el mensaje "Importación correcta". 

También puede instalar certificados mediante programación con las API de .NET, como se muestra más adelante en el ejemplo de .NET de este artículo. 

Normalmente, las aplicaciones utilizan la pila TLS proporcionada por Windows llamada [Schannel](https://docs.microsoft.com/windows/desktop/com/schannel) para conectarse de forma segura mediante TLS. Schannel *requiere* que todos los certificados se instalen en el almacén de certificados de Windows antes de intentar establecer una conexión TLS.

## <a name="use-certificates-with-azure-iot-sdks"></a>Uso de certificados con los SDK de Azure IoT

En este artículo hace referencia al certificado de entidad de certificación raíz como la *entidad de certificación del propietario*, ya que es el término utilizado por los scripts que generan el certificado autofirmado de los artículos de requisitos previos. 

Esta sección describe cómo conectar los SDK de Azure IoT a un dispositivo IoT Edge con aplicaciones de ejemplo simples. El objetivo de todos los ejemplos es conectar el cliente de dispositivo y enviar mensajes de telemetría a la puerta de enlace, cerrar la conexión y salir. 

### <a name="common-concepts-across-all-azure-iot-sdks"></a>Conceptos comunes en todos los SDK de Azure IoT

Debe tener dos cosas listas antes de usar los ejemplos de nivel de aplicación:

1. La cadena de conexión de IoT Hub del dispositivo de bajada modificada para que apunte al dispositivo de puerta de enlace.

    La cadena de conexión tiene un formato similar a: `HostName=yourHub.azure-devices.net;DeviceId=yourDevice;SharedAccessKey=XXXYYYZZZ=;`. Anexe la propiedad **GatewayHostName** con el nombre de host del dispositivo de puerta de enlace al final de la cadena de conexión. El valor de **GatewayHostName** debe coincidir con el valor de **hostname** en el archivo config.yaml del dispositivo de la puerta de enlace. 

    La cadena final es similar a: `HostName=yourHub.azure-devices.net;DeviceId=yourDevice;SharedAccessKey=XXXYYYZZZ=;GatewayHostName=mygateway.contoso.com`.

2. La ruta de acceso completa del certificado de entidad de certificación raíz que ha copiado y guardado en algún lugar del dispositivo de bajada.

    Por ejemplo, `<path>/azure-iot-test-only.root.ca.cert.pem`. 

### <a name="nodejs"></a>NodeJS

Esta sección proporciona una aplicación de ejemplo para conectar un cliente de dispositivo de NodeJS de Azure IoT a una puerta de enlace IoT Edge. Para los hosts con Windows y Linux, debe instalar el certificado de entidad de certificación raíz en el nivel de aplicación como se muestra aquí, ya que las aplicaciones de NodeJS no utilizan el almacén de certificados del sistema. 

1. Puede obtener el ejemplo para **edge_downstream_device.js** desde el [repositorio de ejemplos del SDK de dispositivo IoT de Azure para Node.js](https://github.com/Azure/azure-iot-sdk-node/tree/master/device/samples). 
2. Asegúrese de que tiene todos los requisitos previos para ejecutar el ejemplo; para ello, revise el archivo **readme.md**. 
3. En el archivo edge_downstream_device.js, actualice las variables **connectionString** y **edge_ca_cert_path**. 
4. Consulte la documentación del SDK para obtener instrucciones sobre cómo ejecutar el ejemplo en el dispositivo. 

Para entender el ejemplo que se está ejecutando, el siguiente fragmento de código contiene el modo en el que el SDK de cliente lee el archivo de certificado y lo usa para establecer una conexión TLS segura: 

```javascript
// Provide the Azure IoT device client via setOptions with the X509
// Edge root CA certificate that was used to setup the Edge runtime
var options = {
    ca : fs.readFileSync(edge_ca_cert_path, 'utf-8'),
};
```

### <a name="net"></a>.NET

Esta sección presenta una aplicación de ejemplo para conectar un cliente de dispositivo de .NET de Azure IoT a una puerta de enlace IoT Edge. Sin embargo, las aplicaciones .NET pueden usar automáticamente los certificados instalados en el almacén de certificados del sistema en los hosts con Linux y Windows.

1. Puede obtener el ejemplo para **EdgeDownstreamDevice** desde la [carpeta de ejemplos de .NET de IoT Edge ](https://github.com/Azure/iotedge/tree/master/samples/dotnet/EdgeDownstreamDevice). 
2. Asegúrese de que tiene todos los requisitos previos para ejecutar el ejemplo; para ello, revise el archivo **readme.md**. 
3. En el archivo **Propiedades / launchSettings.json**, actualice las variables **DEVICE_CONNECTION_STRING** y **CA_CERTIFICATE_PATH**. Si desea utilizar el certificado instalado en el almacén de certificados de confianza en el sistema host, deje en blanco esta variable. 
4. Consulte la documentación del SDK para obtener instrucciones sobre cómo ejecutar el ejemplo en el dispositivo. 

Para instalar mediante programación un certificado de confianza en el almacén de certificados mediante una aplicación de .NET, consulte la función **InstallCACert()** del archivo **EdgeDownstreamDevice / Program.cs**. Esta operación es idempotente, por lo que se puede ejecutar varias veces con los mismos valores sin ningún efecto adicional. 

### <a name="c"></a>C

Esta sección presenta una aplicación de ejemplo para conectar un cliente de dispositivo de Azure IoT para C a una puerta de enlace IoT Edge. El SDK para C puede funcionar con muchas bibliotecas TLS, incluidas OpenSSL, WolfSSL y Schannel. Para más información, consulte el [SDK de Azure IoT para C](https://github.com/Azure/azure-iot-sdk-c). 

1. Puede obtener la aplicación **iotedge_downstream_device_sample** en los [Ejemplos del SDK de dispositivo IoT de Azure para C](https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples). 
2. Asegúrese de que tiene todos los requisitos previos para ejecutar el ejemplo; para ello, revise el archivo **readme.md**. 
3. En el archivo iotedge_downstream_device_sample.js, actualice las variables **connectionString** y **edge_ca_cert_path**. 
4. Consulte la documentación del SDK para obtener instrucciones sobre cómo ejecutar el ejemplo en el dispositivo. 

El SDK de dispositivo IoT de Azure para C proporciona una opción para registrar un certificado de entidad de certificación al configurar el cliente. Esta operación no instala el certificado en ningún lugar y en su lugar usa un formato de cadena del certificado en memoria. Al establecer una conexión, se proporciona el certificado guardado a la pila TLS subyacente. 

```C
(void)IoTHubDeviceClient_SetOption(device_handle, OPTION_TRUSTED_CERT, cert_string);
```

En hosts con Windows, si no utiliza OpenSSL ni otra biblioteca de TLS, el SDK utiliza de manera predeterminada Schannel. Para que Schannel funcione, se debe instalar el certificado de entidad de certificación raíz de IoT Edge en el almacén de certificados de Windows, no se debe establecer mediante la operación `IoTHubDeviceClient_SetOption`. 

### <a name="java"></a>Java

Esta sección presenta una aplicación de ejemplo para conectar un cliente de dispositivo de Java de Azure IoT a una puerta de enlace IoT Edge. 

1. Puede obtener el ejemplo **Send-event** en los [Ejemplos del SDK de dispositivo IoT de Azure para Java](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples). 
2. Asegúrese de que tiene todos los requisitos previos para ejecutar el ejemplo; para ello, revise el archivo **readme.md**. 
3. Consulte la documentación del SDK para obtener instrucciones sobre cómo ejecutar el ejemplo en el dispositivo.

### <a name="python"></a>Python

Esta sección presenta una aplicación de ejemplo para conectar un cliente de dispositivo de Python de Azure IoT a una puerta de enlace IoT Edge. 

1. Puede obtener el ejemplo **edge_downstream_client** en los [Ejemplos del SDK de dispositivo IoT de Azure para Python](https://github.com/Azure/azure-iot-sdk-python/tree/master/device/samples). 
2. Asegúrese de que tiene todos los requisitos previos para ejecutar el ejemplo; para ello, revise el archivo **readme.md**. 
3. En el archivo edge_downstream_client.py, actualice las variables **CONNECTION_STRING** y **TRUSTED_ROOT_CA_CERTIFICATE_PATH**. 
4. Consulte la documentación del SDK para obtener instrucciones sobre cómo ejecutar el ejemplo en el dispositivo. 


## <a name="test-the-gateway-connection"></a>Prueba de la conexión de puerta de enlace

Este es un comando de ejemplo que comprueba que todo se ha configurado correctamente. Verá un mensaje que indica que la verificación es correcta.

```cmd/sh
openssl s_client -connect mygateway.contoso.com:8883 -CAfile <CERTDIR>/certs/azure-iot-test-only.root.ca.cert.pem -showcerts
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo puede extender IoT Edge las [funcionalidades sin conexión](offline-capabilities.md) en los dispositivos de bajada. 