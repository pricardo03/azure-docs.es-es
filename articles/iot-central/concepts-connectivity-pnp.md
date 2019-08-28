---
title: Conectividad de dispositivos en Azure IoT Central | Microsoft Docs
description: En este artículo se presentan conceptos clave relacionados con la conectividad de dispositivos en Azure IoT Central
author: dominicbetts
ms.author: dobett
ms.date: 04/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 26f05e60761af0b8f0db9508488f28613b82293f
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69879136"
---
# <a name="device-connectivity-in-azure-iot-central-preview-features"></a>Conectividad de dispositivos en Azure IoT Central (característica de versión preliminar)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

En este artículo se presentan conceptos clave relacionados con la conectividad de dispositivos en Microsoft Azure IoT Central.

Azure IoT Central usa Azure [IoT Device Provisioning Service (DPS)](https://docs.microsoft.com/azure/iot-dps/about-iot-dps) para administrar el registro y la conexión de todos los dispositivos.

El uso de DPS permite:

- Que IoT Central admite la incorporación y la conexión de dispositivos a escala.
- Que usted pueda generar credenciales de dispositivo y configurar los dispositivos sin conexión sin tener que registrarlos primero en la interfaz de usuario de IoT Central.
- Que los dispositivos se conecten con firmas de acceso compartido (SAS).
- Que los dispositivos se conecten con los certificados X.509 estándar del sector.
- Que usted pueda usar identificadores de dispositivo propios para registrar estos en IoT Central. Esto simplifica la integración con sistemas del área de operaciones existentes.
- Una manera sencilla y coherente de conectar dispositivos a IoT Central.

En este artículo se describen los siguientes casos de uso:

1. [Conexión rápida a un solo dispositivo con SAS](#connect-a-single-device)
1. [Conexión de dispositivos a escala con SAS](#connect-devices-at-scale-using-sas)
1. [Conexión de dispositivos a escala con certificados X.509](#connect-devices-using-x509-certificates). Este es el enfoque recomendado para entornos de producción.
1. [Conéctese sin registrar primero los dispositivos](#connect-without-registering-devices)
1. [Conexión de dispositivos con las características de IoT Plug and Play](howto-connect-pnp-device-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)

## <a name="connect-a-single-device"></a>Conectar un solo dispositivo

Este enfoque es útil cuando está experimentando con IoT Central o probado dispositivos. Puede usar la información de conexión del dispositivo desde la aplicación de IoT Central para generar la cadena de conexión para un dispositivo. Para los pasos detallados, consulte [Generación de una cadena de conexión de dispositivo para conectarse a una aplicación de Azure IoT Central](howto-generate-connection-string.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

## <a name="connect-devices-at-scale-using-sas"></a>Conexión de dispositivos a escala con SAS

Para conectar dispositivos con IoT Central a escala con SAS, es preciso registrarlos y configurarlos:

### <a name="register-devices-in-bulk"></a>Registro de dispositivos de forma masiva

Para registrar un gran número de dispositivos con la aplicación de IoT Central, use un archivo CSV para [importar identificadores y nombres de dispositivo](howto-manage-devices.md#import-devices).

Para recuperar la información de conexión de los dispositivos importados, [exporte un archivo CSV desde la aplicación de IoT Central](howto-manage-devices.md#export-devices).

> [!NOTE]
> Para aprender a conectar dispositivos sin registrarlos primero en IoT Central, consulte [Conectarse sin registro de dispositivos](#connect-without-registering-devices).

### <a name="set-up-your-devices"></a>Configuración de los dispositivos

Utilice la información de conexión del archivo de exportación del código del dispositivo para permitir que los dispositivos se conecten a la aplicación de IoT Central y envíen datos. Para más información sobre cómo conectarse a dispositivos, consulte los [Pasos siguientes](#next-steps).

## <a name="connect-devices-using-x509-certificates"></a>Conexión de dispositivos con certificados X.509

En un entorno de producción, se recomienda el uso de certificados X.509 como mecanismo de autenticación de dispositivos para IoT Central. Para más información consulte [Autenticación de dispositivos mediante certificados de entidades de certificación X.509](../iot-hub/iot-hub-x509ca-overview.md).

En los siguientes pasos se describe la conexión de dispositivos con IoT Central con certificados X.509:

1. En la aplicación de IoT Central, _agregue y compruebe el nivel intermedio o raíz del certificado X.509_ que usa para generar certificados de dispositivo:

    - Vaya a **Administración > Conexión del dispositivo > Certificados (X.509)** y agregue el certificado X.509 intermedio o raíz que usa para generar los certificados de dispositivo de hoja.

      ![Configuración de conexión](media/concepts-connectivity-pnp/connection-settings.png)

      Si sufre una infracción de seguridad o si el certificado principal está configurado para expirar, use el certificado secundario para reducir el tiempo de inactividad. Puede continuar con el aprovisionamiento de los dispositivos mediante el certificado secundario al actualizar el principal.

    - La verificación de la titularidad del certificado garantiza que la persona que carga el certificado está en posesión de la clave privada. Para verificar el certificado:
        - Seleccione el botón junto a **Código de verificación** para generar un código.
        - Cree un certificado de verificación X.509 con el código de verificación que generó en el paso anterior. Guarde el certificado como archivo .cer.
        - Cargue el certificado de verificación firmado y seleccione **Verificar**.

          ![Configuración de conexión](media/concepts-connectivity-pnp/verify-cert.png)

1. Use un archivo CSV para _importar y registrar dispositivos_ en la aplicación de IoT Central.

1. _Configuración de los dispositivos_. genere los certificados de hoja con el certificado raíz cargado. Use el **Id. de dispositivo** como valor CNAME en los certificados de hoja. El id. de dispositivo debe estar en minúsculas. Programe los dispositivos con la información del servicio de aprovisionamiento. Al activar un dispositivo la primera vez, recupera la información de conexión para la aplicación de IoT Central de DPS.

### <a name="further-reference"></a>Referencia adicional

- Implementación de muestra para [RaspberryPi.](https://aka.ms/iotcentral-docs-Raspi-releases)

- [Cliente de dispositivo de muestra en C.](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_provisioning_client.md)

### <a name="for-testing-purposes-only"></a>Solo para fines de prueba

Solo para las pruebas, puede generar los certificados de entidad de certificación y los certificados de dispositivo con estas utilidades.

- Si usa un dispositivo DevKit, esta [herramienta de la línea de comandos](https://aka.ms/iotcentral-docs-dicetool) genera un certificado de entidad de certificación que puede agregar a la aplicación de IoT Central para verificar los certificados.

- Use esta [herramienta de la línea de comandos](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md ) para:
  - Crear una cadena de certificados. Siga el paso 2 del artículo de GitHub.
  - Guardar los certificados como archivos .cer para cargarlos en la aplicación de IoT Central.
  - Usar el código de verificación de la aplicación de IoT Central para generar el certificado de verificación. Siga el paso 3 del artículo de GitHub.
  - Crear certificados de hoja para los dispositivos mediante sus identificadores de dispositivo como parámetro en la herramienta. Siga el paso 4 del artículo de GitHub.

## <a name="connect-without-registering-devices"></a>Conexión sin registro de los dispositivos

Un escenario clave con IoT Central consiste en la fabricación masiva por parte de las OEM de dispositivos que puedan conectarse a una aplicación de IoT Central sin tener que registrarlos primero. Un fabricante debe generar las credenciales adecuadas y configurar los dispositivos en la fábrica. Cuando un dispositivo se enciende por primera vez, se conecta automáticamente a una aplicación de IoT Central. Un operador de IoT Central debe aprobar el dispositivo para que empiece a enviar datos.

El siguiente diagrama describe este flujo:

![Configuración de conexión](media/concepts-connectivity-pnp/device-connection-flow1.png)

En los siguientes pasos se describe este proceso con más detalles. Los pasos varían ligeramente en función de si usa certificados X.509 o SAS para la autenticación de los dispositivos:

1. Configuración de la conexión:

    - **Certificados X.509:** [agregue y compruebe el certificado raíz o intermedio](#connect-devices-using-x509-certificates) y úselo para generar los certificados de dispositivo en el siguiente paso.
    - **SAS**: Copie el valor de la clave principal. Esta clave es la clave SAS de grupo para la aplicación de IoT Central. Utilícela para generar las claves SAS de dispositivo en el siguiente paso.
    ![Configuración de conexión de SAS](media/concepts-connectivity-pnp/connection-settings-sas.png)

1. Generación de las credenciales del dispositivo
    - **Certificados X.509:** genere los certificados de hoja para sus dispositivos mediante el certificado raíz o intermedio que agregó a la aplicación de IoT Central. Asegúrese de utilizar el **id. de dispositivo** como elemento CNAME en los certificados hoja. Únicamente con fines de prueba, use este [herramienta de la línea de comandos](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md ) para generar certificados de dispositivo.
    - **SAS**: use esta [herramienta de la línea de comandos](https://www.npmjs.com/package/dps-keygen) para generar las claves SAS del dispositivo. Use el grupo **Clave principal** del paso anterior. El id. de dispositivo debe estar en minúsculas.

      Para instalar la utilidad [generadora de claves](https://github.com/Azure/dps-keygen), ejecute el comando siguiente:

      ```cmd/sh
      npm i -g dps-keygen
      ```

      Para generar una clave de dispositivo a partir de la clave principal de SAS de grupo, ejecute el siguiente comando:

      ```cmd/sh
      dps-keygen -mk:<Primary_Key(GroupSAS)> -di:<device_id>
      ```

1. Para configurar los dispositivos, proyéctelos con el **Id. de ámbito**, el **Id. de dispositivo**, y el **certificado de dispositivo X.509** o la **clave SAS**.

1. Después, enciéndalos para conectarlos a la aplicación de IoT Central. Al encender un dispositivo, primero se conecta a DPS para recuperar la información de registro de IoT Central.

1. El dispositivo conectado aparece inicialmente como **No asociado** en la página **Dispositivos**. El estado de aprovisionamiento del dispositivos es **Registrado**. **Migre** el dispositivo a la plantilla de dispositivos adecuada y apruebe que el dispositivo se conecte a la aplicación de IoT Central. Entonces este podrá recuperar una cadena de conexión de IoT Hub y empezar a enviar datos. El aprovisionamiento de dispositivos estará ahora completo y el estado de aprovisionamiento pasará a **Aprovisionado**.

## <a name="connect-devices-with-iot-plug-and-play"></a>Conexión de dispositivos con IoT Plug and Play

Una de las características clave de IoT Plug and Play con IoT Central es la posibilidad de asociar plantillas de dispositivo automáticamente en la conexión del dispositivo. Junto con las credenciales del dispositivo, los dispositivos ya pueden enviar **CapabilityModelId** como parte de la llamada de registro del dispositivo e IoT Central detectará y asociará la plantilla de dispositivo. El proceso de detección sigue este orden:

1. Se asocia a la plantilla de dispositivo si ya se ha publicado en la aplicación de IoT Central.
1. Captura del repositorio público modelos de funcionalidad publicados y certificados.

A continuación se muestra el formato de la carga adicional que el dispositivo enviará durante la llamada de registro de DPS.

```javascript
'__iot:interfaces': {
              CapabilityModelId: <this is the URN for the capability model>
          }
```

Para más información sobre cómo conectar un dispositivo IoT Plug and Play, consulte [Conexión de un dispositivo IoT Plug and Play](howto-connect-pnp-device-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

> [!NOTE]
> Tenga en cuenta que la opción de aprobación automática debe estar habilitada para que los dispositivos se conecten automáticamente, detecten el modelo y comiencen a enviar datos.

## <a name="device-status"></a>Estado del dispositivo

Cuando un dispositivo real se conecta a la aplicación de IoT Central, su estado de dispositivo cambia como sigue:

1. Primero, el estado del dispositivo es **Registrado**. Este estado significa que el dispositivo se ha creado en IoT Central y tiene un identificador. El dispositivo está registrado cuando:
    - Se agrega un nuevo dispositivo real en la página **Dispositivos**.
    - Se agrega un conjunto de dispositivos mediante **Importar** en la página **Dispositivos**.

1. El estado del dispositivo cambia a **Aprovisionado** cuando el dispositivo conectado a la aplicación de IoT Central con credenciales válidas completa el paso de aprovisionamiento. En este paso, el dispositivo recupera una cadena de conexión de IoT Hub. Ahora, el dispositivo podrá conectarse a IoT Hub y empezar a enviar datos.

1. Los dispositivos los puede bloquear un operador. Cuando están bloqueados, no pueden enviar datos a la aplicación de IoT Central. Los dispositivos bloqueados tienen un estado de **Bloqueado**. Un operador debe restablecer el dispositivo para que pueda volver a enviar datos. Cuando un operador desbloquea un dispositivo, el estado vuelve a su valor anterior, **Registrado** o **Aprovisionado**.

1. El estado del dispositivo es **En espera de aprobación**, lo que significa que la opción de **aprobación automática** está deshabilitada y requiere que todos los dispositivos que se conectan a IoT Central sean aprobados explícitamente por un operador. Los dispositivos no registrados manualmente en la página **Dispositivos**, pero conectados con credenciales válidas, tendrán el estado del dispositivo **En espera de aprobación**. Los operadores pueden aprobar estos dispositivos desde la página **Dispositivos** mediante el botón **Aprobar**.

1. El estado del dispositivo es **No asociado**, lo que significa que los dispositivos que se conectan a IoT Central no tienen asociada una plantilla de dispositivo. Esto suele ocurrir en los escenarios siguientes:
    - Se agrega un conjunto de dispositivos mediante la opción **Importar** en la página **Dispositivos** sin especificar la plantilla de dispositivo.
    - Los dispositivos no se registran manualmente en la página **Dispositivos** conectados con credenciales válidas, pero sin especificar el identificador de plantilla durante el registro.  
El operador puede asociar un dispositivo a una plantilla desde la página **Dispositivos** mediante el botón **Migrar**.

## <a name="sdk-support"></a>Compatibilidad con SDK

La oferta de SDK de dispositivos de Azure es la manera más fácil de implementar el código del dispositivo. En la actualidad, están disponibles los siguientes SDK:

- [SDK de Azure IoT para C](https://github.com/azure/azure-iot-sdk-c)
- [SDK de Azure IoT para Python](https://github.com/azure/azure-iot-sdk-python)
- [SDK de Azure IoT para Node.js](https://github.com/azure/azure-iot-sdk-node)
- [SDK de Azure IoT para Java](https://github.com/azure/azure-iot-sdk-java)
- [SDK de Azure IoT para .NET](https://github.com/azure/azure-iot-sdk-csharp)

Cada dispositivo se conecta mediante una cadena de conexión única que identifica al dispositivo. Un dispositivo solo puede conectarse al centro de IoT donde está registrado. Cuando se crea un dispositivo real en la aplicación de Azure IoT Central, esta genera la información necesaria para construir una cadena de conexión con `dps-keygen`.

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

Para obtener más información sobre el uso de los SDK de dispositivo, consulte [Conexión de un dispositivo DevKit a una aplicación de Azure IoT Central ](howto-connect-devkit-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) para obtener código de ejemplo.

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

Ahora que ha aprendido sobre la conectividad de dispositivos en Azure IoT Central, estos son los siguientes pasos sugeridos:

- [Conexión de un dispositivo IoT Plug and Play](howto-connect-pnp-device-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
- [Preparación y conexión de un dispositivo DevKit](howto-connect-devkit-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
- [SDK de C: SDK de cliente de aprovisionamiento de dispositivos](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_provisioning_client.md)
