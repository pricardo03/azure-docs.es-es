---
title: Creación de una puerta de enlace transparente con Azure IoT Edge | Microsoft Docs
description: Uso de un dispositivo Azure IoT Edge como una puerta de enlace transparente que puede procesar información para varios dispositivos
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 11/01/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: a48699507fbba18b20cb94e404c4814f25d31f44
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "50915083"
---
# <a name="configure-an-iot-edge-device-to-act-as-a-transparent-gateway"></a>Configuración de un dispositivo IoT Edge para que actúe como puerta de enlace transparente

Este artículo proporciona instrucciones detalladas para configurar dispositivos IoT Edge para que funcionen como una puerta de enlace transparente para que otros dispositivos se comuniquen con IoT Hub. En este artículo, el término *puerta de enlace IoT Edge* hace referencia a un dispositivo IoT Edge que se usa como una puerta de enlace transparente. Para obtener información más detallada, consulte [Uso de un dispositivo IoT Edge como puerta de enlace](./iot-edge-as-gateway.md), que brinda información general conceptual.

>[!NOTE]
>Actualmente:
> * Si la puerta de enlace se desconecta desde IoT Hub, los dispositivos de nivel inferior no se pueden autenticar con la puerta de enlace.
> * Los dispositivos habilitados para Edge no pueden conectarse a puertas de enlace de IoT Edge. 
> * Los dispositivos de bajada no pueden usar la carga de archivos.

Para que un dispositivo funcione como una puerta de enlace, debe poder conectarse de forma segura a los dispositivos de bajada. Azure IoT Edge le permite usar una infraestructura de clave pública (PKI) para configurar conexiones seguras entre los dispositivos. En este caso, vamos a permitir que un dispositivo de bajada se conecte a un dispositivo IoT Edge que actúa como puerta de enlace transparente. Para mantener una seguridad razonable, el dispositivo de bajada debe confirmar la identidad del dispositivo de Edge, porque lo que quiere es que los dispositivos solo se conecten a sus puertas de enlace y no a una puerta de enlace posiblemente malintencionada.

Un dispositivo de bajada puede ser cualquier aplicación o plataforma que tenga una identidad creada con el servicio en la nube [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub). En muchos casos, estas aplicaciones utilizan el [SDK de dispositivo IoT de Azure](../iot-hub/iot-hub-devguide-sdks.md). En la práctica, un dispositivo de bajada podría ser incluso una aplicación que se ejecuta en el propio dispositivo de puerta de enlace IoT Edge. 

Puede crear cualquier infraestructura de certificados que permita la confianza necesaria para la topología de la puerta de enlace de dispositivo. En este artículo, se da por hecho que usa la misma configuración de certificado que usaría para habilitar la [seguridad de entidad de certificación X.509](../iot-hub/iot-hub-x509ca-overview.md) en IoT Hub, lo que implica un certificado de entidad de certificación X.509 asociado a una instancia de IoT Hub específica (la entidad de certificación del propietario de la instancia de IoT Hub) y una serie de certificados, firmados con esta entidad de certificación, y una entidad de certficación para el dispositivo Edge.

![Instalación de la puerta de enlace](./media/how-to-create-transparent-gateway/gateway-setup.png)

La puerta de enlace presenta su certificado de entidad de certificación de Edge al dispositivo de bajada durante el inicio de la conexión. El dispositivo de bajada comprueba que el certificado de entidad de certificación del dispositivo de Edge está firmado con el certificado de entidad de certificación del propietario. Este proceso permite que el dispositivo de bajada confirme que la puerta de enlace procede de un origen de confianza.

Los pasos siguientes le guían por el proceso de crear los certificados e instalarlos en los lugares adecuados.

## <a name="prerequisites"></a>Requisitos previos

Un dispositivo Azure IoT Edge para su configuración como una puerta de enlace. Puede usar la máquina de desarrollo o una máquina virtual como dispositivo IoT Edge siguiendo los pasos que se indican para los sistemas operativos siguientes:
* [Windows](./how-to-install-iot-edge-windows-with-windows.md)
* [Linux x64](./how-to-install-iot-edge-linux.md)
* [Linux ARM32](./how-to-install-iot-edge-linux-arm.md)

Puede usar cualquier máquina para generar los certificados y, a continuación, copiarlos en el dispositivo IoT Edge. 

## <a name="generate-certificates-with-windows"></a>Generación de certificados con Windows

