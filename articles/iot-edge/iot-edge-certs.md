---
title: Información sobre los certificados de Azure IoT Edge | Microsoft Docs
description: Más información sobre los certificados de Azure IoT Edge y cómo se usan.
author: stevebus
manager: timlt
ms.author: stevebus
ms.date: 09/13/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: af1a14f87eaf20a7f49a8fc6112c4f3c7595f6bd
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "50914383"
---
# <a name="azure-iot-edge-certificate-usage-detail"></a>Información de uso de los certificados de Azure IoT Edge

Los certificados de IoT Edge se usan para que los módulos y los dispositivos IoT de nivel inferior comprueben la identidad y la legitimidad del módulo del entorno de ejecución del [centro de IoT Edge](iot-edge-runtime.md#iot-edge-hub) al que se conectan. Estas comprobaciones permiten una conexión TLS (seguridad de la capa de transporte) segura entre el entorno de ejecución, los módulos y los dispositivos IoT. Al igual que el propio IoT Hub, IoT Edge requiere una conexión segura y cifrada de los dispositivos IoT (u hoja) de nivel inferior y módulos de IoT Edge. Para establecer una conexión TLS segura, el módulo del centro de IoT Edge presenta una cadena de certificados de servidor para conectar a los clientes con el fin de comprobar su identidad.

Para evitar cualquier confusión sobre el uso de certificados de IoT Edge, en este artículo se explica cómo los certificados de IoT Edge pueden funcionar en escenarios a escala, así como en escenarios de desarrollo y pruebas. Aunque los scripts son diferentes (PowerShell frente a Bash), los conceptos son los mismos entre Windows y Linux.

## <a name="iot-edge-certificates"></a>Certificados de IoT Edge

En la mayoría de los casos, los fabricantes son entidades independientes de los usuarios finales de un dispositivo de Edge. A veces, la única relación entre el fabricante y el operador es la compra de un dispositivo de Edge comercialmente disponible. En otras ocasiones, el fabricante que trabaja por contrato para crear un dispositivo de Edge en nombre del operador. El diseño del certificado de IoT Edge intenta tener en cuenta ambos escenarios.

La ilustración siguiente muestra el uso de certificados de IoT Edge. Puede haber cero, uno o varios certificados de firma intermedios entre el certificado de CA raíz y el certificado de CA del dispositivo. En función del número de entidades implicadas, se muestra un caso a continuación.

![Diagrama de relaciones típicas de certificado](./media/iot-edge-certs/edgeCerts-general.png)

### <a name="certificate-authority"></a>Entidad de certificación

La entidad de certificación, o "CA" para abreviar, es una entidad que emite certificados digitales. Una entidad de certificación actúa como un tercero de confianza entre el propietario y el receptor del certificado. Un certificado digital certifica la propiedad de una clave pública por parte del receptor del certificado. La cadena de certificados de confianza funciona mediante la emisión de un certificado raíz inicialmente, que es la base de confianza de todos los certificados emitidos por la autoridad. Después, el propietario puede usar el certificado raíz para emitir más certificados intermedios (certificados "hoja").

### <a name="root-ca-certificate"></a>Certificado de entidad de certificación raíz

Un certificado de CA raíz es la raíz de confianza de todo el proceso. En escenarios de producción, normalmente será un certificado de CA adquirido a través de una entidad de certificación comercial de confianza, como Baltimore, Verisign, DigiCert, etc. Para tener un control completo sobre los dispositivos que se conectan a los dispositivos de Edge, es posible usar una entidad de certificación de nivel corporativo. En cualquier caso, la cadena de certificados completa del centro de IoT Edge se distribuye en él, por lo que los dispositivos hoja IoT deben confiar en el certificado raíz. Puede almacenar el certificado de CA raíz en el almacén de la entidad de certificación raíz de confianza o proporcionar los detalles del certificado de CA raíz en el código cuando el cliente de IoT se conecte en IoT Edge.

### <a name="intermediate-certificates"></a>Certificados intermedios

En un proceso de fabricación típico para crear dispositivos seguros, rara vez se usan certificados de CA raíz directamente, principalmente debido al riesgo de pérdida o exposición. Por lo general, se crean uno o varios certificados de CA intermedios y el certificado de CA raíz los firma digitalmente. Puede haber solo uno o bien una cadena de estos certificados intermedios. Los escenarios que requieren una cadena de certificados intermedios incluyen:

* Una jerarquía de departamentos dentro de un fabricante.

* Varias empresas implicadas en serie en la producción de un dispositivo.

* Un cliente que compra un certificado de CA raíz y deriva un certificado de firma para que el fabricante firme los dispositivos que realizan en nombre de ese cliente.

En cualquier caso, el fabricante utiliza un certificado de CA intermedio al final de esta cadena para firmar el certificado de CA de dispositivo colocado en el dispositivo final. Por lo general, estos certificados intermedios están estrechamente protegidos en la planta industrial. Se someten a procesos estrictos, tanto físicos como electrónicos, para su uso.

### <a name="device-ca-certificate"></a>Certificado de entidad de certificación de dispositivo

El certificado de CA de dispositivo se genera a partir del certificado de CA intermedio final y obtiene la firma de este en el proceso. Este certificado se instala en el dispositivo de Edge, preferiblemente en un almacenamiento seguro, como un módulo de seguridad de hardware (HSM). Además, un certificado de CA de dispositivo identifica un dispositivo IoT Edge. En el caso de IoT Edge, el certificado de CA de dispositivo es capaz de emitir otros certificados. Por ejemplo, el certificado de CA de dispositivo emite certificados de dispositivo hoja que se usan para autenticar los dispositivos en el [servicio Azure IoT Device Provisioning](..\iot-dps\about-iot-dps.md).

### <a name="iot-edge-workload-ca"></a>Entidad de certificación de carga de trabajo de IoT Edge

Este certificado, el primero en el lado del "operador" del proceso, se genera mediante el [Administrador de seguridad de IoT Edge](iot-edge-security-manager.md) cuando IoT Edge se inicia por primera vez. El certificado se genera a partir del "certificado de CA de dispositivo" y obtiene la firma de este. Este certificado, que es simplemente otro certificado de firma intermedio, se usa para generar y firmar otros certificados utilizados por el entorno de ejecución de IoT Edge. Hoy en día, es principalmente el certificado de servidor del centro de IoT Edge que se describe en la sección siguiente, pero, en el futuro, puede incluir otros certificados para autenticar los componentes de IoT Edge.

### <a name="edge-hub-server-certificate"></a>Certificado de servidor del centro de IoT Edge

El certificado de servidor del centro de IoT Edge es el certificado real presente en los dispositivos hoja y en los módulos para la verificación de identidad durante el establecimiento de la conexión TLS que requiere IoT Edge. Este certificado presenta la cadena completa de certificados de firma que se utilizó para generarlo hasta el certificado de CA raíz, en el que debe confiar el dispositivo hoja IoT. Cuando lo genera el administrador de seguridad de IoT Edge, el nombre común (CN) de este certificado de IoT Edge se establece en el nombre proporcionado por la propiedad "hostname" en el archivo config.yaml después de la conversión a minúsculas. Se trata de un origen común de confusión con IoT Edge.

## <a name="production-implications"></a>Implicaciones de producción

Una pregunta razonable podría ser "¿Por qué necesita IoT Edge el certificado adicional 'entidad de certificación de la carga de trabajo'? ¿No se podría usar el certificado de CA de dispositivo para generar directamente el certificado de servidor del centro de IoT Edge?". Técnicamente, se podría. Sin embargo, el propósito de este certificado intermedio de "carga de trabajo" consiste en dividir las preocupaciones entre el fabricante del dispositivo y el operador del dispositivo. Imagine un escenario donde un dispositivo IoT Edge se vende o se transfiere de un cliente a otro. Es probable que quiera que el certificado de CA de dispositivo proporcionado por el fabricante sea inmutable. Sin embargo, los certificados de "carga de trabajo" específicos para el funcionamiento del dispositivo deben borrarse y volver a crearse para la nueva implementación.

Dado que los procesos del fabricante y del operador son independientes, hay algunas implicaciones que deben tenerse en cuenta con relación a la implementación de IoT Edge para dispositivos de producción.

* Con cualquier proceso basado en certificados, el certificado de CA raíz y todos los certificados de CA intermedios deben protegerse y supervisarse durante todo el proceso de implementación de un dispositivo IoT Edge. El fabricante del dispositivo IoT Edge debe tener procesos seguros en lugar de un uso y almacenamiento adecuados de sus certificados intermedios. Además, el certificado de CA de dispositivo se debe mantener en un almacenamiento tan seguro como sea posible en el dispositivo, preferiblemente un módulo de seguridad de hardware.

* Puesto que el certificado de servidor del centro de IoT Edge lo presentará el centro de IoT Edge para los módulos y dispositivos del cliente de conexión, el nombre común del certificado de CA de dispositivo **no debe ser** el mismo que el "nombre de host" que se usará en config.yaml en el dispositivo IoT Edge. El nombre utilizado por los clientes para conectarse a IoT Edge (por ejemplo, mediante el parámetro GatewayHostName de la cadena de conexión o el comando CONNECT en MQTT) **no puede ser** el mismo que el nombre común usado en el certificado de CA de dispositivo. Esto se debe a que el centro de IoT Edge presenta su cadena de certificados completa para la comprobación por parte de los clientes. Si el certificado de servidor del centro de IoT Edge y el certificado de CA de dispositivo tienen el mismo nombre común, permanecerá en un bucle de comprobación y se invalidará el certificado.

* Dado que el demonio de seguridad de IoT Edge usa el certificado de CA de dispositivo para generar los certificados de IoT Edge finales, debe ser un certificado de firma, lo que significa que tiene funcionalidades de firma de certificados. Para convertir esto en un certificado de CA de firma, al aplicar básicamente "restricciones básicas V3 CA:True" al certificado de CA de dispositivo, se configurarán automáticamente las propiedades de uso de clave necesarias.

>[!Tip]
> Si ya ha realizado la configuración de IoT Edge como una puerta de enlace transparente en un escenario de desarrollo y pruebas con nuestros "scripts de comodidad" (consulte la sección siguiente) y ha usado el mismo nombre de host al crear el certificado de entidad de certificación de dispositivo que para el nombre de host en config.yaml, tal vez se pregunte por qué ha funcionado. En un esfuerzo por simplificar la experiencia del desarrollador, los scripts de comodidad anexan ".ca" al final del nombre que pasó al script. Por lo tanto, por ejemplo, si ha usado "mygateway" para el nombre del dispositivo en los scripts y el nombre de host en config.yaml, el primero se convertirá en mygateway.ca antes de que se use como nombre común para el certificado de entidad de certificación de dispositivo.

## <a name="devtest-implications"></a>Implicaciones de desarrollo y pruebas

Para facilitar los escenarios de desarrollo y pruebas, Microsoft proporciona un conjunto de [scripts de comodidad](https://github.com/Azure/azure-iot-sdk-c/tree/master/tools/CACertificates) para generar certificados que no sean de producción adecuados para IoT Edge en el escenario de puerta de enlace transparente. Para obtener ejemplos de cómo funcionan los scripts, consulte [Configuración de un dispositivo IoT Edge para que actúe como puerta de enlace transparente](how-to-create-transparent-gateway.md).

Estos scripts generan certificados que siguen la estructura de la cadena de certificados que se explica en este artículo. Los siguientes comandos generan el "certificado de CA raíz" y un único "certificado de CA intermedio".

```bash
./certGen.sh create_root_and_intermediate 
```

```Powershell
New-CACertsCertChain rsa 
```

Del mismo modo, estos comandos generan el "certificado de entidad de certificación de dispositivo".

```bash
./certGen.sh create_edge_device_certificate "<gateway device name>"
```

```Powershell
New-CACertsEdgeDevice "<gateway device name>"
```

* El **\<nombre de dispositivo de puerta de enlace\>** que se pase a esos scripts **no debe** ser el mismo que el parámetro "hostname" en config.yaml. Como se mencionó anteriormente, los scripts le ayudan a evitar cualquier problema mediante el anexo de una cadena ".ca" al **\<nombre de dispositivo de puerta de enlace\>** para impedir que se produzca un conflicto de nombres en caso de que el usuario configure Edge con el mismo nombre en ambos lugares. Sin embargo, es conveniente evitar usar el mismo nombre.

>[!Tip]
> Para conectar las aplicaciones y los dispositivos "hoja" IoT del dispositivo que usan nuestro SDK de dispositivo IoT mediante IoT Edge, debe agregar el parámetro opcional GatewayHostName al final de la cadena de conexión del dispositivo. Cuando se genera el certificado de servidor del centro de IoT Edge, se basa en una versión en minúsculas del nombre de host de config.yaml, por lo que, para que los nombres coincidan y la comprobación del certificado TLS se realice correctamente, debe escribir el parámetro GatewayHostName en minúsculas.

## <a name="example-of-iot-edge-certificate-hierarchy"></a>Ejemplo de jerarquía de certificados de IoT Edge

Para ilustrar un ejemplo de la ruta de acceso de este certificado, la siguiente captura de pantalla es de un dispositivo IoT Edge en funcionamiento configurado como puerta de enlace transparente. OpenSSL se usa para conectar con el centro de IoT Edge, validar y volcar los certificados.

![Captura de pantalla de la jerarquía de certificados en cada nivel](./media/iot-edge-certs/iotedge-cert-chain.png)

Puede ver la jerarquía de profundidad del certificado representada en la captura de pantalla:

| Certificado de entidad de certificación raíz         | Solo prueba de certificado de entidad de certificación de Azure IoT Hub                                                                           |
|-----------------------------|-----------------------------------------------------------------------------------------------------------|
| Certificado de entidad de certificación intermedio | Solo prueba de certificado intermedio de Azure IoT Hub                                                                 |
| Certificado de entidad de certificación de dispositivo       | iotgateway.ca ("iotgateway" se pasó como < nombre de host de puerta de enlace > para los scripts de comodidad)      |
| Certificado de entidad de certificación de carga de trabajo     | Entidad de certificación de carga de trabajo de IoT Edge                                                                                       |
| Certificado de servidor del centro de IoT Edge | iotedgegw.local (coincide con el "hostname" de config.yaml)                                                |

## <a name="next-steps"></a>Pasos siguientes

[Información sobre los módulos de Azure IoT Edge](iot-edge-modules.md)

[Configuración de un dispositivo IoT Edge para que actúe como puerta de enlace transparente](how-to-create-transparent-gateway.md)