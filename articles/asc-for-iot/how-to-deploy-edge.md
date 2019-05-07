---
title: Implementar Azure Security Center para el módulo de IoT Edge | Microsoft Docs
description: Obtenga información sobre cómo implementar un centro de seguridad de Azure para el agente de seguridad de IoT en IoT Edge.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 32a9564d-16fd-4b0d-9618-7d78d614ce76
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/1/2019
ms.author: mlottner
ms.openlocfilehash: 5d56c878eab92a7152bc0b248cab5c2ebf7e6dde
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65198407"
---
# <a name="deploy-a-security-module-on-your-iot-edge-device"></a>Implementar un módulo de seguridad en el dispositivo de IoT Edge

> [!IMPORTANT]
> Azure Security Center for IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

**Azure Security Center (ASC) para IoT** módulo proporciona una solución de seguridad completa para el dispositivo IoT Edge.
Módulo de seguridad recopila, agrega y analiza los datos sin procesar de seguridad del sistema del sistema operativo y el contenedor en las alertas y recomendaciones de seguridad que requieren acción.
Para obtener más información, consulte [módulo de seguridad de IoT Edge](security-edge-architecture.md).

En esta guía, obtendrá información sobre cómo implementar un módulo de seguridad en el dispositivo de IoT Edge.

## <a name="deploy-security-module"></a>Implementar en módulo de seguridad

Siga estos pasos para implementar un ASC para el módulo de seguridad de IoT para IoT Edge.

### <a name="prerequisites"></a>Requisitos previos

