---
title: Conectividad de dispositivos en Azure IoT Central | Microsoft Docs
description: En este artículo se presentan conceptos clave relacionados con la conectividad de dispositivos en Azure IoT Central
author: dominicbetts
ms.author: dobett
ms.date: 04/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: 9e1e85d1ab1c5e7ce0cbd96c64137309c2e2916a
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59425974"
---
# <a name="device-connectivity-in-azure-iot-central"></a>Conectividad de dispositivos en Azure IoT Central

En este artículo se presentan conceptos clave relacionados con la conectividad de dispositivos en Microsoft Azure IoT Central.

Azure IoT Central usa el [servicio Azure IoT Hub Device Provisioning (DPS)](https://docs.microsoft.com/azure/iot-dps/about-iot-dps) para administrar todas las conexiones y registro de dispositivos.

Uso de DPS permite:

- IoT Central para admitir la incorporación y la conexión de dispositivos a escala.
- Dispositivo de generar las credenciales y configura los dispositivos sin conexión sin registrar los dispositivos a través de la interfaz de usuario de IoT Central.
- Los dispositivos se conecten mediante firmas de acceso comparten (SAS).
- Dispositivos se conecten con los certificados X.509 estándar del sector.
- Los usuarios pueden usar sus propios identificadores de dispositivo para registrar dispositivos en IoT Central. Usar sus propios identificadores de dispositivo simplifica la integración con sistemas de back-end existentes.
- Una forma única y coherente para conectar dispositivos a IoT Central.

En este artículo se describe los siguientes casos de uso de cuatro:

1. [Conectarse rápidamente un único dispositivo mediante SAS](#connect-a-single-device)
1. [Conectar dispositivos a escala mediante SAS](#connect-devices-at-scale-using-sas)
1. [Conectar dispositivos a escala mediante certificados X.509](#connect-devices-using-x509-certificates) este es el enfoque recomendado para entornos de producción.
1. [Conéctese sin registrar primero los dispositivos](#connect-without-registering-devices)

## <a name="connect-a-single-device"></a>Conectar un solo dispositivo

Este enfoque es útil cuando está experimentando con IoT Central o dispositivos de prueba. Puede usar la información de conexión del dispositivo desde la aplicación IoT Central para generar la cadena de conexión para un dispositivo. Para obtener instrucciones detalladas, consulte [cómo generar una cadena de conexión de dispositivo para conectarse a una aplicación de Azure IoT Central](howto-generate-connection-string.md).

## <a name="connect-devices-at-scale-using-sas"></a>Conectar dispositivos a escala mediante SAS

Para conectar dispositivos con IoT Central a escala mediante SAS, es preciso registrar y, a continuación, configurar los dispositivos:

### <a name="register-devices-in-bulk"></a>Registrar dispositivos en masa

Para registrar un gran número de dispositivos con la aplicación IoT Central, use un archivo CSV para [importar identificadores de dispositivo y los nombres de dispositivo](howto-manage-devices.md#import-devices).

Para recuperar la información de conexión para los dispositivos importados, [exportar un archivo CSV desde su aplicación IoT Central](howto-manage-devices.md#export-devices).

> [!NOTE]
> Para obtener información sobre cómo puede conectar dispositivos sin registrarlos primero en IoT Central, consulte [Connect sin el primer registro de dispositivos](#connect-without-registering-devices).

### <a name="set-up-your-devices"></a>Configurar los dispositivos

Utilice la información de conexión del archivo de exportación en el código del dispositivo para permitir que los dispositivos para conectarse y enviar datos a IoT para la aplicación IoT Central. Para obtener más información sobre cómo conectar dispositivos, consulte [pasos siguientes](#next-steps).

## <a name="connect-devices-using-x509-certificates"></a>Conectar dispositivos mediante certificados X.509

En un entorno de producción, mediante certificados X.509 es el mecanismo de autenticación de dispositivo recomendada para el centro de IoT. Para más información consulte [Autenticación de dispositivos mediante certificados de entidades de certificación X.509](../iot-hub/iot-hub-x509ca-overview.md).

Los pasos siguientes describen cómo conectar dispositivos con IoT Central mediante certificados X.509:

1. En la aplicación IoT Central, _agregar y comprobar el nivel intermedio o raíz del certificado X.509_ usa para generar certificados de dispositivo:

    - Vaya a **Administración > conexión del dispositivo > certificados (X.509)** y agregue X.509 certificado raíz o intermedio usa para generar los certificados de dispositivo de hoja.

      ![Configuración de conexión](media/concepts-connectivity/connection-settings.png)

      Si tiene una infracción de seguridad o el certificado principal está configurado para expirar, puede usar el certificado secundario para reducir el tiempo de inactividad. Puede seguir aprovisionar dispositivos mediante el certificado secundario al actualizar el certificado principal.

    - Comprobando la propiedad del certificado, se garantiza que el usuario de carga del certificado tiene una clave privada del certificado. Para comprobar el certificado:
        - Seleccione el botón junto a **código de verificación** para generar un código.
        - Cree un certificado de verificación X.509 con el código de verificación que generó en el paso anterior. Guarde el certificado como un archivo .cer.
        - Cargue el certificado de verificación firmado y seleccione **compruebe**.

          ![Configuración de conexión](media/concepts-connectivity/verify-cert.png)

1. Usar un archivo CSV a _importar y registrar dispositivos_ en la aplicación IoT Central.

1. _Configurar los dispositivos._ genere los certificados de hoja con el certificado raíz cargado. Use la **Id. de dispositivo** como el valor CNAME en los certificados de hoja. El identificador de dispositivo debe estar todo en minúsculas. A continuación, los dispositivos con información de servicio de aprovisionamiento del programa. Cuando un dispositivo está activado para el primero, recupera su información de conexión para la aplicación IoT Central de DP.

### <a name="further-reference"></a>Referencia adicional

- Implementación de muestra para [RaspberryPi.](https://aka.ms/iotcentral-docs-Raspi-releases)

- [Cliente de dispositivo de muestra en C.](https://github.com/Azure/azure-iot-sdk-c/blob/dps_symm_key/provisioning_client/devdoc/using_provisioning_client.md)

### <a name="for-testing-purposes-only"></a>Solo con fines de prueba

Solo para pruebas, puede usar estas utilidades para generar certificados de CA y los certificados de dispositivo.

- Si usa un dispositivo DevKit, esto [herramienta de línea de comandos](https://aka.ms/iotcentral-docs-dicetool) genera un certificado de CA que puede agregar a la aplicación IoT Central para comprobar los certificados.

- Use esta [herramienta de línea de comandos](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md ) para:
  - Crear una cadena de certificados. Siga el paso 2 en el artículo de GitHub.
  - Guarde los certificados como archivos .cer para cargar la aplicación IoT Central.
  - Utilice el código de verificación de la aplicación IoT Central para generar el certificado de verificación. Siga el paso 3 en el artículo de GitHub.
  - Crear certificados de hoja para los dispositivos mediante sus identificadores de dispositivo como un parámetro a la herramienta. Siga el paso 4 en el artículo de GitHub.

## <a name="connect-without-registering-devices"></a>Conectarse sin registro de dispositivos

Es un escenario clave IoT Central permite para que los OEM fabricar masiva de dispositivos que pueden conectarse a una aplicación IoT Central sin tener que se está registrando. Un fabricante debe generar las credenciales adecuadas y configurar los dispositivos en la fábrica. Cuando un dispositivo se enciende por primera vez, se conecta automáticamente a una aplicación IoT Central. Un operador de IoT Central debe aprobar el dispositivo antes de que pueda stat envío de datos.

El siguiente diagrama muestra este flujo:

![Configuración de conexión](media/concepts-connectivity/device-connection-flow.png)

Los pasos siguientes describen este proceso con más detalle. Los pasos varían ligeramente en función de si usa certificados X.509 o SAS para la autenticación de dispositivo:

1. Configurar la conexión:

    - **Certificados X.509:** [Agregar y comprobar el certificado raíz o intermedia](#connect-devices-using-x509-certificates) y usarlo para generar los certificados de dispositivo en el paso siguiente.
    - **SAS**: Copie la clave principal. Esta clave es la clave SAS de grupo para la aplicación IoT Central. Utilice la clave para generar las claves SAS de dispositivo en el paso siguiente.
    ![Configuración de conexión de SAS](media/concepts-connectivity/connection-settings-sas.png)

1. Generar las credenciales del dispositivo
    - **Certificados X.509:** Generar los certificados de hoja para los dispositivos mediante el certificado raíz o intermedio que agrega a la aplicación IoT Central. Asegúrese de utilizar la letra minúscula **Id. de dispositivo** como el registro CNAME en los certificados de hoja. Para fines de prueba, use esta opción sólo [herramienta de línea de comandos](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md ) para generar certificados de dispositivo.
    - **SAS**: Use esta [herramienta de línea de comandos](https://www.npmjs.com/package/dps-keygen) para generar las claves SAS del dispositivo. Use el grupo **Primary Key** del paso anterior. El identificador de dispositivo debe estar en minúscula.

      Para instalar el [clave de la utilidad de generador](https://github.com/Azure/dps-keygen), ejecute el siguiente comando:

      ```cmd/sh
      npm i -g dps-keygen
      ```

      Para generar una clave de dispositivo de la clave principal de SAS de grupo, ejecute el siguiente comando:

      ```cmd/sh
      dps-keygen -mk:<Primary_Key(GroupSAS)> -di:<device_id>
      ```

1. Para configurar los dispositivos, cada dispositivo con flash el **Id. de ámbito**, **Id. de dispositivo**, y **certificado de dispositivo X.509** o **clave SAS**.

1. A continuación, encienda el dispositivo para conectarse a la aplicación IoT Central. Cuando se cambia en un dispositivo, primero se conecta a DPS para recuperar la información de registro IoT Central.

1. El dispositivo conectado aparece inicialmente como un **dispositivo desasociado** en el **Device Explorer** página. El estado de aprovisionamiento del dispositivos es **Registrado**. **Asociar** el dispositivo a la plantilla de dispositivo adecuado y aprobar el dispositivo para conectarse a la aplicación IoT Central. El dispositivo, a continuación, puede recuperar una cadena de conexión de IoT Hub y empezar a enviar datos. Aprovisionamiento de dispositivos ahora está completado y el estado de aprovisionamiento es ahora **aprovisionado**.

## <a name="provisioning-status"></a>Estado de aprovisionamiento

Cuando un dispositivo real se conecta a la aplicación IoT Central, sus cambios de estado de aprovisionamiento como sigue:

1. El estado de aprovisionamiento de dispositivos es la primera **registrado**. Este estado significa que el dispositivo se crea en IoT Central y tiene un identificador de dispositivo. Se registra un dispositivo cuando:
    - Se agrega un nuevo dispositivo real en el **Device Explorer** página.
    - Se agrega un conjunto de dispositivos mediante **importación** en el **Device Explorer** página.
    - Un dispositivo no se ha registrado manualmente en el **Device Explorer** página, pero con unas credenciales válidas y es visible como un **no asociada** dispositivo en el **Device Explorer**página.

1. Cambia el estado de aprovisionamiento de dispositivos a **aprovisionado** cuando el dispositivo conectado a su aplicación IoT Central con credenciales válidas completa el paso de aprovisionamiento. En este paso, el dispositivo recupera una cadena de conexión de IoT Hub. Ahora, el dispositivo puede conectarse a IoT Hub y empezar a enviar datos.

1. Un operador puede bloquear un dispositivo. Cuando un dispositivo está bloqueado, no puede enviar datos a la aplicación IoT Central. Dispositivos bloqueados tienen un estado de aprovisionamiento de **bloqueado**. Un operador debe restablecer el dispositivo antes de que puede reanudar el envío de datos. Cuando un dispositivo que se devuelve el estado de aprovisionamiento a su valor anterior, desbloquea un operador **registrado** o **aprovisionado**.

## <a name="sdk-support"></a>Compatibilidad con SDK

La oferta de SDK de dispositivos de Azure la manera más fácil para implemente el código del dispositivo. En la actualidad, están disponibles los siguientes SDK:

- [SDK de Azure IoT para C](https://github.com/azure/azure-iot-sdk-c)
- [SDK de Azure IoT para Python](https://github.com/azure/azure-iot-sdk-python)
- [SDK de Azure IoT para Node.js](https://github.com/azure/azure-iot-sdk-node)
- [SDK de Azure IoT para Java](https://github.com/azure/azure-iot-sdk-java)
- [SDK de Azure IoT para .NET](https://github.com/azure/azure-iot-sdk-csharp)

Cada dispositivo se conecta mediante una cadena de conexión única que identifica al dispositivo. Solo se puede conectar un dispositivo a IoT hub en que está registrado. Cuando se crea un dispositivo real en la aplicación de Azure IoT Central, la aplicación genera la información que necesita construir una cadena de conexión mediante `dps-keygen`.

### <a name="sdk-features-and-iot-hub-connectivity"></a>Características del SDK y la conectividad de IoT Hub

Toda comunicación del dispositivo con la instancia de IoT Hub utiliza las siguientes opciones de conectividad:

- [Mensajería del dispositivo a la nube](../iot-hub/iot-hub-devguide-messages-d2c.md)
- [Dispositivos gemelos](../iot-hub/iot-hub-devguide-device-twins.md)

En la tabla siguiente se resume cómo las características de los dispositivos de Azure IoT Central se asignan a las características de IoT Hub:

| Azure IoT Central | Azure IoT Hub |
| ----------- | ------- |
| Medida: Telemetría | Mensajería de un dispositivo a la nube |
| Propiedades del dispositivo | Propiedades notificadas de dispositivos gemelos |
| Configuración | Propiedades deseadas y notificadas de dispositivos gemelos |

Para más información sobre el uso de los SDK de dispositivos, consulte cualquiera de los siguientes artículos para ver código de ejemplo:

- [Conexión de un cliente de Node.js genérico a una aplicación de Azure IoT Central](howto-connect-nodejs.md)
- [Conexión de un dispositivo Raspberry Pi a su aplicación de Azure IoT Central](howto-connect-raspberry-pi-python.md)
- [Conexión de un dispositivo de DevKit a su aplicación de Azure IoT Central](howto-connect-devkit.md)

### <a name="protocols"></a>Protocolos

Los SDK de dispositivo admiten los siguientes protocolos de red para conectarse a un centro de IoT:

- MQTT
- AMQP
- HTTPS

Para obtener información sobre estos protocolos de diferencia y guía sobre cómo elegir uno, consulte [Elección de un protocolo de comunicación](../iot-hub/iot-hub-devguide-protocols.md).

Si el dispositivo no puede usar ninguno de los protocolos admitidos, utilice Azure IoT Edge para realizar la conversión de protocolos. IoT Edge es compatible con otros escenarios de inteligencia perimetral para descargar el procesamiento al perímetro desde la aplicación Azure IoT Central.

## <a name="security"></a>Seguridad

Todos los datos intercambiados entre los dispositivos y la instancia de Azure IoT Central están cifrados. IoT Hub autentica todas las solicitudes de un dispositivo que se conecta a cualquiera de los puntos de conexión de IoT Hub orientados al dispositivo. Para evitar el intercambio de credenciales por la red, el dispositivo utiliza tokens firmados para autenticarse. Para más información, consulte [Control del acceso a IoT Hub](../iot-hub/iot-hub-devguide-security.md).

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido acerca de la conectividad de dispositivos en Azure IoT Central, estos son los siguientes pasos sugeridos:

- [Preparación y conexión de un dispositivo DevKit](howto-connect-devkit.md)
- [Preparación y conexión de una instancia de Raspberry Pi](howto-connect-raspberry-pi-python.md)
- [Conexión de un cliente de Node.js genérico a una aplicación de Azure IoT Central](howto-connect-nodejs.md)
- [SDK DE C: Aprovisionamiento de SDK de cliente de dispositivo](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_provisioning_client.md)
