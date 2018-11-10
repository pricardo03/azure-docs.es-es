---
title: Esta guía de inicio rápido le muestra cómo inscribir dispositivos X.509 en el servicio Azure Device Provisioning con Node.js | Microsoft Docs
description: Esta guía de inicio rápido usa inscripciones de grupos. En esta guía de inicio rápido, inscribirá dispositivos X.509 en el servicio Azure IoT Hub Device Provisioning Service con el SDK de servicio de Node.js
author: wesmc7777
ms.author: wesmc
ms.date: 12/21/2017
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.devlang: nodejs
ms.custom: mvc
ms.openlocfilehash: 67ed6f2039bad90716edadb2ecdb5e9ac9faa172
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "50156245"
---
# <a name="quickstart-enroll-x509-devices-to-the-device-provisioning-service-using-nodejs"></a>Guía de inicio rápido: Inscripción de dispositivos X.509 en el servicio Device Provisioning con Node.js

[!INCLUDE [iot-dps-selector-quick-enroll-device-x509](../../includes/iot-dps-selector-quick-enroll-device-x509.md)]

En esta guía de inicio rápido se muestra cómo usar Node.js para crear mediante programación un [grupo de inscripción](concepts-service.md#enrollment-group) que usa certificados X.509 intermedios o raíces de una entidad de certificación. El grupo de inscripción se crea mediante el [SDK de IoT para Node.js](https://github.com/Azure/azure-iot-sdk-node) y una aplicación Node.js de ejemplo. Un grupo de inscripción controla el acceso al servicio de aprovisionamiento de los dispositivos que comparten un certificado de firma común en su cadena de certificados. Para más información, consulte [Control del acceso de dispositivo al servicio de aprovisionamiento con certificados X.509](./concepts-security.md#controlling-device-access-to-the-provisioning-service-with-x509-certificates). Para más información sobre el uso de la infraestructura de clave pública (PKI) basada en certificados X.509 con Azure IoT Hub y el servicio Device Provisioning, consulte [Introducción a la seguridad mediante certificados de entidades de certificación X.509](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-overview). 

En esta guía de inicio rápido se asume que ya ha creado un centro de IoT y una instancia de Device Provisioning Service. Si aún no ha creado estos recursos, complete la guía de inicio rápido [Configuración del servicio Azure IoT Hub Device Provisioning con Azure Portal](./quick-setup-auto-provision.md) antes de continuar con este artículo.

Aunque los pasos de este artículo funcionan en máquinas Windows y Linux, este artículo se ha desarrollado para una máquina de desarrollo de Windows.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Requisitos previos

- Instale [Node.js v4.0 o posterior](https://nodejs.org).
- Instale [Git](https://git-scm.com/download/).


## <a name="prepare-test-certificates"></a>Preparación de los certificados de prueba

En esta guía de inicio rápido debe tener un archivo .pem o .cer que contenga la porción pública de un certificado X.509 intermedio o raíz de entidad de certificación. Se debe cargar este certificado en el servicio de aprovisionamiento y que este lo compruebe. 

El [SDK de Azure IoT para C](https://github.com/Azure/azure-iot-sdk-c) contiene herramientas de prueba que pueden ayudarle a crear una cadena de certificados X.509, cargar un certificado raíz o intermedio a partir de esa cadena y realizar la prueba de posesión con el servicio para comprobar el certificado. Los certificados creados con las herramientas del SDK están diseñados para su uso en las **pruebas de desarrollo únicamente**. Estos certificados **no se deben usar en producción**. Contienen contraseñas codificadas de forma rígida ("1234") que expiran después de 30 días. Para saber cómo obtener certificados adecuados para el código de producción, consulte [Cómo obtener un certificado de entidad de certificación X.509](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-overview#how-to-get-an-x509-ca-certificate) en la documentación de Azure IoT Hub.

Para usar estas herramientas de prueba para generar certificados, realice los pasos siguientes: 
 
1. Abra un símbolo del sistema o el shell de Git Bash y cambie a una carpeta de trabajo de la máquina. Ejecute el siguiente comando para clonar el repositorio de GitHub del [SDK para C de Azure IoT](https://github.com/Azure/azure-iot-sdk-c):
    
  ```cmd/sh
  git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive
  ```

  Actualmente, el tamaño de este repositorio es de unos 220 MB. Esta operación puede tardar varios minutos en completarse.

  Las herramientas de prueba se encuentran en el directorio *azure-iot-sdk-c/tools/CACertificates* del repositorio que clonó.    

2. Siga los pasos descritos en [Managing test CA certificates for samples and tutorials](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) (Administración de certificados de entidad de certificación de prueba para ejemplos y tutoriales). 



## <a name="create-the-enrollment-group-sample"></a>Crear el ejemplo del grupo de inscripción 

 
1. Desde una ventana de comandos en la carpeta de trabajo, ejecute:
  
     ```cmd\sh
     npm install azure-iot-provisioning-service
     ```  

2. Con un editor de texto, cree un archivo **create_enrollment_group.js** en la carpeta de trabajo. Agregue el siguiente código al archivo y guárdelo:

    ```
    'use strict';
    var fs = require('fs');

    var provisioningServiceClient = require('azure-iot-provisioning-service').ProvisioningServiceClient;

    var serviceClient = provisioningServiceClient.fromConnectionString(process.argv[2]);

    var enrollment = {
      enrollmentGroupId: 'first',
      attestation: {
        type: 'x509',
        x509: {
          signingCertificates: {
            primary: {
              certificate: fs.readFileSync(process.argv[3], 'utf-8').toString()
            }
          }
        }
      },
      provisioningStatus: 'disabled'
    };

    serviceClient.createOrUpdateEnrollmentGroup(enrollment, function(err, enrollmentResponse) {
      if (err) {
        console.log('error creating the group enrollment: ' + err);
      } else {
        console.log("enrollment record returned: " + JSON.stringify(enrollmentResponse, null, 2));
        enrollmentResponse.provisioningStatus = 'enabled';
        serviceClient.createOrUpdateEnrollmentGroup(enrollmentResponse, function(err, enrollmentResponse) {
          if (err) {
            console.log('error updating the group enrollment: ' + err);
          } else {
            console.log("updated enrollment record returned: " + JSON.stringify(enrollmentResponse, null, 2));
          }
        });
      }
    });
    ````

## <a name="run-the-enrollment-group-sample"></a>Ejecutar el ejemplo del grupo de inscripción
 
1. Para ejecutar el ejemplo, necesita la cadena de conexión para el servicio de aprovisionamiento. 
    1. Inicie sesión en Azure Portal, haga clic en el botón **Todos los recursos** situado en el menú izquierdo y abra el servicio Device Provisioning. 
    2. Haga clic en **Directivas de acceso compartido** y, a continuación, haga clic en la directiva de acceso que quiere usar para abrir sus propiedades. En la ventana **Directiva de acceso**, copie y tome nota de la cadena de conexión de la clave principal. 

    ![Obtención de la cadena de conexión del servicio de aprovisionamiento desde el portal](./media/quick-enroll-device-x509-node/get-service-connection-string.png) 


3. Como se indica en [Preparación de los certificados de prueba](quick-enroll-device-x509-node.md#prepare-test-certificates), necesita un archivo .pem que contenga un certificado de entidad emisora X.509 intermedio o raíz cargado y verificado previamente con su servicio de aprovisionamiento. Para comprobar que el certificado se ha cargado y verificado, en la página de resumen del servicio Device Provisioning de Azure Portal, haga clic en **Certificados**. Busque el certificado que quiere usar para la inscripción de grupo y asegúrese de que su valor de estado sea *verificado*.

    ![Certificado verificado en el portal](./media/quick-enroll-device-x509-node/verify-certificate.png) 

1. Para crear un grupo de inscripción para su certificado, ejecute el comando siguiente (incluidas las comillas alrededor de los argumentos de comando):
 
     ```cmd\sh
     node create_enrollment_group.js "<the connection string for your provisioning service>" "<your certificate's .pem file>"
     ```
 
3. Tras una creación correcta, la ventana de comandos muestra las propiedades del grupo de inscripción nuevo.

    ![Propiedades de la inscripción en la salida del comando](./media/quick-enroll-device-x509-node/sample-output.png) 

4. Compruebe que se ha creado el grupo de inscripción. En Azure Portal, en la hoja de resumen del servicio Device Provisioning, seleccione **Administrar inscripciones**. Seleccione la pestaña **Grupos de inscripción** y compruebe que haya la nueva entrada de inscripción (*primera*).

    ![Propiedades de la inscripción en el portal](./media/quick-enroll-device-x509-node/verify-enrollment-portal.png) 
 
## <a name="clean-up-resources"></a>Limpieza de recursos
Si tiene previsto explorar los ejemplos del servicio de Node.js, no elimine los recursos que se crearon con esta guía de inicio rápido. Si no va a continuar, use el siguiente procedimiento para eliminar todos los recursos de Azure creados con esta guía de inicio rápido.
 
1. Cierre la ventana de salida de ejemplo de Node.js en su máquina.
2. Navegue a su servicio Device Provisioning en Azure Portal, haga clic en **Administrar inscripciones** y, a continuación, seleccione la pestaña **Grupos de inscripción**. Seleccione el *id. de registro* para la entrada de inscripción que creó en esta guía de inicio rápido y haga clic en el botón **Eliminar** situado en la parte superior de la hoja.  
3. Desde el servicio Device Provisioning en Azure Portal, haga clic en **Certificados**, en el certificado que cargó para esta guía de inicio rápido y en el botón **Eliminar** situado en la parte superior de la ventana **Detalles del certificado**.  
 
## <a name="next-steps"></a>Pasos siguientes
En esta guía de inicio rápido, ha creado una inscripción de grupo para un certificado de entidad de certificación X.509 intermedio o raíz mediante el servicio Azure IoT Hub Device Provisioning. Para más información acerca del aprovisionamiento de dispositivos, continúe con el tutorial para instalar el servicio Device Provisioning en Azure Portal. 
 
> [!div class="nextstepaction"]
> [Tutoriales del servicio Azure IoT Hub Device Provisioning](./tutorial-set-up-cloud.md)