Use los pasos de esta sección para generar certificados de prueba en un dispositivo Windows. Puede generar los certificados en el propio dispositivo IoT Edge o utilizar un equipo independiente y copiar los certificados finales en cualquier dispositivo IoT Edge que ejecute un sistema operativo compatible. 

Los certificados generados en esta sección están pensados solo para fines de prueba. 

### <a name="install-openssl"></a>Instalación de OpenSSL

Instale OpenSSL para Windows en el equipo que usa para generar los certificados. Existen varias maneras de instalar OpenSSL:

   >[!NOTE]
   >Si ya ha instalado OpenSSL en el dispositivo Windows, puede omitir este paso, pero asegúrese de que openssl.exe está disponible en la variable de entorno PATH.

* **Sencillo:** Descargue e instale cualquier [archivo binario de OpenSSL de terceros](https://wiki.openssl.org/index.php/Binaries), por ejemplo, de [este proyecto en SourceForge](https://sourceforge.net/projects/openssl/). Agregue la ruta de acceso completa al archivo openssl.exe a la variable de entorno PATH. 
   
* **Recomendado:** Descargue el código fuente de OpenSSL y compile los archivos binarios en su máquina por sí mismo o mediante [vcpkg](https://github.com/Microsoft/vcpkg). Las instrucciones que aparecen a continuación usan vcpkg para descargar el código fuente, compilar e instalar OpenSSL en el equipo Windows con pasos fáciles.

   1. Vaya al directorio donde quiera instalar vcpkg. Nos referiremos a este directorio como *\<VCPKGDIR>*. Siga las instrucciones para descargar e instalar [vcpkg](https://github.com/Microsoft/vcpkg).
   
   2. Después de instalar vcpkg, desde un símbolo del sistema de Powershell, ejecute el siguiente comando para instalar el paquete de OpenSSL para Windows x64. Esta instalación suele tardar, aproximadamente, 5 minutos en completarse.

      ```PowerShell
      .\vcpkg install openssl:x64-windows
      ```
   3. Agregue `<VCPKGDIR>\installed\x64-windows\tools\openssl` a la variable de entorno PATH para que el archivo openssl.exe esté disponible para su invocación.

### <a name="prepare-creation-scripts"></a>Preparación de los scripts de creación

El SDK de dispositivo IoT de Azure para C contiene scripts que puede usar para generar certificados de prueba. En esta sección, va a clonar el SDK y configurar PowerShell.

1. Abra una ventana de Azure PowerShell en modo de administrador. 

2. Clone el repositorio de git que contiene los scripts para generar los certificados para entornos que no sean de producción. Estos scripts ayudan a crear los certificados necesarios para configurar una puerta de enlace transparente. Use el comando `git clone` o [descargue el archivo ZIP](https://github.com/Azure/azure-iot-sdk-c/archive/master.zip). 

   ```PowerShell
   git clone https://github.com/Azure/azure-iot-sdk-c.git
   ```

3. Vaya al directorio en el que quiere trabajar. Nos referiremos a este directorio como *\<WRKDIR>*.  Todos los archivos se crearán en este directorio.

4. Copie los archivos de configuración y de script en el directorio de trabajo. 

   ```PowerShell
   copy <path>\azure-iot-sdk-c\tools\CACertificates\*.cnf .
   copy <path>\azure-iot-sdk-c\tools\CACertificates\ca-certs.ps1 .
   ```

   Si descargó el SDK como un archivo ZIP, el nombre de la carpeta es `azure-iot-sdk-c-master` y el resto de la ruta de acceso es la misma. 

5. Establezca la variable de entorno OPENSSL_CONF para que use el archivo de configuración openssl_root_ca.cnf.

    ```PowerShell
    $env:OPENSSL_CONF = "$PWD\openssl_root_ca.cnf"
    ```

6. Habilite PowerShell para ejecutar los scripts.

   ```PowerShell
   Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser
   ```

7. Lleve las funciones, usadas por los scripts, al espacio de nombres global de PowerShell.
   
   ```PowerShell
   . .\ca-certs.ps1
   ```

8. Compruebe que OpenSSL se ha instalado correctamente y asegúrese de que no haya conflictos de nombres con los certificados existentes. Si hay problemas, el script debe describir cómo corregirlos en el sistema.

   ```PowerShell
   Test-CACertsPrerequisites
   ```

### <a name="create-certificates"></a>Creación de certificados

En esta sección, se crean tres certificados y, a continuación, se conectan en una cadena. La colocación de los certificados en un archivo de cadena permite instalarlos fácilmente en el dispositivo de puerta de enlace IoT Edge y los dispositivos de bajada.  

1. Cree el certificado de entidad de certificación de propietario y firme con él un certificado intermedio. Los certificados se colocan en *\<WRKDIR>*.

      ```PowerShell
      New-CACertsCertChain rsa
      ```

2. Cree el certificado de entidad de certificación del dispositivo Edge y una clave privada con el siguiente comando. Proporcione un nombre para el dispositivo de puerta de enlace, que se usará para dar nombre a los archivos y durante la generación del certificado. 

   ```PowerShell
   New-CACertsEdgeDevice "<gateway name>"
   ```

3. Cree una cadena de certificado a partir del certificado de la entidad de certificación del propietario, el certificado intermedio y el certificado de la entidad de certificación del dispositivo Edge con el siguiente comando. 

   ```PowerShell
   Write-CACertsCertificatesForEdgeDevice "<gateway name>"
   ```

   El script crea los certificados y la clave siguientes:
   * `<WRKDIR>\certs\new-edge-device.*`
   * `<WRKDIR>\private\new-edge-device.key.pem`
   * `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

## <a name="generate-certificates-with-linux"></a>Generación de certificados con Linux

Use los pasos de esta sección para generar certificados de prueba en un dispositivo Linux. Puede generar los certificados en el propio dispositivo IoT Edge o utilizar un equipo independiente y copiar los certificados finales en cualquier dispositivo IoT Edge que ejecute un sistema operativo compatible. 

### <a name="prepare-creation-scripts"></a>Preparación de los scripts de creación

1. Clone el repositorio de git que contiene los scripts para generar los certificados para entornos que no sean de producción. Estos scripts ayudan a crear los certificados necesarios para configurar una puerta de enlace transparente. 

   ```bash
   git clone https://github.com/Azure/azure-iot-sdk-c.git
   ```

2. Vaya al directorio en el que quiere trabajar. Nos referiremos a este directorio como *\<WRKDIR>*.  Todos los archivos se crearán en este directorio.
  
3. Copie los archivos de configuración y de script en el directorio de trabajo.

   ```bash
   cp <path>/azure-iot-sdk-c/tools/CACertificates/*.cnf .
   cp <path>/azure-iot-sdk-c/tools/CACertificates/certGen.sh .
   ```

4. Configure OpenSSL para generar los certificados mediante el script proporcionado. 

   ```bash
   chmod 700 certGen.sh 
   ```

### <a name="create-certificates"></a>Creación de certificados

En esta sección, se crean tres certificados y, a continuación, se conectan en una cadena. La colocación de los certificados en un archivo de cadena permite instalarlos fácilmente en el dispositivo de puerta de enlace IoT Edge y los dispositivos de bajada.  

1.  Cree el certificado de entidad de certificación de propietario y un certificado intermedio. Estos certificados se colocan en *\<WRKDIR>*.

   ```bash
   ./certGen.sh create_root_and_intermediate
   ```

   El script crea los certificados y las claves siguientes:
   * `<WRKDIR>/certs/azure-iot-test-only.root.ca.cert.pem`
   * `<WRKDIR>/certs/azure-iot-test-only.intermediate.cert.pem`
   * `<WRKDIR>/private/azure-iot-test-only.root.ca.key.pem`
   * `<WRKDIR>/private/azure-iot-test-only.intermediate.key.pem`

2.  Cree el certificado de entidad de certificación del dispositivo Edge y una clave privada con el siguiente comando. Proporcione un nombre para el dispositivo de puerta de enlace, que se usará para dar nombre a los archivos y durante la generación del certificado. 

   ```bash
   ./certGen.sh create_edge_device_certificate "<gateway name>"
   ```

   El script crea los certificados y la clave siguientes:
   * `<WRKDIR>/certs/new-edge-device.*`
   * `<WRKDIR>/private/new-edge-device.key.pem`

3. Cree una cadena de certificados llamada **new-edge-device-full-chain.cert.pem** a partir del certificado de la entidad de certificación del propietario, el certificado intermedio y el certificado de la entidad de certificación del dispositivo Edge.

   ```bash
   cat ./certs/new-edge-device.cert.pem ./certs/azure-iot-test-only.intermediate.cert.pem ./certs/azure-iot-test-only.root.ca.cert.pem > ./certs/new-edge-device-full-chain.cert.pem
   ```

## <a name="install-certificates-on-the-gateway"></a>Instalación de los certificados en la puerta de enlace

Ahora que ha creado una cadena de certificados, debe instalarla en el dispositivo de puerta de enlace IoT Edge y configurar el entorno de ejecución de IoT Edge para que haga referencia a los nuevos certificados. 

1. Copie los archivos siguientes desde *\<WRKDIR>*. Guárdelos en cualquier lugar en el dispositivo IoT Edge. Nos referiremos al directorio de destino en el dispositivo IoT Edge como *\<CERTDIR>*. 

   Si ha generado los certificados en el propio dispositivo Edge, puede omitir este paso y usar la ruta de acceso al directorio de trabajo.

   * Certificado de entidad de certificación de dispositivo: `<WRKDIR>\certs\new-edge-device-full-chain.cert.pem`
   * Clave privada de entidad de certificación de dispositivo: `<WRKDIR>\private\new-edge-device.key.pem`
   * Entidad de certificación de propietario: `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

2. Abra el archivo de configuración del demonio de seguridad de IoT Edge. 

   * Windows: `C:\ProgramData\iotedge\config.yaml`
   * Linux: `/etc/iotedge/config.yaml`

3. Establezca las propiedades del **certificado** en el archivo config.yaml en la ruta de acceso en la que ha colocado los archivos de certificado y de clave en el dispositivo IoT Edge.

```yaml
certificates:
  device_ca_cert: "<CERTDIR>\\certs\\new-edge-device-full-chain.cert.pem"
  device_ca_pk: "<CERTDIR>\\private\\new-edge-device.key.pem"
  trusted_ca_certs: "<CERTDIR>\\certs\\azure-iot-test-only.root.ca.cert.pem"
```

## <a name="deploy-edgehub-to-the-gateway"></a>Implementación de EdgeHub en la puerta de enlace

Cuando se instala por primera vez IoT Edge en un dispositivo, se inicia automáticamente un único módulo del sistema: el agente de Edge. Para que el dispositivo funcione como una puerta de enlace, necesita ambos módulos del sistema. Si nunca ha implementado módulos en el dispositivo de puerta de enlace, cree una implementación para que el dispositivo inicie el segundo módulo del sistema, el centro de Edge. La implementación parecerá vacía porque no agrega ningún módulo en el asistente, pero implementará ambos módulos del sistema. 

Puede comprobar qué módulos se ejecutan en un dispositivo con el comando `iotedge list`.

1. En Azure Portal, navegue hasta el centro de IoT.

2. Vaya a **IoT Edge** y seleccione el dispositivo IoT Edge que quiere usar como puerta de enlace.

3. Seleccione **Set modules** (Establecer módulos).

4. Seleccione **Next** (Siguiente).

5. En la página **Specify routes** (Especificar rutas), debe tener una ruta predeterminada que envíe todos los mensajes de todos los módulos a IoT Hub. Si no es así, agregue el código siguiente y, a continuación, seleccione **Next** (Siguiente).

   ```JSON
   {
       "routes": {
           "route": "FROM /* INTO $upstream"
       }
   }
   ```

6. En la página **Review template** (Revisar plantilla), seleccione **Submit** (Enviar).

## <a name="route-messages-from-downstream-devices"></a>Enrutamiento de mensajes desde dispositivos de bajada
El entorno de ejecución de Azure IoT Edge puede enrutar los mensajes enviados desde dispositivos de bajada igual que los mensajes enviados por los módulos. Esto le permite realizar análisis en un módulo que se ejecuta en la puerta de enlace antes de enviar datos a la nube. La siguiente ruta se usaría para enviar mensajes desde un dispositivo de bajada llamado `sensor` a un módulo llamado `ai_insights`.

```json
{
    "routes":{
        "sensorToAIInsightsInput1":"FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/ai_insights/inputs/input1\")", 
        "AIInsightsToIoTHub":"FROM /messages/modules/ai_insights/outputs/output1 INTO $upstream" 
    } 
}
```

Para obtener más información sobre el enrutamiento de mensajes, consulte la [composición del módulo](./module-composition.md).

[!INCLUDE [iot-edge-extended-ofline-preview](../../includes/iot-edge-extended-offline-preview.md)]

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene un dispositivo IoT Edge que funciona como una puerta de enlace transparente, deberá configurar los dispositivos de bajada para que confíen en la puerta de enlace y envíen mensajes. Para más información, consulte [Conexión de un dispositivo de bajada a una puerta de enlace Azure IoT Edge](how-to-connect-downstream-device.md).
