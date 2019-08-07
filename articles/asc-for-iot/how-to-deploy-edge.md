---
title: Implementación del módulo Edge de Azure Security Center para IoT (versión preliminar) | Microsoft Docs
description: Aprenda cómo implementar un agente de seguridad de Azure Security Center para IoT en IoT Edge.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 32a9564d-16fd-4b0d-9618-7d78d614ce76
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: 7171923e4badb3355a64b63515d40e73fadca6b0
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2019
ms.locfileid: "68596361"
---
# <a name="deploy-a-security-module-on-your-iot-edge-device"></a>Implementación de un módulo de seguridad en el dispositivo IoT Edge

> [!IMPORTANT]
> La compatibilidad con dispositivos IoT Edge en Azure Security Center para IoT se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

El módulo **Azure Security Center para IoT** proporciona una solución de seguridad completa para los dispositivos IoT Edge.
El módulo de seguridad recopila, agrega y analiza los datos de seguridad sin procesar del sistema operativo y del sistema de contenedores para generar alertas y recomendaciones de seguridad que requieren acción.
Para aprender más, consulte [Módulo de seguridad de Azure Security Center for IoT](security-edge-architecture.md).

En este artículo, aprenderá a implementar un módulo de seguridad en el dispositivo IoT Edge.

## <a name="deploy-security-module"></a>Implementación de un módulo de seguridad

Use los pasos siguientes para implementar un módulo de seguridad de Azure Security Center para IoT para IoT Edge.

### <a name="prerequisites"></a>Requisitos previos

- En su IoT Hub, asegúrese de que el dispositivo está [registrado como un dispositivo IoT Edge](https://docs.microsoft.com/azure/iot-edge/how-to-register-device-portal).

- El módulo Edge de Azure Security Center para IoT requiere que el [marco AuditD](https://linux.die.net/man/8/auditd) esté instalado en el dispositivo IoT Edge.

    - Instale el marco de trabajo ejecutando el comando siguiente en el dispositivo IoT Edge:
   
      `sudo apt-get install auditd audispd-plugins`
   
    - Compruebe que AuditID está activo mediante la ejecución del comando siguiente:
   
      `sudo systemctl status auditd`
      
        La respuesta esperada es `active (running)`. 

### <a name="deployment-using-azure-portal"></a>Implementación mediante Azure Portal

1. Desde Azure Portal, abra **Marketplace**.

1. Seleccione **Internet de las cosas** y luego busque **Azure Security Center para IoT** y selecciónelo.

   ![Selección de Azure Security Center para IoT](media/howto/edge-onboarding-8.png)

1. Haga clic en **Crear** para configurar la implementación. 

1. Elija la **suscripción** de Azure de IoT Hub y luego seleccione su **IoT Hub**.<br>Seleccione **Implementar en un dispositivo** para un único dispositivo de destino o seleccione **Implementar a escala** para varios dispositivos de destino y haga clic en **Crear**. Para más información sobre la implementación a escala, consulte [Implementación y supervisión de módulos de IoT Edge a escala mediante Azure Portal](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-monitor). 

    >[!Note] 
    >Si seleccionó **Implementar a escala**, agregue el nombre del dispositivo y los detalles antes de continuar con la pestaña **Agregar módulos** en las instrucciones siguientes.     

Hay tres pasos para crear una implementación de IoT Edge de Azure Security Center para IoT. En las siguientes secciones se abordan cada uno de ellos. 

#### <a name="step-1-add-modules"></a>Paso 1: Adición de módulos

1. En la pestaña **Agregar módulos**, área **Módulos de implementación**, haga clic en **AzureSecurityCenterforIoT**. 
   
1. Cambiar el **nombre** a **azureiotsecurity**.
1. Cambie el **URI de la imagen** a **mcr.microsoft.com/ascforiot/azureiotsecurity:0.0.3**.
1. Compruebe que el valor de **Opciones de creación del contenedor** está establecido en:      
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
1. Compruebe que la opción **Establecer propiedades deseadas de gemelo del módulo**  está seleccionada y cambie el objeto de configuración a:
      
    ``` json
      "properties.desired": {
        "azureiot*com^securityAgentConfiguration^1*0*0": {
        }
      }
      ```

1. Haga clic en **Save**(Guardar).
1. Desplácese hasta el final de la pestaña y seleccione **Configurar las opciones avanzadas del entorno en tiempo de ejecución de Edge**.
   
   
1. Cambie la **imagen** bajo **Centro de Microsoft Edge**  a **mcr.microsoft.com/ascforiot/edgehub:1.0.9-preview**.

   >[!Note]
   > El módulo Azure Security Center para IoT requiere una versión bifurcada del centro de IoT Edge basada en la versión 1.20 del SDK.
   > Al cambiar la imagen del centro de IoT Edge, está indicando al dispositivo IoT Edge que reemplace la versión estable más reciente por la versión bifurcada del centro de IoT Edge, que oficialmente no es compatible con el servicio IoT Edge.

1. Verifique que **Opciones de creación** se ha establecido en: 
         
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

#### <a name="step-2-specify-routes"></a>Paso 2: Especificación de rutas 

1. En la pestaña **Especificar rutas**, asegúrese de que tiene una ruta (explícita o implícita) que reenviará los mensajes del módulo **azureiotsecurity** a **$upstream**. 
1. Haga clic en **Next**.

    ~~~Default implicit route
    "route": "FROM /messages/* INTO $upstream 
    ~~~

    ~~~Explicit route
    "ASCForIoTRoute": "FROM /messages/modules/azureiotsecurity/* INTO $upstream
    ~~~

#### <a name="step-3-review-deployment"></a>Paso 3: Revisar la implementación

- En la pestaña **Revisar la implementación**, revise la información de implementación y luego seleccione **Enviar** para completar la implementación.

## <a name="diagnostic-steps"></a>Pasos de diagnósticos

Si encuentra algún problema, los registros de contenedor son la mejor manera conocer el estado de un dispositivo de módulo de seguridad IoT Edge. Use los comandos y las herramientas de esta sección para recopilar información.

### <a name="verify-the-required-containers-are-installed-and-functioning-as-expected"></a>Comprobación de que los contenedores necesarios están instalados y funcionan según lo previsto

1. Ejecute el comando siguiente en el dispositivo de IoT Edge:
    
     `sudo docker ps`
   
1. Compruebe que los siguientes contenedores se están ejecutando:
   
   | NOMBRE | IMAGEN |
   | --- | --- |
   | azureiotsecurity | mcr.microsoft.com/ascforiot/azureiotsecurity:0.0.3 |
   | edgeHub | mcr.microsoft.com/ascforiot/edgehub:1.0.9-preview |
   | edgeAgent | mcr.microsoft.com/azureiotedge-agent:1.0 |
   
   Si no están presentes los contenedores mínimos necesarios, compruebe si el manifiesto de implementación de IoT Edge está alineado con la configuración recomendada. Para más información, consulte [Implementación del módulo IoT Edge](#deployment-using-azure-portal).

### <a name="inspect-the-module-logs-for-errors"></a>Inspección de los registros de módulo en busca de errores
   
1. Ejecute el comando siguiente en el dispositivo de IoT Edge:

   `sudo docker logs azureiotsecurity`
   
1. Para los registros más detallados, agregue la siguiente variable de entorno a la implementación del módulo **azureiotsecurity**: `logLevel=Debug`.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre las opciones de configuración, prosiga con la guía paso a paso para configurar el módulo. 
> [!div class="nextstepaction"]
> [Guía paso a paso de configuración del módulo](./how-to-agent-configuration.md)
