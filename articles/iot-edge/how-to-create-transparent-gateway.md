---
title: Creación de un dispositivo de puerta de enlace transparente con Azure IoT Edge | Microsoft Docs
description: Uso de un dispositivo Azure IoT Edge como una puerta de enlace transparente que puede procesar información para dispositivos de bajada
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 08/17/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: d0ac7fa3a1dbb1c91da5b9919bc2c62de74213b5
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2019
ms.locfileid: "74456787"
---
# <a name="configure-an-iot-edge-device-to-act-as-a-transparent-gateway"></a>Configuración de un dispositivo IoT Edge para que actúe como puerta de enlace transparente

Este artículo proporciona instrucciones detalladas para configurar un dispositivo IoT Edge de modo que funcione como una puerta de enlace transparente para que otros dispositivos se comuniquen con IoT Hub. En este artículo, el término *puerta de enlace IoT Edge* hace referencia a un dispositivo IoT Edge que se usa como una puerta de enlace transparente. Para más información, consulte [Uso de un dispositivo IoT Edge como puerta de enlace](./iot-edge-as-gateway.md).

>[!NOTE]
>Actualmente:
> * Los dispositivos habilitados para Edge no pueden conectarse a puertas de enlace de IoT Edge. 
> * Los dispositivos de bajada no pueden usar la carga de archivos.

Hay tres pasos generales para configurar una conexión de puerta de enlace transparente correcta. En este artículo se describe el primer paso:

1. **El dispositivo de puerta de enlace debe ser capaz de conectarse a dispositivos de bajada de forma segura, recibir comunicaciones de dispositivos de bajada y enrutar los mensajes al destino adecuado.**
2. El dispositivo de bajada debe tener una identidad de dispositivo para poder autenticarse con IoT Hub y saber comunicarse a través de su dispositivo de puerta de enlace. Para más información, consulte [Autenticación de un dispositivo de bajada en Azure IoT Hub](how-to-authenticate-downstream-device.md).
3. El dispositivo de bajada debe poder conectarse de forma segura a su dispositivo de puerta de enlace. Para más información, consulte [Conexión de un dispositivo de bajada a una puerta de enlace Azure IoT Edge](how-to-connect-downstream-device.md).


Para que un dispositivo funcione como puerta de enlace, debe poder conectarse de forma segura a sus dispositivos de bajada. Azure IoT Edge le permite usar una infraestructura de clave pública (PKI) para configurar conexiones seguras entre los dispositivos. En este caso, vamos a permitir que un dispositivo de bajada se conecte a un dispositivo IoT Edge que actúa como puerta de enlace transparente. Para mantener una seguridad razonable, el dispositivo de bajada debe confirmar la identidad del dispositivo de puerta de enlace. Esta comprobación de identidad evita que los dispositivos se conecten a puertas de enlace que pueden ser malintencionadas.

Un dispositivo de bajada en un escenario de puerta de enlace transparente puede ser cualquier aplicación o plataforma que tenga una identidad creada con el servicio en la nube [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub). En muchos casos, estas aplicaciones utilizan el [SDK de dispositivo IoT de Azure](../iot-hub/iot-hub-devguide-sdks.md). En la práctica, un dispositivo de bajada podría ser incluso una aplicación que se ejecuta en el propio dispositivo de puerta de enlace IoT Edge. Sin embargo, un dispositivo IoT Edge no puede ser inferior a una puerta de enlace de IoT Edge. 

Puede crear cualquier infraestructura de certificados que permita la confianza necesaria para la topología de la puerta de enlace de dispositivo. En este artículo se da por hecho que usa la misma configuración de certificado que usaría para habilitar la [seguridad de entidad de certificación X.509](../iot-hub/iot-hub-x509ca-overview.md) en IoT Hub, lo que implica un certificado de entidad de certificación X.509 asociado a un centro de IoT específico (la entidad de certificación raíz del centro de IoT), una serie de certificados firmados con esta entidad de certificación y una entidad de certificación para el dispositivo IoT Edge.

