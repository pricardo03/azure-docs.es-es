---
title: 'Autenticación de dispositivos de bajada: Azure IoT Edge | Microsoft Docs'
description: Procedimientos para autenticar dispositivos de bajada o de hoja en IoT Hub, y enrutar su conexión a través de dispositivos de puerta de enlace de Azure IoT Edge.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/07/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 7a032056a684107de3dd00fe4861f34c013a80db
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2019
ms.locfileid: "71003621"
---
# <a name="authenticate-a-downstream-device-to-azure-iot-hub"></a>Autenticación de un dispositivo de bajada en Azure IoT Hub

En un escenario de puerta de enlace transparente, los dispositivos de bajada (también denominados dispositivos de hoja o secundarios) necesitan identidades en IoT Hub como cualquier otro dispositivo. Este artículo le guía a través de las opciones para autenticar un dispositivo de bajada en IoT Hub y, después, se muestra cómo declarar la conexión de puerta de enlace.

Hay tres pasos generales para configurar una conexión de puerta de enlace transparente correcta. En este artículo se describe el segundo paso:

1. El dispositivo de puerta de enlace debe ser capaz de conectarse a dispositivos de bajada de forma segura, recibir comunicaciones de dispositivos de bajada y enrutar mensajes al destino adecuado. Para más información, vea [Configuración de un dispositivo IoT Edge para que actúe como puerta de enlace transparente](how-to-create-transparent-gateway.md).
2. **El dispositivo de bajada debe tener una identidad de dispositivo para poder autenticarse con IoT Hub y saber comunicarse a través de su dispositivo de puerta de enlace.**
3. El dispositivo de bajada se debe poder conectar de forma segura a su dispositivo de puerta de enlace. Para más información, consulte [Conexión de un dispositivo de bajada a una puerta de enlace Azure IoT Edge](how-to-connect-downstream-device.md).

Los dispositivos de bajada se pueden autenticar con IoT Hub mediante uno de estos tres métodos: claves simétricas (en ocasiones denominadas claves de acceso compartido), certificados X.509 autofirmados o certificados X.509 firmados por una entidad de certificación (CA). Los pasos de autenticación son similares a los que se usan para configurar cualquier dispositivo que no es IoT Edge con IoT Hub, con pequeñas diferencias para declarar la relación de la puerta de enlace.

Los pasos descritos en este artículo muestran el aprovisionamiento manual de dispositivos, no el automático con el servicio Azure IoT Hub Device Provisioning. 

## <a name="prerequisites"></a>Requisitos previos

Siga los pasos de [Configuración de un dispositivo IoT Edge para que actúe como puerta de enlace transparente](how-to-create-transparent-gateway.md).

Este artículo hace referencia al *nombre de host de la puerta de enlace* en varios puntos. Este nombre se declara en el parámetro **hostname** del archivo config.yaml del dispositivo de puerta de enlace IoT Edge. Se utiliza para crear los certificados de este artículo; se hace referencia a él en la cadena de conexión de los dispositivos de bajada. El nombre de host de la puerta de enlace debe poderse resolverse en una dirección IP, ya sea mediante DNS o una entrada del archivo de hosts.

## <a name="symmetric-key-authentication"></a>Autenticación de clave simétrica

La autenticación de clave simétrica (o de clave de acceso compartido) es la manera más sencilla de autenticarse con IoT Hub. Con la autenticación de clave simétrica, se asocia una clave de base64 con el identificador de dispositivo IoT en la instancia de IoT Hub. Esa clave se incluye en las aplicaciones de IoT para que el dispositivo pueda presentarla cuando se conecta a IoT Hub. 

### <a name="create-the-device-identity"></a>Creación de la identidad del dispositivo 

Agregue un nuevo dispositivo IoT en IoT Hub, mediante Azure Portal, la CLI de Azure o la extensión IoT para Visual Studio Code. Recuerde que los dispositivos de bajada se deben identificar en IoT Hub como dispositivos IoT normales, no como dispositivos IoT Edge. 

Al crear la identidad del dispositivo, proporcione la información siguiente: 