- En su centro de IoT, asegúrese de que el dispositivo está [registrado como un dispositivo IoT Edge](https://docs.microsoft.com/azure/iot-edge/how-to-register-device-portal).

- ASC para el módulo de IoT Edge requiere [AuditD framework](https://linux.die.net/man/8/auditd) está instalado en el dispositivo de IoT Edge.

    - Instale el marco de trabajo ejecutando el comando siguiente en el dispositivo de IoT Edge:
   
      `sudo apt-get install auditd audispd-plugins`
   
    - Comprobar que auditd está activa, ejecute el comando siguiente:
   
      `sudo systemctl status auditd`
      
        La respuesta esperada es `active (running)`. 

### <a name="deployment-using-azure-portal"></a>Implementación mediante Azure portal

1. Desde Azure portal, abra **Marketplace**.

1. Seleccione **Internet de las cosas**, a continuación, busque **Azure Security Center para IoT** y selecciónelo.

   ![Seleccione Azure Security Center para IoT](media/howto/edge-onboarding-8.png)

1. Haga clic en **crear** para configurar la implementación. 

1. Elija Azure **suscripción** de IoT Hub, a continuación, seleccione su **IoT Hub**.<br>Seleccione **implementar en un dispositivo** para un único dispositivo de destino o seleccione **implementar a escala** para varios dispositivos de destino y haga clic en **crear**. Para obtener más información acerca de la implementación a escala, consulte [cómo implementar](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-monitor). 

    >[!Note] 
    >Si seleccionó **implementar a escala**, agregue el nombre del dispositivo y los detalles antes de continuar con la **agregar módulos** ficha en las instrucciones siguientes.     

Hay tres pasos para crear una implementación de IoT Edge de Azure Security Center para IoT. En las siguientes secciones se abordan cada uno de ellos. 

#### <a name="step-1-add-modules"></a>Paso 1: Agregar módulos

1. Desde el **agregar módulos** ficha, **módulos de implementación** área, haga clic en **AzureSecurityCenterforIoT**. 
   
1. Cambiar el **nombre** a **azureiotsecurity**.
1. Cambiar el **URI de la imagen** a **mcr.microsoft.com/ascforiot/azureiotsecurity:0.0.3**.
1. Compruebe el **opciones de creación del contenedor** valor está establecido en:      
    ``` json
    {
        "NetworkingConfig": {
            "EndpointsConfig": {
                "host": {}
            }
        },
        "HostConfig": {
            "Privileged": true,
            "NetworkMode": "host",
            "PidMode": "host",
            "Binds": [
                "/:/host"
            ]
        }
    }    
    ```
1. Compruebe que **propiedades deseadas del gemelo de módulo de conjunto** está seleccionada y cambie el objeto de configuración para:
      
    ``` json
      "properties.desired": {
        "azureiot*com^securityAgentConfiguration^1*0*0": {
        }
      }
      ```

1. Haga clic en **Save**(Guardar).
1. Desplácese hasta el final de la pestaña y seleccione **configurar opciones de tiempo de ejecución de Edge avanzadas**.
   
   >[!Note]
   > Hacer **no** deshabilitar la comunicación de AMQP para el centro de IoT Edge.
   > Azure Security Center para el módulo de IoT requiere una comunicación de AMQP con el centro de IoT Edge.
   
1. Cambiar el **imagen** en **Edge Hub** a **mcr.microsoft.com/ascforiot/edgehub:1.0.9-preview**.

   >[!Note]
   > Azure Security Center para el módulo de IoT requiere una versión bifurcada de IoT Edge Hub, que se basa en SDK versión 1.20.
   > Al cambiar la imagen del centro de IoT Edge, está indicando el dispositivo de IoT Edge para reemplazar la versión estable más reciente con la versión bifurcada del centro de IoT Edge, que no es compatible oficialmente con el servicio de IoT Edge.

1. Comprobar **crear opciones** está establecido en: 
         
    ``` json
    {
      "HostConfig": {
        "PortBindings": {
          "8883/tcp": [{"HostPort": "8883"}],
          "443/tcp": [{"HostPort": "443"}],
          "5671/tcp": [{"HostPort": "5671"}]
        }
      }
    }
    ```
      
1. Haga clic en **Save**(Guardar).
   
1. Haga clic en **Next**.

#### <a name="step-2-specify-routes"></a>Paso 2: Especificar rutas 

1. En el **especificar rutas** pestaña, establezca el **ASCForIoTToIoTHub** enrutar a **"de/messages/módulos/azureiotsecurity/\* en $upstream"** y haga clic en  **Siguiente**.

   ![Especificación de rutas](media/howto/edge-onboarding-9.png)

#### <a name="step-3-review-deployment"></a>Paso 3: Revisar la implementación

1. En el **revisar implementación** pestaña, revise la información de implementación y luego seleccione **enviar** para completar la implementación.

## <a name="diagnostic-steps"></a>Pasos de diagnóstico

Si encuentra algún problema, los registros de contenedor son la mejor manera para obtener información sobre el estado de un dispositivo de módulo de seguridad de IoT Edge. Use los comandos y las herramientas de esta sección para recopilar información.

### <a name="verify-the-required-containers-are-installed-and-functioning-as-expected"></a>Compruebe que los contenedores necesarios están instalados y funciona según lo previsto

1. Ejecute el siguiente comando en el dispositivo de IoT Edge:
    
     `sudo docker ps`
   
1. Compruebe que se están ejecutando los siguientes contenedores:
   
   | NOMBRE | IMAGEN |
   | --- | --- |
   | azureIoTSecurity | mcr.microsoft.com/ascforiot/azureiotsecurity:0.0.3 |
   | edgeHub | mcr.microsoft.com/ascforiot/edgehub:1.0.9-preview |
   | edgeAgent | mcr.microsoft.com/azureiotedge-agent:1.0 |
   
   Si es el mínimo necesario contenedores no están presentes, compruebe si el manifiesto de implementación de IoT Edge se alinea con la configuración recomendada. Para obtener más información, consulte [módulo de implementación de IoT Edge](#deployment-using-azure-portal).

### <a name="inspect-the-module-logs-for-errors"></a>Inspeccione los registros de módulo para errores
   
1. Ejecute el siguiente comando en el dispositivo de IoT Edge:

   `sudo docker logs azureiotsecurity`
   
1. Para los registros más detallados, agregue la siguiente variable de entorno para **azureiotsecurity** la implementación del módulo: `logLevel=Debug`.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre las opciones de configuración, seguir la Guía de procedimientos de configuración del módulo. 
> [!div class="nextstepaction"]
> [Guía de procedimientos de configuración de módulo](./how-to-agent-configuration.md)
