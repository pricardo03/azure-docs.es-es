---
title: Instalación de Windows de Azure Security Center para vista previa del agente de IoT | Microsoft Docs
description: Obtenga información sobre cómo instalar Azure Security Center para el agente de IoT en los dispositivos de Windows de 32 bits o 64 bits.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 2cf6a49b-5d35-491f-abc3-63ec24eb4bc2
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/19/2019
ms.author: mlottner
ms.openlocfilehash: 601ee706adedf522890acc3f3996a7d36b349348
ms.sourcegitcommit: 48a41b4b0bb89a8579fc35aa805cea22e2b9922c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/15/2019
ms.locfileid: "59577849"
---
# <a name="deploy-an-azure-security-center-for-iot-c-based-security-agent-for-windows"></a>Implementar un centro de seguridad de Azure para IoT C#-basándose security agent para Windows

> [!IMPORTANT]
> Azure Security Center for IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Esta guía explica cómo instalar Azure Security Center (ASC) para IoT C#-en función de agente de seguridad en Windows.

En esta guía, aprenderá a: 
> [!div class="checklist"]
> * Instalación
> * Comprobación de la implementación
> * Desinstalación del agente
> * Solución de problemas 

## <a name="prerequisites"></a>Requisitos previos

Para otras plataformas y versiones de agente, consulte [Elección del agente de seguridad correcto](how-to-deploy-agent.md).

1. Derechos de administrador local en el equipo que desea instalar en. 

1. [Crear un módulo de seguridad](quickstart-create-security-twin.md) para el dispositivo.

## <a name="installation"></a>Instalación 

Para instalar al agente de seguridad, realice las siguientes operaciones:

1. Para instalar el ASC para Windows IoT C# agente en el dispositivo, descargue la versión más reciente para la máquina desde la ASC para IoT [repositorio de GitHub](https://github.com/Azure/Azure-IoT-Security-Agent-CS).

2. Extraiga el contenido del paquete y vaya a la carpeta /Install.

3. Abra Windows PowerShell como administrador. 
    1. Agregue permisos de ejecución al script de InstallSecurityAgent, para lo que debe ejecutar ```Unblock-File .\InstallSecurityAgent.ps1```
    
        y ejecute:

    ```
    .\InstallSecurityAgent.ps1 -Install -aui <authentication identity> -aum <authentication method> -f <file path> -hn <host name> -di <device id> -cl <certificate location kind>
    ```
    
    Por ejemplo: 
    
    ```
    .\InstallSecurityAgent.ps1 -Install -aui Device -aum SymmetricKey -f c:\Temp\Key.txt -hn MyIotHub.azure-devices.net -di Mydevice1 -cl store
    ```
    
    Para más información acerca de los parámetros de autenticación, consulte [Procedimiento para configurar la autenticación](concept-security-agent-authentication-methods.md).

El script hace lo siguiente:

- Instala los requisitos previos.

- Agrega un usuario de servicio (con el inicio de sesión interactivo deshabilitado).

- Instala el agente como un **servicio del sistema**.

- Configura al agente con los parámetros de autenticación proporcionados.


Para más ayuda, use el comando Get-Help en PowerShell <br>Ejemplo de Get-Help:  
    ```Get-Help .\InstallSecurityAgent.ps1```

### <a name="verify-deployment-status"></a>Comprobación del estado de la implementación

- Compruebe el estado de la implementación del agente, para lo que debe ejecutar:<br>
    ```sc.exe query "ASC IoT Agent"```

### <a name="uninstall-the-agent"></a>Desinstalación del agente

Para desinstalar el agente:

1. Ejecute el siguiente script de PowerShell con el parámetro **-mode** parámetro establecido en **Uninstall**.  

    ```
    .\InstallSecurityAgent.ps1 -Uninstall
    ``` 

## <a name="troubleshooting"></a>solución de problemas

Si el agente no se inicia, active el registro (el registro está *desactivado* de forma predeterminada) para más información.

Para activar el registro:

1. Abra el archivo de configuración (General.config) para la edición con un editor de archivos estándar.

1. Edite los valores siguientes:

   ```xml
   <add key="logLevel" value="Debug" />
   <add key="fileLogLevel" value="Debug"/> 
   <add key="diagnosticVerbosityLevel" value="Some" /> 
   <add key="logFilePath" value="IoTAgentLog.log" />
   ```

    > [!NOTE]
    > Se recomienda **desactivar** el registro una vez que se complete la solución de problemas, ya que si se deja **activado** aumenta el tamaño del archivo de registro y el uso de datos. 

1. Reinicie el agente, para lo que debe ejecutar lo siguiente en PowerShell o en la línea de comandos:

    **PowerShell**
     ```
     Restart-Service "ASC IoT Agent"
     ```
     
   o

    **CMD**
     ```
     sc.exe stop "ASC IoT Agent" 
     sc.exe start "ASC IoT Agent" 
     ```

1. Examine el archivo de registro para más información acerca del error.

   Ubicación del archivo de registro: `%WinDir%/System32/IoTAgentLog.log`


## <a name="next-steps"></a>Pasos siguientes
- Lea la [información general](overview.md) del servicio ASC for IoT
- Obtenga más información acerca de la [arquitectura](architecture.md) de ASC for IoT
- Habilite el [servicio](quickstart-onboard-iot-hub.md)
- Lea las [preguntas más frecuentes](resources-frequently-asked-questions.md)
- Obtenga información acerca de las [alertas](concept-security-alerts.md)