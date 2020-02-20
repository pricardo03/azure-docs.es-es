---
title: 'Instalación de certificados en el dispositivo: Azure IoT Edge | Microsoft Docs'
description: Cree certificados de prueba y obtenga información sobre cómo instalarlos en un dispositivo Azure IoT Edge para preparar la implementación de producción.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 02/11/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: d9f715a6ab43206d02107f6335e9b4c0bb4266e0
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77471013"
---
# <a name="install-production-certificates-on-an-iot-edge-device"></a>Instalación de certificados de producción en un dispositivo IoT Edge

Todos los dispositivos IoT Edge usan certificados para crear conexiones seguras entre el entorno de ejecución y los módulos que se ejecutan en el dispositivo.
Los dispositivos IoT Edge que funcionan como puertas de enlace usan estos mismos certificados para conectarse también a sus dispositivos de nivel inferior.

La primera vez que instala IoT Edge y aprovisiona el dispositivo, el dispositivo se configura con certificados temporales para que pueda probar el servicio.
Estos certificados temporales expiran en 90 días, o bien se puede reiniciar el equipo para restablecerlos.
Una vez que esté listo para migrar los dispositivos a un escenario de producción, o si quiere crear un escenario de puerta de enlace, tendrá que proporcionar certificados propios.
En este artículo se muestran los pasos para instalar certificados en los dispositivos IoT Edge.

Para obtener más información sobre los diferentes tipos de certificados y sus roles en un escenario de IoT Edge, vea [Información de uso de los certificados de Azure IoT Edge](iot-edge-certs.md).

>[!NOTE]
>El término "entidad de certificación raíz" que se usa en este artículo hace referencia al certificado público de la entidad de nivel superior de la cadena de certificados de la solución de IoT. No es necesario usar la raíz del certificado de una entidad de certificación sindicada o la raíz de la entidad de certificación de la organización. En muchos casos, se trata realmente de un certificado público intermedio de la entidad de certificación.

## <a name="prerequisites"></a>Prerrequisitos

* Un dispositivo IoT Edge que se ejecute en [Windows](how-to-install-iot-edge-windows.md) o [Linux](how-to-install-iot-edge-linux.md).
* Tener un certificado de entidad de certificación (CA) raíz, ya sea autofirmado o comprado a través de una entidad de certificación comercial de confianza como Baltimore, Verisign, DigiCert o GlobalSign.

Si todavía no tiene una entidad de certificación raíz pero quiere probar características de IoT Edge que requieren certificados de producción (por ejemplo, escenarios de puerta de enlace), puede [crear certificados de demostración para probar las características del dispositivo IoT Edge](how-to-create-test-certificates.md).

## <a name="create-production-certificates"></a>Creación de certificados de producción

Debe usar una entidad de certificación propia para crear los archivos siguientes:

* Entidad de certificación raíz
* Certificado de entidad de certificación de dispositivo
* Clave privada de entidad de certificación del dispositivo

En este artículo, lo que se denomina *entidad de certificación raíz* no es la entidad de certificación de nivel superior de una organización. Es la entidad de certificación de nivel superior para el escenario de IoT Edge, que el módulo de IoT Edge Hub, los módulos de usuario y los dispositivos de nivel inferior usan para establecer la confianza entre sí.

Para ver un ejemplo de estos certificados, revise los scripts que crean los certificados de demostración en [Administración de certificados de entidad de certificación de prueba para ejemplos y tutoriales](https://github.com/Azure/iotedge/tree/master/tools/CACertificates).

## <a name="install-certificates-on-the-device"></a>Instalación de certificados en el dispositivo

Instale la cadena de certificados en el dispositivo IoT Edge y configure el entorno de ejecución de IoT Edge para que haga referencia a los nuevos certificados.

Por ejemplo, si ha usado los scripts de ejemplo para [Crear certificados de demostración](how-to-create-test-certificates.md), los tres archivos que necesita copiar en el dispositivo IoT Edge son los siguientes:

* Certificado de entidad de certificación del dispositivo: `<WRKDIR>\certs\iot-edge-device-MyEdgeDeviceCA-full-chain.cert.pem`
* Clave privada de entidad de certificación del dispositivo: `<WRKDIR>\private\iot-edge-device-MyEdgeDeviceCA.key.pem`
* Entidad de certificación raíz: `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

1. Copie los tres archivos de certificado y clave en el dispositivo IoT Edge.

   Puede usar un servicio como [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) o una función como [Protocolo de copia segura](https://www.ssh.com/ssh/scp/) para mover los archivos de certificado.  Si ha generado los certificados en el propio dispositivo IoT Edge, puede omitir este paso y usar la ruta de acceso al directorio de trabajo.

2. Abra el archivo de configuración del demonio de seguridad de IoT Edge.

   * Windows: `C:\ProgramData\iotedge\config.yaml`
   * Linux: `/etc/iotedge/config.yaml`

3. Establezca las propiedades de **certificado** del archivo config.yaml en la ruta de acceso completa a los archivos de certificado y de clave del dispositivo IoT Edge. Elimine el carácter `#` antes de las propiedades de certificado para quitar la marca de comentario de las cuatro líneas. Asegúrese de que la línea **certificates:** no tiene ningún espacio en blanco delante y de que los elementos anidados muestran una sangría de dos espacios. Por ejemplo:

   * Windows:

      ```yaml
      certificates:
        device_ca_cert: "c:\\<path>\\device-ca.cert.pem"
        device_ca_pk: "c:\\<path>\\device-ca.key.pem"
        trusted_ca_certs: "c:\\<path>\\root-ca.root.ca.cert.pem"
      ```

   * Linux:

      ```yaml
      certificates:
        device_ca_cert: "<path>/device-ca.cert.pem"
        device_ca_pk: "<path>/device-ca.key.pem"
        trusted_ca_certs: "<path>/root-ca.root.ca.cert.pem"
      ```

4. En los dispositivos Linux, asegúrese de que el usuario **iotedge** tiene permisos de lectura para el directorio que contiene los certificados.

5. Si anteriormente ha usado cualquier otro certificado para IoT Edge en el dispositivo, elimine los archivos de los dos directorios siguientes antes de iniciar o reiniciar IoT Edge:

   * Windows: `C:\ProgramData\iotedge\hsm\certs` y `C:\ProgramData\iotedge\hsm\cert_keys`

   * Linux: `/var/lib/iotedge/hsm/certs` y `/var/lib/iotedge/hsm/cert_keys`

## <a name="next-steps"></a>Pasos siguientes

La instalación de certificados en un dispositivo IoT Edge es un paso necesario antes de implementar la solución en producción. Obtenga más información sobre cómo [preparar la implementación de la solución de IoT Edge en producción](production-checklist.md).