![Configuración del certificado de puerta de enlace](./media/how-to-create-transparent-gateway/gateway-setup.png)

>[!NOTE]
>El término "entidad de certificación raíz" utilizado en este artículo hace referencia al certificado público de entidad de nivel superior de la cadena de certificados de PKI y no necesariamente a la raíz del certificado de una entidad de certificación sindicada. En muchos casos, se trata realmente de un certificado público intermedio de la entidad de certificación. 

La puerta de enlace presenta su certificado de entidad de certificación de IoT Edge al dispositivo de bajada durante el inicio de la conexión. El dispositivo de bajada comprueba que el certificado de entidad de certificación del dispositivo IoT Edge está firmado con el certificado de entidad de certificación raíz. Este proceso permite que el dispositivo de bajada confirme que la puerta de enlace procede de un origen de confianza.

Los pasos siguientes le guían por el proceso de crear los certificados e instalarlos en los lugares adecuados de la puerta de enlace. Puede usar cualquier máquina para generar los certificados y, a continuación, copiarlos en el dispositivo IoT Edge. 

## <a name="prerequisites"></a>Requisitos previos

* Una máquina de desarrollo para crear certificados. 
* Un dispositivo Azure IoT Edge para su configuración como una puerta de enlace. Use los pasos de instalación de IoT Edge para uno de los siguientes sistemas operativos:
  * [Windows](how-to-install-iot-edge-windows.md)
  * [Linux](how-to-install-iot-edge-linux.md)

## <a name="generate-certificates-with-windows"></a>Generación de certificados con Windows

Use los pasos de esta sección para generar certificados de prueba en Windows. Puede usar una máquina de Windows para generar los certificados y, a continuación, copiarlos en cualquier dispositivo IoT Edge que se ejecute en cualquier sistema operativo compatible. 

Los certificados generados en esta sección están pensados solo para fines de prueba. 

### <a name="install-openssl"></a>Instalación de OpenSSL

Instale OpenSSL para Windows en el equipo que usa para generar los certificados. Si ya ha instalado OpenSSL en el dispositivo Windows, puede omitir este paso, pero asegúrese de que openssl.exe está disponible en la variable de entorno PATH. 

Existen varias maneras de instalar OpenSSL, como las siguientes:

