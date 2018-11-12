---
title: Conectividad de dispositivos en Azure IoT Central | Microsoft Docs
description: En este artículo se presentan conceptos clave relacionados con la conectividad de dispositivos en Azure IoT Central
author: dominicbetts
ms.author: dobett
ms.date: 11/30/2017
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: 7b2dd4e97d23f37c8261e51f3f65e78436493ddc
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51238747"
---
# <a name="device-connectivity-in-azure-iot-central"></a>Conectividad de dispositivos en Azure IoT Central

En este artículo se presentan conceptos clave relacionados con la conectividad de dispositivos en Microsoft Azure IoT Central.

Azure IoT Central usa [Azure IoT Hub Device Provisioning Service (DPS)](https://docs.microsoft.com/azure/iot-dps/about-iot-dps), permitiendo así que IoT Central admita la incorporación y conexión de dispositivos a escala.

-   Los clientes ahora pueden crear credenciales de dispositivo y configurar los dispositivos si conexión sin tener que registrarlos primero en IoT Central.
-   IoT Central admite la conexión de dispositivos con la conectividad X509 basada en certificados que recomienda el sector, a la vez que proporciona soporte técnico y mejora la conectividad de la Firma de acceso compartido (SAS).
-   Los clientes de IoT Central pueden traer sus propios identificadores de dispositivos para registrar sus dispositivos en IoT Central, lo que les permite realizar una integración sencilla mediante los sistemas de áreas de operaciones existentes.
-   Existe una manera consistente de conectar dispositivos a IoT Central. 

>[!NOTE]
>IoT Central usa Azure IoT Device Provisioning Service (DPS) para registrar y conectar todos los dispositivos; [obtenga más información sobre DPS](https://docs.microsoft.com/azure/iot-dps/about-iot-dps).

En función de su caso de uso, siga las instrucciones para conectar dispositivos a IoT Central.
1. [Conecte un solo dispositivo rápidamente (mediante las firmas de acceso compartido)](#connect-a-single-device)
1. [Conecte dispositivos a escala mediante las firmas de acceso compartido (SAS)](#connect-devices-at-scale-using-shared-access-signatures)
1. [Conecte dispositivos a escala mediante certificados X509](#connect-devices-using-x509-certificates) **recomendado para las cargas de trabajo de producción**
1. [Conéctese sin registrar primero los dispositivos](#connect-without-first-registering-devices) 


>[!NOTE]
>Aquí está el punto de conexión global para que los dispositivos se conecten y aprovisionen en **global.azure-devices-provisioning.net**.

## <a name="connect-a-single-device"></a>Conectar un solo dispositivo
El modo de conectar un solo dispositivo a IoT Central mediante SAS es fácil y solo le llevará unos pocos pasos. 
1. Agregue un **dispositivo real** en Device Explorer, haga clic en **+New > Real** (+Nuevo > Real) para agregar un dispositivo real.
    * Escriba el identificador de dispositivo **<span style="color:Red">(debe estar en minúsculas)</span>** o use el identificador de dispositivo sugerido.
    * Escriba el nombre del dispositivo o use el nombre sugerido   
    ![Agregar dispositivo](media\concepts-connectivity\add-device.png)
1. Obtenga detalles de conexión como el **id. de ámbito, el id. de dispositivo y la clave principal** para el dispositivo agregado; para ello, haga clic en **Conectar** en la página del dispositivo.
    * **[Id. de ámbito](https://docs.microsoft.com/azure/iot-dps/concepts-device#id-scope)**: se proporciona según la aplicación IoT Central y lo genera DPS; se usa para asegurar un id. de dispositivo único en una aplicación.
    * **Id. de dispositivo**: es el id. de dispositivo único según la aplicación; el dispositivo debe enviar el id. de dispositivo como parte de la llamada de registro.   
    * **Clave principal**: es un token de SAS, que crea IoT Central para este dispositivo específico. 
    ![Detalles de conexión](media\concepts-connectivity\device-connect.PNG)
1. Use estos detalles de conexión **Identidad del dispositivo, Nombre del dispositivo y Clave principal del dispositivo**  en el código de su dispositivo para aprovisionar y conectar el dispositivo y comenzar a ver el flujo de datos de forma instantánea. Si está usando el dispositivo MxChip, siga [las instrucciones paso a paso que se detallan aquí](howto-connect-devkit.md#add-a-real-device) y comience desde la sección **Preparación del dispositivo DevKit**.   

    A continuación se muestran las referencias de otros idiomas que puede utilizar.

    *   **Lenguaje C:** si está usando C, siga [este cliente de dispositivo de ejemplo de C](https://github.com/Azure/azure-iot-sdk-c/blob/dps_symm_key/provisioning_client/devdoc/using_provisioning_client.md) para conectar un dispositivo de muestra. Utilice la siguiente configuración en la muestra.   

         ```
         hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;
         
         static const char* const SYMMETRIC_KEY_VALUE = "Enter Primary Symmetric key here";

         static const char* const REGISTRATION_NAME = "Enter Device Id here";
        ```

    *   **Node.js:** si quiere usar Node.js [use las instrucciones paso a paso aquí](tutorial-add-device.md#prepare-the-client-code); comience desde la sección **Preparar el código de cliente**.



## <a name="connect-devices-at-scale-using-shared-access-signatures"></a>Conecte los dispositivos a escala mediante las firmas de acceso compartido

Para conectar dispositivos a escala con IoT Central mediante SAS, tiene que realizar dos pasos. 
1. **Registre los dispositivos** importándolos a IoT Central a través de un archivo CSV y expórtelos proporcionando los detalles de conexión que usará para conectarlos.
1. **Configuración del dispositivo**; el dispositivo está programado con los detalles de conexión (**id. de ámbito, id.de dispositivo y clave principal**), lo que le permite llamar al servicio de aprovisionamiento para obtener la información de conexión y la asignación de aplicaciones de IoT Central cuando se enciende.

>[!NOTE]
>También tiene disponible una opción avanzada donde puede conectar dispositivos sin tener que registrarlos primero en IoT Central; [obtenga más información aquí](https://docs.microsoft.com/azure/iot-dps/about-iot-dps).

**Registro de dispositivos**

Para conectar un gran número de dispositivos a la aplicación, Azure IoT Central ofrece importar dispositivos en bloque a través de un archivo CSV. 

Requisitos del archivo CSV: el archivo CSV debe tener las siguientes columnas (y encabezados)
1.  IOTC_DeviceID **<span style="color:Red">(debe estar en minúscula)</span>**
1.  IOTC_DeviceName (opcional)



Importe dispositivos para registrarlos en la aplicación.
1.  Elija **Explorer** en el menú de navegación de la izquierda.
1.  En el panel izquierdo, elija la plantilla de dispositivo para la que desea crear dispositivos en bloque. 
1.  Haga clic en **Importar**, seleccione el archivo CSV que tiene la lista de identificadores de dispositivo que quiere importar.
El archivo CSV debe tener las siguientes columnas (y encabezados)
    *   IOTC_DeviceID **<span style="color:Red">(debe estar en minúscula)</span>**
    *   IOTC_DeviceName (opcional)
1.  Una vez finalizada la importación, se muestra un mensaje de confirmación en la cuadrícula de dispositivos.

Exporte los dispositivos para obtener los detalles de la conexión; el proceso de exportación crea un archivo CSV con el id. de dispositivo, el nombre del dispositivo y la clave del dispositivo. Use estos detalles para conectar el dispositivo a IoT Central.
Para realizar la exportación masiva de dispositivos desde la aplicación:
1.  Elija **Explorer** en el menú de navegación de la izquierda.
1.  Seleccione los dispositivos que desea exportar y, a continuación, haga clic en la acción **Exportar**.
1.  Una vez finalizada la exportación, se muestra un mensaje de confirmación junto con un vínculo para descargar el archivo generado.
1.  Haga clic en el mensaje de operación correcta para descargar el archivo en una carpeta local en el disco.
1.  El archivo CSV exportado tendrá la siguiente información de columnas: **identificador de dispositivo, nombre de dispositivo, claves primaria y secundaria del dispositivo y huellas digitales del certificado primario y secundario**
    *   IOTC_DEVICEID
    *   IOTC_DEVICENAME
    *   IOTC_SASKEY_PRIMARY
    *   IOTC_SASKEY_SECONDARY
    *   IOTC_X509THUMBPRINT_PRIMARY 
    *   IOTC_X509THUMBPRINT_SECONDARY


**Configuración del dispositivo**

Use estos detalles de conexión, **identidad del dispositivo (IOTC_DEVICEID), clave principal del dispositivo (IOTC_SASKEY_PRIMARY) y el identificador de ámbito** en el código del dispositivo para aprovisionar y conectar el mismo. Si aún no lo tiene, obtenga el **id. de ámbito** de la aplicación IoT Central **Administración > Conexión del dispositivo > id. de ámbito**.
Si está usando el dispositivo **MxChip**, siga las [instrucciones paso a paso que se detallan aquí](howto-connect-devkit.md#add-a-real-device) para conectarlo y comience desde la sección **Preparación del dispositivo DevKit**.   

A continuación se muestran las referencias de otros idiomas que puede utilizar.

   *   **Lenguaje C:** si está usando C, siga [este cliente de dispositivo de ejemplo de C](https://github.com/Azure/azure-iot-sdk-c/blob/dps_symm_key/provisioning_client/devdoc/using_provisioning_client.md) para conectar un dispositivo de muestra. Utilice la siguiente configuración en la muestra.   
         ```
         hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;

         static const char* const SYMMETRIC_KEY_VALUE = "Enter Primary Symmetric key here";
         static const char* const REGISTRATION_NAME = "Enter Device Id here";
        ```
    * **Node.js:** si quiere usar Node.js [use las instrucciones paso a paso aquí](tutorial-add-device.md#prepare-the-client-code); comience desde la sección **Preparar el código de cliente**.


## <a name="connect-devices-using-x509-certificates"></a>Conectar dispositivos mediante certificados X509

El uso de certificados X.509 como un mecanismo de atestación es una manera excelente para escalar la producción y simplificar el aprovisionamiento de dispositivos. Los certificados X.509 normalmente están organizados en una cadena de certificados de confianza en la que cada certificado de la cadena está firmado por la clave privada del certificado superior siguiente y así sucesivamente, terminando en un certificado raíz autofirmado. Esto establece una cadena de confianza delegada a partir del certificado raíz generado por una entidad de certificación (CA) raíz de confianza que va descendiendo mediante cada entidad de certificación intermedia hasta el certificado "de hoja" de entidad final instalado en un dispositivo. Para más información consulte [Autenticación de dispositivos mediante certificados de entidades de certificación X.509](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-overview). 

Para conectar dispositivos a IoT Central mediante certificados X509, hay tres pasos clave: 
1. **Configure las opciones de conexión** en la aplicación IoT Central; para ello, agregue o verifique el certificado raíz o intermedio X509 que se usa para generar los certificados del dispositivo.  Hay dos pasos para configurar las opciones de conexión de los certificados X509.  

    *   **Agregue el certificado raíz o intermedio X509**  que está usando para generar los certificados del dispositivo hoja. Vaya a Administración > Conexión del dispositivo > Certificados. 
    
        ![Configuración de conexión](media\concepts-connectivity\connection-settings.PNG)
    *   **Verificación de certificados:** la verificación de la titularidad del certificado garantizar que la persona que carga el certificado está en posesión de su clave privada. Para verificar el certificado:
        *  Cree el código de verificación, haga clic en el botón situado junto al campo de código de verificación para generar ese código de verificación. 
        *  Cree un certificado de verificación X.509 con el código de verificación y guarde el certificado como un archivo .cer. 
        *  Cargue el certificado de verificación firmado y haga clic en Verificar.

        ![Configuración de conexión](media\concepts-connectivity\verify-cert.png)
    *   **Certificado secundario**: durante el ciclo de vida de la solución de IoT, necesitará implementar certificados. Dos de las razones principales para implementar certificados sería una infracción de seguridad y la caducidad de los certificados. Los certificados secundarios se usan para reducir el tiempo de inactividad de los dispositivos que intente aprovisionar mientras actualiza el certificado principal.

    **SOLO PARA FINES DE PRUEBA** 
    
    A continuación se muestran algunas herramientas útiles de línea de comandos que puede usar para generar certificados de entidad de certificación y certificados de dispositivo.

    * Si usa MxChip, aquí tiene una [herramienta de línea de comandos](https://aka.ms/iotcentral-docs-dicetool) para generar certificados de entidad de certificación que le permitirá agregarlos a la aplicación IoT Central y comprobar los mismos. 

    *   Use esta [herramienta de línea de comandos](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md ) para:
        * Crear la cadena de certificados (siga el paso 2 de los documentos de GitHub). 
         Guardar los certificados como archivos .cer y cargarlos en IoT Central (principal).   
        * Obtener el código de verificación de la aplicación IoT Central, generar el certificado (siga el paso 3 de la documentación de GitHub) y cargarlo para su verificación. 
        * Crear certificados de hoja con el id. de dispositivo como un parámetro de la herramienta (siga el paso 4). Guarde el certificado y úselo en el dispositivo.     

1. **Registre dispositivos** importándolos en IoT Central mediante de un archivo CSV.

1. **Configuración del dispositivo**: genere los certificados hoja con el certificado raíz cargado. Asegúrese de utilizar el **id. de dispositivo** como elemento CNAME en los certificados hoja y escríbalo en **minúsculas**. Aquí tiene una [herramienta de línea de comandos](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md ) para generar certificados hoja o de dispositivo **SOLO PARA REALIZAR PRUEBAS**.

    Programe el dispositivo con información del servicio de aprovisionamiento, para que pueda obtener los detalles de la conexión y la asignación de la aplicación IoT Central cuando está encendido.    

    **Más información** 
    *   Implementación de muestra para [RaspberryPi.](https://aka.ms/iotcentral-docs-Raspi-releases)  

    *   [Cliente de dispositivo de muestra en C.](https://github.com/Azure/azure-iot-sdk-c/blob/dps_symm_key/provisioning_client/devdoc/using_provisioning_client.md)

>[!NOTE]
>Use el **id. de dispositivo** como un registro CNAME al generar los certificados hoja para los dispositivos.

>[!NOTE]
>El **id. de dispositivo** debe estar en minúsculas. 
 
## <a name="connect-without-first-registering-devices"></a>Conéctese sin registrar primero los dispositivos
Uno de los escenarios clave que IoT Central habilita, es la posibilidad de que los OEM fabriquen dispositivos de forma masiva, generen las credenciales correspondientes y los configuren en fábrica sin tener que registrarlos primero en IoT Central. Una vez que se encienden esos dispositivos y se conectan a IoT Central, el operador aprueba esos dispositivos para que puedan conectarse a la aplicación IoT Central.

A continuación se muestra el flujo para conectar dispositivos con esta característica.

![Configuración de conexión](media\concepts-connectivity\device-connection-flow.PNG)


Siga los pasos según su elección del esquema de autenticación de dispositivos (X509/SAS).

1. **Configuración de conexión** 
    * **Certificados X509:** [agregue y compruebe el certificado raíz o intermedio](#connect-devices-using-x509-certificates) y úselo para generar los certificados del dispositivo en el siguiente paso.
    * **SAS:** copie la clave principal (esta clave es la clave del grupo SAS de la aplicación IoT Central) y úsela para generar las claves SAS del dispositivo en el siguiente paso. 
![Configuración de conexión de SAS](media\concepts-connectivity\connection-settings-sas.png)

1. **Generar credenciales del dispositivo** 
    *   **Certificados X509:** genere los certificados hoja para sus dispositivos mediante el certificado raíz o intermedio que agregó a esta aplicación. Asegúrese de utilizar el **id. de dispositivo** como registro CNAME en los certificados hoja y **<span style="color:Red">escríbalo en minúsculas</span>**. Aquí tiene una [herramienta de línea de comandos](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md ) para generar certificados hoja o de dispositivo solo para realizar pruebas.
    *   **SAS:** las claves de SAS del dispositivo se pueden generar con esta [herramienta de línea de comandos](https://www.npmjs.com/package/dps-keygen). Use la clave principal de SAS (clave de grupo SAS) del paso anterior. Asegúrese de que el id. del dispositivo **<span style="color:Red">esté en minúsculas</span>**.

        Utilice las siguientes instrucciones para generar la clave SAS del dispositivo.           

        ```
        npm i -g dps-keygen
        ```
    
        **Uso**
                        
        ```
        dps-keygen <Primary_Key(GroupSAS)> <device_id>
        ```

1. **Configuración del dispositivo** 
    
     Actualice el dispositivo con el  **id. de ámbito, el id. de dispositivo, la clave de certificado o SAS del dispositivo**, y encienda el dispositivo para conectarse a la aplicación IoT Central.

1. **Conecte el dispositivo a IoT Central:** una vez encendidos, los dispositivos se conectan a DPS o IoT Central para registrarse.

1. **Asocie el dispositivo a una plantilla:** el dispositivo conectado se mostrará en la opción **UnAssociated Devices** (Dispositivos no asociados) en **Device Explorer**. El estado de aprovisionamiento del dispositivos es **Registrado**. **Asocie** el dispositivo a la plantilla de dispositivos adecuada y apruebe que el dispositivo se conecte a la aplicación IoT Central. El dispositivo obtiene los detalles de conexión para la aplicación IoT Central, se conecta y comienza a enviar datos. El aprovisionamiento del dispositivo ahora está completo y el *estado de aprovisionamiento* pasa a **Aprovisionado**.

## <a name="device-provisioning-status"></a>Estado de aprovisionamiento de dispositivos
Hay una serie de pasos que es necesario dar cuando un dispositivo real está conectado a Azure IoT Central. 
1. **Registrado**: el dispositivo se **registra** primero, lo que significa que se creó en IoT Central y que tiene el id. de dispositivo que le corresponde.
Un dispositivo se registra cuando:  
    *   Se agrega un nuevo dispositivo real en **Explorer**.
    *   Se agrega un conjunto de dispositivos mediante la opción **Importar** en **Explorer**.
    *   Un dispositivo que no se haya registrado pero que se conecte con credenciales válidas y esté visible en los dispositivos **no asociados**. 

    En todos los casos anteriores, el *estado de aprovisionamiento* es **Registrado**.

1. **Aprovisionado**: el siguiente paso comienza cuando el dispositivo se conecta con credenciales válidas, e IoT Central completa el paso de aprovisionamiento (al crear el dispositivo en IoT Hub). A continuación, devuelve la cadena de conexión al dispositivo para conectarse y comenzar a enviar datos. El dispositivo con *estado de aprovisionamiento* pasa de **Registrado** a **Aprovisionado**.

1.  **Bloqueado**: el operador puede bloquear un dispositivo; recuerde que, una vez que el dispositivo está bloqueado, no puede enviar datos a IoT Central y tendrá que restablecerse. Los dispositivos que están bloqueados tienen el *estado de aprovisionamiento* de **Bloqueado**. El operador también puede desbloquear el dispositivo. Una vez desbloqueado, el *estado de aprovisionamiento* del dispositivo vuelve a su *estado anterior de aprovisionamiento* (Registrado o Aprovisionado). 

## <a name="getting-device-connection-string"></a>Obtener la cadena de conexión del dispositivo
Puede obtener la cadena de conexión del dispositivo de Iot Hub para Azure IoT Hub siguiendo los siguientes pasos: 
1. Obtenga detalles de conexión como el **id. de ámbito, el id. de dispositivo o la clave principal del dispositivo** desde la página de ese dispositivo (acceda a la página del dispositivo y haga clic en Conectar). 

    ![Detalles de conexión](media\concepts-connectivity\device-connect.PNG)

1. Obtenga la cadena de conexión del dispositivo utilizando la herramienta de línea de comandos que tiene a continuación.
    Siga las instrucciones a continuación para obtener la cadena de conexión del dispositivo.  

    ```cmd/sh
    npm i -g dps-keygen
    ```
    **Uso**

    Para crear una cadena de conexión, busque el elemento binario en la carpeta bin/.
    ```cmd/sh
    dps_cstr <scope_id> <device_id> <Primary Key(for device)>
    ```
    Obtenga más información sobre la [herramienta dps-keygen aquí.](https://www.npmjs.com/package/dps-keygen)

## <a name="sdk-support"></a>Compatibilidad con SDK

Los SDK de dispositivos Azure ofrecen la forma más fácil de implementar el código en los dispositivos que se conecta a la aplicación Azure IoT Central. En la actualidad, están disponibles los siguientes SDK:

- [SDK de Azure IoT para C](https://github.com/azure/azure-iot-sdk-c)
- [SDK de Azure IoT para Python](https://github.com/azure/azure-iot-sdk-python)
- [SDK de Azure IoT para Node.js](https://github.com/azure/azure-iot-sdk-node)
- [SDK de Azure IoT para Java](https://github.com/azure/azure-iot-sdk-java)
- [SDK de Azure IoT para .NET](https://github.com/azure/azure-iot-sdk-csharp)

Cada dispositivo se conecta mediante una cadena de conexión única que identifica al dispositivo. Un dispositivo solo puede conectarse al centro de IoT donde está registrado. Cuando se crea un dispositivo real en la aplicación Azure IoT Central, esta genera una cadena de conexión para que se utilice.

## <a name="sdk-features-and-iot-hub-connectivity"></a>Características del SDK y la conectividad de IoT Hub

Toda comunicación del dispositivo con la instancia de IoT Hub utiliza las siguientes opciones de conectividad:

- [Mensajería del dispositivo a la nube](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c)
- [Dispositivos gemelos](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-device-twins)

En la tabla siguiente se resume cómo las características de los dispositivos de Azure IoT Central se asignan a las características de IoT Hub:

| Azure IoT Central | Azure IoT Hub |
| ----------- | ------- |
| Medida: telemetría | Mensajería de un dispositivo a la nube |
| Propiedades del dispositivo | Propiedades notificadas de dispositivos gemelos |
| Configuración | Propiedades deseadas y notificadas de dispositivos gemelos |

Para más información sobre el uso de los SDK de dispositivos, consulte cualquiera de los siguientes artículos para ver código de ejemplo:

- [Conexión de un cliente de Node.js genérico a una aplicación de Azure IoT Central](howto-connect-nodejs.md)
- [Conexión de un dispositivo Raspberry Pi a su aplicación de Azure IoT Central](howto-connect-raspberry-pi-python.md)
- [Conexión de un dispositivo de DevKit a su aplicación de Azure IoT Central](howto-connect-devkit.md)


## <a name="protocols"></a>Protocolos

Los SDK de dispositivo admiten los siguientes protocolos de red para conectarse a un centro de IoT:

- MQTT
- AMQP
- HTTPS

Para obtener información sobre estos protocolos de diferencia y guía sobre cómo elegir uno, consulte [Elección de un protocolo de comunicación](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-protocols).

Si el dispositivo no puede usar ninguno de los protocolos admitidos, utilice Azure IoT Edge para realizar la conversión de protocolos. IoT Edge es compatible con otros escenarios de inteligencia perimetral para descargar el procesamiento al perímetro desde la aplicación Azure IoT Central.

## <a name="security"></a>Seguridad

Todos los datos intercambiados entre los dispositivos y la instancia de Azure IoT Central están cifrados. IoT Hub autentica todas las solicitudes de un dispositivo que se conecta a cualquiera de los puntos de conexión de IoT Hub orientados al dispositivo. Para evitar el intercambio de credenciales por la red, el dispositivo utiliza tokens firmados para autenticarse. Para más información, consulte [Control del acceso a IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security).

> [!NOTE]
> Actualmente, los dispositivos que se conectan a Azure IoT Central deben usar tokens SAS. Los certificados X.509 no son compatibles con los dispositivos que se conectan a Azure IoT Central.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido sobre la conectividad de dispositivos en Azure IoT Central, estos son los siguientes pasos sugeridos:

- [Preparación y conexión de un dispositivo DevKit](howto-connect-devkit.md)
- [Preparación y conexión de una instancia de Raspberry Pi](howto-connect-raspberry-pi-python.md)
- [Conexión de un cliente de Node.js genérico a una aplicación de Azure IoT Central](howto-connect-nodejs.md)