* Cree un identificador para el dispositivo.

* Seleccione **Clave simétrica** como el tipo de autenticación. 

* Opcionalmente, elija **Establecer un dispositivo primario** y seleccione el dispositivo de puerta de enlace IoT Edge a través del que se va a conectar este dispositivo de bajada. Este paso es opcional para la autenticación de clave simétrica, pero se recomienda porque la configuración de un dispositivo primario habilita [funciones sin conexión](offline-capabilities.md) para el dispositivo de bajada. Siempre puede actualizar los detalles del dispositivo para agregar o cambiar el primario más adelante. 

   ![Creación de la identidad del dispositivo con la autenticación de clave simétrica en el portal](./media/how-to-authenticate-downstream-device/symmetric-key-portal.png)

Puede usar la [extensión IoT para la CLI de Azure](https://github.com/Azure/azure-iot-cli-extension) con el fin de completar la misma operación. En el ejemplo siguiente se crea un dispositivo IoT con la autenticación de clave simétrica y se asigna un dispositivo primario: 

```cli
az iot hub device-identity create -n {iothub name} -d {device ID} --pd {gateway device ID}
```

Para obtener más información sobre los comandos de la CLI de Azure para la creación de dispositivos y la administración de elementos primarios y secundarios, vea el contenido de referencia para los comandos [az iot hub device-identity](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest).

### <a name="connect-to-iot-hub-through-a-gateway"></a>Conexión a IoT Hub a través de una puerta de enlace

El mismo proceso que se usa para autenticar dispositivos IoT convencionales en IoT Hub con claves simétricas también se aplica a los dispositivos de bajada. La única diferencia es que tendrá que agregar un puntero al dispositivo de puerta de enlace para enrutar la conexión o, en escenarios sin conexión, para controlar la autenticación en nombre de IoT Hub. 

Para la autenticación de clave simétrica, no es necesario realizar ningún paso adicional en el dispositivo para que se autentique con IoT Hub. Seguirá necesitando que los certificados estén presentes para que el dispositivo de bajada se pueda conectar a su dispositivo de puerta de enlace, como se describe en [Conexión de un dispositivo de bajada a una puerta de enlace Azure IoT Edge](how-to-connect-downstream-device.md).

Después de crear una identidad de dispositivo IoT en el portal, puede recuperar sus claves principales o secundarias. Una de estas claves se debe incluir en la cadena de conexión que se incluye en todas las aplicaciones que se comunican con IoT Hub. Para la autenticación de clave simétrica, IoT Hub proporciona la cadena de conexión completamente formada en los detalles del dispositivo para su comodidad. Tendrá que agregar información adicional sobre el dispositivo de puerta de enlace a la cadena de conexión. 

Las cadenas de conexión de clave simétrica para los dispositivos de bajada necesitan los componentes siguientes: 

* La instancia de IoT Hub a la que se conecta el dispositivo: `Hostname={iothub name}.azure-devices.net`
* El id. de dispositivo que se ha registrado con el centro: `DeviceID={device ID}`
* La clave principal o la secundaria: `SharedAccessKey={key}`
* El dispositivo de puerta de enlace a través del que se conecta el dispositivo. Proporcione el valor **hostname** (nombre de host) del archivo config.yaml del dispositivo de puerta de enlace de IoT Edge: `GatewayHostName={gateway hostname}`

Con todos los elementos, una cadena de conexión completa tiene este aspecto:

``` 
HostName=myiothub.azure-devices.net;DeviceId=myDownstreamDevice;SharedAccessKey=xxxyyyzzz;GatewayHostName=myGatewayDevice
```

Si ha establecido una relación de tipo primario-secundario para este dispositivo de bajada, puede simplificar la cadena de conexión mediante una llamada a la puerta de enlace directamente como el host de la conexión. Por ejemplo: 

```
HostName=myGatewayDevice;DeviceId=myDownstreamDevice;SharedAccessKey=xxxyyyzzz
```

## <a name="x509-authentication"></a>Autenticación X.509 

Hay dos maneras de autenticar un dispositivo IoT mediante certificados X.509. Con independencia de la forma de autenticación que elija, los pasos para conectar el dispositivo a IoT Hub son los mismos. Elija certificados autofirmados o firmados por una CA para la autenticación y luego continúe para obtener información sobre cómo conectarse a IoT Hub. 

Para más información sobre cómo se usa la autenticación de X.509 en IoT Hub, vea los artículos siguientes: 
* [Autenticación de dispositivos mediante certificados de entidades de certificación X.509](../iot-hub/iot-hub-x509ca-overview.md)
* [Explicación de los conceptos de certificados de entidad de certificación X.509 en el sector de IoT](../iot-hub/iot-hub-x509ca-concept.md)

### <a name="create-the-device-identity-with-x509-self-signed-certificates"></a>Creación de la identidad del dispositivo con certificados X.509 autofirmados

Para la autenticación de X.509 autofirmada, en ocasiones denominada autenticación de huella digital, tendrá que crear certificados para el dispositivo IoT. Estos certificados contienen una huella digital que se comparte con IoT Hub para la autenticación. 

La manera más fácil de probar este escenario es usar el mismo equipo que se ha utilizado para crear certificados en [Configuración de un dispositivo IoT Edge para que actúe como puerta de enlace transparente](how-to-create-transparent-gateway.md). Ese equipo ya debe estar configurado con la herramienta, el certificado de entidad de certificación raíz y el certificado de entidad de certificación intermedio adecuados para crear los certificados de dispositivo IoT. Puede copiar los certificados finales y sus claves privadas en el dispositivo de bajada después. Siguiendo los pasos del artículo de puerta de enlace, ha configurado openssl en el equipo y después ha clonado el repositorio de IoT Edge para acceder a los scripts de creación de certificado. Después, ha creado un directorio de trabajo denominado **\<WRKDIR>** para almacenar los certificados. Los certificados predeterminados están diseñados para desarrollo y pruebas, por lo que solo duran 30 días. Debe haber creado un certificado de entidad de certificación raíz y un certificado intermedio. 

1. Navegue hasta el directorio de trabajo en una ventana de PowerShell o bash. 

2. Cree dos certificados (principal y secundario) para el dispositivo de bajada. Proporcione el nombre del dispositivo y, después, la etiqueta principal o secundaria. Esta información se usa para denominar los archivos y poder realizar el seguimiento de los certificados para varios dispositivos. 

   ```PowerShell
   New-CACertsDevice "<device name>-primary"
   New-CACertsDevice "<device name>-secondary"
   ```

   ```bash
   ./certGen.sh create_device_certificate "<device name>-primary"
   ./certGen.sh create_device_certificate "<device name>-secondary"
   ```

3. Recupere la huella digital SHA1 (denominada huella digital en la interfaz de IoT Hub) de cada certificado, que es una cadena de 40 caracteres hexadecimales. Use el comando de openssl siguiente para ver el certificado y localizar la huella digital:

   ```PowerShell/bash
   openssl x509 -in <WORKDIR>/certs/iot-device-<device name>-primary.cert.pem -text -fingerprint | sed 's/[:]//g'
   ```

4. Vaya a la instancia de IoT Hub en Azure Portal y cree una identidad del dispositivo IoT con los valores siguientes: 

   * Seleccione **X.509 autofirmado** como el tipo de autenticación.
   * Pegue las cadenas hexadecimales que ha copiado de los certificados principal y secundario del dispositivo.
   * Seleccione **Establecer un dispositivo primario** y elija el dispositivo de puerta de enlace de IoT Edge a través del que se va a conectar este dispositivo de bajada. Para la autenticación X.509 de un dispositivo de bajada se requiere un dispositivo primario. 

   ![Creación de la identidad del dispositivo con la autenticación autofirmada de X.509 en el portal](./media/how-to-authenticate-downstream-device/x509-self-signed-portal.png)

5. Copie los archivos siguientes en cualquier directorio en el dispositivo de bajada:

   * `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device name>*.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device id>*.cert.pfx`
   * `<WRKDIR>\certs\iot-device-<device name>*-full-chain.cert.pem`
   * `<WRKDIR>\private\iot-device-<device name>*.key.pem`

   Tendrá que hacer referencia a estos archivos en las aplicaciones de dispositivo hoja que se conectan a IoT Hub. Puede usar un servicio como [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) o una función como [Protocolo de copia segura](https://www.ssh.com/ssh/scp/) para mover los archivos de certificado.

Puede usar la [extensión IoT para la CLI de Azure](https://github.com/Azure/azure-iot-cli-extension) para completar la misma operación de creación de dispositivos. En el ejemplo siguiente se crea un dispositivo IoT con autenticación X.509 autofirmado y se asigna un dispositivo primario: 

```cli
az iot hub device-identity create -n {iothub name} -d {device ID} --pd {gateway device ID} --am x509_thumbprint --ptp {primary thumbprint} --stp {secondary thumbprint}
```

Para obtener más información sobre los comandos de la CLI de Azure para la creación de dispositivos, la generación de certificados y la administración de elementos primarios y secundarios, vea el contenido de referencia para los comandos [az iot hub device-identity](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest).

### <a name="create-the-device-identity-with-x509-ca-signed-certificates"></a>Creación de la identidad del dispositivo con certificados X.509 firmados por CA

Para la autenticación de certificados X.509 firmados por entidad de certificación (CA), necesita un certificado de entidad de certificación raíz registrado en IoT Hub y usarlo para firmar los certificados para el dispositivo IoT. Cualquier dispositivo con un certificado emitido por el certificado de entidad de certificación raíz o cualquiera de sus certificados intermedios tendrá permiso para autenticarse. 

Esta sección se basa en las instrucciones detalladas en el artículo de IoT Hub [Configuración de la seguridad de X.509 en Azure IoT Hub](../iot-hub/iot-hub-security-x509-get-started.md). Siga los pasos descritos en esta sección para saber qué valores se usan para configurar un dispositivo de bajada que se conecta a través de una puerta de enlace. 

La manera más fácil de probar este escenario es usar el mismo equipo que se ha utilizado para crear certificados en [Configuración de un dispositivo IoT Edge para que actúe como puerta de enlace transparente](how-to-create-transparent-gateway.md). Ese equipo ya debe estar configurado con la herramienta, el certificado de entidad de certificación raíz y el certificado de entidad de certificación intermedio adecuados para crear los certificados de dispositivo IoT. Puede copiar los certificados finales y sus claves privadas en el dispositivo de bajada después. Siguiendo los pasos del artículo de puerta de enlace, ha configurado openssl en el equipo y después ha clonado el repositorio de IoT Edge para acceder a los scripts de creación de certificado. Después, ha creado un directorio de trabajo denominado **\<WRKDIR>** para almacenar los certificados. Los certificados predeterminados están diseñados para desarrollo y pruebas, por lo que solo duran 30 días. Debe haber creado un certificado de entidad de certificación raíz y un certificado intermedio. 

1. Siga las instrucciones de la sección [Registro de certificados de entidad de certificación X.509 en una instancia de IoT Hub](../iot-hub/iot-hub-security-x509-get-started.md#register-x509-ca-certificates-to-your-iot-hub) de *Configuración de la seguridad de X.509 en Azure IoT Hub*. En esa sección, realice los pasos siguientes: 

   1. Cargue un certificado de entidad de certificación raíz. Si va a usar los certificados que ha creado en el artículo de puerta de enlace transparente, cargue **\<WRKDIR>/certs/azure-iot-test-only.root.ca.cert.pem** como el archivo de certificado raíz. 
   2. Compruebe que es el propietario de ese certificado de entidad de certificación raíz. Puede comprobarlo con las herramientas de cert en \<WRKDIR>. 

      ```powershell
      New-CACertsVerificationCert "<verification code from Azure portal>"
      ```

      ```bash
      ./certGen.sh create_verification_certificate <verification code from Azure portal>"
      ```

2. Siga las instrucciones de la sección [Creación de un dispositivo X.509 para una instancia de IoT Hub](../iot-hub/iot-hub-security-x509-get-started.md#create-an-x509-device-for-your-iot-hub) de *Configuración de la seguridad de X.509 en Azure IoT Hub*. En esa sección, realice los pasos siguientes: 

   1. Agregue un nuevo dispositivo. Proporcione un nombre en minúsculas para **Id. de dispositivo** y elija el tipo de autenticación **X.509 firmado por CA**. 
   2. Configure un dispositivo primario. Para los dispositivos de bajada, seleccione **Establecer un dispositivo primario** y elija el dispositivo de puerta de enlace de IoT Edge que proporcionará la conexión a IoT Hub. 

3. Cree una cadena de certificados para el dispositivo de bajada. Use el mismo certificado de entidad de certificación raíz que ha cargado en IoT Hub para crear esta cadena. Use el mismo identificador de dispositivo en minúsculas que ha asignado a la identidad del dispositivo en el portal.

   ```powershell
   New-CACertsDevice "<device id>"
   ```

   ```bash
   ./certGen.sh create_device_certificate "<device id>"
   ```

4. Copie los archivos siguientes en cualquier directorio en el dispositivo de bajada: 

   * `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device id>*.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device id>*.cert.pfx`
   * `<WRKDIR>\certs\iot-device-<device id>*-full-chain.cert.pem`
   * `<WRKDIR>\private\iot-device-<device id>*.key.pem`

   Tendrá que hacer referencia a estos archivos en las aplicaciones de dispositivo hoja que se conectan a IoT Hub. Puede usar un servicio como [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) o una función como [Protocolo de copia segura](https://www.ssh.com/ssh/scp/) para mover los archivos de certificado.

Puede usar la [extensión IoT para la CLI de Azure](https://github.com/Azure/azure-iot-cli-extension) para completar la misma operación de creación de dispositivos. En el ejemplo siguiente se crea un dispositivo IoT con autenticación X.509 firmado por CA y se asigna un dispositivo primario: 

```cli
az iot hub device-identity create -n {iothub name} -d {device ID} --pd {gateway device ID} --am x509_ca
```

Para obtener más información sobre los comandos de la CLI de Azure para la creación de dispositivos y la administración de elementos primarios y secundarios, vea el contenido de referencia para los comandos [az iot hub device-identity](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest).


### <a name="connect-to-iot-hub-through-a-gateway"></a>Conexión a IoT Hub a través de una puerta de enlace

Cada SDK de Azure IoT controla la autenticación X.509 de forma diferente. Pero el mismo proceso que se usa para autenticar dispositivos IoT convencionales en IoT Hub con certificados X.509 también se aplica a los dispositivos de bajada. La única diferencia es que tendrá que agregar un puntero al dispositivo de puerta de enlace para enrutar la conexión o, en escenarios sin conexión, para controlar la autenticación en nombre de IoT Hub. En general, puede seguir los mismos pasos de autenticación de X.509 para todos los dispositivos IoT Hub y luego reemplazar simplemente el valor de **Hostname** en la cadena de conexión con el nombre de host del dispositivo de puerta de enlace. 

En las secciones siguientes se muestran algunos ejemplos para distintos lenguajes de SDK. 

>[!IMPORTANT]
>En los ejemplos siguientes se muestra cómo los SDK de IoT Hub usan los certificados para la autenticación de dispositivos. En una implementación de producción, debe almacenar todos los secretos, como las claves privadas o SAS, en un módulo de seguridad de hardware (HSM). 

#### <a name="net"></a>.NET

Para obtener un ejemplo de autenticación de un programa de C# en IoT Hub con certificados X.509, vea [Configuración de la seguridad de X.509 en Azure IoT Hub](../iot-hub/iot-hub-security-x509-get-started.md#authenticate-your-x509-device-with-the-x509-certificates). Algunas de las líneas clave de ese ejemplo se incluyen aquí para demostrar el proceso de autenticación.

Al declarar el nombre de host para la instancia de DeviceClient, use el nombre de host del dispositivo de puerta de enlace de IoT Edge. El nombre de host se puede encontrar en el archivo config.yaml del dispositivo de puerta de enlace. 

Si va a usar los certificados de prueba proporcionados por el repositorio de Git de IoT Edge, la clave para los certificados es **1234**.

```csharp
try
{
    var cert = new X509Certificate2(@"<absolute-path-to-your-device-pfx-file>", "1234");
    var auth = new DeviceAuthenticationWithX509Certificate("<device-id>", cert);
    var deviceClient = DeviceClient.Create("<gateway hostname>", auth, TransportType.Amqp_Tcp_Only);

    if (deviceClient == null)
    {
        Console.WriteLine("Failed to create DeviceClient!");
    }
    else
    {
        Console.WriteLine("Successfully created DeviceClient!");
        SendEvent(deviceClient).Wait();
    }

    Console.WriteLine("Exiting...\n");
}
catch (Exception ex)
{
    Console.WriteLine("Error in sample: {0}", ex.Message);
}
```

#### <a name="c"></a>C

Para obtener un ejemplo de la autenticación de un programa de C en IoT Hub con certificados X.509, vea el ejemplo [iotedge_downstream_device_sample](https://github.com/Azure/azure-iot-sdk-c/tree/x509_edge_bugbash/iothub_client/samples/iotedge_downstream_device_sample) del SDK de IoT de C. Algunas de las líneas clave de ese ejemplo se incluyen aquí para demostrar el proceso de autenticación.

Al definir la cadena de conexión para el dispositivo de bajada, use el nombre de host del dispositivo de puerta de enlace de IoT Edge para el parámetro **HostName**. El nombre de host se puede encontrar en el archivo config.yaml del dispositivo de puerta de enlace. 

```C
// If your downstream device uses X.509 authentication (self signed or X.509 CA) then
// resulting connection string should look like the following:
// "HostName=<gateway device hostname>;DeviceId=<device_id>;x509=true"
static const char* connectionString = "[Downstream device IoT Edge connection string]";

// Path to the Edge "owner" root CA certificate
static const char* edge_ca_cert_path = "[Path to root CA certificate]";

// When the downstream device uses X.509 authentication, a certificate and key 
// in PRM format must be provided.
static const char * x509_device_cert_path = "[Path to primary or secondary device cert]";
static const char * x509_device_key_path = "[Path to primary or secondary device key]";

int main(void)
{
    // Create the iothub handle here
    device_handle = IoTHubDeviceClient_CreateFromConnectionString(connectionString, protocol);

    // Provide the Azure IoT device client with the same root
    // X509 CA certificate that was used to set up the IoT Edge gateway runtime
    if (edge_ca_cert_path != NULL)
    {
        cert_string = obtain_edge_ca_certificate();
        (void)IoTHubDeviceClient_SetOption(device_handle, OPTION_TRUSTED_CERT, cert_string);
    }

    if ((x509_device_cert_path != NULL) && (x509_device_key_path != NULL))
    {
        const char *x509certificate = obtain_file_contents(x509_device_cert_path);
        const char *x509privatekey = obtain_file_contents(x509_device_key_path);
        if ((IoTHubDeviceClient_SetOption(device_handle, OPTION_X509_CERT, x509certificate) != IOTHUB_CLIENT_OK) ||
            (IoTHubDeviceClient_SetOption(device_handle, OPTION_X509_PRIVATE_KEY, x509privatekey) != IOTHUB_CLIENT_OK)
            )
        {
            printf("failure to set options for x509, aborting\r\n");
            exit(1);
        }
    }
}
```

#### <a name="nodejs"></a>Node.js

Para obtener un ejemplo de la autenticación de un programa de Node.js en IoT Hub con certificados X.509, vea el ejemplo [simple_sample_device_x509.js](https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/simple_sample_device_x509.js) del SDK de IoT de Node.js. Algunas de las líneas clave de ese ejemplo se incluyen aquí para demostrar el proceso de autenticación.

Al definir la cadena de conexión para el dispositivo de bajada, use el nombre de host del dispositivo de puerta de enlace de IoT Edge para el parámetro **HostName**. El nombre de host se puede encontrar en el archivo config.yaml del dispositivo de puerta de enlace. 

Si va a usar los certificados de prueba proporcionados por el repositorio de Git de IoT Edge, la clave para los certificados es **1234**.

```node
// String containing Hostname and Device Id in the following format:
//  "HostName=<gateway device hostname>;DeviceId=<device_id>;x509=true"
var connectionString = '<DEVICE CONNECTION STRING WITH x509=true>';
var certFile = '<PATH-TO-CERTIFICATE-FILE>';
var keyFile = '<PATH-TO-KEY-FILE>';
var passphrase = '<KEY PASSPHRASE IF ANY>';

// fromConnectionString must specify a transport constructor, coming from any transport package.
var client = Client.fromConnectionString(connectionString, Protocol);

var options = {
   cert : fs.readFileSync(certFile, 'utf-8').toString(),
   key : fs.readFileSync(keyFile, 'utf-8').toString(),
   passphrase: passphrase
 };

// Calling setOptions with the x509 certificate and key (and optionally, passphrase) will configure the client transport to use x509 when connecting to IoT Hub
client.setOptions(options);
```

#### <a name="python"></a>Python

Actualmente, el SDK de Python solo admite certificados X509 y claves de archivos, no los que se definen en línea. En el siguiente ejemplo las rutas de archivo pertinentes se almacenan en variables de entorno.

Al definir el nombre de host para el dispositivo de bajada, use el del dispositivo de puerta de enlace de IoT Edge para el parámetro **HostName**. El nombre de host se puede encontrar en el archivo config.yaml del dispositivo de puerta de enlace. 

```python
import os
from azure.iot.device import IoTHubDeviceClient, X509

HOSTNAME = "[IoT Edge Gateway Hostname]"
DEVICE_ID = "[Device ID]"

def iothub_client_init():
    x509 = X509(
        cert_file=os.getenv("X509_CERT_FILE"),
        key_file=os.getenv("X509_KEY_FILE")
    )

    client = IoTHubDeviceClient.create_from_x509_certificate(
        x509=x509,
        hostname=HOSTNAME,
        device_id=DEVICE_ID
    )
)

if __name__ == '__main__':
    iothub_client_init()
```

#### <a name="java"></a>Java

Para obtener un ejemplo de la autenticación de un programa de Java en IoT Hub con certificados X.509, vea el ejemplo [SendEventX509.java](https://github.com/Azure/azure-iot-sdk-python/blob/master/device/samples/iothub_client_sample_x509.py) del SDK de IoT de Java. Algunas de las líneas clave de ese ejemplo se incluyen aquí para demostrar el proceso de autenticación.

Al definir la cadena de conexión para el dispositivo de bajada, use el nombre de host del dispositivo de puerta de enlace de IoT Edge para el parámetro **HostName**. El nombre de host se puede encontrar en el archivo config.yaml del dispositivo de puerta de enlace. 

```java
//PEM encoded representation of the public key certificate
private static String publicKeyCertificateString =
    "-----BEGIN CERTIFICATE-----\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "-----END CERTIFICATE-----\n";

//PEM encoded representation of the private key
private static String privateKeyString =
    "-----BEGIN EC PRIVATE KEY-----\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "-----END EC PRIVATE KEY-----\n";

DeviceClient client = new DeviceClient(connectionString, protocol, publicKeyCertificateString, false, privateKeyString, false);
```

## <a name="next-steps"></a>Pasos siguientes

Al completar este artículo, debería tener un dispositivo IoT Edge que funciona como una puerta de enlace transparente y un dispositivo de bajada registrado con una instancia de IoT Hub. A continuación, tendrá que configurar los dispositivos de bajada para que confíen en el dispositivo de puerta de enlace y le envíen mensajes. Para más información, consulte [Conexión de un dispositivo de bajada a una puerta de enlace Azure IoT Edge](how-to-connect-downstream-device.md).