* **Más fácil:** Descargue e instale cualquier [archivo binario de OpenSSL de terceros](https://wiki.openssl.org/index.php/Binaries), por ejemplo, de [OpenSSL en SourceForge](https://sourceforge.net/projects/openssl/). Agregue la ruta de acceso completa al archivo openssl.exe a la variable de entorno PATH. 
   
* **Se recomienda que use:** Descargue el código fuente de OpenSSL y compile los archivos binarios en su máquina por sí mismo o a través de [vcpkg](https://github.com/Microsoft/vcpkg). Las instrucciones que aparecen a continuación usan vcpkg para descargar el código fuente, compilar e instalar OpenSSL en el equipo Windows con pasos fáciles.

   1. Vaya al directorio donde quiera instalar vcpkg. Nos referiremos a este directorio como *\<VCPKGDIR>* . Siga las instrucciones para descargar e instalar [vcpkg](https://github.com/Microsoft/vcpkg).
   
   2. Después de instalar vcpkg, ejecute el siguiente comando desde un símbolo del sistema de Powershell para instalar el paquete de OpenSSL para Windows x64. Esta instalación suele tardar, aproximadamente, 5 minutos en completarse.

      ```powershell
      .\vcpkg install openssl:x64-windows
      ```
   3. Agregue `<VCPKGDIR>\installed\x64-windows\tools\openssl` a la variable de entorno PATH para que el archivo openssl.exe esté disponible para su invocación.

### <a name="prepare-creation-scripts"></a>Preparación de los scripts de creación

El repositorio de Git de Azure IoT Edge contiene scripts que puede usar para generar certificados de prueba. En esta sección, se clona el repositorio de IoT Edge y se ejecutan los scripts. 

1. Abra una ventana de Azure PowerShell en modo de administrador. 

2. Clone el repositorio de git que contiene los scripts para generar los certificados para entornos que no sean de producción. Estos scripts ayudan a crear los certificados necesarios para configurar una puerta de enlace transparente. Use el comando `git clone` o [descargue el archivo ZIP](https://github.com/Azure/iotedge/archive/master.zip). 

   ```powershell
   git clone https://github.com/Azure/iotedge.git
   ```

3. Vaya al directorio en el que quiere trabajar. En este artículo, llamaremos a este directorio *\<WRKDIR>* . Todos los certificados y claves se crearán en este directorio de trabajo.

4. Copie los archivos de configuración y de script del repositorio clonado en el directorio de trabajo. 

   ```powershell
   copy <path>\iotedge\tools\CACertificates\*.cnf .
   copy <path>\iotedge\tools\CACertificates\ca-certs.ps1 .
   ```

   Si descargó el repositorio como un archivo ZIP, el nombre de la carpeta es `iotedge-master` y el resto de la ruta de acceso es igual. 
<!--
5. Set environment variable OPENSSL_CONF to use the openssl_root_ca.cnf configuration file.

    ```powershell
    $env:OPENSSL_CONF = "$PWD\openssl_root_ca.cnf"
    ```
-->
5. Habilite PowerShell para ejecutar los scripts.

   ```powershell
   Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser
   ```

7. Lleve las funciones usadas por los scripts al espacio de nombres global de PowerShell.
   
   ```powershell
   . .\ca-certs.ps1
   ```

   La ventana de PowerShell mostrará una advertencia para indicar que los certificados generados por ese script son solo para fines de prueba y no se deben usar en escenarios de producción.

8. Compruebe que OpenSSL se ha instalado correctamente y asegúrese de que no haya conflictos de nombres con los certificados existentes. Si hay problemas, el script debe describir cómo corregirlos en el sistema.

   ```powershell
   Test-CACertsPrerequisites
   ```

### <a name="create-certificates"></a>Creación de certificados

En esta sección, se crean tres certificados y, a continuación, se conectan en una cadena. La colocación de los certificados en un archivo de cadena permite instalarlos fácilmente en el dispositivo de puerta de enlace IoT Edge y los dispositivos de bajada.  

1. Cree el certificado de entidad de certificación raíz y firme con él un certificado intermedio. Los certificados se colocan en el directorio de trabajo.

   ```powershell
   New-CACertsCertChain rsa
   ```

   Este comando de script crea varios archivos de certificado y clave, pero más adelante en este artículo se hará referencia a uno en concreto:
   * `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

2. Cree el certificado de entidad de certificación del dispositivo IoT Edge y una clave privada con el siguiente comando. Proporcione un nombre para el certificado de la entidad de certificación, por ejemplo **MyEdgeDeviceCA**. Este nombre se usa para asignar nombre a los archivos y durante la generación del certificado. 

   ```powershell
   New-CACertsEdgeDeviceCA "MyEdgeDeviceCA"
   ```

   Este comando de script crea varios archivos de certificado y clave, incluidos dos a los que se hará referencia más adelante en este artículo:
   * `<WRKDIR>\certs\iot-edge-device-ca-MyEdgeDeviceCA-full-chain.cert.pem`
   * `<WRKDIR>\private\iot-edge-device-ca-MyEdgeDeviceCA.key.pem`

   >[!TIP]
   >Si indica un nombre distinto de **MyEdgeDeviceCA**, los certificados y claves creados por este comando reflejarán ese nombre. 

Ahora que tiene los certificados, puede ir directamente a [Instalación de los certificados en la puerta de enlace](#install-certificates-on-the-gateway).

## <a name="generate-certificates-with-linux"></a>Generación de certificados con Linux

Use los pasos de esta sección para generar certificados de prueba en Linux. Puede usar una máquina Linux para generar los certificados y, a continuación, copiarlos en cualquier dispositivo IoT Edge que se ejecute en cualquier sistema operativo compatible. 

Los certificados generados en esta sección están pensados solo para fines de prueba. 

### <a name="prepare-creation-scripts"></a>Preparación de los scripts de creación

El repositorio de Git de Azure IoT Edge contiene scripts que puede usar para generar certificados de prueba. En esta sección, se clona el repositorio de IoT Edge y se ejecutan los scripts. 

1. Clone el repositorio de git que contiene los scripts para generar los certificados para entornos que no sean de producción. Estos scripts ayudan a crear los certificados necesarios para configurar una puerta de enlace transparente. 

   ```bash
   git clone https://github.com/Azure/iotedge.git
   ```

2. Vaya al directorio en el que quiere trabajar. Nos referiremos a este directorio en el artículo como *\<WRKDIR>* . Todos los archivos de certificados y claves se crearán en este directorio.
  
3. Copie los archivos de configuración y de script del repositorio de IoT Edge clonado en el directorio de trabajo.

   ```bash
   cp <path>/iotedge/tools/CACertificates/*.cnf .
   cp <path>/iotedge/tools/CACertificates/certGen.sh .
   ```

<!--
4. Configure OpenSSL to generate certificates using the provided script. 

   ```bash
   chmod 700 certGen.sh 
   ```
-->

### <a name="create-certificates"></a>Creación de certificados

En esta sección, se crean tres certificados y, a continuación, se conectan en una cadena. La colocación de los certificados en un archivo de cadena permite instalarlos fácilmente en el dispositivo de puerta de enlace IoT Edge y los dispositivos de bajada.  

1. Cree el certificado de entidad de certificación raíz y un certificado intermedio. Estos certificados se colocan en *\<WRKDIR>* .

   Si ya ha creado certificados raíz e intermedios en este directorio de trabajo, no vuelva a ejecutar este script. Si lo hace, se sobrescribirán los certificados existentes. Continúe con el paso siguiente. 

   ```bash
   ./certGen.sh create_root_and_intermediate
   ```

   El script crea varios certificados y claves. Tome nota de uno, al que haremos referencia en la sección siguiente:
   * `<WRKDIR>/certs/azure-iot-test-only.root.ca.cert.pem`

2. Cree el certificado de entidad de certificación del dispositivo IoT Edge y una clave privada con el siguiente comando. Proporcione un nombre para el certificado de la entidad de certificación, por ejemplo **MyEdgeDeviceCA**. Este nombre se usa para asignar nombre a los archivos y durante la generación del certificado. 

   ```bash
   ./certGen.sh create_edge_device_ca_certificate "MyEdgeDeviceCA"
   ```

   El script crea varios certificados y claves. Tome nota de dos, a los que haremos referencia en la sección siguiente: 
   * `<WRKDIR>/certs/iot-edge-device-ca-MyEdgeDeviceCA-full-chain.cert.pem`
   * `<WRKDIR>/private/iot-edge-device-ca-MyEdgeDeviceCA.key.pem`

   >[!TIP]
   >Si indica un nombre distinto de **MyEdgeDeviceCA**, los certificados y claves creados por este comando reflejarán ese nombre. 

## <a name="install-certificates-on-the-gateway"></a>Instalación de los certificados en la puerta de enlace

Ahora que ha creado una cadena de certificados, debe instalarla en el dispositivo de puerta de enlace IoT Edge y configurar el entorno de ejecución de IoT Edge para que haga referencia a los nuevos certificados. 

1. Copie los archivos siguientes desde *\<WRKDIR>* . Guárdelos en cualquier lugar en el dispositivo IoT Edge. Nos referiremos al directorio de destino en el dispositivo IoT Edge como *\<CERTDIR>* . 

   * Certificado de entidad de certificación del dispositivo: `<WRKDIR>\certs\iot-edge-device-ca-MyEdgeDeviceCA-full-chain.cert.pem`
   * Clave privada de entidad de certificación del dispositivo: `<WRKDIR>\private\iot-edge-device-ca-MyEdgeDeviceCA.key.pem`
   * Entidad de certificación raíz: `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

   Puede usar un servicio como [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) o una función como [Protocolo de copia segura](https://www.ssh.com/ssh/scp/) para mover los archivos de certificado.  Si ha generado los certificados en el propio dispositivo IoT Edge, puede omitir este paso y usar la ruta de acceso al directorio de trabajo.

2. Abra el archivo de configuración del demonio de seguridad de IoT Edge. 

   * Windows: `C:\ProgramData\iotedge\config.yaml`
   * Linux: `/etc/iotedge/config.yaml`

3. Establezca las propiedades de **certificado** del archivo config.yaml en la ruta de acceso completa a los archivos de certificado y de clave del dispositivo IoT Edge. Elimine el carácter `#` antes de las propiedades de certificado para quitar la marca de comentario de las cuatro líneas. Recuerde que las sangrías de YAML son dos espacios.

   * Windows:

      ```yaml
      certificates:
        device_ca_cert: "<CERTDIR>\\certs\\iot-edge-device-ca-MyEdgeDeviceCA-full-chain.cert.pem"
        device_ca_pk: "<CERTDIR>\\private\\iot-edge-device-ca-MyEdgeDeviceCA.key.pem"
        trusted_ca_certs: "<CERTDIR>\\certs\\azure-iot-test-only.root.ca.cert.pem"
      ```
   
   * Linux: 
      ```yaml
      certificates:
        device_ca_cert: "<CERTDIR>/certs/iot-edge-device-ca-MyEdgeDeviceCA-full-chain.cert.pem"
        device_ca_pk: "<CERTDIR>/private/iot-edge-device-ca-MyEdgeDeviceCA.key.pem"
        trusted_ca_certs: "<CERTDIR>/certs/azure-iot-test-only.root.ca.cert.pem"
      ```

4. En los dispositivos Linux, asegúrese de que el usuario **iotedge** tiene permisos de lectura para el directorio que contiene los certificados. 

## <a name="deploy-edgehub-to-the-gateway"></a>Implementación de EdgeHub en la puerta de enlace

Cuando se instala IoT Edge por primera vez en un dispositivo, se inicia automáticamente un único módulo del sistema: el agente de IoT Edge. Para que el dispositivo funcione como una puerta de enlace, necesita ambos módulos del sistema. Si nunca ha implementado módulos en el dispositivo de puerta de enlace, cree una implementación inicial para que el dispositivo inicie el segundo módulo del sistema, el centro de IoT Edge. La implementación parecerá vacía porque no agrega ningún módulo en el asistente, pero garantizará que se ejecutan ambos módulos del sistema. 

Puede comprobar qué módulos se ejecutan en un dispositivo con el comando `iotedge list`. Si la lista solo devuelve el módulo **edgeAgent** sin **edgeHub**, siga estos pasos:

1. En Azure Portal, vaya hasta el centro de IoT.

2. Vaya a **IoT Edge** y seleccione el dispositivo IoT Edge que quiere usar como puerta de enlace.

3. Seleccione **Set modules** (Establecer módulos).

4. Seleccione **Next** (Siguiente).

5. En la página **Specify routes** (Especificar rutas), debe tener una ruta predeterminada que envíe todos los mensajes de todos los módulos a IoT Hub. Si no es así, agregue el código siguiente y seleccione **Next** (Siguiente).

   ```JSON
   {
       "routes": {
           "route": "FROM /* INTO $upstream"
       }
   }
   ```

6. En la página **Review template** (Revisar plantilla), seleccione **Submit** (Enviar).

## <a name="open-ports-on-gateway-device"></a>Apertura de puertos en el dispositivo de puerta de enlace

Los dispositivos IoT Edge estándar no necesitan conectividad entrante a para funcionar, porque toda la comunicación con IoT Hub se realiza a través de conexiones salientes. Los dispositivos de puerta de enlace son diferentes porque deben recibir mensajes de sus dispositivos de bajada. Si hay un firewall entre los dispositivos de bajada y el dispositivo de puerta de enlace, la comunicación también debe ser posible a través de él.

Para que un escenario de puerta de enlace funcione, al menos uno de los protocolos admitidos en el centro de IoT Edge debe estar abierto para el tráfico entrante procedente de los dispositivos de bajada. Los protocolos compatibles son MQTT, AMQP y HTTPS. 

| Port | Protocolo |
| ---- | -------- |
| 8883 | MQTT |
| 5671 | AMQP |
| 443 | HTTPS <br> MQTT + WS <br> AMQP + WS | 

## <a name="route-messages-from-downstream-devices"></a>Enrutamiento de mensajes desde dispositivos de bajada
El entorno de ejecución de Azure IoT Edge puede enrutar los mensajes enviados desde dispositivos de bajada igual que los mensajes enviados por los módulos. Esta característica permite realizar análisis en un módulo que se ejecuta en la puerta de enlace antes de enviar datos a la nube. 

Actualmente, la manera de enrutar los mensajes enviados por los dispositivos de bajada es diferenciándolos de los mensajes enviados por los módulos. Los mensajes enviados por los módulos contienen una propiedad del sistema denominada **connectionModuleId**, pero los mensajes enviados por los dispositivos de bajada no la tienen. Puede usar la cláusula WHERE de la ruta para excluir los mensajes que contengan dicha propiedad del sistema. 

La siguiente ruta es un ejemplo que enviaría mensajes desde cualquier dispositivo de bajada a un módulo denominado `ai_insights` y, a continuación, desde `ai_insights` a IoT Hub.

```json
{
    "routes":{
        "sensorToAIInsightsInput1":"FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/ai_insights/inputs/input1\")", 
        "AIInsightsToIoTHub":"FROM /messages/modules/ai_insights/outputs/output1 INTO $upstream" 
    } 
}
```

Para obtener más información sobre el enrutamiento de mensajes, vea [Implementar módulos y establecer rutas](./module-composition.md#declare-routes).


## <a name="enable-extended-offline-operation"></a>Habilitación del funcionamiento sin conexión extendido

A partir de la [versión v1.0.4](https://github.com/Azure/azure-iotedge/releases/tag/1.0.4) del entorno de ejecución de Azure IoT Edge, el dispositivo de puerta de enlace y los dispositivos de bajada conectados a él pueden configurarse para un funcionamiento sin conexión extendido. 

Con esta funcionalidad, los módulos locales o los dispositivos de bajada pueden volver a autenticarse con el dispositivo IOT Edge según sea necesario y comunicarse entre sí mediante el uso de mensajes y métodos incluso cuando están desconectados del centro de IoT. Para obtener más información, consulte [Understand extended offline capabilities for IoT Edge devices, modules, and child devices](offline-capabilities.md) (Entender las capacidades sin conexión extendidas para dispositivos IoT Edge, módulos y dispositivos secundarios).

Para habilitar la funcionalidad sin conexión extendida, se debe establecer una relación principal-secundario entre el dispositivo de puerta de enlace IoT Edge y los dispositivos de bajada que se conectarán a él. Estos pasos se explican con más detalle en [Authenticate a downstream device to Azure IoT Hub](how-to-authenticate-downstream-device.md) (Autenticación de un dispositivo de bajada en Azure IoT Hub).

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene un dispositivo IoT Edge que funciona como una puerta de enlace transparente, deberá configurar los dispositivos de bajada para que confíen en la puerta de enlace y envíen mensajes. Continúe con [Autenticación de un dispositivo de bajada en Azure IoT Hub](how-to-authenticate-downstream-device.md) para ver los pasos siguientes en la configuración del escenario de puerta de enlace transparente. 
