---
title: Métodos de autenticación para ASC para la versión preliminar de IoT | Microsoft Docs
description: Obtenga información sobre los diferentes métodos de autenticación disponibles cuando se usa la ASC para el servicio IoT.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 10b38f20-b755-48cc-8a88-69828c17a108
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 23bc4d0df1c8124ec225ac31239c7acb3f1ab546
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541818"
---
# <a name="security-agent-authentication-methods"></a>Métodos de autenticación de seguridad del agente 

> [!IMPORTANT]
> ASC para IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se proporciona sin un contrato de nivel de servicio y no se recomienda para las cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En este artículo se explica los diferentes métodos de autenticación que puede utilizar con el agente AzureIoTSecurity para autenticarse con IoT Hub.

Para cada dispositivo se incorporó a ASC para IoT en IoT Hub, se requiere un módulo de seguridad. Para autenticar el dispositivo, ASC para IoT puede usar uno de estos dos métodos. Elija el método más adecuado para la solución de IoT existente. 

> [!div class="checklist"]
> * Opción de módulo de seguridad
> * Opción de dispositivo

## <a name="authentication-methods"></a>Métodos de autenticación

Los dos métodos para el agente AzureIoTSecurity realizar la autenticación:

 - **Módulo** modo de autenticación<br>
   El módulo se autentica con independencia del dispositivo gemelo.
   La información necesaria para este tipo de autenticación está definida en el archivo Authentication.config para C# y el LocalConfiguration.json para C.
        
 - **Dispositivo** modo de autenticación<br>
    En este método, el agente de seguridad en primer lugar se autentica con el dispositivo. Después de la autenticación inicial, se realiza la ASC para el agente de IoT **Rest** llamar al centro de IoT mediante la API de Rest con los datos de autenticación del dispositivo. La ASC para el agente de IoT, a continuación, solicita el método de autenticación del módulo de seguridad y los datos desde el centro de IoT. En el paso final, el ASC para el agente de IoT realiza una autenticación en los procesos de ASC para el módulo de IoT.    

Consulte [parámetros de instalación del agente de seguridad](#security-agent-installation-parameters) para aprender a configurar.
                                
## <a name="authentication-methods-known-limitations"></a>Limitaciones conocidas de métodos de autenticación

- **Módulo** modo de autenticación solo admite la autenticación de clave simétrica.
- Certificado de firma de CA no es compatible con **dispositivo** modo de autenticación.  

## <a name="security-agent-installation-parameters"></a>Parámetros de instalación del agente de seguridad

Cuando [implementar un agente de seguridad](select-deploy-agent.md), se deben proporcionar los detalles de autenticación como argumentos.
Estos argumentos se documentan en la tabla siguiente.


|Parámetro|DESCRIPCIÓN|Opciones|
|---------|---------------|---------------|
|**identity**|Modo de autenticación| **Módulo** o **dispositivo**|
|**type**|Tipo de autenticación|**SymmetricKey** o **SelfSignedCertificate**|
|**filePath**|Ruta de acceso absoluta completa del archivo que contiene el certificado o la clave simétrica| |
|**gatewayHostname**|FQDN de IoT Hub|Ejemplo: ContosoIotHub.azure-devices.net|
|**deviceId**|Id. de dispositivo|Ejemplo: MyDevice1|
|**certificateLocationKind**|Ubicación del almacenamiento de certificados|**ArchivoLocal** o **Store**|


Cuando se usa el script instalación del agente de seguridad, la siguiente configuración se realiza automáticamente.

Para modificar manualmente la autenticación de seguridad del agente, modifique el archivo de configuración. 

## <a name="change-authentication-method-after-deployment"></a>Cambiar método de autenticación después de la implementación

Al implementar a un agente de seguridad con un script de instalación, se crea automáticamente un archivo de configuración.

Para cambiar los métodos de autenticación después de la implementación, no se requiere la edición manual del archivo de configuración.


### <a name="c-based-security-agent"></a>C#-agente de seguridad basado en

Editar _Authentication.config_ con los siguientes parámetros:

```xml
<Authentication>
  <add key="deviceId" value=""/>
  <add key="gatewayHostname" value=""/>
  <add key="filePath" value=""/>
  <add key="type" value=""/>
  <add key="identity" value=""/>
  <add key="certificateLocationKind" value="" />
</Authentication>
```

### <a name="c-based-security-agent"></a>Agente de seguridad basada en C

Editar _LocalConfiguration.json_ con los siguientes parámetros:

```json
"Authentication" : {
    "Identity" : "",
    "AuthenticationMethod" : "",
    "FilePath" : "",
    "DeviceId" : "",
    "HostName" : ""
}
```

## <a name="see-also"></a>Vea también
- [Información general sobre los agentes de seguridad](security-agent-architecture.md)
- [Implementar el agente de seguridad](select-deploy-agent.md)
- [Acceder a los datos sin procesar de seguridad](how-to-security-data-access.md)
